---
title: Netzwerkfeatures
description: Erfahren Sie mehr über die Netzwerkfeatures in Azure App Service und darüber, welche Features Sie für Ihre Netzwerkanforderungen in Bezug auf Sicherheit oder Funktionalität benötigen.
author: ccompy
ms.assetid: 5c61eed1-1ad1-4191-9f71-906d610ee5b7
ms.topic: article
ms.date: 10/18/2020
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 860b1ac1713ac7afb7db2643d68974b399b5236b
ms.sourcegitcommit: 957c916118f87ea3d67a60e1d72a30f48bad0db6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92207047"
---
# <a name="app-service-networking-features"></a>App Service-Netzwerkfunktionen

Anwendungen in Azure App Service können auf verschiedene Arten bereitgestellt werden. Für Apps, die von App Service gehostet werden, gilt standardmäßig, dass sie direkt über das Internet zugänglich sind und dass sie nur Endpunkte erreichen können, die im Internet gehostet werden. Viele Kundenanwendungen müssen jedoch den ein- und den ausgehenden Netzwerkdatenverkehr steuern. Es gibt mehrere Funktionen in App Service, mit denen diese Anforderungen erfüllt werden können. Die Herausforderung besteht darin, zu wissen, welche Funktion zur Lösung eines bestimmten Problems verwendet werden sollte. Dieses Dokument soll Kunden ermöglichen, anhand einiger exemplarischer Anwendungsfälle zu bestimmen, welche Funktion jeweils verwendet werden soll.

Es gibt zwei primäre Bereitstellungsarten für Azure App Service. Es gibt den mehrinstanzenfähigen öffentlichen Dienst, der App Service-Pläne in den Preis-SKUs (Tarife) „Free“, „Shared“, „Basic“, „Standard“, „Premium“, „Premiumv2“ und „Premiumv3“ hostet. Außerdem gibt es die App Service-Umgebung (App Service Environment, ASE) für einzelne Mandanten, in der App Service Pläne für isolierte SKUs direkt in Ihrem Azure Virtual Network (VNet) gehostet werden. Sie verwenden unterschiedliche Funktionen abhängig davon, ob Sie mit dem mehrinstanzenfähigen Dienst oder in einer ASE arbeiten. 

## <a name="multi-tenant-app-service-networking-features"></a>Netzwerkfunktionen für mehrinstanzenfähigen App Service 

Azure App Service ist ein verteiltes System. Die Rollen, mit denen eingehende HTTP/HTTPS-Anforderungen verarbeitet werden, werden als Front-Ends bezeichnet. Die Rollen, mit denen die Kundenworkload gehostet wird, werden als Worker bezeichnet. Alle Rollen in einer App Service-Bereitstellung sind in einem mehrinstanzenfähigen Netzwerk vorhanden. Da es viele verschiedene Kunden in derselben App Service-Skalierungseinheit gibt, können Sie das App Service-Netzwerk nicht direkt mit Ihrem Netzwerk verbinden. Statt die Netzwerke zu verbinden, sind Funktionen erforderlich, mit denen die verschiedenen Aspekte der Anwendungskommunikation verwaltet werden. Die Funktionen, mit denen Anforderungen AN Ihre App verwaltet werden, können nicht dazu verwendet werden, Probleme zu lösen, wenn Aufrufe AUS Ihrer App erfolgen. Umgekehrt können die Funktionen, die Probleme für Aufrufe AUS Ihrer App lösen, nicht dazu verwendet werden, Probleme mit Anforderungen AN Ihre App zu lösen.  

| Eingehende Funktionen | Ausgehende Funktionen |
|---------------------|-------------------|
| Von der App zugewiesene Adresse | Hybridverbindungen |
| Zugriffseinschränkungen | VNET-Integration, die ein Gateway erfordert |
| Dienstendpunkte | VNET-Integration |

Sofern nicht anders angegeben ist, können alle Funktionen zusammen verwendet werden. Sie können die Funktionen kombinieren, um Ihre verschiedenen Probleme zu lösen.

## <a name="use-case-and-features"></a>Anwendungsfall und Funktionen

Für jeden vorliegenden Anwendungsfall kann es einige Möglichkeiten geben, das Problem zu lösen.  Die richtige zu verwendende Funktion hängt manchmal von Aspekten ab, die über den Anwendungsfall selbst hinausgehen. Anhand der folgenden Anwendungsfälle für eingehende Anforderungen wird vorgeschlagen, wie App Service-Netzwerkfunktionen verwendet werden können, um Probleme hinsichtlich des Steuerns von Datenverkehr zu lösen, der an Ihre App gesendet wird. 
 
