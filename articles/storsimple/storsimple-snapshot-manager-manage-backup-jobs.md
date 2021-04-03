---
title: StorSimple Snapshot Manager-Sicherungsaufträge | Microsoft Docs
description: Beschreibt, wie das MMC-Snap-in von StorSimple Snapshot Manager zum Anzeigen und Verwalten von geplanten, aktuell ausgeführten und abgeschlossenen Sicherungsaufträge verwendet wird.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: bf4dcff6-c819-4766-b9d9-9922831cb200
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: 3c26a84e32a17cba83b5ca895f146e561072fa62
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "95998191"
---
# <a name="use-storsimple-snapshot-manager-to-view-and-manage-backup-jobs"></a>Verwenden von StorSimple Snapshot Manager zum Anzeigen und Verwalten von Sicherungsaufträgen

## <a name="overview"></a>Übersicht
Der Knoten **Aufträge** im Fensterbereich **Bereich** zeigt Sicherungsaufträge in den Phasen **Geplant**, **Letzte 24 Stunden** und **Wird ausgeführt**, die Sie interaktiv oder durch eine konfigurierte Richtlinie initiiert haben. 

In diesem Tutorial wird erklärt, wie Sie den Knoten **Aufträge** verwenden können, um Informationen über geplante, aktuelle und aktuell ausgeführte Sicherungsaufträge anzuzeigen. (Die Liste der Aufträge und die entsprechenden Informationen werden im Bereich **Ergebnisse** angezeigt.) Darüber hinaus können Sie mit der rechten Maustaste auf einen aufgelisteten Auftrag klicken, sodass ein Kontextmenü mit verfügbaren Aktionen erscheint.

## <a name="view-scheduled-jobs"></a>Anzeigen geplanter Aufträge
Verwenden Sie das folgende Verfahren, um geplante Sicherungsaufträge anzuzeigen.

#### <a name="to-view-scheduled-jobs"></a>So zeigen Sie geplante Aufträge an:
1. Klicken Sie auf das Desktopsymbol, um den StorSimple Snapshot Manager zu starten. 
2. Erweitern Sie im Fenster **Bereich** den Knoten **Aufträge**, und klicken Sie auf **Geplant**. Die folgenden Informationen werden im **Ergebnisbereich** angezeigt:
   
   * **Name**: Der Name der geplanten Momentaufnahme.
   * **Nächste Ausführung**: Datum und Uhrzeit der nächsten geplanten Momentaufnahme.
   * **Letzte Ausführung**: Datum und Uhrzeit der zuletzt geplanten Momentaufnahme.
     
     > [!NOTE]
     > Bei einmaligen Momentaufnahmen sind die Angaben unter **Nächste Ausführung** und **Letzte Ausführung** identisch.
     
     ![Geplante Sicherungsaufträge](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
3. Um zusätzliche Aktionen für einen bestimmten Auftrag durchzuführen, klicken Sie mit der rechten Maustaste auf den Namen des Auftrags im Bereich **Ergebnisse** , und wählen Sie aus den Menüoptionen.

## <a name="view-recent-jobs"></a>Anzeigen der zuletzt verwendeten Aufträge
Verwenden Sie das folgende Verfahren zum Anzeigen von Sicherungs- und Wiederherstellungsaufträgen, die in den letzten 24 Stunden abgeschlossen wurden.

#### <a name="to-view-recent-jobs"></a>So zeigen Sie kürzlich ausgeführte Aufträge an:
1. Klicken Sie auf das Desktopsymbol, um den StorSimple Snapshot Manager zu starten.
2. Erweitern Sie im Fenster **Bereich** den Knoten **Aufträge**, und klicken Sie auf **Letzte 24 Stunden**. Das Ergebnisfenster zeigt Sicherungsaufgaben für die letzten 24 Stunden (bis maximal 64 Aufträge) an. Im Ergebnisbereich werden, abhängig von den unter **Ansicht** angegebenen Optionen, die folgenden Informationen angezeigt:
   
   * **Name**: Der Name der geplanten Momentaufnahme.
   * **Gestartet**: Startdatum und -zeit der Momentaufnahme.
   * **Beendet**: Datum und Uhrzeit, zu dem bzw. der die Momentaufnahme fertig gestellt oder beendet wurde.
   * **Verstrichen**: Die verstrichene Zeitspanne zischen den Zeiten unter **Gestartet** und **Beendet**.
   * **Status**: Der Status des zuletzt abgeschlossenen Auftrags. **Erfolg** gibt an, dass die Sicherung erfolgreich erstellt wurde. **Fehler** gibt an, dass der Auftrag nicht erfolgreich ausgeführt wurde.
   * **Informationen**: Die Ursache für den Fehler.
   * **Verarbeitete Bytes (MB)**: Die Datenmenge der Volumegruppe, die bereits verarbeitet wurde (in MB). 
     
     ![Aufträge, die in den letzten 24 Stunden ausgeführt wurden](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 
3. Um zusätzliche Aktionen für einen bestimmten Auftrag durchzuführen, klicken Sie mit der rechten Maustaste auf den Namen des Auftrags im Bereich **Ergebnisse** , und wählen Sie aus den Menüoptionen.
   
    ![Löschen eines Auftrags](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png)

## <a name="view-currently-running-jobs"></a>Anzeigen aktuell ausgeführter Aufträge
Verwenden Sie das folgende Verfahren zum Anzeigen von Aufträgen, die derzeit ausgeführt werden.

#### <a name="to-view-currently-running-jobs"></a>So zeigen Sie aktuell ausgeführte Aufträge an:
1. Klicken Sie auf das Desktopsymbol, um den StorSimple Snapshot Manager zu starten.
2. Erweitern Sie im Fenster **Bereich** den Knoten **Aufträge**, und klicken Sie auf **Wird ausgeführt**. Abhängig von den unter **Ansicht** angegebenen Optionen werden im Bereich **Ergebnisse** die folgenden Informationen angezeigt:
   
   * **Name**: Der Name der geplanten Momentaufnahme.
   * **Gestartet**: Startdatum und -zeit der Momentaufnahme.
   * **Prüfpunkt** – Die aktuelle Aktion der Sicherung.
   * **Status** – Der Vollendungsgrad in Prozent.
   * **Verstrichen**: Die seit dem Sicherungsstart verstrichene Zeit. 
   * **Durchschnittlicher Durchsatz (MB)** – Verhältnis der gesamten verarbeiteten Datenbytes zur gesamten Verarbeitungszeit (in MB).
   * **Verarbeitete Bytes (MB)** – gesamte verarbeitete Datenbytes (in MB).
   * **Geschriebene Bytes (MB)** – gesamte geschriebene Datenbytes (in MB). Dieser Wert umfasst die Daten sowie die Metadaten und ist daher normalerweise größer als der Wert von „Verarbeitete Bytes (MB)“.
     
     ![Aufträge, die derzeit ausgeführt werden](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)
3. Um zusätzliche Aktionen für einen bestimmten Auftrag durchzuführen, klicken Sie mit der rechten Maustaste auf den Namen des Auftrags im Bereich **Ergebnisse** , und wählen Sie aus den Menüoptionen.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über das [Verwenden von StorSimple Snapshot Manager zum Verwalten der StorSimple-Lösung](storsimple-snapshot-manager-admin.md).
* Informationen zum [Verwenden von StorSimple Snapshot Manager zum Verwalten des Sicherungkatalogs](storsimple-snapshot-manager-manage-backup-catalog.md).

