---
title: Herstellen einer Verbindung mit China über Azure Virtual WAN und einen sicheren Hub
description: Hier erfahren Sie, wie Sie eine Verbindung mit China über Azure Virtual WAN und einen geschützten Hub herstellen.
services: virtual-wan
author: skishen525
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: sukishen
ms.openlocfilehash: 83cc7757f31a631af755155b49c7c26753618426
ms.sourcegitcommit: 4313e0d13714559d67d51770b2b9b92e4b0cc629
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2020
ms.locfileid: "91399107"
---
# <a name="interconnect-with-china-using-azure-virtual-wan-and-secure-hub"></a>Herstellen einer Verbindung mit China über Azure Virtual WAN und einen sicheren Hub

Bei der Betrachtung der allgemeinen Automobil-, Fertigungs- und Logistikbranche sowie anderer Institutionen wie Botschaften stellt sich oft die Frage, wie die Verbindung mit China verbessert werden kann. Diese Verbesserungen sind hauptsächlich für die Verwendung von Clouddiensten wie Microsoft 365, Azure Global Services oder die Anbindung von Branches in China an einen Kundenbackbone relevant.

In den meisten Fällen kämpfen Kunden gegen hohe Latenz, geringe Bandbreite, instabile Verbindung und hohe Kosten bei Verbindungen von außen (z. B. von Europa oder den USA) nach China.

Ein Grund für diese Probleme ist die „große Firewall von China“, die den chinesischen Teil des Internets schützt und den Datenverkehr nach China filtert. Fast der gesamte Datenverkehr, der vom chinesischen Festland nach außen geht, mit Ausnahme von Sonderverwaltungszonen wie Hongkong und Macau, geht durch die große Firewall. Der Datenverkehr, der über Hongkong und Macau läuft, trifft nicht voll auf die große Firewall, sondern wird von einem Teil der großen Firewall verarbeitet.

![Verbindungen zwischen Anbietern](./media/interconnect-china/provider.png)

Mit Virtual WAN kann ein Kunde eine leistungsfähigere und stabilere Verbindung mit Microsoft Cloud Services und eine Verbindung mit dem Unternehmensnetzwerk herstellen, ohne gegen das chinesische Cybersicherheitsrecht zu verstoßen.

## <a name="requirements-and-workflow"></a><a name="requirements"></a>Anforderungen und Workflow

Um das chinesische Cybersicherheitsrecht zu befolgen, müssen mehrere festgelegte Bedingungen erfüllt werden.

Zunächst müssen Sie mit einem Netzwerkbetreiber und ISP zusammenarbeiten, der eine ICP-Lizenz (Internet Content Provider) für China besitzt. In den meisten Fällen landen Sie bei einem der folgenden Anbieter:

* China Telecom Global Ltd.
* China Mobile Ltd.
* China Unicom Ltd.
* PCCW Global Ltd.
* Hong Kong Telecom Ltd.

Abhängig vom Anbieter und Ihren Anforderungen müssen Sie nun einen der folgenden Netzwerkkonnektivitätsdienste erwerben, um ihre Branches in China miteinander zu verbinden.

* MPLS/IPVPN-Netzwerk 
* Softwaredefiniertes WAN (SDWAN)
* Dedizierter Internetzugang

Als nächstes müssen Sie mit diesem Anbieter eine Abzweigung zum globalen Netzwerk von Microsoft und dem zugehörigen Umkreisnetzwerk in Hongkong, nicht in Peking oder Schanghai, vereinbaren. In diesem Fall ist Hongkong wegen der physischen Verbindung und der Position in China sehr wichtig.

Obwohl die meisten Kunden denken, dass Singapur für die Verbindung am besten geeignet ist, da es bei der Betrachtung der Karte näher an China liegt, ist dies nicht richtig. Wenn Sie den Netzwerkglasfaserleitungen auf der Karte folgen, werden fast alle Netzwerkverbindungen über Peking, Schanghai und Hongkong geleitet. Deshalb ist Hongkong eine bessere Wahl für Verbindungen mit China.

