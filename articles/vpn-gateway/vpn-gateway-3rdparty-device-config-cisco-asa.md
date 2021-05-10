---
title: Beispielkonfiguration für das Verbinden von Cisco ASA-Geräten mit VPN-Gateways
titleSuffix: Azure VPN Gateway
description: Anzeigen von Beispielkonfigurationen für das Verbinden von Cisco ASA-Geräten mit Azure VPN-Gateways.
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 04/29/2021
ms.author: yushwang
ms.openlocfilehash: c9de069b657f991444d84f5bff5f61d1150c0b0f
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108292142"
---
# <a name="sample-configuration-cisco-asa-device-ikev2no-bgp"></a>Beispielkonfiguration: Cisco ASA-Gerät (IKEv2/no BGP)
Dieser Artikel enthält Beispielkonfigurationen für das Verbinden von Cisco ASA-Geräten (Adaptive Security Appliance) mit Azure-VPN-Gateways. Das Beispiel gilt für Cisco ASA-Geräte, auf denen IKEv2 ohne Border Gateway Protocol (BGP) ausgeführt wird. 

## <a name="device-at-a-glance"></a>Gerät auf einen Blick

* Gerätehersteller: **Cisco**
* Gerätemodell: **ASA**           
* Zielversion: **8.4 und höher**
* Getestetes Modell: **ASA 5505**
* Getestete Version: **9.2**             
* IKE-Version: **IKEv2**                  
* BGP: **Nein**      
* Typ des Azure VPN-Gateways: **Routenbasiertes VPN-Gateway**

> [!NOTE]
> Mit der Beispielkonfiguration wird ein Cisco ASA-Gerät mit einem **routenbasierten** Azure-VPN-Gateway verbunden. Die Verbindung verwendet eine benutzerdefinierte IPsec-/IKE-Richtlinie mit der **UsePolicyBasedTrafficSelectors**-Option wie in [diesem Artikel](vpn-gateway-connect-multiple-policybased-rm-ps.md) beschrieben.
>
> Für das Beispiel müssen die ASA-Geräte die **IKEv2**-Richtlinie mit zugriffslistenbasierten und nicht mit VTI-basierten Konfigurationen verwenden. Vergewissern Sie sich in den Spezifikationen Ihres VPN-Geräteanbieters, dass die IKEv2-Richtlinie von Ihren lokalen VPN-Geräten unterstützt wird.


## <a name="vpn-device-requirements"></a>Anforderungen an das VPN-Gerät
Azure-VPN-Gateways verwenden für den Aufbau von Site-to-Site-VPN-Tunneln (S2S) Standard-IPsec/IKE-Protokollsammlungen. Die ausführlichen IPsec-/IKE-Protokollparameter und die standardmäßigen Kryptoalgorithmen für Azure-VPN-Gateways finden Sie unter [Informationen zu VPN-Geräten](vpn-gateway-about-vpn-devices.md).

> [!NOTE]
> Sie können die genaue Kombination der Kryptoalgorithmen und Schlüsselstärken für eine spezifische Verbindung angeben. Dies ist optional und wird unter [Informationen zu kryptografischen Anforderungen](vpn-gateway-about-compliance-crypto.md) beschrieben. Wenn Sie eine spezifische Kombination aus Algorithmen und Schlüsselstärken angeben, müssen Sie unbedingt die entsprechenden Spezifikationen auf Ihren VPN-Geräten verwenden.

## <a name="single-vpn-tunnel"></a>Einmaliger VPN-Tunnel
Diese Konfiguration besteht aus einem einmaligen Site-to-Site-VPN-Tunnel (S2S) zwischen dem Azure-VPN-Gateway und einem lokalen VPN-Gerät. Als Option können Sie im VPN-Tunnel BGP konfigurieren.

![Einmaliger S2S-VPN-Tunnel](./media/vpn-gateway-3rdparty-device-config-cisco-asa/singletunnel.png)

