---
title: 'Azure ExpressRoute: Beispiele für die Routerkonfiguration – NAT'
description: Diese Seite enthält Konfigurationsbeispiele für Router von Cisco und Juniper.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: article
ms.date: 01/07/2021
ms.author: duau
ms.openlocfilehash: ae0a39d65bf0f1bc5221cd5e46493c489f7630f8
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98012664"
---
# <a name="router-configuration-samples-to-set-up-and-manage-nat"></a>Beispiele für die Routerkonfiguration zum Einrichten und Verwalten von NAT

Dieser Artikel enthält NAT-Konfigurationsbeispiele für Router der Serien Cisco ASA und Juniper SRX bei Verwendung mit ExpressRoute. Diese Routerkonfigurationen sind nur Beispiele und dürfen nicht wie angegeben verwendet werden. Wenden Sie sich an Ihren Anbieter, um geeignete Konfigurationen für Ihr Netzwerk zu erhalten.

> [!IMPORTANT]
> Die Beispiele auf dieser Seite sind ausschließlich als Leitfaden konzipiert. Sie müssen mit dem Vertriebsteam/Technikteam Ihres Anbieters und Ihrem Netzwerkteam zusammenarbeiten, um für Ihre Anforderungen geeignete Konfigurationen zu bestimmen. Microsoft bietet keine Unterstützung bei Problemen im Zusammenhang mit Konfigurationen, die auf dieser Seite aufgeführt sind. Sie müssen sich bei Problemen an den Gerätehersteller wenden.
> 
> 

* Die nachstehenden Beispiele für die Routerkonfiguration gelten für Azure Public- und Microsoft-Peerings. NAT wird nicht für privates Peering in Azure konfiguriert. Unter [ExpressRoute-Peerings](expressroute-circuit-peerings.md) und [NAT-Anforderungen für ExpressRoute](expressroute-nat.md) finden Sie weitere Details.

* Sie MÜSSEN separate NAT-IP-Adresspools für Konnektivität mit dem Internet und ExpressRoute verwenden. Das Verwenden des gleichen NAT-IP-Adresspools für Internet und ExpressRoute führt zu einem asymmetrischen Routing und Verlust der Konnektivität.


## <a name="cisco-asa-firewalls"></a>Cisco ASA-Firewalls
### <a name="pat-configuration-for-traffic-from-customer-network-to-microsoft"></a>PAT-Konfiguration für den Datenverkehr von Kundennetzwerk zu Microsoft

```console
object network MSFT-PAT
  range <SNAT-START-IP> <SNAT-END-IP>


object-group network MSFT-Range
  network-object <IP> <Subnet_Mask>

object-group network on-prem-range-1
  network-object <IP> <Subnet-Mask>

object-group network on-prem-range-2
  network-object <IP> <Subnet-Mask>

object-group network on-prem
  network-object object on-prem-range-1
  network-object object on-prem-range-2

nat (outside,inside) source dynamic on-prem pat-pool MSFT-PAT destination static MSFT-Range MSFT-Range
```

### <a name="pat-configuration-for-traffic-from-microsoft-to-customer-network"></a>PAT-Konfiguration für den Datenverkehr von Microsoft zum Kundennetzwerk

**Schnittstellen und Richtung:**

Quellschnittstelle (bei der der Datenverkehr in die ASA gelangt): innerhalb Zielschnittstelle (bei der der Datenverkehr die ASA verlässt): außerhalb

**Konfiguration:**

NAT-Pool:

```console
object network outbound-PAT
    host <NAT-IP>
```

Zielserver:

```console
object network Customer-Network
    network-object <IP> <Subnet-Mask>
```

Objektgruppe für Kunden-IP-Adressen:

```console
object-group network MSFT-Network-1
    network-object <MSFT-IP> <Subnet-Mask>

object-group network MSFT-PAT-Networks
    network-object object MSFT-Network-1
```

NAT-Befehle:

```console
nat (inside,outside) source dynamic MSFT-PAT-Networks pat-pool outbound-PAT destination static Customer-Network Customer-Network
```


## <a name="juniper-srx-series-routers"></a>Router der Juniper SRX-Serie
### <a name="1-create-redundant-ethernet-interfaces-for-the-cluster"></a>1. Erstellen redundanter Ethernet-Schnittstellen für den Cluster

```console
    interfaces {
        reth0 {
            description "To Internal Network";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 1;
            }
            unit 100 {
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
        reth1 {
            description "To Microsoft via Edge Router";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 2;
            }
            unit 100 {
                description "To Microsoft via Edge Router";
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
    }
```

