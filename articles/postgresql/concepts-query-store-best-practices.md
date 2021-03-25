---
title: Bewährte Methoden für den Abfragespeicher in Azure Database for PostgreSQL – Einzelserver
description: Dieser Artikel beschreibt bewährte Methoden für den Abfragespeicher in Azure Database for PostgreSQL – Einzelserver.
author: sunilagarwal
ms.author: sunila
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: dd39b7ecd51902f5035b4cd17d59dea964d0c962
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91708831"
---
# <a name="best-practices-for-query-store"></a>Bewährte Methoden für den Abfragespeicher

**Anwendungsbereich:** Azure Database for PostgreSQL – Einzelserverversionen 9.6, 10, 11

Dieser Artikel beschreibt bewährte Methoden für die Verwendung des Abfragespeichers in Azure Database for PostgreSQL.

## <a name="set-the-optimal-query-capture-mode"></a>Festlegen des optimalen Abfrageerfassungsmodus
Lassen Sie den Abfragespeicher die Daten erfassen, die für Sie wichtig sind. 

|**pg_qs.query_capture_mode** | **Szenario**|
|---|---|
|_Alle_  |Analysieren Sie Ihre Workload sorgfältig im Hinblick auf alle Abfragen und deren Ausführungshäufigkeit und andere Statistiken. Identifizieren Sie neue Abfragen in Ihrer Workload. Erkennen Sie, ob Ad-hoc-Abfragen verwendet werden, um Möglichkeiten für Benutzer oder eine automatische Parametrisierung zu identifizieren. Bei _All_ sind die Kosten für den Ressourcenverbrauch höher. |
|_Top_  |Konzentrieren Sie sich auf die wichtigsten Abfragen – die von Clients ausgestellt wurden.
|_None_ |Sie haben bereits einen Abfragesatz und ein Zeitfenster erfasst, das Sie untersuchen möchten, und Sie möchten die nun Ablenkungen beseitigen, die ggf. durch andere Abfragen entstehen. _None_ ist für Testzwecke sowie für Benchmarkingumgebungen geeignet. _None_ sollte mit Vorsicht verwendet werden, da Sie unter Umständen keine Möglichkeit haben, wichtige neue Abfragen nachzuverfolgen und zu optimieren. Sie können keine Daten für diese vergangenen Zeitfenster wiederherstellen. |

Der Abfragespeicher enthält auch einen Speicher für Wartestatistiken. Eine zusätzliche Erfassungsmodusabfrage bestimmt Wartestatistiken: **pgms_wait_sampling.query_capture_mode** kann auf _None_ oder _All_ festgelegt werden. 

> [!NOTE] 
> **pg_qs.query_capture_mode** ersetzt **pgms_wait_sampling.query_capture_mode**. Wenn für pg_qs.query_capture_mode _None_ festgelegt ist, hat die Einstellung pgms_wait_sampling.query_capture_mode keine Auswirkungen. 


## <a name="keep-the-data-you-need"></a>Beibehalten der erforderlichen Daten
Der Parameter **pg_qs.retention_period_in_days** gibt den Datenaufbewahrungszeitraum für den Abfragespeicher in Tagen an. Ältere Abfrage- und Statistikdaten werden gelöscht. Standardmäßig ist der Abfragespeicher so konfiguriert, dass die Daten sieben Tage lang aufbewahrt werden. Speichern Sie keine historischen Daten, wenn Sie nicht beabsichtigen, diese zu verwenden. Legen Sie hierfür ggf. einen höheren Wert fest, wenn Sie die Daten länger aufbewahren möchten.


## <a name="set-the-frequency-of-wait-stats-sampling"></a>Festlegen der Häufigkeit für die Wartestatistik-Stichprobenentnahme 
Der Parameter **pgms_wait_sampling.history_period** gibt an, wie oft für Warteereignisse Stichproben entnommen werden (in Millisekunden). Je kürzer der Zeitraum, desto häufiger erfolgt die Stichprobenentnahme. Weitere Informationen werden abgerufen, wobei jedoch die Ressourcennutzung ansteigt. Vergrößern Sie diesen Zeitraum, wenn der Server ausgelastet ist oder keine Granularität erforderlich ist.


## <a name="get-quick-insights-into-query-store"></a>Schnelle Erkenntnisse zum Abfragespeicher
Nutzen Sie [Query Performance Insight](concepts-query-performance-insight.md) im Azure-Portal, um schnelle Erkenntnisse zu den Daten im Abfragespeicher zu erhalten. Die Visualisierungsoberfläche der am längsten ausgeführten Abfragen und der längsten Warteereignisse im Zeitverlauf.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie Parameter mithilfe des [Azure-Portals](howto-configure-server-parameters-using-portal.md) oder der [Azure-Befehlszeilenschnittstelle](howto-configure-server-parameters-using-cli.md) abrufen oder festlegen.