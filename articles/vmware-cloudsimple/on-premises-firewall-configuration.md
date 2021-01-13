---
title: Lokaler Zugriff auf die Azure VMware-Lösung von CloudSimple
titleSuffix: Azure VMware Solution by CloudSimple
description: Zugreifen auf Azure VMware Solution by CloudSimple aus Ihrem lokalen Netzwerk über eine Firewall
author: sharaths-cs
ms.author: dikamath
ms.date: 08/08/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 6ff057d99e29c7c6fe30e77f38a0bff265dbe7bf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86998889"
---
# <a name="accessing-your-cloudsimple-private-cloud-environment-and-applications-from-on-premises"></a>Lokales Zugreifen auf Ihre private CloudSimple-Cloudumgebung und darin enthaltene Anwendungen

Eine Verbindung von einem lokalen Netzwerk mit CloudSimple kann mithilfe von Azure ExpressRoute oder über Site-to-Site-VPN eingerichtet werden.  Greifen Sie über die Verbindung in Ihrer privaten CloudSimple-Cloud auf vCenter und die in der privaten Cloud ausgeführten Workloads zu.  Sie können mithilfe einer Firewall in Ihrem lokalen Netzwerk steuern, welche Ports für die Verbindung geöffnet werden.  In diesem Artikel werden einige der Portanforderungen für typische Anwendungen erläutert.  Für andere Anwendungen finden Sie Informationen zu den Portanforderungen in der Anwendungsdokumentation.

## <a name="ports-required-for-accessing-vcenter"></a>Für den Zugriff auf vCenter erforderliche Ports

Für den Zugriff auf vCenter und NSX-T-Manager in Ihrer privaten Cloud müssen die in der nachstehenden Tabelle definierten Ports in der lokalen Firewall geöffnet sein.  

| Port       | `Source`                           | Destination                      | Zweck                                                                                                                |
|------------|----------------------------------|----------------------------------|------------------------------------------------------------------------------------------------------------------------|
| 53 (UDP)   | Lokale DNS-Server          | DNS-Server in der privaten Cloud        | Erforderlich für die Weiterleitung der DNS-Suche von *az.cloudsimple.io* an DNS-Server in der privaten Cloud aus dem lokalen Netzwerk.       |
| 53 (UDP)   | DNS-Server in der privaten Cloud        | Lokale DNS-Server          | Erforderlich für die Weiterleitung der DNS-Suche von lokalen Domänennamen aus vCenter in der privaten Cloud an lokale DNS-Server. |
| 80 (TCP)   | Lokales Netzwerk              | Verwaltungsnetzwerk für die private Cloud | Erforderlich für die Umleitung der vCenter-URL von *http* zu *https*.                                                           |
| 443 (TCP)  | Lokales Netzwerk              | Verwaltungsnetzwerk für die private Cloud | Erforderlich für den Zugriff auf vCenter und NSX-T-Manager aus dem lokalen Netzwerk.                                             |
| 8000 (TCP) | Lokales Netzwerk              | Verwaltungsnetzwerk für die private Cloud | Erforderlich für VMotion für virtuelle Computer aus der lokalen Umgebung in die private Cloud.                                            |
| 8000 (TCP) | Verwaltungsnetzwerk für die private Cloud | Lokales Netzwerk              | Erforderlich für VMotion für virtuelle Computer aus der privaten Cloud in die lokale Umgebung.                                            |

## <a name="ports-required-for-using-on-premises-active-directory-as-an-identity-source"></a>Erforderliche Ports für die Verwendung des lokalen Active Directory als Identitätsquelle

Um das lokale Active Directory als Identitätsquelle für vCenter in der privaten Cloud zu konfigurieren, müssen die in der Tabelle definierten Ports geöffnet sein.  Konfigurationsschritte finden Sie unter [Verwenden von Azure AD als Identitätsanbieter für vCenter in privaten CloudSimple-Clouds](./azure-ad.md).

