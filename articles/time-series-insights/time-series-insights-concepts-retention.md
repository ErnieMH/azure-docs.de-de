---
title: Grundlegendes zur Datenaufbewahrung in Ihrer Umgebung – Azure Time Series Insight | Microsoft-Dokumentation
description: In diesem Artikel werden zwei Einstellungen beschrieben, mit denen die Datenaufbewahrung in Ihrer Azure Time Series Insights-Umgebung gesteuert wird.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.reviewer: jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 09/29/2020
ms.custom: seodec18
ms.openlocfilehash: 4f236679d0662df852581a6a8408ed6bc0d4e3fe
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91535688"
---
# <a name="understand-data-retention-in-azure-time-series-insights-gen1"></a>Grundlagen der Datenaufbewahrung in Azure Time Series Insights Gen1

> [!CAUTION]
> Dies ist ein Artikel zu Azure Time Series Insights Gen1.

In diesem Artikel werden zwei primäre Einstellungen beschrieben, die sich auf die Datenaufbewahrung in Ihrer Azure Time Series Insights-Umgebung auswirken.

## <a name="video"></a>Video

### <a name="the-following-video-summarizes-azure-time-series-insights-data-retention-and-how-to-plan-for-itbr"></a>Das folgende Video ist eine Zusammenfassung der Azure Time Series Insights-Datenaufbewahrung und der Planung dafür.</br>

