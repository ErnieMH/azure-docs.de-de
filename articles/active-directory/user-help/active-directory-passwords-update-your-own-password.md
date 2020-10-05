---
title: Zurücksetzen von Kennwörtern mit Sicherheitsinformationen – Azure Active Directory | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe Ihrer Sicherheitsinformationen und der zweistufigen Überprüfung Ihr Kennwort zurücksetzen, falls Sie es vergessen haben.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 09/10/2020
ms.author: curtand
ms.openlocfilehash: c2bf4d5facc96a0876d4321a61ed6f414dfd78da
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2020
ms.locfileid: "90029544"
---
# <a name="reset-your-work-or-school-password-using-security-info"></a>Zurücksetzen des Kennworts für Ihr Geschäfts-, Schul- oder Unikonto mithilfe von Sicherheitsinformationen

Wenn Sie das Kennwort für Ihr Geschäfts-, Schul -oder Unikonto vergessen haben, von Ihrem Unternehmen keines erhalten haben oder aus Ihrem Konto ausgesperrt wurden, können Sie Ihre Sicherheitsinformation und Ihr mobiles Gerät verwenden, um das Kennwort Ihres Geschäfts-, Schul -oder Unikontos zurückzusetzen. Ihr Administrator muss dieses Feature aktivieren, damit Sie Ihre Informationen einrichten und [Ihr Kennwort zurücksetzen](./active-directory-passwords-reset-register.md) können.

