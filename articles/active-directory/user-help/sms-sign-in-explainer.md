---
title: 'Anmeldevorgang mit SMS für Telefonnummer: Azure AD'
description: Erfahren Sie mehr über den Anmeldungsvorgang mit SMS für neue oder vorhandene Telefonnummern.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 06/10/2021
ms.author: curtand
ms.reviewer: kasimpso
ms.custom: user-help, seo-update-azuread-jan
ms.openlocfilehash: 32210818979c6314d2fb262ca4fd51ec1190bcc5
ms.sourcegitcommit: e39ad7e8db27c97c8fb0d6afa322d4d135fd2066
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111984480"
---
# <a name="use-your-phone-number-as-a-user-name"></a>Verwenden Ihrer Telefonnummer als Benutzername

Durch das Registrieren eines Geräts erhält Ihr Telefon Zugriff auf die Dienste Ihrer Organisation, ohne dass Ihre Organisation auf Ihr Telefon zugreifen kann. Wenn Sie Administrator sind, finden Sie weitere Informationen unter [Konfigurieren und Aktivieren der SMS-basierten Authentifizierung von Benutzern](../authentication/howto-authentication-sms-signin.md).

Wenn Ihre Organisation keine SMS-Anmeldung zur Verfügung gestellt hat, wird keine Option dafür angezeigt, wenn Sie ein Telefon bei Ihrem Konto registrieren.  

## <a name="when-you-have-a-new-phone-number"></a>Wenn Sie eine neue Telefonnummer haben

Wenn Sie ein neues Telefon oder eine neue Nummer erhalten und dieses bzw. diese bei einer Organisation registrieren, bei der die SMS-Anmeldung verfügbar ist, können Sie den normalen Telefonregistrierungsvorgang ausführen:

1. Wählen Sie **Methode hinzufügen** aus.
1. Wählen Sie **Telefon** aus.
1. Geben Sie die Telefonnummer ein, und wählen Sie **Code per SMS an mich senden** aus.
1. Geben Sie den Code ein, und wählen Sie **Weiter** aus.
1. Es wird eine Eingabeaufforderung mit folgendem Text angezeigt: „Die SMS wurde verifiziert. Ihr Telefon wurde erfolgreich registriert.“

> [!Important]
> Aufgrund eines bekannten Problems wird die Nummer beim Hinzufügen der Telefonnummer für kurze Zeit nicht für die SMS-Anmeldung registriert. Sie müssen sich mit der hinzugefügten Nummer anmelden und dann den Eingabeaufforderungen folgen, um die Nummer für die SMS-Anmeldung zu registrieren.

### <a name="when-the-phone-number-is-in-use"></a>Wenn die Telefonnummer bereits verwendet wird

Wenn Sie versuchen, eine Telefonnummer zu verwenden, die von einer anderen Person in Ihrer Organisation verwendet wird, wird die folgende Meldung angezeigt:

![Fehlermeldung, wenn Ihre Telefonnummer bereits verwendet wird](media/sms-sign-in-explainer/sms-sign-in-error.png)

Wenden Sie sich an Ihren Administrator, um das Problem zu beheben.

## <a name="when-you-have-an-existing-number"></a>Wenn Sie bereits über eine Telefonnummer verfügen

Wenn Sie bereits eine Telefonnummer in einer Organisation verwenden und die Möglichkeit, Ihre Telefonnummer als Benutzername zu verwenden, verfügbar wird, können Sie sich über die folgenden Schritte anmelden.

1. Wenn die SMS-Anmeldung verfügbar ist, wird ein Banner mit der Frage angezeigt, ob Sie die Telefonnummer für die SMS-Anmeldung aktivieren möchten:

    :::image type="content" source="media/sms-sign-in-explainer/sms-sign-in-banner.png" alt-text="Der Screenshot zeigt das Banner zum Aktivieren der SMS-Anmeldung für eine Telefonnummer, mit ausgewählter Aktion „Aktivieren“." lightbox="media/sms-sign-in-explainer/sms-sign-in-banner.png":::

1. Außerdem wird die Schaltfläche **Aktivieren** angezeigt, wenn Sie das Caretzeichen auf der Kachel für die Telefonmethode auswählen:

    [![Banner zum Aktivieren der SMS-Anmeldung für eine Telefonnummer](media/sms-sign-in-explainer/sms-sign-in-phone-method.png)](media/sms-sign-in-explainer/sms-sign-in-phone-method.png#lightbox)

1. Wählen Sie zum Aktivieren der Methode **Aktivieren** aus. Sie werden aufgefordert, die Aktion zu bestätigen:

    ![Bestätigungsdialogfeld zum Aktivieren der SMS-Anmeldung für eine Telefonnummer](media/sms-sign-in-explainer/sms-sign-in-confirmation.png)

1. Wählen Sie **Aktivieren** aus.

## <a name="when-you-remove-your-phone-number"></a>Wenn Sie Ihre Telefonnummer entfernen

1. Um die Telefonnummer zu löschen, wählen Sie auf der Kachel der Telefonmethode für die SMS-Anmeldung die Schaltfläche „Löschen“ aus.

    [![Banner zum Löschen der SMS-Anmeldung für eine Telefonnummer](media/sms-sign-in-explainer/sms-sign-in-delete-method.png)](media/sms-sign-in-explainer/sms-sign-in-delete-method.png#lightbox)

2. Wählen Sie **OK** aus, wenn Sie aufgefordert werden, die Aktion zu bestätigen.

Eine Telefonnummer, die als Standardanmeldemethode verwendet wird, kann nicht entfernt werden. Um die Nummer zu entfernen, müssen Sie die Standardanmeldemethode ändern und die Telefonnummer dann erneut entfernen.
