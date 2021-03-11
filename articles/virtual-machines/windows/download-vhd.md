---
title: Herunterladen einer Windows-VHD von Azure
description: Laden Sie eine Windows-VHD mithilfe des Azure-Portals herunter.
author: cynthn
manager: gwallace
ms.service: virtual-machines
ms.subservice: disks
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 01/13/2019
ms.author: cynthn
ms.openlocfilehash: a33b248c18bcbf322a1e2d911453a1c4c087e625
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102550517"
---
# <a name="download-a-windows-vhd-from-azure"></a>Herunterladen einer Windows-VHD von Azure

In diesem Artikel erfahren Sie, wie Sie mithilfe des Azure-Portals eine Windows-VHD-Datei (Virtual Hard Disk, virtuelle Festplatte) von Azure herunterladen.

## <a name="optional-generalize-the-vm"></a>Optional: Generalisieren des virtuellen Computers

Wenn Sie die VHD als [Image](tutorial-custom-images.md) verwenden möchten, um andere VMs zu erstellen, sollten Sie mithilfe von [Sysprep](/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation) das Betriebssystem generalisieren. 

Um die VHD als Image zum Erstellen von anderen VMs zu verwenden, generalisieren Sie die VM.

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com/)an, falls Sie dies noch nicht getan haben.
2. [Stellen Sie eine Verbindung mit der VM her](connect-logon.md). 
3. Öffnen Sie auf der VM das Eingabeaufforderungsfenster als Administrator.
4. Wechseln Sie zum Verzeichnis *%windir%\system32\sysprep*, und führen Sie „sysprep.exe“ aus.
5. Wählen Sie im Dialogfeld „Systemvorbereitungsprogramm“ die Option **Out-of-Box-Experience (OOBE) für System aktivieren** aus, und vergewissern Sie sich, dass **Verallgemeinern** ausgewählt ist.
6. Wählen Sie unter „Optionen für Herunterfahren“ die Option **Herunterfahren** aus, und klicken Sie dann auf **OK**. 


## <a name="stop-the-vm"></a>Beenden des virtuellen Computers

Eine VHD kann nicht von Azure heruntergeladen werden, wenn sie an eine ausgeführte VM angefügt ist. Sie müssen die VM beenden, um eine VHD herunterzuladen. 

1. Klicken Sie im Hub-Menü im Azure-Portal auf **Virtuelle Computer**.
1. Wählen Sie die VM aus der Liste aus.
1. Klicken Sie auf dem Blatt für die VM auf **Beenden**.


## <a name="generate-download-url"></a>Generieren der Download-URL

Um die VHD-Datei herunterzuladen, müssen Sie eine [SAS-URL (Shared Access Signature)](../../storage/common/storage-sas-overview.md?toc=/azure/virtual-machines/windows/toc.json) generieren. Wenn die URL generiert wird, wird der URL eine Ablaufzeit zugewiesen.

1. Klicken Sie auf der Seite für den virtuellen Computer im linken Menü auf **Datenträger**.
1. Wählen Sie den Betriebssystemdatenträger für die VM aus.
1. Wählen Sie auf der Seite für den Datenträger im linken Menü die Option **Datenträgerexport** aus.
1. Die Standardablaufzeit der URL beträgt *3.600* Sekunden. Erhöhen Sie dies für Windows-Betriebssystemdatenträger auf **36.000**.
1. Klicken Sie auf **URL generieren**.

> [!NOTE]
> Die Ablaufzeit wird von der Standardzeit erhöht, um genug Zeit für das Herunterladen der großen VHD-Datei für ein Windows Server-Betriebssystem bereitzustellen. Sie können davon ausgehen, dass das Herunterladen einer VHD-Datei mit dem Windows Server-Betriebssystem mehrere Stunden dauert, je nach Ihrer Verbindung. Wenn Sie eine VHD für einen Datenträger herunterladen, reicht die Standardzeit. 
> 
> 

## <a name="download-vhd"></a>Herunterladen der VHD

1. Klicken Sie unter der URL, die generiert wurde, auf „VHD-Datei herunterladen“.
1. Möglicherweise müssen Sie im Browser auf **Speichern** klicken, um den Download zu starten. Der Standardname für die VHD-Datei lautet *abcd*.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie eine [VHD-Datei in Azure hochladen](upload-generalized-managed.md). 
- [Erstellen verwalteter Datenträger auf der Grundlage nicht verwalteter Datenträger in einem Speicherkonto](attach-disk-ps.md)
- [Verwalten von Azure-Datenträgern mit PowerShell](tutorial-manage-data-disk.md)
