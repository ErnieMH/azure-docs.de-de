---
title: Verwenden des U-SQL-Katalogs in Azure Data Lake Analytics
description: Erfahren Sie, wie Sie den U-SQL-Katalog für die gemeinsame Nutzung von Code und Daten verwenden können. Erstellen Sie Tabellenwertfunktionen, Ansichten und Tabellen, und führen Sie Abfragen aus.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 05/09/2017
ms.openlocfilehash: f92aadc8ccf18dd91b5dd4b35285f60b174e4cf7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92220073"
---
# <a name="get-started-with-the-u-sql-catalog-in-azure-data-lake-analytics"></a>Erste Schritte mit dem U-SQL-Katalog in Azure Data Lake Analytics

## <a name="create-a-tvf"></a>Erstellen einer Tabellenwertfunktion

Im vorherigen U-SQL-Skript haben Sie die Verwendung von EXTRACT zum Lesen aus derselben Quelldatei wiederholt. Mithilfe der Tabellenwertfunktion von U-SQL können Sie die Daten zur späteren Wiederverwendung kapseln.  

Im folgenden Skript wird eine Tabellenwertfunktion mit dem Namen `Searchlog()` in der Standarddatenbank und im Standardschema erstellt:

```usql
DROP FUNCTION IF EXISTS Searchlog;

CREATE FUNCTION Searchlog()
RETURNS @searchlog TABLE
(
            UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
)
AS BEGIN
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
USING Extractors.Tsv();
RETURN;
END;
```

Das folgende Skript veranschaulicht, wie Sie die im obigen Skript definierte Tabellenwertfunktion verwenden:

```usql
@res =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM Searchlog() AS S
GROUP BY Region
HAVING SUM(Duration) > 200;

OUTPUT @res
    TO "/output/SearchLog-use-tvf.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

## <a name="create-views"></a>Erstellen von Ansichten

Wenn ein einzelner Abfrageausdruck vorhanden ist, können Sie anstelle einer Tabellenwertfunktion eine U-SQL-ANSICHT verwenden, um diesen Ausdruck zu kapseln.

Im folgenden Skript wird eine Ansicht mit dem Namen `SearchlogView` in der Standarddatenbank und im Standardschema erstellt:

```usql
DROP VIEW IF EXISTS SearchlogView;

CREATE VIEW SearchlogView AS  
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string
    FROM "/Samples/Data/SearchLog.tsv"
USING Extractors.Tsv();
```

Das folgende Skript veranschaulicht die Verwendung der definierten Ansicht:

```usql
@res =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM SearchlogView
GROUP BY Region
HAVING SUM(Duration) > 200;

OUTPUT @res
    TO "/output/Searchlog-use-view.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

## <a name="create-tables"></a>Erstellen von Tabellen
Ähnlich wie bei relationalen Datenbanktabellen ermöglicht U-SQL Ihnen die Erstellung einer Tabelle mit einem vordefinierten Schema. Sie können auch eine Tabelle erstellen und das Schema aus der Abfrage ableiten, mit der die Tabelle aufgefüllt wird (auch als CREATE TABLE AS SELECT oder CTAS bezeichnet).

Erstellen Sie mithilfe des folgenden Skripts eine Datenbank und zwei Tabellen:

```usql
DROP DATABASE IF EXISTS SearchLogDb;
CREATE DATABASE SearchLogDb;
USE DATABASE SearchLogDb;

DROP TABLE IF EXISTS SearchLog1;
DROP TABLE IF EXISTS SearchLog2;

CREATE TABLE SearchLog1 (
            UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int?,
            Urls            string,
            ClickedUrls     string,

            INDEX sl_idx CLUSTERED (UserId ASC)
                DISTRIBUTED BY HASH (UserId)
);

INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

CREATE TABLE SearchLog2(
    INDEX sl_idx CLUSTERED (UserId ASC)
            DISTRIBUTED BY HASH (UserId)
) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here
```

## <a name="query-tables"></a>Abfragen von Tabellen
Sie können Tabellen wie die im obigen Skript erstellten auf die gleiche Weise wie Datendateien abfragen. Anstatt mit EXTRACT ein Rowset zu erstellen, können Sie jetzt auf den Tabellennamen verweisen.

Um aus den Tabellen zu lesen, ändern Sie das zuvor verwendete Transformationsskript:

```usql
@rs1 =
    SELECT
        Region,
        SUM(Duration) AS TotalDuration
    FROM SearchLogDb.dbo.SearchLog2
GROUP BY Region;

@res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

OUTPUT @res
    TO "/output/Searchlog-query-table.csv"
    ORDER BY TotalDuration DESC
    USING Outputters.Csv();
```

 >[!NOTE]
 >Derzeit können Sie eine SELECT-Anweisung für eine Tabelle nicht im gleichen Skript ausführen, in dem Sie die Tabelle erstellt haben.

## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
* [Überwachung und Problembehandlung von Azure Data Lake Analytics-Aufträgen mithilfe des Azure-Portals](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
