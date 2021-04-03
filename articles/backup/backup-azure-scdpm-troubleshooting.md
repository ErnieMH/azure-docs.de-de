---
title: Behandeln von Problemen in System Center Data Protection Manager
description: Entdecken Sie in diesem Artikel Lösungen von Problemen, die bei der Verwendung von System Center Data Protection Manager auftreten könnten.
ms.topic: troubleshooting
ms.date: 01/30/2019
ms.openlocfilehash: 84646f34d905b570855f655465529eb19b717cab
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "86513811"
---
# <a name="troubleshoot-system-center-data-protection-manager"></a>Behandeln von Problemen in System Center Data Protection Manager

Dieser Artikel beschreibt Lösungen für Probleme, die bei der Verwendung von Data Protection Manager auftreten können.

Die neuesten Versionshinweise für System Center Data Protection Manager finden Sie in der [Dokumentation zu System Center](/system-center/dpm/dpm-release-notes). Weitere Informationen zur Unterstützung von Data Protection Manager finden Sie in [dieser Matrix](/system-center/dpm/dpm-protection-matrix).

## <a name="error-replica-is-inconsistent"></a>Error: Replikat inkonsistent

Ein Replikat kann aus den folgenden Gründen inkonsistent sein:

- Bei der Replikaterstellung tritt ein Fehler auf.
- Es gibt Probleme mit dem Änderungsjournal.
- Die Volumeebenenfilterbitmap enthält Fehler.
- Der Quellcomputer fährt unerwartet herunter.
- Das Synchronisierungsprotokoll führt zu einem Überlauf.
- Das Replikat ist tatsächlich inkonsistent.

Führen Sie die folgenden Schritte aus, um das Problem zu beheben:

- Führen Sie zum Entfernen des inkonsistenten Status die Konsistenzprüfung manuell durch, oder planen Sie eine tägliche Konsistenzprüfung.
- Stellen Sie sicher, dass Sie die neueste Version von Microsoft Azure Backup Server und Data Protection Manager verwenden.
- Stellen Sie sicher, dass die Einstellung für **automatische Konsistenz** aktiviert ist.
- Starten Sie die Dienste über die Eingabeaufforderung neu. Verwenden den Befehl `net stop dpmra` gefolgt von `net start dpmra`.
- Stellen Sie sicher, dass Anforderungen an die Netzwerkverbindung und Bandbreite erfüllt sind.
- Überprüfen Sie, ob der Quellcomputer unerwartet heruntergefahren wurde.
- Stellen Sie sicher, dass der Datenträger fehlerfrei und genügend Speicherplatz für das Replikat vorhanden ist.
- Stellen Sie sicher, dass keine doppelten Sicherungsaufträge gleichzeitig ausgeführt werden.

## <a name="error-online-recovery-point-creation-failed"></a>Error: Fehler bei der Erstellung eines Onlinewiederherstellungspunkts.

Führen Sie die folgenden Schritte aus, um das Problem zu beheben:

- Vergewissern Sie sich, dass Sie die neueste Version des Azure Backup-Agents verwenden.
- Versuchen Sie, manuell einen Wiederherstellungspunkt im Aufgabenbereich „Schutz“ zu erstellen.
- Stellen Sie sicher, dass Sie die Konsistenzprüfung für die Datenquelle ausführen.
- Stellen Sie sicher, dass Anforderungen an die Netzwerkverbindung und Bandbreite erfüllt sind.
- Wenn sich die Replikatdaten in einem inkonsistenten Zustand befinden, erstellen Sie einen Datenträgerwiederherstellungspunkt für die Datenquelle.
- Stellen Sie sicher, dass das Replikat vorhanden ist und nicht fehlt.
- Stellen Sie sicher, dass im Replikat ausreichend Speicherplatz für das Aktualisieren des USN-Journals (Updatesequenznummer) vorhanden ist.

## <a name="error-unable-to-configure-protection"></a>Error: Der Schutz kann nicht konfiguriert werden.

