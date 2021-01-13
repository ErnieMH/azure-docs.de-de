---
title: Optimieren des VM-Netzwerkdurchsatzes | Microsoft-Dokumentation
description: Informationen zur Optimierung des Netzwerkdurchsatzes für Microsoft Azure-VMs unter Windows und Linux, einschließlich der wichtigsten Distributionen wie etwa Ubuntu, CentOS und Red Hat.
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/06/2020
ms.author: steveesp
ms.openlocfilehash: a9db2bcc0b44dfb6146517de8a139f34cd8584af
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97654454"
---
# <a name="optimize-network-throughput-for-azure-virtual-machines"></a>Optimieren des Netzwerkdurchsatzes für virtuelle Azure-Computer

Virtuelle Azure-Computer weisen standardmäßige Netzwerkeinstellungen auf, die zur Steigerung des Netzwerkdurchsatzes weiter optimiert werden können. Dieser Artikel beschreibt die Optimierung des Netzwerkdurchsatzes für virtuelle Microsoft Azure-Computer unter Windows und Linux, einschließlich der wichtigsten Distributionen wie etwa Ubuntu, CentOS und Red Hat.

## <a name="windows-vm"></a>Windows-VM

Wenn Ihr virtueller Windows-Computer den [beschleunigten Netzwerkbetrieb](create-vm-accelerated-networking-powershell.md) unterstützt, ist die Aktivierung dieser Funktion im Hinblick auf den Durchsatz die optimale Konfiguration. Alle anderen virtuellen Windows-Computer, die die empfangsseitige Skalierung (Receive Side Scaling; RSS) verwenden, können einen höheren maximalen Durchsatz als ein virtueller Computer ohne RSS erreichen. RSS kann auf virtuellen Windows-Computern standardmäßig deaktiviert sein. Führen Sie die folgenden Schritte aus, um zu ermitteln, ob RSS aktiviert ist und um die Funktion ggf. zu aktivieren:

1. Geben Sie den PowerShell-Befehl `Get-NetAdapterRss` ein, um zu ermitteln, ob RSS für einen Netzwerkadapter aktiviert ist. In der folgenden Beispielausgabe aus `Get-NetAdapterRss` ist RSS nicht aktiviert.

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled                 : False
    ```
2. Geben Sie den folgenden Befehl ein, um RSS zu aktivieren:

    ```powershell
    Get-NetAdapter | % {Enable-NetAdapterRss -Name $_.Name}
    ```
    Der vorherige Befehl weist keine Ausgabe auf. Mit dem Befehl wurden die NIC-Einstellungen geändert und dadurch etwa eine Minute lang vorübergehende Konnektivitätsverluste verursacht. Während des Konnektivitätsverlusts wird das Dialogfeld „Verbindung wird wiederhergestellt“ angezeigt. In der Regel ist die Konnektivität nach dem dritten Versuch wiederhergestellt.
3. Bestätigen Sie, dass RSS auf dem virtuellen Computer aktiviert ist, indem Sie den Befehl `Get-NetAdapterRss` erneut eingeben. Bei Erfolg wird die folgende Beispielausgabe zurückgegeben:

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled                  : True
    ```

## <a name="linux-vm"></a>Linux-VM

RSS ist auf einem virtuellen Azure Linux-Computer immer standardmäßig aktiviert. Linux-Kernel, die seit Oktober 2017 veröffentlicht wurden, enthalten neue Netzwerkoptimierungsoptionen, die einem virtuellen Linux-Computer einen höheren Netzwerkdurchsatz ermöglichen.

### <a name="ubuntu-for-new-deployments"></a>Ubuntu für neue Bereitstellungen

Der Ubuntu Azure-Kernel ist für Netzwerkleistung in Azure am besten optimiert. Um die neuesten Optimierungen zu erhalten, installieren Sie zuerst die neueste unterstützte Version von 18.04-LTS wie folgt:

```json
"Publisher": "Canonical",
"Offer": "UbuntuServer",
"Sku": "18.04-LTS",
"Version": "latest"
```

Nach der Erstellung geben Sie die folgenden Befehle zum Abrufen des neuesten Updates ein: Diese Schritte können auch für virtuelle Computer verwendet werden, auf denen derzeit der Ubuntu-Azure-Kernel ausgeführt wird.

