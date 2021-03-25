---
title: Verwenden von Azure ExpressRoute zur Unterstützung von Remotebenutzern
description: Auf dieser Seite wird beschrieben, wie Sie Azure ExpressRoute nutzen können, um Remotearbeit aufgrund der COVID-19-Pandemie zu ermöglichen.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 03/22/2020
ms.author: duau
ms.openlocfilehash: d51f47b73fe572ce81d3e7b54f902f94fcd11b5b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "89567672"
---
# <a name="using-azure-expressroute-to-create-hybrid-connectivity-to-support-remote-users"></a>Verwenden von Azure ExpressRoute, um Hybridkonnektivität für Remotebenutzer bereitzustellen

In diesem Artikel wird beschrieben, wie Sie ExpressRoute, Azure, das Microsoft-Netzwerk und das Azure-Partnerökosystem nutzen können, um remote zu arbeiten.

## <a name="connecting-to-azure-services-with-expressroute"></a>Herstellen einer Verbindung mit Azure-Diensten mit ExpressRoute

>[!NOTE]
>In diesem Artikel wird beschrieben, wie Sie ExpressRoute, Azure, das Microsoft-Netzwerk und das Azure-Partnerökosystem nutzen können, um remote zu arbeiten und Netzwerkprobleme zu beheben, die Sie aufgrund der COVID-19-Krise bewältigen müssen.
>

[ExpressRoute](expressroute-introduction.md) ist ein Azure-Dienst, der private Verbindungen zwischen Microsoft-Datencentern und der Infrastruktur bei Ihnen vor Ort oder in einer Kollokationsumgebung ermöglicht. ExpressRoute-Verbindungen verlaufen nicht über das öffentliche Internet. Sie bieten sichere Verbindungen, größere Zuverlässigkeit und höhere Geschwindigkeiten bei konstanter Latenz als herkömmliche Verbindungen über das Internet.

## <a name="useful-links"></a>Nützliche Links

* [Erhöhen der Bandbreite für vorhandene ExpressRoute-Verbindung](expressroute-howto-circuit-portal-resource-manager.md#modify)
* [ExpressRoute-Überwachung, Metriken und Warnungen](expressroute-monitoring-metrics-alerts.md#expressroute-gateway-connections-in-bitsseconds)
* [Routenoptimierung über ExpressRoute](expressroute-optimize-routing.md)
* [Azure ExpressRoute für Microsoft 365](/microsoft-365/enterprise/azure-expressroute)
* [Überlegungen zum asymmetrischen Routing](expressroute-asymmetric-routing.md)
* [Öffnen einer Supportanfrage im Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)

## <a name="troubleshoot"></a>Problembehandlung

* [Überprüfen der ExpressRoute-Konnektivität](expressroute-troubleshooting-expressroute-overview.md)
* Abrufen einer ARP-Tabelle in [Resource Manager](expressroute-troubleshooting-arp-resource-manager.md) und [Classic](expressroute-troubleshooting-arp-classic.md)
* [Zurücksetzen einer fehlerhaften Verbindung](reset-circuit.md)
* [Behandeln von Problemen mit der Netzwerkleistung](expressroute-troubleshooting-network-performance.md)

## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zu ExpressRoute-Konnektivitätsmodellen](expressroute-connectivity-models.md)
* Informieren Sie sich über ExpressRoute-Verbindungen und Routingdomänen. Siehe [ExpressRoute-Verbindungen und Routingdomänen](expressroute-circuit-peerings.md).
* Suchen Sie nach einem Service Provider. Siehe [ExpressRoute-Partner und Peeringstandorte](expressroute-locations.md).
* Stellen Sie sicher, dass alle Voraussetzungen erfüllt sind. Informationen finden Sie unter [ExpressRoute-Voraussetzungen](expressroute-prerequisites.md).
* [Erstellen und Ändern einer ExpressRoute-Verbindung](expressroute-howto-circuit-portal-resource-manager.md)
* [Erstellen und Ändern des Peerings für eine ExpressRoute-Verbindung](expressroute-howto-routing-portal-resource-manager.md)
* [Verbinden eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-portal-resource-manager.md)
