---
author: cynthn
ms.service: azure
ms.topic: include
ms.date: 11/09/2018
ms.author: cynthn
ms.openlocfilehash: c2ed33aea77b5478e8d17f6bd0213ef3e778b806
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "67178428"
---
## <a name="transfer-local-files-to-cloud-shell"></a>Übertragen Sie lokale Dateien auf Cloud Shell
Das Verzeichnis `clouddrive` wird mit dem Blatt „Storage“ im Azure-Portal synchronisiert. Verwenden Sie dieses Blatt, um lokale Dateien in Ihre oder aus Ihrer Dateifreigabe zu übertragen. Die Aktualisierung von Dateien aus Cloud Shell wird auf der GUI für die Dateispeicherung widergespiegelt, wenn Sie das Blatt aktualisieren.

### <a name="download-files"></a>Herunterladen von Dateien

![Auflisten von lokalen Dateien](../articles/cloud-shell/media/persisting-shell-storage/download.png)
1. Navigieren Sie im Azure-Portal zur bereitgestellten Dateifreigabe.
2. Wählen Sie die Zieldatei aus.
3. Wählen Sie die Schaltfläche **Herunterladen**.

### <a name="upload-files"></a>Hochladen von Dateien

![Hochzuladende lokale Dateien](../articles/cloud-shell/media/persisting-shell-storage/upload.png)
1. Navigieren Sie zu Ihrer bereitgestellten Dateifreigabe.
2. Wählen Sie die Schaltfläche **Hochladen**.
3. Wählen Sie die Dateien aus, die Sie hochladen möchten.
4. Bestätigen Sie den Upload.

Nun sollten Sie die Dateien angezeigt werden, auf die Sie in Ihrem Verzeichnis `clouddrive` in Cloud Shell zugreifen können.