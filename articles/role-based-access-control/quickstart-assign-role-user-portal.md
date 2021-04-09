---
title: 'Tutorial: Gewähren des Zugriffs auf Azure-Ressourcen für einen Benutzer mit dem Azure-Portal – Azure RBAC'
description: In diesem Tutorial erfahren Sie, wie Sie einem Benutzer mit dem Azure-Portal und der rollenbasierten Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC) Zugriff auf Azure-Ressourcen gewähren.
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 02/22/2019
ms.author: rolyon
ms.openlocfilehash: fcba9cad208c2ac170f91cc06a6db22e271f2a70
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "100559316"
---
# <a name="tutorial-grant-a-user-access-to-azure-resources-using-the-azure-portal"></a>Tutorial: Gewähren des Zugriffs auf Azure-Ressourcen für einen Benutzer mit dem Azure-Portal

Der Zugriff auf Azure-Ressourcen wird mithilfe der [rollenbasierten Zugriffssteuerung in Azure (Azure RBAC)](overview.md) verwaltet. In diesem Tutorial gewähren Sie einem Benutzer Zugriff, um virtuelle Computer in einer Ressourcengruppe zu erstellen und zu verwalten.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Gewähren des Zugriffs für einen Benutzer im Ressourcengruppenbereich
> * Zugriff entfernen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

1. Klicken Sie in der Navigationsliste auf **Ressourcengruppen**.

1. Klicken Sie auf **Hinzufügen**, um das Blatt **Ressourcengruppe** zu öffnen.

   ![Hinzufügen einer neuen Ressourcengruppe](./media/quickstart-assign-role-user-portal/resource-group.png)

1. Geben Sie für **Ressourcengruppenname** die Zeichenfolge **rbac-resource-group** ein.

1. Wählen Sie ein Abonnement und einen Standort aus.

1. Klicken Sie auf **Erstellen** , um die Ressourcengruppe zu erstellen.

1. Klicken Sie auf **Aktualisieren**, um die Liste der Ressourcengruppen zu aktualisieren.

   Die neue Ressourcengruppe wird in der Liste Ihrer Ressourcengruppen angezeigt.

   ![Ressourcengruppenliste](./media/quickstart-assign-role-user-portal/resource-group-list.png)

## <a name="grant-access"></a>Gewähren von Zugriff

In Azure RBAC weisen Sie zum Gewähren des Zugriffs eine Azure-Rolle zu.

1. Klicken Sie in der Liste der **Ressourcengruppen** auf die neue Ressourcengruppe **rbac-resource-group**.

1. Klicken Sie auf **Zugriffssteuerung (IAM)** .

1. Klicken Sie auf die Registerkarte **Rollenzuweisungen**, um die aktuelle Liste mit den Rollenzuweisungen anzuzeigen.

   ![Blatt „Zugriffssteuerung (IAM)“ für Ressourcengruppe](./media/quickstart-assign-role-user-portal/access-control.png)

1. Klicken Sie auf **Hinzufügen** > **Rollenzuweisung hinzufügen**, um den Bereich „Rollenzuweisung hinzufügen“ zu öffnen.

   Wenn Sie keine Berechtigungen zum Zuweisen von Rollen haben, ist die Option „Rollenzuweisung hinzufügen“ deaktiviert.

   ![Menü „Rollenzuweisung hinzufügen“](./media/shared/add-role-assignment-menu.png)

    Der Bereich „Rollenzuweisung hinzufügen“ wird geöffnet.

   ![Bereich „Rollenzuweisung hinzufügen“](./media/quickstart-assign-role-user-portal/add-role-assignment.png)

1. Wählen Sie in der Dropdownliste **Rolle** die Rolle **Mitwirkender für virtuelle Computer** aus.

1. Wählen Sie in der Liste **Auswählen** sich selbst oder einen anderen Benutzer aus.

1. Klicken Sie auf **Speichern**, um die Rolle zuzuweisen.

   Nach einigen Augenblicken wird dem Benutzer die Rolle „Mitwirkender für virtuelle Computer“ im Bereich der Ressourcengruppe „rbac-resource-group“ zugewiesen.

   ![Rollenzuweisung: Mitwirkender von virtuellen Computern](./media/quickstart-assign-role-user-portal/vm-contributor-assignment.png)

## <a name="remove-access"></a>Zugriff entfernen

In Azure RBAC entfernen Sie eine Rollenzuweisung, um den Zugriff zu entfernen.

1. Fügen Sie in der Liste der Rollenzuweisungen ein Häkchen neben dem Benutzer mit der Rolle „Mitwirkender für virtuelle Computer“ hinzu.

1. Klicken Sie auf **Entfernen**.

   ![Nachricht zum Entfernen der Rollenzuweisung](./media/quickstart-assign-role-user-portal/remove-role-assignment.png)

1. Klicken Sie in der angezeigten Meldung zum Entfernen der Rollenzuweisung auf **Ja**.

## <a name="clean-up"></a>Bereinigung

1. Klicken Sie in der Navigationsliste auf **Ressourcengruppen**.

1. Klicken Sie auf **rbac-resource-group**, um die Ressourcengruppe zu öffnen.

1. Klicken Sie auf **Ressourcengruppe löschen**, um die Ressourcengruppe zu löschen.

   ![Ressourcengruppe löschen](./media/quickstart-assign-role-user-portal/delete-resource-group.png)

1. Geben Sie auf dem Blatt **Löschvorgang bestätigen**  den Namen der Ressourcengruppe ein: **rbac-resource-group**.

1. Klicken Sie auf **Löschen**, um die Ressourcengruppe zu löschen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Gewähren des Zugriffs auf Azure-Ressourcen für einen Benutzer mithilfe von Azure PowerShell](tutorial-role-assignments-user-powershell.md)