| Eingehende Anwendungsfälle | Funktion |
|---------------------|-------------------|
| Unterstützung für IP-basiertes SSL für Ihre App benötigt | Von der App zugewiesene Adresse |
| Nicht freigegeben, dedizierte eingehende Adresse für Ihre App | Von der App zugewiesene Adresse |
| Beschränken des Zugriffs auf Ihre App aus einem Satz klar definierter Adressen | Zugriffseinschränkungen |
| Beschränken des Zugriffs auf meine App von Ressourcen in einem VNet | Dienstendpunkte </br> ILB ASE </br> Private Endpunkte |
| Verfügbarmachen meiner App über eine private IP-Adresse in meinem VNet | ILB ASE </br> Private Endpunkte </br> Private IP-Adresse für eingehenden Datenverkehr in Application Gateway mit Dienstendpunkten |
| Schützen meiner App mit einer Web Application Firewall (WAF) | Application Gateway + ILB ASE </br> Application Gateway mit privaten Endpunkten </br> Application Gateway mit Dienstendpunkten </br> Azure Front Door mit Zugriffsbeschränkungen |
| Lastenausgleich für Datenverkehr zu meinen Apps in verschiedenen Regionen | Azure Front Door mit Zugriffsbeschränkungen | 
| Lastenausgleich für Datenverkehr in derselben Region | [Application Gateway mit Dienstendpunkten][appgwserviceendpoints] | 

Anhand der folgenden Anwendungsfälle für ausgehenden Datenverkehr wird vorgeschlagen, wie App Service-Netzwerkfunktionen verwendet werden können, um Anforderungen zu lösen, die Ihre App hinsichtlich ausgehendem Zugriff stellt. 

| Ausgehende Anwendungsfälle | Funktion |
|---------------------|-------------------|
| Zugreifen auf Ressourcen in einem virtuellen Azure-Netzwerk in derselben Region | VNET-Integration </br> ASE |
| Zugreifen auf Ressourcen in einem virtuellen Azure-Netzwerk in einer anderen Region | VNET-Integration, die ein Gateway erfordert </br> ASE und VNet-Peering |
| Zugreifen auf Ressourcen, die mit Dienstendpunkten geschützt sind | VNET-Integration </br> ASE |
| Zugreifen auf Ressourcen in einem privaten Netzwerk, das nicht mit Azure verbunden ist | Hybridverbindungen |
| Zugreifen auf Ressourcen über ExpressRoute-Verbindungen | VNET-Integration </br> ASE | 
| Sicherer ausgehender Datenverkehr von Ihrer Web-App | VNET-Integration und Netzwerksicherheitsgruppen </br> ASE | 
| Weiterleiten ausgehenden Datenverkehrs von Ihrer Web-App | VNET-Integration und Routingtabellen </br> ASE | 


### <a name="default-networking-behavior"></a>Standardmäßiges Netzwerkverhalten

Die Azure App Service-Skalierungseinheiten unterstützen viele Kunden in jeder Bereitstellung. Für die SKU-Pläne „Free“ und „Shared“ werden Kundenworkloads in mehrinstanzenfähigen Workern gehostet. Für den Plan „Basic“ und höhere Pläne werden Kundenworkloads gehostet, die nur einem App Service-Plan (ASP) zugeordnet sind. Wenn Sie den App Service-Plan „Standard“ hatten, werden alle Apps in diesem Plan im selben Worker ausgeführt. Wenn Sie den Worker aufskalieren, werden alle Apps in diesem Plan in einem neuen Worker für jede Instanz in Ihrem ASP repliziert. 

#### <a name="outbound-addresses"></a>Ausgehende Adressen

Die Worker-VMs werden größtenteils durch die App Service-Tarife aufgeschlüsselt. Die Tarife „Free“, „Shared“, „Basic“, „Standard“ und „Premium“ verwenden alle denselben Worker-VM-Typ. Premiumv2 verwendet einen anderen VM-Typ. Premiumv3 verwendet noch einen anderen VM-Typ. Bei jeder Änderung der VM-Familie gibt es einen anderen Satz an ausgehenden Adressen. Wenn Sie von „Standard“ auf „Premiumv2“ skalieren, ändern sich Ihre ausgehenden Adressen. Wenn Sie von „Premiumv2“ auf „Premiumv3“ skalieren, ändern sich Ihre ausgehenden Adressen. Es gibt einige ältere Skalierungseinheiten, die sowohl die eingehenden als auch die ausgehenden Adressen ändern, wenn Sie von „Standard“ auf „Premiumv2“ skalieren. Es gibt eine Reihe von Adressen, die für ausgehende Aufrufe verwendet werden. Die von Ihrer App für ausgehende Aufrufe verwendeten ausgehenden Adressen werden in den Eigenschaften für Ihre App aufgeführt. Diese Adressen werden von allen Apps, die auf derselben Familie von Worker-VM ausgeführt werden, in dieser App Service-Bereitstellung gemeinsam genutzt. Wenn Sie alle möglichen Adressen anzeigen möchten, die Ihre App in dieser Skalierungseinheit möglicherweise verwenden könnte, gibt es eine andere Eigenschaft namens „possibleOutboundAddresses“, über die sie aufgelistet werden. 

