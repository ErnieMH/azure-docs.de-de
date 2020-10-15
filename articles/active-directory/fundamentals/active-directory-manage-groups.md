---
title: Verwalten des Zugriffs auf Apps und Ressourcen mithilfe von Gruppen – Azure AD
description: Erfahren Sie, wie Sie den Zugriff auf die cloudbasierten und lokalen Apps und auf die Ressourcen Ihrer Organisation mithilfe von Azure Active Directory-Gruppen verwalten.
services: active-directory
author: ajburnle
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 01/08/2020
ms.author: ajburnle
ms.reviewer: piotrci
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 25dace3ad7d467d6add236782c5e39f85d6462a6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87797306"
---
# <a name="manage-app-and-resource-access-using-azure-active-directory-groups"></a>Verwalten des Zugriffs auf Apps und Ressourcen mithilfe von Azure Active Directory-Gruppen
Mit Azure Active Directory (Azure AD) können Sie den Zugriff auf Ihre cloudbasierten und lokalen Apps und Ihre Ressourcen mithilfe von Gruppen verwalten. Ihre Ressourcen können Teil der Azure AD-Organisation sein, wie im Fall von Berechtigungen zum Verwalten von Objekten mithilfe von Rollen in Azure AD, oder externe Ressourcen, etwa SaaS-Apps (Software-as-a-Service), Azure-Dienste, SharePoint-Websites und lokale Ressourcen.

>[!NOTE]
> Im Azure-Portal werden einige Gruppen angezeigt, deren Mitgliedschafts- und Gruppendetails nicht im Portal verwaltet werden können:
>
> - Gruppen, die vom lokalen Active Directory synchronisiert werden, können nur im lokalen Active Directory verwaltet werden.
> - Andere Gruppentypen wie Verteilerlisten und E-Mail-aktivierte Sicherheitsgruppen werden nur im Exchange Admin Center oder im Microsoft 365 Admin Center verwaltet. Zum Verwalten dieser Gruppen müssen Sie sich beim Exchange Admin Center bzw. beim Microsoft 365 Admin Center anmelden.

## <a name="how-access-management-in-azure-ad-works"></a>So funktioniert die Verwaltung in Azure AD

Mit Azure AD können Sie Zugriff auf die Ressourcen Ihrer Organisation gewähren, indem Sie Zugriffsrechte für einen einzelnen Benutzer oder eine gesamte Azure AD-Gruppe erteilen. Mithilfe von Gruppen kann der Ressourcenbesitzer (oder Azure AD-Verzeichnisbesitzer) allen Mitgliedern der Gruppe Zugriffsberechtigungen gewähren, statt die Berechtigungen einzeln erteilen zu müssen. Der Ressourcen- oder Verzeichnisbesitzer kann darüber hinaus Verwaltungsrechte für die Mitgliederliste an eine andere Person übertragen, etwa den Abteilungsleiter oder einen Helpdesk-Administrator, und dadurch dieser Person das Hinzufügen und Entfernen von Mitgliedern nach Bedarf ermöglichen. Weitere Informationen zum Verwalten von Gruppenbesitzern finden Sie unter [Verwalten von Besitzern einer Gruppe](active-directory-accessmanagement-managing-group-owners.md).

![Das Azure Active Directory Access Management-Diagramm](./media/active-directory-manage-groups/active-directory-access-management-works.png)

## <a name="ways-to-assign-access-rights"></a>Möglichkeiten zum Zuweisen von Zugriffsrechten

Es gibt vier Möglichkeiten zum Zuweisen von Ressourcenzugriffsrechten zu Ihren Benutzern:

- **Direkte Zuweisung:** Der Ressourcenbesitzer weist den Benutzer direkt der Ressource zu.