Dieser Fehler tritt auf, wenn der Data Protection Manager-Server keinen Kontakt zum geschützten Server herstellen kann.

Führen Sie die folgenden Schritte aus, um das Problem zu beheben:

- Vergewissern Sie sich, dass Sie die neueste Version des Azure Backup-Agents verwenden.
- Stellen Sie sicher, dass eine Verbindung (Netzwerk/Firewall/Proxy) zwischen dem Data Protection Manager-Server und dem geschützten Server besteht.
- Wenn Sie einen SQLServer schützen, stellen Sie sicher, dass in der Eigenschaft **Anmeldungseigenschaften** > **NT AUTHORITY\SYSTEM** die Einstellung **Sysadmin** aktiviert ist.

## <a name="error-server-not-registered-as-specified-in-vault-credential-file"></a>Error: Serverregistrierung entspricht nicht den Tresoranmeldeinformationen.

Dieser Fehler tritt während des Wiederherstellungsprozesses von Data Protection Manager/Azure Backup-Server-Daten aus. Die im Wiederherstellungsprozess verwendete Datei mit den Tresoranmeldeinformationen gehört nicht zum Recovery Services-Tresor für den Data Protection Manager/Azure Backup-Server.

Gehen Sie folgendermaßen vor, um das Problem zu beheben:

1. Laden Sie die Datei mit den Tresoranmeldeinformationen aus dem Recovery Services-Tresor herunter, bei dem der Data Protection Manager/Azure Backup-Server registriert ist.
2. Versuchen Sie, den Server mit der neuesten heruntergeladenen Datei mit den Tresoranmeldeinformationen bei dem Tresor zu registrieren.

## <a name="error-no-recoverable-data-or-selected-server-not-a-data-protection-manager-server"></a>Error: Keine wiederherstellbaren Daten oder gewählter Server ist kein Data Protection Manager-Server

Dieser Fehler tritt aus den folgenden Gründen auf:

- Keine anderen Data Protection Manager/Azure Backup-Server sind im Recovery Services-Tresor registriert.
- Der Server hat die Metadaten noch nicht hochgeladen.
- Der ausgewählte Server ist kein Data Protection Manager/Azure Backup-Server.

Führen Sie zum Beheben des Problems die folgenden Schritte aus, wenn keine anderen Data Protection Manager/Azure Backup-Server im Recovery Services-Tresor registriert sind:

1. Prüfen Sie, ob der neueste Azure Backup-Agent installiert ist.
2. Wenn Sie sichergestellt haben, dass der aktuelle Agent installiert ist, warten Sie ein Tag, bis Sie den Wiederherstellungsprozess starten. Im nächtlichen Sicherungsauftrag werden die Metadaten für alle geschützten Sicherungen in die Cloud hochgeladen. Die Sicherungsdaten sind dann für die Wiederherstellung verfügbar.

## <a name="error-provided-encryption-passphrase-doesnt-match-passphrase-for-server"></a>Error: Angegebene Verschlüsselungspassphrase stimmt nicht mit Passphrase für Server überein.

Dieser Fehler tritt bei der Verschlüsselung während des Wiederherstellungsprozesses von Data Protection Manager/Azure Backup-Server-Daten aus. Die im Wiederherstellungsprozess verwendete Verschlüsselungspassphrase stimmt nicht mit der Verschlüsselungspassphrase des Servers überein. Daher kann der Agent die Daten nicht entschlüsseln, und die Wiederherstellung schlägt fehl.

> [!IMPORTANT]
> Wenn Sie die Verschlüsselungspassphrase vergessen oder verlieren, sind keine anderen Methoden zum Wiederherstellen der Daten verfügbar. Die einzige Möglichkeit ist, die Passphrase erneut zu generieren. Verwenden Sie die neue Passphrase, um zukünftige Sicherungsdaten zu verschlüsseln.
>
> Wenn Sie Daten wiederherstellen, geben Sie immer die Verschlüsselungspassphrase an, die mit dem Data Protection Manager/Azure Backup-Server verknüpft ist.
>