![App-Eigenschaften](media/networking-features/app-properties.png)

App Service hat einige Endpunkte, die zur Verwaltung des Diensts verwendet werden.  Diese Adressen werden in einem gesonderten Dokument veröffentlicht und sind auch im IP-Diensttag „AppServiceManagement“ enthalten. Das Tag „AppServiceManagement“ wird nur mit einer App Service-Umgebung verwendet, in der Sie solchen Datenverkehr zuzulassen müssen. Der eingehenden Adressen für App Service werden im IP-Diensttag „AppService“ nachverfolgt. Es gibt kein IP-Diensttag, das die ausgehenden Adressen enthält, die von App Service verwendet werden. 

![Diagramm für eingehenden und ausgehenden Datenverkehr für App Service](media/networking-features/default-behavior.png)

### <a name="app-assigned-address"></a>Von der App zugewiesene Adresse 

Die Funktion „von der App zugewiesene Adresse“ ist ein Ableger der IP-basierten SSL-Fähigkeit, und auf sie wird zugegriffen, indem SSL für Ihre App eingerichtet wird. Diese Funktion kann für IP-basierte SSL-Anrufe verwendet werden, aber sie kann auch verwendet werden, um Ihrer App eine Adresse zu geben, die nur sie hat. 

![Diagramm zu von der App zugewiesene Adresse](media/networking-features/app-assigned-address.png)

Wenn Sie eine von der App zugewiesene Adresse verwenden, durchläuft Ihr Datenverkehr weiterhin genau die Front-End-Rollen, die den gesamten Datenverkehr verarbeiten, der in der App Service-Skalierungseinheit eingeht. Die Adresse, die Ihrer App zugewiesen ist, wird jedoch nur von Ihrer App verwendet. Die Anwendungsfälle für diese Funktion sehen wie folgt aus:

* Unterstützung für IP-basiertes SSL für Ihre App benötigt
* Festlegen einer dedizierten Adresse für Ihre App, die mit keinem anderen Element gemeinsam genutzt wird

Informationen, wie Sie eine Adresse für Ihre App festlegen, finden Sie im Tutorial zu [Hinzufügen eines TLS/SSL-Zertifikats in Azure App Service][appassignedaddress]. 

### <a name="access-restrictions"></a>Zugriffseinschränkungen 

Über die Funktionalität „Zugriffsbeschränkungen“ können Sie **eingehende** Anforderungen anhand der Ursprungs-IP-Adresse filtern. Die Filteraktion wird mit den Front-End-Rollen ausgeführt, die den Workerrollen vorgelagert sind, mit denen Ihre Apps ausgeführt werden. Da die Front-End-Rollen den Workern vorgelagert sind, kann die Funktionalität „Zugriffsbeschränkungen“ als Schutz auf Netzwerkebene für Ihre Apps angesehen werden. Die Funktion ermöglicht es Ihnen, eine Liste von zulässigen und abgelehnten Adressblöcken zu erstellen, die in der Reihenfolge ihrer Priorität ausgewertet werden. Diese Funktion ist mit der Netzwerksicherheitsgruppe-Funktion (NSG-Funktion) vergleichbar, die in Azure-Netzwerken vorhanden ist.  Sie können diese Funktion in einer App Service-Umgebung (ASE) oder im mehrinstanzenfähigen Dienst verwenden. Bei Verwendung mit einer ILB ASE können Sie den Zugriff von privaten Adressblöcken beschränken.

![Zugriffseinschränkungen](media/networking-features/access-restrictions.png)

Die „Zugriffsbeschränkungen“-Funktion ist in den Fällen nützlich, in denen Sie die IP-Adressen einschränken möchten, über die Ihre App erreicht werden kann. Zu den Anwendungsfällen für diese Funktion gehören:

* Beschränken des Zugriffs auf Ihre App aus einem Satz klar definierter Adressen 
* Beschränken des Zugriffs so, dass er über einen Lastenausgleichsdienst erfolgt, z. B. über Azure Front Door. Wenn Sie Ihren eingehenden Datenverkehr an Azure Front Door sperren möchten, erstellen Sie Regeln, um Datenverkehr von 147.243.0.0/16 und 2a01:111:2050::/44 zuzulassen. 

![Zugriffsbeschränkungen mit Azure Front Door](media/networking-features/access-restrictions-afd.png)

Wenn Sie den Zugriff auf Ihre App sperren möchten, sodass sie nur von Ressourcen in Ihrem virtuellen Azure-Netzwerk (VNet) erreicht werden kann, benötigen Sie eine statische öffentliche Adresse für jede Komponente, die in Ihrem VNet als Quelle fungiert. Haben die Ressourcen keine öffentliche Adresse, sollten Sie stattdessen die Funktion „Dienstendpunkte“ verwenden. Informationen, wie Sie diese Funktion aktivieren, finden Sie im Tutorial zu [Azure App Service – Statische Zugriffseinschränkungen][iprestrictions].

