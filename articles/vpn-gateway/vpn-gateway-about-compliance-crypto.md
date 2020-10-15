---
title: 'Azure-VPN Gateway: Kryptografische Anforderungen'
description: Hier erfahren Sie, wie Sie Azure-VPN-Gateways so konfigurieren, dass kryptografische Anforderungen für standortübergreifende S2S-VPN-Tunnel und VNET-zu-VNET-Verbindungen in Azure erfüllt werden.
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: article
ms.date: 01/10/2020
ms.author: yushwang
ms.openlocfilehash: 499d184d373b896ba7bffcf990693ef5e6ac466b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88032423"
---
# <a name="about-cryptographic-requirements-and-azure-vpn-gateways"></a>Kryptografische Anforderungen und Azure-VPN-Gateways

In diesem Artikel wird erläutert, wie Sie Azure-VPN-Gateways so konfigurieren können, dass Ihre kryptografischen Anforderungen für standortübergreifende S2S-VPN-Tunnel und VNET-zu-VNET-Verbindungen in Azure erfüllt werden.

## <a name="about-ikev1-and-ikev2-for-azure-vpn-connections"></a>Informationen zu IKEv1 und IKEv2 für Azure-VPN-Verbindungen

Normalerweise sind IKEv1-Verbindungen nur für Basic-SKUs und IKEv2-Verbindungen für alle VPN Gateway-SKUs außer Basic-SKUs zulässig. Die Basic-SKUs erlauben nur eine Verbindung. Neben anderen Einschränkungen, z. B. der Leistung, verfügen Kunden, die Legacygeräte verwenden, die nur IKEv1-Protokolle unterstützen, über eine eingeschränkte Funktionalität. IKEv1-Verbindungen sind nun für alle VPN Gateway-SKUs mit Ausnahme der Basic-SKU zulässig, wodurch die Funktionalität für Kunden verbessert wird, die IKEv1-Protokolle verwenden. Weitere Informationen finden Sie unter [VPN Gateway-SKUs](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings#gwsku).

![IKEv1- und IKEv2-Verbindungen von Azure-VPN-Gateways](./media/vpn-gateway-about-compliance-crypto/ikev1-ikev2-connections.png)

Wenn IKEv1- und IKEv2-Verbindungen für das gleiche VPN-Gateway verwendet werden, wird die Übertragung zwischen diesen beiden Verbindungen automatisch aktiviert.

## <a name="about-ipsec-and-ike-policy-parameters-for-azure-vpn-gateways"></a>IPsec- und IKE-Richtlinienparameter für Azure-VPN-Gateways

Der IPsec- und IKE-Protokollstandard unterstützt ein breites Spektrum von Kryptografiealgorithmen in verschiedenen Kombinationen. Wenn keine bestimmte Kombination aus Kryptografiealgorithmen und Parametern erforderlich ist, verwenden Azure-VPN-Gateways einen Satz von Standardvorschlägen. Die Standardrichtliniensätze wurden so gewählt, dass maximale Interoperabilität mit einer Vielzahl von VPN-Geräten von Drittanbietern in Standardkonfigurationen gewährleistet ist. Somit können die Richtlinien und zahlreichen Vorschläge nicht alle möglichen Kombinationen aus verfügbaren Kryptografiealgorithmen und Schlüsselstärken abdecken.

Der Standardrichtliniensatz für Azure-VPN-Gateways ist im Artikel [Informationen zu VPN-Geräten und IPsec-/IKE-Parametern für VPN-Gatewayverbindungen zwischen Standorten](vpn-gateway-about-vpn-devices.md) aufgeführt.

## <a name="cryptographic-requirements"></a>Kryptografische Anforderungen

Für Kommunikationsverbindungen, die spezielle Kryptografiealgorithmen oder Parameter erfordern, was normalerweise auf Konformitäts- oder Sicherheitsanforderungen zurückzuführen ist, können Sie ihre Azure-VPN-Gateways jetzt so konfigurieren, dass anstelle der Azure-Standardrichtliniensätze eine benutzerdefinierte IPsec/IKE-Richtlinie mit bestimmten Kryptografiealgorithmen und Schlüsselstärken verwendet wird.

Beispielsweise nutzen die IKEv2-Hauptmodusrichtlinien für Azure-VPN-Gateways nur Diffie-Hellman-Gruppe 2 (1024 Bit), während Sie möglicherweise stärkere Gruppen zur Verwendung in IKE angeben möchten, z.B. Gruppe 14 (2048 Bit), Gruppe 24 (2048-Bit-MODP-Gruppe) oder ECP (Gruppe für elliptische Kurve) 256 oder 384 Bit (Gruppe 19 bzw. Gruppe 20). Ähnliche Anforderungen gelten auch für IPsec-Schnellmodusrichtlinien.

## <a name="custom-ipsecike-policy-with-azure-vpn-gateways"></a>Benutzerdefinierte IPsec/IKE-Richtlinie mit Azure-VPN-Gateways

Azure-VPN-Gateways unterstützen jetzt eine benutzerdefinierte IPsec/IKE-Richtlinie pro Verbindung. Für eine Standort-zu-Standort- oder eine VNET-zu-VNET-Verbindung können Sie eine bestimmte Kombination aus Kryptografiealgorithmen für IPsec und IKE mit der gewünschten Schlüsselstärke auswählen, wie im folgenden Beispiel gezeigt:

![IPsec/IKE-Richtlinie](./media/vpn-gateway-about-compliance-crypto/ipsecikepolicy.png)

Sie können eine IPsec/IKE-Richtlinie erstellen und auf eine neue oder vorhandene Verbindung anwenden.

### <a name="workflow"></a>Workflow

1. Erstellen Sie die virtuellen Netzwerke, VPN-Gateways oder lokalen Netzwerkgateways für Ihre Netzwerktopologie entsprechend den Beschreibungen in anderen Anleitungen.
2. Erstellen Sie eine IPsec/IKE-Richtlinie.
3. Sie können die Richtlinie anwenden, wenn Sie eine S2S- oder VNET-zu-VNET-Verbindung erstellen.
4. Wenn die Verbindung bereits erstellt wurde, können Sie die Richtlinie für eine vorhandene Verbindung anwenden oder aktualisieren.

## <a name="ipsecike-policy-faq"></a>Häufig gestellte Fragen zur IPsec/IKE-Richtlinie

[!INCLUDE [vpn-gateway-ipsecikepolicy-faq-include](../../includes/vpn-gateway-faq-ipsecikepolicy-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Ausführliche Anweisungen zum Konfigurieren einer benutzerdefinierten IPsec/IKE-Richtlinie für eine Verbindung finden Sie unter [Konfigurieren einer IPsec/IKE-Richtlinie](vpn-gateway-ipsecikepolicy-rm-powershell.md).

Weitere Informationen zur Option UsePolicyBasedTrafficSelectors finden Sie unter [Verbinden mehrerer richtlinienbasierter VPN-Geräte](vpn-gateway-connect-multiple-policybased-rm-ps.md).
