---
title: Azure Application Insights-Übersichtsdashboard | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Anwendungen mit Azure Application Insights und dem Übersichtsdashboard überwachen.
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 714488d88b269f252dfd6677f847db315969a270
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90970604"
---
# <a name="application-insights-overview-dashboard"></a>Application Insights-Übersichtsdashboard

Application Insights hat schon immer einen Übersichtsbereich mit einer Zusammenfassung enthalten, um Ihnen eine schnelle Bewertung der Integrität und Leistung Ihrer Anwendung auf einen Blick zu ermöglichen. Die neue Version des Übersichtsdashboards ist jetzt noch schneller und flexibler.

## <a name="how-do-i-test-out-the-new-experience"></a>Wie kann ich die neue Oberfläche testen?

Das neue Übersichtsdashboard wird jetzt standardmäßig gestartet:

![Vorschauversion des Übersichtsbereichs](./media/overview-dashboard/overview.png)

## <a name="better-performance"></a>Bessere Leistung

Die Auswahl des Zeitbereichs wurde zu einer 1-Klick-Oberfläche vereinfacht.

![Uhrzeitbereich](./media/overview-dashboard/app-insights-overview-dashboard-03.png)

Die Gesamtleistung wurde deutlich erhöht. Mit nur einem Klick haben Sie Zugriff auf beliebte Features wie **Suche** und **Analyse**. Jede KPI-Kachel, die standardmäßig dynamisch aktualisiert wird, bietet Einblick in die entsprechenden Application Insights-Features. Um weitere Informationen über Fehler bei Anforderungen zu erhalten, wählen Sie **Fehler** unter dem Header **Untersuchen** aus:

![Fehler](./media/overview-dashboard/app-insights-overview-dashboard-04.png)

## <a name="application-dashboard"></a>Anwendungsdashboard

Im Anwendungsdashboard wird die vorhandene Dashboardtechnologie in Azure genutzt, um eine vollständig anpassbare einzelne Bereichsansicht Ihrer Anwendungsintegrität und -leistung bereitzustellen.

Wählen Sie oben links die Option _Anwendungsdashboard_, um auf das Standarddashboard zuzugreifen.

![Screenshot zeigt die markierte Schaltfläche für das Anwendungsdashboard.](./media/overview-dashboard/app-insights-overview-dashboard-05.png)

Wenn Sie zum ersten Mal auf das Dashboard zugreifen, wird eine Standardansicht gestartet:

![Dashboardansicht](./media/overview-dashboard/0001-dashboard.png)

Sie können die Standardansicht beibehalten, wenn Sie möchten. Alternativ können Sie dem Dashboard auch Elemente hinzufügen und daraus löschen, um die Anforderungen Ihres Teams bestmöglich zu erfüllen.

> [!NOTE]
> Die Dashboardumgebung ist für alle Benutzer mit Zugriff auf die Application Insights-Ressource gleich. Von einem Benutzer vorgenommene Änderungen wirken sich für alle Benutzer auf die Ansicht aus.

Wählen Sie Folgendes, um zurück zur Übersichtsoberfläche zu navigieren:

![Schaltfläche „Übersicht“](./media/overview-dashboard/app-insights-overview-dashboard-07.png)

## <a name="troubleshooting"></a>Problembehandlung

Wenn Sie **Kacheleinstellungen konfigurieren** auswählen und einen benutzerdefinierten Zeitbereich über 31 Tagen festlegen, werden im Dashboard keine Daten angezeigt, die über 31 Tage hinausgehen. Dies gilt selbst bei der standardmäßigen Datenaufbewahrung von 90 Tagen. Für dieses Verhalten gibt es derzeit keine Problemumgehung.

## <a name="next-steps"></a>Nächste Schritte

- [Trichter](./usage-funnels.md)
- [Vermerkdauer](./usage-retention.md)
- [Benutzerabläufe](./usage-flows.md)