### <a name="service-endpoints"></a>Dienstendpunkte

Mit Dienstendpunkten können Sie **eingehenden** Zugriff auf Ihre App so sperren, dass die Quelladresse aus einem Satz von Subnetzen stammen muss, die Sie auswählen. Diese Funktion funktioniert zusammen mit den IP-Zugriffseinschränkungen. Dienstendpunkte sind mit Remotedebugging nicht kompatibel. Wenn Sie Remotedebuggen mit Ihrer App verwenden möchten, darf sich Ihr Client nicht in einem Subnetz mit aktivierten Dienstendpunkten befinden. Dienstendpunkte werden in derselben Benutzeroberfläche wie die IP-Zugriffseinschränkungen festgelegt. Sie können eine Zulassungs-/Verweigerungsliste mit Zugriffsregeln erstellen, die sowohl öffentliche Adressen als auch Subnetze in Ihren VNets umfassen. Diese Funktion unterstützt Szenarien wie die folgenden:

![Dienstendpunkte](media/networking-features/service-endpoints.png)

* Einrichten eines Anwendungsgateways mit Ihrer App, um eingehenden Datenverkehr an Ihre App zu sperren
* Einschränken von Zugriff auf Ihre App auf Ressourcen in Ihrem VNet. Dies kann virtuelle Computer, ASEs oder sogar andere Apps umfassen, für die VNet-Integration verwendet wird. 

![Dienstendpunkte mit Anwendungsgateway](media/networking-features/service-endpoints-appgw.png)

Weitere Informationen zum Konfigurieren von Dienstendpunkten mit Ihrer App finden Sie im Tutorial zu [Azure App Service – Statische Zugriffseinschränkungen][serviceendpoints].

### <a name="private-endpoints"></a>Private Endpunkte

Ein privater Endpunkt ist eine Netzwerkschnittstelle, die Sie über eine private und sichere Verbindung mit Azure Private Link mit Ihrer Web-App verbindet. Der private Endpunkt verwendet eine private IP-Adresse in Ihrem VNET und bindet die Web-App effektiv in Ihr VNET ein. Diese Funktion gilt nur für bei Ihrer Web-App **eingehenden** Flows.
[Verwenden von privaten Endpunkten für eine Azure-Web-App][privateendpoints]

Private Endpunkte ermöglichen beispielsweise folgende Szenarien:

* Beschränken des Zugriffs auf meine App von Ressourcen in einem VNet 
* Verfügbarmachen meiner App über eine private IP-Adresse in meinem VNet 
* Schützen meiner App mit einer Web Application Firewall (WAF) 

Private Endpunkte verhindern Datenexfiltration, weil Sie über den privaten Endpunkt ausschließlich Zugriff auf die App erhalten, die damit konfiguriert ist. 
 
### <a name="hybrid-connections"></a>Hybridverbindungen

Mit App Service-Hybridverbindungen wird es ermöglicht, in Ihren Apps **ausgehende** Aufrufe an bestimmte TCP-Endpunkte auszuführen. Der jeweilige Endpunkt kann sich an einer lokalen Stelle, in einem VNet oder an einer beliebigen Stelle befinden, die ausgehenden Datenverkehr an Azure über Port 443 zulässt. Für die Funktion muss ein Relay-Agent, der als Hybridverbindungs-Manager (Hybrid Connection Manager, HCM) bezeichnet wird, auf einem Host mit Windows Server 2012 oder höher installiert sein. Der HCM muss in der Lage sein, Azure Relay über Port 443 erreichen zu können. Der HCM kann im Portal über die Benutzeroberfläche für App Service-Hybridverbindungen heruntergeladen werden. 

![Netzwerkdatenfluss für Hybridverbindungen](media/networking-features/hybrid-connections.png)

Die Funktion „App Service-Hybridverbindungen“ setzt auf der Funktionalität für Azure Relay-Hybridverbindungen auf. In App Service wird eine spezielle Form der Funktion verwendet, die es nur unterstützt, dass ausgehende Aufrufe aus Ihrer App an einen TCP-Host und -Port ausgeführt werden. Dieser Host und Port müssen nur auf dem Host aufgelöst werden, auf dem der HCM installiert ist. Wenn die App in App Service eine DNS-Suche (DNS-Lookup) für den Host und den Port ausführt, die in Ihrer Hybridverbindung definiert sind, wird der Datenverkehr automatisch umgeleitet, sodass er über die Hybridverbindung und den Hybridverbindungs-Manager nach außen gesendet wird. Weitere Informationen über Hybrid Connections finden Sie in der Dokumentation zu [Azure App Service Hybrid Connections][hybridconn].

