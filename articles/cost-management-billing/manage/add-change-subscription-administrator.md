---
title: Hinzufügen oder Ändern von Azure-Abonnementadministratoren
description: Beschreibt das Hinzufügen oder Ändern eines Azure-Abonnementadministrators mithilfe der rollenbasierten Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC).
author: genlin
ms.reviewer: dcscontentpm
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: conceptual
ms.date: 08/20/2020
ms.author: banders
ms.openlocfilehash: 10956953f9ab3a9e32b9da4ab8a3501d38b0e2c3
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2020
ms.locfileid: "92369657"
---
# <a name="add-or-change-azure-subscription-administrators"></a>Hinzufügen oder Ändern von Azure-Abonnementadministratoren


Für die Verwaltung von Azure-Ressourcen müssen Sie über die entsprechende Administratorrolle verfügen. Azure bietet ein als [rollenbasierte Zugriffssteuerung](../../role-based-access-control/overview.md) (Azure Role-Based Access Control, Azure RBAC) bezeichnetes Autorisierungssystem mit verschiedenen integrierten Rollen, unter denen Sie wählen können. Sie können diesen Rollen verschiedene Gültigkeitsbereiche zuweisen, wie etwa Verwaltungsgruppe, Abonnement oder Ressourcengruppe. Standardmäßig kann die Person, die ein neues Azure-Abonnement erstellt, anderen Benutzern Administratorzugriff auf ein Abonnement gewähren.

In diesem Artikel wird beschrieben, wie die Administratorrolle für einen Benutzer mithilfe von Azure RBAC auf Abonnementebene hinzugefügt oder geändert werden kann.

Microsoft empfiehlt das Verwalten des Zugriffs auf Ressourcen mithilfe der Azure RBAC. Wenn Sie aber weiterhin das klassische Bereitstellungsmodell verwenden und die klassischen Ressourcen mit dem [PowerShell-Modul für die Azure-Dienstverwaltung](/powershell/module/servicemanagement/azure.service) verwalten, müssen Sie einen klassischen Administrator verwenden.

> [!TIP]
> Wenn Sie klassische Ressourcen nur über das Azure-Portal verwalten, müssen Sie den klassischen Administrator nicht verwenden.

Weitere Informationen finden Sie unter [Azure Resource Manager und klassische Bereitstellung](../../azure-resource-manager/management/deployment-models.md) und [Azure classic subscription administrators](../../role-based-access-control/classic-administrators.md) (Klassische Azure-Abonnementadministratoren).

<a name="add-an-admin-for-a-subscription"></a>

## <a name="assign-a-subscription-administrator"></a>Zuweisen eines Abonnementadministrators

Wenn ein Administrator einen Benutzer zum Administrator für ein Azure-Abonnement machen möchte, weist er ihm die Rolle [Besitzer](../../role-based-access-control/built-in-roles.md#owner) (eine Azure-Rolle) im Abonnementbereich zu. Durch die Rolle „Besitzer“ erhält der Benutzer vollständigen Zugriff auf alle Ressourcen im Abonnement, einschließlich des Rechts, den Zugriff an andere Personen zu delegieren. Diese Schritte sind identisch mit allen anderen Rollenzuweisungen.

Verwenden Sie die folgenden Schritte für die Ermittlung, wenn Sie nicht sicher sind, wer der Kontoadministrator für ein Abonnement ist.

1. Öffnen Sie die [Seite „Abonnements“ im Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Wählen Sie das zu überprüfende Abonnement aus, und sehen Sie unter **Einstellungen** nach.
1. Wählen Sie **Eigenschaften** aus. Der Kontoadministrator des Abonnements wird im Feld **Kontoadministrator** angezeigt.

### <a name="to-assign-a-user-as-an-administrator"></a>Zuweisen eines Benutzers als Administrator

1. Melden Sie sich beim Azure-Portal als Besitzer des Abonnements an, und öffnen Sie [Abonnements](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

1. Klicken Sie auf das Abonnement, für das Sie Zugriff erteilen möchten.

1. Klicken Sie auf **Zugriffssteuerung (IAM)** .

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen** , um alle Rollenzuweisungen für dieses Abonnement anzuzeigen.

    ![Screenshot von Rollenzuweisungen](./media/add-change-subscription-administrator/role-assignments.png)

1. Klicken Sie auf **Hinzufügen** > **Rollenzuweisung hinzufügen** , um den Bereich **Rollenzuweisung hinzufügen** zu öffnen.

    Wenn Sie keine Berechtigungen zum Zuweisen von Rollen besitzen, ist die Option deaktiviert.

1. Wählen Sie in der Dropdownliste **Rolle** die Option **Besitzer** aus.

1. Wählen Sie in der Liste **Auswählen** einen Benutzer aus. Wird der Benutzer in der Liste nicht angezeigt, können Sie im Feld **Auswählen** einen Suchbegriff eingeben, um das Verzeichnis nach Anzeigenamen und E-Mail-Adressen zu durchsuchen.

    ![Screenshot mit ausgewählter Besitzerrolle](./media/add-change-subscription-administrator/add-role.png)

1. Klicken Sie auf **Speichern** , um die Rolle zuzuweisen.

    Nach einigen Augenblicken wird dem Benutzer im Abonnementbereich die Rolle „Besitzer“ zugewiesen.

## <a name="next-steps"></a>Nächste Schritte

* [Was ist die rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC)?](../../role-based-access-control/overview.md)
* [Grundlegendes zu den verschiedenen Rollen in Azure](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* [Zuordnen oder Hinzufügen eines Azure-Abonnements zu Ihrem Azure Active Directory-Mandanten](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)
* [Berechtigungen der Administratorrolle in Azure Active Directory](../../active-directory/roles/permissions-reference.md)

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.
