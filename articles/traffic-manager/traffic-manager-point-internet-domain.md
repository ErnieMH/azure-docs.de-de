---
title: Verweisen einer Internetdomäne auf Traffic Manager – Azure Traffic Manager
description: In diesem Artikel erfahren Sie, wie Sie mit Ihrem Unternehmensdomänennamen auf einen Traffic Manager-Domänennamen verweisen.
services: traffic-manager
author: duongau
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: duau
ms.openlocfilehash: e0e2acfb0ec0068dcd08ae660e397f65e039a665
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98183743"
---
# <a name="point-a-company-internet-domain-to-an-azure-traffic-manager-domain"></a>Verweisen einer Unternehmens-Internetdomäne auf einen Azure Traffic Manager-Domänennamen

Wenn Sie ein Traffic Manager-Profil erstellen, weist Azure dem Profil automatisch einen DNS-Name zu. Um einen Namen aus Ihrer DNS-Zone zu verwenden, erstellen Sie einen CNAME-DNS-Eintrag, der dem Domänennamen des Traffic Manager-Profils zugeordnet ist. Sie finden den Traffic Manager-Domänennamen auf der Konfigurationsseite des Traffic Manager-Profils im Abschnitt **Allgemein** .

Beispiel: Um mit dem Namen `www.contoso.com` auf den Traffic Manager-DNS-Namen `contoso.trafficmanager.net` zu verweisen, erstellen Sie den folgenden DNS-Ressourceneintrag:

`www.contoso.com IN CNAME contoso.trafficmanager.net.`

Alle Datenverkehrsanforderungen an *www\.contoso.com* werden an *contoso.trafficmanager.net* umgeleitet.

> [!IMPORTANT]
> Sie können nicht mit Domänen der zweiten Ebene wie *contoso.com* auf Traffic Manager-Domänen verweisen. Die DNS-Protokollstandards erlauben keine CNAME-Einträge für Domänennamen der zweiten Ebene.

## <a name="next-steps"></a>Nächste Schritte

* [Traffic Manager-Routingmethoden](traffic-manager-routing-methods.md)
* [Deaktivieren, Aktivieren oder Löschen eines Traffic Manager-Profils](./traffic-manager-manage-profiles.md)
* [Deaktivieren oder Aktivieren eines Traffic Manager-Endpunkts](./traffic-manager-manage-endpoints.md)