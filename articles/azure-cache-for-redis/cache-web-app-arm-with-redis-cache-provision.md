---
title: Bereitstellen einer Web-App mit Azure Cache for Redis
description: Verwenden Sie eine Azure Resource Manager-Vorlage, um eine Web-App mit Azure Cache for Redis bereitzustellen.
services: app-service
author: yegu-ms
ms.service: app-service
ms.topic: conceptual
ms.date: 01/06/2017
ms.author: yegu
ms.openlocfilehash: ec8d4f5611425734974d07ae6ee7008b10b9b406
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85833773"
---
# <a name="create-a-web-app-plus-azure-cache-for-redis-using-a-template"></a>Erstellen einer Web-App und eines Azure Cache for Redis mithilfe einer Vorlage

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

In diesem Thema erfahren Sie, wie Sie eine Azure Resource Manager-Vorlage erstellen, die eine Azure Web-App mit einem Azure Cache for Redis bereitstellt. Sie erfahren, wie Sie definieren, welche Ressourcen bereitgestellt werden und wie Sie Parameter definieren, die angegeben werden, wenn die Bereitstellung ausgeführt wird. Sie können diese Vorlage für Ihre eigenen Bereitstellungen verwenden oder an Ihre Anforderungen anpassen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](../azure-resource-manager/templates/template-syntax.md). Weitere Informationen zur JSON-Syntax und den Eigenschaften für Cacheressourcentypen finden Sie unter [Microsoft.Cache resource types](/azure/templates/microsoft.cache/allversions) (Microsoft.Cache-Ressourcentypen).

Die vollständige Vorlage finden Sie unter [Web-App mit Azure Cache for Redis-Vorlage](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-with-redis-cache/azuredeploy.json).

## <a name="what-you-will-deploy"></a>Was Sie bereitstellen
In dieser Vorlage stellen Sie Folgendes bereit:

* Azure-Web-App
* Azure Cache for Redis

Klicken Sie auf folgende Schaltfläche, um die Bereitstellung automatisch auszuführen:

[![In Azure bereitstellen](./media/cache-web-app-arm-with-redis-cache-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-with-redis-cache%2Fazuredeploy.json)

## <a name="parameters-to-specify"></a>Anzugebende Parameter
[!INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

[!INCLUDE [cache-deploy-parameters](../../includes/cache-deploy-parameters.md)]

## <a name="variables-for-names"></a>Variablen für Namen
Diese Vorlage verwendet Variablen für die Erstellung von Namen für die Ressourcen. Sie nutzt die [uniqueString](../azure-resource-manager/templates/template-functions-string.md#uniquestring) -Funktion, um einen Wert basierend auf der Ressourcengruppen-ID zu erstellen.

```json
"variables": {
  "hostingPlanName": "[concat('hostingplan', uniqueString(resourceGroup().id))]",
  "webSiteName": "[concat('webSite', uniqueString(resourceGroup().id))]",
  "cacheName": "[concat('cache', uniqueString(resourceGroup().id))]"
},
```


## <a name="resources-to-deploy"></a>Bereitzustellende Ressourcen
[!INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="azure-cache-for-redis"></a>Azure Cache for Redis
Erstellt den Azure Cache for Redis, der mit der Web-App verwendet werden soll. Der Name des Cache wird in der **cacheName** -Variablen angegeben.

Die Vorlage erstellt den Cache am gleichen Speicherort wie die Ressourcengruppe.

```json
{
  "name": "[variables('cacheName')]",
  "type": "Microsoft.Cache/Redis",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-08-01",
  "dependsOn": [ ],
  "tags": {
    "displayName": "cache"
  },
  "properties": {
    "sku": {
      "name": "[parameters('cacheSKUName')]",
      "family": "[parameters('cacheSKUFamily')]",
      "capacity": "[parameters('cacheSKUCapacity')]"
    }
  }
}
```


### <a name="web-app"></a>Web-App
Erstellt die Web-App mit dem Namen, der in der **webSiteName** -Variablen angegeben ist.

Beachten Sie, dass die Web-App mit App-Einstellungseigenschaften konfiguriert ist, die es der App ermöglichen, den Azure Cache for Redis zu verwenden. Diese App-Einstellungen werden dynamisch anhand der Werte erstellt, die während der Bereitstellung angegeben wurden.

```json
{
  "apiVersion": "2015-08-01",
  "name": "[variables('webSiteName')]",
  "type": "Microsoft.Web/sites",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Web/serverFarms/', variables('hostingPlanName'))]",
    "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
  ],
  "tags": {
    "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', variables('hostingPlanName'))]": "empty",
    "displayName": "Website"
  },
  "properties": {
    "name": "[variables('webSiteName')]",
    "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
  },
  "resources": [
    {
      "apiVersion": "2015-08-01",
      "type": "config",
      "name": "appsettings",
      "dependsOn": [
        "[concat('Microsoft.Web/Sites/', variables('webSiteName'))]",
        "[concat('Microsoft.Cache/Redis/', variables('cacheName'))]"
      ],
      "properties": {
       "CacheConnection": "[concat(variables('cacheName'),'.redis.cache.windows.net,abortConnect=false,ssl=true,password=', listKeys(resourceId('Microsoft.Cache/Redis', variables('cacheName')), '2015-08-01').primaryKey)]"
      }
    }
  ]
}
```

## <a name="commands-to-run-deployment"></a>Befehle zum Ausführen der Bereitstellung
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

```azurepowershell
New-AzResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup
```

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli
azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-with-redis-cache/azuredeploy.json -g ExampleDeployGroup
```