### <a name="2-create-two-security-zones"></a>2. Erstellen von zwei Sicherheitszonen
* Richten Sie eine Trust Zone für das interne Netzwerk und eine Untrust Zone für Edgerouter zum externen Netzwerk ein.
* Weisen Sie den Zonen entsprechende Schnittstellen zu.
* Lassen Sie Dienste an den Schnittstellen zu.

```console
    security {
        zones {
            security-zone Trust {
                host-inbound-traffic {
                    system-services {
                        ping;
                    }
                    protocols {
                        bgp;
                    }
                }
                interfaces {
                    reth0.100;
                }
            }
            security-zone Untrust {
                host-inbound-traffic {
                    system-services {
                        ping;
                    }
                    protocols {
                        bgp;
                    }
                }
                interfaces {
                    reth1.100;
                }
            }
        }
    }
```


### <a name="3-create-security-policies-between-zones"></a>3. Erstellen von Sicherheitsrichtlinien zwischen Zeitzonen

```console
    security {
        policies {
            from-zone Trust to-zone Untrust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
            from-zone Untrust to-zone Trust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
        }
    }
```

### <a name="4-configure-nat-policies"></a>4. Konfigurieren von NAT-Richtlinien
* Erstellen Sie zwei NAT-Pools. Einer dient für an Microsoft ausgehenden NAT-Datenverkehr, der andere für NAT-Datenverkehr von Microsoft zum Kunden.
* Erstellen Sie Regeln für die NAT des jeweiligen Datenverkehrs.

```console
       security {
           nat {
               source {
                   pool SNAT-To-ExpressRoute {
                       routing-instance {
                           External-ExpressRoute;
                       }
                       address {
                           <NAT-IP-address/Subnet-mask>;
                       }
                   }
                   pool SNAT-From-ExpressRoute {
                       routing-instance {
                           Internal;
                       }
                       address {
                           <NAT-IP-address/Subnet-mask>;
                       }
                   }
                   rule-set Outbound_NAT {
                       from routing-instance Internal;
                       to routing-instance External-ExpressRoute;
                       rule SNAT-Out {
                           match {
                               source-address 0.0.0.0/0;
                           }
                           then {
                               source-nat {
                                   pool {
                                       SNAT-To-ExpressRoute;
                                   }
                               }
                           }
                       }
                   }
                   rule-set Inbound-NAT {
                       from routing-instance External-ExpressRoute;
                       to routing-instance Internal;
                       rule SNAT-In {
                           match {
                               source-address 0.0.0.0/0;
                           }
                           then {
                               source-nat {
                                   pool {
                                       SNAT-From-ExpressRoute;
                                   }
                               }
                           }
                       }
                   }
               }
           }
       }
```

### <a name="5-configure-bgp-to-advertise-selective-prefixes-in-each-direction"></a>5. Konfigurieren von BGP zum Ankündigen selektiver Namespacepräfixe in jede Richtung
Beispiele finden Sie auf der Seite [Beispiele für Routingkonfiguration ](expressroute-config-samples-routing.md).

### <a name="6-create-policies"></a>6. Erstellen von Richtlinien

```console
    routing-options {
                  autonomous-system <Customer-ASN>;
    }
    policy-options {
        prefix-list Microsoft-Prefixes {
            <IP-Address/Subnet-Mask;
            <IP-Address/Subnet-Mask;
        }
        prefix-list private-ranges {
            10.0.0.0/8;
            172.16.0.0/12;
            192.168.0.0/16;
            100.64.0.0/10;
        }
        policy-statement Advertise-NAT-Pools {
            from {
                protocol static;
                route-filter <NAT-Pool-Address/Subnet-mask> prefix-length-range /32-/32;
            }
            then accept;
        }
        policy-statement Accept-from-Microsoft {
            term 1 {
                from {
                    instance External-ExpressRoute;
                    prefix-list-filter Microsoft-Prefixes orlonger;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
        policy-statement Accept-from-Internal {
            term no-private {
                from {
                    instance Internal;
                    prefix-list-filter private-ranges orlonger;
                }
                then reject;
            }
            term bgp {
                from {
                    instance Internal;
                    protocol bgp;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
    }
    routing-instances {
        Internal {
            instance-type virtual-router;
            interface reth0.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Microsoft;
            }
            protocols {
                bgp {
                    group customer {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-ASN-1>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
        External-ExpressRoute {
            instance-type virtual-router;
            interface reth1.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Internal;
            }
            protocols {
                bgp {
                    group edge-router {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-Public-ASN>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
    }
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [ExpressRoute – FAQ](expressroute-faqs.md).

