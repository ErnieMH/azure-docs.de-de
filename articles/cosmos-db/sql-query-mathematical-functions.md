---
title: Mathematische Funktionen in der Abfragesprache für Azure Cosmos DB
description: Erfahren Sie etwas über die mathematischen Funktionen in Azure Cosmos DB, mit denen Berechnungen basierend auf Eingabewerten, die als Argument bereitgestellt werden und einen numerischen Wert zurückgeben, durchgeführt werden.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: bd53feb175c5be77f559a4d2e724a55e41df48eb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85562830"
---
# <a name="mathematical-functions-azure-cosmos-db"></a>Mathematische Funktionen (Azure Cosmos DB)  

Jede mathematische Funktion führt eine Berechnung durch, basierend auf Eingabewerten, die als Argument bereitgestellt werden, und gibt einen numerischen Wert zurück.

Sie können Abfragen wie im folgenden Beispiel ausführen:

```sql
    SELECT VALUE ABS(-4)
```

Es wird folgendes Ergebnis ausgegeben:

```json
    [4]
```

## <a name="functions"></a>Functions

Die folgenden unterstützten integrierten mathematischen Funktionen führen eine Berechnung durch, die normalerweise auf Eingabeargumenten basiert, und geben einen numerischen Ausdruck zurück:
 
* [ABS](sql-query-abs.md)
* [ACOS](sql-query-acos.md)
* [ASIN](sql-query-asin.md)
* [ATAN](sql-query-atan.md)
* [ATN2](sql-query-atn2.md)
* [CEILING](sql-query-ceiling.md)
* [COS](sql-query-cos.md)
* [COT](sql-query-cot.md)
* [DEGREES](sql-query-degrees.md)
* [EXP](sql-query-exp.md)
* [FLOOR](sql-query-floor.md)
* [LOG](sql-query-log.md)
* [LOG10](sql-query-log10.md)
* [PI](sql-query-pi.md)
* [POWER](sql-query-power.md)
* [RADIANS](sql-query-radians.md)
* [RAND](sql-query-rand.md)
* [ROUND](sql-query-round.md)
* [SIGN](sql-query-sign.md)
* [SIN](sql-query-sin.md)
* [SQRT](sql-query-sqrt.md)
* [SQUARE](sql-query-square.md)
* [TAN](sql-query-tan.md)
* [TRUNC](sql-query-trunc.md)

  
Alle mathematischen Funktionen, mit Ausnahme von RAND, sind deterministische Funktionen. Dies bedeutet, dass sie bei jedem Aufruf mit einer bestimmten Gruppe von Eingabewerten dieselben Ergebnisse zurückgeben.

## <a name="next-steps"></a>Nächste Schritte

- [Systemfunktionen in Azure Cosmos DB](sql-query-system-functions.md)
- [Einführung in Azure Cosmos DB](introduction.md)
- [Benutzerdefinierte Funktionen](sql-query-udfs.md)
- [Aggregate](sql-query-aggregates.md)
