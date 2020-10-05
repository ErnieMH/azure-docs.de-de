---
title: include file
description: include file
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/30/2019
ms.author: zivr
ms.custom: include file
ms.openlocfilehash: c7e3c9292b53aeb073e11a5293459e39a22ca81d
ms.sourcegitcommit: c52e50ea04dfb8d4da0e18735477b80cafccc2cf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89570159"
---
Wenn Sie VMs in einer einzelnen Region anordnen, verringert sich der physische Abstand zwischen den Instanzen. Wenn Sie sie in einer einzelnen Verfügbarkeitszone anordnen, verringert sich ebenfalls ihr physischer Abstand. Wenn jedoch Ihre Ressourcen in Azure zunehmen, kann eine einzige Verfügbarkeitszone auch mehrere physische Rechenzentren umspannen. Dies kann zu Netzwerklatenzen führen, die sich auf die Anwendung auswirken. 

Um den Abstand zwischen den VMs so stark wie möglich zu verringern und somit die geringstmögliche Latenz zu erzielen, sollten Sie sie in einer Näherungsplatzierungsgruppe bereitstellen.

Eine Näherungsplatzierungsgruppe ist eine logische Gruppierung, mit der ein möglichst geringer Abstand zwischen Azure-Compute-Ressourcen sichergestellt wird. Näherungsplatzierungsgruppen sind für Workloads hilfreich, die eine geringe Latenz erfordern.


- Geringe Latenz zwischen eigenständigen VMs.
- Geringe Latenz zwischen VMs in einer einzelnen Verfügbarkeitsgruppe oder VM-Skalierungsgruppe. 
- Geringe Latenz zwischen eigenständigen VMs, VMs in mehreren Verfügbarkeitsgruppen oder mehreren Skalierungsgruppen. Sie können mehrere Computeressourcen in einer einzelnen Platzierungsgruppe zusammenfassen, um eine Multi-Tier-Anwendung zusammenzuführen. 
- Geringe Latenz zwischen mehreren Logikschichten, die unterschiedliche Hardwaretypen verwenden. Beispiel: Sie führen in einer einzelnen Näherungsplatzierungsgruppe das Back-End mithilfe der M-Serie in einer Verfügbarkeitsgruppe und das Front-End in einer Instanz der D-Serie in einer Skalierungsgruppe aus.


![Grafik für Näherungsplatzierungsgruppen](./media/virtual-machines-common-ppg/ppg.png)

## <a name="using-proximity-placement-groups"></a>Verwenden von Näherungsplatzierungsgruppen 

Eine Näherungsplatzierungsgruppe ist ein neuer Ressourcentyp in Azure. Sie müssen eine erstellen, bevor Sie sie mit anderen Ressourcen verwenden können. Einmal erstellt, kann sie mit virtuellen Computern, Verfügbarkeitsgruppen oder VM-Skalierungsgruppen verwendet werden. Sie geben bei der Erstellung von Computeressourcen, die die Näherungsplatzierungsgruppe-ID bereitstellen, eine Näherungsplatzierungsgruppe an. 

Sie können auch eine vorhandene Ressource in eine Näherungsplatzierungsgruppe verschieben. Wenn Sie eine Ressource in eine Näherungsplatzierungsgruppe verschieben, sollten Sie die Ressource zuerst beenden (freigeben), da sie möglicherweise in einem anderen Rechenzentrum in der Region neu bereitgestellt wird, um der Zusammenstellungseinschränkung zu entsprechen. 

Im Falle von Verfügbarkeitsgruppen und VM-Skalierungsgruppen sollten Sie die Näherungsplatzierungsgruppe auf Ressourcenebene und nicht auf der Ebene der einzelnen virtuellen Computer festlegen. 

Eine Näherungsplatzierungsgruppe ist eher eine Zusammenstellungseinschränkung als ein Anheftmechanismus. Sie wird an ein bestimmtes Rechenzentrum angeheftet, wobei die erste von ihr verwendete Ressource bereitgestellt wird. Sobald alle Ressourcen, die die Näherungsplatzierungsgruppe verwenden, beendet (Zuordnung aufgehoben) oder gelöscht wurden, wird sie nicht mehr angeheftet. Daher ist es bei der Verwendung einer Näherungsplatzierungsgruppe mit mehreren VM-Serien wichtig, möglichst alle erforderlichen Typen im Voraus in einer Vorlage anzugeben oder einer Bereitstellungssequenz zu folgen, die Ihre Chancen auf eine erfolgreiche Bereitstellung verbessert. Wenn bei der Bereitstellung ein Fehler auftritt, starten Sie die Bereitstellung mit der VM-Größe neu, bei der die erste Bereitstellung fehlgeschlagen ist.