- **Gruppenzuweisung:** Der Ressourcenbesitzer weist der Ressource eine Azure AD-Gruppe zu. Dadurch erhalten automatisch alle Gruppenmitglieder Zugriff auf die Ressource. Die Gruppenmitgliedschaft wird vom Gruppenbesitzer und vom Ressourcenbesitzer verwaltet, sodass beide Besitzer Mitglieder zur Gruppe hinzufügen bzw. daraus entfernen können. Weitere Informationen zum Hinzufügen oder Entfernen der Gruppenmitgliedschaft finden Sie unter [Gewusst wie: Hinzufügen oder Entfernen einer Gruppe zu bzw. aus einer anderen Gruppe in Azure Active Directory](active-directory-groups-membership-azure-portal.md). 

- **Regelbasierte Zuweisung:** Der Ressourcenbesitzer erstellt eine Gruppe und verwendet eine Regel, um festzulegen, welche Benutzer einer bestimmten Ressource zugewiesen werden. Die Regel basiert auf Attributen, die einzelnen Benutzern zugewiesen sind. Der Ressourcenbesitzer verwaltet die Regel und bestimmt, welche Attribute und Werte zum Zulassen des Zugriffs auf die Ressource erforderlich sind. Weitere Informationen finden Sie unter [Erstellen einer dynamischen Gruppe und Überprüfen des Status](../users-groups-roles/groups-create-rule.md).

    Sie können außerdem dieses kurze Video mit einer kurzen Erläuterung zum Erstellen und Verwenden von dynamischen Gruppen anschauen:

    >[!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD--Introduction-to-Dynamic-Memberships-for-Groups/player]

- **Zuweisung durch eine externe Autorität:** Der Zugriff stammt aus einer externen Quelle, etwa einem lokalen Verzeichnis oder einer SaaS-App. In diesem Fall weist der Ressourcenbesitzer eine Gruppe zu, um Zugriff auf die Ressource zu ermöglichen, und die externe Quelle verwaltet die Gruppenmitglieder.

   ![Übersicht über das Access Management-Diagramm](./media/active-directory-manage-groups/access-management-overview.png)

## <a name="can-users-join-groups-without-being-assigned"></a>Können Benutzer Gruppen beitreten, ohne zugewiesen zu werden?
Der Gruppenbesitzer kann zulassen, dass Benutzer ihre Gruppen selbst ermitteln, denen sie beitreten möchten, statt sie zuzuweisen. Der Besitzer kann die Gruppe auch so einrichten, dass sie alle beitretenden Benutzer automatisch akzeptiert oder dass eine Genehmigung erforderlich ist.

Nachdem ein Benutzer den Beitritt zu einer Gruppe angefordert hat, wird diese Anforderung an den Gruppenbesitzer weitergeleitet. Wenn es erforderlich ist, kann der Besitzer die Anforderung genehmigen, und der Benutzer wird über die Gruppenmitgliedschaft informiert. Wenn jedoch mehrere Besitzer vorhanden sind und einer davon die Anforderung ablehnt, wird der Benutzer zwar benachrichtigt, aber nicht zur Gruppe hinzugefügt. Weitere Informationen sowie Anweisungen dazu, wie Sie Benutzern die Anforderung des Gruppenbeitritts ermöglichen, finden Sie unter [Einrichten von Azure Active Directory für die Self-Service-Gruppenverwaltung](../users-groups-roles/groups-self-service-management.md).

## <a name="next-steps"></a>Nächste Schritte
Dieser Artikel enthielt eine kurze Einführung zur Zugriffsverwaltung mithilfe von Gruppen. Nun können Sie mit der Verwaltung Ihrer Ressourcen und Apps beginnen.

- [Erstellen einer neuen Gruppe mit Azure Active Directory](active-directory-groups-create-azure-portal.md) oder [Erstellen und Verwalten einer neuen Gruppe mit PowerShell-Cmdlets](../users-groups-roles/groups-settings-v2-cmdlets.md)

- [Verwenden von Gruppen zum Zuweisen des Zugriffs auf eine integrierte SaaS-App](../users-groups-roles/groups-saasapps.md)

- [Synchronisieren einer lokalen Gruppe in Azure mittels Azure AD Connect](../hybrid/whatis-hybrid-identity.md)