| Port         | `Source`                           | Destination                                         | Zweck                                                                                                                                          |
|--------------|----------------------------------|-----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| 53 (UDP)      | DNS-Server in der privaten Cloud        | Lokale DNS-Server                             | Erforderlich für die Weiterleitung der DNS-Suche von lokalen Active Directory-Domänennamen aus vCenter in der privaten Cloud an lokale DNS-Server.          |
| 389 (TCP/UDP) | Verwaltungsnetzwerk für die private Cloud | Lokale Active Directory-Domänencontroller     | Erforderlich für die LDAP-Kommunikation vom vCenter-Server in der privaten Cloud mit Active Directory-Domänencontrollern für die Benutzerauthentifizierung.                |
| 636 (TCP)     | Verwaltungsnetzwerk für die private Cloud | Lokale Active Directory-Domänencontroller     | Erforderlich für die Secure LDAP-Kommunikation (LDAPS) vom vCenter-Server in der privaten Cloud mit Active Directory-Domänencontrollern für die Benutzerauthentifizierung. |
| 3268 (TCP)    | Verwaltungsnetzwerk für die private Cloud | Globale Katalogserver für das lokale Active Directory | Erforderlich für die LDAP-Kommunikation in Bereitstellungen mit mehreren Domänencontrollern.                                                                        |
| 3269 (TCP)    | Verwaltungsnetzwerk für die private Cloud | Globale Katalogserver für das lokale Active Directory | Erforderlich für die LDAPS-Kommunikation in Bereitstellungen mit mehreren Domänencontrollern.                                                                       |                                           |

## <a name="common-ports-required-for-accessing-workload-virtual-machines"></a>Allgemeine erforderliche Ports für den Zugriff auf virtuelle Computer für Workloads

Zum Zugreifen auf virtuelle Computer für Workloads, die in der privaten Cloud ausgeführt werden, müssen Ports in der lokalen Firewall geöffnet sein.  In der nachstehenden Tabelle sind einige der erforderlichen allgemeinen Ports mit ihrem Zweck aufgeführt.  Die spezifischen Portanforderungen aller Anwendungen finden Sie in der Anwendungsdokumentation.

| Port         | `Source`                         | Destination                          | Zweck                                                                              |
|--------------|--------------------------------|--------------------------------------|--------------------------------------------------------------------------------------|
| 22 (TCP)      | Lokales Netzwerk            | Netzwerk für die Workload in der privaten Cloud       | Secure Shell-Zugriff auf virtuelle Linux-Computer, die in der privaten Cloud ausgeführt werden.              |
| 3389 (TCP)    | Lokales Netzwerk            | Netzwerk für die Workload in der privaten Cloud       | Remotedesktopzugriff auf virtuelle Windows-Computer, die in der privaten Cloud ausgeführt werden.                 |
| 80 (TCP)      | Lokales Netzwerk            | Netzwerk für die Workload in der privaten Cloud       | Zugriff auf Webserver, die auf virtuellen Computern in der privaten Cloud bereitgestellt werden.        |
| 443 (TCP)     | Lokales Netzwerk            | Netzwerk für die Workload in der privaten Cloud       | Zugriff auf sichere Webserver, die auf virtuellen Computern in der privaten Cloud bereitgestellt werden. |
| 389 (TCP/UDP) | Netzwerk für die Workload in der privaten Cloud | Lokales Active Directory-Netzwerk | Einbinden von virtuellen Windows-Computern für Workloads in die lokale Active Directory-Domäne.       |
| 53 (UDP)      | Netzwerk für die Workload in der privaten Cloud | Lokales Netzwerk                  | Zugriff des DNS-Diensts für virtuelle Computer für Workloads auf lokale DNS-Server.         |

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen und Verwalten von VLANs und Subnetzen](./create-vlan-subnet.md)
* [Connect from on-premises to CloudSimple using ExpressRoute](./on-premises-connection.md)
* [Set up VPN gateways on CloudSimple network](./vpn-gateway.md)
