---
title: Konzepte – Netzwerke in Azure Kubernetes Service (AKS)
description: Lernen Sie Netzwerke in Azure Kubernetes Service (AKS) kennen, einschließlich kubenet- und Azure CNI-Netzwerke, Eingangscontroller, Lastenausgleichsmodule und statische IP-Adressen.
ms.topic: conceptual
ms.date: 06/11/2020
ms.custom: fasttrack-edit
ms.openlocfilehash: edb195fae2e05a1f746c10482576f7e0b1bff7c9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "88243903"
---
# <a name="network-concepts-for-applications-in-azure-kubernetes-service-aks"></a>Netzwerkkonzepte für Anwendungen in Azure Kubernetes Service (AKS)

In einem containergestützten Microservice-Ansatz für die Anwendungsentwicklung müssen die Anwendungskomponenten zusammenarbeiten, um ihre Aufgaben zu verarbeiten. Kubernetes stellt verschiedene Ressourcen bereit, die diese Anwendungskommunikation ermöglichen. Sie können eine Verbindung mit Anwendungen herstellen und diese intern oder extern verfügbar machen. Zum Erstellen hochverfügbarer Anwendungen können Sie für Ihre Anwendungen einen Lastenausgleich vornehmen. Bei komplexeren Anwendungen ist möglicherweise die Konfiguration des eingehenden Datenverkehrs für die SSL/TLS-Terminierung oder ein Weiterleiten mehrerer Komponenten erforderlich. Aus Sicherheitsgründen müssen Sie möglicherweise auch den Netzwerkdatenverkehrsfluss in oder zwischen Pods und Knoten beschränken.

In diesem Artikel werden die wichtigsten Konzepte vorgestellt, mit denen Sie Netzwerke für Ihre Anwendungen in AKS bereitstellen:

- [Dienste](#services)
- [Virtuelle Azure-Netzwerke](#azure-virtual-networks)
- [Eingangscontroller](#ingress-controllers)
- [Netzwerkrichtlinien](#network-policies)

## <a name="kubernetes-basics"></a>Kubernetes-Grundlagen

Um den Zugriff auf Ihre Anwendungen oder die gegenseitige Kommunikation von Anwendungskomponenten zu erlauben, stellt Kubernetes eine Abstraktionsschicht für virtuelle Netzwerke bereit. Kubernetes-Knoten sind mit einem virtuellen Netzwerk verbunden und können eingehende und ausgehende Konnektivität für Pods bereitstellen. Die Komponente *kube-proxy*, die auf jedem Knoten ausgeführt wird, stellt diese Netzwerkfunktionen bereit.

In Kubernetes werden Pods von *Diensten* logisch gruppiert, um den direkten Zugriff über eine IP-Adresse oder einen DNS-Namen und an einem bestimmten Port zu erlauben. Sie können Datenverkehr auch über einen *Lastenausgleich* verteilen. Ein komplexeres Routing von Anwendungsdatenverkehr kann auch mit *Eingangscontrollern* erzielt werden. Die Sicherheit und das Filtern des Netzwerkdatenverkehrs für Pods kann mit Kubernetes-*Netzwerkrichtlinien* ermöglicht werden.

Außerdem trägt die Azure-Plattform zur Vereinfachung der virtuellen Netzwerke für AKS-Cluster bei. Wenn Sie einen Kubernetes-Lastenausgleich erstellen, wird die zugrunde liegende Azure Load Balancer-Ressource erstellt und konfiguriert. Wenn Sie Netzwerkports für Pods öffnen, werden die entsprechenden Regeln für Azure-Netzwerksicherheitsgruppen konfiguriert. Für das HTTP-Anwendungsrouting kann Azure auch *externes DNS* konfigurieren, wenn neue Eingangsrouten konfiguriert werden.

## <a name="services"></a>Dienste

Kubernetes verwendet *Dienste*, um die Netzwerkkonfiguration für Anwendungsworkloads zu vereinfachen. Dabei werden eine Reihe von Pods logisch miteinander gruppiert und Netzwerkkonnektivität bereitgestellt. Folgende Arten von Diensten sind verfügbar:

- **Cluster IP**: Erstellt eine interne IP-Adresse zur Verwendung innerhalb des AKS-Clusters. Ideal für rein interne Anwendungen, die andere Workloads im Cluster unterstützen.

    ![Diagramm mit Cluster IP-Datenverkehrsfluss in einem AKS-Cluster][aks-clusterip]

- **NodePort**: Erstellt eine Port-Zuordnung auf dem zugrunde liegenden Knoten, über die mithilfe von Knoten-IP-Adresse und Port der direkte Zugriff auf die Anwendung erfolgen kann.

    ![Diagramm mit NodePort-Datenverkehrsfluss in einem AKS-Cluster][aks-nodeport]

- **LoadBalancer**: Erstellt eine Azure Load Balancer-Ressource, konfiguriert eine externe IP-Adresse und verbindet die angeforderten Pods mit dem Back-End-Pool des Load Balancer. An den gewünschten Ports werden Regeln für den Lastenausgleich erstellt, damit der Datenverkehr der Kunden die Anwendung erreichen kann. 

    ![Diagramm mit Load Balancer-Datenverkehrsfluss in einem AKS-Cluster][aks-loadbalancer]

    Als zusätzliche Steuerung und für das Routing des eingehenden Datenverkehrs können Sie auch einen [Eingangscontroller](#ingress-controllers) verwenden.

- **ExternalName**: Erstellt einen bestimmten DNS-Eintrag für einen einfacheren Anwendungszugriff.

Die IP-Adresse für Lastenausgleichsmodule und Dienste kann dynamisch zugewiesen werden. Sie können aber auch eine vorhandene statische IP-Adresse angeben. Zugewiesen werden können sowohl interne als auch externe statische IP-Adressen. Diese vorhandene statische IP-Adresse ist häufig an einen DNS-Eintrag gebunden.

Es können sowohl *interne* als auch *externe* Lastenausgleichsmodule erstellt werden. Internen Lastenausgleichsmodulen wird nur eine private IP-Adresse zugeordnet, sodass nicht über das Internet darauf zugegriffen werden kann.

## <a name="azure-virtual-networks"></a>Virtuelle Azure-Netzwerke

In AKS können Sie einen Cluster bereitstellen, der eines der beiden folgenden Netzwerkmodelle verwendet:

- *Kubenet*-Netzwerke: Die Netzwerkressourcen werden normalerweise bei der Bereitstellung des AKS-Clusters erstellt und konfiguriert.
- *Azure Container Networking Interface (CNI)* -Netzwerke: Der AKS-Cluster wird mit vorhandenen virtuellen Netzwerkressourcen und -konfigurationen verbunden.

### <a name="kubenet-basic-networking"></a>Kubenet-Netzwerke – „Basic“ (Grundlegend)

Die Netzwerkoption *kubenet* ist die Standardkonfiguration für die AKS-Clustererstellung. Mit *kubenet* erhalten Knoten eine IP-Adresse aus dem Subnetz des virtuellen Azure-Netzwerks. Pods erhalten eine IP-Adresse von einem logisch unterschiedlichen Adressraum zum Subnetz des virtuellen Azure-Netzwerks der Knoten. Die Netzwerkadressübersetzung (NAT, Network Address Translation) wird dann so konfiguriert, dass die Pods Ressourcen im virtuellen Azure-Netzwerk erreichen können. Die Quell-IP-Adresse des Datenverkehrs wird mit NAT in die primäre IP-Adresse des Knotens übersetzt.

Knoten verwenden das Kubernetes-Plug-In [kubenet][kubenet]. Sie können die Azure-Plattform die virtuellen Netzwerke für Sie erstellen und konfigurieren lassen oder Ihren AKS-Cluster in einem bestehenden Subnetz des virtuellen Netzwerks bereitstellen. Auch hier wird NAT nur von den Knoten, die eine routingfähige IP-Adresse erhalten, und den Pods verwendet, um mit anderen Ressourcen außerhalb des AKS-Clusters zu kommunizieren. Dieser Ansatz reduziert die Anzahl der IP-Adressen, die Sie in Ihrem Netzwerkadressraum für die Verwendung von Pods reservieren müssen, erheblich.

Weitere Informationen finden Sie unter [Konfigurieren von kubernet-Netzwerken für AKS-Cluster][aks-configure-kubenet-networking].

### <a name="azure-cni-advanced-networking"></a>Azure CNI-Netzwerke – „Advanced“ (Erweitert)

Mit Azure CNI erhält jeder Pod eine IP-Adresse aus dem Subnetz und kann direkt angesprochen werden. Diese IP-Adressen müssen in Ihrem Netzwerkadressraum eindeutig sein und im Voraus geplant werden. Jeder Knoten verfügt über einen Konfigurationsparameter für die maximale Anzahl von Pods, die er unterstützt. Die entsprechende Anzahl von IP-Adressen pro Knoten wird dann im Voraus für diesen Knoten reserviert. Dieser Ansatz erfordert mehr Planung, da andernfalls die IP-Adressen ausgehen können oder der Cluster in einem größeren Subnetz neu erstellt werden muss, wenn die Anforderungen Ihrer Anwendung zunehmen.

Anders als bei Kubenet wird der Datenverkehr zu Endpunkten in demselben virtuellen Netzwerk nicht per NAT zur primären IP-Adresse des Knotens geleitet. Die Quelladresse für den Datenverkehr im virtuellen Netzwerk ist die Pod-IP-Adresse. Datenverkehr außerhalb des virtuellen Netzwerks wird weiterhin per NAT zur primären IP-Adresse des Knotens geleitet.

Knoten verwenden das Kubernetes-Plug-In [Azure Container Networking Interface (CNI)][cni-networking].

![Diagramm mit zwei Knoten jeweils mit Bridges für die Verbindungsherstellung mit einem Azure VNET][advanced-networking-diagram]

Weitere Informationen finden Sie unter [Konfigurieren der Azure CNI für einen AKS-Cluster][aks-configure-advanced-networking].

### <a name="compare-network-models"></a>Vergleich der Netzwerkmodelle

Sowohl kubenet als auch Azure CNI bieten Netzwerkkonnektivität für Ihre AKS-Cluster. Es gibt jedoch jeweils Vor- und Nachteile. Ganz allgemein gelten die folgenden Überlegungen:

* **kubenet**
    * Spart IP-Adressraum
    * Verwendet den internen oder externen Lastenausgleich von Kubernetes, um die Pods von außerhalb des Clusters zu erreichen
    * Erfordert eine manuelle Verwaltung und Wartung von benutzerdefinierten Routen (UDRs)
    * Maximal 400 Knoten pro Cluster
* **Azure CNI**
    * Pods erhalten vollständige VM-Konnektivität und können direkt über die private IP-Adresse aus verbundenen Netzwerken erreicht werden.
    * Erfordert mehr IP-Adressraum

Folgende Unterschiede treten im Verhalten von kubenet und Azure CNI auf:

| Funktion                                                                                   | Kubenet   | Azure CNI |
|----------------------------------------------------------------------------------------------|-----------|-----------|
| Bereitstellen von Clustern in vorhandenen oder neuen virtuellen Netzwerken                                            | Unterstützt – benutzerdefinierte Routen werden manuell angewandt. | Unterstützt |
| Pod-Pod-Konnektivität                                                                         | Unterstützt | Unterstützt |
| Pod-VM-Konnektivität; VM im gleichen virtuellen Netzwerk                                          | Funktioniert, wenn vom Pod initiiert | Funktioniert in beide Richtungen |
| Pod-VM-Konnektivität; VM in virtuellem Peernetzwerk                                            | Funktioniert, wenn vom Pod initiiert | Funktioniert in beide Richtungen |
| Lokaler Zugriff per VPN oder ExpressRoute                                                | Funktioniert, wenn vom Pod initiiert | Funktioniert in beide Richtungen |
| Zugriff auf Ressourcen, die von Dienstendpunkten geschützt werden                                             | Unterstützt | Unterstützt |
| Verfügbarmachen von Kubernetes-Diensten über einen Lastenausgleichsdienst, App Gateway oder einen Eingangscontroller | Unterstützt | Unterstützt |
| Standard: Azure DNS und private Zonen                                                          | Unterstützt | Unterstützt |

DNS wird sowohl mit Kubenet- als auch mit Azure-CNI-Plug-Ins von CoreDNS angeboten, einer Bereitstellung, die in AKS mit eigener automatischer Skalierung ausgeführt wird. Weitere Informationen zu CoreDNS in Kubernetes finden Sie unter [Anpassen des DNS-Dienst](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/). CoreDNS ist standardmäßig so konfiguriert, dass unbekannte Domänen an die Knoten-DNS-Server weitergeleitet werden, also an die DNS-Funktionalität der Azure Virtual Network-Instanz, in der der AKS-Cluster bereitgestellt wird. Daher können Azure DNS und private Zonen für Pods verwendet werden, die in AKS ausgeführt werden.

### <a name="support-scope-between-network-models"></a>Supportumfang der Netzwerkmodelle

Unabhängig vom verwendeten Netzwerkmodell können kubenet und Azure CNI mit einer der folgenden Methoden bereitgestellt werden:

* Die Azure-Plattform kann die Ressourcen des virtuellen Netzwerks automatisch erstellen und konfigurieren, wenn Sie einen AKS-Cluster erstellen.
* Sie können die Ressourcen des virtuellen Netzwerks manuell erstellen und konfigurieren und an diese Ressourcen anfügen, wenn Sie Ihren AKS-Cluster erstellen.

Auch wenn Funktionen wie Dienstendpunkte oder benutzerdefinierte Routen (UDRs) mit kubenet und Azure CNI unterstützt werden, legen die [Supportrichtlinien für AKS][support-policies] fest, welche Änderungen Sie vornehmen können. Beispiel:

* Wenn Sie die Ressourcen des virtuellen Netzwerks für einen AKS-Cluster manuell erstellen, werden Sie unterstützt, wenn Sie Ihre eigenen benutzerdefinierten Routen oder Dienstendpunkte konfigurieren.
* Wenn die Azure-Plattform die Ressourcen des virtuellen Netzwerks automatisch für Ihren AKS-Cluster erstellt, werden manuelle Änderungen an diesen von AKS verwalteten Ressourcen zum Konfigurieren Ihrer eigenen benutzerdefinierten Routen oder Dienstendpunkte nicht unterstützt.

## <a name="ingress-controllers"></a>Eingangscontroller

Wenn Sie einen Dienst des Typs "LoadBalancer" erstellen, wird eine zugrunde liegende Azure Load Balancer-Ressource erstellt. Der Load Balancer wird zum Verteilen von Datenverkehr an die Pods in Ihrem Dienst an einem bestimmten Port konfiguriert. Der Load Balancer arbeitet nur auf Schicht 4. Der Dienst hat keine Kenntnis von den tatsächlichen Anwendungen und kann keine zusätzlichen Überlegungen zum Netzwerkrouting vornehmen.

*Eingangscontroller* fungieren auf Schicht 7 und können intelligentere Regeln zum Verteilen des Anwendungsdatenverkehrs verwenden. Die übliche Aufgabe eines Eingangscontroller ist das Routing von HTTP-Datenverkehr an verschiedene Anwendungen anhand der eingehenden URL.

![Diagramm mit Eingangsdatenverkehrsfluss in einem AKS-Cluster][aks-ingress]

In AKS können Sie mit NGINX (oder ähnlich) eine Dateneingangsressource erstellen oder die AKS-Funktion für das HTTP-Anwendungsrouting verwenden. Wenn Sie das HTTP-Anwendungsrouting für einen AKS-Cluster aktivieren, erstellt die Azure-Plattform den Eingangscontroller und einen *externen DNS-Controller*. Wenn in Kubernetes neue Eingangsressourcen erstellt werden, werden in einer clusterspezifischen DNS-Zone die erforderlichen DNS-A-Einträge erstellt. Weitere Informationen finden Sie unter [Bereitstellen von HTTP-Anwendungsrouting][aks-http-routing].

Mit dem ACIG-Add-On (Application Gateway Ingress Controller, Application Gateway-Eingangscontroller) können AKS-Kunden den nativen L7-Lastenausgleich von Application Gateway nutzen, um Cloudsoftware im Internet verfügbar zu machen. AGIC überwacht den Kubernetes-Cluster, auf dem er gehostet wird, und aktualisiert fortlaufend eine Application Gateway-Instanz, sodass ausgewählte Dienste im Internet bereitgestellt werden. Weitere Informationen zum ACIG-Add-On für AKS finden Sie unter [Was ist der Application Gateway-Eingangscontroller?][agic-overview].

Ein weiteres allgemeines Feature des Dateneingangs ist die SSL/TLS-Terminierung. Bei großen Webanwendungen, auf die über HTTPS zugegriffen wird, kann die TLS-Terminierung durch die Eingangsressource erfolgen und braucht nicht innerhalb der Anwendung verarbeitet zu werden. Um die automatische Generierung und Konfiguration der TLS-Zertifizierung bereitzustellen, können Sie die Eingangsressource für die Verwendung von Anbietern wie Let's Encrypt konfigurieren. Weitere Informationen zum Konfigurieren eines NGINX-Eingangscontrollers mit Let's Encrypt finden Sie unter [Eingang und TLS][aks-ingress-tls].

Sie können den Eingangscontroller auch so konfigurieren, dass die Quell-IP des Clients bei Anforderungen an Container im AKS-Cluster beibehalten wird. Wenn die Anforderung eines Clients über den Eingangscontroller an einen Container im AKS-Cluster weitergeleitet wird, ist die ursprüngliche Quell-IP dieser Anforderung für den Zielcontainer nicht verfügbar. Wenn Sie die *Beibehaltung der Clientquell-IP* aktivieren, ist die Quell-IP für den Client im Anforderungsheader unter *X-Forwarded-For* verfügbar. Wenn Sie die Beibehaltung der Clientquell-IP auf dem Eingangscontroller verwenden, können Sie kein TLS-Pass-Through verwenden. Die Beibehaltung der Clientquell-ID und TLS-Pass-Through-können mit anderen Diensten verwendet werden, z. B. dem *LoadBalancer*-Typ.

## <a name="network-security-groups"></a>Netzwerksicherheitsgruppen

Eine Netzwerksicherheitsgruppe filtert Datenverkehr für virtuelle Computer wie beispielsweise die AKS-Knoten. Beim Erstellen von Diensten (z. B. LoadBalancer) konfiguriert die Azure-Plattform automatisch alle erforderlichen Netzwerksicherheitsgruppen-Regeln. Sie sollten keine Netzwerksicherheitsgruppen-Regeln manuell konfigurieren, um Datenverkehr für Pods in einem AKS-Cluster zu filtern. Definieren Sie alle erforderlichen Ports und die Weiterleitung im Rahmen Ihrer Kubernetes-Dienstmanifeste, und überlassen Sie das Erstellen oder Aktualisieren der entsprechenden Regeln der Azure-Plattform. Sie können auch Netzwerkrichtlinien verwenden, wie im nächsten Abschnitt erläutert, um automatisch auf Pods Filterregeln für den Datenverkehr anzuwenden.

## <a name="network-policies"></a>Netzwerkrichtlinien

Standardmäßig können alle Pods in einem AKS-Cluster Datenverkehr ohne Einschränkungen senden und empfangen. Aus Sicherheitsgründen möchten Sie möglicherweise Regeln definieren, die den Datenverkehrsfluss steuern. Back-End-Anwendungen werden häufig nur für erforderlichen Front-End-Dienste verfügbar gemacht, oder Datenbankkomponenten sind nur für die Anwendungsebenen zugänglich, die eine Verbindung zu ihnen herstellen.

Netzwerkrichtlinien sind ein in AKS verfügbares Kubernetes-Feature, mit dem Sie den Datenverkehrsfluss zwischen Pods steuern können. Anhand von Einstellungen wie zugewiesene Bezeichnungen, Namespace oder Port für den Datenverkehr können Sie Datenverkehr zulassen oder verweigern. Netzwerksicherheitsgruppen sind eher für die AKS-Knoten und nicht für Pods bestimmt. Die Verwendung von Netzwerkrichtlinien ist eine besser geeignete, cloudnative Möglichkeit, den Datenverkehrsfluss zu steuern. Wenn Pods in einem AKS-Cluster dynamisch erstellt werden, können automatisch die erforderlichen Netzwerkrichtlinien angewendet werden.

Weitere Informationen finden Sie unter [Sicherer Datenverkehr zwischen Pods durch Netzwerkrichtlinien in Azure Kubernetes Service (AKS)][use-network-policies].

## <a name="next-steps"></a>Nächste Schritte

Um mit AKS-Netzwerken zu beginnen, erstellen und konfigurieren Sie einen AKS-Cluster mit Ihren eigenen IP-Adressbereichen unter Verwendung von [kubenet][aks-configure-kubenet-networking] oder [Azure CNI][aks-configure-advanced-networking].

Entsprechende bewährte Methoden finden Sie unter [Best Practices für Netzwerkkonnektivität und Sicherheit in AKS][operator-best-practices-network].

Weitere Informationen zu den wesentlichen Konzepten von Kubernetes und AKS finden Sie in den folgenden Artikeln:

- [Kubernetes/AKS: Cluster und Workloads][aks-concepts-clusters-workloads]
- [Kubernetes/AKS: Zugriff und Identität][aks-concepts-identity]
- [Kubernetes/AKS: Sicherheit][aks-concepts-security]
- [Kubernetes/AKS: Speicher][aks-concepts-storage]
- [Kubernetes/AKS: Skalierung][aks-concepts-scale]

<!-- IMAGES -->
[aks-clusterip]: ./media/concepts-network/aks-clusterip.png
[aks-nodeport]: ./media/concepts-network/aks-nodeport.png
[aks-loadbalancer]: ./media/concepts-network/aks-loadbalancer.png
[advanced-networking-diagram]: ./media/concepts-network/advanced-networking-diagram.png
[aks-ingress]: ./media/concepts-network/aks-ingress.png

<!-- LINKS - External -->
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet

<!-- LINKS - Internal -->
[aks-http-routing]: http-application-routing.md
[aks-ingress-tls]: ./ingress-tls.md
[aks-configure-kubenet-networking]: configure-kubenet.md
[aks-configure-advanced-networking]: configure-azure-cni.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-identity]: concepts-identity.md
[agic-overview]: ../application-gateway/ingress-controller-overview.md
[use-network-policies]: use-network-policies.md
[operator-best-practices-network]: operator-best-practices-network.md
[support-policies]: support-policies.md
