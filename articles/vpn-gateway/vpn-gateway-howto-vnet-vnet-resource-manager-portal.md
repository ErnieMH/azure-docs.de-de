---
title: 'Konfigurieren einer VNET-zu-VNET-VPN-Gatewayverbindung: Azure-Portal'
description: Erstellen Sie eine VPN Gateway-Verbindung zwischen VNets unter Verwendung von Resource Manager und Azure-Portal.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/03/2020
ms.author: cherylmc
ms.openlocfilehash: 12ab54e51b26c4b3d77f5518d4e4f44efe34affd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90983307"
---
# <a name="configure-a-vnet-to-vnet-vpn-gateway-connection-by-using-the-azure-portal"></a>Konfigurieren einer VNET-zu-VNET-VPN-Gatewayverbindung über das Azure-Portal

In diesem Artikel erfahren Sie, wie Sie zwischen virtuellen Netzwerken (VNETs) eine VNET-zu-VNET-Verbindung herstellen. Virtuelle Netzwerke können sich in verschiedenen Regionen befinden und zu verschiedenen Abonnements gehören. Beim Verbinden von VNETs aus unterschiedlichen Abonnements müssen die Abonnements nicht demselben Active Directory-Mandanten zugeordnet sein. 

![v2v-Diagramm](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

Die Schritte in diesem Artikel gelten für das Azure Resource Manager-Bereitstellungsmodell und verwenden das Azure-Portal. Sie können diese Konfiguration mit einem anderen Bereitstellungstool oder Modell erstellen, indem Sie Optionen verwenden, die in den folgenden Artikeln beschrieben werden:

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
> * [Azure-Befehlszeilenschnittstelle](vpn-gateway-howto-vnet-vnet-cli.md)
> * [Azure-Portal (klassisch)](vpn-gateway-howto-vnet-vnet-portal-classic.md)
> * [Verbinden von verschiedenen Bereitstellungsmodellen – Azure-Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [Verbinden von virtuellen Netzwerken aus verschiedenen Bereitstellungsmodellen mit PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
>
>

## <a name="about-connecting-vnets"></a>Informationen zum Verbinden von VNETs

In den folgenden Abschnitten werden die unterschiedlichen Verbindungsmethoden für virtuelle Netzwerke beschrieben.

### <a name="vnet-to-vnet"></a>VNet-zu-VNet

Durch Konfigurieren einer VNET-zu-VNet-Verbindung können Sie sehr einfach eine Verbindung zwischen VNets herstellen. Wenn Sie eine VNET-zu-VNET-Verbindung (VNET2VNET) zwischen zwei virtuellen Netzwerken herstellen, ist dies vergleichbar mit dem Herstellen einer Site-to-Site-IPsec-Verbindung mit einem lokalen Standort. Bei beiden Verbindungstypen wird ein VPN-Gateway verwendet, um per IPsec/IKE einen sicheren Tunnel zu erstellen, und auch die Kommunikation läuft jeweils gleich ab. Jedoch unterscheiden sie sich in der Art, in der das lokale Netzwerkgateway konfiguriert wird. 

Beim Herstellen einer VNET-zu-VNET-Verbindung wird der Adressraum des lokalen Netzwerkgateways automatisch erstellt und gefüllt. Wenn Sie den Adressraum für ein VNET aktualisieren, leitet das andere VNET den Datenverkehr automatisch an den aktualisierten Adressraum weiter. Es ist in der Regel schneller und einfacher, eine VNET-zu-VNET-Verbindung herzustellen, als eine Site-to-Site-Verbindung.

### <a name="site-to-site-ipsec"></a>Site-to-Site (IPsec)

Wenn Sie mit einer komplizierten Netzwerkkonfiguration arbeiten, empfiehlt es sich unter Umständen, die Verbindung zwischen Ihren VNETs stattdessen mit einer [Site-to-Site-Verbindung](vpn-gateway-howto-site-to-site-resource-manager-portal.md) herzustellen. Bei Verwendung der Site-to-Site-Schritte (IPsec) erstellen und konfigurieren Sie die lokalen Netzwerkgateways manuell. Das lokale Netzwerkgateway für die einzelnen VNets behandelt das andere VNet als lokalen Standort. Mit diesen Schritten können Sie zusätzliche Adressräume für das lokale Netzwerkgateway angeben, um Datenverkehr weiterzuleiten. Wenn sich der Adressraum für ein VNET ändert, müssen Sie das dazugehörige lokale Netzwerkgateway manuell aktualisieren.

### <a name="vnet-peering"></a>VNet-Peering

Sie können Ihre VNETs auch per VNET-Peering verbinden. Beim VNET-Peering wird kein VPN-Gateway verwendet, und es gelten andere Einschränkungen. Außerdem werden die [Preise für VNet-Peering](https://azure.microsoft.com/pricing/details/virtual-network) anders berechnet als die [Preise für VPN Gateway (VNet-zu-VNet)](https://azure.microsoft.com/pricing/details/vpn-gateway). Weitere Informationen finden Sie unter [VNet-Peering](../virtual-network/virtual-network-peering-overview.md).

## <a name="why-create-a-vnet-to-vnet-connection"></a>Gründe für das Herstellen einer VNET-zu-VNET-Verbindung

Folgende Gründe sprechen für das Herstellen einer VNET-zu-VNET-Verbindung zwischen virtuellen Netzwerken:

### <a name="cross-region-geo-redundancy-and-geo-presence"></a>Regionsübergreifende Georedundanz und Geopräsenz

  * Sie können Ihre eigene Georeplikation oder -synchronisierung mit sicheren Verbindungen einrichten, ohne Endpunkte mit Internetzugriff verwenden zu müssen.
  * Mit Azure Traffic Manager und Azure Load Balancer können Sie eine hoch verfügbare Workload mit Georedundanz über mehrere Azure-Regionen hinweg einrichten. Sie können z.B. SQL Server Always On-Verfügbarkeitsgruppen über mehrere Azure-Regionen hinweg einrichten.

### <a name="regional-multi-tier-applications-with-isolation-or-administrative-boundaries"></a>Regionale Anwendungen mit mehreren Ebenen und Isolation oder Verwaltungsgrenzen

  * In derselben Region können Sie Anwendungen mit mehreren Ebenen und mehreren virtuellen Netzwerken einrichten, die aufgrund von Isolations- oder Verwaltungsanforderungen miteinander verbunden sind.

Die VNET-zu-VNET-Kommunikation kann mit Konfigurationen für mehrere Standorte kombiniert werden. Mit diesen Konfigurationen können Sie Netzwerktopologien einrichten, die wie in der folgenden Abbildung dargestellt standortübergreifende Konnektivität mit Konnektivität zwischen virtuellen Netzwerken kombinieren:

![Informationen zu Verbindungen](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Informationen zu Verbindungen")

In diesem Artikel wird beschrieben, wie Sie mit dem Verbindungstyp „VNET-zu-VNET“ VNETs verbinden. Sie können die folgenden Beispieleinstellungswerte nutzen, wenn Sie diese Schritte als Übung verwenden. Im Beispiel befinden sich die virtuellen Netzwerke im selben Abonnement, aber in verschiedenen Ressourcengruppen. Wenn sich Ihre VNets in unterschiedlichen Abonnements befinden, können Sie die Verbindung nicht im Portal herstellen. Sie können [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) oder stattdessen die [CLI](vpn-gateway-howto-vnet-vnet-cli.md) verwenden. Weitere Informationen zu VNET-zu-VNET-Verbindungen finden Sie unter [Häufig gestellte Fragen zu VNET-zu-VNET-Verbindungen](#vnet-to-vnet-faq).

### <a name="example-settings"></a>Beispieleinstellungen

**Werte für VNet1:**

- **Einstellungen für das virtuelle Netzwerk**
    - **Name**: VNet1
    - **Adressraum:** 10.1.0.0/16
    - **Abonnement**: Wählen Sie das Abonnement aus, das Sie verwenden möchten.
    - **Ressourcengruppe**: TestRG1
    - **Standort**: East US
    - **Subnetz**
        - **Name**: FrontEnd
        - **Adressbereich**: 10.1.0.0/24
    - **Gatewaysubnetz**:
        - **Name**: *GatewaySubnet* wird automatisch eingetragen
        - **Adressbereich**: 10.1.255.0/27

- **Einstellungen des Gateways für virtuelle Netzwerke**
    - **Name**: VNet1GW
    - **Gatewaytyp**: Wählen Sie **VPN** aus.
    - **VPN-Typ:** Wählen Sie **Routenbasiert** aus.
    - **SKU**: Wählen Sie die gewünschte Gateway-SKU aus.
    - **Öffentliche IP-Adresse**: VNet1GWpip
    - **Connection**
       - **Name**: VNet1toVNet4
       - **Gemeinsam verwendeter Schlüssel**: Sie können den gemeinsam verwendeten Schlüssel selbst erstellen. Wenn Sie die Verbindung zwischen den VNETs herstellen, müssen die Werte übereinstimmen. Verwenden Sie für diese Übung abc123.

**Werte für VNet4:**

- **Einstellungen für das virtuelle Netzwerk**
   - **Name**: VNet4
   - **Adressraum:** 10.41.0.0/16
   - **Abonnement**: Wählen Sie das Abonnement aus, das Sie verwenden möchten.
   - **Ressourcengruppe**: TestRG4
   - **Standort**: USA (Westen)
   - **Subnetz** 
      - **Name**: FrontEnd
      - **Adressbereich**: 10.41.0.0/24
   - **GatewaySubnet** 
      - **Name**: *GatewaySubnet* wird automatisch eingetragen
      - **Adressbereich**: 10.41.255.0/27

- **Einstellungen des Gateways für virtuelle Netzwerke** 
    - **Name**: VNet4GW
    - **Gatewaytyp**: Wählen Sie **VPN** aus.
    - **VPN-Typ:** Wählen Sie **Routenbasiert** aus.
    - **SKU**: Wählen Sie die gewünschte Gateway-SKU aus.
    - **Öffentliche IP-Adresse**: VNet4GWpip
    - **Connection** 
       - **Name**: VNet4toVNet1
       - **Gemeinsam verwendeter Schlüssel**: Sie können den gemeinsam verwendeten Schlüssel selbst erstellen. Wenn Sie die Verbindung zwischen den VNETs herstellen, müssen die Werte übereinstimmen. Verwenden Sie für diese Übung abc123.

## <a name="create-and-configure-vnet1"></a>Erstellen und Konfigurieren von VNet1
Wenn Sie bereits über ein VNet verfügen, sollten Sie überprüfen, ob die Einstellungen mit Ihrem Entwurf des VPN Gateways kompatibel sind. Achten Sie besonders auf Subnetze, die sich unter Umständen mit anderen Netzwerken überlappen. Bei überlappenden Subnetzen funktioniert die Verbindung nicht einwandfrei.

### <a name="to-create-a-virtual-network"></a>So erstellen Sie ein virtuelles Netzwerk
[!INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="create-the-vnet1-gateway"></a>Erstellen des Net1-Gateways
In diesem Schritt erstellen Sie das virtuelle Netzwerkgateway für Ihr VNet. Häufig kann die Erstellung eines Gateways je nach ausgewählter Gateway-SKU mindestens 45 Minuten dauern. Falls Sie diese Konfiguration zu Übungszwecken erstellen, können Sie die [Beispieleinstellungen](#example-settings) verwenden.

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-portal-include.md)]

### <a name="to-create-a-virtual-network-gateway"></a>So erstellen Sie ein Gateway für das virtuelle Netzwerk
[!INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

## <a name="create-and-configure-vnet4"></a>Erstellen und Konfigurieren von VNet4
Nachdem Sie VNet1 konfiguriert haben, erstellen Sie VNet4 und das VNet4-Gateway, indem Sie die vorherigen Schritte wiederholen und die Werte durch VNet4-Werte ersetzen. Sie können mit dem Konfigurieren von VNet4 beginnen, auch wenn die Erstellung des virtuellen Netzwerkgateways für VNet1 noch nicht abgeschlossen ist. Achten Sie bei Verwendung eigener Werte darauf, dass sich die Adressräume nicht mit anderen VNETs überschneiden, mit denen Sie eine Verbindung herstellen möchten.

## <a name="configure-the-vnet1-gateway-connection"></a>Konfigurieren der VNet1-Gatewayverbindung
Nach Abschluss der Vorgänge für die virtuellen Netzwerkgateways für VNet1 und VNet4 können Sie die Verbindungen für das virtuelle Netzwerkgateway erstellen. In diesem Abschnitt erstellen Sie eine Verbindung von „VNet1“ zu „VNet4“. Diese Schritte gelten nur für VNets in demselben Abonnement. Wenn sich Ihre VNETs in unterschiedlichen Abonnements befinden, müssen Sie [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) zum Herstellen der Verbindung verwenden. Wenn Ihre VNETs in verschiedenen Ressourcengruppen in demselben Abonnement enthalten sind, können Sie sie jedoch mithilfe des Portals verbinden.

1. Wählen Sie im Azure-Portal **Alle Ressourcen** aus, geben Sie *Gateway für virtuelle Netzwerke* in das Suchfeld ein, und navigieren Sie dann zum Gateway für virtuelle Netzwerke für Ihr VNET. Beispiel: **VNet1GW**. Wählen Sie das Gateway aus, um die Seite **Gateway für virtuelle Netzwerke** zu öffnen. Wählen Sie unter **Einstellungen** die Option **Verbindungen** aus.

   ![Seite „Verbindungen“](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connections.png "Seite „Verbindungen“")
2. Wählen Sie **+Hinzufügen** aus, um die Seite **Verbindung hinzufügen** zu öffnen.

   ![Verbindung hinzufügen](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/vnet1-vnet4-connection.png "Verbindung hinzufügen")
3. Geben Sie auf der Seite **Verbindung hinzufügen** die Werte zum Herstellen der Verbindung ein:

   - **Name**: Geben Sie einen Namen für Ihre Verbindung ein. Beispiel: *VNet1toVNet4*.

   - **Verbindungstyp**: Wählen Sie **VNET-zu-VNET** in der Dropdownliste aus.

   - **Erstes Gateway für virtuelle Netzwerke**: Der Wert des Felds wird automatisch ausgefüllt, da Sie diese Verbindung über das angegebene virtuelle Netzwerkgateway herstellen.

   - **Zweites Gateway für virtuelle Netzwerke**: Dieses Feld enthält das virtuelle Netzwerkgateway des VNET, mit dem Sie eine Verbindung herstellen möchten. Wählen Sie **Ein weiteres virtuelles Netzwerkgateway auswählen** aus, um die Seite **Ein Gateway für virtuelle Netzwerke auswählen** zu öffnen.

     - Sehen Sie sich die auf der Seite aufgeführten virtuellen Netzwerkgateways an. Beachten Sie, dass nur virtuelle Netzwerkgateways aus Ihrem Abonnement aufgeführt werden. Falls Sie eine Verbindung mit einem Gateway für virtuelle Netzwerke herstellen möchten, das nicht in Ihrem Abonnement enthalten ist, verwenden Sie [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md).

     - Wählen Sie das Gateway für virtuelle Netzwerke aus, mit dem Sie eine Verbindung herstellen möchten.

     - **Gemeinsam verwendeter Schlüssel (PSK)** : Geben Sie in diesem Feld einen gemeinsam verwendeten Schlüssel für die Verbindung ein. Diesen Schlüssel können Sie selbst generieren oder erstellen. Bei einer Site-to-Site-Verbindung wird für Ihr lokales Gerät und für Ihre Verbindung mit dem Gateway für virtuelle Netzwerke der gleiche Schlüssel verwendet. Das Konzept ist hier ähnlich, nur dass diesmal keine Verbindung mit einem VPN-Gerät hergestellt wird, sondern mit einem anderen Gateway für virtuelle Netzwerke.
    
4. Klicken Sie zum Speichern der Änderungen auf **OK** .

## <a name="configure-the-vnet4-gateway-connection"></a>Konfigurieren der VNet4-Gatewayverbindung
Erstellen Sie als Nächstes eine Verbindung zwischen VNet4 und VNet1. Ermitteln Sie im Portal das mit VNet4 verbundene virtuelle Netzwerkgateway. Führen Sie die Schritte im vorherigen Abschnitt aus, und ersetzen Sie dabei die Werte, um eine Verbindung zwischen VNet4 und VNet1 zu erstellen. Verwenden Sie den gleichen gemeinsam verwendeten Schlüssel.

## <a name="verify-your-connections"></a>Überprüfen der Verbindungen

1. Suchen Sie im Azure-Portal nach dem Gateway für virtuelle Netzwerke. 
2. Wählen Sie auf der Seite **Gateway für virtuelle Netzwerke** **Verbindungen** aus, um die Seite **Verbindungen** für das Gateway für virtuelle Netzwerke anzuzeigen. Nach dem Verbindungsaufbau sehen Sie, dass sich die Werte für **Status** in **Verbunden** ändern.

   ![Überprüfen von Verbindungen](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/view-connections.png "Überprüfen von Verbindungen")
3. Wählen Sie unter der Spalte **Name** eine der Verbindungen aus, um weitere Informationen anzuzeigen. Sobald Daten übertragen werden, werden Werte für **Eingehende Daten** und **Ausgehende Daten** angezeigt.

   ![Der Screenshot zeigt eine Ressourcengruppe, die Werte für „Eingehende Daten“ und „Ausgehende Daten“ enthält.](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/status.png "Status")

## <a name="add-additional-connections"></a>Hinzufügen zusätzlicher Verbindungen

Wenn Sie zusätzliche Verbindungen hinzufügen möchten, navigieren Sie zum Gateway für virtuelle Netzwerke, über das Sie die Verbindung herstellen möchten, und wählen Sie **Verbindungen** aus. Sie können eine weitere VNET-to-VNET-Verbindung oder eine IPsec-S2S-Verbindung mit einem lokalen Standort erstellen. Denken Sie daran, den **Verbindungstyp** entsprechend dem gewünschten Verbindungstyp einzustellen. Stellen Sie vor dem Erstellen weiterer Verbindungen sicher, dass sich der Adressraum für Ihr virtuelles Netzwerk nicht mit den Adressräumen überlappt, mit denen Sie eine Verbindung herstellen möchten. Anweisungen zum Erstellen einer S2S-Verbindung finden Sie unter [Erstellen einer Site-to-Site-Verbindung](vpn-gateway-howto-site-to-site-resource-manager-portal.md).

## <a name="vnet-to-vnet-faq"></a>FAQs zu VNet-zu-VNet
Zeigen Sie die Details zu den häufig gestellten Fragen an, um zusätzliche Informationen zu VNet-zu-VNet-Verbindungen zu erhalten.

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-faq-vnet-vnet-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Beschränken des Netzwerkdatenverkehrs auf Ressourcen in einem virtuellen Netzwerk finden Sie unter [Netzwerksicherheit](../virtual-network/security-overview.md).

Informationen zur Weiterleitung von Datenverkehr zwischen Azure, lokalen Speicherorten und Internetressourcen finden Sie unter [Routing von Datenverkehr für virtuelle Netzwerke](../virtual-network/virtual-networks-udr-overview.md).