Eine ausführliche Anleitung für den Aufbau der Azure-Konfigurationen finden Sie unter [Einrichten eines einmaligen VPN-Tunnels](vpn-gateway-3rdparty-device-config-overview.md#singletunnel).

### <a name="virtual-network-and-vpn-gateway-information"></a>Informationen zu virtuellen Netzwerken und VPN-Gateways
In diesem Abschnitt werden die Parameter für dieses Beispiel aufgeführt.

| **Parameter**                | **Wert**                    |
| ---                          | ---                          |
| Adresspräfixe des virtuellen Netzwerks        | 10.11.0.0/16<br>10.12.0.0/16 |
| IP-Adresse des Azure-VPN-Gateways         | Azure_Gateway_Public_IP      |
| Lokale Adresspräfixe | 10.51.0.0/16<br>10.52.0.0/16 |
| IP-Adresse des lokalen VPN-Geräts    | OnPrem_Device_Public_IP     |
| * BGP-ASN des virtuellen Netzwerks                | 65010                        |
| * IP-Adresse des Azure-BGP-Peers           | 10.12.255.30                 |
| * Lokale BGP-ASN         | 65050                        |
| * IP-Adresse des lokalen BGP-Peers     | 10.52.255.254                |
|                              |                              |

\* Optionale Parameter nur für BGP

### <a name="ipsecike-policy-and-parameters"></a>IPsec/IKE-Richtlinie und -Parameter
In der folgenden Tabelle werden die im Beispiel verwendeten IPsec/IKE-Algorithmen und -Parameter aufgeführt. Überprüfen Sie anhand Ihrer VPN-Gerätespezifikationen, ob alle oben aufgelisteten Algorithmen von Ihren VPN-Gerätemodellen und Firmwareversionen unterstützt werden.

| **IPsec/IKEv2**  | **Wert**                            |
| ---              | ---                                  |
| IKEv2-Verschlüsselung | AES256                               |
| IKEv2-Integrität  | SHA384                               |
| DH-Gruppe         | DHGroup24                            |
| * IPsec-Verschlüsselung | AES256                               |
| * IPsec-Integrität  | SHA1                                 |
| PFS-Gruppe        | PFS24                                |
| QM-SA-Gültigkeitsdauer   | 7\.200 Sekunden                         |
| Datenverkehrsselektor | UsePolicyBasedTrafficSelectors $True |
| Vorab ausgetauschter Schlüssel   | PreSharedKey                         |
|                  |                                      |

\* Auf einigen Geräten muss die IPsec-Integrität ein Nullwert sein, wenn der IPsec-Verschlüsselungsalgorithmus AES-GCM ist.

### <a name="asa-device-support"></a>ASA-Geräteunterstützung

* IKEv2-Unterstützung setzt mindestens die ASA-Version 8.4 voraus.

* Eine höhere DH- und PFS-Gruppenunterstützung (höher als Gruppe 5) setzt mindestens ASA-Version 9.x voraus.

* Unterstützung für IPsec-Verschlüsselung mit AES-GCM und IPsec-Integrität per SHA-256, SHA-384 oder SHA-512 setzt ASA-Version 9.x voraus. Diese Unterstützungsanforderung gilt für neuere ASA-Geräte. Zum Zeitpunkt der Veröffentlichung werden diese Algorithmen von folgenden ASA-Modellen nicht unterstützt: 5505, 5510 5520, 5540, 5550 und 5580. Überprüfen Sie anhand Ihrer VPN-Gerätespezifikationen, ob alle oben aufgelisteten Algorithmen von Ihren VPN-Gerätemodellen und Firmwareversionen unterstützt werden.


### <a name="sample-device-configuration"></a>Beispielgerätekonfiguration
Das Skript enthält ein Beispiel, das auf der Konfiguration und den Parametern basiert, die in den vorherigen Abschnitten beschrieben wurden. Die Standort-zu-Standort-VPN-Tunnelkonfiguration besteht aus folgenden Teilen:

1. Schnittstellen und Routen
2. Zugriffslisten
3. IKE-Richtlinie und -Parameter (Phase 1 oder Hauptmodus)
4. IPsec-Richtlinien und -Parameter (Phase 2 oder Quickmodus)
5. Sonstige Parameter (z.B. TCP-MSS-Clamping)

> [!IMPORTANT]
> Führen Sie vor der Verwendung des Beispielskripts die folgenden Schritte aus. Ersetzen Sie die Platzhalterwerte im Skript durch die Geräteeinstellungen für Ihre Konfiguration.

* Geben Sie die Schnittstellenkonfiguration für innerhalb und außerhalb von Schnittstellen an.
* Geben Sie die Routen für Ihre internen/privaten und externen/öffentlichen Netzwerke an.
* Stellen Sie sicher, dass alle Namen und Richtliniennummern auf dem Gerät eindeutig sind.
* Stellen Sie sicher, dass die Kryptografiealgorithmen auf Ihrem Gerät unterstützt werden.
* Ersetzen Sie die folgenden **Platzhalterwerte** durch die tatsächlichen Werte Ihrer Konfiguration:
  - Name der Schnittstelle nach außen: **outside**
  - **Azure_Gateway_Public_IP**
  - **OnPrem_Device_Public_IP**
  - IKE: **Pre_Shared_Key**
  - Namen der VNET- und lokalen Netzwerkgateways: **VNetName** und **LNGName**
  - VNET- und lokale Netzwerkadresspräfixe: **prefixes**
  - Ordnungsgemäße Netzmasken: **netmasks**

#### <a name="sample-script"></a>Beispielskript

```
! Sample ASA configuration for connecting to Azure VPN gateway
!
! Tested hardware: ASA 5505
! Tested version:  ASA version 9.2(4)
!
! Replace the following place holders with your actual values:
!   - Interface names - default are "outside" and "inside"
!   - <Azure_Gateway_Public_IP>
!   - <OnPrem_Device_Public_IP>
!   - <Pre_Shared_Key>
!   - <VNetName>*
!   - <LNGName>* ==> LocalNetworkGateway - the Azure resource that represents the
!     on-premises network, specifies network prefixes, device public IP, BGP info, etc.
!   - <PrivateIPAddress> ==> Replace it with a private IP address if applicable
!   - <Netmask> ==> Replace it with appropriate netmasks
!   - <Nexthop> ==> Replace it with the actual nexthop IP address
!
! (*) Must be unique names in the device configuration
!
! ==> Interface & route configurations
!
!     > <OnPrem_Device_Public_IP> address on the outside interface or vlan
!     > <PrivateIPAddress> on the inside interface or vlan; e.g., 10.51.0.1/24
!     > Route to connect to <Azure_Gateway_Public_IP> address
!
!     > Example:
!
!       interface Ethernet0/0
!        switchport access vlan 2
!       exit
!
!       interface vlan 1
!        nameif inside
!        security-level 100
!        ip address <PrivateIPAddress> <Netmask>
!       exit
!
!       interface vlan 2
!        nameif outside
!        security-level 0
!        ip address <OnPrem_Device_Public_IP> <Netmask>
!       exit
!
!       route outside 0.0.0.0 0.0.0.0 <NextHop IP> 1
!
! ==> Access lists
!
!     > Most firewall devices deny all traffic by default. Create access lists to
!       (1) Allow S2S VPN tunnels between the ASA and the Azure gateway public IP address
!       (2) Construct traffic selectors as part of IPsec policy or proposal
!
access-list outside_access_in extended permit ip host <Azure_Gateway_Public_IP> host <OnPrem_Device_Public_IP>
!
!     > Object group that consists of all VNet prefixes (e.g., 10.11.0.0/16 &
!       10.12.0.0/16)
!
object-group network Azure-<VNetName>
 description Azure virtual network <VNetName> prefixes
 network-object 10.11.0.0 255.255.0.0
 network-object 10.12.0.0 255.255.0.0
exit
!
!     > Object group that corresponding to the <LNGName> prefixes.
!       E.g., 10.51.0.0/16 and 10.52.0.0/16. Note that LNG = "local network gateway".
!       In Azure network resource, a local network gateway defines the on-premises
!       network properties (address prefixes, VPN device IP, BGP ASN, etc.)
!
object-group network <LNGName>
 description On-Premises network <LNGName> prefixes
 network-object 10.51.0.0 255.255.0.0
 network-object 10.52.0.0 255.255.0.0
exit
!
!     > Specify the access-list between the Azure VNet and your on-premises network.
!       This access list defines the IPsec SA traffic selectors.
!
access-list Azure-<VNetName>-acl extended permit ip object-group <LNGName> object-group Azure-<VNetName>
!
!     > No NAT required between the on-premises network and Azure VNet
!
nat (inside,outside) source static <LNGName> <LNGName> destination static Azure-<VNetName> Azure-<VNetName>
!
! ==> IKEv2 configuration
!
!     > General IKEv2 configuration - enable IKEv2 for VPN
!
group-policy DfltGrpPolicy attributes
 vpn-tunnel-protocol ikev1 ikev2
exit
!
crypto isakmp identity address
crypto ikev2 enable outside
!
!     > Define IKEv2 Phase 1/Main Mode policy
!       - Make sure the policy number is not used
!       - integrity and prf must be the same
!       - DH group 14 and above require ASA version 9.x.
!
crypto ikev2 policy 1
 encryption       aes-256
 integrity        sha384
 prf              sha384
 group            24
 lifetime seconds 86400
exit
!
!     > Set connection type and pre-shared key
!
tunnel-group <Azure_Gateway_Public_IP> type ipsec-l2l
tunnel-group <Azure_Gateway_Public_IP> ipsec-attributes
 ikev2 remote-authentication pre-shared-key <Pre_Shared_Key> 
 ikev2 local-authentication  pre-shared-key <Pre_Shared_Key> 
exit
!
! ==> IPsec configuration
!
!     > IKEv2 Phase 2/Quick Mode proposal
!       - AES-GCM and SHA-2 requires ASA version 9.x on newer ASA models. ASA
!         5505, 5510, 5520, 5540, 5550, 5580 are not supported.
!       - ESP integrity must be null if AES-GCM is configured as ESP encryption
!
crypto ipsec ikev2 ipsec-proposal AES-256
 protocol esp encryption aes-256
 protocol esp integrity  sha-1
exit
!
!     > Set access list & traffic selectors, PFS, IPsec proposal, SA lifetime
!       - This sample uses "Azure-<VNetName>-map" as the crypto map name
!       - ASA supports only one crypto map per interface, if you already have
!         an existing crypto map assigned to your outside interface, you must use
!         the same crypto map name, but with a different sequence number for
!         this policy
!       - "match address" policy uses the access-list "Azure-<VNetName>-acl" defined 
!         previously
!       - "ipsec-proposal" uses the proposal "AES-256" defined previously 
!       - PFS groups 14 and beyond requires ASA version 9.x.
!
crypto map Azure-<VNetName>-map 1 match address Azure-<VNetName>-acl
crypto map Azure-<VNetName>-map 1 set pfs group24
crypto map Azure-<VNetName>-map 1 set peer <Azure_Gateway_Public_IP>
crypto map Azure-<VNetName>-map 1 set ikev2 ipsec-proposal AES-256
crypto map Azure-<VNetName>-map 1 set security-association lifetime seconds 7200
crypto map Azure-<VNetName>-map interface outside
!
! ==> Set TCP MSS to 1350
!
sysopt connection tcpmss 1350
!
```

## <a name="simple-debugging-commands"></a>Einfache Befehle zum Debuggen

Verwenden Sie zum Debuggen die folgenden ASA-Befehle:

* Anzeigen der IPsec- oder IKE-Sicherheitszuordnung (SA):
    ```
    show crypto ipsec sa
    show crypto ikev2 sa
    ```

* Wechseln in den Debugmodus:
    ```
    debug crypto ikev2 platform <level>
    debug crypto ikev2 protocol <level>
    ```
    Die `debug`-Befehle können umfangreiche Ausgaben an der Konsole generieren.

* Anzeigen der aktuellen Konfigurationen auf dem Gerät:
    ```
    show run
    ```
    Verwenden Sie die Unterbefehle von `show`, um bestimmte Teile der Gerätekonfiguration aufzulisten, z.B.:
    ```
    show run crypto
    show run access-list
    show run tunnel-group
    ```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Konfigurieren standortübergreifender Aktiv/aktiv- und VNET-zu-VNET-Verbindungen finden Sie unter [Konfigurieren von Aktiv/aktiv-VPN-Verbindungen](vpn-gateway-activeactive-rm-powershell.md).
