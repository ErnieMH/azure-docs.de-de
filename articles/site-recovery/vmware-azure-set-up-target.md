---
title: Vorbereiten des VMware-VM-Replikationsziels in Azure Site Recovery
description: Dieser Artikel beschreibt, wie Sie Ihre Azure-Zielumgebung für das Replizieren von virtuellen VMware-Computern nach Azure vorbereiten.
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/03/2019
ms.author: mayg
ms.openlocfilehash: c721234f2e1d806d51d31f3466e441bf8360f6b8
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "73693169"
---
# <a name="prepare-the-target-environment-for-disaster-recovery-of-vmware-vms-or-physical-servers-to-azure"></a>Vorbereiten der Zielumgebung für die Notfallwiederherstellung von VMWare-VMs oder lokalen physischen Servern in Azure

Dieser Artikel beschreibt, wie Sie Ihre Azure-Zielumgebung vorbereiten, um mit dem Replizieren von virtuellen VMware-Computern bzw. physischen Servern nach Azure zu beginnen.

## <a name="prerequisites"></a>Voraussetzungen

In diesem Artikel wird Folgendes vorausgesetzt:
- Sie haben einen Recovery Services-Tresor im [Azure-Portal](https://portal.azure.com "Azure-Portal") erstellt, um Ihre Quellcomputer zu schützen.
- Sie haben Ihre lokale Umgebung eingerichtet, um [virtuelle VMware-Quellcomputer](vmware-azure-set-up-source.md) oder [physische Server](physical-azure-set-up-source.md) nach Azure zu replizieren.

## <a name="prepare-target"></a>Vorbereiten des Ziels

Nach Abschluss von **Schritt 1: Auswählen des Schutzziels** und **Schritt 2: Vorbereiten der Quelle** werden Sie weitergeleitet zu **Schritt 3: Ziel**.

![Vorbereiten des Ziels](./media/vmware-azure-set-up-target/prepare-target-vmware-to-azure.png)

1. **Abonnement:** Wählen Sie aus dem Dropdownmenü das Abonnement aus, in das Sie die virtuellen Computer bzw. physischen Server replizieren möchten.
2. **Bereitstellungsmodell:** Wählen Sie entweder das klassische oder das Resource Manager-Bereitstellungsmodell aus.

Basierend auf dem ausgewählten Bereitstellungsmodell wird eine Überprüfung vorgenommen, um sicherzustellen, dass Sie über mindestens ein virtuelles Netzwerk im Zielabonnement verfügen, um die Replikation und das Failover Ihres virtuellen Computers bzw. physischen Servers durchzuführen.

Wenn die Überprüfungen erfolgreich abgeschlossen sind, klicken Sie auf „OK“, um mit dem nächsten Schritt fortzufahren.

Wenn Sie nicht über ein virtuelles Netzwerk verfügen, können Sie eins erstellen, indem Sie oben auf der Seite auf die Schaltfläche **+ Netzwerk** klicken.

## <a name="next-steps"></a>Nächste Schritte
[Konfigurieren der Replikationseinstellungen](vmware-azure-set-up-replication.md).
