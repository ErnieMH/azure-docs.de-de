---
title: Verfügbarkeitsoptionen
description: Erfahren Sie mehr über die Verfügbarkeitsfunktionen für die Ausführung virtueller Computer in Azure.
author: cynthn
ms.author: cynthn
ms.service: virtual-machines
ms.topic: conceptual
ms.date: 02/18/2021
ms.openlocfilehash: 6601cd31cf6e332bf292cd887b498a92946d8af7
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101668298"
---
# <a name="availability-options-for-virtual-machines-in-azure"></a>Verfügbarkeitsoptionen für virtuelle Computer in Azure

Dieser Artikel enthält eine Übersicht über die Verfügbarkeitsfunktionen von virtuellen Azure-Computern (VMs).

## <a name="high-availability"></a>Hochverfügbarkeit

Workloads werden in der Regel auf verschiedene VMs verteilt, um einen hohen Durchsatz, Leistung und Redundanz zu erzielen, falls eine VM aufgrund eines Updates oder eines anderen Ereignisses beeinträchtigt wird. 

Azure bietet verschiedene Optionen für Hochverfügbarkeit. Zunächst sprechen wir über grundlegende Konstrukte. 

### <a name="availability-zones"></a>Verfügbarkeitszonen

Mit [Verfügbarkeitszonen](../availability-zones/az-overview.md) erhalten Sie mehr Kontrolle beim Aufrechterhalten der Verfügbarkeit von Anwendungen und Daten auf Ihren virtuellen Computern. Eine Verfügbarkeitszone ist eine physisch getrennte Zone in einer Azure-Region. Pro unterstützter Azure-Region sind drei Verfügbarkeitszonen vorhanden. 

Jede Verfügbarkeitszone verfügt über eine eigene Stromquelle, ein Netzwerk und eine Kühlung. Indem Sie Ihre Lösungen für die Verwendung von replizierten VMs in Zonen gestalten, können Sie Ihre Apps und Daten vor dem Verlust eines Datencenters schützen. Wenn eine Zone kompromittiert ist, sind replizierte Apps und Daten sofort in einer anderen Zone verfügbar. 

![Verfügbarkeitszonen](./media/virtual-machines-common-regions-and-availability/three-zones-per-region.png)

Informieren Sie sich über das Bereitstellen einer [Windows](./windows/create-powershell-availability-zone.md)- oder [Linux](./linux/create-cli-availability-zone.md)-VM in einer Verfügbarkeitszone.


### <a name="fault-domains"></a>Fehlerdomänen

Eine Fehlerdomäne ist eine logische Gruppe von zugrunde liegender Hardware mit einer gemeinsamen Stromquelle und einem Netzwerkswitch, ähnlich einem Rack in einem lokalen Rechenzentrum. 

### <a name="update-domains"></a>Updatedomänen

Eine Updatedomäne ist eine logische Gruppe von zugrunde liegender Hardware, die zur gleichen Zeit gewartet oder neu gestartet werden kann. 

Mit diesem Ansatz wird sichergestellt, dass mindestens eine Instanz Ihrer Anwendung immer ausgeführt wird, wenn für die Azure-Plattform die regelmäßige Wartung durchgeführt wird. Während einer Wartung werden die Updatedomänen unter Umständen nicht der Reihe nach neu gestartet, sondern es wird jeweils nur eine Updatedomäne neu gestartet.


## <a name="virtual-machines-scale-sets"></a>Skalierungsgruppen für virtuelle Computer 

