---
title: Die serielle Azure-Konsole für Linux | Microsoft-Dokumentation
description: Bidirektionale serielle Konsole für Azure Virtual Machines und Virtual Machine Scale Sets unter Verwendung eines Linux-Beispiels.
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: borisb
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 5/1/2019
ms.author: alsin
ms.openlocfilehash: 9a31a22a5b037162198f594d9bcf35c91a0a4654
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91306870"
---
# <a name="azure-serial-console-for-linux"></a>Die serielle Azure-Konsole für Linux

Die serielle Konsole im Azure-Portal ermöglicht den Zugriff auf eine textbasierte Konsole für virtuelle Linux-Computer (VMs) und Instanzen von VM-Skalierungsgruppen. Diese serielle Verbindung erfolgt über den seriellen ttys0-Port der VM oder der VM-Skalierungsgruppe und ermöglicht Zugriff auf diesen, unabhängig vom Zustand des Netzwerks oder Betriebssystems. Auf die serielle Konsole kann nur über das Azure-Portal und von Benutzern zugegriffen werden, die mindestens über die Zugriffsrolle „Mitwirkender“ für die VM oder VM-Skalierungsgruppeninstanzen verfügen.

Die serielle Konsole funktioniert auf die gleiche Weise für VMs und VM-Skalierungsgruppeninstanzen. Deshalb beziehen sich alle Äußerungen bezüglich VMs in dieser Dokumentation, sofern nicht anders angegeben, implizit auch auf VM-Skalierungsgruppeninstanzen.

Die serielle Konsole ist in den globalen Azure-Regionen allgemein und in Azure Government als Public Preview verfügbar. Sie ist noch nicht in der Cloud „Azure China“ verfügbar.

Die Dokumentation zur seriellen Konsole für Windows finden Sie unter [Serielle Konsole für Windows](./serial-console-windows.md).

> [!NOTE]
> Zurzeit ist die serielle Konsole nicht mit verwalteten Speicherkonten für die Startdiagnose kompatibel. Stellen Sie zum Verwenden der seriellen Konsole sicher, dass Sie ein benutzerdefiniertes Speicherkonto verwenden.

## <a name="prerequisites"></a>Voraussetzungen

- Ihre VM oder VM-Skalierungsgruppeninstanz muss das Bereitstellungsmodell für die Ressourcenverwaltung verwenden. Klassische Bereitstellungen werden nicht unterstützt.

- Ihr Konto, das eine serielle Konsole verwendet, muss die Rolle [Mitwirkender für virtuelle Computer](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) für die VM und das Speicherkonto [Startdiagnose](boot-diagnostics.md) aufweisen.

