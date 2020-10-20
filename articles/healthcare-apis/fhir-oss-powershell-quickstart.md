---
title: 'Mit PowerShell: Bereitstellen eines FHIR-Servers für Azure – Azure API for FHIR'
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie den Open-Source-FHIR-Server von Microsoft mithilfe von PowerShell bereitstellen.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: matjazl
ms.openlocfilehash: c6c1a7e21f0a1554c67c7f1860a2bd3382c941f5
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91817953"
---
# <a name="quickstart-deploy-open-source-fhir-server-using-powershell"></a>Schnellstart: Bereitstellen eines Open-Source-FHIR-Servers mithilfe von PowerShell

In dieser Schnellstartanleitung erfahren Sie, wie Sie den Microsoft Open-Source-FHIR-Server für Azure mithilfe von PowerShell bereitstellen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Wählen Sie einen Namen für die Ressourcengruppe aus, in der die bereitgestellten Ressourcen enthalten sein sollen, und erstellen Sie sie:

```azurepowershell-interactive
$fhirServiceName = "MyFhirService"
$rg = New-AzResourceGroup -Name $fhirServiceName -Location westus2
```

## <a name="deploy-the-fhir-server-template"></a>Bereitstellen der FHIR-Servervorlage

Das [GitHub-Repository](https://github.com/Microsoft/fhir-server) für den Microsoft FHIR-Server für Azure enthält eine Vorlage, mit der alle erforderlichen Ressourcen bereitgestellt werden. Der Bereitstellungsprozess dauert mehrere Minuten, während die erforderlichen Azure-Ressourcen erstellt und konfiguriert werden. Führen Sie die Bereitstellung wie folgt aus:

```azurepowershell-interactive
New-AzResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Microsoft/fhir-server/master/samples/templates/default-azuredeploy.json -ResourceGroupName $rg.ResourceGroupName -serviceName $fhirServiceName
```

> [!NOTE]
> Führen Sie zuerst die folgenden Befehle aus, falls Sie noch nicht angemeldet sind.

```azurepowershell-interactive
Connect-AzAccount
get-azsubscription
Set-AzContext -SubscriptionId yoursubscriptionid
```

Ändern Sie zum Verwenden einer vorhandenen Ressourcengruppe die „$rg“-Werte in der Zeile mit der Definition der Variablen „$rg“ und in der Befehlszeile für die Bereitstellung der Azure ARM-Vorlage. Dies ist im Code dargestellt.

```azurepowershell-interactive
$fhirServiceName = "MyFhirService"
$rg = "MyExistingResourceGroupName"
New-AzResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Microsoft/fhir-server/master/samples/templates/default-azuredeploy.json -ResourceGroupName $rg -serviceName $fhirServiceName
```

## <a name="verify-fhir-server-is-running"></a>Überprüfen, ob der FHIR-Server ausgeführt wird

```azurepowershell-interactive
$metadataUrl = "https://" + $fhirServiceName + ".azurewebsites.net/metadata" 
$metadata = Invoke-WebRequest -Uri $metadataUrl
$metadata.RawContent
```

Es dauert etwa eine Minute, bis der Server zum ersten Mal antwortet.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht mehr benötigen, löschen Sie die Ressourcengruppe wie folgt:

```azurepowershell-interactive
Remove-AzResourceGroup -Name $rg.ResourceGroupName
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie den Microsoft Open-Source-FHIR-Server für Azure in Ihrem Abonnement bereitgestellt. Informationen zum Zugriff auf die FHIR-API mit Postman finden Sie im Tutorial zu Postman.
 
>[!div class="nextstepaction"]
>[Zugriff auf die FHIR-API mit Postman](access-fhir-postman-tutorial.md)
