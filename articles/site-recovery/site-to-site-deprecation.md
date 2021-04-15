---
title: Einstellen der Unterstützung für die Notfallwiederherstellung zwischen von Kunden verwalteten Standorten (mit VMM) mit Azure Site Recovery | Microsoft-Dokumentation
description: Details zur bevorstehenden Einstellung der Notfallwiederherstellung zwischen Kundenstandorten, die Hyper-V verwenden, und zwischen Standorten, die von SCVMM in Azure verwaltet werden, sowie alternative Optionen
services: site-recovery
author: Sharmistha-Rai
manager: gaggupta
ms.service: site-recovery
ms.topic: article
ms.date: 02/25/2020
ms.author: sharrai
ms.openlocfilehash: 9ffe7a3158b1de6828350947dcf81ef41d08708d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "87421840"
---
# <a name="deprecation-of-disaster-recovery-between-customer-managed-sites-with-vmm-using-azure-site-recovery"></a>Einstellen der Unterstützung für die Notfallwiederherstellung zwischen von Kunden verwalteten Standorten (mit VMM) mit Azure Site Recovery

In diesem Artikel werden der Plan zur bevorstehenden Einstellung, die entsprechenden Auswirkungen und die für die Kunden verfügbaren alternativen Optionen für die folgenden Szenarien beschrieben.

Notfallwiederherstellung zwischen Kundenstandorten, die vom System Center Virtual Machine Manager (SCVMM) mit Site Recovery verwaltet werden

> [!IMPORTANT]
> Den Kunden wird empfohlen, dass sie schnellstmöglich die erforderlichen Wartungsschritte ausführen, um jegliche Unterbrechung in ihrer Umgebung zu vermeiden. 

## <a name="what-changes-should-you-expect"></a>Welche Änderungen sind zu erwarten?

- Ab März 2020 erhalten Sie Benachrichtigungen und E-Mail-Kommunikation des Azure-Portals über die bevorstehende Einstellung der Standort-zu-Standort-Replikation von Hyper-V-VMs. Die Einstellung ist für den März 2023 geplant.

- Wenn Sie über eine vorhandene Konfiguration verfügen, hat dies keine Auswirkung auf die Einrichtung.

- Nachdem die Szenarien veraltet sind, werden die bestehenden Replikationen möglicherweise gestört, sofern der Kunde nicht den alternativen Ansätzen folgt. Kunden können keine Vorgänge im Zusammenhang mit der Notfallwiederherstellung über die Azure Site Recovery-Erfahrung im Azure-Portal anzeigen, verwalten oder ausführen.
 
## <a name="alternatives"></a>Alternativen 

Nachstehend finden Sie die Alternativen, mit denen der Kunde dafür sorgen kann, dass seine Notfallwiederherstellungsstrategie nicht beeinträchtigt wird, nachdem das jeweilige Szenario eingestellt wurde. 

- Option 1 (empfohlen): Entscheiden Sie sich für [Azure als neues Ziel für die Notfallwiederherstellung](hyper-v-vmm-azure-tutorial.md).


- Option 2: Entscheiden Sie sich für die Fortsetzung der Standort-zu-Standort-Replikation mit der zugrunde liegenden [Hyper-V-Replikatlösung](/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica). Dadurch können Sie Konfigurationen für die Notfallwiederherstellung jedoch nicht mithilfe von Azure Site Recovery im Azure-Portal verwalten. 


## <a name="remediation-steps"></a>Schritte zur Bereinigung

Wenn Sie sich für Option 1 entscheiden, führen Sie die folgenden Schritte aus:

1. [Deaktivieren des Schutzes aller virtuellen Computer, die den VMMs zugeordnet sind](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-secondary-vmm-server-using-the-system-center-vmm-to-vmm-scenario) Verwenden Sie die Option **Replikation deaktivieren und entfernen**, oder führen Sie die genannten Skripts aus, um sicherzustellen, dass die lokalen Replikationseinstellungen bereinigt werden. 

2. [Aufheben der Registrierung aller VMM-Server](site-recovery-manage-registration-and-protection.md#unregister-a-vmm-server) in der Standort-zu-Standort-Replikationskonfiguration

3. [Vorbereiten von Azure-Ressourcen](tutorial-prepare-azure-for-hyperv.md), um die Replikation Ihrer VMs zu ermöglichen
4. [Vorbereiten lokaler Hyper-V-Server](hyper-v-prepare-on-premises-tutorial.md)
5. [Einrichten der Replikation für die VMs in der VMM-Cloud](hyper-v-vmm-azure-tutorial.md)
6. Optional (jedoch empfohlen): [Ausführen eines DR-Drillvorgangs](tutorial-dr-drill-azure.md)

Wenn Sie sich für die Option 2 der Verwendung von Hyper-V-Replikaten entscheiden, führen Sie die folgenden Schritte aus:

1. Klicken Sie unter **Geschützte Elemente** > **Replizierte Elemente** mit der rechten Maustaste auf den Computer, und klicken Sie dann auf **Replikation deaktivieren**.
2. Wählen Sie unter **Replikation deaktivieren** die Option **Entfernen** aus.

    Dadurch wird das replizierte Element aus Azure Site Recovery entfernt (und die Abrechnung wird beendet). Die Konfiguration der Replikation auf dem lokalen virtuellen Computer wird **nicht** bereinigt. 

## <a name="next-steps"></a>Nächste Schritte
Planen Sie für die Einstellung, und wählen Sie die alternative Option aus, die für Ihre Infrastruktur und Ihr Unternehmen am besten geeignet ist. Falls Sie Fragen dazu haben, wenden Sie sich an den Microsoft-Support.

