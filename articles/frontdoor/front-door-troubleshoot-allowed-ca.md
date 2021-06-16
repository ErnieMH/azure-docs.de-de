---
title: Zulässige Zertifizierungsstelle für die Aktivierung von benutzerdefiniertem HTTPS für Azure Front Door
description: Wenn Sie Ihr eigenes Zertifikat zur Aktivierung von HTTPS in einer benutzerdefinierten Azure Front Door-Domäne verwenden, müssen Sie für die Erstellung eine zulässige Zertifizierungsstelle (ZS) verwenden.
services: frontdoor
documentationcenter: ''
author: duongau
ms.assetid: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/30/2020
ms.author: duau
ms.openlocfilehash: 20c5d611272ee2159ce8ddcc2865797a225a7ebb
ms.sourcegitcommit: e39ad7e8db27c97c8fb0d6afa322d4d135fd2066
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112003618"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-front-door"></a>Zulässige Zertifizierungsstellen für die Aktivierung von benutzerdefiniertem HTTPS für Azure Front Door

Wenn Sie für eine benutzerdefinierte Azure Front Door-Domäne [das HTTPS-Feature mit Ihrem eigenen Zertifikat aktivieren](front-door-custom-domain-https.md?tabs=option-2-enable-https-with-your-own-certificate), benötigen Sie eine zulässige Zertifizierungsstelle (ZS), um Ihr TLS/SSL-Zertifikat zu erstellen. Bei der Verwendung einer unzulässigen Zertifizierungsstelle oder eines selbstsignierten Zertifikats wird Ihre Anforderung abgelehnt.

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]
