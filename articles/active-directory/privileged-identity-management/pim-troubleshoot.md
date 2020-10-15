---
title: Behandeln eines Problems mit Privileged Identity Management – Azure Active Directory | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Systemfehler mit Rollen in Azure AD Privileged Identity Management (PIM) behandeln.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: how-to
ms.workload: identity
ms.subservice: pim
ms.date: 10/18/2019
ms.author: curtand
ms.collection: M365-identity-device-management
ms.openlocfilehash: b02b8f3b94d191c7439aadf5a56ec0e2a1c38e28
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88782497"
---
# <a name="troubleshoot-a-problem-with-privileged-identity-management"></a>Behandeln eines Problems mit Privileged Identity Management

Haben Sie ein Problem mit Privileged Identity Management (PIM) in Azure Active Directory (Azure AD)? In diesem Artikel erfahren Sie, wie Sie solche Probleme beheben.

## <a name="access-to-azure-resources-denied"></a>Verweigerung des Zugriffs auf Azure-Ressourcen

### <a name="problem"></a>Problem

Als aktiver Besitzer oder Benutzerzugriffsadministrator für eine Azure-Ressource können Sie Ihre Ressource in Privileged Identity Management zwar anzeigen, aber keine Aktionen ausführen (also beispielsweise eine berechtigte Zuweisung vornehmen oder eine Liste mit Rollenzuweisungen auf der Ressourcenübersichtsseite anzeigen). Bei jeder dieser Aktionen tritt ein Autorisierungsfehler auf.

### <a name="cause"></a>Ursache

Dieses Problem kann auftreten, wenn die Rolle „Benutzerzugriffsadministrator“ für den PIM-Dienstprinzipal versehentlich aus dem Abonnement entfernt wurde. Damit der Privileged Identity Management-Dienst auf Azure-Ressourcen zugreifen kann, muss dem MS-PIM-Dienstprinzipal immer die [Rolle „Benutzerzugriffsadministrator“](../../role-based-access-control/built-in-roles.md#user-access-administrator) über das Azure-Abonnement zugewiesen sein.

### <a name="resolution"></a>Lösung

Weisen Sie dem Privileged Identity Management-Dienstprinzipalnamen (MS-PIM) die Rolle „Benutzerzugriffsadministrator“ auf der Abonnementebene zu. Diese Zuweisung sorgt dafür, dass der Privileged Identity Management-Dienst auf die Azure-Ressourcen zugreifen kann. Die Rolle kann abhängig von Ihren Anforderungen auf der Ebene einer Verwaltungsgruppe oder auf der Abonnementebene zugewiesen werden. Weitere Informationen zu Dienstprinzipalen finden Sie unter [Zuweisen der Anwendung zu einer Rolle](../develop/howto-create-service-principal-portal.md#assign-a-role-to-the-application).

## <a name="next-steps"></a>Nächste Schritte

- [Lizenzanforderungen für die Verwendung von PIM](subscription-requirements.md)
- [Schützen des privilegierten Zugriffs für hybride und Cloudbereitstellungen in Azure AD](../users-groups-roles/directory-admin-roles-secure.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json)
- [Bereitstellen von Azure AD Privileged Identity Management (PIM)](pim-deployment-plan.md)