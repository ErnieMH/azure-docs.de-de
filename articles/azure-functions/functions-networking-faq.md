---
title: Häufig gestellte Fragen zu Netzwerken in Azure Functions
description: Hier finden Sie Antworten auf einige der häufigsten Fragen und Szenarien im Zusammenhang mit Netzwerken in Azure Functions.
ms.topic: troubleshooting
ms.date: 4/11/2019
ms.reviewer: glenga
ms.openlocfilehash: d07484943f8ba85cc8e46737f3ef85b6e25d5187
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2020
ms.locfileid: "91538057"
---
# <a name="frequently-asked-questions-about-networking-in-azure-functions"></a>Häufig gestellte Fragen zu Netzwerken in Azure Functions

In diesem Artikel sind häufig gestellte Fragen zu Netzwerken in Azure Functions aufgeführt. Eine ausführlichere Übersicht finden Sie unter [Netzwerkoptionen von Azure Functions](functions-networking-options.md).

## <a name="how-do-i-set-a-static-ip-in-functions"></a>Wie richte ich eine statische IP-Adresse in Azure Functions ein?

Das Bereitstellen einer Funktion in einer App Service-Umgebung ist derzeit die einzige Möglichkeit, um eine statische ein- und ausgehende IP-Adresse für Ihre Funktion einzurichten. Details zur Verwendung einer App Service-Umgebung finden Sie im Artikel [Erstellen und Verwenden eines internen Lastenausgleichs mit einer App Service-Umgebung](../app-service/environment/create-ilb-ase.md).

## <a name="how-do-i-restrict-internet-access-to-my-function"></a>Wie schränke ich den Internetzugriff auf meine Funktion ein?

Zum Einschränken des Internetzugriffs stehen mehrere Möglichkeiten zur Verfügung:

* [IP-Einschränkungen:](../app-service/app-service-ip-restrictions.md) Sie können den eingehenden Datenverkehr Ihrer Funktions-App mit einem IP-Adressbereich einschränken.
    * Mit IP-Einschränkungen können Sie auch [Dienstendpunkte](../virtual-network/virtual-network-service-endpoints-overview.md) konfigurieren, die Ihre Funktion so einschränken, dass nur eingehender Datenverkehr von einem bestimmten virtuellen Netzwerk akzeptiert wird.
* Entfernen aller HTTP-Trigger: Bei einigen Anwendungen genügt es, HTTP-Trigger einfach zu vermeiden und eine andere Ereignisquelle zum Auslösen der Funktion zu verwenden.

Beachten Sie hierbei, dass für den Editor im Azure-Portal direkter Zugriff auf die ausgeführte Funktion erforderlich ist. Alle über das Azure-Portal vorgenommenen Codeänderungen erfordern, dass die IP-Adresse des Geräts, das Sie zum Navigieren im Portal verwenden, in der genehmigten Liste enthalten ist. Sie können jedoch weiterhin alle Funktionen auf der Registerkarte „Plattformfeatures“ verwenden, wenn Netzwerkeinschränkungen eingerichtet wurden.

## <a name="how-do-i-restrict-my-function-app-to-a-virtual-network"></a>Wie beschränke ich meine Funktions-App auf ein virtuelles Netzwerk?

Sie können **eingehenden** Datenverkehr für eine Funktions-App mithilfe von [Dienstendpunkten](./functions-networking-options.md#private-site-access) auf ein virtuelles Netzwerk einschränken. Diese Konfiguration ermöglicht der Funktions-App immer noch, ausgehende Aufrufe an das Internet zu senden.

Die einzige Möglichkeit, eine Funktion so einzuschränken, dass der gesamte Datenverkehr durch ein virtuelles Netzwerk fließt, ist die Verwendung einer App Service-Umgebung mit internem Lastenausgleich. In diesem Fall wird die Website in einer dedizierten Infrastruktur in einem virtuellen Netzwerk bereitgestellt, und alle Trigger sowie der gesamte Datenverkehr werden über das virtuelle Netzwerk gesendet. 

Details zur Verwendung einer App Service-Umgebung finden Sie im Artikel [Erstellen und Verwenden eines internen Lastenausgleichs mit einer App Service-Umgebung](../app-service/environment/create-ilb-ase.md).

## <a name="how-can-i-access-resources-in-a-virtual-network-from-a-function-app"></a>Wie kann ich über eine Funktions-App auf Ressourcen in einem virtuellen Netzwerk zugreifen?

Sie können mithilfe der Integration des virtuellen Netzwerks über eine ausgeführte Funktion auf Ressourcen in einem virtuellen Netzwerk zugreifen. Weitere Informationen finden Sie unter [Integration des virtuellen Netzwerks](functions-networking-options.md#virtual-network-integration).

## <a name="how-do-i-access-resources-protected-by-service-endpoints"></a>Wie greife ich auf Ressourcen zu, die durch Dienstendpunkte geschützt sind?

Durch Verwendung der Integration des virtuellen Netzwerks können Sie über eine ausgeführte Funktion auf Ressourcen zugreifen, die durch Dienstendpunkte geschützt sind. Weitere Informationen finden Sie unter [Integration des virtuellen Netzwerks](functions-networking-options.md#virtual-network-integration).

## <a name="how-can-i-trigger-a-function-from-a-resource-in-a-virtual-network"></a>Wie kann ich eine Funktion über eine Ressource in einem virtuellen Netzwerk auslösen?

Sie können mithilfe von [Dienstendpunkten](./functions-networking-options.md#private-site-access) zulassen, dass HTTP-Trigger aus einem virtuellen Netzwerk aufgerufen werden. 

Sie können eine Funktion auch über alle anderen Ressourcen in einem virtuellen Netzwerk auslösen, indem Sie Ihre Funktions-App mit einem Premium-Plan, einem App Service-Plan oder in einer App Service-Umgebung bereitstellen. Weitere Informationen finden Sie im Artikel [Trigger für virtuelle Netzwerke (nicht HTTP)](./functions-networking-options.md#virtual-network-triggers-non-http).

## <a name="how-can-i-deploy-my-function-app-in-a-virtual-network"></a>Wie kann ich meine Funktions-App in einem virtuellen Netzwerk bereitstellen?

Die Bereitstellung in einer App Service Umgebung ist die einzige Möglichkeit, eine Funktions-App zu erstellen, die sich vollständig in einem virtuellen Netzwerk befindet. Details zur Verwendung eines internen Load Balancers in einer App Service-Umgebung finden Sie im Artikel [Erstellen und Verwenden eines internen Lastenausgleichs mit einer App Service-Umgebung](../app-service/environment/create-ilb-ase.md).

Informationen zu Szenarien, in denen Sie nur unidirektionalen Zugriff auf Ressourcen des virtuellen Netzwerks oder eine weniger umfassende Netzwerkisolation benötigen, finden Sie in der [Übersicht über die Netzwerkoptionen von Functions](functions-networking-options.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Netzwerk und zu Funkionen: 

* [Tutorial zum Einstieg in die Integration des virtuellen Netzwerks](./functions-create-vnet.md)
* [Weitere Informationen zu den Netzwerkoptionen in Azure Functions](./functions-networking-options.md)
* [Weitere Informationen zur Integration des virtuellen Netzwerks mit App Service und Functions](../app-service/web-sites-integrate-with-vnet.md)
* [Weitere Informationen zu virtuellen Netzwerken in Azure](../virtual-network/virtual-networks-overview.md)
* [Aktivieren weiterer Netzwerkfunktionen und Steuerung mit App Service-Umgebungen](../app-service/environment/intro.md)
