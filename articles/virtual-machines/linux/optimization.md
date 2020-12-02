---
title: Optimieren virtueller Linux-Computer in Azure
description: Hier finden Sie einige Optimierungstipps, um mit virtuellen Linux-Computern in Azure eine optimale Leistung zu erzielen.
author: rickstercdn
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 09/06/2016
ms.author: rclaus
ms.subservice: disks
ms.openlocfilehash: 1e3551834e7664d5036fa8a5e0497e5a37f61c2f
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2020
ms.locfileid: "96498505"
---
# <a name="optimize-your-linux-vm-on-azure"></a>Optimieren virtueller Linux-Computer in Azure
Virtuelle Linux-Maschinen (VM) lassen sich einfach über die Befehlszeile oder über das Portal erstellen. In diesem Tutorial erfahren Sie, wie Sie mit virtuellen Computern im Rahmen der Microsoft Azure Platform optimale Ergebnisse erzielen. In diesem Thema wird eine Ubuntu Server-VM verwendet, aber Sie können virtuelle Linux-Computer auch mithilfe [Ihrer eigenen Images als Vorlagen](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)erstellen.  

## <a name="prerequisites"></a>Voraussetzungen
Dieses Thema setzt voraus, dass Sie bereits über ein funktionierendes Azure-Abonnement verfügen ([Anmeldung für eine kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)) und einen virtuellen Computer in Ihrem Azure-Abonnement bereitgestellt haben. Stellen Sie sicher, dass die neueste [Azure CLI](/cli/azure/install-az-cli2) installiert ist und dass Sie mit [az login](/cli/azure/reference-index) bei Ihrem Azure-Abonnement angemeldet sind, bevor Sie [einen virtuellen Computer erstellen](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="azure-os-disk"></a>Betriebssystem-Datenträger von Azure
Nach dem Erstellen eines virtuellen Linux-Computers in Azure sind diesem zwei Datenträger zugeordnet. **/dev/sda** ist der Betriebssystem-Datenträger, **/dev/sdb** ist der temporäre Datenträger.  Verwenden Sie den Betriebssystem-Hauptdatenträger ( **/dev/sda**) ausschließlich für das Betriebssystem. Er ist für den schnellen Start des virtuellen Computers optimiert und bietet keine gute Leistung für Ihre Workloads. Es empfiehlt sich, dem virtuellen Computer mindestens einen Datenträger anzufügen, um eine beständige und optimierte Datenspeicherung zu erhalten. 

## <a name="adding-disks-for-size-and-performance-targets"></a>Hinzufügen von Datenträgern für Größe und Leistung
Abhängig von der VM-Größe können Sie bis zu 16 zusätzliche Datenträger (A-Serie), 32 Datenträger (D-Serie) bzw. 64 Datenträger (G-Serie) anfügen, die jeweils eine Größe von bis zu 32 TB haben können. Orientieren Sie sich beim Hinzufügen zusätzlicher Datenträger an Ihren Platz- und IOPS-Anforderungen. Jeder Datenträger hat ein Leistungsziel von 500 IOPS (Storage Standard) bzw. von bis zu 20.000 IOPS (Storage Premium).

Um bei Storage Premium-Datenträgern mit der Cacheeinstellung **ReadOnly** oder **None** die höchstmögliche IOPS-Leistung zu erzielen, müssen beim Bereitstellen des Dateisystems in Linux so genannte **Barriers** deaktiviert werden. „Barriers“ werden nicht benötigt, da Schreibvorgänge auf Storage Premium-Datenträger bei diesen Cacheeinstellungen beständig sind.

* Wenn Sie **reiserFS** verwenden, deaktivieren Sie Barriers mithilfe der Bereitstellungsoption `barrier=none` (zum Aktivieren von Barriers verwenden Sie `barrier=flush`).
* Wenn Sie **ext3/ext4** verwenden, deaktivieren Sie Barriers mithilfe der Bereitstellungsoption `barrier=0` (zum Aktivieren von Barriers verwenden Sie `barrier=1`).
* Wenn Sie **XFS** verwenden, deaktivieren Sie Barriers mithilfe der Bereitstellungsoption `nobarrier` (zum Aktivieren von Barriers verwenden Sie `barrier`).

## <a name="unmanaged-storage-account-considerations"></a>Überlegungen zu nicht verwalteten Speicherkonten
Die Standardaktion beim Erstellen eines virtuellen Computers über die Azure CLI ist die Verwendung von Azure Managed Disks.  Diese Datenträger werden von der Azure-Plattform verarbeitet und erfordern keine Vorbereitung und keinen Speicherort zur Aufbewahrung.  Nicht verwaltete Datenträger erfordern ein Speicherkonto, und es sind einige Leistungsaspekte zu bedenken.  Weitere Informationen zu verwalteten Datenträgern finden Sie in der [Übersicht über Managed Disks](../managed-disks-overview.md).  Im folgenden Abschnitt werden einige Leistungsaspekte erläutert, die nur zu berücksichtigen sind, wenn Sie nicht verwaltete Datenträger verwenden.  Um es noch einmal zu betonen: Die standardmäßige und empfohlene Speicherlösung sind verwaltete Datenträger.

Wenn Sie einen virtuellen Computer mit nicht verwalteten Datenträgern erstellen, stellen Sie sicher, dass Sie Datenträger aus Speicherkonten anfügen, die sich in der gleichen Region befinden wie Ihr virtueller Computer, um für physische Nähe zu sorgen und die Netzwerklatenz zu minimieren.  Die Kapazität jedes Storage Standard-Kontos ist auf maximal 20.000 IOPS und eine Größe von 500 TB beschränkt.  Dieser Grenzwert entspricht etwa 40 stark ausgelasteten Datenträgern, einschließlich des Betriebssystem-Datenträgers und aller von Ihnen erstellten Datenträger. Bei Storage Premium-Konten gilt kein IOPS-Limit, die Größe ist jedoch auf 32 TB beschränkt. 

Wenn Sie sehr IOPS-intensive Workloads verarbeiten müssen und sich für Datenträger des Typs „Standardspeicher“ entschieden haben, müssen Sie die Datenträger ggf. auf mehrere Speicherkonten aufteilen, um zu verhindern, dass Sie das Limit von 20.000 IOPS erreichen. Der virtuelle Computer kann eine Mischung aus Datenträgern verschiedener Speicherkonten und Speicherkontotypen enthalten, um eine optimale Konfiguration zu erreichen.
 

## <a name="your-vm-temporary-drive"></a>Temporäres Laufwerk des virtuellen Computers
Beim Erstellen eines virtuellen Computers stellt Azure standardmäßig einen Betriebssystem-Datenträger ( **/dev/sda**) und einen temporären Datenträger ( **/dev/sdb**) bereit.  Alle weiteren Datenträger, die Sie hinzufügen, werden als **/dev/sdc**, **/dev/sdd**, **/dev/sde** usw. angezeigt. Die Daten auf dem temporären Datenträger ( **/dev/sdb**) sind nicht beständig und können in bestimmten Fällen verloren gehen – etwa, wenn Größenänderungen, erneute Bereitstellungen oder Wartungsarbeiten einen Neustart des virtuellen Computers erzwingen.  Größe und Typ des temporären Datenträgers hängen mit der Größe des virtuellen Computers zusammen, die Sie zum Zeitpunkt der Bereitstellung ausgewählt haben. Bei allen virtuellen Computern in Premium-Tarifen (Serien DS, G und DS_V2) wird das temporäre Laufwerk durch eine lokale SSD unterstützt, um eine höhere Leistung von bis zu 48.000 IOPS zu erzielen. 

## <a name="linux-swap-partition"></a>Linux-Swap-Partition
Wenn Ihre Azure-VM von einem Ubuntu- oder CoreOS-Image erstellt wurde, können Sie mit CustomData eine Cloud-Config-Datei an Cloud-Init senden. Wenn Sie [ein benutzerdefiniertes Linux-Image hochgeladen haben](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), das cloud-init verwendet, können Sie auch Swap-Partitionen konfigurieren, die cloud-init verwenden.

Die Datei **/etc/waagent.conf** kann nicht verwendet werden, um die Auslagerung für alle Images zu verwalten, die von cloud-init bereitgestellt und unterstützt werden. Die vollständige Liste der Images finden Sie unter [cloud-init-Unterstützung für virtuelle Computer in Azure](using-cloud-init.md). 

Die Auslagerung lässt sich für diese Images am einfachsten wie folgt verwalten:

1. Erstellen Sie im Ordner **/var/lib/cloud/scripts/per-boot** eine Datei namens **create_swapfile.sh**:

   **$ sudo touch /var/lib/cloud/scripts/per-boot/create_swapfile.sh**

1. Fügen Sie der Datei die folgenden Zeilen hinzu:

   **$ sudo vi /var/lib/cloud/scripts/per-boot/create_swapfile.sh**

   ```
   #!/bin/sh
   if [ ! -f '/mnt/swapfile' ]; then
   fallocate --length 2GiB /mnt/swapfile
   chmod 600 /mnt/swapfile
   mkswap /mnt/swapfile
   swapon /mnt/swapfile
   swapon -a ; fi
   ```

   > [!NOTE]
   > Sie können den Wert gemäß Ihren Anforderungen und unter Berücksichtigung des verfügbaren Speicherplatzes auf Ihrem Ressourcendatenträger ändern. Dieser unterscheidet sich je nach verwendeter VM-Größe.

1. Machen Sie die Datei zu einer ausführbaren Datei:

   **$ sudo chmod +x /var/lib/cloud/scripts/per-boot/create_swapfile.sh**

1. Führen Sie das Skript direkt nach dem letzten Schritt aus, um die Auslagerungsdatei zu erstellen:

   **$ sudo /var/lib/cloud/scripts/per-boot/./create_swapfile.sh**

Über den Azure Marketplace bereitgestellte VM-Images verfügen für Images ohne cloud-init-Unterstützung über einen in das Betriebssystem integrierten VM-Linux-Agent. Dieser Agent ermöglicht dem virtuellen Computer die Interaktion mit verschiedenen Azure-Diensten. Falls Sie ein Standardimage aus dem Azure Marketplace bereitgestellt haben, gehen Sie wie folgt vor, um die Einstellungen für die Linux-Auslagerungsdatei korrekt zu konfigurieren:

Ändern Sie in der Datei **/etc/waagent.conf** zwei Einträge. Diese steuern, ob eine dedizierte Auslagerungsdatei vorhanden ist und welche Größe sie besitzt. Sie müssen die Parameter `ResourceDisk.EnableSwap` und `ResourceDisk.SwapSizeMB` überprüfen. 

Stellen Sie für einen ordnungsgemäß aktivierten Datenträger und eine eingebundene Auslagerungsdatei sicher, dass für die Parameter die folgenden Einstellungen festgelegt sind:

* ResourceDisk.EnableSwap=Y
* ResourceDisk.SwapSizeMB={Größe in MB entsprechend Ihren Anforderungen} 

Nachdem Sie die Änderungen vorgenommen haben, müssen Sie den Agent (waagent) oder den virtuellen Linux-Computer neu starten, damit diese Änderungen übernommen werden.  Zeigen Sie mithilfe des Befehls `free` den freien Speicherplatz an, um sich zu vergewissern, dass die Änderungen umgesetzt wurden und eine Auslagerungsdatei erstellt wurde. Im folgenden Beispiel wurde infolge der Änderung der Datei **waagent.conf** eine Auslagerungsdatei mit einer Größe von 512 MB erstellt:

```bash
azuseruser@myVM:~$ free
            total       used       free     shared    buffers     cached
Mem:       3525156     804168    2720988        408       8428     633192
-/+ buffers/cache:     162548    3362608
Swap:       524284          0     524284
```

## <a name="io-scheduling-algorithm-for-premium-storage"></a>E/A-Scheduling-Algorithmus für Storage Premium
Mit Version 2.6.18 des Linux-Kernels wurde der standardmäßige E/A-Scheduling-Algorithmus von „Deadline“ in „CFQ“ (Completely Fair Queuing) geändert. Bei wahlfreien E/A-Zugriffsmustern sind die Leistungsunterschiede zwischen „CFQ“ und „Deadline“ vernachlässigbar.  Bei SSD-basierten Datenträgern, bei denen vorwiegend ein sequenzielles E/A-Muster verwendet wird, lässt sich durch einen Wechsel zum zuvor verwendeten NOOP- oder Deadline-Algorithmus unter Umständen eine bessere E/A-Leistung erzielen.

### <a name="view-the-current-io-scheduler"></a>Anzeigen des aktuellen E/A-Schedulers
Verwenden Sie den folgenden Befehl:  

```bash
cat /sys/block/sda/queue/scheduler
```

Sie sehen die folgende Ausgabe, die den aktuellen Scheduler angibt.  

```bash
noop [deadline] cfq
```

### <a name="change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Ändern des aktuellen Geräts (/dev/sda) des E/A-Scheduling-Algorithmus
Verwenden Sie die folgenden Befehle:  

```bash
azureuser@myVM:~$ sudo su -
root@myVM:~# echo "noop" >/sys/block/sda/queue/scheduler
root@myVM:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
root@myVM:~# update-grub
```

> [!NOTE]
> Die Anwendung dieser Einstellung nur für **/dev/sda** ist nicht sinnvoll. Legen Sie die Einstellung für alle Datenträger fest, bei denen vorwiegend ein sequenzielles E/A-Muster verwendet wird.  

Die folgende Ausgabe zeigt an, dass **grub.cfg** erfolgreich neu erstellt wurde und dass der Standardscheduler in NOOP geändert wurde.  

```bash
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-3.13.0-34-generic
Found initrd image: /boot/initrd.img-3.13.0-34-generic
Found linux image: /boot/vmlinuz-3.13.0-32-generic
Found initrd image: /boot/initrd.img-3.13.0-32-generic
Found memtest86+ image: /memtest86+.elf
Found memtest86+ image: /memtest86+.bin
done
```

Bei Red Hat-Distributionen benötigen Sie nur den folgenden Befehl:   

```bash
echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local
```

Ubuntu 18.04 mit dem für Azure optimierten Kernel verwendet E/A-Scheduler für mehrere Warteschlangen. In diesem Szenario ist `none` die geeignete Auswahl anstelle von `noop`. Weitere Informationen finden Sie unter [Ubuntu E/A-Scheduler](https://wiki.ubuntu.com/Kernel/Reference/IOSchedulers).

## <a name="using-software-raid-to-achieve-higher-iops"></a>Erzielen höherer IOPS-Werte mithilfe von Software-RAID
Wenn Ihre Workloads mehr IOPS erfordern als ein einzelner Datenträger bereitstellen kann, müssen Sie eine Software-RAID-Konfiguration mit mehreren Datenträgern verwenden. Da Azure bereits Datenträgerresilienz auf der lokalen Fabric-Ebene bietet, erreichen Sie die höchste Leistung mit einer RAID-0-Konfiguration.  Erstellen Sie Datenträger, stellen Sie diese in der Azure-Umgebung bereit, und fügen Sie sie an den virtuellen Linux-Computer an, bevor Sie die Laufwerke partitionieren, formatieren und bereitstellen.  Ausführlichere Informationen zum Konfigurieren einer Software-RAID-Einrichtung für den virtuellen Linux-Computer in Azure finden Sie unter **[Konfigurieren von Software-RAID unter Linux](/previous-versions/azure/virtual-machines/linux/configure-raid?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)** .

Als Alternative zu einer herkömmlichen RAID-Konfiguration können Sie auch Logical Volume Manager (LVM) installieren, um mehrere physische Datenträger in einem einzelnen logischen Stripesetvolume zu konfigurieren. In dieser Konfiguration werden Lese- und Schreibvorgänge auf mehrere Datenträger in der Volumegruppe verteilt (ähnlich wie bei RAID 0). Aus Leistungsgründen ist die Wahrscheinlichkeit hoch, dass die logischen Volumes als Stripesetvolumes eingerichtet werden sollen, damit für Lese- und Schreibvorgänge alle angeschlossenen Datenträger genutzt werden.  Weitere Informationen zum Konfigurieren eines logischen Stripesetvolumes für den virtuellen Linux-Computer in Azure finden Sie im Dokument **[Konfigurieren von LVM auf einem virtuellen Linux-Computer in Azure](/previous-versions/azure/virtual-machines/linux/configure-lvm?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)** .

## <a name="next-steps"></a>Nächste Schritte
Denken Sie daran: Wie bei allen Optimierungen müssen auch hier vor und nach jeder Änderung Tests ausgeführt werden, um die Auswirkungen der jeweiligen Änderung zu ermitteln.  Die Optimierung ist ein schrittweise ausgeführter Prozess, der auf unterschiedlichen Computern in Ihrer Umgebung unterschiedliche Ergebnisse liefert.  Was bei einer Konfiguration funktioniert, ist für andere möglicherweise ungeeignet.

Einige nützliche Links zu weiteren Ressourcen:

* [Benutzerhandbuch für Azure Linux-Agent](../extensions/agent-linux.md)
* [Konfigurieren von Software-RAID unter Linux](/previous-versions/azure/virtual-machines/linux/configure-raid)