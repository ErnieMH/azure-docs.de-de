---
title: Azure Monitor – Unterschiede in der Protokollabfragesprache | Microsoft-Dokumentation
description: Referenzinformationen für die in Azure Monitor verwendete Abfragesprache Kusto. Enthält zusätzliche Elemente, die spezifisch für Azure Monitor sind, und Elemente, die in Azure Monitor-Protokollabfragen nicht unterstützt werden.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 04/01/2020
ms.openlocfilehash: bfa27b0df7febbfb8c97f11f69f87c352810699b
ms.sourcegitcommit: 83610f637914f09d2a87b98ae7a6ae92122a02f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91993184"
---
# <a name="azure-monitor-log-query-language-differences"></a>Azure Monitor – Unterschiede in der Protokollabfragesprache

Zwar basieren [Protokolle in Azure Monitor](log-query-overview.md) auf [Azure Data Explorer](/azure/data-explorer) und verwenden die gleiche [Abfragesprache Kusto](/azure/kusto/query), die Sprachversion bringt jedoch einige Unterschiede mit sich. In diesem Artikel werden Elemente benannt, die sich in der für Data Explorer und der für Azure Monitor-Protokollabfragen verwendeten Version der Sprache unterscheiden.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="kql-elements-not-supported-in-azure-monitor"></a>In Azure Monitor nicht unterstützte Elemente der Abfragesprache Kusto
In den folgenden Abschnitten werden Elemente der Abfragesprache Kusto beschrieben, die in Azure Monitor nicht unterstützt werden.

### <a name="statements-not-supported-in-azure-monitor"></a>In Azure Monitor nicht unterstützte Anweisungen

* [Alias](/azure/kusto/query/aliasstatement)
* [Abfrageparameter](/azure/kusto/query/queryparametersstatement)

### <a name="functions-not-supported-in-azure-monitor"></a>In Azure Monitor nicht unterstützte Funktionen

* [cluster()](/azure/kusto/query/clusterfunction)
* [cursor_after()](/azure/kusto/query/cursorafterfunction)
* [cursor_before_or_at()](/azure/kusto/query/cursorbeforeoratfunction)
* [cursor_current(), current_cursor()](/azure/kusto/query/cursorcurrent)
* [database()](/azure/kusto/query/databasefunction)
* [current_principal()](/azure/kusto/query/current-principalfunction)
* [extent_id()](/azure/kusto/query/extentidfunction)
* [extent_tags()](/azure/kusto/query/extenttagsfunction)

### <a name="operators-not-supported-in-azure-monitor"></a>In Azure Monitor nicht unterstützte Operatoren

* [Clusterübergreifender Join-Vorgang](/azure/kusto/query/joincrosscluster)

### <a name="plugins-not-supported-in-azure-monitor"></a>In Azure Monitor nicht unterstützte Plug-Ins

* [Python-Plug-In](/azure/kusto/query/pythonplugin)
* [Plug-In „sql_request“](/azure/kusto/query/sqlrequestplugin)


## <a name="additional-operators-in-azure-monitor"></a>Zusätzliche Operatoren in Azure Monitor
Die folgenden Operatoren unterstützen für Azure Monitor spezifische Features und stehen außerhalb von Azure Monitor nicht zur Verfügung.

* [app()](app-expression.md)
* [workspace()](workspace-expression.md)

## <a name="next-steps"></a>Nächste Schritte

- Abrufen von Verweisen auf verschiedene [Ressourcen zum Schreiben von Azure Monitor-Protokollabfragen](/azure/data-explorer/kusto/query/).
- Greifen Sie auf die vollständige [Referenzdokumentation für die Abfragesprache Kusto](/azure/kusto/query/) zu.