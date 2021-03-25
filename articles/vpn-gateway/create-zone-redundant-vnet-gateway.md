---
title: Erstellen eines zonenredundanten Gateways für das virtuelle Netzwerk in Azure-Verfügbarkeitszonen
description: Hier erfahren Sie, wie Sie zonenredundante VPN-Gateways und ExpressRoute-Gateways in Azure-Verfügbarkeitszonen bereitstellen.
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/03/2020
ms.author: cherylmc
ms.openlocfilehash: 2eaf1470e2d861ecfc1c1bc96f6040a1c3e0a644
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "89425227"
---
# <a name="create-a-zone-redundant-virtual-network-gateway-in-azure-availability-zones"></a>Erstellen eines zonenredundanten Gateways für das virtuelle Netzwerk in Azure-Verfügbarkeitszonen

Sie können VPN- und ExpressRoute-Gateways in Azure-Verfügbarkeitszonen bereitstellen. So erzielen Sie Stabilität, Skalierbarkeit und eine höhere Verfügbarkeit für die Gateways des virtuellen Netzwerks. Durch die Bereitstellung von Gateways in Azure-Verfügbarkeitszonen werden die Gateways innerhalb einer Region physisch und logisch getrennt. Gleichzeitig wird die Konnektivität Ihres lokalen Netzwerks mit Azure vor Ausfällen auf Zonenebene geschützt. Weitere Informationen finden Sie unter [Informationen zu zonenredundanten Gateways für das virtuelle Netzwerk](about-zone-redundant-vnet-gateways.md) und [Azure-Verfügbarkeitszonen](../availability-zones/az-overview.md).

## <a name="before-you-begin"></a>Voraussetzungen