> [!VIDEO https://www.youtube.com/embed/03x6zKDQ6DU]

Jede Ihrer Azure Time Series-Umgebungen besitzt eine Einstellung, die **Datenaufbewahrungszeit** kontrolliert. Der Wert kann zwischen 1 und 400 Tagen liegen. Die Daten werden anhand der Speicherkapazität der Umgebung oder der Aufbewahrungsdauer gelöscht – je nachdem, welcher Fall zuerst eintritt.

Darüber hinaus gibt es in Ihrer Azure Time Series Insights-Umgebung die Einstellung **Verhalten bei Überschreitung des Speicherlimits**. Sie steuert das Verhalten bei Eingang und Bereinigung, wenn in einer Umgebung die maximale Kapazität erreicht ist. Es stehen zwei Verhalten zur Auswahl bei der Konfiguration:

- **Alte Daten bereinigen** (Standard)  
- **Eingang anhalten**

> [!NOTE]
> Bei der Erstellung einer neuen Umgebung wird für die Aufbewahrung standardmäßig **Purge old data** (Alte Daten bereinigen) konfiguriert. Diese Einstellung kann nach der Erstellung im Azure-Portal auf der Seite **Konfigurieren** der Azure Time Series Insights-Umgebung je nach Bedarf geändert werden.
>
> - Informationen zum Konfigurieren der Aufbewahrungsrichtlinien finden Sie unter [Konfigurieren der Datenaufbewahrung in Azure Time Series Insights Gen1](time-series-insights-how-to-configure-retention.md).

Beide Datenaufbewahrungsrichtlinien werden weiter unten ausführlicher beschrieben.

## <a name="purge-old-data"></a>Purge old data (Alte Daten bereinigen)

- **Alte Daten bereinigen** ist die Standardeinstellung für Azure Time Series Insights-Umgebungen.  
- **Alte Daten bereinigen** ist zu bevorzugen, wenn Benutzer in ihrer Azure Time Series Insights-Umgebung immer die *aktuellsten Daten* angezeigt bekommen möchten.
- Durch die Einstellung **Alte Daten bereinigen** werden Daten *bereinigt*, wenn die Grenzwerte der Umgebung erreicht sind (Aufbewahrungsdauer, Größe oder Anzahl, je nachdem, was zuerst eintritt). Die Aufbewahrungsdauer ist standardmäßig auf 30 Tage festgelegt.
- Die ältesten erfassten Daten werden zuerst bereinigt (FIFO-Ansatz, First In First Out).

### <a name="example-one"></a>Beispiel eins

Wir sehen uns nun eine Beispielumgebung an, für die das Aufbewahrungsverhalten **Continue ingress and purge old data** (Eingang fortsetzen und alte Daten bereinigen) festgelegt ist:

Die **Datenaufbewahrungszeit** ist auf 400 Tage festgelegt. Die **Kapazität** ist auf die S1-Einheit festgelegt. Dies entspricht einer Gesamtkapazität von 30 GB. Angenommen, jeden Tag fallen durchschnittlich 500 MB an eingehenden Daten an. In dieser Umgebung können bei dieser Eingangsrate nur Daten für 60 Tage aufbewahrt werden, da nach 60 Tagen die maximale Kapazität erreicht ist. Für die Akkumulation der eingehenden Daten gilt Folgendes: 500 MB pro Tag x 60 Tage = 30 GB.

Am 61. Tag werden in der Umgebung die aktuellsten Daten angezeigt; Daten, die älter als 60 Tage sind, werden bereinigt. Durch die Bereinigung wird Platz für die neu eintreffenden Daten geschaffen, damit weiter neue Daten untersucht werden können. Falls Benutzer die Daten länger aufbewahren möchten, können sie die Größe der Umgebung erhöhen, indem sie zusätzliche Einheiten hinzufügen oder weniger Daten per Pushvorgang übertragen.  

### <a name="example-two"></a>Beispiel zwei

Wir sehen uns nun eine Umgebung an, für die wiederum das Aufbewahrungsverhalten **Continue ingress and purge old data** (Eingang fortsetzen und alte Daten bereinigen) festgelegt ist. In diesem Beispiel ist die **Datenaufbewahrungszeit** aber auf einen niedrigeren Wert von 180 Tagen festgelegt. Die **Kapazität** ist auf die S1-Einheit festgelegt. Dies entspricht einer Gesamtkapazität von 30 GB. Um Daten für die gesamten 180 Tage speichern zu können, darf die Menge an eingehenden Daten 0,166 GB (166 MB) pro Tag nicht überschreiten.  

Immer wenn die tägliche Eingangsrate für diese Umgebung über den Wert von 0,166 GB pro Tag steigt, können Daten nicht 180 Tage lang aufbewahrt werden, da einige Daten bereinigt werden. Stellen Sie sich diese Umgebung nun für Zeiten mit hoher Auslastung vor. Angenommen, die Eingangsrate der Umgebung steigt auf durchschnittlich 0,189 GB pro Tag an. Während dieser Zeit mit hoher Auslastung werden Daten etwa 158 Tage lange aufbewahrt (30GB/0,189 = 158,73 Aufbewahrungstage). Dieser Zeitraum ist kürzer als der gewünschte Zeitrahmen für die Datenaufbewahrung.

## <a name="pause-ingress"></a>Pause ingress (Eingang anhalten)

- Mit der Einstellung **Pause ingress** soll sichergestellt werden, dass Daten nicht bereinigt werden, wenn die Grenzwerte für die Größe und Anzahl vor dem Ende des Aufbewahrungszeitraums erreicht werden.  
- Durch **Pause ingress** erhalten Benutzer zusätzliche Zeit, um die Kapazität ihrer Umgebung zu erhöhen, bevor Daten bereinigt werden, weil die Regeln für den Aufbewahrungszeitraum verletzt wurden.
- Dadurch werden Sie vor einem Datenverlust geschützt. Es kann aber auch geschehen, dass Ihre aktuellsten Daten verloren gehen, wenn der Dateneingang über den Aufbewahrungszeitraum Ihrer Ereignisquelle hinaus angehalten wird.
- Nachdem aber die maximale Kapazität einer Umgebung erreicht wurde, wird der Dateneingang von der Umgebung so lange angehalten, bis folgende zusätzliche Aktionen durchgeführt wurden:

  - Sie erhöhen die maximale Kapazität der Umgebung, um entsprechend der Beschreibung unter [Vorgehensweise zur Skalierung Ihrer Azure Time Series Insights Gen1-Umgebung](time-series-insights-how-to-scale-your-environment.md) weitere Skalierungseinheiten hinzuzufügen.
  - Der Zeitraum für die Datenaufbewahrung ist erreicht, und Daten werden bereinigt, wodurch die maximale Kapazität für die Umgebung überschritten wird.

### <a name="example-three"></a>Beispiel drei

Bei der nächsten Umgebung ist für das Aufbewahrungsverhalten **Pause ingress** (Eingang anhalten) konfiguriert. In diesem Beispiel ist die **Datenaufbewahrungszeit** auf 60 Tage festgelegt. Die **Kapazität** ist auf drei (3) S1-Einheiten festgelegt. Angenommen, diese Umgebung verfügt pro Tag über einen Dateneingang von 2 GB. In dieser Umgebung wird der Dateneingang angehalten, nachdem die maximale Kapazität erreicht ist.

Anschließend wird in der Umgebung dasselbe Dataset angezeigt, bis der Dateneingang fortgesetzt oder **continue ingress** (Eingang fortsetzen) aktiviert wird (sodass ältere Daten wieder bereinigt würden, um Platz für neue Daten zu schaffen).

Bei Fortsetzung des Dateneingangs:

- Daten fließen in der Reihenfolge, in der sie von der Ereignisquelle empfangen wurden.
- Die Ereignisse werden basierend auf ihrem Zeitstempel indiziert, sofern Sie nicht gegen die Aufbewahrungsrichtlinien auf Ihrer Ereignisquelle verstoßen haben. Weitere Informationen zur Konfiguration der Aufbewahrung auf der Ereignisquelle finden Sie unter [Häufig gestellte Fragen zu Event Hubs](../event-hubs/event-hubs-faq.md).

> [!IMPORTANT]
> Sie sollten Warnungen festlegen, damit Hinweise angezeigt werden und möglichst verhindert wird, dass der Dateneingang angehalten wird. Es kann zu Datenverlust kommen, da die Standardaufbewahrungsdauer für Azure-Ereignisquellen einen Tag beträgt. Nachdem der Dateneingang angehalten wurde, gehen daher vermutlich die aktuellsten Daten verloren, sofern Sie nicht weitere Maßnahmen ergreifen. Sie müssen die Kapazität erhöhen oder das Verhalten auf **Purge old data** (Alte Daten bereinigen) festlegen, um potenzielle Datenverluste zu vermeiden.

Erwägen Sie für die betroffenen Event Hubs-Instanzen die Anpassung der Eigenschaft **Nachrichtenaufbewahrung**, um Datenverlust zu verringern, wenn in Azure Time Series Insights der Dateneingang angehalten wird.

[![Event Hub-Nachrichtenbeibehaltung.](media/time-series-insights-concepts-retention/event-hub-retention.png)](media/time-series-insights-concepts-retention/event-hub-retention.png#lightbox)

Wenn für die Ereignisquelle (`timeStampPropertyName`) keine Eigenschaften konfiguriert werden, wird für Azure Time Series Insights der Zeitstempel der Ankunft im Event Hub standardmäßig als X-Achse verwendet. Wenn `timeStampPropertyName` als etwas Anderes konfiguriert wird, sucht die Umgebung im Datenpaket bei der Analyse von Ereignissen nach dem konfigurierten `timeStampPropertyName`.

Informationen zum Skalieren Ihrer Umgebung, um zusätzliche Kapazität bereitzustellen oder den Aufbewahrungszeitraum zu verlängern, finden Sie unter [Vorgehensweise zur Skalierung Ihrer Azure Time Series Insights Gen1-Umgebung](time-series-insights-how-to-scale-your-environment.md).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Konfigurieren oder Ändern von Einstellungen für die Datenaufbewahrung finden Sie unter [Konfigurieren der Datenaufbewahrung in Azure Time Series Insights Gen1](time-series-insights-how-to-configure-retention.md).

- Informationen zum [Mildern von Wartezeiten in Azure Time Series Insights](time-series-insights-environment-mitigate-latency.md).
