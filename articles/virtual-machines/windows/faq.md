---
title: Häufig gestellte Fragen zu virtuellen Windows-Computern in Azure
description: Hier finden Sie Antworten auf die häufigsten Fragen zu virtuellen Windows-Computern, die mit dem Resource Manager-Bereitstellungsmodell erstellt wurden.
author: cynthn
ms.service: virtual-machines
ms.collection: windows
ms.workload: infrastructure-services
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: cynthn
ms.openlocfilehash: 0de25b29dc1e930956c01f342ca2614d1a9082ca
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102557504"
---
# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Häufig gestellte Fragen zu virtuellen Windows-Computern
Dieser Artikel enthält einige häufig gestellte Fragen zu virtuellen Windows-Computern, die in Azure mit dem Resource Manager-Bereitstellungsmodell erstellt wurden. Die Linux-Version dieses Themas finden Sie unter [Häufig gestellte Fragen zu virtuellen Linux-Computern](../linux/faq.md).

## <a name="what-can-i-run-on-an-azure-vm"></a>Was kann ich auf einem virtuellen Azure-Computer ausführen?
Alle Abonnenten können Serversoftware auf einem virtuellen Azure-Computer ausführen. Informationen zu den Supportrichtlinien für das Ausführen von Microsoft-Serversoftware in Azure finden Sie unter [Microsoft-Server-Software-Support für virtuelle Maschinen von Microsoft Azure](https://support.microsoft.com/kb/2721672).

Bestimmte Versionen von Windows 7, Windows 8.1 und Windows 10 stehen für Azure-Abonnenten mit MSDN-Vorteilen und für Abonnenten von MSDN Dev/Test Pay-As-You-Go zu Entwicklungs- und Testzwecken bereit. Weitere Informationen, u.a. Anleitungen und Einschränkungen, finden Sie unter [Windows Client images for MSDN subscribers](https://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/) (Windows-Clientimages für MSDN-Abonnenten). 

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Wie viel Speicher kann mit einem virtuellen Computer verwendet werden?
Jeder Datenträger kann bis zu 32.767 GiB groß sein. Die Anzahl der Datenträger, die Sie verwenden können, hängt von der Größe des virtuellen Computers ab. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](../sizes.md).

Azure Managed Disks ist die empfohlene Lösung für die dauerhafte Speicherung von Daten auf Datenträgern, die mit Azure Virtual Machines verwendet werden. Sie können für jeden virtuellen Computer mehrere Datenträger (Managed Disks) verwenden. Managed Disks bietet zwei Arten von permanentem Speicher: verwaltete Datenträger der Tarife Premium und Standard. Informationen zu den Preisen finden Sie unter [Verwaltete Datenträger – Preise](https://azure.microsoft.com/pricing/details/managed-disks).

Mit Azure-Speicherkonten kann auch Speicher für Betriebssystemdatenträger und Datenträger für Daten bereitgestellt werden. Bei jedem Datenträger handelt es sich um eine VHD-Datei, die als Seiten-Blob gespeichert wird. Ausführliche Informationen zu Preisen finden Sie unter [Speicherpreisübersicht](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-can-i-access-my-virtual-machine"></a>Wie kann ich auf meinen virtuellen Computer zugreifen?
Richten Sie eine Remoteverbindung über Remotedesktopverbindung (Remote Desktop Connection, RDP) für einen virtuellen Windows-Computer ein. Anweisungen dazu finden Sie unter [Herstellen einer Verbindung mit einem virtuellen Azure-Computer unter Windows und Anmelden auf diesem Computer](connect-logon.md). Es werden maximal zwei gleichzeitige Verbindungen unterstützt, es sei denn, der Server wurde als Sitzungshost für Remotedesktopdienste konfiguriert.  

Wenn Probleme mit Remotedesktop auftreten, finden Sie weitere Informationen unter [Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](../troubleshooting/troubleshoot-rdp-connection.md?toc=/azure/virtual-machines/windows/toc.json). 

Wenn Sie mit Hyper-V vertraut sind, suchen Sie möglicherweise nach einem ähnlichen Tool wie VMConnect. Azure bietet kein ähnliches Tool, da der Konsolenzugriff auf einem virtuellen Computer nicht unterstützt wird.

## <a name="can-i-use-the-temporary-disk-the-d-drive-by-default-to-store-data"></a>Kann ich den temporären Datenträger (standardmäßig Laufwerk „D:“) verwenden, um Daten zu speichern?
Verwenden Sie den temporären Datenträger nicht zum Speichern von Daten. Dieser dient nur als temporärer Speicher, sodass das Risiko eines Verlusts von Daten besteht, die nicht wiederhergestellt werden können. Wenn ein virtueller Computer auf einen anderen Host verschoben wird, kann Datenverlust auftreten. Gründe für das Verschieben eines virtuellen Computers sind eine Änderung Größe des virtuellen Computers, Aktualisieren des Hosts oder ein Hardwarefehler auf dem Host.

Wenn eine Anwendung den Laufwerkbuchstaben „D:“ benötigt, können Sie die Laufwerkbuchstaben neu zuweisen, damit der temporäre Datenträger einen anderen Buchstaben als „D:“ verwendet. Anweisungen finden Sie unter [Ändern des Datenträgerbuchstabens des temporären Windows-Datenträgers](change-drive-letter.md).


## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>Wie kann ich den Laufwerkbuchstaben des temporären Datenträgers ändern?
Sie können den Laufwerkbuchstaben ändern, indem Sie die Auslagerungsdatei verschieben und die Laufwerkbuchstaben neu zuweisen. Sie müssen dabei jedoch sicherstellen, dass Sie die Schritte in einer bestimmten Reihenfolge ausführen. Anweisungen finden Sie unter [Ändern des Datenträgerbuchstabens des temporären Windows-Datenträgers](change-drive-letter.md).

## <a name="can-i-add-an-existing-vm-to-an-availability-set"></a>Kann ich einen vorhandenen virtuellen Computer zu einer Verfügbarkeitsgruppe hinzufügen?
Nein. Wenn der virtuelle Computer einer Verfügbarkeitsgruppe angehören soll, müssen Sie ihn innerhalb der Gruppe erstellen. Derzeit ist es nicht möglich, einen virtuellen Computer einer Verfügbarkeitsgruppe hinzuzufügen, nachdem er erstellt wurde.

## <a name="can-i-upload-a-virtual-machine-to-azure"></a>Kann ich einen virtuellen Computer in Azure hochladen?
Ja. Eine Anleitung finden Sie unter [Migrieren von Amazon Web Services (AWS) und anderen Plattformen zu Managed Disks in Azure](on-prem-to-azure.md).

## <a name="can-i-resize-the-os-disk"></a>Kann ich die Größe des Betriebssystemlaufwerks ändern?
Ja. Anweisungen finden Sie unter [So erweitern Sie das Betriebssystemlaufwerk eines virtuellen Computers in einer Azure-Ressourcengruppe](expand-os-disk.md).

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Kann ich einen vorhandenen virtuellen Azure-Computer kopieren oder klonen?
Ja. Mithilfe von verwalteten Images können Sie ein Image eines virtuellen Computers erstellen und das Image zum Erstellen mehrerer neuer VMs verwenden. Eine Anleitung finden Sie unter [Erstellen eines benutzerdefinierten Images eines virtuellen Azure-Computers mithilfe von PowerShell](tutorial-custom-images.md).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Warum werden die Regionen „Kanada, Mitte“ und „Kanada, Osten“ nicht in Azure Resource Manager angezeigt?

Die beiden neuen Regionen „Kanada, Mitte“ und „Kanada, Osten“ werden nicht automatisch für das Erstellen von virtuellen Computern im Rahmen von vorhandenen Azure-Abonnements registriert. Diese Registrierung erfolgt automatisch, wenn ein virtueller Computer mit Azure Resource Manager über das Azure-Portal für eine andere Region bereitgestellt wird. Nach der Bereitstellung eines virtuellen Computers in einer anderen Azure-Region sollten die neuen Regionen für nachfolgende virtuelle Computer verfügbar sein.

## <a name="does-azure-support-linux-vms"></a>Unterstützt Azure virtuelle Linux-Computer?
Ja. Unter [Erstellen eines virtuellen Linux-Computers in Azure mithilfe des Portals](../linux/quick-create-portal.md)finden Sie Informationen, wie Sie schnell einen virtuellen Linux-Computer erstellen und ausprobieren können.

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Kann ich meinem virtuellen Computer nach der Erstellung eine NIC hinzufügen?
Ja, dies ist jetzt möglich. Der virtuelle Computer muss zuerst beendet/freigegeben werden. Anschließend können Sie eine NIC hinzufügen oder entfernen (sofern es sich nicht um die letzte NIC auf dem virtuellen Computer handelt). 

## <a name="are-there-any-computer-name-requirements"></a>Gibt es Anforderungen an den Computernamen?
Ja. Der Computername kann maximal 15 Zeichen lang sein. Weitere Informationen zum Benennen von Ressourcen finden Sie unter [Regeln und Einschränkungen der Namenskonventionen](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging).

## <a name="are-there-any-resource-group-name-requirements"></a>Gelten für Namen von Ressourcengruppen bestimmte Anforderungen?
Ja. Der Name der Ressourcengruppe darf maximal 90 Zeichen lang sein. Weitere Informationen zu Ressourcengruppen finden Sie unter [Regeln und Einschränkungen der Namenskonventionen](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging#resource-naming).

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Welche Anforderungen an den Benutzernamen gelten beim Erstellen eines virtuellen Computers?

Benutzernamen können maximal 20 Zeichen lang sein und dürfen nicht mit einem Punkt („.“) enden. 

Die folgenden Benutzernamen sind nicht zulässig:

- `1`
- `123`
- `a`
- `actuser`
- `adm`
- `admin`
- `admin1`
- `admin2`
- `administrator`
- `aspnet`
- `backup`
- `console`
- `david`
- `guest`
- `john`
- `owner`
- `root`
- `server`
- `sql`
- `support_388945a0`
- `support`
- `sys`
- `test`
- `test1`
- `test2`
- `test3`
- `user`
- `user1`
- `user2`

## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Welche Anforderungen an das Kennwort gelten beim Erstellen eines virtuellen Computers?

Je nach verwendetem Tool gelten unterschiedliche Anforderungen an die Kennwortlänge:
 - Portal: zwischen 12 und 72 Zeichen
 - PowerShell: zwischen 8 und 123 Zeichen
 - Befehlszeilenschnittstelle: zwischen 12 und 123 Zeichen

* Kleinbuchstaben
* Großbuchstaben
* Eine Ziffer
* Ein Sonderzeichen (Übereinstimmung mit regulärem Ausdruck [\W_])

Die folgenden Kennwörter sind nicht zulässig:

<table>
    <tr>
        <td>abc@123</td>
        <td>iloveyou!</td>
        <td>P@$$w0rd</td>
        <td>P@ssw0rd</td>
        <td>P@ssword123</td>
    </tr>
    <tr>
        <td>Pa$$word</td>
        <td>pass@word1</td>
        <td>Password!</td>
        <td>Password1</td>
        <td>Password22</td>
    </tr>
</table>