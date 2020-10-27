---
title: Ein virtuelles Netzwerk in Azure kann nicht gelöscht werden | Microsoft-Dokumentationen
description: In diesem Artikel erfahren Sie, was Sie tun können, wenn ein virtuelles Netzwerk in Azure nicht gelöscht werden kann.
services: virtual-network
documentationcenter: na
author: chadmath
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 83afdf7e9dc50e50d747db99cd8439d75e6f7804
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2020
ms.locfileid: "92167813"
---
# <a name="troubleshooting-failed-to-delete-a-virtual-network-in-azure"></a>Problembehandlung: Fehler beim Löschen eines virtuellen Netzwerks in Azure.

Wenn Sie versuchen, in Microsoft Azure ein virtuelles Netzwerk zu löschen, können Fehler auftreten. Dieser Artikel enthält Schritte, mit denen Sie dieses Problem beheben können.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-guidance"></a>Anleitungen zur Problembehandlung 

1. [Überprüfen, ob im virtuellen Netzwerk ein virtuelles Netzwerkgateway ausgeführt wird](#check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network)
2. [Überprüfen, ob im virtuellen Netzwerk ein Anwendungsgateway ausgeführt wird](#check-whether-an-application-gateway-is-running-in-the-virtual-network)
3. [Überprüfen, ob im virtuellen Netzwerk Azure Active Directory Domain Services aktiviert ist](#check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network)
4. [Überprüfen, ob das virtuelle Netzwerk mit anderen Ressourcen verbunden ist](#check-whether-the-virtual-network-is-connected-to-other-resource)
5. [Überprüfen, ob im virtuellen Netzwerk noch ein virtueller Computer ausgeführt wird](#check-whether-a-virtual-machine-is-still-running-in-the-virtual-network)
6. [Überprüfen, ob das virtuelle Netzwerk im Migrationsprozess hängengeblieben ist](#check-whether-the-virtual-network-is-stuck-in-migration)

## <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung

### <a name="check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network"></a>Überprüfen, ob im virtuellen Netzwerk ein virtuelles Netzwerkgateway ausgeführt wird

Um das virtuelle Netzwerk zu entfernen, müssen Sie zunächst das Gateway des virtuellen Netzwerks entfernen.

Für klassische virtuelle Netzwerke wechseln Sie im Azure-Portal auf die **Übersichtsseite** des klassischen virtuellen Netzwerks. Wenn das Gateway im virtuellen Netzwerk ausgeführt wird, wird im Abschnitt **VPN-Verbindungen** die IP-Adresse des Gateways angezeigt. 

![Überprüfen Sie, ob das Gateway ausgeführt wird](media/virtual-network-troubleshoot-cannot-delete-vnet/classic-gateway.png)

Für virtuelle Netzwerke wechseln Sie zur **Übersichtsseite** des virtuellen Netzwerks. Überprüfen Sie **Verbundene Geräte** für das Gateway des virtuellen Netzwerks.

![Screenshot der Liste der verbundenen Geräte für ein virtuelles Netzwerk im Azure-Portal. Das Gateway des virtuellen Netzwerks in der Liste ist hervorgehoben.](media/virtual-network-troubleshoot-cannot-delete-vnet/vnet-gateway.png)

Bevor Sie das Gateway entfernen können, müssen Sie zunächst alle im Gateway vorhandenen **Verbindungsobjekte** entfernen. 

### <a name="check-whether-an-application-gateway-is-running-in-the-virtual-network"></a>Überprüfen, ob im virtuellen Netzwerk ein Anwendungsgateway ausgeführt wird

Wechseln Sie zur **Übersichtsseite** des virtuellen Netzwerks. Überprüfen Sie **Verbundene Geräte** für das Anwendungsgateway.

![Screenshot der Liste der verbundenen Geräte für ein virtuelles Netzwerk im Azure-Portal. Das Anwendungsgateway in der Liste ist hervorgehoben.](media/virtual-network-troubleshoot-cannot-delete-vnet/app-gateway.png)

Wenn ein Anwendungsgateway vorhanden ist, müssen Sie dieses entfernen, bevor Sie das virtuelle Netzwerk löschen können.

### <a name="check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network"></a>Überprüfen, ob im virtuellen Netzwerk Azure Active Directory Domain Services aktiviert ist

Wenn Active Directory Domain Services aktiviert und mit dem virtuellen Netzwerk verbunden ist, können sie dieses virtuelle Netzwerk nicht löschen. 

![Screenshot des Bildschirms „Azure Active Directory Domain Services“ im Azure-Portal. Das Feld „Verfügbar in virtuellem Netzwerk/Subnetz“ ist hervorgehoben.](media/virtual-network-troubleshoot-cannot-delete-vnet/enable-domain-services.png)

Informationen zum Deaktivieren des Service finden Sie unter [Deaktivieren von Azure Active Directory Domain Services mithilfe des Azure-Portals](../active-directory-domain-services/delete-aadds.md).

### <a name="check-whether-the-virtual-network-is-connected-to-other-resource"></a>Überprüfen, ob das virtuelle Netzwerk mit anderen Ressourcen verbunden ist

Suchen Sie nach Verbindungslinks, Verbindungen und virtuelles Netzwerk über Peerings. Dies können die Ursachen dafür sein, dass ein virtuelles Netzwerk nicht gelöscht werden kann. 

Die empfohlene Löschreihenfolge lautet wie folgt:

1. Gatewayverbindungen
2. Gateways
3. IP-Adressen
4. Virtuelles Netzwerk über Peerings
5. App Service-Umgebung (ASE)

### <a name="check-whether-a-virtual-machine-is-still-running-in-the-virtual-network"></a>Überprüfen, ob im virtuellen Netzwerk noch ein virtueller Computer ausgeführt wird

Stellen Sie sicher, dass sich kein virtueller Computer im virtuellen Netzwerk befindet.

### <a name="check-whether-the-virtual-network-is-stuck-in-migration"></a>Überprüfen, ob das virtuelle Netzwerk im Migrationsprozess hängengeblieben ist

Wenn das virtuelle Netzwerk im Migrationszustand hängengeblieben ist, kann es nicht gelöscht werden. Führen Sie den folgenden Befehl aus, um die Migration abzubrechen, und löschen Sie dann das virtuelle Netzwerk.

```azurepowershell
Move-AzureVirtualNetwork -VirtualNetworkName "Name" -Abort
```

## <a name="next-steps"></a>Nächste Schritte

- [Azure Virtual Network](virtual-networks-overview.md)
- [Azure Virtual Network – häufig gestellte Fragen](virtual-networks-faq.md)
