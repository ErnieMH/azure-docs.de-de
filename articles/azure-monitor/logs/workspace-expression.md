---
title: workspace()-Ausdruck in Azure Monitor-Protokollabfragen | Microsoft-Dokumentation
description: Der workspace-Ausdruck wird in Azure Monitor-Protokollabfragen verwendet, um Daten aus einem bestimmten Arbeitsbereich in der gleichen Ressourcengruppe, einer anderen Ressourcengruppe oder einem anderen Abonnement abzurufen.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/10/2018
ms.openlocfilehash: 2f6eb3998c611cb7a72886d1c577c665d73cb5a2
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102035567"
---
# <a name="workspace-expression-in-azure-monitor-log-query"></a>workspace()-Ausdruck in Azure Monitor-Protokollabfragen

Der Ausdruck `workspace` wird in Azure Monitor-Abfragen verwendet, um Daten aus einem bestimmten Arbeitsbereich in der gleichen Ressourcengruppe, einer anderen Ressourcengruppe oder einem anderen Abonnement abzurufen. Dies ist nützlich, um Protokolldaten in einer Application Insights-Abfrage einzuschließen und um Daten übergreifend über mehrere Arbeitsbereiche in einer Protokollabfrage abzufragen.


## <a name="syntax"></a>Syntax

`workspace(`*Bezeichner*`)`

## <a name="arguments"></a>Argumente

- *Identifier*: identifiziert den Arbeitsbereich mit einem der Formate in der folgenden Tabelle.

| Bezeichner | BESCHREIBUNG | Beispiel
|:---|:---|:---|
| Ressourcenname | Für Menschen lesbarer Name des Arbeitsbereichs (auch als „Komponentenname“ bezeichnet) | workspace("contosoretail") |
| Qualifizierter Name | Vollständiger Name des Arbeitsbereichs in der Form: „Abonnementname/Ressourcengruppe/Komponentenname“ | workspace('Contoso/ContosoResource/ContosoWorkspace') |
| ID | GUID des Arbeitsbereichs | workspace("b438b3f6-912a-46d5-9db1-b42069242ab4") |
| Azure-Ressourcen-ID | Bezeichner für die Azure-Ressource | workspace("/subscriptions/e4227-645-44e-9c67-3b84b5982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail") |


## <a name="notes"></a>Notizen

* Sie benötigen Lesezugriff auf den Arbeitsbereich.
* Ein verwandter Ausdruck ist `app`, der Ihnen das übergreifende Abfragen von Application Insights-Anwendungen ermöglicht.

## <a name="examples"></a>Beispiele

```Kusto
workspace("contosoretail").Update | count
```
```Kusto
workspace("b438b4f6-912a-46d5-9cb1-b44069212ab4").Update | count
```
```Kusto
workspace("/subscriptions/e427267-5645-4c4e-9c67-3b84b59a6982/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
```
```Kusto
union 
(workspace("myworkspace").Heartbeat | where Computer contains "Con"),
(app("myapplication").requests | where cloud_RoleInstance contains "Con")
| count  
```
```Kusto
union 
(workspace("myworkspace").Heartbeat), (app("myapplication").requests)
| where TimeGenerated between(todatetime("2018-02-08 15:00:00") .. todatetime("2018-12-08 15:05:00"))
```

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Verweisen auf Application Insights-Apps finden Sie unter [app-Ausdruck](./app-expression.md).
- Erfahren Sie mehr zum Speichern von [Azure Monitor-Daten](./log-query-overview.md).
- Greifen Sie auf die vollständige Dokumentation für die [Abfragesprache Kusto](/azure/kusto/query/) zu.