---
title: 'Azure ExpressRoute: Hinzufügen eines Gateways zu einem VNET: klassisch'
description: Konfigurieren Sie ein VNet-Gateway für ein über das klassische Bereitstellungsmodell bereitgestelltes virtuelles Netzwerk, indem Sie PowerShell für die ExpressRoute-Konfiguration verwenden.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 12/06/2019
ms.author: duau
ms.custom: seodec18
ms.openlocfilehash: 0f42a7d030f45be1b96728fdcac7a13cf797ed34
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89396402"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic"></a>Konfigurieren eines Gateways für ein virtuelles Netzwerk für ExpressRoute mit PowerShell (klassisch)
> [!div class="op_single_selector"]
> * [Resource Manager – PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [Klassisch – PowerShell](expressroute-howto-add-gateway-classic.md)
> * [Video – Azure-Portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

Dieser Artikel führt Sie durch die Schritte, die zum Hinzufügen, Ändern der Größe und Entfernen eines virtuellen Netzwerkgateways (VNet) für ein vorhandenes VNet erforderlich sind. Die Schritte für diese Konfiguration gelten speziell für VNets, die unter Verwendung des **klassischen Bereitstellungsmodells** erstellt wurden und in einer ExpressRoute-Konfiguration verwendet werden. 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Informationen zu Azure-Bereitstellungsmodellen**

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a>Vorbereitungen
Stellen Sie sicher, dass Sie die für diese Konfiguration erforderlichen Azure PowerShell-Cmdlets installiert haben.

[!INCLUDE [classic powershell install instructions](../../includes/expressroute-poweshell-classic-install-include.md)]

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie das VNet-Gateway erstellt haben, können Sie Ihr VNet mit einer ExpressRoute-Verbindung verknüpfen. Weitere Informationen finden Sie unter [Verknüpfen eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-classic.md).

