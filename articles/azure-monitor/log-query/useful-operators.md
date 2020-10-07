---
title: Hilfreiche Operatoren in Azure Monitor-Protokollabfragen | Microsoft-Dokumentation
description: Allgemeine Funktionen zur Verwendung für verschiedene Szenarien in Azure Monitor-Protokollabfragen.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/21/2018
ms.openlocfilehash: ff63b9b7027e99c70971230936ed98186c2208e8
ms.sourcegitcommit: ffa7a269177ea3c9dcefd1dea18ccb6a87c03b70
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2020
ms.locfileid: "91607253"
---
# <a name="useful-operators-in-azure-monitor-log-queries"></a>Hilfreiche Operatoren in Azure Monitor-Protokollabfragen

In der folgenden Tabelle sind einige allgemeine Funktionen zur Verwendung für verschiedene Szenarien in Azure Monitor-Protokollabfragen angegeben.

## <a name="useful-operators"></a>Hilfreiche Operatoren

Category                                |Relevante Analytics-Funktion
----------------------------------------|----------------------------------------
Auswahl- und Spaltenaliase            |`project`, `project-away`, `extend`
Temporäre Tabellen und Konstanten          |`let scalar_alias_name = …;` <br> `let table_alias_name =  …  …  … ;`| 
Vergleichs- und Zeichenfolgenoperatoren         |`startswith`, `!startswith`, `has`, `!has` <br> `contains`, `!contains`, `containscs` <br> `hasprefix`, `!hasprefix`, `hassuffix`, `!hassuffix`, `in`, `!in` <br> `matches regex` <br> `==`, `=~`, `!=`, `!~`
Allgemeine Zeichenfolgenfunktionen                 |`strcat()`, `replace()`, `tolower()`, `toupper()`, `substring()`, `strlen()`
Allgemeine mathematische Funktionen                   |`sqrt()`, `abs()` <br> `exp()`, `exp2()`, `exp10()`, `log()`, `log2()`, `log10()`, `pow()` <br> `gamma()`, `gammaln()`
Analysieren von Text                            |`extract()`, `extractjson()`, `parse`, `split()`
Beschränken der Ausgabe                         |`take`, `limit`, `top`, `sample`
Datumsfunktionen                          |`now()`, `ago()` <br> `datetime()`, `datepart()`, `timespan` <br> `startofday()`, `startofweek()`, `startofmonth()`, `startofyear()` <br> `endofday()`, `endofweek()`, `endofmonth()`, `endofyear()` <br> `dayofweek()`, `dayofmonth()`, `dayofyear()` <br> `getmonth()`, `getyear()`, `weekofyear()`, `monthofyear()`
Gruppierung und Aggregation                |`summarize by` <br> `max()`, `min()`, `count()`, `dcount()`, `avg()`, `sum()` <br> `stddev()`, `countif()`, `dcountif()`, `argmax()`, `argmin()` <br> `percentiles()`, `percentile_array()`
Joins und Unions                        |`join kind=leftouter`, `inner`, `rightouter`, `fullouter`, `leftanti` <br> `union`
Sortieren, Reihenfolge                             |`sort`, `order` 
Dynamisches Objekt (JSON und Array)         |`parsejson()` <br> `makeset()`, `makelist()` <br> `split()`, `arraylength()` <br> `zip()`, `pack()`
Logische Operatoren                       |`and`, `or`, `iff(condition, value_t, value_f)` <br> `binary_and()`, `binary_or()`, `binary_not()`, `binary_xor()`
Machine Learning                        |`evaluate autocluster`, `basket`, `diffpatterns`, `extractcolumns`


## <a name="next-steps"></a>Nächste Schritte

- Arbeiten Sie eine Lektion über das [Schreiben von Protokollabfragen in Azure Monitor](get-started-queries.md) durch.
