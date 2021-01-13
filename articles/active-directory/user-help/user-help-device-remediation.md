---
title: Fehlerbehebungen für den Fehler „Der Wechsel von hier nach dort ist nicht möglich“ – Azure AD
description: Hier finden Sie potenzielle Korrekturen für die Fehlermeldung „Der Wechsel von hier nach dort ist nicht möglich“.
services: active-directory
author: curtand
manager: daveba
ms.assetid: 8ad0156c-0812-4855-8563-6fbff6194174
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 10/10/2018
ms.author: curtand
ms.reviewer: jairoc
ms.custom: user-help, seo-update-azuread-jan
ms.openlocfilehash: df3941c895ce67862eb53b8e96bc7a6d53c1ed02
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88799416"
---
# <a name="possible-fixes-for-the-you-cant-get-there-from-here-error-message"></a>Mögliche Korrekturen für die Fehlermeldung „Der Wechsel von hier nach dort ist nicht möglich“

Beim Zugriff auf interne Web-Apps oder Dienste Ihrer Organisation wird unter Umständen die Fehlermeldung **Der Wechsel von hier nach dort ist nicht möglich.** angezeigt. Diese Meldung bedeutet, dass Ihre Organisation eine Richtlinie eingerichtet hat, die Ihr Gerät davon abhält, auf die Ressourcen Ihrer Organisation zuzugreifen. Es kann sein, dass Sie sich zur Behebung dieses Problems letztendlich an den Helpdesk wenden müssen, vorher gibt es aber noch ein paar Dinge, die Sie versuchen können.

## <a name="make-sure-youre-using-a-supported-browser"></a>Vergewissern Sie sich, dass Sie einen unterstützten Browser verwenden.
Wenn die Meldung **Der Wechsel von hier nach dort ist nicht möglich.** den Hinweis enthält, dass Sie versuchen, über einen nicht unterstützten Browser auf die Websites Ihrer Organisation zuzugreifen, überprüfen Sie, welchen Browser Sie verwenden.

![Fehlermeldung im Zusammenhang mit der Browserunterstützung](media/user-help-device-remediation/browser-version.png)

Zur Behebung dieses Problems müssen Sie einen unterstützten Browser für Ihr Betriebssystem installieren und ausführen. Unter Windows 10 werden unter anderem Microsoft Edge, Internet Explorer und Google Chrome unterstützt. Benutzer anderer Betriebssysteme finden [hier](../conditional-access/concept-conditional-access-conditions.md#supported-browsers) eine umfassende Liste der unterstützten Browser.

## <a name="make-sure-youre-using-a-supported-operating-system"></a>Vergewissern Sie sich, dass Sie ein unterstütztes Betriebssystem verwenden.
Vergewissern Sie sich, dass Sie eine unterstützte Version des Betriebssystems verwenden:

- **Windows-Client:** Windows 7 oder höher

- **Windows-Server:** Windows Server 2008 R2 oder höher

- **macOS:** macOS X oder höher

- **Android und iOS:** Neueste Version der mobilen Betriebssysteme Android und iOS

Zur Behebung dieses Problems müssen Sie ein unterstütztes Betriebssystem installieren und ausführen.

## <a name="make-sure-your-device-is-joined-to-your-network"></a>Vergewissern Sie sich, dass Ihr Gerät in Ihr Netzwerk eingebunden ist.
Wenn die Meldung **Der Wechsel von hier nach dort ist nicht möglich.** den Hinweis enthält, dass Ihr Gerät die Anforderungen der Zugriffsrichtlinie Ihrer Organisation nicht erfüllt, vergewissern Sie sich, dass Ihr Gerät in das Netzwerk Ihrer Organisation eingebunden ist.

![Fehlermeldung im Zusammenhang mit dem Netzwerkverbindungsstatus](media/user-help-device-remediation/network-version.png)

### <a name="to-check-whether-your-device-is-joined-to-your-network"></a>So überprüfen Sie, ob Ihr Gerät in Ihr Netzwerk eingebunden ist
1. Melden Sie sich mit Ihrem Geschäfts-, Schul- oder Unikonto bei Windows an. Beispiel: alain@contoso.com.

2. Stellen Sie über ein VPN (Virtual Private Network) oder über DirectAccess eine Verbindung mit dem Netzwerk Ihrer Organisation her.

3. Drücken Sie nach dem Herstellen der Verbindung die Tastenkombination **Windows-Logo-Taste+L**, um Ihr Gerät zu sperren.

4. Entsperren Sie Ihr Gerät mit Ihrem Geschäfts-, Schul- oder Unikonto, und versuchen Sie erneut, auf die App oder den Dienst zuzugreifen, bei der bzw. bei dem das Problem aufgetreten ist.

    Sollte erneut die Meldung **Der Wechsel von hier nach dort ist nicht möglich.** angezeigt werden, klicken Sie auf den Link **Weitere Details**, und wenden Sie sich mit den Details an Ihren Helpdesk.

### <a name="to-join-your-device-to-your-network"></a>So binden Sie Ihr Gerät in Ihr Netzwerk ein
Falls Ihr Gerät nicht in das Netzwerk Ihrer Organisation eingebunden ist, haben Sie zwei Möglichkeiten:

- **Binden Sie Ihr geschäftliches Gerät ein.** Binden Sie Ihr unternehmenseigenes Windows 10-Gerät in das Netzwerk Ihrer Organisation ein, sodass Sie auf möglicherweise eingeschränkte Ressourcen zugreifen können. Weitere Informationen und eine ausführliche Anleitung finden Sie unter [Einbinden von geschäftlichen Geräten in das Netzwerk der Organisation](user-help-join-device-on-network.md).

- **Registrieren Sie Ihr persönliches Gerät für die Arbeit.** Registrieren Sie Ihr persönliches Gerät, in der Regel ein Smartphone oder ein Tablet, im Netzwerk Ihrer Organisation. Nach der Registrierung Ihres Geräts können Sie damit auf die geschützten Ressourcen Ihrer Organisation zugreifen. Weitere Informationen und eine ausführliche Anleitung finden Sie unter [Registrieren Ihres persönlichen Geräts im Netzwerk Ihrer Organisation](user-help-register-device-on-network.md).

## <a name="next-steps"></a>Nächste Schritte
- [Was ist das MyApps-Portal?](./my-apps-portal-end-user-access.md)

- [Anmelden per Smartphone anstelle Ihres Kennworts](user-help-auth-app-sign-in.md)