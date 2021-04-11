---
title: Anfügen eines verwalteten Datenträgers an einen virtuellen Windows-Computer – Azure
description: Hier erfahren Sie, wie Sie im Azure-Portal einen verwalteten Datenträger an einen virtuellen Windows-Computer anfügen.
author: roygara
ms.service: virtual-machines
ms.collection: windows
ms.topic: how-to
ms.date: 02/06/2020
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 8c64b0ff5b7a9abfa58ec17d0ebcabe05b0ed6e9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102550806"
---
# <a name="attach-a-managed-data-disk-to-a-windows-vm-by-using-the-azure-portal"></a>Anfügen eines verwalteten Datenträgers an einen virtuellen Windows-Computer im Azure-Portal

In diesem Artikel wird beschrieben, wie Sie im Azure-Portal einen neuen verwalteten Datenträger an einen virtuellen Windows-Computer (Virtual Machine, VM) anfügen. Die Größe des virtuellen Computers bestimmt, wie viele Datenträger Sie anfügen können. Weitere Informationen finden Sie unter [Größen für virtuelle Computer](../sizes.md).


## <a name="add-a-data-disk"></a>Hinzufügen eines Datenträgers

1. Wechseln Sie zum [Azure-Portal](https://portal.azure.com), um einen Datenträger für Daten hinzuzufügen. Suchen Sie nach **Virtuelle Computer**, und wählen Sie diese Option aus.
2. Wählen Sie in der Liste einen virtuellen Computer aus.
3. Wählen Sie auf der Seite **Virtueller Computer** die Option **Datenträger** aus.
4. Wählen Sie auf der Seite **Datenträger** die Option **Datenträger hinzufügen** aus.
5. Wählen Sie in der Dropdownliste für den neuen Datenträger **Datenträger erstellen** aus.
6. Geben Sie auf der Seite **Verwalteten Datenträger erstellen** einen Namen für den Datenträger ein, und passen Sie die anderen Einstellungen nach Bedarf an. Wählen Sie **Erstellen**, wenn Sie fertig sind.
7. Wählen Sie auf der Seite **Datenträger** die Option **Speichern** aus, um die neue Datenträgerkonfiguration für den virtuellen Computer zu speichern.
8. Nachdem der Datenträger von Azure erstellt und an den virtuellen Computer angefügt wurde, wird der neue Datenträger in den Datenträgereinstellungen des virtuellen Computers unter **Datenträger** angezeigt.


## <a name="initialize-a-new-data-disk"></a>Initialisieren eines neuen Datenträgers

1. Stellen Sie eine Verbindung zur VM her.
1. Wählen Sie auf dem ausgeführten virtuellen Computer das **Windows-Startmenü** aus, und geben Sie im Suchfeld den Namen **diskmgmt.msc** ein. Die **Datenträgerverwaltungskonsole** wird geöffnet.
2. Die Datenträgerverwaltung erkennt, dass ein neuer nicht initialisierter Datenträger vorhanden ist, und deshalb wird das Fenster **Datenträger initialisieren** angezeigt.
3. Vergewissern Sie sich, dass der neue Datenträger ausgewählt ist, und wählen Sie dann **OK** aus, um ihn zu initialisieren.
4. Der neue Datenträger wird als **Nicht zugeordnet** angezeigt. Klicken Sie mit der rechten Maustaste auf den Datenträger, und wählen **Neues einfaches Volume** aus. Das Fenster **Assistent zum Erstellen neuer einfacher Volumes** wird geöffnet.
5. Führen Sie die Schritte des Assistenten aus, und übernehmen Sie dabei alle Standardeinstellungen. Wählen Sie abschließend **Fertig stellen** aus.
6. Schließen Sie die **Datenträgerverwaltung**.
7. Ein Popupfenster wird angezeigt, in dem Sie darüber informiert werden, dass Sie den neuen Datenträger formatieren müssen, bevor Sie ihn verwenden können. Wählen Sie **Datenträger formatieren** aus.
8. Überprüfen Sie im Fenster **Neuen Datenträger formatieren** die Einstellungen, und wählen Sie dann **Starten** aus.
9. Es wird eine Warnung angezeigt, die Sie darüber informiert, dass bei der Formatierung des Datenträgers alle Daten gelöscht werden. Wählen Sie **OK** aus.
10. Wählen Sie nach Abschluss der Formatierung die Option **OK** aus.

## <a name="next-steps"></a>Nächste Schritte

- Sie können auch [einen Datenträger mithilfe von PowerShell anfügen](attach-disk-ps.md).
- Wenn Ihre Anwendung Laufwerk *D:* für die Datenspeicherung verwenden muss, können Sie [den Laufwerkbuchstaben des temporären Windows-Datenträgers ändern](change-drive-letter.md).