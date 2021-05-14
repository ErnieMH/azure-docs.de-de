---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/04/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 0d8a6b89462429d536b6c96fcb34346b5bf1c210
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108792946"
---
|Name<br /><sub>(Azure-Portal)</sub> |Beschreibung |Auswirkungen |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Windows-Computer überwachen, auf denen angegebene Mitglieder in der Administratorengruppe fehlen](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F30f71ea1-ac77-4f26-9fc5-2d926bbd4ba7) |Erfordert, dass die erforderlichen Komponenten im Bereich der Richtlinienzuweisung bereitgestellt werden. Einzelheiten dazu finden Sie unter [https://aka.ms/gcpol](https://aka.ms/gcpol). Computer werden als nicht konform eingestuft, wenn mindestens eins der im Richtlinienparameter angegebenen Mitglieder nicht in der lokalen Administratorgruppe enthalten ist. |auditIfNotExists |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Guest%20Configuration/GuestConfiguration_AdministratorsGroupMembersToInclude_AINE.json) |
|[Windows-Computer überwachen, die zusätzliche Konten in der Administratorgruppe enthalten](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3d2a3320-2a72-4c67-ac5f-caa40fbee2b2) |Erfordert, dass die erforderlichen Komponenten im Bereich der Richtlinienzuweisung bereitgestellt werden. Einzelheiten dazu finden Sie unter [https://aka.ms/gcpol](https://aka.ms/gcpol). Computer werden als nicht konform eingestuft, wenn die lokale Administratorgruppe Mitglieder enthält, die im Richtlinienparameter nicht angegeben sind. |auditIfNotExists |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Guest%20Configuration/GuestConfiguration_AdministratorsGroupMembers_AINE.json) |
|[Windows-Computer überwachen, auf denen die angegebenen Mitglieder in der Administratorengruppe enthalten sind](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F69bf4abd-ca1e-4cf6-8b5a-762d42e61d4f) |Erfordert, dass die erforderlichen Komponenten im Bereich der Richtlinienzuweisung bereitgestellt werden. Einzelheiten dazu finden Sie unter [https://aka.ms/gcpol](https://aka.ms/gcpol). Computer werden als nicht konform eingestuft, wenn die lokale Administratorgruppe mindestens eins der Mitglieder enthält, die im Richtlinienparameter angegeben sind. |auditIfNotExists |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Guest%20Configuration/GuestConfiguration_AdministratorsGroupMembersToExclude_AINE.json) |
