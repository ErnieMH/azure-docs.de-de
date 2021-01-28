---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 01/25/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 61f42e7f4dfbcfed4b6fc41d737059916e898654
ms.sourcegitcommit: fc8ce6ff76e64486d5acd7be24faf819f0a7be1d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98807379"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Azure Cache for Redis muss sich in einem virtuellen Netzwerk befinden.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7d092e0a-7acd-40d2-a975-dca21cae48c4) |Die Bereitstellung von Azure Virtual Network (VNet) bietet ein höheres Maß an Sicherheit und Isolation für Ihre Azure Cache for Redis-Instanz sowie Subnetze, Zugriffssteuerungsrichtlinien und andere Funktionen zur weiteren Einschränkung des Zugriffs. Wenn eine Azure Cache for Redis-Instanz mit einem VNet konfiguriert ist, kann sie nicht öffentlich adressiert, sondern nur von VMs und Anwendungen innerhalb des VNet aufgerufen werden. |Audit, Deny, Disabled |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cache/RedisCache_CacheInVnet_Audit.json) |
|[Für Ihren Azure Cache for Redis dürfen nur sichere Verbindungen aktiviert sein.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F22bee202-a82f-4305-9a2a-6d7f44d4dedb) |Aktivieren der Überprüfung nur für Verbindungen über SSL mit Azure Cache for Redis Durch die Verwendung sicherer Verbindungen wird eine Authentifizierung zwischen dem Server und dem Dienst sichergestellt, und die übertragenen Daten werden vor Angriffen auf Netzwerkebene geschützt, wie z.B. Man-in-the-Middle-Angriffe, Abhörangriffe und Session Hijacking. |Audit, Deny, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cache/RedisCache_AuditSSLPort_Audit.json) |