- Ihre VM oder VM-Skalierungsgruppeninstanz muss über einen kennwortbasierten Benutzer verfügen. Mit der Funktion [Kennwort zurücksetzen](../extensions/vmaccess.md#reset-password) der Erweiterungen für den Zugriff auf virtuelle Computer können Sie eines erstellen. Wählen Sie im Abschnitt **Support + Problembehandlung** **Kennwort zurücksetzen** aus.

- Die [Startdiagnose](boot-diagnostics.md) muss für Ihre VM oder VM-Skalierungsgruppeninstanz aktiviert sein.

    ![Einstellungen der Startdiagnose](./media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)

- Spezifische Einstellungen für Linux-Distributionen finden Sie unter [Verfügbarkeit der seriellen Konsole in Linux-Distributionen](#serial-console-linux-distribution-availability).

- Ihre VM oder VM-Skalierungsgruppeninstanz muss für die serielle Ausgabe unter `ttys0` konfiguriert werden. Dies ist die Standardeinstellung für Azure-Images, aber es ist ratsam, dies auch für benutzerdefinierte Images zu überprüfen. Details hierzu finden Sie [weiter unten](#custom-linux-images).


> [!NOTE]
> Die serielle Konsole erfordert einen lokalen Benutzer mit konfiguriertem Kennwort. VMs oder VM-Skalierungsgruppen, die nur mit einem öffentlichen SSH-Schlüssel konfiguriert wurden, können sich nicht bei der seriellen Konsole anmelden. Wenn Sie einen lokalen Benutzer mit Kennwort erstellen möchten, verwenden Sie die [Erweiterung für VM-Zugriff](../extensions/vmaccess.md), die im Azure-Portal durch Auswählen von **Kennwort zurücksetzen** verfügbar ist, und erstellen Sie einen lokalen Benutzer mit einem Kennwort.
> Sie können auch das Administratorkennwort in Ihrem Konto zurücksetzen, indem Sie [GRUB verwenden, um im Einzelbenutzermodus zu starten](./serial-console-grub-single-user-mode.md).

## <a name="serial-console-linux-distribution-availability"></a>Verfügbarkeit der seriellen Konsole in Linux-Distributionen
Damit die serielle Konsole ordnungsgemäß ausgeführt wird, muss das Gastbetriebssystem so konfiguriert werden, dass Konsolenmeldungen über den seriellen Anschluss gelesen und geschrieben werden. In den meisten [von Azure unterstützten Linux-Distributionen](../linux/endorsed-distros.md) ist die serielle Konsole standardmäßig konfiguriert. Zugriff auf die serielle Konsole erhalten Sie, indem Sie im Azure-Portal im Bereich **Support + Problembehandlung** **Serielle Konsole** auswählen.

> [!NOTE]
> Wenn in der seriellen Konsole nichts angezeigt wird, überprüfen Sie, ob die Startdiagnose auf Ihrer VM aktiviert ist. Das Drücken der **EINGABETASTE** kann häufig Probleme beheben, bei denen nichts in der seriellen Konsole angezeigt wird.

Distribution      | Zugriff auf die serielle Konsole
:-----------|:---------------------
Red Hat Enterprise Linux    | Der Zugriff auf die serielle Konsole ist standardmäßig aktiviert.
CentOS      | Der Zugriff auf die serielle Konsole ist standardmäßig aktiviert.
Debian      | Der Zugriff auf die serielle Konsole ist standardmäßig aktiviert.
Ubuntu      | Der Zugriff auf die serielle Konsole ist standardmäßig aktiviert.
CoreOS      | Der Zugriff auf die serielle Konsole ist standardmäßig aktiviert.
SUSE        | Für die neueren SLES-Images, die auf Azure verfügbar sind, ist der Zugriff auf die serielle Konsole standardmäßig aktiviert. Wenn Sie in Azure ältere Versionen von SLES (10 oder niedriger) verwenden, lesen Sie die Anweisungen in [diesem KB-Artikel](https://www.novell.com/support/kb/doc.php?id=3456486), um die serielle Konsole zu aktivieren.
Oracle Linux        | Der Zugriff auf die serielle Konsole ist standardmäßig aktiviert.

### <a name="custom-linux-images"></a>Benutzerdefinierte Linux-Images
Um die serielle Konsole für Ihr benutzerdefiniertes Linux-VM-Image zu aktivieren, aktivieren Sie den Konsolenzugriff in der Datei */etc/inittab*, um ein Terminal auf `ttyS0` auszuführen. Beispiel: `S0:12345:respawn:/sbin/agetty -L 115200 console vt102`. Möglicherweise müssen Sie auch einen "Getty on ttyS0"-Vorgang erzeugen. Dies kann mit `systemctl start serial-getty@ttyS0.service` erreicht werden.

Außerdem ist es ratsam, „ttys0“ als Ziel für die serielle Ausgabe hinzuzufügen. Weitere Informationen zur Konfiguration eines benutzerdefinierten Images zur Verwendung mit der seriellen Konsole finden Sie in den allgemeinen Systemanforderungen unter [Erstellen und Hochladen einer Linux-VHD in Azure](https://aka.ms/createuploadvhd#general-linux-system-requirements).

Wenn Sie einen benutzerdefinierten Kernel erstellen, sollten Sie die Aktivierung dieser Kernelflags in Erwägung ziehen: `CONFIG_SERIAL_8250=y` und `CONFIG_MAGIC_SYSRQ_SERIAL=y`. Die Konfigurationsdatei befindet sich normalerweise im Pfad */boot/* .

## <a name="common-scenarios-for-accessing-the-serial-console"></a>Gängige Szenarios für den Zugriff auf die serielle Konsole

Szenario          | Aktionen in der seriellen Konsole
:------------------|:-----------------------------------------
Fehlerhafte *FSTAB*-Datei | Drücken Sie die **EINGABETASTE**, um fortzufahren, und verwenden Sie einen Text-Editor, um die Datei *FSTAB* zu korrigieren. Dazu müssen Sie sich möglicherweise im Einzelbenutzermodus befinden. Weitere Informationen finden Sie im Abschnitt zur seriellen Konsole unter [Beheben von FSTAB-Fehlern](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors) und [Verwenden der seriellen Konsole zum Zugreifen auf GRUB und den Einzelbenutzermodus](serial-console-grub-single-user-mode.md).
Falsche Firewallregeln |  Wenn Sie iptables zum Blockieren der SSH-Konnektivität konfiguriert haben, können Sie die serielle Konsole für die Interaktion mit Ihrer VM verwenden, ohne dass Sie SSH benötigen. Weitere Details finden Sie auf der Seite zu [iptables man](https://linux.die.net/man/8/iptables).<br>Falls der SSH-Zugriff durch firewalld blockiert wird, können Sie auch über die serielle Konsole auf die VM zugreifen und firewalld neu konfigurieren. Weitere Informationen hierzu finden Sie in der [firewalld-Dokumentation](https://firewalld.org/documentation/).
Dateisystembeschädigung/-überprüfung | Weitere Informationen zur Problembehandlung für beschädigte Dateisysteme per serieller Konsole finden Sie im entsprechenden Abschnitt unter [Azure Linux VM kann nicht wegen Dateisystemfehler gestartet werden](https://support.microsoft.com/en-us/help/3213321/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck).
SSH-Konfigurationsprobleme | Greifen Sie auf die serielle Konsole zu, und ändern Sie die Einstellungen. Die serielle Konsole kann unabhängig von der SSH-Konfiguration einer VM verwendet werden, da für die VM hierfür keine Netzwerkverbindung erforderlich ist. Einen Leitfaden zur Problembehandlung finden Sie unter [Behandeln von Problemen, Fehlern oder Ablehnungen im Zusammenhang mit der SSH-Verbindung mit einem virtuellen Azure Linux-Computer](./troubleshoot-ssh-connection.md). Weitere Informationen finden Sie unter [Ausführliche Schritte zum Beheben von SSH-Verbindungsproblemen mit einer Azure-VM unter Linux](./detailed-troubleshoot-ssh-connection.md).
Interaktion mit Bootloader | Starten Sie auf dem Blatt der seriellen Konsole Ihren virtuellen Computer neu, um auf Ihrem virtuellen Linux-Computer auf GRUB zuzugreifen. Weitere Informationen und distributionsspezifische Informationen finden Sie unter [Verwenden der seriellen Konsole zum Zugreifen auf den GRUB- und Einzelbenutzermodus](serial-console-grub-single-user-mode.md).

## <a name="disable-the-serial-console"></a>Deaktivieren der seriellen Konsole

Die serielle Konsole ist standardmäßig für alle Abonnements aktiviert. Sie können die serielle Konsole entweder für das Abonnement oder für die VMs bzw. VM-Skalierungsgruppen deaktivieren. Ausführliche Anweisungen finden Sie unter [Aktivieren und Deaktivieren der seriellen Azure-Konsole](./serial-console-enable-disable.md).

## <a name="serial-console-security"></a>Sicherheit der seriellen Konsole

### <a name="access-security"></a>Zugriffssicherheit
Der Zugriff auf die serielle Konsole ist auf Benutzer eingeschränkt, die über eine Zugriffsrolle als [Mitwirkender für virtuelle Computer](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) oder höher für den virtuellen Computer verfügen. Wenn für Ihren Azure Active Directory-Mandanten mehrstufige Authentifizierung (Multi-Factor Authentication, MFA) erforderlich ist, ist MFA auch für den Zugriff auf die serielle Konsole erforderlich, weil der Zugriff auf die serielle Konsole über das [Azure-Portal](https://portal.azure.com) erfolgt.

### <a name="channel-security"></a>Kanalsicherheit
Alle gesendeten Daten werden bei der Übertragung verschlüsselt.

### <a name="audit-logs"></a>Überwachungsprotokolle
Alle Zugriffe auf die serielle Konsole werden zurzeit in den Protokollen [Startdiagnose](./boot-diagnostics.md) des virtuellen Computers protokolliert. Der Zugriff auf diese Protokolle wird durch den Administrator des virtuellen Azure-Computers (der auch Besitzer dieser Protokolle ist) gesteuert.

> [!CAUTION]
> Zugriffskennwörter für die Konsole werden nicht protokolliert. Wenn jedoch innerhalb der Konsole Befehle ausgeführt werden, die Kennwörter, Geheimnisse, Benutzernamen oder personenbezogene Informationen anderer Art enthalten oder ausgeben, werden diese in den Protokollen der VM-Startdiagnose erfasst. Diese werden gemeinsam mit dem gesamten anderen sichtbaren Text als Teil der Implementierung der Scrollbackfunktion der seriellen Konsole geschrieben. Diese Protokolle sind zirkulär, und nur Personen mit Leseberechtigungen für das Speicherkonto der Diagnose haben auf sie Zugriff. Wenn Sie Daten oder Befehle mit Geheimnissen oder personenbezogenen Informationen einfügen, empfiehlt es sich, SSH zu verwenden, sofern die serielle Konsole nicht unbedingt erforderlich ist.

### <a name="concurrent-usage"></a>Parallele Verwendung
Wenn ein Benutzer mit der seriellen Konsole verbunden ist und ein anderer Benutzer erfolgreich den Zugriff auf denselben virtuellen Computer anfordert, wird der erste Benutzer getrennt und der zweite Benutzer mit der gleichen Sitzung verbunden.

> [!CAUTION]
> Dies bedeutet, dass ein Benutzer, der getrennt wird, nicht abgemeldet wird. Die Möglichkeit, bei der Trennung der Verbindung eine Abmeldung zu erzwingen (mithilfe von SIGHUP oder einem ähnlichen Mechanismus), ist noch nicht implementiert. Für Windows ist in SAC (Special Administrative Console, spezielle Verwaltungskonsole) ein automatisches Timeout aktiviert. Für Linux können Sie die Timeouteinstellung für das Terminal konfigurieren. Fügen Sie zu diesem Zweck Ihrer *.bash_profile*- oder *.profile*-Datei für den Benutzer, den Sie für die Anmeldung bei der Konsole verwenden, `export TMOUT=600` hinzu. Durch diese Einstellung erfolgt nach 10 Minuten ein Timeout der Sitzung.

## <a name="accessibility"></a>Barrierefreiheit
Die Barrierefreiheit ist ein wichtiger Aspekt bei der seriellen Konsole in Azure. Daher haben wir sichergestellt, dass die serielle Konsole vollständig barrierefrei ist.

### <a name="keyboard-navigation"></a>Tastaturnavigation
Verwenden Sie die **TAB**-Taste auf der Tastatur, um im Azure-Portal in der seriellen Konsolenschnittstelle zu navigieren. Auf dem Bildschirm wird hervorgehoben, wo Sie sich gerade befinden. Um den Fokus vom seriellen Konsolenbereich zu entfernen, drücken Sie **STRG**+**F6** auf der Tastatur.

### <a name="use-serial-console-with-a-screen-reader"></a>Verwenden der seriellen Konsole mit einer Sprachausgabe
Die serielle Konsole bietet integrierte Unterstützung für die Sprachausgabe. Beim Navigieren mit aktivierter Sprachausgabe kann der Alternativtext für die aktuell ausgewählte Schaltfläche von der Sprachausgabe vorgelesen werden.

## <a name="known-issues"></a>Bekannte Probleme
Uns sind einige Probleme mit der seriellen Konsole und dem Betriebssystem der VM bekannt. Hier finden Sie eine Liste dieser Probleme und Schritte zur Lösung für Linux-VMs. Diese Probleme und Gegenmaßnahmen gelten sowohl für VMs als auch für VM-Skalierungsgruppeninstanzen. Wenn sie sich nicht auf den Fehler beziehen, den Sie suchen, schlagen Sie unter [Allgemeine Fehler für die serielle Konsole](./serial-console-errors.md) die allgemeinen Fehler für den seriellen Konsolendienst nach.

Problem                           |   Minderung
:---------------------------------|:--------------------------------------------|
Das Drücken der **EINGABETASTE** nach dem Verbindungsbanner führt nicht zur Anzeige einer Anmeldeaufforderung. | Möglicherweise ist GRUB nicht ordnungsgemäß konfiguriert. Führen Sie die folgenden Befehle aus: `grub2-mkconfig -o /etc/grub2-efi.cfg` und/oder `grub2-mkconfig -o /etc/grub2.cfg`. Weitere Informationen finden Sie unter [Hitting enter does nothing](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md) (Das Drücken der Eingabetaste bewirkt nichts). Dieses Problem kann auftreten, wenn Sie eine benutzerdefinierte VM, eine Appliance mit verstärkter Sicherheit oder eine GRUB-Konfiguration ausführen, und Linux aufgrund dessen keine Verbindung mit dem seriellen Port herstellen kann.
Der Text in der seriellen Konsole nimmt nur einen Teil der Größe des Bildschirms in Anspruch (häufig nach Verwendung eines Text-Editors). | Serielle Konsolen unterstützen keine Aushandlung der Fenstergröße ([RFC 1073](https://www.ietf.org/rfc/rfc1073.txt)). Daher wird kein SIGWINCH-Signal gesendet, um die Bildschirmgröße zu aktualisieren, und der virtuelle Computer erhält keine Informationen über die Größe Ihres Terminals. Installieren Sie xterm oder ein ähnliches Hilfsprogramm, das über einen `resize`-Befehl verfügt, und führen Sie dann `resize` aus.
Das Einfügen von langen Zeichenfolgen funktioniert nicht. | Die serielle Konsole begrenzt die Länge der Zeichenfolgen, die in das Terminal eingefügt werden können, auf 2048 Zeichen, um die Bandbreite am seriellen Port nicht zu überlasten.
Fehlerhafte Tastatureingaben in SLES-BYOS-Images. Tastatureingaben werden nur sporadisch erkannt. | Dies ist ein Problem mit dem Plymouth-Paket. Plymouth sollte nicht in Azure ausgeführt werden, da Sie keinen Begrüßungsbildschirm benötigen. Außerdem beeinträchtigt Plymouth die Fähigkeit der Plattform, die serielle Konsole zu verwenden. Entfernen Sie Plymouth mit `sudo zypper remove plymouth`, und führen Sie dann einen Neustart durch. Sie können auch die Kernelzeile in Ihrer GRUB-Konfiguration ändern und `plymouth.enable=0` an das Ende der Zeile anfügen. Dazu [bearbeiten Sie den Starteintrag beim Systemstart](https://aka.ms/serialconsolegrub#single-user-mode-in-suse-sles) oder die Zeile GRUB_CMDLINE_LINUX in `/etc/default/grub`, erstellen GRUB mit `grub2-mkconfig -o /boot/grub2/grub.cfg` neu und führen dann einen Neustart durch.


## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**F: Wie kann ich Feedback senden?**

A. Geben Sie Feedback, indem Sie ein GitHub-Problem unter https://aka.ms/serialconsolefeedback erstellen. Alternativ dazu können Sie Feedback über azserialhelp@microsoft.com oder in der Kategorie „Virtuelle Computer“ von https://feedback.azure.com senden (dies sind die weniger bevorzugten Methoden).

**F: Unterstützt die serielle Konsole das Kopieren und Einfügen?**

A. Ja. Verwenden Sie **STRG**+**UMSCHALT**+**C** und **STRG**+**UMSCHALT**+**V** zum Kopieren und Einfügen in das Terminal.

**F: Kann ich die serielle Konsole anstelle einer SSH-Verbindung verwenden?**

A. Diese Nutzung mag technisch möglich sein, doch ist die serielle Konsole in erster Linie als ein Tool zur Problembehandlung in Situationen gedacht, in denen keine Verbindung über SSH möglich ist. Aus den folgenden Gründen raten wir davon ab, die serielle Konsole als SSH-Ersatz zu verwenden:

- Die serielle Konsole hat nicht so viel Bandbreite wie SSH. Da es sich um eine reine Textverbindung handelt, sind eher GUI-lastige Interaktionen schwierig.
- Der Zugriff auf die serielle Konsole ist derzeit nur über Benutzername und Kennwort möglich. SSH-Schlüssel sind weitaus sicherer als Benutzername/Kennwort-Kombinationen. Daher ist aus Gründen der Anmeldesicherheit SSH der seriellen Konsole vorzuziehen.

**F: Wer kann die serielle Konsole für mein Abonnement aktivieren oder deaktivieren?**

A. Um die serielle Konsole auf Abonnementebene aktivieren oder deaktivieren zu können, müssen Sie über Schreibberechtigungen für das Abonnement verfügen. Die Rollen „Administrator“ und „Besitzer“ verfügen beispielsweise über Schreibberechtigungen. Benutzerdefinierte Rollen können ebenfalls über Schreibberechtigungen verfügen.

**F: Wer kann auf die serielle Konsole für meine VM/VM-Skalierungsgruppe zugreifen?**

A. Sie müssen mindestens der Rolle „Mitwirkender für virtuelle Computer“ einer VM oder VM-Skalierungsgruppe angehören, um auf die serielle Konsole zuzugreifen.

**F: Meine serielle Konsole zeigt nichts an, was muss ich tun?**

A. Ihr Image ist wahrscheinlich nicht richtig für den Zugriff auf die serielle Konsole konfiguriert. Informationen zum Konfigurieren Ihres Images für die Aktivierung der seriellen Konsole finden Sie unter [Verfügbarkeit der seriellen Konsole in Linux-Distributionen](#serial-console-linux-distribution-availability).

**F: Ist die serielle Konsole für VM-Skalierungsgruppen verfügbar?**

A. Ja. Siehe [Serielle Konsole für Virtual Machine Scale Sets](serial-console-overview.md#serial-console-for-virtual-machine-scale-sets).

**F: Ich habe meine VM oder VM-Skalierungsgruppe nur mit SSH-Schlüsselauthentifizierung eingerichtet. Kann ich trotzdem die serielle Konsole verwenden, um eine Verbindung mit meiner VM/VM-Skalierungsgruppeninstanz herzustellen?**

A. Ja. Da die serielle Konsole keine SSH-Schlüssel erfordert, brauchen Sie nur eine Kombination aus Benutzername und Kennwort einzurichten. Dazu können Sie im Azure-Portal **Kennwort zurücksetzen** auswählen und diese Anmeldeinformationen für die Anmeldung bei der seriellen Konsole verwenden.

## <a name="next-steps"></a>Nächste Schritte
* Verwenden der seriellen Konsole zum [Zugreifen auf GRUB und den Einzelbenutzermodus](serial-console-grub-single-user-mode.md).
* Verwenden der seriellen Konsole für [NMI- und SysRq-Aufrufe](serial-console-nmi-sysrq.md).
* Erfahren Sie, wie Sie die serielle Konsole zum [Aktivieren von GRUB in verschiedenen Distributionen](serial-console-grub-proactive-configuration.md) verwenden.
* Die serielle Konsole ist auch für [virtuelle Windows-Computer](./serial-console-windows.md) verfügbar.
* Erfahren Sie mehr zur [Startdiagnose](boot-diagnostics.md).