Diese Funktion wird üblicherweise für Folgendes verwendet:

* Zugreifen auf Ressourcen in privaten Netzwerken, die nicht mit Azure verbunden sind, über ein VPN oder über ExpressRoute
* Unterstützen von Lift & Shift von lokalen Anwendungen zu App Service, ohne dass auch unterstützende Datenbanken verschoben werden müssen  
* Sicheres Bereitstellen von Zugriff auf einen einzelnen Host und Port pro Hybridverbindung. Die meisten Netzwerkfunktionen öffnen Zugriff auf ein Netzwerk, und mit Hybridverbindungen haben Sie nur den einzigen Host und Port, den Sie erreichen können.
* Abdecken von Szenarios, die von anderen Methoden für ausgehende Konnektivität nicht abgedeckt sind
* Entwickeln in App Service, wo für die Apps problemlos lokale Ressourcen genutzt werden können 

Da die Funktion Zugriff auf lokale Ressourcen ohne eine eingehende Firewalllücke ermöglicht, ist sie bei Entwicklern beliebt. Die anderen Funktionen für ausgehenden App Service-Netzwerkdatenverkehr beziehen sich im Wesentlichen auf virtuelle Azure-Netzwerke. Für Hybridverbindungen gibt es keine Abhängigkeit hinsichtlich des Durchlaufens eines VNets, sodass sie für eine größere Bandbreite von Netzwerkanforderungen verwendet werden können. Es ist wichtig zu beachten, dass die Funktion „App Service-Hybridverbindungen“ weder beachtet noch weiß, was über sie erledigt wird. Das heißt, Sie können sie verwenden, um auf eine Datenbank, einen Webdienst oder einen beliebigen TCP-Socket auf einem Mainframe zugreifen. Die Funktion tunnelt im Wesentlichen TCP-Pakete. 

Hybridverbindungen sind nicht für die Entwicklung beliebt, sondern werden auch in zahlreichen Produktionsanwendungen verwendet. Sie eignen sich hervorragend für den Zugriff auf einen Webdienst oder eine Datenbank, sind aber nicht für Situationen geeignet, in denen sehr viele Verbindungen zu erstellen sind. 

### <a name="gateway-required-vnet-integration"></a>VNET-Integration, die ein Gateway erfordert 

Die Funktion „Gatewaygeforderte App Service-VNet-Integration“ ermöglicht, dass in Ihrer App **ausgehende** Anforderungen an ein virtuelles Azure-Netzwerk gesendet werden. Mit der Funktion wird der Host, auf dem Ihre App ausgeführt wird, über einen Punkt-zu-Standort-VPN mit einem virtuellen Netzwerkgateway in Ihrem VNet verbunden. Wenn Sie die Funktion konfigurieren, erhält Ihre App eine der Punkt-zu-Standort-Adressen, die jeder Instanz zugewiesen sind. Über diese Funktion können Sie auf Ressourcen zugreifen, die sich in einer beliebigen Region in klassischen oder in Resource Manager-VNets befinden. 

![VNET-Integration, die ein Gateway erfordert](media/networking-features/gw-vnet-integration.png)

Diese Funktion löst das Problem des Zugreifens auf Ressourcen in anderen VNets und kann sogar genutzt werden, über ein VNet Verbindungen mit anderen VNets oder sogar lokal herzustellen. Die Funktion kann nicht mit VNets, die über ExpressRoute verbunden sind, kann aber mit Netzwerken verwendet werden, die über ein Site-to-Site-VPN (Standort-zu-Standort-VPN) verbunden sind. Es ist normalerweise nicht sinnvoll, diese Funktion aus einer App in einer App Service-Umgebung (ASE) zu verwenden, da sich die ASE bereits in Ihrem VNet befindet. Diese Funktion löst folgende Anwendungsfälle:

* Zugreifen auf Ressourcen über private IP-Adressen in Ihren virtuellen Azure-Netzwerken 
* Zugreifen auf lokale Ressourcen, wenn ein Site-to-Site-VPN vorhanden ist 
* Zugreifen auf Ressourcen in VNets mit Peering 

Wenn diese Funktion aktiviert ist, wird in Ihrer App der DNS-Server verwenden, mit dem das Ziel-VNet konfiguriert ist. Weitere Informationen zu dieser Funktion finden Sie in der Dokumentation zu [Integrieren Ihrer App in ein Azure Virtual Network][vnetintegrationp2s]. 

### <a name="vnet-integration"></a>VNET-Integration

