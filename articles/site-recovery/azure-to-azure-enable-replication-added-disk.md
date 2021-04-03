---
title: Aktivieren der Replikation für einen hinzugefügten Azure-VM-Datenträger in Azure Site Recovery
description: In diesem Artikel wird beschrieben, wie Sie die Replikation für einen Datenträger aktivieren, der einer Azure-VM hinzugefügt wird, bei der die Notfallwiederherstellung mit Azure Site Recovery aktiviert wurde.
author: sideeksh
manager: rochakm
ms.topic: how-to
ms.date: 04/29/2019
ms.openlocfilehash: 6cbbe63d7968816de78256f5a8408517bb8da278
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "75973791"
---
# <a name="enable-replication-for-a-disk-added-to-an-azure-vm"></a>Aktivieren der Replikation für einen Datenträger, der einer Azure-VM hinzugefügt wird


In diesem Artikel wird beschrieben, wie die Replikation für Datenträger aktiviert wird, die einer Azure-VM hinzugefügt werden, bei der die Notfallwiederherstellung in eine andere Region mithilfe von [Azure Site Recovery](site-recovery-overview.md) bereits aktiviert wurde.

Das Aktivieren der Replikation für einen Datenträger, den Sie einer VM hinzufügen, wird bei Azure-VMs mit verwalteten Datenträgern unterstützt.

Wenn Sie einer Azure-VM einen neuen Datenträger hinzufügen, der in eine andere Azure-Region repliziert wird, geschieht Folgendes:

-   Die Replikationsintegrität für die VM zeigt eine Warnung an, und ein Hinweis im Portal informiert sie, dass ein oder mehrere Datenträger für den Schutz zur Verfügung stehen.
-   Wenn Sie den Schutz für die hinzugefügten Datenträger aktivieren, wird die Warnung nach der ersten Replikation des Datenträgers nicht mehr angezeigt.
-   Wenn Sie wählen, dass die Replikation für den Datenträger nicht aktiviert werden soll, können Sie die Warnung wahlweise verwerfen.

![Neuer Datenträger hinzugefügt](./media/azure-to-azure-enable-replication-added-disk/newdisk.png)



## <a name="before-you-start"></a>Vorbereitung

In diesem Artikel wird davon ausgegangen, dass Sie eine Notfallwiederherstellung für die VM, der Sie den Datenträger hinzufügen, bereits eingerichtet haben. Wenn das noch nicht geschehen ist, führen Sie die Schritte im [Tutorial zur Azure-zu-Azure-Notfallwiederherstellung](azure-to-azure-tutorial-enable-replication.md) aus.

## <a name="enable-replication-for-an-added-disk"></a>Aktivieren der Replikation für einen hinzugefügten Datenträger

Führen Sie folgende Schritte aus, um die Replikation für einen hinzugefügten Datenträger zu aktivieren:

1. Klicken Sie im Tresor > **Replizierte Elemente**  auf die VM, der Sie den Datenträger hinzugefügt haben.
2. Klicken Sie auf **Datenträger**, und wählen Sie den Datenträger aus, für den Sie die Replikation aktivieren möchten (diese Datenträger haben den Status **Nicht geschützt**).
3.  Klicken Sie in **Datenträgerdetails** auf **Replikation aktivieren**.

    ![Aktivieren der Replikation für einen hinzugefügten Datenträger](./media/azure-to-azure-enable-replication-added-disk/enabled-added.png)

Nachdem die Ausführung des Auftrags zum Aktivieren der Replikation begonnen hat und die erste Replikation beendet wurde, wird die Warnung zur Replikationsintegrität für das Datenträgerproblem entfernt.



## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr](site-recovery-test-failover-to-azure.md) über die Ausführung von Testfailovervorgängen.
