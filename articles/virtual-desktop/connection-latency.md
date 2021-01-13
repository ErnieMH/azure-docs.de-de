---
title: Windows Virtual Desktop – Verbindungswartezeit für Benutzer – Azure
description: Verbindungswartezeit für Benutzer von Windows Virtual Desktop.
author: Heidilohr
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 403cf584c79bc0a166054ae36c9d2ea50e4b9d9e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88008728"
---
# <a name="determine-user-connection-latency-in-windows-virtual-desktop"></a>Ermitteln der Verbindungswartezeit für Benutzer in Windows Virtual Desktop

Windows Virtual Desktop ist global verfügbar. Administratoren können virtuelle Computer (Virtual Machines, VMS) in jeder gewünschten Azure-Region erstellen. Die Verbindungswartezeit hängt vom Standort der Benutzer und der virtuellen Computer ab. Windows Virtual Desktop-Dienste werden fortlaufend in neuen geografischen Regionen eingeführt, um die Wartezeit zu verbessern.

Mit dem [Tool „Einschätzung der Servicequalität für Windows Virtual Desktop“](https://azure.microsoft.com/services/virtual-desktop/assessment/) können Sie den besten Standort zum Optimieren der Wartezeit Ihrer virtuellen Computer ermitteln. Es wird empfohlen, das Tool alle zwei bis drei Monate zu verwenden, um sicherzustellen, dass sich der optimale Standort nicht geändert hat, während Windows Virtual Desktop in neuen Bereichen eingeführt wird.

## <a name="azure-traffic-manager"></a>Azure Traffic Manager

Windows Virtual Desktop verwendet den Azure Traffic Manager, der den Standort des DNS-Servers des Benutzers prüft, um die nächstgelegene Windows Virtual Desktop-Dienstinstanz zu finden. Administratoren wird empfohlen, diesen Standort zu überprüfen, bevor sie den Standort für die VMs auswählen.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum besten Standort für eine optimale Wartezeit finden Sie unter [Tool „Einschätzung der Servicequalität für Windows Virtual Desktop“](https://azure.microsoft.com/services/virtual-desktop/assessment/).
- Tarife finden Sie unter [Windows Virtual Desktop – Preise](https://azure.microsoft.com/pricing/details/virtual-desktop/).
- Die ersten Schritte mit Ihrer Windows Virtual Desktop-Bereitstellung finden Sie in [unserem Tutorial](./virtual-desktop-fall-2019/tenant-setup-azure-active-directory.md).