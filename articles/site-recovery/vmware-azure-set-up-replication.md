---
title: Einrichten von Replikationsrichtlinien für die VMware-Notfallwiederherstellung mit Azure Site Recovery | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Replikationseinstellungen für die VMware-Notfallwiederherstellung in Azure mit Azure Site Recovery konfigurieren.
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: sutalasi
ms.openlocfilehash: 45921bdf802a649b7b802f44d2842a543e44f02b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "84699599"
---
# <a name="configure-and-manage-replication-policies-for-vmware-disaster-recovery"></a>Konfigurieren und Verwalten von Replikationsrichtlinien für die VMware-Notfallwiederherstellung

In diesem Artikel wird beschrieben, wie Sie mithilfe von [Azure Site Recovery](site-recovery-overview.md) eine Replikationsrichtlinie für die Replikation von VMware-VMs in Azure konfigurieren.

## <a name="create-a-policy"></a>Erstellen einer Richtlinie

1. Wählen Sie **Verwalten** > **Site Recovery-Infrastruktur**.
2. Wählen Sie unter **For VMware and Physical machines** (Für VMware und physische Computer) die Option **Replikationsrichtlinien** aus.
3. Klicken Sie auf **+ Replikationsrichtlinie**, und geben Sie den Richtliniennamen an.
4. Geben Sie unter **RPO-Schwellenwert** den RPO-Grenzwert an. Wenn die fortlaufende Replikation diesen Grenzwert überschreitet, werden Warnungen generiert.
5. Geben Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts** die Dauer des Aufbewahrungszeitfensters für die einzelnen Wiederherstellungspunkte (in Stunden) an. Geschützte Computer können innerhalb eines Aufbewahrungszeitfensters an einem beliebigen Punkt wiederhergestellt werden. Für in Storage Premium replizierte Computer wird eine Aufbewahrungsdauer von bis zu 24 Stunden unterstützt. Bei Storage Standard werden bis zu 72 Stunden unterstützt.
6. Wählen Sie unter **Häufigkeit der Momentaufnahmen für App-Konsistenz** in der Dropdownliste aus, wie häufig (in Stunden) Wiederherstellungspunkte erstellt werden sollen, die anwendungskonsistente Momentaufnahmen enthalten. Wenn Sie die Generierung anwendungskonsistenter Punkte deaktivieren möchten, wählen Sie in der Dropdownliste den Wert „Aus“ aus.
7. Klicken Sie auf **OK**. Die Richtlinie sollte innerhalb von 30 bis 60 Sekunden erstellt werden.

Wenn Sie eine Replikationsrichtlinie erstellen, wird automatisch eine entsprechende Replikationsrichtlinie für das Failback erstellt, die das Suffix „failback“ enthält. Nach dem Erstellen der Richtlinie können Sie sie bearbeiten, indem Sie **Einstellungen bearbeiten** auswählen.

## <a name="associate-a-configuration-server"></a>Zuordnen eines Konfigurationsservers

Ordnen Sie Ihrem lokalen Konfigurationsserver die Replikationsrichtlinie zu.

1. Klicken Sie auf **Zuordnen**, und wählen Sie den Konfigurationsserver aus.

    ![Zuordnen des Konfigurationsservers](./media/vmware-azure-set-up-replication/associate1.png)
2. Klicken Sie auf **OK**. Die Zuordnung des Konfigurationsservers sollte innerhalb von ein bis zwei Minuten abgeschlossen sein.

    ![Zuordnung des Konfigurationsservers](./media/vmware-azure-set-up-replication/associate2.png)

## <a name="edit-a-policy"></a>Bearbeiten einer Richtlinie

Sie können eine Replikationsrichtlinie ändern, nachdem sie erstellt wurde.

- Änderungen an der Richtlinie werden auf alle Computer angewendet, die die Richtlinie verwenden.
- Wenn Sie replizierte Computer einer anderen Replikationsrichtlinie zuordnen möchten, müssen Sie den Schutz für die relevanten Computer deaktivieren und erneut aktivieren.

Bearbeiten Sie eine Richtlinie wie folgt:
1. Wählen Sie **Verwalten** > **Site Recovery-Infrastruktur** > **Replikationsrichtlinien** aus.
2. Wählen Sie die Replikationsrichtlinie aus, die Sie ändern möchten.
3. Klicken Sie auf **Einstellungen bearbeiten**, und aktualisieren Sie die Felder „RPO-Schwellenwert“, „Aufbewahrung des Wiederherstellungspunkts (in Stunden)“ oder „Häufigkeit der Momentaufnahmen für App-Konsistenz“ nach Bedarf.
4. Wenn Sie die Generierung anwendungskonsistenter Punkte deaktivieren möchten, wählen Sie in der Dropdownliste des Felds **App-konsistente Momentaufnahmenhäufigkeit** den Wert „Aus“ aus.
5. Klicken Sie auf **Speichern**. Die Richtlinie sollte innerhalb von 30 bis 60 Sekunden aktualisiert werden.



## <a name="disassociate-or-delete-a-replication-policy"></a>Aufheben der Zuordnung oder Löschen einer Replikationsrichtlinie

1. Wählen Sie die Replikationsrichtlinie aus.
    a. Um die Zuordnung der Richtlinie zum Konfigurationsserver aufzuheben, stellen Sie sicher, dass die Richtlinie von keinen replizierten Computern verwendet wird. Klicken Sie anschließend auf **Zuordnung aufheben**.
    b. Um die Richtlinie zu löschen, stellen Sie sicher, dass diese keinem Konfigurationsserver zugeordnet ist. Klicken Sie anschließend auf **Löschen**. Das Löschen dauert 30 bis 60 Sekunden.
2. Klicken Sie auf **OK**.
