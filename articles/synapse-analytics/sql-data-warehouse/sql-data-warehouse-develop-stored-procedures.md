---
title: Verwenden gespeicherter Prozeduren
description: Tipps zum Entwickeln von Lösungen durch Implementieren gespeicherter Prozeduren im Synapse SQL-Pool.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/02/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 87c7eaa57f9da87bd83f89953afc09632d42b1f8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85213396"
---
# <a name="using-stored-procedures-in-synapse-sql-pool"></a>Verwenden gespeicherter Prozeduren im Synapse SQL-Pool

Dieser Artikel enthält Tipps zum Entwickeln von SQL-Poollösungen durch Implementieren gespeicherter Prozeduren.

## <a name="what-to-expect"></a>Ausblick

Der SQL-Pool unterstützt viele der T-SQL-Features, die in SQL Server verwendet werden. Darüber hinaus sind bestimmte Features für das horizontale Hochskalieren vorhanden, mit denen Sie die Leistung Ihrer Lösung verbessern können.

Außerdem gibt es zusätzliche Features und Funktionen, die Verhaltensunterschiede aufweisen, damit Sie die Skalierung und Leistung des SQL-Pools aufrechterhalten können.

## <a name="introducing-stored-procedures"></a>Einführung in gespeicherte Prozeduren

Gespeicherte Prozeduren sind eine hervorragende Möglichkeit zum Kapseln Ihres SQL-Codes, der in der Nähe Ihrer SQL-Pooldaten gespeichert wird. Gespeicherte Prozeduren unterstützen Entwickler auch beim Modularisieren ihrer Lösungen, indem Code in besser verwaltbare Einheiten eingeschlossen (gekapselt) wird. Dies ermöglicht eine bessere Wiederverwendbarkeit des Codes. Für jede gespeicherte Prozedur können außerdem Parameter verwendet werden, um sie flexibler zu machen.

Der SQL-Pool bietet eine vereinfachte und optimierte Implementierung von gespeicherten Prozeduren. Der größte Unterschied im Vergleich zu SQL Server ist, dass die gespeicherte Prozedur kein vorab kompilierter Code ist.

In Data Warehouses ist die Kompilierzeit im Allgemeinen im Vergleich zu der benötigten Zeit, um Abfragen für große Datenmengen auszuführen, relativ gering. Es ist wichtiger sicherzustellen, dass der Code der gespeicherten Prozedur für große Abfragen ordnungsgemäß optimiert ist.

> [!TIP]
> Das Ziel besteht darin, Stunden, Minuten und Sekunden zu sparen, keine Millisekunden. Es ist daher hilfreich, gespeicherte Prozeduren als Container für SQL-Logik zu betrachten.

Wenn der SQL-Pool Ihre gespeicherte Prozedur ausführt, werden die SQL-Anweisungen zur Laufzeit analysiert, übersetzt und optimiert. Während dieses Vorgangs wird jede Anweisung in verteilte Abfragen konvertiert. Der SQL-Code, der für die Daten ausgeführt wird, unterscheidet sich von der übermittelten Abfrage.

## <a name="nesting-stored-procedures"></a>Schachteln von gespeicherten Prozeduren

Wenn gespeicherte Prozeduren andere gespeicherte Prozeduren aufrufen oder dynamischen SQL-Code ausführen, wird die innere gespeicherte Prozedur bzw. der Codeaufruf als „geschachtelt“ bezeichnet.

Der SQL-Pool unterstützt maximal acht Schachtelungsebenen. Im Gegensatz dazu sind in SQL Server 32 Schachtelungsebenen zulässig.

Der Aufruf der obersten gespeicherten Prozedur entspricht Schachtelungsebene 1.

```sql
EXEC prc_nesting
```

Wenn die gespeicherte Prozedur auch einen weiteren EXEC-Aufruf durchführt, wird die Schachtelungsebene auf 2 erhöht.

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```

Wenn die zweite Prozedur dann dynamischen SQL-Code ausführt, wird die Schachtelungsebene auf 3 erhöht.

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Der SQL-Pool unterstützt [@@NESTLEVEL](/sql/t-sql/functions/nestlevel-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) derzeit nicht. Daher müssen Sie die Schachtelungsebene nachverfolgen. Es ist unwahrscheinlich, dass Sie den Grenzwert von acht Schachtelungsebenen überschreiten. Wenn dies dennoch der Fall ist, müssen Sie Ihren Code so überarbeiten, dass er den Schachtelungsebenen innerhalb dieses Grenzwerts entspricht.

## <a name="insertexecute"></a>INSERT..EXECUTE

Der SQL-Pool lässt nicht zu, dass Sie das Resultset einer gespeicherten Prozedur mit einer INSERT-Anweisung verwenden. Es gibt jedoch eine alternative Methode, die Sie verwenden können. Ein Beispiel finden Sie im Artikel unter [Temporäre Tabellen](sql-data-warehouse-tables-temporary.md).

## <a name="limitations"></a>Einschränkungen

Es gibt einige Aspekte von gespeicherten Transact-SQL-Prozeduren, die im SQL-Pool nicht implementiert sind:

* Temporäre gespeicherte Prozeduren
* Nummerierte gespeicherte Prozeduren
* Erweiterte gespeicherte Prozeduren
* Gespeicherte CLR-Prozeduren
* Verschlüsselungsoption
* Replikationsoption
* Tabellenwertparameter
* Schreibgeschützte Parameter
* Standardparameter
* Ausführungskontexte
* return-Anweisung

## <a name="next-steps"></a>Nächste Schritte

Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht](sql-data-warehouse-overview-develop.md).
