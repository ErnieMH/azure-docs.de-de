---
title: Sichern von SQL Server-Workloads auf Azure Stack
description: In diesem Artikel erfahren Sie, wie Sie Microsoft Azure Backup Server (MABS) zum Schutz von SQL Server-Datenbanken auf Azure Stack konfigurieren.
ms.topic: conceptual
ms.date: 06/08/2018
ms.openlocfilehash: 80de7913b010fca69c3703e423109f2ede653590
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91332813"
---
# <a name="back-up-sql-server-on-azure-stack"></a>Sichern von SQL Server auf Azure Stack

Verwenden Sie diesen Artikel, um Microsoft Azure Backup Server (MABS) zum Schutz von SQL Server-Datenbanken auf Azure Stack zu konfigurieren.

Die Verwaltung der Sicherung und Wiederherstellung von SQL-Datenbanken in und aus Azure umfasst drei Schritte:

1. Erstellen einer Sicherungsrichtlinie zum Schutz von SQL Server-Datenbanken
2. Bedarfsgesteuertes Erstellen von Sicherungskopien
3. Wiederherstellen der Datenbank vom Datenträger und aus Azure

## <a name="prerequisites-and-limitations"></a>Voraussetzungen und Einschränkungen

* Wenn Sie über eine Datenbank mit Dateien auf einer Remotedateifreigabe verfügen, werden die darauf enthaltenen Daten nicht geschützt und ein Fehler mit der ID 104 ausgegeben. Der Schutz von SQL Server-Daten auf einer Remotedateifreigabe wird von MABS nicht unterstützt.
* Datenbanken, die auf SMB-Remotefreigaben gespeichert sind, können von MABS nicht geschützt werden.
* Stellen Sie sicher, dass die [Replikate der Verfügbarkeitsgruppe als schreibgeschützt konfiguriert sind](/sql/database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server).
* Sie müssen das Systemkonto **NTAuthority\System** der Systemadministratorgruppe in SQL Server explizit hinzufügen.
* Wenn Sie für eine teilweise eigenständige Datenbank eine Wiederherstellung an einem anderen Speicherort durchführen, müssen Sie sicherstellen, dass für die SQL-Zielinstanz die Funktion für [eigenständige Datenbanken](/sql/relational-databases/databases/migrate-to-a-partially-contained-database#enable) aktiviert wurde.
* Wenn Sie für eine Filestream-Datenbank eine Wiederherstellung an einem anderen Speicherort durchführen, müssen Sie sicherstellen, dass für die SQL-Zielinstanz die Funktion für [Filestream-Datenbanken](/sql/relational-databases/blob/enable-and-configure-filestream) aktiviert wurde.
* Schutz für SQL Server AlwaysOn:
  * Verfügbarkeitsgruppen werden von MABS beim Ausführen von Abfragen während der Erstellung von Schutzgruppen erkannt.
  * Ein Failover wird von MABS erkannt, und die Datenbank wird weiterhin geschützt.
  * Mehrere Standorte umfassende Clusterkonfigurationen für eine Instanz von SQL Server werden von MABS unterstützt.
* Wenn Sie Datenbanken schützen, für die die Funktion „AlwaysOn“ verwendet wird, gelten für MABS folgende Einschränkungen:
  * Die Sicherungsrichtlinie für Verfügbarkeitsgruppen, die in SQL Server auf Basis der Sicherungseinstellungen festgelegt wird, wird von MABS wie folgt berücksichtigt:
    * Sekundär bevorzugen: Sicherungen müssen für ein sekundäres Replikat ausgeführt werden, es sei denn, das primäre Replikat ist als einziges Replikat online. Wenn mehrere sekundäre Replikate verfügbar sind, wird der Knoten mit der höchsten Sicherungspriorität für die Sicherung ausgewählt. Wenn nur das primäre Replikat verfügbar ist, muss die Sicherung für das primäre Replikat stattfinden.
    * Nur sekundäre: Die Sicherung darf nicht für das primäre Replikat ausgeführt werden. Wenn nur das primäre Replikat online ist, darf keine Sicherung ausgeführt werden.
    * Primär: Sicherungen müssen immer für das primäre Replikat ausgeführt werden.
    * Beliebiges Replikat: Sicherungen können für ein beliebiges Verfügbarkeitsreplikat in der Verfügbarkeitsgruppe ausgeführt werden. Der Knoten, von dem aus die Sicherung erfolgen soll, basiert auf den Sicherungsprioritäten für die einzelnen Knoten.
  * Beachten Sie Folgendes:
    * Sicherungen können für jedes lesbare Replikat erfolgen, d h. für ein primäres, ein synchrones sekundäres oder ein asynchrones sekundäres Replikat.
    * Wenn ein Replikat von der Sicherung ausgeschlossen ist, etwa weil **Replikat ausschließen** aktiviert oder das Replikat als nicht lesbar gekennzeichnet wurde, wird dieses Replikat unter keiner der Optionen für die Sicherung ausgewählt.
    * Wenn mehrere Replikate verfügbar und lesbar sind, wird der Knoten mit der höchsten Sicherungspriorität für die Sicherung ausgewählt.
    * Bei einem Sicherungsfehler auf dem ausgewählten Knoten ist der Sicherungsvorgang fehlerhaft.
    * Die Wiederherstellung am ursprünglichen Speicherort wird nicht unterstützt.
* Sicherungsprobleme bei SQL Server 2014 oder höher:
  * SQL Server 2014 wurde durch eine neue Funktion zum Erstellen einer [Datenbank für lokale SQL Server-Instanzen in Windows Azure Blob Storage](/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure) erweitert. Diese Konfiguration kann nicht mithilfe von MABS geschützt werden.
  * Die Sicherungseinstellung „Sekundär bevorzugen“ verursacht bei Verwendung der Option „SQL AlwaysOn“ einige bekannte Probleme. Von MABS wird immer eine Sicherung für das sekundäre Replikat ausgeführt. Wenn kein sekundäres Replikat gefunden wird, tritt bei der Sicherung ein Fehler auf.

## <a name="before-you-start"></a>Vorbereitung

[Installieren und Vorbereiten von Azure Backup Server](backup-mabs-install-azure-stack.md).

## <a name="create-a-backup-policy-to-protect-sql-server-databases-to-azure"></a>Erstellen einer Sicherungsrichtlinie zum Schutz von SQL Server-Datenbanken mithilfe von Azure

1. Wählen Sie auf der Azure Backup Server-Benutzeroberfläche den Arbeitsbereich **Schutz** aus.

2. Wählen Sie im Menüband **Neu** aus, um eine neue Schutzgruppe zu erstellen.

    ![Erstellen einer Schutzgruppe](./media/backup-azure-backup-sql/protection-group.png)

    Azure Backup Server startet den Schutzgruppen-Assistenten, der Sie schrittweise durch die Erstellung einer **Schutzgruppe** führt. Wählen Sie **Weiter** aus.

3. Wählen Sie unter **Schutzgruppentyp auswählen** die Option **Server** aus.

    ![Auswählen des Schutzgruppentyp – "Server"](./media/backup-azure-backup-sql/pg-servers.png)

4. Auf dem Bildschirm **Gruppenmitglieder auswählen** zeigt die Liste „Verfügbare Mitglieder“ die verschiedenen Datenquellen an. Wählen Sie **+** aus, um einen Ordner zu erweitern und die Unterordner anzuzeigen. Aktivieren Sie das Kontrollkästchen, um ein Element auszuwählen.

    ![Auswählen einer SQL-Datenbank](./media/backup-azure-backup-sql/pg-databases.png)

    Alle ausgewählten Elemente werden in der Liste „Ausgewählte Elemente“ angezeigt. Nach dem Auswählen der Server oder Datenbanken, die Sie schützen möchten, wählen Sie **Weiter** aus.

5. Geben Sie auf dem Bildschirm **Datenschutzmethode auswählen** einen Namen für die Schutzgruppe an, und aktivieren Sie das Kontrollkästchen **Ich möchte Onlineschutz**.

    ![Datenschutzmethode: kurzfristig auf Datenträger und online in Azure](./media/backup-azure-backup-sql/pg-name.png)

6. Geben Sie auf dem Bildschirm **Kurzfristige Ziele angeben** alle erforderlichen Informationen ein, um Sicherungspunkte auf dem Datenträger zu erstellen, und wählen Sie dann **Weiter** aus.

    Im Beispiel beträgt die **Beibehaltungsdauer** **5 Tage**, und die **Synchronisierungsfrequenz** ist ein Mal alle **15 Minuten**. Dies ist die Sicherungshäufigkeit. Der Wert für **Schnelle vollständige Sicherung** ist auf **20:00** festgelegt.

    ![Kurzfristige Ziele](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > Im gezeigten Beispiel wird jeden Tag um 20:00 Uhr ein Sicherungspunkt erstellt, indem die geänderten Daten vom Sicherungspunkt des Vortags um 20:00 Uhr übertragen werden. Dieser Vorgang wird als **Schnelle vollständige Sicherung** bezeichnet. Transaktionsprotokolle werden alle 15 Minuten synchronisiert. Wenn Sie die Datenbank um 21:00 Uhr wiederherstellen müssen, wird der Punkt aus den Protokollen aus dem letzten Punkt der schnellen vollständigen Sicherung erstellt (in diesem Fall 20:00 Uhr).
   >
   >

7. Überprüfen Sie auf dem Bildschirm **Datenträgerzuordnung überprüfen** den gesamten verfügbaren Speicherplatz und den möglichen Speicherplatz. Wählen Sie **Weiter** aus.

8. Wählen Sie unter **Replikaterstellungsmethode auswählen** aus, wie der erste Wiederherstellungspunkt erstellt werden soll. Sie können diese anfängliche Sicherung manuell (nicht über das Netzwerk) übertragen, um nicht zu viel Bandbreite zu belegen, oder die Daten über das Netzwerk senden. Wenn Sie mit der Übertragung der ersten Sicherung warten möchten, können Sie den Zeitpunkt der ersten Übertragung angeben. Wählen Sie **Weiter** aus.

    ![Methode für die anfängliche Replikation](./media/backup-azure-backup-sql/pg-manual.png)

    Die anfängliche Sicherungskopie erfordert eine Übertragung der gesamten Datenquelle (SQL Server-Datenbank) vom Produktionsserver (SQL Server-Computer) zu Azure Backup Server. Der Umfang dieser Daten kann sehr groß sein, und die Übertragung der Daten über das Netzwerk überschreitet möglicherweise die Bandbreite. Aus diesem Grund stehen Ihnen zwei Optionen für die Übertragung der anfänglichen Sicherung zur Verfügung: **Manuell** (mithilfe von Wechselmedien), um eine Überlastung der Bandbreite zu vermeiden, oder **Automatisch über das Netzwerk** (zu einem bestimmten Zeitpunkt).

    Sobald die anfängliche Sicherung abgeschlossen ist, werden nur noch inkrementelle Sicherungen basierend auf der anfänglichen Sicherungskopie erstellt. Inkrementelle Sicherungen sind im Allgemeinen klein und lassen sich problemlos über das Netzwerk übertragen.

9. Wählen Sie aus, wann die Konsistenzprüfung ausgeführt werden soll, und wählen Sie **Weiter** aus.

    ![Konsistenzprüfung](./media/backup-azure-backup-sql/pg-consistent.png)

    Azure Backup Server kann eine Konsistenzprüfung ausführen, um die Integrität des Sicherungspunkts zu prüfen. Hierbei wird die Prüfsumme der Sicherungsdatei auf dem Produktionsserver (in diesem Szenario der SQL Server-Computer) und der gesicherten Daten für diese Datei durch Azure Backup Server berechnet. Wenn ein Konflikt auftritt, wird angenommen, dass die gesicherte Datei auf Azure Backup Server beschädigt ist. Azure Backup Server korrigiert die gesicherten Daten, indem die Datenblöcke gesendet werden, die nicht der Prüfsumme entsprechen. Da Konsistenzprüfungen leistungsintensiv sind, können Sie die Konsistenzprüfung planen oder automatisch ausführen.

10. Um Onlineschutz für die Datenquellen festzulegen, wählen Sie die Datenbanken aus, die Sie mit Azure schützen möchten. Wählen Sie dann **Weiter** aus.

    ![Auswählen der Datenquellen](./media/backup-azure-backup-sql/pg-sqldatabases.png)

11. Wählen Sie Sicherungszeitpläne und Aufbewahrungsrichtlinien aus, die den Richtlinien ihrer Organisation entsprechen.

    ![Planung und Aufbewahrung](./media/backup-azure-backup-sql/pg-schedule.png)

    In diesem Beispiel werden Sicherungen einmal täglich um 12:00 Uhr und um 20:00 Uhr ausgeführt (unterer Bildschirmbereich).

    > [!NOTE]
    > Es ist eine bewährte Methode, einige kurzfristige Wiederherstellungspunkte auf einem Datenträger zur Hand zu haben, um eine schnelle Wiederherstellung zu ermöglichen. Diese Wiederherstellungspunkte werden für die Wiederherstellung operativer Funktionen verwendet. Azure ist dank hoher SLAs und garantierter Verfügbarkeit eine gute Wahl als Offsitestandort.
    >
    >

    **Bewährte Methode**: Wenn Sie planen, Sicherungen in Azure zu starten, nachdem die lokalen Datenträgersicherungen abgeschlossen wurden, werden die neuesten Datenträgersicherungen immer in Azure kopiert.

12. Wählen Sie den Zeitplan für die Aufbewahrungsrichtlinie. Ausführliche Informationen zur Funktionsweise der Aufbewahrungsrichtlinie finden Sie im Artikel [Verwenden von Azure Backup als Ersatz für Ihre Bandinfrastruktur](backup-azure-backup-cloud-as-tape.md).

    ![Aufbewahrungsrichtlinie](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    In diesem Beispiel:

    * Sicherungen einmal täglich um 12:00 Uhr und um 20:00 Uhr ausgeführt (unterer Bildschirmbereich) und für 180 Tage beibehalten.
    * Die Sicherung am Samstag um 12:00 Uhr wird 104 Wochen lang beibehalten.
    * Die Sicherung am letzten Samstag um 12:00 Uhr wird 60 Monate lang beibehalten.
    * Die Sicherung am letzten Samstag im März um 12:00 Uhr wird 10 Jahre lang beibehalten.
13. Wählen Sie **Weiter** und dann die geeignete Option zum Übertragen der anfänglichen Sicherung nach Azure aus. Sie können **Automatisch über das Netzwerk** wählen.

14. Nachdem Sie die Richtliniendetails auf dem Bildschirm **Zusammenfassung** überprüft haben, wählen Sie **Gruppe erstellen** aus, um den Workflow abzuschließen. Sie können anschließend **Schließen** auswählen und den Auftragsfortschritt im Arbeitsbereich „Überwachung“ verfolgen.

    ![Erstellen der Schutzgruppe – in Bearbeitung](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a>Bedarfsgesteuerte Sicherung einer SQL Server-Datenbank

Anhand der zuvor beschriebenen Schritte wurde eine Sicherungsrichtlinie eingerichtet, ein "Wiederherstellungspunkt" wird jedoch erst bei Ausführung der ersten Sicherung erstellt. Statt darauf zu warten, dass der Scheduler in Aktion tritt, können Sie mithilfe der nachstehenden Schritte manuell einen Wiederherstellungspunkt erstellen.

1. Warten Sie, bis der Status der Datenbank für die Schutzgruppe als **OK** angezeigt wird, bevor Sie den Wiederherstellungspunkt erstellen.

    ![Schutzgruppenmitglieder](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. Klicken Sie mit der rechten Maustaste auf die Datenbank, und wählen Sie **Wiederherstellungspunkt erstellen**.

    ![Erstellen eines Onlinewiederherstellungspunkts](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. Wählen Sie **Onlineschutz** im Dropdownmenü und dann **OK** aus, um die Erstellung eines Wiederherstellungspunkts in Azure zu starten.

    ![Wiederherstellungspunkt erstellen](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. Zeigen Sie den Status des Auftrags im Arbeitsbereich **Überwachung** an.

    ![Konsole für die Überwachung](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>Wiederherstellen einer SQL Server-Datenbank aus Azure

Die folgenden Schritte sind erforderlich, um eine geschützte Entität (SQL Server-Datenbank) aus Azure wiederherzustellen.

1. Öffnen Sie die Azure Backup Server-Verwaltungskonsole. Navigieren Sie zum Arbeitsbereich **Wiederherstellung**, in dem die geschützten Server angezeigt werden. Suchen Sie nach der erforderlichen Datenbank (in diesem Fall "ReportServer$MSDPM2012"). Wählen Sie eine Zeit für **Wiederherstellung von** aus, die als ein **Online**punkt angegeben ist.

    ![Auswählen eines Wiederherstellungspunkts](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. Klicken Sie mit der rechten Maustaste auf den Datenbanknamen, und wählen Sie **Wiederherstellen** aus.

    ![Wiederherstellen aus Azure](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. MABS zeigt die Details zum Wiederherstellungspunkt an. Wählen Sie **Weiter** aus. Um die Datenbank zu überschreiben, wählen Sie den Wiederherstellungstyp **In ursprünglicher Instanz von SQL Server wiederherstellen**aus. Wählen Sie **Weiter** aus.

    ![Wiederherstellung am ursprünglichen Speicherort](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    In diesem Beispiel stellt MABS die Datenbank in einer anderen SQL Server-Instanz oder in einem eigenständigen Netzwerkordner wieder her.

4. Auf dem Bildschirm **Wiederherstellungsoptionen angeben** können Sie Optionen auswählen, z.B. „Netzwerk-Bandbreiteneinschränkung“, um die Bandbreitenbelegung bei der Wiederherstellung zu drosseln. Wählen Sie **Weiter** aus.

5. Im Bildschirm **Zusammenfassung** werden alle bisher konfigurierten Wiederherstellungsoptionen angezeigt. Wählen Sie **Wiederherstellen** aus.

    Der Wiederherstellungsstatus zeigt an, dass die Datenbank wiederhergestellt wird. Sie können **Schließen** auswählen, um den Assistenten zu schließen und den Fortschritt im Arbeitsbereich **Überwachung** zu verfolgen.

    ![Auslösen der Wiederherstellung](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    Nach Abschluss der Wiederherstellung ist die wiederhergestellte Datenbank anwendungskonsistent.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie im Artikel [Sichern von Dateien und Anwendungen](backup-mabs-files-applications-azure-stack.md).
Weitere Informationen finden Sie im Artikel [Sichern von SharePoint auf Azure Stack](backup-mabs-sharepoint-azure-stack.md).
