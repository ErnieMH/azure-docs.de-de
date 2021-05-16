---
title: Problembehandlung bei der Systemstatussicherung
description: In diesem Artikel erfahren Sie, wie Sie Probleme bei der Systemstatussicherung für lokale Windows-Server beheben können.
ms.reviewer: srinathv
ms.topic: troubleshooting
ms.date: 07/22/2019
ms.openlocfilehash: 54425496428c3534c4c7ae47d10bc3a68256a656
ms.sourcegitcommit: 62e800ec1306c45e2d8310c40da5873f7945c657
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108163965"
---
# <a name="troubleshoot-system-state-backup"></a>Problembehandlung bei der Systemstatussicherung

In diesem Artikel werden Lösungen für Probleme beschrieben, die beim Verwenden der Systemstatussicherung auftreten können.

## <a name="basic-troubleshooting"></a>Grundlegendes zur Problembehandlung

Sie sollten unbedingt die folgenden Prüfungsschritte durchführen, bevor Sie mit der Problembehandlung der Systemstatussicherung beginnen:

- [Sicherstellen, dass der Microsoft Azure Recovery Services-Agent (MARS) auf dem neuesten Stand ist](https://go.microsoft.com/fwlink/?linkid=229525&clcid=0x409)
- [Stellen Sie sicher, dass zwischen dem MARS-Agent und Azure Netzwerkkonnektivität besteht.](./backup-azure-mars-troubleshoot.md#the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup)
- Vergewissern Sie sich, dass Microsoft Azure Recovery Services ausgeführt wird (auf der Dienstkonsole). Führen Sie bei Bedarf einen Neustart durch, und wiederholen Sie den Vorgang.
- [Sicherstellen, dass am Speicherort des Ablageordners 5 - 10% freier Volumespeicherplatz vorhanden ist](./backup-azure-file-folder-backup-faq.yml#what-s-the-minimum-size-requirement-for-the-cache-folder-)
- [Überprüfen, ob ein anderer Prozess oder Antivirensoftware in Azure Backup eingreift](./backup-azure-troubleshoot-slow-backup-performance-issue.md#cause-another-process-or-antivirus-software-interfering-with-azure-backup)
- [Geplante Sicherung nicht erfolgreich, manuelle Sicherung erfolgreich](./backup-azure-mars-troubleshoot.md#backups-dont-run-according-to-schedule)
- Sicherstellen, dass Ihr Betriebssystem über die neuesten Updates verfügt
- [Sicherstellen, dass nicht unterstützte Laufwerke und Dateien mit nicht unterstützten Attributen aus der Sicherung ausgeschlossen werden](backup-support-matrix-mars-agent.md#supported-drives-or-volumes-for-backup)
- Vergewissern Sie sich, dass die **Systemuhr** des geschützten Systems auf die richtige Zeitzone festgelegt ist. <br>
- [Stellen Sie sicher, dass auf dem Server mindestens .NET Framework Version 4.5.2 oder höher installiert ist.](https://www.microsoft.com/download/details.aspx?id=30653)<br>
- Wenn Sie **Ihren Server erneut bei einem Tresor registrieren** möchten: <br>
  - Stellen Sie sicher, dass der Agent auf dem Server deinstalliert und aus dem Portal gelöscht wird. <br>
  - Verwenden Sie dieselbe Passphrase, die ursprünglich zum Registrieren des Servers verwendet wurde. <br>
- Wenn es sich um eine Offlinesicherung handelt, sorgen Sie dafür, dass Azure PowerShell, Version 3.7.0, sowohl auf dem Quell- als auch auf dem Zielcomputer installiert ist, bevor Sie mit dem Offlinesicherungsvorgang beginnen.
- [Aspekte, wenn der Backup-Agent auf einem virtuellen Azure-Computer ausgeführt wird](./backup-azure-troubleshoot-slow-backup-performance-issue.md#cause-backup-agent-running-on-an-azure-virtual-machine)

### <a name="limitation"></a>Einschränkung

- Eine Systemstatuswiederherstellung auf anderer Hardware wird von Microsoft nicht empfohlen.
- Die Systemstatussicherung unterstützt derzeit „lokale“ Windows-Server. Für Azure-VMs ist diese Funktion nicht verfügbar.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit der Problembehandlung bei der Systemstatussicherung mit Azure Backup beginnen, müssen Sie die folgenden Voraussetzungen überprüfen.

### <a name="verify-windows-server-backup-is-installed"></a>Überprüfen, ob Windows Server-Sicherung installiert ist

Stellen Sie sicher, dass Windows Server-Sicherung auf dem Server installiert und aktiviert ist. Um den Installationsstatus zu überprüfen, führen Sie den folgenden PowerShell-Befehl aus:

```powershell
Get-WindowsFeature Windows-Server-Backup
```

Wenn in der Ausgabe der **Installationsstatus** als **verfügbar** angezeigt wird, dann bedeutet dies, dass das Feature „Windows Server-Sicherung“ für die Installation verfügbar, aber nicht auf dem Server installiert ist. Wenn Windows Server-Sicherung jedoch nicht installiert ist, müssen Sie eine der folgenden Methoden verwenden, um das Feature zu installieren.

#### <a name="method-1-install-windows-server-backup-using-powershell"></a>Methode 1: Installieren von Windows Server-Sicherung mit PowerShell

Führen Sie den folgenden Befehl aus, um Windows Server-Sicherung mithilfe von PowerShell zu installieren:

  ```powershell
  Install-WindowsFeature -Name Windows-Server-Backup
  ```

#### <a name="method-2-install-windows-server-backup-using-server-manager"></a>Methode 2: Installieren von Windows Server-Sicherung mit Server-Manager

Führen Sie die folgenden Schritte aus, um Windows Server-Sicherung mithilfe von Server-Manager zu installieren:

1. Wählen Sie im **Server-Manager** die Option **Rollen und Features hinzufügen** aus. Der **Assistent zum Hinzufügen von Rollen und Features** wird geöffnet.

    ![Dashboard](./media/backup-azure-system-state-troubleshoot/server_management.jpg)

2. Wählen Sie **Installationstyp** und dann **Weiter** aus.

    ![Installationstyp](./media/backup-azure-system-state-troubleshoot/install_type.jpg)

3. Wählen Sie einen Server aus dem Serverpool und dann **Weiter** aus. Übernehmen Sie für „Serverrollen“ die Standardauswahl, und wählen Sie **Weiter** aus.
4. Wählen Sie auf der Registerkarte **Features** das Feature **Windows Server-Sicherung** und dann **Weiter** aus.

    ![Auswählen des Fensters „Features“](./media/backup-azure-system-state-troubleshoot/features.png)

5. Wählen Sie auf der Registerkarte **Bestätigung** die Option **Installieren** aus, um den Installationsvorgang zu starten.
6. Auf der Registerkarte **Ergebnisse** wird angezeigt, dass die Installation des Features „Windows Server-Sicherung“ auf Ihrem Windows Server-Computer erfolgreich abgeschlossen wurde.

    ![Ergebnisse der Installation](./media/backup-azure-system-state-troubleshoot/results.jpg)

### <a name="system-volume-information-permission"></a>Berechtigung für „System Volume Information“

Stellen Sie sicher, dass das lokale SYSTEM über Vollzugriff auf den Ordner **System Volume Information** verfügt, der sich auf dem Volume befindet, auf dem Windows installiert ist. Dies ist in der Regel **C:\System Volume Information**. Wenn die obigen Berechtigungen nicht ordnungsgemäß festgelegt wurden, können bei Windows Server-Sicherung Fehler auftreten.

### <a name="dependent-services"></a>Abhängige Dienste

Stellen Sie sicher, dass die folgenden Dienste ausgeführt werden:

**Dienstname** | **Starttyp**
--- | ---
Remoteprozeduraufruf (RPC) | Automatic
COM+-Ereignissystem (EventSystem) | Automatic
Benachrichtigungsdienst für Systemereignisse (SENS) | Automatic
Volumeschattenkopie-Dienst (VSS) | Manuell
Microsoft-Softwareschattenkopie-Anbieter (SWPRV) | Manuell

### <a name="validate-windows-server-backup-status"></a>Überprüfen des Status von Windows Server-Sicherung

Gehen Sie wie folgt vor, um den Status von Windows Server-Sicherung zu überprüfen:

- Stellen Sie sicher, dass WSB-PowerShell ausgeführt wird:

  - Führen Sie an einer PowerShell-Eingabeaufforderung mit erhöhten Rechten den Befehl `Get-WBJob` aus, und stellen Sie sicher, dass nicht der folgende Fehler zurückgegeben wird:

    > [!WARNING]
    > Get-WBJob: Die Benennung „Get-WBJob“ wurde nicht als Name eines Cmdlets, einer Funktion, einer Skriptdatei oder eines ausführbaren Programms erkannt. Prüfen Sie die Schreibweise des Namens bzw. stellen Sie sicher, dass der Pfad korrekt angegeben wurde, und versuchen Sie es erneut.

    - Wenn dieser Fehler auftritt, müssen Sie das Feature Windows Server-Sicherung auf dem Servercomputer neu installieren, wie in Schritt 1 der Voraussetzungen beschrieben.

  - Stellen Sie sicher, dass Windows Server-Sicherung ordnungsgemäß funktioniert, indem Sie an einer Eingabeaufforderung mit erhöhten Rechten den folgenden Befehl ausführen:

      `wbadmin start systemstatebackup -backuptarget:X: -quiet`

      > [!NOTE]
      > Ersetzen Sie X durch den Laufwerkbuchstaben des Volumes, auf dem das Systemstatussicherungsabbild gespeichert werden soll.

    - Überprüfen Sie regelmäßig den Status des Auftrags, indem Sie an einer PowerShell-Eingabeaufforderung mit erhöhten Rechten den Befehl `Get-WBJob` ausführen.
    - Überprüfen Sie nach Abschluss des Sicherungsauftrags den endgültigen Status des Auftrags, indem Sie den Befehl `Get-WBJob -Previous 1` ausführen.

Wenn bei dem Auftrag ein Fehler auftritt, ist dies ein Hinweis auf ein Problem von Windows Server-Sicherung, das zu einem Fehler bei MARS-Agent-Systemstatussicherungen führen würde.

## <a name="common-errors"></a>Häufige Fehler

### <a name="vss-writer-timeout-error"></a>VSS Writer-Timeoutfehler

| Symptom | Ursache | Lösung
| -- | -- | --
| – Fehler des MARS-Agents mit der folgenden Fehlermeldung: „VSS-Fehler bei WSB-Auftrag. Überprüfen Sie die VSS-Ereignisprotokolle, um den Fehler zu beheben.“<br/><br/> – In VSS-Anwendungsereignisprotokollen ist das folgende Fehlerprotokoll vorhanden: „Von einem VSS Writer wurde ein Ereignis mit dem Fehler 0x800423f2 zurückgewiesen. Das Zeitlimit des Generators für den Zeitraum zwischen dem Freeze- und dem Thaw-Ereignis wurde überschritten.“| VSS Writer kann aufgrund des Mangels an CPU- und Arbeitsspeicherressourcen auf dem Computer nicht rechtzeitig abgeschlossen werden. <br/><br/> Der VSS Writer wird bereits von einer anderen Sicherungssoftware verwendet. Daher konnte der Momentaufnahmevorgang für diese Sicherung nicht abgeschlossen werden. | Warten Sie, bis CPU-/Arbeitsspeicherressourcen auf dem System freigegeben wurden, oder brechen Sie die Prozesse ab, die zu viele Arbeitsspeicher-/CPU-Ressourcen beanspruchen, und wiederholen Sie den Vorgang. <br/><br/>  Warten Sie, bis die laufende Sicherung abgeschlossen ist, und wiederholen Sie den Vorgang zu einem späteren Zeitpunkt, wenn keine Sicherungen auf dem Computer ausgeführt werden.

### <a name="insufficient-disk-space-to-grow-shadow-copies"></a>Nicht genügend Speicherplatz zum Vergrößern des Schattenkopiespeichers

| Symptom | Lösung
| -- | --
| - Fehler des MARS-Agents mit der folgenden Fehlermeldung: Fehler bei der Sicherung: Das Schattenkopievolume konnte nicht wachsen, weil auf Volumes mit Systemdateien nicht genügend Speicherplatz verfügbar war. <br/><br/> – In Volsnap-Systemereignisprotokollen ist das folgende Fehler-/Warnungsprotokoll vorhanden: „Es steht nicht genügend Speicherplatz auf Volume "C:" zur Verfügung, um den Schattenkopiespeicher für Schattenkopien von "C:" zu erhöhen. Möglicherweise werden aufgrund dieses Fehlers alle Schattenkopien auf Volume "C:" gelöscht.“ | – Geben Sie Speicherplatz auf dem im Ereignisprotokoll angegebenen Volume frei, damit genügend Speicherplatz zum Vergrößern der Schattenkopien verfügbar ist, während die Sicherung ausgeführt wird. <br/><br/> - Beim Konfigurieren des Speicherplatzes für Schattenkopien kann die für Schattenkopien verwendete Speicherplatzgröße eingeschränkt werden. Weitere Informationen finden Sie in [diesem Artikel](/windows-server/administration/windows-commands/vssadmin-resize-shadowstorage).

### <a name="efi-partition-locked"></a>Gesperrte EFI-Partition

| Symptom | Lösung
| -- | --
| Fehler des MARS-Agents mit der folgenden Fehlermeldung: „Fehler bei der Sicherung des Systemstatus, weil die EFI-Systempartition gesperrt ist. Die Ursache ist möglicherweise ein Systempartitionszugriff durch eine Sicherheits- oder Sicherungssoftware von Drittanbietern.“ | - Wenn die Sicherheitssoftware eines Drittanbieters das Problem verursacht, müssen Sie sich an den Hersteller des Antivirusprogramms wenden, damit er den MARS-Agent zulässt. <br/><br/> - Wenn eine Sicherungssoftware eines Drittanbieters ausgeführt wird, warten Sie, bis die Ausführung abgeschlossen ist, und wiederholen Sie dann die Sicherung.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Windows-Systemstatus in der Resource Manager-Bereitstellung finden Sie unter [Sichern des Systemstatus von Windows Server](backup-azure-system-state.md).