Die Funktion „Gatewaygeforderte VNet-Integration“ ist nützlich, löst aber nicht das Problem des Zugreifens auf Ressourcen über ExpressRoute. Zusätzlich zur Notwendigkeit der Erreichbarkeit über ExpressRoute-Verbindungen ist es für Apps erforderlich, dass sie Aufrufe an Dienste senden können, die über Dienstendpunkte geschützt sind. Um diese beiden zusätzlichen Anforderungen erfüllen zu können, wurde eine weitere VNet-Integrationsfunktion hinzugefügt. Die neue VNet-Integrationsfunktion ermöglicht es Ihnen, das Back-End Ihrer App in einem Subnetz in einem Resource Manager-VNet in derselben Region zu platzieren. Diese Funktion ist nicht aus einer App Service-Umgebung verfügbar, die sich bereits in einem VNet befindet. Diese Funktion ermöglicht Folgendes:

* Zugreifen auf Ressourcen in Resource Manager-VNets in derselben Region
* Zugreifen auf Ressourcen, die mit Dienstendpunkten geschützt sind 
* Zugreifen auf Ressourcen, auf die über ExpressRoute- oder VPN-Verbindungen zugegriffen werden kann
* Sichern des gesamten ausgehenden Datenverkehrs 
* Erzwingen von Tunnelung für den gesamten ausgehenden Datenverkehr. 

![VNET-Integration](media/networking-features/vnet-integration.png)

Wenn Sie mehr zu dieser Funktion erfahren möchten, lesen Sie den Artikel [Integrieren Ihrer App in ein Azure Virtual Network][vnetintegration].

## <a name="app-service-environment"></a>App Service-Umgebung 

Eine App Service-Umgebung (ASE) ist eine Bereitstellung von Azure App Service für einen einzelnen Mandanten, die in Ihrem VNet ausgeführt wird. Die ASE ermöglicht Anwendungsfälle wie die folgenden:

* Zugriff auf Ressourcen in Ihrem VNet
* Zugriff auf Ressourcen über ExpressRoute
* Verfügbarmachen Ihrer Apps mit einer privaten Adresse in Ihrem VNet 
* Zugriff auf Ressourcen über Dienstendpunkte 

Mit einer ASE müssen Sie keine Funktionen wie „VNet-Integration“ oder „Dienstendpunkte“ verwenden, da sich die ASE bereits in Ihrem VNet befindet. Wenn Sie über Dienstendpunkte auf Ressourcen wie SQL oder Storage zugreifen möchten, aktivieren Sie Dienstendpunkte im ASE-Subnetz. Wenn Sie auf Ressourcen im VNet zugreifen möchten, ist keine zusätzliche Konfiguration erforderlich.  Wenn Sie über ExpressRoute auf Ressourcen zugreifen möchten, befinden Sie sich bereits im VNet und müssen nichts in der ASE oder in den ihr enthaltenen Apps konfigurieren. 

Da die Apps in einer ILB ASE über eine private IP-Adresse verfügbar gemacht werden können, können Sie einfach Web Application Firewall-Geräte hinzufügen, um nur die gewünschten Apps für das Internet verfügbar zu machen und die restlichen Apps geschützt zu belassen. Dies führt immanent zur einfachen Entwicklung von Anwendungen mit mehreren Ebenen. 

Es gibt einige Dinge, die aus dem mehrinstanzenfähigen Dienst noch nicht, aber aus einer ASE möglich sind. Dazu gehören folgende Dinge:

* Verfügbarmachen Ihrer Apps unter einer privaten IP-Adresse
* Schützen des gesamten ausgehenden Datenverkehrs mit Netzwerksteuerelementen, die nicht Bestandteil Ihrer App sind 
* Hosten Ihrer Apps in einem Dienst für einen einzelnen Mandanten 
* Hochskalieren auf sehr viel mehr Instanzen als im mehrinstanzenfähigen Dienst möglich sind 
* Laden von privaten Zertifizierungsstellen-Clientzertifikaten zur Verwendung durch Ihre Apps mit privaten Endpunkten, die über eine Zertifizierungsstelle geschützt sind 
* Erzwingen von TLS 1.1 für alle Apps, die im System gehostet werden, ohne irgendeine Möglichkeit, dies auf App-Ebene zu deaktivieren 
* Bereitstellen einer dedizierten ausgehenden Adresse für alle Apps in Ihrer ASE, die für keinen Kunden freigegeben ist 

![ASE in einem VNet](media/networking-features/app-service-environment.png)

