---
title: Wichtige Konzepte für die Verwaltung von Azure VMware Solution by CloudSimple
titleSuffix: Azure VMware Solution by CloudSimple
description: Hier werden wichtige Konzepte für die Verwaltung von Azure VMware Solution by CloudSimple beschrieben.
author: Ajayan1008
ms.author: v-hborys
ms.date: 04/24/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: e5544ef7725855d28e20d39ff345db6bb07671a2
ms.sourcegitcommit: d7d5f0da1dda786bda0260cf43bd4716e5bda08b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/05/2021
ms.locfileid: "97895324"
---
# <a name="key-concepts-for-administration-of-azure-vmware-solutions-by-cloudsimple"></a>Wichtige Konzepte für die Verwaltung von Azure VMware Solution by CloudSimple

Für die Verwaltung von Azure VMware Solution by CloudSimple müssen Sie mit den folgenden Konzepten vertraut sein:

* CloudSimple-Dienst, angezeigt als: Azure VMware Solution by CloudSimple – Dienst
* CloudSimple-Knoten, angezeigt als: Azure VMware Solution by CloudSimple – Knoten
* Private CloudSimple-Cloud
* Dienstnetzwerk
* Virtueller CloudSimple-Computer, angezeigt als: Azure VMware Solution by CloudSimple – Virtueller Computer

## <a name="cloudsimple-service"></a>CloudSimple-Dienst

Mit dem CloudSimple-Dienst können Sie sämtliche Ressourcen für VMware-Lösungen von CloudSimple über das Azure-Portal erstellen und verwalten. Erstellen Sie in jeder Region, in der Sie den Dienst verwenden möchten, eine Dienstressource.

Weitere Informationen zum CloudSimple-Dienst finden Sie [hier](cloudsimple-service.md).

## <a name="cloudsimple-node"></a>CloudSimple-Knoten

Bei einem CloudSimple-Knoten handelt es sich um einen dedizierten, hyperkonvergenten Bare-Metal-Compute- und Speicherhost, auf dem der VMware ESXi-Hypervisor bereitgestellt wird. Dieser Knoten wird dann in die VMware vSphere-, vCenter-, vSAN- und NSX-Plattform integriert. CloudSimple-Netzwerkdienste und Umkreisnetzwerkdienste werden ebenfalls aktiviert. Jeder Knoten fungiert als Compute- und Speicherkapazitätseinheit, die Sie zur Erstellung [privater CloudSimple-Clouds](cloudsimple-private-cloud.md) bereitstellen können. Sie stellen Knoten in einer Region bereit, in der der CloudSimple-Dienst verfügbar ist, oder reservieren sie.

Weitere Informationen zu CloudSimple-Knoten finden Sie [hier](cloudsimple-node.md).

## <a name="cloudsimple-private-cloud"></a>Private CloudSimple-Cloud

Eine private CloudSimple-Cloud ist eine isolierte VMware-Stapelumgebung, die von einem vCenter-Server in einer eigenen Verwaltungsdomäne verwaltet wird. Der VMware-Stapel beinhaltet ESXi-Hosts, vSphere, vCenter, vSAN und NSX. Der Stapel wird auf dedizierten Knoten (dedizierte und isolierte Bare-Metal-Hardware) ausgeführt und von Benutzern über native VMware-Tools (einschließlich vCenter und NSX Manager) genutzt. Dedizierte Knoten werden an Azure-Standorten bereitgestellt und von Azure verwaltet. Jede private Cloud kann mithilfe von Netzwerkdiensten wie VLANs und Subnetzen sowie Firewalltabellen segmentiert und geschützt werden. Für die Verbindungsherstellung mit Ihrer lokalen Umgebung und dem Azure-Netzwerk werden ein sicheres, privates VPN sowie Azure ExpressRoute-Verbindungen verwendet.

Weitere Informationen zur privaten CloudSimple-Cloud finden Sie [hier](cloudsimple-private-cloud.md).

## <a name="service-networking"></a>Dienstnetzwerk

Der CloudSimple-Dienst stellt jeweils ein Netzwerk pro Region bereit, in der Ihr CloudSimple-Dienst bereitgestellt wird. Das Netzwerk ist ein einzelner TCP-Schicht-3-Adressraum mit standardmäßig aktiviertem Routing. Alle privaten Clouds und Subnetze, die in dieser Region erstellt werden, kommunizieren ohne zusätzliche Konfiguration miteinander. Verteilte Portgruppen in vCenter werden unter Verwendung von VLANs erstellt. Die Workloadressourcen in Ihrer privaten Cloud können mithilfe der folgenden Netzwerkfeatures konfiguriert und geschützt werden:

* [VLANs und Subnetze](cloudsimple-vlans-subnets.md)
* [Firewalltabellen](cloudsimple-firewall-tables.md)
* [VPN-Gateways](cloudsimple-vpn-gateways.md)
* [Öffentliche IP-Adresse](cloudsimple-public-ip-address.md)
* [Azure-Netzwerkverbindung](cloudsimple-azure-network-connection.md)

## <a name="cloudsimple-virtual-machine"></a>Virtueller CloudSimple-Computer

Mit dem CloudSimple-Dienst können Sie virtuelle VMware-Computer über das Azure-Portal verwalten. Cluster oder Ressourcenpools aus Ihrer vSphere-Umgebung können dem Abonnement zugeordnet werden, für das der Dienst erstellt wird.

Weitere Informationen:

* [Virtuelle CloudSimple-Computer](cloudsimple-virtual-machines.md)
* [Azure-Abonnementzuordnung](./azure-subscription-mapping.md)
