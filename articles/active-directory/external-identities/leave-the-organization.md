---
title: Verlassen einer Organisation als Gastbenutzer – Azure Active Directory
description: In diesem Artikel wird gezeigt, wie ein Azure AD B2B-Gastbenutzer eine Organisation über den Zugriffsbereich verlassen kann.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 05/05/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: cc07e038ef382a44c4672b2d63c49d8f5a961cda
ms.sourcegitcommit: 1fbd591a67e6422edb6de8fc901ac7063172f49e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2021
ms.locfileid: "109486315"
---
# <a name="leave-an-organization-as-a-guest-user"></a>Verlassen einer Organisation als Gastbenutzer

Ein Azure Active Directory B2B-Gastbenutzer (Azure AD) kann eine Organisation jederzeit verlassen, wenn er die Apps dieser Organisation nicht mehr benötigt oder keine Verbindung mehr aufrechterhalten muss. Ein Benutzer kann eine Organisation selbst verlassen, ohne sich an einen Administrator wenden zu müssen.

> [!NOTE]
> Ein Gastbenutzer kann eine Organisation nicht verlassen, wenn sein Konto im Basismandanten oder im Ressourcenmandanten deaktiviert ist. Wenn das Konto des Gastbenutzers deaktiviert ist, muss er sich an den Mandantenadministrator wenden. Dieser kann entweder das Gastkonto löschen oder das Gastkonto aktivieren, damit der Benutzer die Organisation verlassen kann.

## <a name="leave-an-organization"></a>Verlassen einer Organisation

Um eine Organisation zu verlassen, führen Sie die folgenden Schritte aus.

1. Wechseln Sie zu Ihrer Seite „Zugriffspanel – Profil“, indem Sie einen der folgenden Schritte ausführen:
   
   - Klicken Sie im [Azure-Portal](https://portal.azure.com) oben rechts auf Ihren Namen, und wählen Sie **Konto anzeigen** aus.
   - Öffnen Sie Ihren [Zugriffsbereich für Apps](https://myapps.microsoft.com), klicken Sie oben rechts auf Ihren Namen, und wählen Sie dann neben **Organisationen** die Option **Konto anzeigen** aus.
 
 
2. Wählen Sie **Organisationen verwalten** aus.
  ![Screenshot mit Benutzereinstellungen im Zugriffsbereich](media/leave-the-organization/manage-organizations.png) 
  
3. Suchen Sie unter **Organisationen** nach der Organisation, die Sie verlassen möchten, und wählen Sie **Organisation verlassen** aus.

   ![Screenshot der Option „Organisation verlassen“ auf der Benutzeroberfläche](media/leave-the-organization/leave-org.png)
4. Wenn Sie aufgefordert werden, Ihre Auswahl zu bestätigen, wählen Sie **Beenden** aus.
> [!NOTE]
   > Ihre eigene Organisation können Sie nicht verlassen.

## <a name="account-removal"></a>Entfernen des Kontos

Wenn ein Benutzer eine Organisation verlässt, wird das Benutzerkonto im Verzeichnis vorläufig gelöscht. Das Benutzerobjekt wird in Azure AD der Organisation standardmäßig in den Bereich **Gelöschte Benutzer** verschoben, aber erst nach 30 Tagen endgültig gelöscht. Bei einer solchen vorläufigen Löschung kann der Administrator das Benutzerkonto (einschließlich Gruppen und Berechtigungen) wiederherstellen, wenn der Benutzer innerhalb eines Zeitraums von 30 Tagen eine Anfrage zur Kontowiederherstellung sendet.

Falls gewünscht, kann ein Mandantenadministrator das Konto innerhalb des Zeitraums von 30 Tagen jederzeit endgültig löschen. Dazu ist Folgendes erforderlich:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Azure Active Directory** aus.
2. Wählen Sie unter **Verwalten** die Option **Benutzer** aus.
3. Wählen Sie **Gelöschte Benutzer** aus.
4. Aktivieren Sie das Kontrollkästchen neben einem gelöschten Benutzer, und wählen Sie dann **Endgültig löschen** aus.

Wenn Sie einen Benutzer endgültig löschen, ist diese Aktion unwiderruflich.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über Azure AD B2B finden Sie unter [Was ist die Azure AD B2B-Zusammenarbeit?](what-is-b2b.md)



