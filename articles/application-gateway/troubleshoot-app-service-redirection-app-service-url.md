---
title: Beheben von Problemen bei der Umleitung zur App Service-URL
titleSuffix: Azure Application Gateway
description: Dieser Artikel enthält Informationen dazu, wie sich das Umleitungsproblem beheben lässt, wenn Azure Application Gateway mit Azure App Service verwendet wird.
services: application-gateway
author: jaesoni
ms.service: application-gateway
ms.topic: troubleshooting
ms.date: 04/15/2021
ms.author: jaysoni
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: d2291bc88a90a703239764a2d5fda9b2889a7af7
ms.sourcegitcommit: 52491b361b1cd51c4785c91e6f4acb2f3c76f0d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108319663"
---
# <a name="troubleshoot-app-service-issues-in-application-gateway"></a>Behandeln von App Service-Problemen in Application Gateway

Es wird beschrieben, wie Sie Probleme diagnostizieren und beheben, die ggf. auftreten, wenn Azure App Service als Back-End-Ziel mit Azure Application Gateway verwendet wird.

## <a name="overview"></a>Übersicht

In diesem Artikel erfahren Sie, wie Sie die folgenden Probleme beheben:

* Die App-Dienst-URL wird im Browser verfügbar gemacht, wenn eine Umleitung durchgeführt wird.
* Die ARRAffinity-Cookiedomäne des App-Diensts ist auf den App-Dienst-Hostnamen „example.azurewebsites.net“ festgelegt, und nicht auf den ursprünglichen Host.

Wenn eine Back-End-Anwendung eine Umleitungsantwort sendet, empfiehlt es sich u.U., den Client an eine andere als die von der Back-End-Anwendung angegebene URL umzuleiten. Dies kann ratsam sein, wenn ein App-Dienst hinter einem Anwendungsgateway gehostet wird und erfordert, dass der Client eine Umleitung zu seinem relativen Pfad durchführt. Ein Beispiel ist eine Umleitung von „contoso.azurewebsites.net/path1“ zu „contoso.azurewebsites.net/path2“. 

Wenn der App-Dienst eine Umleitungsantwort sendet, verwendet er im Adressheader seiner Antwort den gleichen Hostnamen wie in der Anforderung, die er vom Anwendungsgateway empfängt. Der Client sendet die Anforderung beispielsweise direkt an „contoso.azurewebsites.net/path2“ und durchläuft nicht das Anwendungsgateway „contoso.com/path2“. Es kann ratsam sein, das Anwendungsgateway nicht zu umgehen.

Dieses Problem kann folgende Hauptursachen haben:

- Sie haben für Ihren App-Dienst eine Umleitung konfiguriert. Diese kann im einfachsten Fall auf einen nachgestellten Schrägstrich zurückzuführen sein, der der Anforderung hinzugefügt wurde.
- Die Umleitung wird durch die Nutzung der Azure Active Directory-Authentifizierung ausgelöst.

Wenn Sie App-Dienste hinter einem Anwendungsgateway verwenden, unterscheidet sich der Domänenname, der dem Anwendungsgateway (example.com) zugeordnet ist, zudem vom Domänennamen des App-Diensts (z. B. „example.azurewebsites.net“). Der Domänenwert für das vom App-Dienst festgelegte ARRAffinity-Cookie enthält den Domänennamen „example.azurewebsites.net“. Dies ist nicht erwünscht. Der ursprüngliche Hostname „example.com“ sollte dem Domänennamenwert im Cookie entsprechen.

## <a name="sample-configuration"></a>Beispielkonfiguration

- HTTP-Listener: einfach oder mehrere Standorte
- Back-End-Adresspool: App Service
- HTTP-Einstellungen: **Pick Hostname from Backend Address** (Hostnamen aus Back-End-Adresse auswählen) aktiviert
- Test: **Pick Hostname from HTTP Settings** (Hostnamen aus HTTP-Einstellungen auswählen) aktiviert

## <a name="cause"></a>Ursache

