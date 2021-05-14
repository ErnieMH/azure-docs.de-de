---
title: Häufig gestellte Fragen zum Azure Confidential Computing
description: Hier finden Sie Antworten auf häufig gestellte Fragen zum Azure Confidential Computing.
author: JBCook
ms.topic: troubleshooting
ms.workload: infrastructure
ms.service: virtual-machines
ms.subservice: confidential-computing
ms.date: 4/17/2020
ms.author: jencook
ms.openlocfilehash: aa78d6495eeffd0ea733451e029f07413602ce0d
ms.sourcegitcommit: 260a2541e5e0e7327a445e1ee1be3ad20122b37e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2021
ms.locfileid: "107812730"
---
# <a name="frequently-asked-questions-for-azure-confidential-computing"></a>Häufig gestellte Fragen zum Azure Confidential Computing

Dieser Artikel bietet Antworten auf einige der häufigsten Fragen zur Ausführung von [Confidential Computing-Workloads auf virtuellen Azure-Computern](overview.md).

Suchen Sie in den Azure-Foren bei [MSDN und Stack Overflow](https://azure.microsoft.com/support/forums/), falls Sie Ihr Azure-Problem mit diesem Artikel nicht beheben konnten. Sie können Ihr Problem in diesen Foren veröffentlichen oder [auf Twitter an @AzureSupport senden](https://twitter.com/AzureSupport). Sie können auch eine Azure-Supportanfrage senden. Um eine Supportanfrage zu senden, wählen Sie auf der [Azure-Supportseite](https://azure.microsoft.com/support/options/) die Option „Support erhalten“ aus.

## <a name="confidential-computing-virtual-machines"></a>Virtuelle Computer für Confidential Computing <a id="vm-faq"></a>

**Wie kann ich VMs der DCsv2-Reihe in Azure bereitstellen?**

Im Folgenden finden Sie einige Möglichkeiten zum Bereitstellen einer DCsv2-VM:
   - Verwenden einer [Azure Resource Manager-Vorlage](../virtual-machines/windows/template-description.md)
   - Im [Azure-Portal](https://portal.azure.com/#create/hub)
   - In der Marketplace-Lösungsvorlage [Azure Confidential Computing (Virtual Machine)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-compute.acc-virtual-machine-v2?tab=overview). Mithilfe der Marketplace-Lösungsvorlage können Kunden auf unterstützte Szenarien (Regionen, Images, Verfügbarkeit, Datenträgerverschlüsselung) beschränkt werden. 

**Funktionieren alle Betriebssystemimages mit Azure Confidential Computing?**

Nein. Die VM können nur auf Betriebssystemen der Generation 2 unter Ubuntu Server 18.04, Ubuntu Server 20.04, Windows Server 2019 Datacenter und Windows Server 2016 Datacenter bereitgestellt werden. Weitere Informationen zu virtuellen Computern der Generation 2 finden Sie unter [Linux-](../virtual-machines/generation-2.md) und [Windows](../virtual-machines/generation-2.md).

**DCsv2-VMs sind im Portal abgeblendet, und ich kann keine auswählen**

Basierend auf der Informationssprechblase neben der VM können verschiedene Maßnahmen ergriffen werden:
   -    **UnsupportedGeneration**: Ändern Sie die Generation des virtuellen Computerimages in „Gen2“.
   -    **NotAvailableForSubscription**: Die Region ist für Ihr Abonnement noch nicht verfügbar. Wählen Sie eine verfügbare Region aus.
   -    **InsufficientQuota**: [Erstellen Sie eine Supportanfrage zur Erhöhung Ihres Kontingents](../azure-portal/supportability/per-vm-quota-requests.md). Kostenlose Testabonnements verfügen über kein Kontingent für Confidential Computing-VMs. 

**DCsv2-VMs werden nicht angezeigt, wenn ich nach ihnen in der Größenauswahl des Portals suche**

Stellen Sie sicher, dass Sie eine [verfügbare Region](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines) ausgewählt haben. Stellen Sie außerdem sicher, dass Sie in der Größenauswahl „Alle Filter löschen“ ausgewählt haben. 

**Kann ich den beschleunigten Netzwerkbetrieb mit Azure Confidential Computing aktivieren?**

 Nein. Beschleunigter Netzwerkbetrieb wird auf virtuellen Computern der DC-Serie oder der DCsv2-Serie nicht unterstützt. Der beschleunigte Netzwerkbetrieb kann nicht für die Bereitstellung von virtuellen Computern mit Confidential Computing oder für die Bereitstellung von Azure Kubernetes Service-Clustern für Confidential Computing aktiviert werden.

**Kann ich Azure Dedicated Host mit diesen Computern verwenden?**

Ja. Azure Dedicated Host unterstützt virtuelle Computer der DCsv2-Serie. Azure Dedicated Host bietet einen physischen Server mit einem einzelnen Mandanten, auf dem Ihre virtuellen Computer ausgeführt werden. Benutzer verwenden Azure Dedicated Host in der Regel, um Complianceanforderungen in Bezug auf physische Sicherheit, Datenintegrität und Überwachung zu erfüllen. 

**Ich erhalte eine Fehlermeldung bezüglich der Azure Resource Manager-Vorlagenbereitstellung: „Der Vorgang konnte nicht abgeschlossen werden, weil er zu einer Überschreitung des genehmigten Kontingents für Kerne der Standard DCsv2-Familie führt.“**

[Erstellen Sie eine Supportanfrage zur Erhöhung Ihres Kontingents](../azure-portal/supportability/per-vm-quota-requests.md). Kostenlose Testabonnements verfügen über kein Kontingent für Confidential Computing-VMs. 

**Worin besteht der Unterschied zwischen VMs der DCsv2-Serie und der DC-Serie?**

Virtuelle Computer der DC-Serie werden mit älteren 6-Kern-Intel-Prozessoren mit Intel SGX ausgeführt und verfügen über weniger Gesamtarbeitsspeicher und weniger EPC-Arbeitsspeicher (Enclave Page Cache). Sie sind nur in zwei Regionen verfügbar (in „USA, Osten“ und „Europa, Westen“ in den Größen „Standard_DC2s“ und „Standard_DC4s“). Es ist nicht geplant, diese VMs allgemein verfügbar zu machen, und sie werden nicht für die Verwendung in der Produktion empfohlen. Um diese VMs verfügbar zu machen, verwenden Sie die Marketplaceinstanz der [Confidential Compute-VM der DC-Serie [Vorschau]](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-compute.confidentialcompute?tab=Overview).

**Sind DCsv2-VMs global verfügbar?**

Nein. Diese virtuellen Computer sind zurzeit nur in ausgewählten Regionen verfügbar. Überprüfen Sie die Seite [Produkte nach Regionen](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines) auf die neuesten verfügbaren Regionen. 

**Ist Hyperthreading auf diesen Computern deaktiviert?**

Hyperthreading ist in allen Azure-Clustern für die Verarbeitung vertraulicher Informationen deaktiviert.

**Wie installiere ich das Open Enclave SDK auf den DCsv2-VMs?**
   
Anweisungen zum Installieren des OE SDK auf einem Computer in Azure oder einem lokalen Computer finden Sie in den Anleitungen im [Open Enclave SDK GitHub](https://github.com/openenclave/openenclave).
     
Sie können auch die betriebssystemspezifischen Installationsanweisungen im Open Enclave SDK GitHub konsultieren:
   - [Installieren des OE SDK unter Windows](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/install_oe_sdk-Windows.md)
   - [Installieren des OE SDK unter Ubuntu 18.04](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/install_oe_sdk-Ubuntu_18.04.md)