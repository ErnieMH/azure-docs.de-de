---
title: Überführen einer Web-App für Benutzeranmeldungen in die Produktion – Microsoft Identity Platform | Azure
description: Erfahren Sie, wie Sie eine Web-App erstellen, die Benutzer anmeldet (Übergang in die Produktion).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/17/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 47a45b52ac10a44b6efd54c41b3fec1e61a47a35
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "82181629"
---
# <a name="web-app-that-signs-in-users-move-to-production"></a>Web-App für Benutzeranmeldungen: Überführen in die Produktion

Da Sie nun wissen, wie Sie ein Token zum Abrufen von Web-APIs aufrufen, erfahren Sie in diesem Artikel, wie Ihnen der Übergang in die Produktion gelingt.

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Nächste Schritte

### <a name="troubleshooting"></a>Problembehandlung

> [!NOTE]
> Wenn Benutzer sich das erste Mal bei der Webanwendung anmelden, ist eine Einwilligung erforderlich. In manchen Organisationen wird Benutzern jedoch eine Meldung ähnlich der folgenden angezeigt:
>
> *Name_der_App benötigt für den Zugriff auf Ressourcen in Ihrer Organisation Berechtigungen, die nur ein Administrator erteilen kann. Bitten Sie einen Administrator, die Berechtigungen für diese App zu erteilen, damit Sie die App verwenden können.*
>
> Dies liegt daran, dass Ihr Mandantenadministrator die Möglichkeit für Benutzer zur Zustimmung **deaktiviert** hat. In diesem Fall müssen Sie sich an Ihre Mandantenadministratoren wenden, damit diese eine Administratorzustimmung für die Bereiche erteilen, die für die Anwendung erforderlich sind.

### <a name="same-site"></a>Gleiche Website

Informieren Sie sich über mögliche Probleme im Zusammenhang mit neuen Versionen des Chrome-Browsers:

> [!div class="nextstepaction"]
> [Verarbeiten von SameSite-Cookieänderungen im Chrome-Browser](howto-handle-samesite-cookie-changes-chrome-browser.md)

Das Microsoft.Identity.Web-NuGet-Paket verarbeitet die häufigsten SameSite-Probleme.

### <a name="scenario-for-calling-web-apis"></a>Szenario zum Aufrufen von Web-APIs

Sobald Ihre Web-App Benutzer anmeldet, kann sie für diese angemeldeten Benutzer Web-APIs aufrufen. Genau darum geht es im folgenden Szenario:

> [!div class="nextstepaction"]
> [Web-App, die Web-APIs aufruft](scenario-web-app-call-api-overview.md)

## <a name="deep-dive-aspnet-core-web-app-tutorial"></a>Ausführliche Betrachtung: Tutorial für ASP.NET Core-Web-Apps

Lernen Sie mithilfe dieses ASP.NET Core-Tutorials andere Möglichkeiten zum Anmelden von Benutzern kennen: 

> [!div class="nextstepaction"]
> [Ermöglichen Sie Ihren Web-Apps das Anmelden von Benutzern und das Aufrufen von APIs mit der Microsoft Identity-Plattform für Entwickler](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial) 

Dieses fortschreitende Tutorial enthält Web-App-Code, der in Produktionsumgebungen eingesetzt werden kann. Außerdem wird beschrieben, wie eine Anmeldung mit Konten hinzugefügt wird, die sich in folgenden Umgebungen befinden:

- Ihrer Organisation
- mehreren Organisationen
- Geschäfts-, Schul- oder Unikonten bzw. persönlichen Microsoft-Konten
- [Azure AD B2C](https://aka.ms/aadb2c)
- Nationale Clouds

## <a name="sample-code-java-web-app"></a>Beispielcode: Java-Web-App

Weitere Informationen zur Java-Web-App aus dem Beispiel auf GitHub finden Sie hier: 

> [!div class="nextstepaction"]
> [Eine Java-Webanwendung, die Benutzer bei Microsoft Identity Platform anmeldet und Microsoft Graph aufruft](https://github.com/Azure-Samples/ms-identity-java-webapp)
