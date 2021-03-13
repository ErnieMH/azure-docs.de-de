---
title: Erstellen einer Zugriffsüberprüfung für Azure-Ressourcenrollen in PIM – Azure AD | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Zugriffsüberprüfung für Azure-Ressourcenrollen in Azure AD Privileged Identity Management (PIM) erstellen.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: pim
ms.date: 03/09/2021
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4276b48584ecbad91794de58abafd7e3367f6877
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102564032"
---
# <a name="create-an-access-review-of-azure-resource-roles-in-privileged-identity-management"></a>Erstellen einer Zugriffsüberprüfung für Azure-Ressourcenrollen in Privileged Identity Management

Die Notwendigkeit, auf privilegierte Ressourcenrollen zugreifen zu müssen, kann sich für Mitarbeiter im Laufe der Zeit ändern. Daher sollten Sie den Zugriff in regelmäßigen Abständen überprüfen, um das mit veralteten Rollenzuweisungen verbundene Risiko zu verringern. Mit Azure Active Directory (Azure AD) Privileged Identity Management (PIM) können Sie Zugriffsüberprüfungen für den privilegierten Zugriff auf Azure-Ressourcenrollen erstellen. Sie können auch wiederholte Zugriffsüberprüfungen konfigurieren, die automatisch ausgeführt werden. In diesem Artikel wird beschrieben, wie Sie eine oder mehrere Zugriffsüberprüfungen erstellen.

## <a name="prerequisite-role"></a>Erforderliche Rolle

 Zum Erstellen von Zugriffsüberprüfungen müssen Sie der Azure-Rolle [Besitzer](../../role-based-access-control/built-in-roles.md#owner) oder [Benutzerzugriffsadministrator](../../role-based-access-control/built-in-roles.md#user-access-administrator) für die Ressource zugewiesen sein.

## <a name="open-access-reviews"></a>Öffnen von Zugriffsüberprüfungen

1. Melden Sie sich im [Azure-Portal](https://portal.azure.com/) als Benutzer an, der einer der erforderlichen Rollen zugewiesen ist.

1. Öffnen Sie **Azure AD Privileged Identity Management**.

1. Wählen Sie im linken Menü die Option **Azure-Ressourcen** aus.

1. Wählen Sie die Ressource aus, die Sie verwalten möchten, z. B. ein Abonnement.

1. Wählen Sie unter „Verwalten“ die Option **Zugriffsüberprüfungen** aus.

    ![Azure-Ressourcen – Liste der Zugriffsüberprüfungen mit dem Status aller Überprüfungen](./media/pim-resource-roles-start-access-review/access-reviews.png)

[!INCLUDE [Privileged Identity Management access reviews](../../../includes/active-directory-privileged-identity-management-access-reviews.md)]

## <a name="start-the-access-review"></a>Starten der Zugriffsüberprüfung

Klicken Sie nach dem Festlegen der Einstellungen für eine Zugriffsüberprüfung auf **Starten**. Die Zugriffsüberprüfung wird in der Liste mit einer Angabe des Status angezeigt.

![Liste der Zugriffsüberprüfungen mit dem Status einer gestarteten Überprüfung](./media/pim-resource-roles-start-access-review/access-reviews-list.png)

Standardmäßig sendet Azure AD kurz nach dem Start der Überprüfung eine E-Mail an die Prüfer. Wenn Sie nicht möchten, dass Azure AD die E-Mail sendet, stellen Sie sicher, dass die Prüfer darüber in Kenntnis gesetzt werden, dass sie eine ausstehende Zugriffsüberprüfung abschließen müssen. Sie können ihnen die Anweisungen zum [Durchführen einer Zugriffsüberprüfung für Azure-Ressourcenrollen in PIM](pim-resource-roles-perform-access-review.md) anzeigen.

## <a name="manage-the-access-review"></a>Verwalten der Zugriffsüberprüfung

Sie können den Fortschritt der Überprüfungen durch die Prüfer auf der Seite **Übersicht** der Zugriffsüberprüfung nachverfolgen. Zugriffsrechte werden im Verzeichnis erst geändert, wenn die [Überprüfung abgeschlossen](pim-resource-roles-complete-access-review.md) ist.

![Übersichtsseite der Zugriffsüberprüfung mit den Details der Überprüfung](./media/pim-resource-roles-start-access-review/access-review-overview.png)

Führen Sie bei einer einmaligen Überprüfung nach Ablauf des Zeitraums für die Zugriffsüberprüfung oder nach Beenden der Zugriffsüberprüfung durch den Administrator die Schritte unter [Abschließen einer Zugriffsüberprüfung für Azure-Ressourcenrollen in PIM](pim-resource-roles-complete-access-review.md) aus, um die Ergebnisse anzuzeigen und anzuwenden.  

Um eine Serie von Zugriffsüberprüfungen zu verwalten, navigieren Sie zur Zugriffsüberprüfung. Dort finden Sie unter den geplanten Überprüfungen die anstehenden Überprüfungen, und Sie können das Enddatum bearbeiten oder Prüfer entsprechend hinzufügen/entfernen.

Basierend auf Ihrer Auswahl unter **Einstellungen nach Abschluss** wird nach dem Enddatum der Überprüfung oder bei manueller Beendigung der Überprüfung die automatische Anwendung ausgeführt. Der Status der Überprüfung ändert sich von **Abgeschlossen** über Zwischenzustände wie **Wird angewandt** schließlich in den Status **Angewandt**. Erwartungsgemäß sollten abgelehnte Benutzer (sofern vorhanden) innerhalb weniger Minuten aus den Rollen entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

- [Durchführen einer Zugriffsüberprüfung für Azure-Ressourcenrollen in PIM](pim-resource-roles-perform-access-review.md)
- [Abschließen einer Zugriffsüberprüfung für Azure-Ressourcenrollen in PIM](pim-resource-roles-complete-access-review.md)
- [Erstellen einer Zugriffsüberprüfung für Azure AD-Rollen in PIM](pim-how-to-start-security-review.md)
