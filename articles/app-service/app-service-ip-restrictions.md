---
title: Azure App Service-Zugriffseinschränkungen
description: Erfahren Sie, wie Sie Ihre App in Azure App Service schützen, indem Sie Zugriffseinschränkungen angeben.
author: ccompy
ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.topic: article
ms.date: 06/06/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 61ccc0231989589836e00088b9ca03d0cb49baca
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2020
ms.locfileid: "92790949"
---
# <a name="azure-app-service-access-restrictions"></a>Azure App Service-Zugriffseinschränkungen

Mit Zugriffseinschränkungen können Sie eine nach Priorität sortierte Zulassen/Ablehnen-Liste definieren, über die der Zugriff auf Ihre App gesteuert wird. Die Liste kann IP-Adressen oder Azure Virtual Network-Subnetze enthalten. Wenn mindestens ein Eintrag vorhanden ist, enthält die Liste am Ende einen impliziten Eintrag vom Typ „Alle ablehnen“.

Die Funktion „Zugriffseinschränkungen“ funktioniert mit allen von App Service gehosteten Workloads. Hierzu gehören Web-Apps, API-Apps, Linux-Apps, Linux-Container-Apps und Functions.

Wenn eine Anforderung an Ihre App gesendet wird, wird die VON-Adresse anhand der IP-Adressregeln in Ihrer Liste mit den Zugriffseinschränkungen ausgewertet. Wenn die VON-Adresse in einem Subnetz enthalten ist, das mit Dienstendpunkten für Microsoft.Web konfiguriert ist, wird das Quellsubnetz mit den Regeln für das virtuelle Netzwerk in Ihrer Liste mit den Zugriffseinschränkungen verglichen. Wenn der Zugriff für die Adresse basierend auf den Regeln in der Liste nicht zugelassen wird, antwortet der Dienst mit dem Statuscode [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403).

Die Funktion „Zugriffseinschränkungen“ wird in den App Service-Front-End-Rollen implementiert, die den Workerhosts vorgeschaltet sind, auf denen Ihr Code ausgeführt wird. Es handelt sich somit bei Zugriffseinschränkungen im Grunde um Zugriffssteuerungslisten für das Netzwerk.

Die Möglichkeit, den Zugriff auf Ihre Web-App auf ein Azure Virtual Network (VNET) zu beschränken, wird als [Dienstendpunkte][serviceendpoints] bezeichnet. Mit Dienstendpunkten können Sie den Zugriff auf einen mehrinstanzenfähigen Dienst auf ausgewählte Subnetze beschränken. Der Datenverkehr für Apps, die in einer App Service-Umgebung gehostet werden, kann dadurch nicht eingeschränkt werden. In einer App Service-Umgebung können Sie den Zugriff auf Ihre App mit IP-Adressregeln steuern.

> [!NOTE]
> Dienstendpunkte müssen sowohl für die Netzwerkseite als auch für den Azure-Dienst aktiviert werden, mit dem die Aktivierung erfolgt. Eine Liste der Azure-Dienste, die Dienstendpunkte unterstützen, finden Sie unter [Virtual Network-Dienstendpunkte](../virtual-network/virtual-network-service-endpoints-overview.md).
>

![Flow von Zugriffseinschränkungen](media/app-service-ip-restrictions/access-restrictions-flow.png)

## <a name="adding-and-editing-access-restriction-rules-in-the-portal"></a>Hinzufügen und Bearbeiten von Regeln für Zugriffseinschränkungen im Portal ##

Um Ihrer App eine Zugriffseinschränkungsregel hinzuzufügen, wählen Sie im Menü **Netzwerk**>**Zugriffseinschränkungen** aus und klicken auf **Zugriffseinschränkungen konfigurieren**.

![App Service-Netzwerkoptionen](media/app-service-ip-restrictions/access-restrictions.png)  

Auf der Benutzeroberfläche für Zugriffseinschränkungen können Sie die Liste mit den Regeln für die Zugriffseinschränkungen überprüfen, die für Ihre App definiert sind.

![Screenshot des Bildschirms „Zugriffseinschränkungen“ im Azure-Portal mit der Liste der Regeln für Zugriffseinschränkungen, die für die ausgewählte App definiert sind.](media/app-service-ip-restrictions/access-restrictions-browse.png)

In der Liste werden alle aktuellen Einschränkungen für Ihre App angezeigt. Wenn eine VNET-Einschränkung für die App festgelegt ist, wird in der Tabelle angezeigt, ob Dienstendpunkte für Microsoft.Web aktiviert sind. Wenn keine definierten Einschränkungen für die App vorhanden sind, kann von überall aus auf die App zugegriffen werden.  

## <a name="adding-ip-address-rules"></a>Hinzufügen von Regeln für IP-Adressen

Klicken Sie auf **[+] Regel hinzufügen** , um eine neue Zugriffseinschränkungsregel hinzuzufügen. Wenn Sie eine Regel hinzufügen, wird sie sofort wirksam. Regeln werden in der Reihenfolge ihrer Priorität erzwungen, wobei mit der niedrigsten Zahl begonnen wird. Implizit gilt die Regel „Alle ablehnen“, die wirksam ist, sobald Sie die erste Regel hinzugefügt haben.