```bash
#run as root or preface with sudo
apt-get -y update
apt-get -y upgrade
apt-get -y dist-upgrade
```

Der folgende optionale Befehlssatz kann bei vorhandenen Ubuntu-Bereitstellungen hilfreich sein, die den Azure-Kernel bereits enthalten, aber aufgrund von Fehlern nicht weiter aktualisiert werden konnten.

```bash
#optional steps may be helpful in existing deployments with the Azure kernel
#run as root or preface with sudo
apt-get -f install
apt-get --fix-missing install
apt-get clean
apt-get -y update
apt-get -y upgrade
apt-get -y dist-upgrade
```

#### <a name="ubuntu-azure-kernel-upgrade-for-existing-vms"></a>Ubuntu-Azure-Kernelupgrade für vorhandene virtuelle Computer

Signifikante Durchsatzleistung kann durch ein Upgrade auf den Linux-Kernel von Azure erreicht werden. Um zu erfahren, ob Sie über diesen Kernel verfügen, überprüfen Sie die Kernelversion. Sie sollte gleich oder höher als das Beispiel sein.

```bash
#Azure kernel name ends with "-azure"
uname -r

#sample output on Azure kernel:
#4.13.0-1007-azure
```

Wenn Ihr virtueller Computer den Azure-Kernel nicht umfasst, beginnt die Versionsnummer in der Regel mit „4.4“. Wenn Ihr VM den Azure-Kernel nicht enthält, führen Sie die folgenden Befehle als Root-Benutzer aus:

```bash
#run as root or preface with sudo
apt-get update
apt-get upgrade -y
apt-get dist-upgrade -y
apt-get install "linux-azure"
reboot
```

### <a name="centos"></a>CentOS

Um die neuesten Optimierungen zu erhalten, empfiehlt es sich, einen virtuellen Computer mit der letzten unterstützten Version zu erstellen. Geben Sie dazu die folgenden Parameter an:

```json
"Publisher": "OpenLogic",
"Offer": "CentOS",
"Sku": "7.7",
"Version": "latest"
```

Bei neuen und vorhandenen virtuellen Computer kann die Installation der aktuellen Version von Linux Integration Services (LIS) vorteilhaft sein. Die Optimierung des Durchsatzes ist in LIS enthalten, beginnend mit Version 4.2.2-2. Spätere Verbesserungen können außerdem weitere Verbesserungen enthalten. Geben Sie die folgenden Befehle zum Installieren der neuesten Version von LIS ein:

```bash
sudo yum update
sudo reboot
sudo yum install microsoft-hyper-v
```

### <a name="red-hat"></a>Red Hat

Um die Optimierungen zu erhalten, empfiehlt es sich, einen virtuellen Computer mit der letzten unterstützten Version zu erstellen. Geben Sie dazu die folgenden Parameter an:

```json
"Publisher": "RedHat"
"Offer": "RHEL"
"Sku": "7-RAW"
"Version": "latest"
```

Bei neuen und vorhandenen virtuellen Computer kann die Installation der aktuellen Version von Linux Integration Services (LIS) vorteilhaft sein. Die Optimierung des Durchsatzes erfolgt in LIS, beginnend mit 4.2. Geben Sie die folgenden Befehle ein, um LIS herunterzuladen und zu installieren:

```bash
wget https://aka.ms/lis
tar xvf lis
cd LISISO
sudo ./install.sh #or upgrade.sh if prior LIS was previously installed
```

Weitere Informationen zu Linux Integration Services Version 4.2 für Hyper-V erhalten Sie auf der [Downloadseite](https://www.microsoft.com/download/details.aspx?id=55106).

## <a name="next-steps"></a>Nächste Schritte
* Bereitstellen von VMs in der Nähe zueinander für geringe Latenz mit einer [Näherungsplatzierungsgruppe](../virtual-machines/windows/co-location.md)
* Sehen Sie sich das optimierte Ergebnis an, indem Sie [die Bandbreite bzw. den Durchsatz der Azure-VM](virtual-network-bandwidth-testing.md) für Ihr Szenario testen.
* Lesen Sie mehr über die [Zuweisung von Bandbreite zu virtuellen Computern](virtual-machine-network-throughput.md).
* Weitere Informationen finden Sie unter [Azure Virtual Network – häufig gestellte Fragen (FAQs)](virtual-networks-faq.md).
