---
title: Anhalten, Fortsetzen und Skalieren mit REST-APIs für dedizierte SQL-Pools (ehemals SQL DW)
description: Verwalten der Computeleistung für einen dedizierten SQL-Pool (bisher SQL DW) in Azure Synapse Analytics Data Warehouse über REST-APIs.
services: synapse-analytics
author: antvgski
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 03/29/2019
ms.author: anvang
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: aab2897b4042657492d04494b589fbaa2605cc6d
ms.sourcegitcommit: 5ce88326f2b02fda54dad05df94cf0b440da284b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107886782"
---
# <a name="rest-apis-for-dedicated-sql-pool-formerly-sql-dw-in-azure-synapse-analytics"></a>REST-APIs für einen dedizierten SQL-Pool (bisher SQL DW) in Azure Synapse Analytics

REST-APIs für die Computeverwaltung für einen dedizierten SQL-Pool (bisher SQL DW) in Azure Synapse Analytics.

> [!NOTE]
> Die REST-APIs, die in diesem Artikel beschrieben werden, sind nicht für einen dedizierten SQL-Pool anwendbar, der im Azure Synapse Analytics-Arbeitsbereich erstellt wird. Informationen zu REST-APIs, die speziell für einen Azure Synapse Analytics-Arbeitsbereich verwendet werden, finden Sie unter [Azure Synapse Analytics-Arbeitsbereichs-REST-API.](/rest/api/synapse/)

## <a name="scale-compute"></a>Skalieren von Computeressourcen

Um die DWUs (Data Warehouse Units) zu ändern, verwenden Sie die REST-API zum [Erstellen oder Aktualisieren einer Datenbank](/rest/api/sql/databases/createorupdate). Im folgenden Beispiel werden die DWUs für die Datenbank „MySQLDW“, die auf dem Server „MyServer“ gehostet wird, auf „DW1000“ festgelegt. Der Server befindet sich in einer Azure-Ressourcengruppe namens „ResourceGroup1“.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2020-08-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    location: "West Central US",
    "sku": {
    "name": "DW200c"
    }
}
```

## <a name="pause-compute"></a>Anhalten von Computeressourcen

Zum Anhalten einer Datenbank verwenden Sie die REST-API zum [Anhalten der Datenbank](/rest/api/sql/databases/pause). Im folgenden Beispiel wird die auf dem Server „Server01“ gehostete Datenbank mit der Bezeichnung „Database02“ angehalten. Der Server befindet sich in einer Azure-Ressourcengruppe namens „ResourceGroup1“.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/pause?api-version=2020-08-01-preview HTTP/1.1
```

## <a name="resume-compute"></a>Fortsetzen von Computeressourcen

Zum Starten einer Datenbank verwenden Sie die REST-API zum [Fortsetzen der Datenbank](/rest/api/sql/databases/resume). Im folgenden Beispiel wird die auf dem Server „Server01“ gehostete Datenbank mit der Bezeichnung „Database02“ gestartet. Der Server befindet sich in einer Azure-Ressourcengruppe namens „ResourceGroup1“.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/resume?api-version=2020-08-01-preview HTTP/1.1
```

## <a name="check-database-state"></a>Überprüfen des Datenbankzustands

> [!NOTE]
> Derzeit kann beim Überprüfen des Datenbankzustands ONLINE zurückgegeben werden, während die Datenbank den Online-Workflow ausführt. Dies führt zu Verbindungsfehlern. Sie müssen möglicherweise eine Verzögerung von 2 bis 3 Minuten in Ihrem Anwendungscode hinzufügen, wenn Sie diesen API-Aufruf zum Auslösen von Verbindungsversuchen verwenden.

```
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Sql/servers/{serverName}/databases/{databaseName}?api-version=2020-08-01-preview
```

## <a name="get-maintenance-schedule"></a>Abrufen des Wartungszeitplans

Überprüfen Sie den Wartungsplan, der für einen dedizierten SQL-Pool (bisher SQL DW) festgelegt wurde.

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/maintenanceWindows/current?maintenanceWindowName=current&api-version=2017-10-01-preview HTTP/1.1

```

## <a name="set-maintenance-schedule"></a>Festlegen des Wartungszeitplans

Festlegen und Aktualisieren eines Wartungsplans für einen dedizierten SQL-Pool (bisher SQL DW).

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/maintenanceWindows/current?maintenanceWindowName=current&api-version=2017-10-01-preview HTTP/1.1

{
    "properties": {
        "timeRanges": [
                {
                                "dayOfWeek": "Saturday",
                                "startTime": "00:00",
                                "duration": "08:00",
                },
                {
                                "dayOfWeek": "Wednesday",
                                "startTime": "00:00",
                                "duration": "08:00",
                }
                ]
    }
}

```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Verwalten von Computeressourcen in Azure SQL Data Warehouse](sql-data-warehouse-manage-compute-overview.md).
