---
title: Application Gateway-Komponenten
description: Dieser Artikel bietet Informationen zu den verschiedenen Komponenten in einem Anwendungsgateway.
services: application-gateway
author: surajmb
ms.service: application-gateway
ms.topic: conceptual
ms.date: 08/21/2020
ms.author: surmb
ms.openlocfilehash: ebd06b0b78ee511dce535ff4220df03087fb6906
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "88723315"
---
# <a name="application-gateway-components"></a>Application Gateway-Komponenten

 Ein Anwendungsgateway dient als zentraler Kontaktpunkt für Clients. Es verteilt eingehenden Anwendungsdatenverkehr über mehrere Back-End-Pools hinweg, z.B. virtuelle Azure-Computer, VM-Skalierungsgruppen, Azure App Service oder lokale bzw. externe Server. Zum Verteilen von Datenverkehr verwendet ein Anwendungsgateway verschiedene Komponenten, die in diesem Artikel beschrieben werden.

![In einem Anwendungsgateway verwendete Komponenten](./media/application-gateway-components/application-gateway-components.png)

## <a name="frontend-ip-addresses"></a>Front-End-IP-Adressen

Eine Front-End-IP-Adresse ist die IP-Adresse, die einem Anwendungsgateway zugeordnet ist. Sie können ein Anwendungsgateway mit einer öffentlichen IP-Adresse, mit einer privaten IP-Adresse oder mit beidem konfigurieren. Ein Anwendungsgateway unterstützt eine öffentliche oder eine private IP-Adresse. Ihr virtuelles Netzwerk und die öffentliche IP-Adresse müssen sich am gleichen Standort befinden wie Ihr Anwendungsgateway. Eine Front-End-IP-Adresse wird nach dem Erstellen einem Listener zugeordnet.

### <a name="static-versus-dynamic-public-ip-address"></a>Statische und dynamische öffentliche IP-Adresse im Vergleich

Die V2-SKU von Azure Application Gateway kann so konfiguriert werden, dass sowohl statische interne IP-Adressen als auch statische öffentliche IP-Adressen oder ausschließlich Letztere konfiguriert werden. Nur statische interne IP-Adressen werden nicht unterstützt.

Die V1-SKU kann so konfiguriert werden, dass sowohl statische oder dynamische interne IP-Adressen als auch dynamische öffentliche IP-Adressen verwendet werden. Die dynamische IP-Adresse der Application Gateway-Instanz ändert sich nicht, solange das Gateway aktiv ist. Sie kann sich nur dann ändern, wenn Sie das Gateway anhalten oder starten. Wenn beispielsweise Systemfehler auftreten oder Updates und Azure-Hostupdates durchgeführt werden, ändert sich die IP-Adresse nicht. 

Der einem Anwendungsgateway zugeordnete DNS-Name ändert sich während des Lebenszyklus des Gateways nicht. Daher wird empfohlen, einen CNAME-Alias zu verwenden und damit auf die DNS-Adresse des Anwendungsgateways zu verweisen.

## <a name="listeners"></a>Listener

Ein Listener ist eine logische Entität, die auf eingehende Verbindungsanforderungen prüft. Ein Listener akzeptiert eine Anforderung, wenn das Protokoll, der Port, der Hostname und die IP-Adresse mit den entsprechenden Elementen der Listenerkonfiguration übereinstimmen.

Bevor Sie ein Anwendungsgateway verwenden, müssen Sie mindestens einen Listener hinzufügen. An ein Anwendungsgateway können mehrere Listener angefügt sein, und sie können für dasselbe Protokoll verwendet werden.

Wenn ein Listener eingehende Anforderungen von Clients erkannt hat, leitet das Anwendungsgateway diese Anforderungen an in der Regel konfigurierte Mitglieder im Back-End-Pool weiter.

Listener unterstützen die folgenden Ports und Protokolle.

### <a name="ports"></a>Ports

Ein Port wird von einem Listener auf die Clientanforderung überwacht. Sie können Ports von 1 bis 65502 für die v1-SKU und 1 bis 65199 für die v2-SKU konfigurieren.

### <a name="protocols"></a>Protokolle

