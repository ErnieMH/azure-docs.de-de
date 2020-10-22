---
title: Verwalten der Benutzereinstellungen für Azure Multi-Factor Authentication – Azure Active Directory
description: Erfahren Sie, wie Sie Azure Active Directory-Benutzereinstellungen für Azure Multi-Factor Authentication konfigurieren können.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 10/05/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2006422d3516aa67076233b0b4b9d3e7c58a7232
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2020
ms.locfileid: "92166510"
---
# <a name="manage-user-settings-for-azure-multi-factor-authentication"></a>Verwalten der Benutzereinstellungen für Azure Multi-Factor Authentication

Zur Unterstützung der Verwaltung der Azure Multi-Factor Authentication-Benutzer können Sie festlegen, dass Benutzer Ihr Kennwort zurücksetzen, sich erneut für MFA registrieren oder vorhandene MFA-Sitzungen widerrufen müssen. Für Benutzer, die App-Kennwörter definiert haben, können Sie diese Kennwörter auch löschen, sodass bei der Legacy-Authentifizierung in diesen Anwendungen ein Fehler auftritt. Diese Aktionen sind möglicherweise erforderlich, wenn Sie einen Benutzer unterstützen müssen oder seinen Sicherheitsstatus zurücksetzen möchten.

## <a name="manage-user-authentication-options"></a>Verwalten von Authentifizierungsoptionen

Wenn Ihnen die Rolle *Authentifizierungsadministrator* zugewiesen wurde, können Sie festlegen, dass Benutzer ihr Kennwort zurücksetzen, sich für die mehrstufige Authentifizierung noch mal registrieren oder vorhandene MFA-Sitzungen von ihrem Benutzerobjekt widerrufen müssen. Um Benutzereinstellungen zu verwalten, führen Sie die folgenden Schritte aus:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie auf der linken Seite **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie den Benutzer aus, für den Sie eine Aktion ausführen möchten, und wählen Sie anschließend **Authentifizierungsmethoden** aus. Wählen Sie dann am oberen Rand des Fensters eine der folgenden Optionen für den Benutzer aus:
   - **Kennwort zurücksetzen**: Das Kennwort des Benutzers wird zurückgesetzt, und ein temporäres Kennwort wird zugewiesen, das bei der nächsten Anmeldung geändert werden muss.
   - **Erneute MFA-Registrierung erforderlich**: Sorgt dafür, dass der Benutzer bei der nächsten Anmeldung zur Einrichtung einer neuen MFA-Authentifizierungsmethode aufgefordert wird.
   
      > [!NOTE]
      > Die derzeit registrierten Authentifizierungsmethoden des Benutzers werden nicht gelöscht, wenn ein Administrator eine erneute Registrierung für MFA benötigt. Nachdem sich ein Benutzer erneut für die MFA registriert hat, empfiehlt es sich, seine Sicherheitsinformationen zu überprüfen und alle zuvor registrierten Authentifizierungsmethoden zu löschen, die nicht mehr verwendet werden können.
   
   - **MFA-Sitzungen widerrufen**: Löscht die gespeicherten MFA-Sitzungen des Benutzers. Wenn auf dem Gerät gemäß Richtlinie das nächste Mal eine MFA erforderlich ist, muss der Benutzer diese durchführen.
   
    :::image type="content" source="media/howto-mfa-userdevicesettings/manage-authentication-methods-in-azure.png" alt-text="Verwalten von Authentifizierungsmethoden im Azure-Portal":::

## <a name="delete-users-existing-app-passwords"></a>Löschen vorhandener App-Kennwörter eines Benutzers

Falls erforderlich, können Sie alle App-Kennwörter löschen, die ein Benutzer erstellt hat. Nicht-Browser-Apps, die diesen App-Kennwörtern zugeordnet waren, sind bis zur Erstellung eines neuen App-Kennworts nicht mehr funktionsfähig. Zum Ausführen dieser Aktion sind *globale Administratorberechtigungen* erforderlich.

Um die App-Kennwörter eines Benutzers zu löschen, führen Sie die folgenden Schritte aus:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie auf der linken Seite **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie **Multi-Factor Authentication** aus. Scrollen Sie bei Bedarf nach rechts, um diese Menüoption anzuzeigen. Wählen Sie den unten gezeigten Beispielscreenshot aus, um das gesamte Fenster des Azure-Portals sowie die Menüposition anzuzeigen: [![Aktivieren von MFA (Multi-Factor Authentication) im Fenster „Benutzer“ in Azure AD](media/howto-mfa-userstates/selectmfa-cropped.png)](media/howto-mfa-userstates/selectmfa.png#lightbox)
1. Aktivieren Sie das Kontrollkästchen neben den Benutzern, die Sie verwalten möchten. Auf der rechten Seite wird eine Liste mit Optionen für schnelle Schritte angezeigt.
1. Wählen Sie **Benutzereinstellungen verwalten** aus, und aktivieren Sie dann das Kontrollkästchen **Alle vorhandenen App-Kennwörter löschen, die von den ausgewählten Benutzern erstellt wurden**, wie im folgenden Beispiel gezeigt: ![Alle vorhandenen App-Kennwörter löschen](./media/howto-mfa-userdevicesettings/deleteapppasswords.png)
1. Wählen Sie **Speichern** und dann **Schließen** aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel lernten Sie, wie Sie die Konfiguration individueller Benutzereinstellungen unterstützen. Informationen zum Konfigurieren der Azure Multi-Factor Authentication-Diensteinstellungen finden Sie unter [Konfigurieren von Azure Multi-Factor Authentication-Einstellungen](howto-mfa-mfasettings.md).

Wenn Ihre Benutzer Hilfe benötigen, finden Sie weitere Informationen im [Benutzerhandbuch für Azure Multi-Factor Authentication](../user-help/multi-factor-authentication-end-user-first-time.md).
