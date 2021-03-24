---
title: resource()-Ausdruck in Azure Monitor-Protokollabfragen | Microsoft-Dokumentation
description: Der resource-Ausdruck wird in einer Azure Monitor-Protokollabfrage mit Ressourcenschwerpunkt verwendet, um Daten von mehreren Ressourcen abzurufen.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/10/2018
ms.openlocfilehash: 9a5e5c7959a243d6c9d243b706524f624ffa3cdb
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102047212"
---
# <a name="resource-expression-in-azure-monitor-log-query"></a>resource()-Ausdruck in Azure Monitor-Protokollabfragen

Der `resource`-Ausdruck wird in einer Azure Monitor-Abfrage [mit dem Umfang einer Ressource](scope.md#query-scope) verwendet, um Daten von anderen Ressourcen abzurufen. 


## <a name="syntax"></a>Syntax

`resource(`*Bezeichner*`)`

## <a name="arguments"></a>Argumente

- *Bezeichner*: Die Ressourcen-ID einer Ressource.

| Bezeichner | BESCHREIBUNG | Beispiel
|:---|:---|:---|
| Resource | Schließt Daten für die Ressource ein. | resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup/providers/microsoft.compute/virtualmachines/myvm") |
| Ressourcengruppe oder Abonnement | Enthält Daten für die Ressource und alle darin enthaltenen Ressourcen.  | resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup) |


## <a name="notes"></a>Notizen

* Sie benötigen Lesezugriff auf die Ressource.


## <a name="examples"></a>Beispiele

```Kusto
union (Heartbeat),(resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup/providers/microsoft.compute/virtualmachines/myvm").Heartbeat) | summarize count() by _ResourceId, TenantId
```
```Kusto
union (Heartbeat),(resource("/subscriptions/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcesgroups/myresourcegroup).Heartbeat) | summarize count() by _ResourceId, TenantId
```


## <a name="next-steps"></a>Nächste Schritte

- Details zum Abfragebereich finden Sie unter [Protokollabfragebereich und Zeitbereich in Azure Monitor Log Analytics](scope.md).
- Greifen Sie auf die vollständige Dokumentation für die [Abfragesprache Kusto](/azure/kusto/query/) zu.
