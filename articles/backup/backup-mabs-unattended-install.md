---
title: Automatische Installation von Azure Backup Server V2
description: Verwenden Sie ein PowerShell-Skript für die automatische Installation von Azure Backup Server v2. Diese Art der Installation wird auch als unbeaufsichtigte Installation bezeichnet.
ms.topic: conceptual
ms.date: 11/13/2018
ms.openlocfilehash: 1539089e713bcf8e959707c6ff4a608f062a7c00
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "74172241"
---
# <a name="run-an-unattended-installation-of-azure-backup-server"></a>Ausführen einer unbeaufsichtigten Installation von Azure Backup Server

Hier erfahren Sie, wie Sie eine unbeaufsichtigte Installation von Azure Backup Server ausführen.

Diese Schritte gelten nicht, wenn Sie Azure Backup Server V1 installieren.

## <a name="install-backup-server"></a>Installieren von Backup Server

1. Erstellen Sie auf dem Server, der Azure Backup Server V2 oder höher hostet, eine Textdatei. (Sie können die Datei in Editor oder einem anderen Texteditor erstellen.) Speichern Sie die Datei als „MABSSetup.ini“.

2. Fügen Sie den folgenden Code in die Datei „MABSSetup.ini“ ein. Ersetzen Sie den Text in Klammern (\< \>) durch Werte aus Ihrer Umgebung. Der folgende Text ist ein Beispiel:

   ```text
   [OPTIONS]
   UserName=administrator
   CompanyName=<Microsoft Corporation>
   SQLMachineName=localhost
   SQLInstanceName=<SQL instance name>
   SQLMachineUserName=administrator
   SQLMachinePassword=<admin password>
   SQLMachineDomainName=<machine domain>
   ReportingMachineName=localhost
   ReportingInstanceName=<reporting instance name>
   SqlAccountPassword=<admin password>
   ReportingMachineUserName=<username>
   ReportingMachinePassword=<reporting admin password>
   ReportingMachineDomainName=<domain>
   VaultCredentialFilePath=<vault credential full path and complete name>
   SecurityPassphrase=<passphrase>
   PassphraseSaveLocation=<passphrase save location>
   UseExistingSQL=<1/0 use or do not use existing SQL>
   ```

3. Speichern Sie die Datei . Geben Sie dann an einer Eingabeaufforderung mit erhöhten Rechten auf dem Installationsserver diesen Befehl ein:

   ```cmd
   start /wait <cdlayout path>/Setup.exe /i  /f <.ini file path>/setup.ini /L <log path>/setup.log
   ```

Sie können diese Flags für die Installation verwenden:</br>
**/f:** Pfad der INI-Datei</br>
**/l**: Protokollpfad</br>
**/i**: Installationspfad</br>
**/x**: Deinstallationspfad</br>

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie nach der Installation von Backup Server Ihren Server vorbereiten oder mit dem Schutz einer Workload beginnen.

- [Vorbereiten von Backup Server-Workloads](backup-azure-microsoft-azure-backup.md)
- [Sichern eines VMware-Servers mit Backup Server](backup-azure-backup-server-vmware.md)
- [Sichern von SQL Server mit Backup Server](backup-azure-sql-mabs.md)
- [Hinzufügen von Modern Backup Storage zu Backup Server](backup-mabs-add-storage.md)
