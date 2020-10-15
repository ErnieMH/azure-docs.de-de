---
title: Verwalten von Ressourcengruppen – Azure-Portal
description: Verwenden Sie das Azure-Portal, um Ihre Ressourcengruppen über Azure Resource Manager zu verwalten. Hier wird gezeigt, wie Sie Ressourcengruppen erstellen, auflisten und löschen.
author: mumian
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: jgao
ms.openlocfilehash: 6086dffaefba003461a6edd8177afab05377103d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91371251"
---
# <a name="manage-azure-resource-manager-resource-groups-by-using-the-azure-portal"></a>Verwalten von Azure Resource Manager-Gruppen mithilfe des Azure-Portals

Erfahren Sie , wie Sie mit dem [Azure-Portal](https://portal.azure.com) und [Azure Resource Manager](overview.md) Ihre Azure-Ressourcengruppen verwalten. Informationen zum Verwalten von Azure-Ressourcen finden Sie unter [Manage Azure resources by using the Azure portal (Verwalten von Azure-Ressourcen mithilfe des Azure-Portals)](manage-resources-portal.md).

Weitere Artikel zum Verwalten von Ressourcengruppen:

- [Verwalten von Azure-Ressourcengruppen mithilfe der Azure-Befehlszeilenschnittstelle](manage-resources-cli.md)
- [Verwalten von Azure-Ressourcengruppen mithilfe von Azure PowerShell](manage-resources-powershell.md)

[!INCLUDE [Handle personal data](../../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-resource-group"></a>Was ist eine Ressourcengruppe?

Eine Ressourcengruppe ist ein Container, der verwandte Ressourcen für eine Azure-Lösung enthält. Die Ressourcengruppe kann alle Ressourcen für die Lösung oder nur die Ressourcen enthalten, die Sie als Gruppe verwalten möchten. Sie entscheiden in Abhängigkeit davon, was für Ihre Organisation am sinnvollsten ist, wie Sie die Ressourcen den Ressourcengruppen zuordnen möchten. Im Allgemeinen fügen Sie einer Ressourcengruppe Ressourcen hinzu, die den gleichen Lebenszyklus haben, damit Sie diese einfacher als Gruppe bereitstellen, aktualisieren und löschen können.

In der Ressourcengruppe werden Metadaten zu den Ressourcen gespeichert. Wenn Sie einen Standort für die Ressourcengruppe angeben, legen Sie also fest, wo die Metadaten gespeichert werden. Aus Compliance-Gründen müssen Sie unter Umständen sicherstellen, dass Ihre Daten in einer bestimmten Region gespeichert werden.


## <a name="create-resource-groups"></a>Erstellen von Ressourcengruppe

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie **Ressourcengruppen** aus.

    ![Ressourcengruppe hinzufügen](./media/manage-resource-groups-portal/manage-resource-groups-add-group.png)
3. Wählen Sie **Hinzufügen**.
4. Geben Sie die folgenden Werte ein:

   - **Abonnement**: Wählen Sie Ihr Azure-Abonnement. 
   - **Ressourcengruppe**: Geben Sie einen neuen Namen für die Ressourcengruppe ein. 
   - **Region**: Wählen Sie einen Azure-Standort aus, z. B. **USA, Mitte**.

     ![Erstellen einer Ressourcengruppe](./media/manage-resource-groups-portal/manage-resource-groups-create-group.png)
5. Klicken Sie auf **Bewerten + erstellen**.
6. Klicken Sie auf **Erstellen**. Die Erstellung einer Ressourcengruppe dauert ein paar Sekunden.
7. Klicken Sie oben im Menü auf **Aktualisieren**, um die Liste mit Ressourcengruppen zu aktualisieren. Klicken Sie dann auf die neu erstellte Ressourcengruppe, um sie zu öffnen. Alternativ können Sie die neu erstellte Ressourcengruppe auch öffnen, indem Sie oben auf **Benachrichtigungen** (das Glockensymbol) klicken, und dann **Zu Ressourcengruppe wechseln** auswählen.

    ![Screenshot: Zu Ressourcengruppe wechseln](./media/manage-resource-groups-portal/manage-resource-groups-add-group-go-to-resource-group.png)

## <a name="list-resource-groups"></a>Ressourcengruppen auflisten

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wenn Sie eine Auflistung der Ressourcengruppen anzeigen lassen wollen, klicken Sie auf **Ressourcengruppen**.

    ![Ressourcengruppen durchsuchen](./media/manage-resource-groups-portal/manage-resource-groups-list-groups.png)

3. Wählen Sie zum Anpassen der für die Ressourcengruppen angezeigten Informationen die Option **Spalten bearbeiten** aus. Im folgenden Screenshot werden die zusätzlichen Spalten gezeigt, die Sie anzeigen lassen können:

## <a name="open-resource-groups"></a>Ressourcengruppen öffnen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Ressourcengruppen**.
3. Wählen Sie die Ressourcengruppe aus, die Sie öffnen möchten.

## <a name="delete-resource-groups"></a>Löschen von Ressourcengruppen

1. Öffnen Sie die Ressourcengruppe, die Sie löschen möchten.  Weitere Informationen finden Sie unter [Ressourcengruppen öffnen](#open-resource-groups).
2. Wählen Sie die Option **Ressourcengruppe löschen**.

    ![Screenshot: Azure-Ressourcengruppe löschen](./media/manage-resource-groups-portal/delete-group.png)

Weitere Informationen dazu, in welcher Reihenfolge Ressourcenlöschungen in Azure Resource Manager durchgeführt werden, finden Sie unter [Azure Resource Manager: Löschvorgang von Ressourcengruppen](delete-resource-group.md).

## <a name="deploy-resources-to-a-resource-group"></a>Bereitstellen von Ressourcen in einer Ressourcengruppe

Nachdem Sie eine Azure Resource Manager-Vorlage erstellt haben, können Sie Ihre Azure-Ressourcen über das Azure-Portal bereitstellen. Eine Anleitung zum Erstellen einer Vorlage finden Sie unter [Schnellstart: Erstellen und Bereitstellen von Azure Resource Manager-Vorlagen über das Azure-Portal](../templates/quickstart-create-templates-use-the-portal.md). Informationen zum Bereitstellen einer Vorlage über das Portal finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure-Portal](../templates/deploy-portal.md).

## <a name="move-to-another-resource-group-or-subscription"></a>Verschieben in eine andere Ressourcengruppe oder ein anderes Abonnement

Sie können Ressourcen aus einer Ressourcengruppe in eine andere Ressourcengruppe verschieben. Weitere Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](move-resource-group-and-subscription.md).

## <a name="lock-resource-groups"></a>Ressourcengruppen sperren

Das Sperren verhindert, dass andere Benutzer in Ihrer Organisation versehentlich wichtige Ressourcen löschen oder ändern, z. B. ein Azure-Abonnement, eine Ressourcengruppe oder eine Ressource. 

1. Öffnen Sie die Ressourcengruppe, die Sie sperren möchten.  Weitere Informationen finden Sie unter [Ressourcengruppen öffnen](#open-resource-groups).
2. Wählen Sie im linken Bereich die Option **Sperren** aus.
3. Klicken Sie auf **Hinzufügen**, um der Ressourcengruppe eine Sperre hinzuzufügen.
4. Geben Sie **Sperrenname**, **Sperrentyp** und **Hinweise** ein. Die Sperrentypen beinhalten **Schreibgeschützt** und **Löschen**.

    ![Screenshot: Sperren einer Azure-Ressourcengruppe](./media/manage-resource-groups-portal/manage-resource-groups-add-lock.png)

Weitere Informationen finden Sie unter [Sperren von Ressourcen, um unerwartete Änderungen zu verhindern](lock-resources.md).

## <a name="tag-resource-groups"></a>Hinzufügen von Tags zu Ressourcengruppen

Sie können Ressourcengruppen und Ressourcen Tags zuordnen, um sie logisch zu organisieren. Weitere Informationen finden Sie unter [Verwenden von Tags zum Organisieren von Azure-Ressourcen](tag-resources.md#portal).

## <a name="export-resource-groups-to-templates"></a>Exportieren von Ressourcengruppen in Vorlagen

Informationen zum Exportieren von Vorlagen finden Sie unter [Exportieren von einzelnen und mehreren Ressourcen in eine Vorlage – Portal](../templates/export-template-portal.md).

## <a name="manage-access-to-resource-groups"></a>Verwalten des Zugriffs auf Ressourcengruppen

Der Zugriff auf Ressourcen in Azure wird mithilfe der [rollenbasierten Zugriffssteuerung in Azure (Azure RBAC)](../../role-based-access-control/overview.md) verwaltet. Weitere Informationen finden Sie unter [Hinzufügen oder Entfernen von Rollenzuweisungen mithilfe des Azure-Portals](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Azure Resource Manager finden Sie unter [Übersicht über den Azure Resource Manager](overview.md).
- Informationen zur Resource Manager-Vorlagensyntax finden Sie unter [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](../templates/template-syntax.md).
- Informationen zum Entwickeln von Vorlagen finden Sie in den [Schritt-für-Schritt-Tutorials](../index.yml).
- Informationen zum Anzeigen der Vorlagenschemas für Azure Resource Manager finden Sie in der [Referenz zu Vorlagen](/azure/templates/).
