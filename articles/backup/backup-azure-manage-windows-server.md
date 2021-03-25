---
title: Verwalten von Azure Recovery Services-Tresoren und -Servern
description: In diesem Artikel erfahren Sie, wie Sie das Dashboard „Übersicht“ des Recovery Services-Tresors zum Überwachen und Verwalten Ihrer Recovery Services-Tresore verwenden.
ms.topic: conceptual
ms.date: 07/08/2019
ms.openlocfilehash: 74351d781287d863db8be0fc7d20517e0479106c
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "89002129"
---
# <a name="monitor-and-manage-recovery-services-vaults"></a>Überwachen und Verwalten von Recovery Services-Tresoren

In diesem Artikel wird erläutert, wie Sie das Dashboard **Übersicht** des Recovery Services-Tresors zum Überwachen und Verwalten Ihrer Recovery Services-Tresore verwenden. Wenn Sie einen Recovery Services-Tresor aus der Liste öffnen, wird das Dashboard **Übersicht** für den ausgewählten Tresor geöffnet. Das Dashboard stellt verschiedene Details zum Tresor bereit. Es gibt *Kacheln*, auf denen Folgendes angezeigt wird: der Status von kritischen Warnungen und allgemeinen Warnmeldungen, in Bearbeitung befindliche und fehlerhafte Sicherungsaufträge sowie die Menge von belegtem lokal redundantem Speicher (LRS) und georedundantem Speicher (GRS). Wenn Sie Azure-VMs in den Tresor sichern, werden auf der Kachel [**Status der Sicherungsvorüberprüfung** alle kritischen oder fehlerhaften Elemente angezeigt](#backup-pre-check-status). In der folgenden Abbildung wird das Dashboard **Übersicht** für **Contoso-vault** angezeigt. Auf der Kachel **Sicherungselemente** wird angezeigt, dass es neun beim Tresor registrierte Elemente gibt.

![Dashboard des Recovery Services-Tresors](./media/backup-azure-manage-windows-server/rs-vault-blade.png)

Die Voraussetzungen für diesen Artikel sind: ein Azure-Abonnement, ein Recovery Services-Tresor und mindestens ein für den Tresor konfiguriertes Sicherungselement.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="open-a-recovery-services-vault"></a>Öffnen eines Recovery Services-Tresors

Öffnen Sie den Tresor, um Warnungen zu überwachen oder Verwaltungsdaten zu einem Recovery Services-Tresor anzuzeigen.

1. Melden Sie sich unter Verwendung Ihres Azure-Abonnements beim [Azure-Portal](https://portal.azure.com/) an.

2. Klicken Sie im Portal auf **Alle Dienste**.

   ![Öffnen der Liste mit den Recovery Services-Tresoren – Schritt 1](./media/backup-azure-manage-windows-server/open-rs-vault-list.png)

3. Geben Sie im Dialogfeld **Alle Dienste** **Recovery Services** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wenn die Option **Recovery Services-Tresore** angezeigt wird, wählen Sie sie aus, um die Liste mit den Recovery Services-Tresoren in Ihrem Abonnement anzuzeigen.

    ![Erstellen eines Recovery Services-Tresors – Schritt 1](./media/backup-azure-manage-windows-server/list-of-rs-vaults.png) <br/>

4. Wählen Sie in der Liste mit den Tresoren einen Tresor aus, um dessen Dashboard **Übersicht** zu öffnen.

    ![Dashboard des Recovery Services-Tresors](./media/backup-azure-manage-windows-server/rs-vault-blade.png) <br/>

    Das Dashboard „Übersicht“ stellt Warnungen und Sicherungsauftragsdaten auf Kacheln bereit.

## <a name="monitor-backup-jobs-and-alerts"></a>Überwachen von Sicherungsaufträgen und Warnungen

Das Dashboard **Übersicht** des Recovery Services-Tresors enthält Kacheln für Überwachungs- und Nutzungsinformationen. Auf den Kacheln im Abschnitt „Überwachung“ werden kritische Warnungen und allgemeine Warnmeldungen sowie in Bearbeitung befindliche und fehlerhaften Aufträge angezeigt. Wählen Sie eine bestimmte Warnung oder einen Auftrag aus, um das nach dieser Warnung bzw. diesem Auftrag gefilterte Menü „Sicherungswarnungen“ oder „Sicherungsaufträge“ zu öffnen.

![Aufgaben im Sicherungsdashboard](./media/backup-azure-manage-windows-server/monitor-dashboard-tiles-warning.png)

Im Abschnitt „Überwachung“ werden die Ergebnisse vordefinierter Abfragen für **Sicherungswarnungen** und **Sicherungsaufträge** angezeigt. Die Kacheln des Abschnitts „Überwachung“ enthalten aktuelle Informationen über:

* Kritische Warnungen und allgemeine Warnmeldungen für Sicherungsaufträge (in den letzten 24 Stunden)
* Status der Vorüberprüfung für virtuelle Azure-Computer. Ausführliche Informationen zum Status der Vorüberprüfung finden Sie unter [Status der Sicherungsvorüberprüfung](#backup-pre-check-status).
* Die Sicherungsaufträge, die gerade ausgeführt werden, und Aufträge, die fehlgeschlagen sind (in den letzten 24 Stunden).

Die Kacheln des Abschnitts „Nutzung“ enthalten folgende Informationen:

* Die Anzahl der für den Tresor konfigurierten Sicherungselemente.
* Der vom Tresor belegte Azure-Speicher (aufgeteilt nach LRS und GRS).

Wählen Sie die Kacheln (außer „Sicherungsspeicher“) aus, um das zugehörige Menü zu öffnen. In der Abbildung oben werden auf der Kachel „Sicherungswarnungen“ drei kritische Warnungen angezeigt. Durch Auswählen der Zeile „Kritische Warnungen“ auf der Kachel „Sicherungswarnungen“ werden die nach kritischen Warnungen gefilterten Sicherungswarnungen angezeigt.

![Menü „Sicherungswarnungen“, gefiltert nach kritischen Warnungen](./media/backup-azure-manage-windows-server/critical-backup-alerts.png)

Das Menü „Sicherungswarnungen“ im Bild oben ist gefiltert nach: Status ist „Aktiv“, Schweregrad ist „Kritisch, und die Zeit sind die letzten 24 Stunden.

### <a name="backup-pre-check-status"></a>Status der Sicherungsvorüberprüfung

Sicherungsvorüberprüfungen überprüfen die Konfiguration Ihrer virtuellen Computer auf Probleme, die Sicherungen beeinträchtigen können. Sie aggregieren diese Informationen, damit Sie sie direkt auf dem Dashboard des Recovery Services-Tresors anzeigen können. Außerdem stellen sie Empfehlungen für Korrekturmaßnahmen bereit, um erfolgreiche datei- oder anwendungskonsistente Sicherungen zu gewährleisten. Für die Vorüberprüfungen ist keine Infrastruktur erforderlich, und es fallen keine zusätzlichen Kosten an.  

Sicherungsvorüberprüfungen werden im Rahmen der geplanten Sicherungsvorgänge für Ihre virtuellen Azure-Computer ausgeführt. Sie werden mit einer der folgenden Statusoptionen abgeschlossen:

* **Erfolgreich**: Dieser Status gibt an, dass die Konfiguration Ihres virtuellen Computers zu erfolgreichen Sicherungen führen sollte und keine Korrekturmaßnahmen erforderlich sind.
* **Warnung:** Dieser Status gibt an, dass mindestens ein Problem mit der Konfiguration des virtuellen Computers vorliegt, das *möglicherweise* zu Sicherungsfehlern führt. Es werden *empfohlene* Schritte angegeben, um erfolgreiche Sicherungen zu gewährleisten. So kann es beispielsweise zu zeitweiligen Sicherungsfehlern kommen, wenn nicht der aktuelle VM-Agent installiert ist. In diesem Fall wird ein Warnungsstatus ausgegeben.
* **Kritisch**: Dieser Status gibt an, dass mindestens ein Problem mit der Konfiguration des virtuellen Computers vorliegt, das *sicher* zu Sicherungsfehlern führt, und es werden *erforderliche* Schritte angegeben, um erfolgreiche Sicherungen zu gewährleisten. So führt beispielsweise ein Netzwerkproblem, das durch eine Aktualisierung der NSG-Regeln eines virtuellen Computers verursacht wird, dazu, dass Sicherungen nicht erfolgreich sind, da der virtuelle Computer nicht mit dem Azure Backup-Dienst kommunizieren kann. In diesem Fall wird ein kritischer Status ausgegeben.

Führen Sie die folgenden Schritte zum Beheben aller Probleme aus, die von den Sicherungsvorüberprüfungen für VM-Sicherungen in Ihrem Recovery Services-Tresor gemeldet werden.

* Wählen Sie im Dashboard des Recovery Services-Tresors die Kachel **Status der Sicherungsvorüberprüfung (Azure-VMs)** aus.
* Wählen Sie einen beliebigen virtuellen Computer mit dem Sicherungsvorüberprüfungsstatus **Kritisch** oder **Warnung** aus. Daraufhin wird der Bereich **VM-Details** geöffnet.
* Wählen Sie im oberen Bereich des Bereichs die Bereichsbenachrichtigung aus, um die Beschreibung des Konfigurationsproblems und die Abhilfeschritte anzuzeigen.

## <a name="manage-backup-alerts"></a>Verwalten von Sicherungswarnungen

Wählen Sie im Menü „Recovery Services-Tresor“ die Option **Sicherungswarnungen** aus, um das Menü „Sicherungswarnungen“ zu öffnen.

![Sicherungswarnungen](./media/backup-azure-manage-windows-server/backup-alerts-menu.png)

Im Bericht „Sicherungswarnungen“ sind die Warnungen für den Tresor aufgelistet.

![Bericht „Sicherungswarnungen“](./media/backup-azure-manage-windows-server/backup-alerts.png)

### <a name="alerts"></a>Alerts

Die Liste „Sicherungswarnungen“ enthält die ausgewählten Informationen für die gefilterten Warnungen. Im Menü „Sicherungswarnungen“ können Sie nach kritischen Warnungen oder allgemeinen Warnmeldungen filtern.

| Warnstufe | Ereignisse, die Warnungen generieren |
| ----------- | ----------- |
| Kritisch | Sie erhalten kritische Warnungen, wenn: Sicherungsaufträge fehlschlagen, wenn Wiederherstellungsaufträge fehlschlagen und wenn Sie den Schutz auf einem Server beenden, aber die Daten beibehalten.|
| Warnung | Allgemeine Warnmeldungen werden angezeigt, wenn: Sicherungsaufträge mit Warnungen abgeschlossen werden. (Beispielsweise wenn weniger als 100 Dateien aufgrund von Beschädigungen nicht gesichert wurden oder wenn mehr als 1.000.000 Dateien erfolgreich gesichert wurden). |
| Informational | Derzeit werden keine Informationsmeldungen verwendet. |

### <a name="viewing-alert-details"></a>Anzeigen von Warnungsdetails

Der Bericht „Sicherungswarnungen“ verfolgt acht Details zu jeder Warnung. Verwenden Sie die Schaltfläche **Spalten auswählen**, um die Details im Bericht zu bearbeiten.

![Schaltfläche „Spalten auswählen“ für Sicherungswarnungen](./media/backup-azure-manage-windows-server/backup-alerts.png)

Standardmäßig werden alle Details, im Bericht angezeigt, mit Ausnahme von **Zeitpunkt des letzten Auftretens**.

* Warnung
* Sicherungselement
* Geschützter Server
* severity
* Duration
* Erstellungszeit
* Status
* Zeitpunkt des letzten Auftretens

### <a name="change-the-details-in-alerts-report"></a>Ändern der Details im Bericht „Warnungen“

1. Zum Ändern der Berichtsinformationen wählen Sie im Menü **Sicherungswarnungen** die Option **Spalten auswählen** aus.

   ![„Spalten auswählen“ auswählen](./media/backup-azure-manage-windows-server/alerts-menu-choose-columns.png)

   Das Menü **Spalten auswählen** wird geöffnet.

2. Wählen Sie im Menü **Spalten auswählen** die Details aus, die im Bericht angezeigt werden sollen.

    ![Menü „Spalten auswählen“](./media/backup-azure-manage-windows-server/choose-columns-menu.png)

3. Wählen Sie **Fertig** aus, um die Änderungen zu speichern und das Menü „Spalten auswählen“ zu schließen.

   Wenn Sie Änderungen vornehmen, die Änderungen aber nicht beibehalten möchten, wählen Sie **Zurücksetzen** aus, um zur letzten gespeicherten Konfiguration zurückzukehren.

### <a name="change-the-filter-in-alerts-report"></a>Ändern des Filters im Bericht „Warnungen“

Verwenden Sie das Menü **Filter**, um Schweregrad, Status, Startzeit und Endzeit für die Warnungen zu ändern.

> [!NOTE]
> Durch Bearbeiten des Filters für die Sicherungswarnungen werden die kritischen Warnungen oder allgemeinen Warnmeldungen im Dashboard „Übersicht“ des Tresors nicht geändert.
>  

1. Zum Ändern des Filters für die Sicherungswarnungen wählen Sie im Menü „Sicherungswarnungen“ die Option **Filter** aus.

   ![Menü „Filter“ auswählen](./media/backup-azure-manage-windows-server/alerts-menu-choose-filter.png)

   Das Menü „Filter“ wird angezeigt.

   ![Menü „Filter“ für Warnungen](./media/backup-azure-manage-windows-server/filter-alert-menu.png)

2. Bearbeiten Sie Schweregrad, Status, Startzeit oder Endzeit, und wählen Sie zum Speichern der Änderungen die Option **Fertig** aus.

## <a name="configuring-notifications-for-alerts"></a>Konfigurieren von Benachrichtigungen für Warnungen

Konfigurieren Sie Benachrichtigungen, um E-Mails zu generieren, wenn eine Warnmeldung oder kritische Warnung auftritt. Sie können E-Mail-Benachrichtigungen jede Stunde oder direkt bei Eintreten einer bestimmten Warnung senden.

   ![Warnungen filtern](./media/backup-azure-manage-windows-server/configure-notification.png)

E-Mail-Benachrichtigungen sind standardmäßig **aktiviert**. Wählen Sie **Aus** aus, damit keine E-Mail-Benachrichtigungen mehr gesendet werden.

Wählen Sie im Steuerelement **Benachrichtigen** die Option **Pro Warnung** aus, wenn keine Gruppierung erfolgen soll, oder wenn Sie nur über wenige Elemente verfügen, die Warnungen generieren können. Jede Warnung führt zu einer Benachrichtigung (Standardeinstellung), und es wird sofort eine Lösungs-E-Mail gesendet.

Bei Auswahl von **Stündliche Übersicht** wird eine E-Mail an die Empfänger gesendet, die die in der letzten Stunde generierten, nicht aufgelösten Warnungen erläutert. Nach einer Stunde wird jeweils eine Lösungs-E-Mail gesendet.

Wählen Sie den Schweregrad der Warnung aus (kritische Warnung oder allgemeine Warnmeldung), bei der eine E-Mail-Adresse generiert werden soll. Derzeit werden keine Informationsmeldungen verwendet.

## <a name="manage-backup-items"></a>Verwalten von Sicherungselementen

In einem Recovery Services-Tresor werden viele Arten von Sicherungsdaten gespeichert. [Erfahren Sie mehr](backup-overview.md#what-can-i-back-up) über die Elemente, die Sie sichern können. Um die verschiedenen Server, Computer, Datenbanken und Workloads zu verwalten, wählen Sie die Kachel **Sicherungselemente** aus, um den Inhalt des Tresors anzuzeigen.

![Kachel „Sicherungselemente“](./media/backup-azure-manage-windows-server/backup-items.png)

Die Liste der Sicherungselemente wird geöffnet, organisiert nach Sicherungsverwaltungstyp.

![Liste der Sicherungselemente](./media/backup-azure-manage-windows-server/list-backup-items.png)

Um einen bestimmten Typ einer geschützten Instanz zu untersuchen, wählen Sie das Element in der Spalte „Sicherungsverwaltungstyp“ aus. In der Abbildung oben sind beispielsweise zwei virtuelle Azure-Computer dargestellt, die in diesem Tresor geschützt werden. Indem Sie **Virtueller Azure-Computer** auswählen, wird die Liste der geschützten virtuellen Computer in diesem Tresor geöffnet.

![Liste der geschützten virtuellen Computer](./media/backup-azure-manage-windows-server/list-of-protected-virtual-machines.png)

Die Liste der virtuellen Computer enthält nützliche Daten: die zugeordnete Ressourcengruppe, die vorherige [Sicherungsvorüberprüfung](#backup-pre-check-status), der Status der letzten Sicherung und das Datum des letzten Wiederherstellungspunkts. Über die Auslassungspunkte in der letzten Spalte wird das Menü zum Auslösen allgemeiner Aufgaben geöffnet. Die Spalten enthalten für die einzelnen Sicherungstypen unterschiedliche hilfreiche Daten.

![Öffnen des Menüs mit den Auslassungspunkten für häufige Aufgaben](./media/backup-azure-manage-windows-server/ellipsis-menu.png)

## <a name="manage-backup-jobs"></a>Verwalten von Sicherungsaufträgen

Auf der Kachel **Sicherungsaufträge** im Dashboard des Tresors wird die Anzahl der Aufträge angezeigt, die in den letzten 24 Stunden ausgeführt wurden oder fehlgeschlagen sind. Die Kachel bietet einen Einblick in das Menü „Sicherungsaufträge“.

![Kachel „Sicherungsaufträge“](./media/backup-azure-manage-windows-server/backup-jobs-tile.png)

Um weitere Details zu den Aufträgen anzuzeigen, wählen Sie **In Bearbeitung** oder **Fehler** aus, um das Menü „Sicherungsaufträge“ zu öffnen und nach dem jeweiligen Status zu filtern.

### <a name="backup-jobs-menu"></a>Menü„Sicherungsaufträge“

Im Menü **Sicherungsaufträge** werden Informationen zu Elementtyp, Vorgang, Status, Startzeit und Dauer angezeigt.  

Um das Menü „Sicherungsaufträge“ im Hauptmenü des Tresors zu öffnen, wählen Sie **Sicherungsaufträge** aus.

![Sicherungsaufträge auswählen](./media/backup-azure-manage-windows-server/backup-jobs-menu-item.png)

Die Liste der Sicherungsaufträge wird geöffnet.

![Liste der Sicherungstypen](./media/backup-azure-manage-windows-server/backup-jobs-list.png)

Im Menü „Sicherungsaufträge“ wird der Status für alle Vorgänge für alle Sicherungstypen für die letzten 24 Stunden angezeigt. Verwenden Sie **Filter**, um den Filter zu ändern. Die Filter werden in den folgenden Abschnitten erläutert.

So ändern Sie die Filter:

1. Wählen Sie im Menü „Sicherungsaufträge“ des Tresors **Filter** aus.

   ![Filter für Sicherungsaufträge auswählen](./media/backup-azure-manage-windows-server/vault-backup-job-menu-filter.png)

    Das Menü „Filter“ wird geöffnet.

   ![Menü „Filter“ für Sicherungsaufträge wird geöffnet](./media/backup-azure-manage-windows-server/filter-menu-backup-jobs.png)

2. Wählen Sie die Filtereinstellungen und dann **Fertig** aus. Die gefilterte Liste wird basierend auf den neuen Einstellungen aktualisiert.

#### <a name="item-type"></a>Elementtyp

Der Elementtyp ist der Sicherungsverwaltungstyp der geschützten Instanz. Es gibt vier Arten. Weitere Informationen finden Sie in der folgende Liste. Sie können alle Elementtypen oder nur einem Elementtyp anzeigen. Sie können nicht zwei oder drei Elementtypen auswählen. Die verfügbaren Elementtypen sind:

* Alle Elementtypen
* Virtueller Azure-Computer
* Dateien und Ordner
* Azure Storage
* Azure-Workload

#### <a name="operation"></a>Vorgang

Sie können einen Vorgang oder alle Vorgänge anzeigen. Sie können nicht zwei oder drei Vorgänge auswählen. Die verfügbaren Vorgänge sind:

* Alle Vorgänge
* Register
* Konfigurieren der Sicherung
* Backup
* Restore
* Deaktivieren einer Sicherung
* Löschen von Sicherungsdaten

#### <a name="status"></a>Status

Sie können alle Statusangaben oder einen Status anzeigen. Sie können nicht zwei oder drei Statusangaben auswählen. Die verfügbaren Statusangaben sind:

* Alle Statusangaben
* Abgeschlossen
* In Bearbeitung
* Fehler
* Canceled
* Abgeschlossen mit Warnungen

#### <a name="start-time"></a>Startzeit

Der Tag und die Uhrzeit, zu der die Abfrage beginnt. Der Standardwert ist ein 24-Stunden-Zeitraum.

#### <a name="end-time"></a>Endzeit

Der Tag und die Uhrzeit, zu der die Abfrage endet.

### <a name="export-jobs"></a>Aufträge exportieren

Verwenden Sie **Aufträge exportieren** zum Erstellen einer Tabelle, die alle Auftragsmenüinformationen enthält. Die Tabelle verfügt über ein Blatt, das eine Zusammenfassung aller Aufträge enthält, sowie einzelne Blätter für jeden Auftrag.

Um die Auftragsinformationen in eine Tabelle zu exportieren, wählen Sie **Aufträge exportieren** aus. Der Dienst erstellt eine Tabelle mit dem Namen des Tresors und dem Datum, aber Sie können den Namen ändern.

## <a name="monitor-backup-usage"></a>Überwachen der Sicherungsverwendung

Auf der Kachel „Sicherungsspeicher“ im Dashboard wird der in Azure genutzte Speicher angezeigt. Die Speicherverwendung wird für Folgendes bereitgestellt:

* Cloud-LRS-Speicherverwendung des Tresors
* Cloud-GRS-Speicherverwendung des Tresors

## <a name="troubleshooting-monitoring-issues"></a>Problembehandlung bei der Überwachung

**Problem:** Aufträge und/oder Warnungen aus dem Azure Backup-Agent werden im Portal nicht angezeigt.

**Schritte zur Problembehandlung:** Der Prozess ```OBRecoveryServicesManagementAgent``` sendet den Auftrag und die Warnungsdaten an den Azure Backup-Dienst. Gelegentlich kann es bei diesem Prozess zu Unterbrechungen kommen, oder er kann ungewollt beendet werden.

1. Um sicherzustellen, dass der Prozess nicht gerade ausgeführt wird, öffnen Sie den **Task-Manager**, und überprüfen Sie, ob ```OBRecoveryServicesManagementAgent``` gerade ausgeführt wird.

2. Wenn der Prozess nicht ausgeführt wird, öffnen Sie die **Systemsteuerung**, und durchsuchen Sie die Liste der Dienste. Starten Sie den **Microsoft Azure Recovery Services Management-Agent** bzw. starten Sie ihn neu.

    Weitere Informationen finden Sie in den Protokollen unter:<br/>
   Beispiel: `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*`<br/>
   `C:\Program Files\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider0.errlog`

## <a name="next-steps"></a>Nächste Schritte

* [Wiederherstellen von Windows-Servern oder Windows-Clients aus Azure](backup-azure-restore-windows-server.md)
* Weitere Informationen zu Azure Backup finden Sie unter [Azure Backup – Übersicht](./backup-overview.md)