Wenn Sie Ihr Kennwort kennen und es ändern möchten, führen Sie die Schritte in den Abschnitten zum [Ändern des Kennworts](#how-to-change-your-password) dieses Artikels aus.

>[!Important]
>Dieser Artikel richtet sich an Benutzer, die versuchen, ein vergessenes oder unbekanntes Kennwort für ein Geschäfts-, Schul- oder Unikonto zurückzusetzen. Wenn Sie ein Administrator sind und nach Informationen zum Aktivieren der Self-Service-Kennwortzurücksetzung für Ihre Mitarbeiter oder andere Benutzer suchen, lesen Sie [Bereitstellen von Self-Service-Kennwortzurücksetzung in Azure AD](../authentication/howto-sspr-deployment.md) und weitere Artikel zu diesem Thema.

## <a name="how-to-reset-or-unlock-your-password-for-a-work-or-school-account"></a>Zurücksetzen oder Entsperren Ihres Kennworts für ein Geschäfts-, Schul- oder Unikonto

Wenn Sie nicht auf Ihr Azure Active Directory-Konto (Azure AD) zugreifen können, kann dies mehrere Gründe haben:

- Ihr Kennwort funktioniert nicht, und Sie möchten es zurücksetzen.

- Sie kennen Ihr Kennwort, Ihr Konto ist jedoch gesperrt, und Sie müssen es entsperren.

### <a name="to-reset-your-password-and-get-back-into-your-account"></a>Zurücksetzen Ihres Kennworts und erneutes Aktivieren des Kontos

1. Wählen Sie auf dem Bildschirm **Kennwort eingeben** die Option **Kennwort vergessen** aus.

2. Geben Sie auf dem Bildschirm **Konto wieder aktivieren** die **Benutzer-ID** Ihres Geschäfts-, Schul- oder Unikontos ein, beweisen Sie, dass Sie kein Roboter sind, indem Sie die auf dem Bildschirm angezeigten Zeichen eingeben, und klicken Sie anschließend auf **Weiter**.

   ![Bildschirm „Konto wieder aktivieren“](media/security-info/security-info-back-into-acct.png)

   >[!NOTE]
   >Wenn Ihr Administrator es Ihnen nicht ermöglicht hat, Ihr Kennwort selbst zurückzusetzen, wird anstelle des Bildschirms **Konto wieder aktivieren** ein der **Wenden Sie sich an Ihren Administrator** angezeigt. Über diesen Link können Sie sich mit Ihrem Administrator in Verbindung setzen, um Ihr Kennwort per E-Mail oder in einem Webportal zurücksetzen zu lassen.

3. Wählen Sie eine der folgenden Methoden aus, um Ihre Identität zu verifizieren und Ihr Kennwort zu ändern. Je nachdem, wie Ihr Administrator dieses Verfahren für Ihre Organisation eingerichtet hat, müssen Sie diesen Prozess möglicherweise ein zweites Mal durchlaufen und dabei Informationen zu einem zweiten Überprüfungsschritt hinzufügen.

    ![Konto wieder aktivieren, Überprüfungsschritt 1](media/security-info/security-info-back-into-acct2.png)

    >[!NOTE]
    >Je nachdem, wie Ihr Administrator dieses Verfahren für Ihre Organisation eingerichtet hat, stehen einige dieser Überprüfungsoptionen möglicherweise nicht zur Verfügung. Sie müssen zuvor Ihr Mobilgerät so eingerichtet haben, dass es mit mindestens einer dieser Methoden zur Überprüfung verwendet werden kann.<br><br>Darüber hinaus muss Ihr Kennwort möglicherweise bestimmte Anforderungen an die Kennwortsicherheit erfüllen. Sichere Kennwörter bestehen in der Regel aus 8 bis 16 Zeichen und umfassen Groß- und Kleinbuchstaben, mindestens eine Ziffer und mindestens ein Sonderzeichen.

- **Kennwort unter Verwendung einer E-Mail-Adresse zurücksetzen**. Hierbei wird eine E-Mail an die E-Mail-Adresse gesendet, die Sie zuvor in der zweistufigen Überprüfung oder in den Sicherheitsinformationen eingerichtet haben. Wenn Ihr Administrator die Oberfläche für Sicherheitsinformationen aktiviert hat, finden Sie im Artikel [Einrichten der Sicherheitsinformationen zur Verwendung einer E-Mail-Adresse (Vorschau)](security-info-setup-email.md) weitere Informationen zum Einrichten einer E-Mail-Adresse. Wenn Sie noch keine Sicherheitsinformationen verwenden, finden Sie weitere Informationen zum Einrichten einer E-Mail-Adresse im Artikel [Einrichten meines Kontos für die zweistufige Überprüfung](multi-factor-authentication-end-user-first-time.md). 

    1. Wählen Sie **E-Mail an meine alternative E-Mail-Adresse senden** und dann **E-Mail-Adresse** aus.

    2. Geben Sie den Prüfcode aus der E-Mail in das Feld ein, und wählen Sie dann **Weiter**.

    3. Geben Sie Ihr neues Kennwort ein, bestätigen Sie es, und wählen Sie dann **Fertig stellen**.

- **Kennwort unter Verwendung einer SMS zurücksetzen**. Hierbei erfolgt ein Anruf an die Telefonnummer, die Sie zuvor in den Sicherheitsinformationen eingerichtet haben. Wenn Ihr Administrator die Oberfläche für Sicherheitsinformationen aktiviert hat, finden Sie im Artikel [Einrichten der Sicherheitsinformationen zur Verwendung von SMS (Vorschau)](security-info-setup-text-msg.md) weitere Informationen zum Einrichten der Benachrichtigung per SMS. Wenn Sie noch keine Sicherheitsinformationen verwenden, finden Sie weitere Informationen zum Einrichten der Benachrichtigung per SMS im Artikel [Einrichten meines Kontos für die zweistufige Überprüfung](multi-factor-authentication-end-user-first-time.md).

    1. Wählen Sie **Textnachricht an mein Mobiltelefon senden** aus, geben Sie Ihre Telefonnummer ein, und wählen Sie dann **SMS** aus.

    2. Geben Sie den Prüfcode aus der SMS in das Feld ein, und wählen Sie dann **Weiter**.

    3. Geben Sie Ihr neues Kennwort ein, bestätigen Sie es, und wählen Sie dann **Fertig stellen**.

- **Kennwort unter Verwendung einer Telefonnummer zurücksetzen**. Hierbei erfolgt ein Anruf an die Telefonnummer, die Sie zuvor in den Sicherheitsinformationen eingerichtet haben. Wenn Ihr Administrator die Oberfläche für Sicherheitsinformationen aktiviert hat, finden Sie im Artikel [Einrichten der Sicherheitsinformationen zur Verwendung eines Telefonanrufs (Vorschau)](security-info-setup-phone-number.md) weitere Informationen zum Einrichten einer Telefonnummer. Wenn Sie noch keine Sicherheitsinformationen verwenden, finden Sie weitere Informationen zum Einrichten einer Telefonnummer im Artikel [Einrichten meines Kontos für die zweistufige Überprüfung](multi-factor-authentication-end-user-first-time.md).

    1. Wählen Sie **Mein Mobiltelefon anrufen** aus, geben Sie Ihre Telefonnummer ein, und wählen Sie dann **Anruf** aus.

    2. Nehmen Sie den Anruf an, und befolgen Sie die Anweisungen zur Überprüfung Ihrer Identität, und wählen Sie dann **Weiter**.

    3. Geben Sie Ihr neues Kennwort ein, bestätigen Sie es, und wählen Sie dann **Fertig stellen**.

- **Kennwort unter Verwendung von Sicherheitsfragen zurücksetzen**. Hierbei wird Ihnen eine Liste von Sicherheitsfragen angezeigt, die Sie in den Sicherheitsinformationen eingerichtet haben. Wenn Ihr Administrator die Oberfläche für Sicherheitsinformationen aktiviert hat, finden Sie im Artikel [Einrichten der Sicherheitsinformationen zur Verwendung von Sicherheitsfragen (Vorschau)](security-info-setup-questions.md) weitere Informationen zum Einrichten von Sicherheitsfragen. Wenn Sie noch keine Sicherheitsinformationen verwenden, finden Sie weitere Informationen zum Einrichten Sicherheitsfragen im Artikel [Einrichten meines Kontos für die zweistufige Überprüfung](multi-factor-authentication-end-user-first-time.md).

    1. Wählen Sie **Meine Sicherheitsfragen beantworten** aus, beantworten Sie die Fragen, und wählen Sie dann **Weiter**.

    2. Geben Sie Ihr neues Kennwort ein, bestätigen Sie es, und wählen Sie dann **Fertig stellen**.

- **Kennwort unter Verwendung einer Benachrichtigung von Ihrer Authenticator-App zurücksetzen**. Hier wird eine Genehmigungsbenachrichtigung an die Authenticator-App gesendet. Wenn Ihr Administrator die Oberfläche für Sicherheitsinformationen aktiviert hat, finden Sie im Artikel [Einrichten der Sicherheitsinformationen zur Verwendung einer Authentifizierungs-App (Vorschau)](security-info-setup-auth-app.md) weitere Informationen dazu, wie Sie eine Authenticator-App für das Senden einer Benachrichtigung einrichten. Wenn Sie noch keine Sicherheitsinformationen verwenden, finden Sie weitere dazu, wie Sie eine Authenticator-App für das Senden einer Benachrichtigung einrichten, im Artikel [Einrichten meines Kontos für die zweistufige Überprüfung](multi-factor-authentication-end-user-first-time.md).

    1. Wählen Sie **Benachrichtigung in meiner Authenticator-App genehmigen** und dann **Benachrichtigung senden** aus.

    2. Genehmigen Sie die Anmeldung in Ihrer Authenticator-App.

    3. Geben Sie Ihr neues Kennwort ein, bestätigen Sie es, und wählen Sie dann **Fertig stellen**.

- **Kennwort unter Verwendung eines Codes von Ihrer Authenticator-App zurücksetzen**. Hierbei wird ein zufällig erzeugter Code akzeptiert, der von Ihrer Authentifizierungs-App bereitgestellt wird. Wenn Ihr Administrator die Oberfläche für Sicherheitsinformationen aktiviert hat, finden Sie im Artikel [Einrichten der Sicherheitsinformationen zur Verwendung einer Authentifizierungs-App (Vorschau)](security-info-setup-auth-app.md) weitere Informationen dazu, wie Sie eine Authenticator-App für die Bereitstellung eines Codes einrichten. Wenn Sie noch keine Sicherheitsinformationen verwenden, finden Sie weitere Informationen dazu, wie Sie eine Authenticator-App für die Bereitstellung eines Codes einrichten, im Artikel [Einrichten meines Kontos für die zweistufige Überprüfung](multi-factor-authentication-end-user-first-time.md).

  1. Wählen Sie **Code von meiner Authenticator-App eingeben** und dann **Benachrichtigung senden** aus.

  2. Öffnen Sie Ihre Authenticator-App, geben Sie den Prüfcode für Ihr Konto in das Feld ein, und wählen Sie dann **Weiter**.

  3. Geben Sie Ihr neues Kennwort ein, bestätigen Sie es, und wählen Sie dann **Fertig stellen**.

  4. Nachdem Sie die Meldung erhalten haben, dass Ihr Kennwort zurückgesetzt wurde, können Sie sich mit Ihrem neuen Kennwort bei Ihrem Konto anmelden.

     Sollten Sie weiterhin nicht auf Ihr Konto zugreifen können, wenden Sie sich an den Administrator Ihrer Organisation, um weitere Hilfe zu erhalten.

Nach dem Zurücksetzen des Kennworts erhalten Sie möglicherweise eine Bestätigungs-E-Mail von einem Konto wie „Microsoft im Auftrag von \<*your_organization*>“. Wenn Sie eine solche E-Mail erhalten, aber nicht kurz zuvor Ihr Kennwort zurückgesetzt haben, müssen Sie sofort den Administrator Ihrer Organisation kontaktieren.

## <a name="how-to-change-your-password"></a>Ändern des Kennworts

Wenn Sie Ihr Kennwort nur ändern möchten, können Sie dies im Office 365-Portal, im Portal „Meine Apps“ oder auf der Windows 10-Anmeldeseite tun.

### <a name="to-change-your-password-using-the-office-365-portal"></a>Ändern des Kennworts über das Office 365-Portal

Verwenden Sie die folgende Methode, wenn Sie für gewöhnlich über das Office-Portal auf Ihre Apps zugreifen:

1. Melden Sie sich mit Ihrem vorhandenen Kennwort bei Ihrem [Office 365-Konto](https://portal.office.com) an.

2. Wählen Sie rechts oben Ihr Profil und dann **Konto anzeigen** aus.

3. Wählen Sie **Sicherheit und Datenschutz** > **Kennwort** aus.

4. Geben Sie Ihr altes Kennwort ein, legen Sie Ihr neues Kennwort fest, bestätigen Sie es, und wählen Sie anschließend **Absenden** aus.

### <a name="to-change-your-password-from-the-my-apps-portal"></a>So ändern Sie Ihr Kennwort im Portal „Meine Apps“

Verwenden Sie die folgende Methode, wenn Sie für gewöhnlich über das Portal „Meine Apps“ auf Ihre Apps zugreifen:

1. Melden Sie sich mit Ihrem vorhandenen Kennwort beim [Portal „Meine Apps“](https://myapps.microsoft.com/) an.

2. Wählen Sie rechts oben Ihr Profil und dann **Profil** aus.

3. Wählen Sie **Kennwort ändern** aus.

4. Geben Sie Ihr altes Kennwort ein, legen Sie Ihr neues Kennwort fest, bestätigen Sie es, und wählen Sie anschließend **Absenden** aus.

### <a name="to-change-your-password-at-windows-sign-in"></a>Ändern eines Kennworts bei der Windows-Anmeldung

Wenn Ihr Administrator diese Funktion aktiviert hat, sehen Sie auf dem Anmeldebildschirm von Windows 7, Windows 8, Windows 8.1 oder Windows 10 einen Link **Kennwort zurücksetzen**.

1. Öffnen Sie den Link **Kennwort zurücksetzen**, um den Prozess der Kennwortzurücksetzung zu starten, ohne die normale webbasierte Oberfläche verwenden zu müssen.

2. Bestätigen Sie Ihre Benutzer-ID, und wählen Sie **Weiter** aus.

3. Wählen Sie eine Kontaktmethode für die Überprüfung aus, und bestätigen Sie diese. Wählen Sie ggf. eine zweite Überprüfungsoption, die sich von der bisherigen unterscheidet, und geben Sie die erforderlichen Informationen an.

4. Geben Sie auf der Seite **Neues Kennwort erstellen** ein neues Kennwort ein, bestätigen Sie Ihr neues Kennwort, und wählen Sie anschließend **Weiter** aus.

    Sichere Kennwörter bestehen in der Regel aus 8 bis 16 Zeichen und umfassen Groß- und Kleinbuchstaben, mindestens eine Ziffer und mindestens ein Sonderzeichen.

5. Nachdem Sie die Meldung erhalten haben, dass Ihr Kennwort zurückgesetzt wurde, können Sie **Fertig stellen** auswählen.

    Sollten Sie weiterhin nicht auf Ihr Konto zugreifen können, wenden Sie sich an den Administrator Ihrer Organisation, um weitere Hilfe zu erhalten.

## <a name="common-problems-and-their-solutions"></a>Häufige Probleme und Lösungen

Hier finden Sie einige häufige Fehler und die passenden Lösungen:

|Problem|BESCHREIBUNG|Lösung|
| --- | --- | --- |
|Beim Versuch, mein Kennwort zu ändern, tritt ein Fehler auf. |Ihr Kennwort ist aufgrund eines enthaltenen Worts, Ausdrucks oder Musters leider leicht zu erraten.| Verwenden Sie ein sichereres Kennwort, und versuchen Sie es noch mal.|
|Nachdem ich meine Benutzer-ID eingegeben habe, gelange ich auf eine Seite, die mich auffordert, mich an den Administrator zu wenden.|Microsoft hat festgestellt, dass das Kennwort Ihres Benutzerkontos von Ihrem Administrator in einer lokalen Umgebung verwaltet wird. Sie können Ihr Kennwort daher nicht über den Link „Zugriff auf Ihr Konto nicht möglich“ zurücksetzen. |Wenden Sie sich an den Administrator, um Hilfe zu erhalten.|
|Nach dem Eingeben meiner Benutzer-ID erhalte ich die Fehlermeldung „Ihr Konto ist für ein Zurücksetzen des Kennworts nicht aktiviert“.|Ihr Administrator hat Ihr Konto nicht so eingerichtet, dass Sie Ihr Kennwort selbst zurücksetzen können.|Ihr Administrator hat die Kennwortzurücksetzung über den Link „Zugriff auf Ihr Konto nicht möglich“ für Ihre Organisation nicht aktiviert oder Ihnen die Nutzung dieser Funktion nicht gestattet.<br><br> Öffnen Sie zum Zurücksetzen Ihres Kennworts den Link zur Kontaktaufnahme mit einem Administrator, und teilen Sie dem Administrator Ihres Unternehmens in einer E-Mail mit, dass Sie Ihr Kennwort zurücksetzen möchten.|
|Nachdem ich meine Benutzer-ID eingegeben habe, erhalte ich eine Fehlermeldung, dass mein Konto nicht überprüft werden konnte.|Während des Anmeldevorgangs konnten Ihre Kontoinformationen nicht überprüft werden.|Es gibt zwei Gründe dafür, dass diese Meldung angezeigt wird.<br><br>1. Ihr Administrator hat die Kennwortzurücksetzung für Ihre Organisation aktiviert, Sie haben sich jedoch noch nicht für die Nutzung des Diensts registriert. In einem der folgenden Artikel finden Sie die für Sie geeigneten Informationen zum Registrieren für die Kennwortzurücksetzung (je nach verwendeter Überprüfungsmethode): [Einrichten der Sicherheitsinformationen zur Verwendung einer Authenticator-App (Vorschau)](security-info-setup-auth-app.md), [Einrichten der Sicherheitsinformationen zur Verwendung eines Telefonanrufs (Vorschau)](security-info-setup-phone-number.md), [Einrichten der Sicherheitsinformationen zur Verwendung von SMS (Vorschau)](security-info-setup-text-msg.md), [Einrichten der Sicherheitsinformationen zur Verwendung einer E-Mail-Adresse (Vorschau)](security-info-setup-email.md) oder [Einrichten der Sicherheitsinformationen zur Verwendung von Sicherheitsfragen (Vorschau)](security-info-setup-questions.md).<br><br>2. Ihr Administrator hat die Kennwortzurücksetzung für Ihre Organisation nicht aktiviert. In diesem Fall müssen Sie den Link zur Kontaktaufnahme mit einem Administrator öffnen, um eine E-Mail an Ihren Administrator zu senden und darum zu bitten, dass Ihr Kennwort zurückgesetzt wird.|

## <a name="next-steps"></a>Nächste Schritte

- Unter [Übersicht über die Sicherheitsinformationen (Vorschau)](./security-info-setup-signin.md) erfahren Sie mehr über die Sicherheitsinformationen.

- Wenn Sie versuchen, wieder Zugriff auf ein persönliches Konto (beispielsweise ein Xbox-, hotmail.com- oder outlook.com-Konto) zu erhalten, probieren Sie die Vorschläge im Artikel [Wenn Sie sich nicht bei Ihrem Microsoft-Konto anmelden können](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) aus.