Beim Erstellen einer Regel müssen Sie „Zulassen/Ablehnen“ und außerdem den Typ der Regel auswählen. Sie müssen auch den Prioritätswert festlegen und angeben, worauf der Zugriff beschränkt wird.  Optional können Sie einen Namen und eine Beschreibung für die Regel hinzufügen.  

![Hinzufügen einer IP-Zugriffseinschränkungsregel](media/app-service-ip-restrictions/access-restrictions-ip-add.png)

Um eine auf IP-Adressen basierende Regel hinzuzufügen, wählen Sie einen der Typen IPv4 oder IPv6 aus. Die Notation von IP-Adressen muss für IPv4- und IPv6-Adressen in CIDR-Notation angegeben werden. Zum Angeben einer genauen Adresse können Sie beispielsweise 1.2.3.4/32 verwenden, wobei die ersten vier Oktette für Ihre IP-Adresse stehen und mit /32 die Maske angegeben wird. Die IPv4-CIDR-Notation für alle Adressen ist 0.0.0.0/0. Weitere Informationen zur CIDR-Notation finden Sie unter [Classless Inter-Domain Routing](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing). 

## <a name="service-endpoints"></a>Dienstendpunkte

Dienstendpunkte ermöglichen das Einschränken des Zugriffs auf ausgewählte Subnetze von virtuellen Azure-Netzwerken. Erstellen Sie zum Einschränken des Zugriffs auf ein bestimmtes Subnetz eine Einschränkungsregel vom Typ „Virtual Network“. Sie können das Abonnement, das VNET und das Subnetz auswählen, für die Sie den Zugriff zulassen oder verweigern möchten. Wenn noch keine Dienstendpunkte mit Microsoft.Web für das ausgewählte Subnetz aktiviert sind, werden sie automatisch aktiviert, es sei denn, Sie aktivieren das Kontrollkästchen, über das dies unterbunden wird. Der Fall, in dem dies für die App, jedoch nicht für das Subnetz aktiviert werden soll, hängt größtenteils davon ab, ob Sie über die Berechtigungen verfügen, Dienstendpunkte für das Subnetz zu aktivieren. Wenn eine andere Person Dienstendpunkte für das Subnetz für Sie aktivieren muss, können Sie das Kontrollkästchen aktivieren, sodass die App für Dienstendpunkte konfiguriert wird, mit der Erwartung, dass diese später für das Subnetz aktiviert werden. 

![Hinzufügen einer VNET-Zugriffseinschränkungsregel](media/app-service-ip-restrictions/access-restrictions-vnet-add.png)

Der Zugriff auf Apps, die in einer App Service-Umgebung ausgeführt werden, kann nicht mit Dienstendpunkten eingeschränkt werden. Wenn Ihre App in einer App Service-Umgebung enthalten ist, können Sie den Zugriff darauf mit IP-Zugriffsregeln steuern. 

Bei Dienstendpunkten können Sie Ihre App mit Anwendungsgateways oder anderen WAF-Geräten konfigurieren. Sie können auch Anwendungen mit mehreren Ebenen und sicheren Back-Ends konfigurieren. Weitere Informationen zu diesen Möglichkeiten finden Sie unter [App Service-Netzwerkfunktionen](networking-features.md) und [Application Gateway-Integration mit Dienstendpunkten](networking/app-gateway-with-service-endpoints.md).

> [!NOTE]
> - Dienstendpunkte werden derzeit für Web-Apps nicht unterstützt, die eine virtuelle IP (VIP) mit IP-SSL verwenden.
> - Der Grenzwert für IP- oder Dienstendpunkteinschränkungen liegt bei 512 Zeilen. Wenn Sie mehr als 512 Zeilen an Einschränkungen benötigen, sollten Sie ein eigenständiges Sicherheitsprodukt in Erwägung ziehen, z. B. Azure Front Door, Azure Application Gateway oder Web Application Firewall (WAF).
>

## <a name="managing-access-restriction-rules"></a>Verwalten von Zugriffseinschränkungsregeln

Sie können auf eine beliebige Zeile klicken, um eine vorhandene Zugriffseinschränkungsregel zu bearbeiten. Änderungen sind sofort wirksam. Dies gilt auch für Änderungen der Prioritätsreihenfolge.

![Screenshot des Dialogfelds „IP-Einschränkung bearbeiten“ im Azure-Portal mit den Feldern für eine vorhandene Regel für Zugriffseinschränkungen.](media/app-service-ip-restrictions/access-restrictions-ip-edit.png)

Wenn Sie eine Regel bearbeiten, können Sie den Typ nicht zwischen einer IP-Adressregel und einer Regel für virtuelle Netzwerke ändern. 

![Screenshot des Dialogfelds „IP-Einschränkung bearbeiten“ im Azure-Portal mit den Einstellungen für eine Regel für virtuelle Netzwerke.](media/app-service-ip-restrictions/access-restrictions-vnet-edit.png)

