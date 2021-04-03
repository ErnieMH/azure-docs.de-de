---
title: Verwenden von Azure DNS mit anderen Azure-Diensten
description: In diesem Lernpfad erhalten Sie eine Einführung in die Verwendung von Azure DNS zum Auflösen von Namen für andere Azure-Dienste.
services: dns
documentationcenter: na
author: rohinkoul
manager: kumudD
tags: azure dns
ms.assetid: e9b5eb94-7984-4640-9930-564bb9e82b78
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 09/21/2016
ms.author: rohink
ms.openlocfilehash: fa2c1ced6405c967ca33562d6215b304b8507e5a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "76937250"
---
# <a name="how-azure-dns-works-with-other-azure-services"></a>Funktionsweise von Azure DNS mit anderen Azure-Diensten

Azure DNS ist ein gehosteter Dienst für die DNS-Verwaltung und -Namensauflösung. Er kann verwendet werden, um öffentliche DNS-Namen für andere Anwendungen und Dienste zu erstellen, die Sie in Azure bereitstellen. Die Erstellung eines Namens für einen Azure-Dienst in Ihrer benutzerdefinierten Domäne ist ganz einfach. Sie müssen Ihrem Dienst lediglich einen passenden Eintragstyp hinzufügen.

* Für dynamisch zugewiesene IP-Adressen können Sie einen DNS CNAME-Eintrag erstellen, der dem DNS-Namen zugeordnet ist, den Azure für Ihren Dienst erstellt hat. DNS-Standards hindern Sie am Verwenden eines CNAME-Eintrags für die oberste Ebene der Zone. Sie können stattdessen einen Aliaseintrag verwenden. Weitere Informationen finden Sie unter [Tutorial: Konfigurieren eines Aliaseintrags zum Verweisen auf eine öffentliche Azure-IP-Adresse](tutorial-alias-pip.md).
* Für statisch zugewiesene IP-Adressen können Sie einen DNS A-Eintrag mit einem beliebigen Namen erstellen – einschließlich eines *Domänennamens ohne „www“* im Zonen-Apex.

Die folgende Tabelle enthält die unterstützten Eintragstypen, die Sie für verschiedene Azure-Dienste verwenden können. Wie in der Tabelle zu sehen, unterstützt Azure DNS nur DNS-Einträge für Netzwerkressourcen mit Internetzugriff. Azure DNS kann nicht für die Namensauflösung interner, privater Adressen verwendet werden.

| Azure-Dienst | Netzwerkschnittstelle | BESCHREIBUNG |
| --- | --- | --- |
| Azure Application Gateway |[Öffentliche IP-Adresse des Front-Ends](dns-custom-domain.md#public-ip-address) |Sie können einen DNS A- oder CNAME-Eintrag erstellen. |
| Azure Load Balancer |[Öffentliche IP-Adresse des Front-Ends](dns-custom-domain.md#public-ip-address) |Sie können einen DNS A- oder CNAME-Eintrag erstellen. Load Balancer kann über eine öffentliche IPv6-Adresse verfügen, die dynamisch zugewiesen wird. Erstellen Sie einen CNAME-Eintrag für eine IPv6-Adresse. |
| Azure Traffic Manager |Öffentlicher Name |Sie können einen Aliaseintrag erstellen, der dem Namen „trafficmanager.net“ des Traffic Manager-Profils zugewiesen ist. Weitere Informationen finden Sie unter [Tutorial: Konfigurieren eines Alias-Ressourceneintrags zur Unterstützung von Apex-Domänennamen mit Traffic Manager](tutorial-alias-tm.md). |
| Azure Cloud Services |[Öffentliche IP-Adresse](dns-custom-domain.md#public-ip-address) |Für statisch zugewiesene IP-Adressen können Sie einen DNS A-Eintrag erstellen. Für dynamisch zugewiesenen IP-Adressen müssen Sie einen CNAME-Eintrag erstellen, der dem *cloudapp.net* -Namen zugeordnet ist.|
| Azure App Service | [Externe IP-Adresse](dns-custom-domain.md#app-service-web-apps) |Für externe IP-Adressen können Sie einen DNS A-Eintrag erstellen. Andernfalls müssen Sie einen CNAME-Eintrag erstellen, der dem „azurewebsites.net“-Namen zugeordnet ist. Weitere Informationen finden Sie unter [Zuordnen eines benutzerdefinierten Domänennamens zu einer Azure-App](../app-service/app-service-web-tutorial-custom-domain.md) |
| Virtuelle Azure Resource Manager-Computer |[Öffentliche IP-Adresse](dns-custom-domain.md#public-ip-address) |Virtuelle Azure Resource Manager-Computer können öffentliche IP-Adressen besitzen. Ein virtueller Computer mit einer öffentlichen IP-Adresse kann sich auch hinter einem Lastenausgleich befinden. Sie können einen DNS-Eintrag vom Typ A oder CNAME oder einen Aliaseintrag für die öffentliche Adresse erstellen. Dieser benutzerdefinierte Name kann verwendet werden, um die VIP-Adresse des Lastenausgleichs zu umgehen. |
| Klassische virtuelle Computer |[Öffentliche IP-Adresse](dns-custom-domain.md#public-ip-address) |Klassische virtuelle Computer, die mithilfe von PowerShell oder über die Befehlszeilenschnittstelle erstellt werden, können mit einer dynamischen oder statischen (reservierten) virtuellen IP-Adresse konfiguriert werden. Sie können einen DNS-Eintrag vom Typ CNAME oder A erstellen. |
