---
title: 'Azure ExpressRoute: Konfigurieren von BFD'
description: Dieser Artikel enthält Anweisungen zum Konfigurieren von BFD (Bidirectional Forwarding Detection) über das private Peering einer ExpressRoute-Leitung.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: article
ms.date: 11/1/2018
ms.author: duau
ms.openlocfilehash: fd1cad4031d83fd0e17286bfaabb77aa746b646a
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92202326"
---
# <a name="configure-bfd-over-expressroute"></a>Konfigurieren von BFD über ExpressRoute

ExpressRoute unterstützt Bidirectional Forwarding Detection (BFD) sowohl über privates als auch Microsoft-Peering. Durch die Aktivierung von BFD über ExpressRoute können Sie die Erkennung von Verbindungsfehlern zwischen MSEE-Geräten (Microsoft Enterprise Edge) und den Routern beschleunigen, auf denen Sie die ExpressRoute-Leitung (CE/PE) beenden. Sie können ExpressRoute Customer-Edge-Routinggeräte oder Partner Edge-Routinggeräte beenden (wenn Sie den verwalteten Layer 3-Verbindungsdienst verwendet haben). In diesem Dokument werden die Notwendigkeit von BFD und die Vorgehensweise zur Aktivierung von BFD über ExpressRoute erläutert.

## <a name="need-for-bfd"></a>Notwendigkeit von BFD

Das folgende Diagramm zeigt den Vorteil der Aktivierung von BFD über die ExpressRoute-Leitung: [![1]][1]

Sie können die ExpressRoute-Leitung entweder über Layer 2-Verbindungen oder über verwaltete Layer 3-Verbindungen aktivieren. In beiden Fällen ist das darüber liegende BGP für die Erkennung von Verbindungsfehlern in dem Pfad verantwortlich, wenn sich im ExpressRoute-Verbindungspfad mehrere Layer 2-Geräte befinden.

Auf den MSEE-Geräten sind keepalive und hold-time im BGP in der Regel für eine Dauer von 60 bzw. 180 Sekunden konfiguriert. Folglich würde es nach Auftreten eines Verbindungsfehlers bis zu drei Minuten dauern, um den Verbindungsfehler zu erkennen und den Datenverkehr über eine alternative Verbindung weiterzuleiten.

Sie können die BGP-Zeitgeber steuern, indem Sie für keepalive und hold-time im BGP auf dem Customer Edge-Peeringgerät niedrigere Werte konfigurieren. Wenn die BGP-Zeitgeber zwischen den beiden Peeringgeräten nicht übereinstimmen, würden in der BGP-Sitzung niedrigere Zeitgeberwerte zwischen den Peers verwendet. Der Wert für keepalive im BGP kann auf nur drei Sekunden festgelegt werden und der Wert für hold-time in der Größenordnung auf mehr als zehn Sekunden. Das dynamische Festlegen von BGP-Zeitgebern wird jedoch weniger bevorzugt, da das Protokoll prozessintensiv ist.

In diesem Szenario kann BFD helfen. Mit BFD können Verbindungsfehler mit geringem Mehraufwand in einem Zeitintervall von unter 1 Sekunde erkannt werden. 


## <a name="enabling-bfd"></a>Aktivieren von BFD

BFD wird standardmäßig unter allen neu erstellten privaten ExpressRoute-Peeringschnittstellen auf den MSEE-Geräten konfiguriert. Daher müssen Sie zum Aktivieren von BFD nur auf Ihren CEs/PEs (sowohl auf den primären als auch den sekundären Geräten) BFD konfigurieren. BFD wird in zwei Schritten konfiguriert: Sie müssen BFD in der Schnittstelle konfigurieren und anschließend mit der BGP-Sitzung verknüpfen.

Eine CE/PE-Beispielkonfiguration (unter Verwendung von Cisco IOS XE) ist unten dargestellt. 

```console
interface TenGigabitEthernet2/0/0.150
   description private peering to Azure
   encapsulation dot1Q 15 second-dot1q 150
   ip vrf forwarding 15
   ip address 192.168.15.17 255.255.255.252
   bfd interval 300 min_rx 300 multiplier 3


router bgp 65020
   address-family ipv4 vrf 15
      network 10.1.15.0 mask 255.255.255.128
      neighbor 192.168.15.18 remote-as 12076
      neighbor 192.168.15.18 fall-over bfd
      neighbor 192.168.15.18 activate
      neighbor 192.168.15.18 soft-reconfiguration inbound
   exit-address-family
```

>[!NOTE]
>Zum Aktivieren von BFD unter einem bereits vorhandenen privaten Peering müssen Sie das Peering zurücksetzen. Informationen dazu finden Sie unter [Zurücksetzen von ExpressRoute-Peerings][ResetPeering].
>

## <a name="bfd-timer-negotiation"></a>Aushandlung des BFD-Zeitgebers

Zwischen BFD-Peers bestimmt der langsamere der beiden Peers die Übertragungsrate. BFD-Übertragung auf MSEE-Geräten/Empfangsintervalle werden auf 300 Millisekunden festgelegt. In bestimmten Szenarien kann das Intervall auf einen höheren Wert von 750 Millisekunden festgelegt werden. Durch das Konfigurieren höherer Werte können Sie erzwingen, dass diese Intervalle länger sind; kürzere Intervalle können jedoch nicht erzwungen werden.

>[!NOTE]
>Wenn Sie georedundante ExpressRoute-Leitungen konfiguriert haben oder IPSec-VPN-Konnektivität zwischen Standorten als Sicherung verwenden; durch die Aktivierung von BFD könnte der Failover nach einem ExpressRoute-Konnektivitätsfehler schneller erfolgen. 
>

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen oder Hilfe finden Sie unter den folgenden Links:

- [Erstellen und Ändern einer ExpressRoute-Verbindung][CreateCircuit]
- [Erstellen und Ändern des Routings für eine ExpressRoute-Verbindung][CreatePeering]

<!--Image References-->
[1]: ./media/expressroute-bfd/BFD_Need.png "BFD beschleunigt die Zeit zur Erkennung von Verbindungsfehlern"

<!--Link References-->
[CreateCircuit]: ./expressroute-howto-circuit-portal-resource-manager.md
[CreatePeering]: ./expressroute-howto-routing-portal-resource-manager.md
[ResetPeering]: ./expressroute-howto-reset-peering.md