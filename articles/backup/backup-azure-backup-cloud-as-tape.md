---
title: Ersetzen Ihrer Bandinfrastruktur
description: Hier erfahren Sie, wie Azure Backup eine Semantik bereitstellt, die derjenigen von Bandsicherungen ähnelt, damit Sie Ihre Daten in Azure sichern und wiederherstellen können.
ms.topic: conceptual
ms.date: 04/30/2017
ms.openlocfilehash: 695cc2644521384527ecd871f3613a078e987aa7
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "89378439"
---
# <a name="move-your-long-term-storage-from-tape-to-the-azure-cloud"></a>Verschieben langfristiger Speicher von Bändern in die Azure-Cloud

Kunden von Azure Backup und System Center Data Protection Manager haben folgende Möglichkeiten:

* Sichern von Daten gemäß Zeitplänen, die den Anforderungen ihrer Organisation am besten entsprechen
* Beibehalten der Sicherungsdaten für längere Zeiträume.
* Integrieren von Azure in ihre Anforderungen an die langfristige Aufbewahrung (anstelle von Bändern)

In diesem Artikel wird erläutert, wie Kunden Sicherungs- und Aufbewahrungsrichtlinien aktivieren können. Kunden, die für ihre Anforderungen an eine langfristige Aufbewahrung Bänder verwenden, steht jetzt dank dieses Features eine leistungsstarke und geeignete Alternative zur Verfügung. Das Feature ist in der neuesten Version von Azure Backup aktiviert (die [hier](https://aka.ms/azurebackup_agent)erhältlich ist). System Center DPM-Kunden müssen mindestens auf DPM 2012 R2 UR5 aktualisieren, bevor sie DPM mit dem Azure Backup-Dienst verwenden können.

## <a name="what-is-the-backup-schedule"></a>Was ist ein Sicherungszeitplan?

Der Sicherungszeitplan gibt die Häufigkeit des Sicherungsvorgangs an. Die Einstellungen auf dem folgenden Bildschirm geben beispielsweise an, dass Sicherungen täglich um 18:00 Uhr und um Mitternacht ausgeführt werden.

![Täglicher Zeitplan](./media/backup-azure-backup-cloud-as-tape/dailybackupschedule.png)

Kunden können auch eine wöchentliche Sicherung planen. Die Einstellungen auf dem folgenden Bildschirm geben beispielsweise an, dass Sicherungen jeden zweiten Sonntag und Mittwoch um 9:30 Uhr und um 1 Uhr morgens erstellt werden.

![Wöchentlicher Zeitplan](./media/backup-azure-backup-cloud-as-tape/weeklybackupschedule.png)

## <a name="what-is-the-retention-policy"></a>Worum handelt es sich bei der Aufbewahrungsrichtlinie?

Die Aufbewahrungsrichtlinie gibt an, wie lange die Sicherung gespeichert werden muss. Anstatt nur eine "flache Richtlinie" für alle Sicherungspunkte anzugeben, können Kunden je nach Sicherungszeitpunkt verschiedene Aufbewahrungsrichtlinien festlegen. Zum Beispiel kann der täglich erstellte Sicherungspunkt, der als Wiederherstellungspunkt für den alltäglichen Betrieb dient, 90 Tage lang aufbewahrt werden. Der Sicherungspunkt, der zu Überwachungszwecken am Ende jedes Quartals erstellt wird, kann wesentlich länger aufbewahrt werden.

![Aufbewahrungsrichtlinie](./media/backup-azure-backup-cloud-as-tape/retentionpolicy.png)

Die Gesamtanzahl der in dieser Richtlinie angegebenen "Aufbewahrungspunkte" ist 90 (tägliche Punkte) + 40 (einer pro Quartal für 10 Jahre) = 130.

## <a name="example--putting-both-together"></a>Beispiel – Kombination aus beidem

![Beispielbildschirm](./media/backup-azure-backup-cloud-as-tape/samplescreen.png)

1. **Tägliche Aufbewahrungsrichtlinie**: Täglich erstellte Sicherungen werden sieben Tage lang gespeichert.
2. **Wöchentliche Aufbewahrungsrichtlinie**: Sicherungen, die jeden Samstag um Mitternacht und 18 Uhr erstellt werden, werden vier Wochen lang aufbewahrt.
3. **Monatliche Aufbewahrungsrichtlinie**: Sicherungen, die am letzten Samstag im Monat um Mitternacht und 18 Uhr erstellt werden, werden zwölf Monate lang aufbewahrt.
4. **Jährliche Aufbewahrungsrichtlinie**: Sicherungen, die am letzten Samstag im März um Mitternacht erstellt werden, werden zehn Jahre lang aufbewahrt.

Die Gesamtanzahl der „Aufbewahrungspunkte“ (Punkte, von denen ein Kunde Daten wiederherstellen kann) in der obigen Abbildung wird wie folgt berechnet:

* Zwei Punkte pro Tag, sieben Tage lang = 14 Wiederherstellungspunkte
* Zwei Punkte pro Woche, vier Wochen lang = 8 Wiederherstellungspunkte
* Zwei Punkte pro Monat, 12 Monate lang = 24 Wiederherstellungspunkte
* Ein Punkt pro Jahr, 10 Jahre lang = 10 Wiederherstellungspunkte

Die Gesamtzahl der Wiederherstellungspunkte beträgt 56.

> [!NOTE]
> Pro geschützter Instanz können mit Azure Backup bis zu 9.999 Wiederherstellungspunkte erstellt werden. Geschützte Instanzen sind Computer, Server (physisch oder virtuell) oder Workloads, die in Azure gesichert werden.
>

## <a name="advanced-configuration"></a>Erweiterte Konfiguration

Durch Auswahl von **Ändern** im Bildschirm oben können Kunden noch flexiblere Aufbewahrungszeitpläne angeben.

![Fenstern zum Ändern der Richtlinie](./media/backup-azure-backup-cloud-as-tape/modify.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Backup finden Sie hier:

* [Einführung in Azure Backup](./backup-overview.md)
* [Azure Backup testen](./backup-windows-with-mars-agent.md)