Abhängig vom Anbieter können Sie unterschiedliche Dienstangebote erhalten. In der folgenden Tabelle finden Sie ein Beispiel für Anbieter und dem jeweils angebotenen Dienst, basierend auf den Informationen zum Zeitpunkt, zu dem dieser Artikel verfasst wurde.

| Dienst | Beispiele für Anbieter |
| --- | --- |
| MPLS/IPVPN-Netzwerk |PCCW, China Telecom Global |
|SDWAN| PCCW, China Telecom Global|
| Dedizierter Internetzugang | PCCW, Hong Kong Telecom, China Mobil|

Mit Ihrem Anbieter können Sie eine der beiden folgenden Lösungen vereinbaren, um den globalen Backbone von Microsoft zu erreichen:

* Errichten einer Microsoft Azure ExpressRoute-Verbindung, die in Hongkong endet. Dies wäre bei der Verwendung von MPLS/IPVPN der Fall. Derzeit ist China Telecom Global der einzige ICP-Lizenzanbieter mit ExpressRoute-Verbindungen nach Hongkong. Sie können jedoch auch andere Anbieter ansprechen, wenn diese Cloud Exchange-Anbieter wie Megaport oder InterCloud nutzen. Weitere Informationen finden Sie unter [ExpressRoute-Konnektivitätsanbieter](../expressroute/expressroute-locations-providers.md#partners).

* Verwenden eines dedizierten Internetzugangs direkt an einem der folgenden Internetknoten oder einer privaten Netzwerkverbindung.

In der folgenden Liste sind die in Hongkong möglichen Internetknoten aufgeführt:

* AMS-IX Hong Kong
* BBIX Hong Kong
* Equinix Hong Kong
* HKIX

Wenn Sie diese Verbindung verwenden, muss Ihr nächster BGP-Hop für Microsoft-Dienste die autonome Microsoft-Systemnummer (AS#) 8075 sein. Wenn Sie einen einzelnen Standort oder eine SDWAN-Lösung verwenden, wäre dies die optimale Verbindung.

In jedem Fall wird empfohlen, eine zweite und reguläre Internetabzweigung zum chinesischen Festland vorzuhalten. Dadurch wird erreicht, dass der Datenverkehr auf Unternehmensdatenverkehr zu Clouddiensten wie Microsoft 365 sowie Azure und Internetdatenverkehr, der gesetzlichen Vorschriften unterliegt, aufgeteilt wird.

Eine konforme Netzwerkarchitektur in China könnte wie im folgenden Beispiel aussehen:

![Mehrere Branches](./media/interconnect-china/multi-branch.png)

In diesem Beispiel mit einer Verbindung zum globalen Netzwerk von Microsoft in Hongkong können Sie nun die [Azure Virtual WAN Global Transit Architecture](virtual-wan-global-transit-network-architecture.md) und zusätzliche Dienste wie sichere Azure Virtual WAN-Hubs nutzen, um Dienste zu nutzen und Verbindungen zu Branches sowie Rechenzentren außerhalb von China herzustellen.

## <a name="hub-to-hub-communication"></a><a name="hub-to-hub"></a>Hub-zu-Hub-Kommunikation

In diesem Abschnitt wird die Virtual WAN-Hub-zu-Hub-Kommunikation verwendet, um eine Verbindung herzustellen. In diesem Szenario erstellen Sie eine neue Virtual WAN-Hub-Ressource für die Verbindung mit einem Virtual WAN-Hub in Hongkong, anderen bevorzugten Regionen, eine Region, in denen Sie bereits über Azure-Ressourcen verfügen, oder einen anderen gewünschten Verbindungsknoten.

Eine Beispielarchitektur könnte wie folgt aussehen:

![Beispiel-WAN](./media/interconnect-china/sample.png)

In diesem Beispiel stellen die Branches in China über VPN-oder MPLS-Verbindungen eine Verbindung mit Azure Cloud China her. Branches, die Verbindungen zu globalen Diensten benötigen, verwenden MPLS oder internetbasierte Dienste, die direkte mit Hongkong verbunden sind. Wenn Sie ExpressRoute sowohl in Hongkong als auch in der anderen Region verwenden möchten, müssen Sie [ExpressRoute Global Reach](../expressroute/expressroute-global-reach.md) konfigurieren, um beide ExpressRoute-Leitungen miteinander zu verbinden.

ExpressRoute Global Reach ist in einigen Regionen nicht verfügbar. Wenn Sie z. B. eine Verbindung mit Brasilien oder Indien herstellen müssen, müssen Sie [Cloud Exchange-Anbieter](../expressroute/expressroute-locations.md#connectivity-through-exchange-providers) nutzen, um die Routingdienste bereitzustellen.

Die folgende Abbildung zeigt beide Beispiele für dieses Szenario.

![Global Reach](./media/interconnect-china/global.png)

## <a name="secure-internet-breakout-for-microsoft-365"></a><a name="secure"></a>Sichere Internetabzweigung für Microsoft 365

Ein weiterer Aspekt ist die Netzwerksicherheit und die Protokollierung für den Einstiegspunkt zwischen China und der über Virtual WAN angebundenen Backbonekomponente und dem Kundenbackbone. In den meisten Fällen ist es erforderlich, eine Abzweigung zum Internet in Hongkong vorzusehen, um das Microsoft-Umkreisnetzwerk und so die Azure Front Door-Server, die für Microsoft 365-Dienste verwendet werden, direkt zu erreichen.

Für beide Szenarien mit Virtual WAN wird der [sichere Azure Virtual WAN-Hub](../firewall-manager/secured-virtual-hub.md) verwendet. Mit Azure Firewall Manager können Sie einen regulären Virtual WAN-Hub in einen sicheren Hub ändern und dann innerhalb dieses Hubs eine Azure Firewall bereitstellen und verwalten.

Die folgende Abbildung zeigt ein Beispiel für dieses Szenario:

![Internetabzweigung für Webdatenverkehr und Datenverkehr für Microsoft-Dienste](./media/interconnect-china/internet.png)

## <a name="architecture-and-traffic-flows"></a><a name="traffic"></a>Architektur und Datenverkehrsflüsse

Abhängig von Ihrer Auswahl für die Verbindung nach Hongkong kann sich die Gesamtarchitektur geringfügig ändern. In diesem Abschnitt werden drei verfügbare Architekturen mit unterschiedlichen Kombinationen von VPN oder SDWAN und/oder ExpressRoute gezeigt.

Alle beschriebenen Optionen verwenden den geschützten Azure Virtual WAN-Hub für die direkte Microsoft 365-Konnektivität in Hongkong. Diese Architekturen unterstützen auch die Complianceanforderungen für [Microsoft 365 Multi-Geo](/microsoft-365/enterprise/microsoft-365-multi-geo) und sorgen dafür, dass der Datenverkehr in der Nähe des nächsten Azure Front Door-Standorts bleibt. So wird auch eine Verbesserung bei der Verwendung von Microsoft 365 ausgehend von China erreicht.

Wenn Sie Azure Virtual WAN zusammen mit Internetverbindungen verwenden, kann jede Verbindung von zusätzlichen Diensten wie [Microsoft Azure Peering Services (MAPS)](https://docs.microsoft.com/azure/peering-service/about) profitieren. MAPS wurde erstellt, um den Datenverkehr zu optimieren, der von externen Internetdienstanbietern an das globale Netzwerk von Microsoft geht.

### <a name="option-1-sdwan-or-vpn"></a><a name="option-1"></a>Option 1: SDWAN oder VPN

In diesem Abschnitt wird ein Design erläutert, der ein SDWAN oder VPN nach Hongkong und zu anderen Branches verwendet. Diese Option veranschaulicht Nutzung und Datenverkehrsfluss bei Verwendung einer reinen Internetverbindung an beiden Standorten des Virtual WAN-Backbones. In diesem Fall erfolgt die Verbindung nach Hongkong über einen dedizierten Internetzugang oder eine SDWAN-Lösung eines ICP-Anbieters. Andere Branches verwenden ebenfalls reine Internet- oder SDWAN-Lösungen.

![Datenverkehr von China nach Hongkong](./media/interconnect-china/china-traffic.png)

In dieser Architektur ist jeder Standort über VPN und Azure Virtual WAN mit dem globalen Microsoft-Netzwerk verbunden. Der Datenverkehr zwischen den Standorten und Hongkong wird über das Microsoft-Netzwerk übertragen. Die reguläre Internetverbindung wird lediglich auf der letzten Meile verwendet.

### <a name="option-2-expressroute-and-sdwan-or-vpn"></a><a name="option-2"></a>Option 2: ExpressRoute und SDWAN oder VPN

In diesem Abschnitt wird ein Design erläutert, das ExpressRoute in Hongkong und anderen Branches mit VPN/SDWAN-Branches verwendet. Diese Option veranschaulicht die Verwendung einer ExpressRoute-Verbindung, die in Hongkong endet, und anderen Branches, die über SDWAN oder VPN verbunden sind. ExpressRoute in Hongkong ist zurzeit auf wenige Anbieter beschränkt, die Sie in der Liste der [ExpressRoute-Partner](../expressroute/expressroute-locations-providers.md#global-commercial-azure) finden.

![Datenverkehr von China nach Hongkong über ExpressRoute](./media/interconnect-china/expressroute.png)

Es gibt auch Optionen, die ExpressRoute-Verbindung aus China beispielsweise in Südkorea oder Japan enden zu lassen. In Anbetracht von Compliance, Vorschriften und Latenz ist Hongkong jedoch derzeit die beste Wahl.

### <a name="option-3-expressroute-only"></a><a name="option-3"></a>Option 3: Nur ExpressRoute

In diesem Abschnitt wird ein Design erläutert, in dem ExpressRoute für Hongkong und andere Branches verwendet wird. Diese Option veranschaulicht die Verwendung von ExpressRoute-Verbindungen an beiden Enden. Hier fließt der Datenverkehr anders als bei den anderen Optionen. Der Microsoft 365-Datenverkehr geht an den sicheren Azure Virtual WAN-Hub und von dort an das Microsoft-Umkreisnetzwerk und das Internet.

Der Datenverkehr, der an die verbundenen Branches oder von diesen an die Standorte in China geht, verwendet in dieser Architektur eine andere Strategie. Virtual WAN unterstützt zurzeit keine ExpressRoute-zu ExpressRoute-Übertragung. Der Datenverkehr nutzt ExpressRoute Global Reach oder die Verbindung eines Drittanbieters, ohne den virtuellen WAN-Hub zu durchlaufen. Er geht direkt von einem Microsoft Enterprise Edge-Knoten (MSEE) zum anderen.

![ExpressRoute Global Reach](./media/interconnect-china/expressroute-virtual.png)

Zurzeit ist ExpressRoute Global Reach nicht in jedem Land/jeder Region verfügbar, Sie können jedoch eine Lösung mit Azure Virtual WAN konfigurieren.

Sie können beispielsweise eine ExpressRoute-Verbindung mit Microsoft-Peering konfigurieren und über dieses Peering einen VPN-Tunnel mit Azure Virtual WAN verbinden. Nun ist wiederum eine Übertragung zwischen VPN und ExpressRoute ohne Global Reach und Drittanbieter und Dienst, z. B. Megaport Cloud, möglich.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in den folgenden Artikeln:

* [Architektur mit globalem Transitnetzwerk: Azure Virtual WAN](virtual-wan-global-transit-network-architecture.md)

* [Erstellen eines Virtual WAN-Hubs](virtual-wan-site-to-site-portal.md)

* [Konfigurieren eines sicherten Virtual WAN-Hubs](../firewall-manager/secure-cloud-network.md)

* [Azure Peering Service (Vorschauversion) – Übersicht](https://docs.microsoft.com/azure/peering-service/about)
