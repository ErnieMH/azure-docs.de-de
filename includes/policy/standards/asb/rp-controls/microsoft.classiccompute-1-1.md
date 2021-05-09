---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/28/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: bd1bda8f39f60ef1bb1663d4d6a21e571fef3225
ms.sourcegitcommit: 516eb79d62b8dbb2c324dff2048d01ea50715aa1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108212293"
---
|Name<br /><sub>(Azure-Portal)</sub> |BESCHREIBUNG |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Virtuelle Computer mit Internetzugang sollten über Netzwerksicherheitsgruppen geschützt werden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff6de0be7-9a8a-4b8a-b349-43cf02d22f7c) |Schützen Sie Ihre virtuellen Computer vor potenziellen Bedrohungen, indem Sie den Zugriff darauf mithilfe von Netzwerksicherheitsgruppen (NSGs) einschränken. Weitere Informationen zum Steuern des Datenverkehrs mit NSGs finden Sie unter [https://aka.ms/nsg-doc](https://aka.ms/nsg-doc). |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_NetworkSecurityGroupsOnInternetFacingVirtualMachines_Audit.json) |
|[Die IP-Weiterleitung muss auf Ihrem virtuellen Computer deaktiviert sein.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fbd352bd5-2853-4985-bf0d-73806b4a5744) |Durch die Aktivierung der IP-Weiterleitung für die Netzwerkschnittstelle einer VM kann die VM Datenverkehr empfangen, der an andere Ziele adressiert ist. Da die IP-Weiterleitung nur selten benötigt wird (z. B. bei Verwendung der VM als virtuelles Netzwerkgerät), sollte dieser Vorgang vom Netzwerksicherheitsteam geprüft werden. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_IPForwardingOnVirtualMachines_Audit.json) |
|[Verwaltungsports sollten auf Ihren virtuellen Computern geschlossen werden](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F22730e10-96f6-4aac-ad84-9383d35b5917) |Offene Remoteverwaltungsports setzen Ihre VM einem hohen Risiko aus, weil sie internetbasierte Brute-Force-Angriffe zur Erlangung von Anmeldeinformationen begünstigen, um Administratorzugriff auf den Computer zu erhalten. |AuditIfNotExists, Disabled |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_OpenManagementPortsOnVirtualMachines_Audit.json) |