Da App Service ein mehrinstanzenfähiger Dienst ist, nutzt er den Hostheader in der Anforderung zur Weiterleitung der Anforderung an den richtigen Endpunkt. Der Standarddomänenname von App Service („*.azurewebsites.net“, z. B. „contoso.azurewebsites.net“) unterscheidet sich vom Domänennamen des Anwendungsgateways (z. B. „contoso.com“). 

Die ursprüngliche Anforderung vom Client weist als Hostname den Domänennamen des Anwendungsgateways („contoso.com“) auf. Sie müssen das Anwendungsgateway so konfigurieren, dass der Hostname in der ursprünglichen Anforderung in den Hostnamen des App-Diensts geändert wird, wenn die Anforderung an das Back-End des App-Diensts geleitet wird. Verwenden Sie den Switch **Hostnamen aus Back-End-Adresse auswählen** in der HTTP-Einstellungskonfiguration des Anwendungsgateways. Verwenden Sie den Switch **Hostnamen aus Back-End-HTTP-Einstellungen auswählen** in der Integritätstestkonfiguration.



![Änderung des Hostnamens durch das Anwendungsgateway](./media/troubleshoot-app-service-redirection-app-service-url/appservice-1.png)

Wenn der App-Dienst eine Umleitung durchführt, wird anstelle des ursprünglichen Hostnamens „contoso.com“ der außer Kraft gesetzte Hostname „contoso.azurewebsites.net“ im Adressheader verwendet, falls keine andere Konfiguration vorgenommen wurde. Sehen Sie sich die folgende Beispielanforderung und die Antwortheader an.
```
## Request headers to Application Gateway:

Request URL: http://www.contoso.com/path

Request Method: GET

Host: www.contoso.com

## Response headers:

Status Code: 301 Moved Permanently

Location: http://contoso.azurewebsites.net/path/

Server: Microsoft-IIS/10.0

Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=contoso.azurewebsites.net

X-Powered-By: ASP.NET
```
Beachten Sie im vorherigen Beispiel, dass der Antwortheader über den Statuscode 301 für die Umleitung verfügt. Der Adressheader enthält den Hostnamen des App-Diensts und nicht den ursprünglichen Hostnamen `www.contoso.com`.

## <a name="solution-rewrite-the-location-header"></a>Lösung: Erneutes Generieren des Adressheaders