Die ASE bietet die beste Lösung rund um isoliertes und dediziertes App-Hosting, bringt aber einige Verwaltungsherausforderungen mit sich. Folgende Punkte sind einige der Punkte, die vor dem Verwenden einer operativen ASE zu beachten sind:
 
 * Eine ASE wird in Ihrem VNet ausgeführt, hat aber Abhängigkeiten außerhalb des VNet. Diese Abhängigkeiten müssen zulässig sein. Weitere Informationen finden Sie unter [Überlegungen zum Netzwerkbetrieb in einer App Service-Umgebung][networkinfo].
 * Eine ASE lässt sich nicht unmittelbar skalieren wie der mehrinstanzenfähige Dienst. Sie müssen die Skalierungsanforderungen vorausplanen, statt reagierend zu skalieren. 
 * Eine ASE hat höhere mit ihr verbundene Vorlaufkosten. Um Ihre ASE optimal zu nutzen, sollten Sie so planen, dass viele Workloads in einer ASE platziert werden, statt sie für kleine Kapazitäten zu nutzen.
 * Für die Apps in einer ASE ist es nicht möglich, Zugriff auf einige Apps in der ASE zu beschränken, auf andere dagegen nicht.
 * Die ASE befindet sich in einem Subnetz, und Netzwerkregeln gelten für den gesamten Datenverkehr an diese und aus dieser ASE. Wenn Sie nur für eine App Regeln für eingehenden Datenverkehr zuweisen möchten, verwenden Sie Zugriffseinschränkungen. 

## <a name="combining-features"></a>Kombinieren von Funktionen 

Die Funktionen, die für den mehrinstanzenfähigen Dienst notiert sind, können zusammen verwendet werden, um komplexere Anwendungsfälle zu lösen. Zwei der häufigsten Anwendungsfälle sind hier beschrieben, aber sie sind nur Beispiele. Sobald Sie verstanden haben, was die verschiedenen Funktionen tun, können Sie nahezu alle Ihrer Systemarchitekturanforderungen lösen.

### <a name="inject-app-into-a-vnet"></a>Einfügen einer App in ein VNet

Eine häufige Anforderung besteht darin, wie Sie Ihre App in einem VNet platzieren. Ein Platzieren Ihrer App in einem VNet bedeutet, dass sich die eingehenden und ausgehenden Endpunkte für die App in einem VNet befinden. Die ASE bietet die beste Lösung für dieses Problem, aber Sie können das beste Ergebnis hinsichtlich der Erfordernisse im mehrinstanzenfähigen Dienst erzielen, indem Sie Funktionen kombinieren. Beispielsweise können Sie reine Intranetanwendungen mit privaten ein- und ausgehenden Adressen hosten durch:

* Erstellen eines Anwendungsgateways mit privaten eingehenden und ausgehenden Adressen
* Schützen von eingehendem Datenverkehr an Ihre App mit Dienstendpunkten 
* Verwenden der neuen VNet-Integration, sodass sich das Back-End Ihrer App nicht in Ihrem VNet befindet 

Diese Art der Bereitstellung bringt Ihnen weder eine dedizierte Adresse für ausgehenden Datenverkehr in das Internet noch die Möglichkeit, den gesamten ausgehenden Datenverkehr aus Ihrer App zu sperren.  Diese Art der Bereitstellung eröffnet Ihnen viele der Möglichkeiten, die Sie andernfalls nur mit einer ASE erhalten. 

### <a name="create-multi-tier-applications"></a>Erstellen von Anwendungen mit mehreren Ebenen

Eine Anwendung mit mehreren Ebene ist eine Anwendung, in der nur über die Front-End-Ebene auf die API-Back-End-Apps zugegriffen werden kann. Eine App mit mehreren Ebenen kann auf zwei Arten erstellt werden. Beide beginnen mit der Verwendung von VNet-Integration, um Ihre Front-End-Web-App mit einem Subnetz in einem VNet zu verbinden. Dies ermöglicht Ihrer Web-App das Ausführen von Aufrufen in Ihrem VNet. Nachdem Ihre Front-End-App mit dem VNet verbunden ist, müssen Sie auswählen, wie Sie den Zugriff auf Ihre API-Anwendung sperren möchten.  Ihre Möglichkeiten:

* Hosten von sowohl Front-End- als auch API-App in derselben ILB-ASE und Verfügbarmachen der Front-End-App im Internet mit einem Anwendungsgateway
* Hosten des Front-Ends im mehrinstanzenfähigen Dienst und des Back-Ends in einer ILB-ASE
* Hosten von sowohl Front-End- als auch API-App im mehrinstanzenfähigen Dienst

Wenn Sie sowohl die Front-End- als auch die API-App für eine Anwendung mit mehreren Ebenen hosten, haben Sie folgende Möglichkeiten:

Verfügbarmachen Ihrer API-Anwendung mit privaten Endpunkten in Ihrem VNet

![App mit zwei Ebenen und privaten Endpunkten](media/networking-features/multi-tier-app-private-endpoint.png)

Verwenden von Dienstendpunkten, um eingehenden Datenverkehr an Ihre API-App dadurch zu schützen, dass er nur aus dem Subnetz stammt, das für Ihre Front-End-Web-App verwendet wird

![Mit Dienstendpunkten geschützte App](media/networking-features/multi-tier-app.png)

Die Vor- und Nachteile beider Methoden sind:

* Mit Dienstendpunkten müssen Sie nur den Datenverkehr zu Ihrer API-App in das Integrationssubnetz sichern. Hierdurch wird die API-App gesichert, aber es bestünde immer noch die Möglichkeit zur Datenexfiltration aus Ihrer Front-End-App in andere Apps im App Service.
* Mit privaten Endpunkten verfügen Sie über zwei aktive Subnetze. Dies erhöht die Komplexität. Außerdem ist der private Endpunkt eine Ressource der obersten Ebene, die den Verwaltungsaufwand erhöht. Der Vorteil bei der Verwendung privater Endpunkte besteht darin, dass eine Datenexfiltration ausgeschlossen ist. 

Beide Methoden funktionieren mit mehreren Front-Ends. In kleineren Szenarien lassen sich Dienstendpunkte wesentlich einfacher verwenden, weil Sie Dienstendpunkte einfach für die API-App im Front-End-Integrationssubnetz aktivieren. Wenn Sie weitere Front-End-Apps hinzufügen, müssen Sie jede API-App so anpassen, dass sie über Dienstendpunkte in dem Integrationssubnetz verfügt. Mit privaten Endpunkten haben Sie eine höhere Komplexität, müssen aber nichts bei Ihren API-Apps ändern, nachdem Sie einen privaten Endpunkt festgelegt haben. 

### <a name="line-of-business-applications"></a>Branchenanwendungen

Branchenanwendungen (Line-of-Business, LOB) sind interne Anwendungen, die in der Regel nicht für den Zugriff über das Internet verfügbar gemacht werden. Diese Anwendungen werden aus Unternehmensnetzwerken heraus aufgerufen, in denen sich der Zugriff streng kontrollieren lässt. Wenn Sie eine ILB-ASE verwenden, lassen sich Ihre Branchenanwendungen einfach hosten. Wenn Sie den mehrinstanzenfähigen Dienst verwenden, können Sie entweder private Endpunkte oder Dienstendpunkte in Kombination mit einem Application Gateway verwenden. Es gibt zwei Gründe, um ein Application Gateway mit Dienstendpunkten anstelle privater Endpunkte zu verwenden:

* Sie benötigen WAF-Schutz für Ihre Branchenanwendungen.
* Sie möchten einen Lastenausgleich für mehrere Instanzen Ihrer Branchenanwendungen ausführen.

Wenn nichts davon der Fall ist, ist es besser, private Endpunkte zu verwenden. Wenn private Endpunkte in App Service verfügbar sind, können Sie Ihre Apps über private Adressen in Ihrem VNet verfügbar machen. Der private Endpunkt, den Sie in Ihrem VNet platzieren, kann über ExpressRoute- und VPN-Verbindungen erreicht werden. Das Konfigurieren privater Endpunkte macht Ihre Apps über eine private Adresse verfügbar, aber Sie müssen das DNS so konfigurieren, dass diese Adresse von lokalen Standorten aus erreichbar ist. Damit dies funktioniert, müssen Sie die private Azure DNS-Zone, die Ihre privaten Endpunkten enthält, an Ihre lokalen DNS-Server weiterleiten. Private Azure DNS-Zonen unterstützen keine Zonenweiterleitung, aber Sie können dies unterstützen, indem Sie einen DNS-Server zu diesem Zweck verwenden. Diese Vorlage, [DNS-Weiterleitung](https://azure.microsoft.com/resources/templates/301-dns-forwarder/), vereinfacht das Weiterleiten Ihrer privaten Azure DNS-Zone an Ihre lokalen DNS-Server.

## <a name="app-service-ports"></a>App Service-Ports

Wenn Sie den App Service überprüfen, finden Sie mehrere Ports, die für eingehende Verbindungen verfügbar gemacht sind. Es gibt keine Möglichkeit, den Zugriff auf diese Ports im mehrinstanzenfähigen Dienst zu blockieren oder zu kontrollieren. Folgende Ports sind verfügbar gemacht:

| Zweck | Ports |
|----------|-------------|
|  HTTP/HTTPS  | 80, 443 |
|  Verwaltung | 454, 455 |
|  FTP/FTPS    | 21, 990, 10001-10020 |
|  Remotedebuggen in Visual Studio  |  4020, 4022, 4024 |
|  Web Deploy-Dienst | 8172 |
|  Infrastrukturverwendung | 7654, 1221 |

<!--Links-->
[appassignedaddress]: https://docs.microsoft.com/azure/app-service/configure-ssl-certificate
[iprestrictions]: https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions
[serviceendpoints]: https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions
[hybridconn]: https://docs.microsoft.com/azure/app-service/app-service-hybrid-connections
[vnetintegrationp2s]: https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet
[vnetintegration]: https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet
[networkinfo]: https://docs.microsoft.com/azure/app-service/environment/network-info
[appgwserviceendpoints]: https://docs.microsoft.com/azure/app-service/networking/app-gateway-with-service-endpoints
[privateendpoints]: https://docs.microsoft.com/azure/app-service/networking/private-endpoint