Mit Azure-VM-Skalierungsgruppen können Sie eine Gruppe VMs mit Lastenausgleich erstellen und verwalten. Die Anzahl von VM-Instanzen kann automatisch erhöht oder verringert werden, wenn sich der Bedarf ändert, oder es kann ein Zeitplan festgelegt werden. Skalierungsgruppen ermöglichen Hochverfügbarkeit für Ihre Anwendungen und das zentrale Verwalten, Konfigurieren und Aktualisieren zahlreicher VMs. Es empfiehlt sich, mindestens zwei VMs in einer Skalierungsgruppe zu erstellen, um eine Anwendung mit hoher Verfügbarkeit zu erhalten und die [Azure-SLA von 99,95 Prozent](https://azure.microsoft.com/support/legal/sla/virtual-machines/) zu erfüllen. Für die Skalierungssgruppe selbst entstehen keine Kosten. Sie bezahlen nur für die einzelnen VM-Instanzen, die Sie erstellen. Wenn ein einzelner virtuelle Computer [Azure Premium SSD](./disks-types.md#premium-ssd) verwendet, gilt die Azure-SLA für ungeplante Wartungsereignisse. VMs in einer Skalierungsgruppe können über mehrere Updatedomänen und Fehlerdomänen hinweg bereitgestellt werden, um die Verfügbarkeit und Ausfallsicherheit für den Fall von Rechenzentrumsausfällen sowie geplanten oder ungeplanten Wartungsereignissen zu maximieren. VMs in einer Skalierungsgruppe können auch in einer einzelnen Verfügbarkeitszone oder regional bereitgestellt werden. Die Bereitstellungsoptionen für Verfügbarkeitszonen können je nach Orchestrierungsmodus variieren.

**Fehler- und Updatedomänen**

VM-Skalierungsgruppen vereinfachen durch das Ausrichten von Fehlerdomänen und Updatedomänen das Entwerfen für Hochverfügbarkeit. Sie müssen nur die Anzahl der Fehlerdomänen für die Skalierungsgruppe definieren. Die Anzahl der für die Skalierungsgruppe verfügbaren Fehlerdomänen kann je nach Region variieren. Weitere Informationen finden Sie unter [Verwalten der Verfügbarkeit virtueller Windows-Computer in Azure](./manage-availability.md).

**Orchestrierungsmodi für Skalierungsgruppen**

Die Orchestrierungsmodi für VM-Skalierungsgruppen ermöglichen Ihnen eine bessere Kontrolle darüber, wie die Instanzen virtueller Computer von der Skalierungsgruppe verwaltet werden. Sie können einen einheitlichen oder flexiblen Orchestrierungsmodus für Ihre Skalierungsgruppe aktivieren. Eine einheitliche Orchestrierung ist für große zustandslose Workloads mit identischen Instanzen optimiert. Die flexible Orchestrierung (Vorschau) ist für Hochverfügbarkeit im großen Stil mit identischen oder mehreren virtuellen Computern vorgesehen. Erfahren Sie mehr über diese [Orchestrierungsmodi](./virtual-machine-scale-sets/virtual-machine-scale-sets-orchestration-modes.md) und wie Sie sie aktivieren können.


## <a name="availability-sets"></a>Verfügbarkeitsgruppen
Eine Verfügbarkeitsgruppe ist eine logische Gruppierung von virtuellen Computern, die es Azure ermöglicht zu verstehen, wie Ihre Anwendung erstellt ist, um Redundanz und Verfügbarkeit zu bieten. Es empfiehlt sich, mindestens zwei virtuelle Computer in einer Verfügbarkeitsgruppe zu erstellen, um eine Anwendung mit hoher Verfügbarkeit zu erhalten und die [Azure-SLA von 99,95 Prozent](https://azure.microsoft.com/support/legal/sla/virtual-machines/) zu erfüllen. Für die Verfügbarkeitsgruppe selbst entstehen keine Kosten. Sie bezahlen nur für die einzelnen VM-Instanzen, die Sie erstellen. Wenn ein einzelner virtuelle Computer [Azure Premium SSD](./disks-types.md#premium-ssd) verwendet, gilt die Azure-SLA für ungeplante Wartungsereignisse.

In einer Verfügbarkeitsgruppe werden VMs automatisch auf diese Fehlerdomänen verteilt. Bei diesem Ansatz werden die Auswirkungen von potenziellen Hardwareausfällen, Netzwerkausfällen oder Stromausfällen beschränkt.

Virtuelle Computer mit [Azure Managed Disks](./faq-for-disks.md) werden bei Verwendung einer verwalteten Verfügbarkeitsgruppe auf Fehlerdomänen für verwaltete Datenträger ausgerichtet. Diese Ausrichtung stellt sicher, dass sich alle verwalteten, an einen virtuellen Computer angefügten Datenträger innerhalb der gleichen Fehlerdomäne für verwaltete Datenträger befinden. 

Nur virtuelle Computer mit verwalteten Datenträgern können in einer verwalteten Verfügbarkeitsgruppe erstellt werden. Die Anzahl der Fehlerdomänen für verwaltete Datenträger variiert je nach Region – pro Region sind entweder zwei oder drei Fehlerdomänen für verwaltete Datenträger vorhanden. Sie können sich weiter über diese Fehlerdomänen für verwaltete Datenträger für [Linux-VMs](./manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) oder [Windows-VMs](./manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) informieren.

![Verwaltete Verfügbarkeitsgruppe](./media/virtual-machines-common-manage-availability/md-fd-updated.png)


VMs innerhalb einer Verfügbarkeitsgruppe werden automatisch auf die Updatedomänen verteilt. 

![Verfügbarkeitsgruppen](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

## <a name="next-steps"></a>Nächste Schritte
Sie können diese Verfügbarkeits- und Redundanzfunktionen jetzt nutzen, um Ihre Azure-Umgebung zu erstellen. Weitere Informationen zu bewährten Methoden finden Sie unter [Checkliste für die Verfügbarkeit](/azure/architecture/checklist/resiliency-per-service).
