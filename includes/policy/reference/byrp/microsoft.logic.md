---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/05/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 14454d8b2c09bbf230e287463e7059454829250d
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2021
ms.locfileid: "102424092"
---
|Name<br /><sub>(Azure-Portal)</sub> |Beschreibung |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Diagnoseeinstellungen für Logic Apps in Event Hub bereitstellen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa1dae6c7-13f3-48ea-a149-ff8442661f60) |Hiermit werden die Diagnoseeinstellungen für Logic Apps zum Streamen in eine regionale Event Hub-Instanz bereitgestellt, wenn eine Logic Apps-Instanz erstellt oder aktualisiert wird, in der diese Diagnoseeinstellungen fehlen. |DeployIfNotExists, Disabled |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/LogicApps_DeployDiagnosticLog_Deploy_EventHub.json) |
|[Diagnoseeinstellungen für Logic Apps in Log Analytics-Arbeitsbereich bereitstellen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb889a06c-ec72-4b03-910a-cb169ee18721) |Hiermit werden die Diagnoseeinstellungen für Logic Apps zum Streamen in einen regionalen Log Analytics-Arbeitsbereich bereitgestellt, wenn eine Logic Apps-Instanz erstellt oder aktualisiert wird, in der diese Diagnoseeinstellungen fehlen. |DeployIfNotExists, Disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/LogicApps_DeployDiagnosticLog_Deploy_LogAnalytics.json) |
|[In Logic Apps müssen Ressourcenprotokolle aktiviert sein](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F34f95f76-5386-4de7-b824-0d8478470c9d) |Hiermit wird die Aktivierung von Ressourcenprotokollen überwacht. Auf diese Weise können Sie vergangene Aktivitäten nachvollziehen, wenn es zu einem Sicherheitsincident kommt oder Ihr Netzwerk gefährdet ist. |AuditIfNotExists, Disabled |[4.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Logic%20Apps/LogicApps_AuditDiagnosticLog_Audit.json) |