Klicken Sie zum Löschen einer Regel für die entsprechende Regel auf **...** und dann auf **Entfernen**.

![Löschen einer Zugriffseinschränkungsregel](media/app-service-ip-restrictions/access-restrictions-delete.png)

## <a name="blocking-a-single-ip-address"></a>Blockieren einer einzelnen IP-Adresse ##

Wenn Sie die erste Einschränkungsregel für IP-Adressen hinzufügen, fügt der Dienst eine explizite Regel **Alle ablehnen** mit der Priorität 2147483647 hinzu. In der Praxis wird die explizite Regel **Alle ablehnen** als letzte Regel ausgeführt und blockiert den Zugriff auf alle IP-Adressen, die nicht mit einer Regel des Typs **Zulassen** explizit zugelassen sind.

In einem Szenario, in dem Benutzer eine einzelne IP-Adresse oder einen IP-Adressblock explizit blockieren, aber den Zugriff für alles andere zulassen möchten, muss eine explizite Regel **Alle zulassen** hinzugefügt werden.

![Blockieren einer einzelnen IP-Adresse](media/app-service-ip-restrictions/block-single-address.png)

## <a name="scm-site"></a>SCM-Website 

Neben der Möglichkeit, den Zugriff auf Ihre App zu steuern, können Sie auch den Zugriff auf die von der App verwendete SCM-Website einschränken. Die SCM-Website ist der Web Deploy-Endpunkt und zudem die Kudu-Konsole. Sie können die Zugriffseinschränkungen für die SCM-Website getrennt von der App zuweisen oder die gleichen Einschränkungen für die App und die SCM-Website verwenden. Wenn Sie das Kontrollkästchen aktivieren, damit die gleichen Einschränkungen wie für die App gelten, wird alles ausgeblendet. Wenn Sie das Kontrollkästchen deaktivieren, werden alle vorherigen Einstellungen für die SCM-Website angewandt. 

![Screenshot des Bildschirms „Zugriffseinschränkungen“ im Azure-Portal, der zeigt, dass für die SCM-Website oder die App keine Zugriffseinschränkungen festgelegt sind.](media/app-service-ip-restrictions/access-restrictions-scm-browse.png)

## <a name="programmatic-manipulation-of-access-restriction-rules"></a>Programmgesteuerte Bearbeitung von Zugriffseinschränkungsregeln ##

[Azure CLI](/cli/azure/webapp/config/access-restriction?view=azure-cli-latest) und [Azure PowerShell](/powershell/module/Az.Websites/Add-AzWebAppAccessRestrictionRule?view=azps-3.1.0) besitzen Unterstützung für die Bearbeitung von Zugriffseinschränkungen. Beispiel für das Hinzufügen einer Zugriffseinschränkung mithilfe von Azure CLI:

```azurecli-interactive
az webapp config access-restriction add --resource-group ResourceGroup --name AppName \
    --rule-name 'IP example rule' --action Allow --ip-address 122.133.144.0/24 --priority 100
```
Beispiel für das Hinzufügen einer Zugriffseinschränkung mithilfe von Azure PowerShell:

```azurepowershell-interactive
Add-AzWebAppAccessRestrictionRule -ResourceGroupName "ResourceGroup" -WebAppName "AppName"
    -Name "Ip example rule" -Priority 100 -Action Allow -IpAddress 122.133.144.0/24
```

Sie können die Werte auch manuell per PUT-Vorgang der [Azure-REST-API](/rest/api/azure/) in der App-Konfiguration in Resource Manager festlegen oder mithile einer Azure Resource Manager-Vorlage. Sie können beispielsweise „resources.azure.com“ verwenden und den ipSecurityRestrictions-Block bearbeiten, um den erforderlichen JSON-Code hinzuzufügen.

Der Speicherort dieser Informationen im Resource Manager ist:

management.azure.com/subscriptions/ **Abonnement-ID** /resourceGroups/ **Ressourcengruppen** /providers/Microsoft.Web/sites/ **Web-App-Name** /config/web?api-version=2018-02-01

Die JSON-Syntax für das obige Beispiel lautet:
```json
{
  "properties": {
    "ipSecurityRestrictions": [
      {
        "ipAddress": "122.133.144.0/24",
        "action": "Allow",
        "priority": 100,
        "name": "IP example rule"
      }
    ]
  }
}
```

## <a name="azure-functions-access-restrictions"></a>Zugriffseinschränkungen für Azure Functions

Zugriffseinschränkungen stehen auch für Funktions-Apps mit derselben Funktionalität sowie für App Service-Pläne zur Verfügung. Durch die Aktivierung von Zugriffseinschränkungen wird der Portalcode-Editor für alle unzulässigen IP-Adressen deaktiviert.

## <a name="next-steps"></a>Nächste Schritte
[Zugriffseinschränkungen für Azure Functions](../azure-functions/functions-networking-options.md#inbound-ip-restrictions)

[Application Gateway-Integration mit Dienstendpunkten](networking/app-gateway-with-service-endpoints.md)

<!--Links-->
[serviceendpoints]: ../virtual-network/virtual-network-service-endpoints-overview.md
