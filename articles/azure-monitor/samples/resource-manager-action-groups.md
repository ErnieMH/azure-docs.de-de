---
title: Beispiele für Resource Manager-Vorlagen für Aktionsgruppen
description: Hier finden Sie Beispiele für Azure Resource Manager-Vorlagen zum Bereitstellen von Azure Monitor-Aktionsgruppen.
ms.subservice: logs
ms.topic: sample
author: bwren
ms.author: bwren
ms.date: 05/18/2020
ms.openlocfilehash: fda9a7e39f1bb103a203cdfa61a5044c51c692f2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "83852973"
---
# <a name="resource-manager-template-samples-for-action-groups-in-azure-monitor"></a>Beispiele für Resource Manager-Vorlagen für Aktionsgruppen in Azure Monitor
Dieser Artikel enthält Beispiele für [Azure Resource Manager-Vorlagen](../../azure-resource-manager/templates/template-syntax.md) zum Erstellen von [Aktionsgruppen](../platform/action-groups.md) in Azure Monitor. Jedes Beispiel umfasst eine Vorlagendatei und eine Parameterdatei mit Beispielwerten für die Vorlage.

[!INCLUDE [azure-monitor-samples](../../../includes/azure-monitor-resource-manager-samples.md)]

## <a name="create-an-action-group"></a>Erstellen einer Aktionsgruppe
Im folgenden Beispiel wird eine Aktionsgruppe erstellt.


### <a name="template-file"></a>Vorlagendatei

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name within the resource group for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name up to 12 characters for the Action group."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Insights/actionGroups",
      "apiVersion": "2018-03-01",
      "name": "[parameters('actionGroupName')]",
      "location": "Global",
      "properties": {
        "groupShortName": "[parameters('actionGroupShortName')]",
        "enabled": true,
        "smsReceivers": [
          {
            "name": "contosoSMS",
            "countryCode": "1",
            "phoneNumber": "5555551212"
          },
          {
            "name": "contosoSMS2",
            "countryCode": "1",
            "phoneNumber": "5555552121"
          }
        ],
        "emailReceivers": [
          {
            "name": "contosoEmail",
            "emailAddress": "devops@contoso.com"
          },
          {
            "name": "contosoEmail2",
            "emailAddress": "devops2@contoso.com"
          }
        ],
        "webhookReceivers": [
          {
            "name": "contosoHook",
            "serviceUri": "http://requestb.in/1bq62iu1"
          },
          {
            "name": "contosoHook2",
            "serviceUri": "http://requestb.in/1bq62iu2"
          }
        ]
      }
    }
  ],
  "outputs":{
      "actionGroupId":{
          "type":"string",
          "value":"[resourceId('Microsoft.Insights/actionGroups',parameters('actionGroupName'))]"
      }
  }
}
```

### <a name="parameter-file"></a>Parameterdatei

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "actionGroupName": {
        "value": "My Action Group"
      },
      "actionGroupShortName": {
        "value": "mygroup"
      }
  }
}
```



## <a name="next-steps"></a>Nächste Schritte

* [Rufen Sie weitere Beispielvorlagen für Azure Monitor ab](resource-manager-samples.md).
* [Erfahren Sie mehr über Aktionsgruppen](../platform/action-groups.md).

