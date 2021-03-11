---
title: Erstellen und Hochladen einer VHD-Datei mit Flatcar Container Linux zur Verwendung in Azure
description: Erfahren Sie, wie Sie eine VHD erstellen und hochladen, die das Betriebssystem Flatcar Container Linux enthält.
author: marga-kinvolk
ms.author: danis
ms.service: virtual-machines
ms.collection: linux
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 07/16/2020
ms.reviewer: cynthn
ms.openlocfilehash: 5d8be9493b7a312270301e3520f301f797fe2167
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102565290"
---
# <a name="using-a-prebuilt-flatcar-image-for-azure"></a>Verwenden eines vordefinierten Flatcar-Images für Azure

Sie können vorab erstellte Images von virtuellen Azure-Festplatten mit Flatcar Container Linux für jeden der von Flatcar unterstützten Kanäle herunterladen:

- [stable](https://stable.release.flatcar-linux.net/amd64-usr/current/flatcar_production_azure_image.vhd.bz2)
- [beta](https://beta.release.flatcar-linux.net/amd64-usr/current/flatcar_production_azure_image.vhd.bz2)
- [alpha](https://alpha.release.flatcar-linux.net/amd64-usr/current/flatcar_production_azure_image.vhd.bz2)
- [edge](https://edge.release.flatcar-linux.net/amd64-usr/current/flatcar_production_azure_image.vhd.bz2)

Dieses Image ist bereits vollständig eingerichtet und für die Ausführung in Azure optimiert. Sie müssen es nur dekomprimieren.

## <a name="building-your-own-flatcar-based-virtual-machine-for-azure"></a>Erstellen eines eigenen Flatcar-basierten virtuellen Computers für Azure

Alternativ können Sie auch Ihr eigenes Flatcar Container Linux-Image erstellen.

Befolgen Sie auf einem Linux-Computer die Anweisungen im [Flatcar Container Linux developer SDK guide](https://docs.flatcar-linux.org/os/sdk-modifying-flatcar/) (Entwickler-SDK-Handbuch für Flatcar Container Linux). Wenn Sie das Skript `image_to_vm.sh` ausführen, stellen Sie sicher, dass Sie `--format=azure` übergeben, um eine virtuelle Azure-Festplatte zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

Sobald Sie über die VHD-Datei verfügen, können Sie mit der resultierenden Datei neue virtuelle Computer in Azure erstellen. Wenn Sie zum ersten Mal eine VHD-Datei in Azure hochladen, lesen Sie den Artikel [Erstellen einer Linux-VM aus einem benutzerdefinierten Datenträger](upload-vhd.md#option-1-upload-a-vhd).