[!INCLUDE [powershell](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

## <a name="1-declare-your-variables"></a><a name="variables"></a>1. Deklarieren von Variablen

Deklarieren Sie die gewünschten Variablen. Verwenden Sie das unten gezeigte Beispiel, und ersetzen Sie die Werte nach Bedarf durch Ihre eigenen. Wenn Sie Ihre PowerShell-/Cloud Shell-Sitzung zu einem beliebigen Zeitpunkt während der Übung schließen, kopieren Sie einfach die Werte noch mal, und fügen Sie sie ein, um die Variablen erneut zu deklarieren. Wenn Sie einen Standort angeben, stellen Sie sicher, dass die angegebene Region unterstützt wird. Weitere Informationen finden Sie in den [häufig gestellten Fragen](#faq).

```azurepowershell-interactive
$RG1         = "TestRG1"
$VNet1       = "VNet1"
$Location1   = "CentralUS"
$FESubnet1   = "FrontEnd"
$BESubnet1   = "Backend"
$GwSubnet1   = "GatewaySubnet"
$VNet1Prefix = "10.1.0.0/16"
$FEPrefix1   = "10.1.0.0/24"
$BEPrefix1   = "10.1.1.0/24"
$GwPrefix1   = "10.1.255.0/27"
$Gw1         = "VNet1GW"
$GwIP1       = "VNet1GWIP"
$GwIPConf1   = "gwipconf1"
```

## <a name="2-create-the-virtual-network"></a><a name="configure"></a>2. Erstellen des virtuellen Netzwerks

Erstellen Sie eine Ressourcengruppe.

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName $RG1 -Location $Location1
```

Erstellen Sie ein virtuelles Netzwerk.

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubnet1 -AddressPrefix $FEPrefix1
$besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubnet1 -AddressPrefix $BEPrefix1
$vnet = New-AzVirtualNetwork -Name $VNet1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNet1Prefix -Subnet $fesub1,$besub1
```

## <a name="3-add-the-gateway-subnet"></a><a name="gwsub"></a>3. Hinzufügen des Gatewaysubnetzes

Das Gatewaysubnetz enthält die reservierten IP-Adressen, die von den Diensten des virtuellen Netzwerkgateways verwendet werden. Verwenden Sie die folgenden Beispiele, um ein Gatewaysubnetz hinzufügen und festzulegen:

Fügen Sie das Gatewaysubnetz hinzu.

```azurepowershell-interactive
$getvnet = Get-AzVirtualNetwork -ResourceGroupName $RG1 -Name VNet1
Add-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.1.255.0/27 -VirtualNetwork $getvnet
```

Legen Sie die Konfiguration des Gatewaysubnetzes für das virtuelle Netzwerk fest.

```azurepowershell-interactive
$getvnet | Set-AzVirtualNetwork
```
## <a name="4-request-a-public-ip-address"></a><a name="publicip"></a>4. Anfordern einer öffentlichen IP-Adresse
 
In diesem Schritt folgen Sie den Anweisungen, die auf das von Ihnen erstellte Gateway zutreffen. Die Auswahl der Zonen, in denen die Gateways bereitgestellt werden, richtet sich nach den Zonen, die für die öffentliche IP-Adresse angegeben wurden.

### <a name="for-zone-redundant-gateways"></a><a name="ipzoneredundant"></a>Für zonenredundante Gateways

Fordern Sie eine öffentliche IP-Adresse mit einer PublicIpaddress-SKU vom Typ **Standard** an, und geben Sie keine Zone an. In diesem Fall handelt es sich bei der erstellten öffentlichen IP-Adresse vom Typ „Standard“ um eine zonenredundante öffentliche IP-Adresse.   

```azurepowershell-interactive
$pip1 = New-AzPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard
```

### <a name="for-zonal-gateways"></a><a name="ipzonalgw"></a>Für zonenbasierte Gateways

Fordern Sie eine öffentliche IP-Adresse mit einer PublicIpaddress-SKU vom Typ **Standard** an. Geben Sie die Zone an (1, 2 oder 3). Alle Gatewayinstanzen werden in dieser Zone bereitgestellt.

```azurepowershell-interactive
$pip1 = New-AzPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard -Zone 1
```

### <a name="for-regional-gateways"></a><a name="ipregionalgw"></a>Für regionsbezogene Gateways

Fordern Sie eine öffentliche IP-Adresse mit einer PublicIpaddress-SKU vom Typ **Basic** an. In diesem Fall wird das Gateway als regionsbezogenes Gateway bereitgestellt und bietet keine in das Gateway integrierte Zonenredundanz. Die Gatewayinstanzen werden jeweils in beliebigen Zonen erstellt.

```azurepowershell-interactive
$pip1 = New-AzPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Dynamic -Sku Basic
```
## <a name="5-create-the-ip-configuration"></a><a name="gwipconfig"></a>5. Erstellen der IP-Konfiguration

```azurepowershell-interactive
$getvnet = Get-AzVirtualNetwork -ResourceGroupName $RG1 -Name $VNet1
$subnet = Get-AzVirtualNetworkSubnetConfig -Name $GwSubnet1 -VirtualNetwork $getvnet
$gwipconf1 = New-AzVirtualNetworkGatewayIpConfig -Name $GwIPConf1 -Subnet $subnet -PublicIpAddress $pip1
```

## <a name="6-create-the-gateway"></a><a name="gwconfig"></a>6. Erstellen des Gateways

Erstellen Sie das virtuelle Netzwerkgateway.

### <a name="for-expressroute"></a>Für ExpressRoute

```azurepowershell-interactive
New-AzVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType ExpressRoute -GatewaySku ErGw1AZ
```

### <a name="for-vpn-gateway"></a>Für VPN Gateway

```azurepowershell-interactive
New-AzVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku VpnGw1AZ
```

## <a name="faq"></a><a name="faq"></a>Häufig gestellte Fragen

### <a name="what-will-change-when-i-deploy-these-new-skus"></a>Was ändert sich durch die Bereitstellung dieser neuen SKUs?

Aus Ihrer Sicht können Sie Ihre Gateways mit Zonenredundanz bereitstellen. Das bedeutet, dass alle Instanzen der Gateways in Azure-Verfügbarkeitszonen bereitgestellt werden und jede Verfügbarkeitszone eine eigene Fehler- und Updatedomäne darstellt. Dadurch werden Ihre Gateways zuverlässiger, verfügbarer und robuster bei Zonenausfällen.

### <a name="can-i-use-the-azure-portal"></a>Kann ich das Azure-Portal verwenden?

Ja, Sie können die neuen SKUs über das Azure-Portal bereitstellen. Allerdings werden diese neuen SKUs nur in den Azure-Regionen angezeigt, die Azure-Verfügbarkeitszonen haben.

### <a name="what-regions-are-available-for-me-to-use-the-new-skus"></a>In welchen Regionen stehen die neuen SKUs zur Verfügung?

Die aktuelle Liste der verfügbaren Regionen finden Sie unter [Verfügbarkeitszonen](../availability-zones/az-region.md).

### <a name="can-i-changemigrateupgrade-my-existing-virtual-network-gateways-to-zone-redundant-or-zonal-gateways"></a>Kann ich meine vorhandenen Gateways für virtuelle Netzwerke in zonenredundante oder zonenbasierte Gateways ändern/migrieren/aktualisieren?

Die Migration vorhandener Gateways für virtuelle Netzwerke zu zonenredundanten oder zonenbasierten Gateways wird derzeit nicht unterstützt. Sie können jedoch Ihr vorhandenes Gateway löschen und ein neues zonenredundantes oder zonenbasiertes Gateway erstellen.

### <a name="can-i-deploy-both-vpn-and-express-route-gateways-in-same-virtual-network"></a>Kann ich VPN- und ExpressRoute-Gateways im gleichen virtuellen Netzwerk bereitstellen?

Die Bereitstellung von VPN- und ExpressRoute-Gateways im gleichen virtuellen Netzwerk wird unterstützt. Sie sollten jedoch den IP-Adressbereich „/27“ für das Gatewaysubnetz reservieren.