Legen Sie den Hostnamen im Adressheader auf den Domänennamen des Anwendungsgateways fest. Erstellen Sie hierzu eine [Regel zum erneuten Generieren](./rewrite-http-headers-url.md) mit einer Bedingung, die prüft, ob der Adressheader in der Antwort „azurewebsites.net“ enthält. Außerdem muss eine Aktion zum erneuten Generieren des Adressheaders durchgeführt werden, damit der Hostname des Anwendungsgateways vorhanden ist. Weitere Informationen finden Sie in der Anleitung unter [Ändern einer Umleitungs-URL](./rewrite-http-headers-url.md#modify-a-redirection-url).

> [!NOTE]
> Die Unterstützung für das erneute Generieren von HTTP-Headern ist nur für die [Standard_v2- und WAF_v2-SKU](./application-gateway-autoscaling-zone-redundant.md) von Application Gateway verfügbar. Es wird empfohlen, für das erneute Schreiben von Headern und andere [erweiterte Funktionen](./application-gateway-autoscaling-zone-redundant.md#feature-comparison-between-v1-sku-and-v2-sku), die mit der v2-SKU verfügbar sind, zu v2 zu [migrieren](./migrate-v1-v2.md).

## <a name="alternate-solution-use-a-custom-domain-name"></a>Alternativlösung: Verwenden Sie einen benutzerdefinierten Domänennamen

Die App Service-Custom Domain von App Service ist eine weitere Lösung, um den Datenverkehr immer an Application Gateway Domänennamen (in unserem Beispiel `www.contoso.com`) umzuleiten. Diese Konfiguration dient auch als Lösung für das ARR Affinitycookie-Problem. Standardmäßig ist die ARRAffinity-Cookiedomäne auf den App Service-Standardhostnamen (example.azurewebsites.net) anstatt auf den Domänennamen des Application Gateway festgelegt. Daher lehnt der Browser in solchen Fällen das Cookie aufgrund des Unterschieds in den Domänennamen der Anforderung und des Cookies ab.

Sie können die angegebenen Methoden sowohl für die Probleme mit Redirection als auch ARRAffinity-Cookiedomänenkonflikten befolgen. Für diese Methode benötigen Sie Zugriff auf die DNS-Zone Ihrer benutzerdefinierten Domäne.

**Schritt 1**: Legen Sie eine Custom Domain in App Service fest und überprüfen Sie den Domänenbesitz, indem Sie die [DNS-Einträge CNAME & TXT](../app-service/app-service-web-tutorial-custom-domain.md#get-a-domain-verification-id) hinzufügen.
Die Datensätze würden in etwa wie die folgenden aussehen
-  `www.contoso.com` IN CNAME `contoso.azurewebsite.net`
-  `asuid.www.contoso.com` IN TXT „`<verification id string>`“


**Schritt 2**: Der CNAME-Eintrag im vorherigen Schritt wurde nur für die Domänenüberprüfung benötigt. Letztendlich benötigen wir den Datenverkehr, der über die Application Gateway läuft. Sie können daher den CNAME von `www.contoso.com` jetzt ändern, um auf Application Gateway FQDN zu verweisen. Navigieren Sie zum Festlegen eines FQDN für Ihre Application Gateway zu ihrer öffentlichen IP-Adressressource, und weisen Sie ihr eine „DNS-Namensbezeichnung“ zu. Der aktualisierte CNAME-Eintrag sollte nun wie folgt aussehen: 
-  `www.contoso.com` IN CNAME `contoso.eastus.cloudapp.azure.com`


**Schritt 3**: Deaktivieren Sie „Hostnamen aus Back-End-Adresse auswählen“ für die zugeordnete HTTP-Einstellung.

Verwenden Sie in PowerShell nicht den Switch `-PickHostNameFromBackendAddress` im Befehl `Set-AzApplicationGatewayBackendHttpSettings`.


**Schritt 4**: Damit die Tests das Back-End als fehlerfrei und als betriebsbereiten Datenverkehr bestimmen können, legen Sie einen benutzerdefinierten Integritätstest mit dem Hostfield als benutzerdefinierte oder Standarddomäne des App Service fest.

Verwenden Sie in PowerShell nicht den `-PickHostNameFromBackendHttpSettings`-Schalter im `Set-AzApplicationGatewayProbeConfig`-Befehl, sondern verwenden Sie entweder die benutzerdefinierte Domäne oder die Standarddomäne des App Service im -HostName des Tests.

Verwenden Sie das folgende PowerShell-Beispielskript, um die obigen Schritte für ein vorhandenes Setup mit PowerShell zu implementieren. Beachten Sie, dass die **-PickHostname**-Switches in der Konfiguration für die Test- und HTTP-Einstellungen nicht verwendet wurden.

```azurepowershell-interactive
$gw=Get-AzApplicationGateway -Name AppGw1 -ResourceGroupName AppGwRG
Set-AzApplicationGatewayProbeConfig -ApplicationGateway $gw -Name AppServiceProbe -Protocol Http -HostName "example.azurewebsites.net" -Path "/" -Interval 30 -Timeout 30 -UnhealthyThreshold 3
$probe=Get-AzApplicationGatewayProbeConfig -Name AppServiceProbe -ApplicationGateway $gw
Set-AzApplicationGatewayBackendHttpSettings -Name appgwhttpsettings -ApplicationGateway $gw -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 30
Set-AzApplicationGateway -ApplicationGateway $gw
```
  ```
  ## Request headers to Application Gateway:

  Request URL: http://www.contoso.com/path

  Request Method: GET

  Host: www.contoso.com

  ## Response headers:

  Status Code: 301 Moved Permanently

  Location: http://www.contoso.com/path/

  Server: Microsoft-IIS/10.0

  Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=www.contoso.com

  X-Powered-By: ASP.NET
  ```
  ## <a name="next-steps"></a>Nächste Schritte

Wenn sich das Problem mit den obigen Schritten nicht beheben lässt, sollten Sie ein [Supportticket](https://azure.microsoft.com/support/options/) erstellen.