Application Gateway unterstützt vier Protokolle: HTTP, HTTPS, HTTP/2 und WebSocket:
>[!NOTE]
>Die Unterstützung des HTTP/2-Protokolls ist nur für Clients verfügbar, die mit Application Gateway-Listenern verbunden sind. Die Kommunikation mit Back-End-Serverpools erfolgt immer über HTTP/1.1. Die HTTP/2-Unterstützung ist standardmäßig deaktiviert. Sie können es aktivieren.

- Sie geben die Option für HTTP- und HTTPS-Protokolle in der Listenerkonfiguration an.
- Unterstützung für [die Protokolle WebSocket und HTTP/2](features.md#websocket-and-http2-traffic) wird systemintern bereitgestellt, und [WebSocket-Unterstützung](application-gateway-websocket.md) ist standardmäßig aktiviert. Die WebSocket-Unterstützung kann von Benutzern nicht selektiv aktiviert oder deaktiviert werden. Sie können das WebSocket-Protokoll sowohl mit HTTP- als auch mit HTTPS-Listenern verwenden.

Verwenden Sie einen HTTPS-Listener für die TLS-Terminierung. Ein HTTPS-Listener lagert die Ver- und Entschlüsselung an Ihr Anwendungsgateway aus, sodass Ihre Webserver mit diesem Overhead nicht belastet werden.

### <a name="custom-error-pages"></a>Benutzerdefinierte Fehlerseiten

Mit Application Gateway können Sie benutzerdefinierte Fehlerseiten erstellen, anstatt Standardfehlerseiten anzuzeigen. Sie können für eine benutzerdefinierte Fehlerseite Ihr eigenes Branding und Layout verwenden. Application Gateway zeigt eine benutzerdefinierte Fehlerseite an, wenn eine Anforderung das Back-End nicht erreichen kann.

Weitere Informationen finden Sie unter [Benutzerdefinierte Application Gateway-Fehlerseiten](custom-error.md).

### <a name="types-of-listeners"></a>Arten von Listenern

Es gibt zwei Arten von Listenern:

- **Basic**. Diese Listener lauschen an einer einzelnen Domänenwebsite, wo eine einzelne DNS-Zuordnung zur IP-Adresse des Anwendungsgateways vorliegt. Diese Listenerkonfiguration ist erforderlich, wenn Sie eine einzelne Website hinter einem Anwendungsgateway hosten.

- **Mehrere Websites**. Diese Listenerkonfiguration ist erforderlich, wenn Sie das Routing basierend auf dem Host- oder Domänennamen für mehr als eine Webanwendung auf demselben Anwendungsgateway konfigurieren möchten. Sie können eine effizientere Topologie für Ihre Bereitstellungen konfigurieren, indem Sie bis zu hundert Websites zu einem einzigen Anwendungsgateway hinzufügen. Jede Website kann an ihren eigenen Back-End-Pool weitergeleitet werden. Beispielsweise können die drei Domänen „contoso.com“, „fabrikam.com“ und „adatum.com“ auf die IP-Adresse des Anwendungsgateways verweisen. Sie erstellen drei [Listener vom Typ „Mehrere Websites“](multiple-site-overview.md) und konfigurieren jeden Listener für den jeweiligen Port und die jeweilige Protokolleinstellung. 

    Sie können auch Platzhalterhostnamen in einem Listener für mehrere Standorte und bis zu fünf Hostnamen pro Listener definieren. Weitere Informationen finden Sie unter [Platzhalterhostnamen in Listenern (Vorschauversion)](multiple-site-overview.md#wildcard-host-names-in-listener-preview).

    Weitere Informationen zum Konfigurieren eines Listener für mehrere Standorte finden Sie unter [Tutorial: Erstellen und Konfigurieren eines Anwendungsgateways als Host mehrerer Websites über das Azure-Portal](create-multiple-sites-portal.md).

Nachdem Sie einen Listener erstellt haben, ordnen Sie ihm eine Anforderungsroutingregel zu. Diese Regel bestimmt, wie die vom Listener empfangene Anforderung an das Back-End weitergeleitet werden soll. Die Anforderungsroutingregel enthält auch den Back-End-Pool für die Weiterleitung sowie die HTTP-Einstellung, in der der Back-End-Port, das Protokoll usw. angegeben werden.

## <a name="request-routing-rules"></a>Anforderungsroutingregeln

Eine Anforderungsroutingregel ist eine wichtige Komponente eines Anwendungsgateways, da sie bestimmt, wie Datenverkehr im Listener weitergeleitet wird. Diese Regel bindet den Listener, den Back-End-Serverpool und die Back-End-HTTP-Einstellungen.

Wenn ein Listener eine Anforderung akzeptiert, leitet die Anforderungsroutingregel die Anwendung an das Back-End weiter oder an einen anderen Ort um. Wenn die Anforderung an das Back-End weitergeleitet wird, definiert die Anforderungsroutingregel, an welchen Back-End-Serverpool die Weiterleitung erfolgen soll. Die Anforderungsroutingregel legt fest, ob die Header in der Anforderung neu geschrieben werden sollen. Ein Listener kann mit genau einer Regel verknüpft werden.

Es gibt zwei Arten von Anforderungsroutingregeln:

- **Basic**. Alle Anforderungen für den zugeordneten Listener (Beispiel: blog.contoso.com/*) werden mit der zugeordneten HTTP-Einstellung an den zugeordneten Back-End-Pool weitergeleitet.

- **Pfadbasiert**. Mit einer solchen Routingregel können Sie Anforderungen für den zugeordneten Listener basierend auf der URL in der Anforderung an einen bestimmten Back-End-Pool weiterleiten. Wenn der Pfad der URL in einer Anforderung dem Pfadmuster in einer pfadbasierten Regel entspricht, wird die Anforderung anhand dieser Regel geroutet. Das Pfadmuster wird nur auf den URL-Pfad angewendet, nicht auf die zugehörigen Abfrageparameter. Wenn der URL-Pfad einer Anforderung in einem Listener keiner pfadbasierten Regel entspricht, wird die Anforderung mit den HTTP-Standardeinstellungen an den standardmäßigen Back-End-Pool weitergeleitet.

Weitere Informationen finden Sie unter [URL-basiertes Routing](url-route-overview.md).

### <a name="redirection-support"></a>Umleitungsunterstützung

Die Routingregel für Anforderungen ermöglicht es Ihnen auch, Datenverkehr im Anwendungsgateway umzuleiten. Vielmehr handelt es sich um einen generischen Umleitungsmechanismus, sodass Sie Umleitungen für jeden Port durchführen können, den Sie mithilfe von Regeln definieren.

Sie können als Umleitungsziel einen anderen Listener (wodurch die automatische Umleitung von HTTP zu HTTPS ermöglicht werden kann) oder eine externe Website auswählen. Sie können außerdem auswählen, ob die Umleitung temporär oder dauerhaft erfolgen soll, oder angeben, dass der URI-Pfad und die Abfragezeichenfolge an die Umleitungs-URL angefügt werden sollen.

Weitere Informationen finden Sie unter [Umleiten von Datenverkehr im Application Gateway](redirect-overview.md).

### <a name="rewrite-http-headers-and-url"></a>Neuschreibung von HTTP-Headern und der URL

Mithilfe von Neuschreibungsregeln können Sie HTTP(S)-Anforderungs- und -Antwortheader sowie den URL-Pfad und Abfragezeichenfolgenparameter hinzufügen, entfernen oder aktualisieren, während die Anforderungs- und Antwortpakete über das Anwendungsgateway zwischen dem Client und den Back-End-Pools weitergeleitet werden.

Die Header und URL-Parameter können auf statische Werte oder auf andere Header und Servervariablen festgelegt werden. Dies ist hilfreich bei wichtigen Anwendungsfällen, z.B. beim Extrahieren von Client-IP-Adressen, beim Entfernen vertraulicher Informationen zum Back-End, beim Verbessern der Sicherheit usw.

Weitere Informationen finden Sie im Artikel zum [erneuten Generieren von HTTP-Headern und URLs für Ihr Anwendungsgateway](rewrite-http-headers-url.md).

## <a name="http-settings"></a>HTTP-Einstellungen

Ein Anwendungsgateway routet Datenverkehr an die Back-End-Server (diese sind in der Anforderungsroutingregel angegeben, die HTTP-Einstellungen einschließt). Dabei werden Portnummer, Protokoll sowie weitere in dieser Komponente angegebene Einstellungen verwendet.

Die Angaben für den Port und das Protokoll in den HTTP-Einstellungen bestimmen, ob der Datenverkehr zwischen dem Anwendungsgateway und den Back-End-Servern verschlüsselt wird (um End-to-End-TLS bereitzustellen) oder ob er unverschlüsselt bleibt.

Diese Komponente wird außerdem für folgende Zwecke verwendet:

- Bestimmen, ob eine Benutzersitzung durch [cookiebasierte Sitzungsaffinität](features.md#session-affinity) auf demselben Server beibehalten wird.

- Ordnungsgemäßes Entfernen von Mitgliedern des Back-End-Pools über den [Verbindungsausgleich](features.md#connection-draining).

- Zuordnen eines benutzerdefinierten Tests zum Überwachen der Integrität des Back-Ends, Überschreiben von Hostname und -pfad in der Anforderung und benutzerfreundliches Angeben von Einstellungen für das App Service-Back-End mit nur einem Klick.

## <a name="backend-pools"></a>Back-End-Pools

Ein Back-End-Adresspool leitet die Anforderung an Back-End-Server weiter, die sie verarbeiten. Back-End-Pools können Folgendes umfassen:

- NICs
- VM-Skalierungsgruppen
- Öffentliche IP-Adressen
- Interne IP-Adressen
- FQDN
- Mehrinstanzenfähige Back-Ends (z.B. App Service)

Mitglieder des Application Gateway-Back-End-Pools sind nicht an eine Verfügbarkeitsgruppe gebunden. Ein Anwendungsgateway kann mit Instanzen außerhalb des eigenen virtuellen Netzwerks kommunizieren. Mitglieder von Back-End-Pools können auf mehrere Cluster und Rechenzentren verteilt sein oder sich außerhalb von Azure befinden, sofern sie über IP-Konnektivität verfügen.

Wenn Sie interne IP-Adressen als Back-End-Pool-Mitglieder verwenden, ist das [Peering virtueller Netzwerke](../virtual-network/virtual-network-peering-overview.md) oder ein [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) erforderlich. Das Peering virtueller Netzwerke wird unterstützt und bietet Vorteile für den Lastenausgleich von Datenverkehr in anderen virtuellen Netzwerken.

Ein Anwendungsgateway kann auch mit lokalen Servern kommunizieren, wenn diese über Azure ExpressRoute oder VPN-Tunnel verbunden sind und Datenverkehr zulässig ist.

Sie können verschiedene Back-End-Pools für verschiedene Arten von Anforderungen erstellen. Sie können z.B. einen Back-End-Pool für allgemeine Anforderungen und einen anderen für Anforderungen an die Microservices Ihrer Anwendung erstellen.

## <a name="health-probes"></a>Integritätstests

In der Standardeinstellung überwacht ein Anwendungsgateway die Integrität aller Ressourcen in seinem Back-End-Pool und entfernt automatisch alle fehlerhaften Ressourcen. Es überwacht die fehlerhaften Instanzen weiterhin und fügt sie dem fehlerfreien Back-End-Pool hinzu, sobald sie verfügbar sind und auf Integritätsüberprüfungen reagieren.

Zusätzlich zur Nutzung der standardmäßigen Überwachung der Integritätsüberprüfung können Sie die Integritätsüberprüfung auch an die Anforderungen Ihrer Anwendung anpassen. Benutzerdefinierte Überprüfungen ermöglichen Ihnen eine präzisere Kontrolle über die Überwachung des Systemzustands. Bei Verwendung benutzerdefinierter Tests können Sie einen benutzerdefinierten Hostnamen, einen URL-Pfad und ein Testintervall konfigurieren und festlegen, wie viele fehlerhafte Antworten akzeptiert werden, bevor die Back-End-Pool-Instanz als fehlerhaft gekennzeichnet wird. Außerdem können Sie unter anderem benutzerdefinierte Statuscodes und den Abgleich des HTTP-Antworttexts konfigurieren. Wir empfehlen, benutzerdefinierte Überprüfungen zu konfigurieren, um die Integrität jedes Back-End-Pools zu überwachen.

Weitere Informationen finden Sie unter [Überwachen der Integrität Ihres Anwendungsgateways](../application-gateway/application-gateway-probe-overview.md).

## <a name="next-steps"></a>Nächste Schritte

Erstellen eines Anwendungsgateways

* [Im Azure-Portal](quick-create-portal.md)
* [Mit Azure PowerShell](quick-create-powershell.md)
* [Mithilfe der Azure-Befehlszeilenschnittstelle (Azure CLI)](quick-create-cli.md)
