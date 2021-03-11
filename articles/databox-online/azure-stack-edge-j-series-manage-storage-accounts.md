---
title: Verwaltung von Azure Stack Edge Pro-GPU-Speicherkonten | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie im Azure-Portal ein Speicherkonto auf Ihrem Azure Stack Edge Pro-Gerät verwalten.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 02/22/2021
ms.author: alkohli
ms.openlocfilehash: 58ea8709af33510b1848d0c031d513e4af7a352d
ms.sourcegitcommit: 5bbc00673bd5b86b1ab2b7a31a4b4b066087e8ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2021
ms.locfileid: "102436548"
---
# <a name="use-the-azure-portal-to-manage-edge-storage-accounts-on-your-azure-stack-edge-pro"></a>Verwalten von Edge-Speicherkonten auf Ihrem Azure Stack Edge Pro-Gerät im Azure-Portal

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

In diesem Artikel wird beschrieben, wie Sie Edge-Speicherkonten auf Ihrem Azure Stack Edge Pro-Gerät verwalten. Sie können das Azure Stack Edge Pro-Gerät im Azure-Portal oder über die lokale Webbenutzeroberfläche verwalten. Im Azure-Portal können Sie Ihrem Gerät Edge-Speicherkonten hinzufügen oder diese löschen.

## <a name="about-edge-storage-accounts"></a>Informationen zu Edge-Speicherkonten

Sie können Daten von Ihrem Azure Stack Edge Pro-Gerät über das SMB-, NFS- oder REST-Protokoll übertragen. Wenn Sie Daten mithilfe der REST-APIs an Blob Storage übertragen möchten, müssen Sie auf Ihrem Azure Stack Edge Pro-Gerät Edge-Speicherkonten erstellen. 

Die Edge-Speicherkonten, die Sie auf dem Azure Stack Edge Pro-Gerät hinzufügen, werden Azure Storage-Konten zugeordnet. Alle Daten, die in die Edge-Speicherkonten geschrieben werden, werden automatisch per Push in die Cloud übertragen.

Ein Diagramm, das die beiden Typen von Konten und die Weise, wie die Daten von jedem dieser Konten in Azure gelangen, detailliert zeigt, ist nachstehend abgebildet:

![Diagramm für Blob Storage-Konten](media/azure-stack-edge-j-series-manage-storage-accounts/ase-blob-storage.svg)

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Hinzufügen eines Edge-Speicherkontos
> * Löschen eines Edge-Speicherkontos


## <a name="add-an-edge-storage-account"></a>Hinzufügen eines Edge-Speicherkontos

Führen Sie die folgenden Schritte aus, um ein Edge-Speicherkonto zu erstellen:

[!INCLUDE [Add an Edge storage account](../../includes/azure-stack-edge-gateway-add-storage-account.md)]

## <a name="delete-an-edge-storage-account"></a>Löschen eines Edge-Speicherkontos

Führen Sie die folgenden Schritte aus, um ein Edge-Speicherkonto löschen.

1. Wechseln Sie in Ihrer Ressource zu **Konfiguration > Speicherkonten**. Wählen Sie in der Liste der Speicherkonten das Speicherkonto aus, das Sie löschen möchten. Wählen Sie auf der oberen Befehlsleiste **Speicherkonto löschen** aus.

    ![Zur Liste der Speicherkonten wechseln](media/azure-stack-edge-j-series-manage-storage-accounts/delete-edge-storage-account-1.png)

2. Bestätigen Sie auf dem Blatt **Speicherkonto löschen** das zu löschende Speicherkonto, und wählen Sie **Löschen** aus.

    ![Löschen eines Speicherkontos bestätigen](media/azure-stack-edge-j-series-manage-storage-accounts/delete-edge-storage-account-2.png)

Die Liste der Speicherkonten wird aktualisiert, um den Löschvorgang widerzuspiegeln.


## <a name="add-delete-a-container"></a>Hinzufügen oder Löschen eines Containers

Sie können die Container für diese Speicherkonten auch hinzufügen oder löschen.

Führen Sie die folgenden Schritte aus, um einen Container hinzuzufügen:

1. Wählen Sie das Speicherkonto aus, das Sie verwalten möchten. Wählen Sie auf der oberen Befehlsleiste **+ Container hinzufügen** aus.

    ![Speicherkonto zum Hinzufügen eines Containers auswählen](media/azure-stack-edge-j-series-manage-storage-accounts/add-container-1.png)

2. Geben Sie einen Namen für den Container an. Dieser Container wird in Ihrem Edge-Speicherkonto sowie im Azure-Speicherkonto erstellt, das diesem Konto zugeordnet ist. 

    ![Edge-Container hinzufügen](media/azure-stack-edge-j-series-manage-storage-accounts/add-container-2.png)

Die Liste der Container wird aktualisiert, um den neu hinzugefügten Container widerzuspiegeln.

![Aktualisierte Liste der Container](media/azure-stack-edge-j-series-manage-storage-accounts/add-container-4.png)

Sie können jetzt in dieser Liste einen Container auswählen und auf der oberen Befehlsleiste **+ Container löschen** auswählen. 

![Löschen eines Containers](media/azure-stack-edge-j-series-manage-storage-accounts/add-container-3.png)

## <a name="sync-storage-keys"></a>Synchronisieren von Speicherschlüsseln

Sie können die Zugriffsschlüssel für die (lokalen) Edge-Speicherkonten auf Ihrem Gerät synchronisieren. 

Führen Sie die folgenden Schritte aus, um den Zugriffsschlüssel für das Speicherkonto zu synchronisieren:

1. Wählen Sie in Ihrer Ressource das Speicherkonto aus, das Sie verwalten möchten. Wählen Sie auf der oberen Befehlsleiste **Speicherschlüssel synchronisieren** aus.

    ![„Speicherkontoschlüssel synchronisieren“ auswählen](media/azure-stack-edge-j-series-manage-storage-accounts/sync-storage-key-1.png)

2. Wählen Sie **Ja** aus, wenn Sie zur Bestätigung aufgefordert werden.

    ![Auswählen von „Speicherkontoschlüssel synchronisieren“ 2](media/azure-stack-edge-j-series-manage-storage-accounts/sync-storage-key-2.png)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [Benutzer über das Azure-Portal verwalten](azure-stack-edge-j-series-manage-users.md).
