---
title: "include file" description: "include file" services: app-service author: cephalin ms.service: app-service ms.topic: "include" ms.date: 04/27/2021 ms.author: cephalin ms.custom: "include file", devx-track-azurecli
---

Erstellen Sie in Cloud Shell mit dem Befehl [`az appservice plan create`](/cli/azure/appservice/plan) einen App Service-Plan.

<!-- [!INCLUDE [app-service-plan](app-service-plan.md)] -->

Im folgenden Beispiel wird ein App Service-Plan namens `myAppServicePlan` mit dem Tarif **Free** erstellt:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

Nach Erstellung des App Service-Plans zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

<pre>
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "West Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  &lt; JSON data removed for brevity. &gt;
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
</pre>