## <a name="what-to-expect-when-using-proximity-placement-groups"></a>Erwartung bei der Verwendung von Näherungsplatzierungsgruppen 
Näherungsplatzierungsgruppen bieten eine Zusammenstellung in demselben Rechenzentrum. Da jedoch Näherungsplatzierungsgruppen eine zusätzliche Bereitstellungseinschränkung darstellen, kann es zu Zuordnungsfehlern kommen. Es gibt nur wenige Anwendungsfälle, in denen Sie bei der Verwendung von Näherungsplatzierungsgruppen Zuordnungsfehler feststellen können:

- Wenn Sie nach dem ersten virtuellen Computer in der Näherungsplatzierungsgruppe fragen, wird das Rechenzentrum automatisch ausgewählt. In einigen Fällen kann eine zweite Anforderung der SKU für einen anderen virtuellen Computer fehlschlagen, wenn sie in diesem Rechenzentrum nicht vorhanden ist. In diesem Fall wird der Fehler **OverconstrainedAllocationRequest** zurückgegeben. Um dies zu vermeiden, versuchen Sie, die Reihenfolge zu ändern, in der Sie Ihre SKUs bereitstellen, oder lassen Sie beide Ressourcen mit einer einzelnen ARM-Vorlage bereitstellen.
-   Im Fall von flexiblen Workloads, bei denen Sie VM-Instanzen hinzufügen und entfernen, kann das Vorhandensein einer Einschränkung für Näherungsplatzierungsgruppen für Ihre Bereitstellung dazu führen, dass die Anforderung nicht erfüllt wird, was zu einem **AllocationFailure**-Fehler führt. 
- Das bedarfsgesteuerte Beenden (Freigeben) und Starten Ihrer virtuellen Computer ist eine weitere Möglichkeit, Flexibilität zu erreichen. Da die Kapazität nicht erhalten bleibt, wenn Sie einen virtuellen Computer beenden (freigeben), kann ein erneuter Start zu einem **AllocationFailure**-Fehler führen.


## <a name="best-practices"></a>Bewährte Methoden 
- Verwenden Sie für die niedrigste Latenz Näherungsplatzierungsgruppen zusammen mit beschleunigtem Netzwerkbetrieb. Weitere Informationen finden Sie unter [Erstellen eines virtuellen Linux-Computers mit beschleunigtem Netzwerkbetrieb](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oder [Erstellen eines virtuellen Windows-Computers mit beschleunigtem Netzwerkbetrieb](/azure/virtual-network/create-vm-accelerated-networking-powershell?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
- Stellen Sie alle VM-Größen in einer einzelnen Vorlage bereit. Um die Verwendung von Hardware zu vermeiden, die nicht alle erforderlichen VM-SKUs und -Größen unterstützt, schließen Sie alle Logikschichten in eine einzelne Vorlage ein, damit diese alle gleichzeitig bereitgestellt werden.
- Wenn Sie mit PowerShell, der CLI oder dem SDK ein Bereitstellungsskript erstellen, erhalten Sie möglicherweise den Zuordnungsfehler `OverconstrainedAllocationRequest`. In diesem Fall sollten Sie alle vorhandenen VMs beenden bzw. ihre Zuordnung aufheben und die Sequenz im Bereitstellungsskript so ändern, dass es mit den fehlgeschlagenen VM-SKUs/-Größen beginnt. 
- Wenn Sie eine vorhandene Platzierungsgruppe wiederverwenden, aus der VMs gelöscht wurden, warten Sie, bis der Löschvorgang vollständig durchgeführt wurde, bevor Sie der Gruppe VMs hinzufügen.
- Wenn Latenz oberste Priorität hat, fügen Sie die VMs in eine Näherungsplatzierungsgruppe und die gesamte Lösung in eine Verfügbarkeitszone ein. Wenn jedoch Resilienz oberste Priorität hat, verteilen Sie die Instanzen auf mehrere Verfügbarkeitszonen (eine einzelne Näherungsplatzierungsgruppe kann nicht mehrere Zonen umfassen).
