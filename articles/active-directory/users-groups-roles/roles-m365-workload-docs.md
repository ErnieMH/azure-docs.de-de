---
title: Dokumente zu Administratorrollen für Microsoft 365-Dienste – Azure AD | Microsoft-Dokumentation
description: Suchen von Inhalten und API-Referenzen für Administratorrollen für Microsoft 365-Dienste in Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: reference
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6060bc286024183661024abd56567eeefed76430
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2020
ms.locfileid: "90053547"
---
# <a name="administrator-roles-for-microsoft-365-services"></a>Administratorrollen für Microsoft 365-Dienste

Alle Produkte in Microsoft 365 können mit Administratorrollen in Azure AD verwaltet werden. Einige Produkte bieten darüber hinaus zusätzliche Rollen, die für dieses Produkt spezifisch sind. Informationen zu den von den einzelnen Produkten unterstützten Rollen finden Sie in der folgenden Tabelle. Allgemeine Informationen zu Delegierungsproblemen finden Sie unter [Planen der Rollendelegierung in Azure Active Directory](roles-concept-delegation.md).

## <a name="where-to-find-content"></a>Ort

Microsoft 365-Dienst | Rolleninhalt | API-Inhalt
---------------------- | ------------------ | -----------------
Administratorrollen in Office 365- und Microsoft 365-Businessplänen | [Microsoft 365-Administratorrollen](/office365/admin/add-users/about-admin-roles?view=o365-worldwide) | Nicht verfügbar
Azure Active Directory (Azure AD) und Azure AD Identity Protection| [Azure AD-Administratorrollen](directory-assign-admin-roles.md) | [Graph-API](/graph/api/overview?view=graph-rest-1.0)<br>[Abrufen von Rollenzuweisungen](/graph/api/directoryrole-list?view=graph-rest-1.0)
Exchange Online| [Rollenbasierte Zugriffssteuerung in Exchange](/exchange/understanding-role-based-access-control-exchange-2013-help) |  [PowerShell für Exchange](/powershell/module/exchange/role-based-access-control/add-managementroleentry?view=exchange-ps)<br>[Abrufen von Rollenzuweisungen](/powershell/module/exchange/role-based-access-control/get-rolegroup?view=exchange-ps)
SharePoint Online | [Azure AD-Administratorrollen](directory-assign-admin-roles.md)<br>Auch [Informationen zur SharePoint-Administratorrolle in Microsoft 365](/sharepoint/sharepoint-admin-role) | [Graph-API](/graph/api/overview?view=graph-rest-1.0)<br>[Abrufen von Rollenzuweisungen](/graph/api/directoryrole-list?view=graph-rest-1.0)
Teams/Skype for Business | [Azure AD-Administratorrollen](directory-assign-admin-roles.md) | [Graph-API](/graph/api/overview?view=graph-rest-1.0)<br>[Abrufen von Rollenzuweisungen](/graph/api/directoryrole-list?view=graph-rest-1.0)
Security & Compliance Center (Office 365 Advanced Threat Protection, Schutz für Exchange Online, Information Protection) | [Office 365-Administratorrollen](/office365/SecurityCompliance/permissions-in-the-security-and-compliance-center) | [Exchange PowerShell](/powershell/module/exchange/role-based-access-control/add-managementroleentry?view=exchange-ps)<br>[Abrufen von Rollenzuweisungen](/powershell/module/exchange/role-based-access-control/get-rolegroup?view=exchange-ps)
Sicherheitsbewertung | [Azure AD-Administratorrollen](directory-assign-admin-roles.md) | [Graph-API](/graph/api/overview?view=graph-rest-1.0)<br>[Abrufen von Rollenzuweisungen](/graph/api/directoryrole-list?view=graph-rest-1.0)
Compliance-Manager | [Compliance-Manager-Rollen](/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud#permissions-and-role-based-access-control) | Nicht verfügbar
Azure Information Protection | [Azure AD-Administratorrollen](directory-assign-admin-roles.md) | [Graph-API](/graph/api/overview?view=graph-rest-1.0)<br>[Abrufen von Rollenzuweisungen](/graph/api/directoryrole-list?view=graph-rest-1.0)
Microsoft Cloud App Security | [Rollenbasierte Zugriffssteuerung](/cloud-app-security/manage-admins) | [API-Referenz](/cloud-app-security/api-tokens) 
Azure Advanced Threat Protection | [Azure ATP-Rollengruppen](/azure-advanced-threat-protection/atp-role-groups) | Nicht verfügbar
Windows Defender Advanced Threat Protection | [Rollenbasierte Zugriffssteuerung von Windows Defender ATP](/windows/security/threat-protection/windows-defender-atp/rbac-windows-defender-advanced-threat-protection) | Nicht verfügbar
Privileged Identity Management | [Azure AD-Administratorrollen](directory-assign-admin-roles.md) | [Graph-API](/graph/api/overview?view=graph-rest-1.0)<br>[Abrufen von Rollenzuweisungen](/graph/api/directoryrole-list?view=graph-rest-1.0)
Intune | [Rollenbasierte Zugriffssteuerung von Intune](/intune/role-based-access-control) | [Graph-API](/graph/api/resources/intune-rbac-conceptual?view=graph-rest-beta)<br>[Abrufen von Rollenzuweisungen](/graph/api/intune-rbac-roledefinition-list?view=graph-rest-beta)
Managed Desktop | [Azure AD-Administratorrollen](directory-assign-admin-roles.md) | [Graph-API](/graph/api/overview?view=graph-rest-1.0)<br>[Abrufen von Rollenzuweisungen](/graph/api/directoryrole-list?view=graph-rest-1.0)

## <a name="next-steps"></a>Nächste Schritte

* [Anzeigen und Zuweisen von Azure AD-Administratorrollen](directory-manage-roles-portal.md)
* [Berechtigungen von Administratorrollen in Azure Active Directory](directory-assign-admin-roles.md)