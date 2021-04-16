---
title: Anmelden von Benutzern über eine Web-App | Azure
titleSuffix: Microsoft identity platform
description: Erfahren Sie, wie Sie eine Web-App erstellen, die Benutzer anmeldet (Übersicht).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/17/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 0bbc799f946d318c305a96d9cb8c6831d9242ff6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104578293"
---
# <a name="scenario-web-app-that-signs-in-users"></a>Szenario: Web-App, die Benutzer anmeldet

Hier erfahren Sie, wie Sie eine Web-App erstellen, die Benutzer unter Verwendung von Microsoft Identity Plattform anmeldet.

## <a name="getting-started"></a>Erste Schritte

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Wenn Sie Ihre erste portable ASP.NET Core-Web-App mit Benutzeranmeldung erstellen möchten, verwenden Sie die folgende Schnellstartanleitung:

[Schnellstart: ASP.NET Core-Web-App, die Benutzer anmeldet](quickstart-v2-aspnet-core-webapp.md)

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Wenn Sie wissen möchten, wie Sie einer bereits vorhandenen ASP.NET-Webanwendung Anmeldefunktionen hinzufügen, verwenden Sie die folgende Schnellstartanleitung:

[Schnellstart: ASP.NET-Web-App, die Benutzer anmeldet](quickstart-v2-aspnet-webapp.md)

# <a name="java"></a>[Java](#tab/java)

Für Java-Entwickler steht die folgende Schnellstartanleitung zur Verfügung:

[Schnellstart: Hinzufügen von „Mit Microsoft anmelden“ zu einer Java-Web-App](quickstart-v2-java-webapp.md)

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

Für Node.js-Entwickler steht der folgende Schnellstart zur Verfügung:

[Schnellstart: Hinzufügen von „Mit Microsoft anmelden“ zu einer Node.js-Web-App](quickstart-v2-nodejs-webapp-msal.md)

# <a name="python"></a>[Python](#tab/python)

Für Python-Entwickler steht die folgende Schnellstartanleitung zur Verfügung:

[Schnellstart: Hinzufügen von „Mit Microsoft anmelden“ zu einer Python-Web-App](quickstart-v2-python-webapp.md)

---

## <a name="overview"></a>Übersicht

Eine Web-App mit Authentifizierung ist in der Lage, Benutzer anzumelden. Durch Hinzufügen einer Authentifizierung kann Ihre App auf eingeschränkte Profilinformationen zugreifen, um beispielsweise die Umgebung für Benutzer anpassen.

Web-Apps authentifizieren Benutzer in einem Webbrowser. In diesem Szenario weist die Web-App den Browser des Benutzers an, ihn bei Azure Active Directory (Azure AD) anzumelden. Azure AD gibt über den Browser des Benutzers eine Anmeldeantwort mit Benutzeransprüchen in einem Sicherheitstoken zurück. Bei der Benutzeranmeldung wird das Standardprotokoll [OpenID Connect](./v2-protocols-oidc.md) genutzt – vereinfacht durch die Verwendung von [Middlewarebibliotheken](scenario-web-app-sign-user-app-configuration.md#microsoft-libraries-supporting-web-apps).

![Web-App meldet Benutzer an](./media/scenario-webapp/scenario-webapp-signs-in-users.svg)

In einem zweiten Schritt können Sie die Anwendung so konfigurieren, dass sie Web-APIs für den angemeldeten Benutzer aufruft. Bei diesem nächsten Schritt handelt es sich um ein anderes Szenario, das unter [Szenario: Web-App, die Web-APIs aufruft](scenario-web-app-call-api-overview.md) beschrieben wird.

> [!NOTE]
> Das Hinzufügen einer Anmeldung dient zum Schutz der Web-App sowie zur Überprüfung eines Benutzertokens. Zu diesem Zweck werden **Middlewarebibliotheken** verwendet. Im Falle von .NET sind für dieses Szenario noch keine Microsoft-Authentifizierungsbibliothek (Microsoft Authentication Library, MSAL) erforderlich. Diese Bibliothek dient zum Abrufen eines Tokens, um geschützte APIs aufrufen zu können. Authentifizierungsbibliotheken für .NET werden im nächsten Szenario eingeführt, wenn die Web-App Web-APIs aufrufen muss.

## <a name="specifics"></a>Besonderheiten

- Bei der Anwendungsregistrierung müssen Sie einen Antwort-URI oder – wenn die App an mehreren Standorten bereitgestellt wird – mehrere Antwort-URIs angeben. In bestimmten Fällen (etwa bei ASP.NET und ASP.NET Core) muss das ID-Token aktiviert werden. Außerdem empfiehlt es sich, einen Abmelde-URI einzurichten, damit Ihre Anwendung entsprechend reagiert, wenn sich ein Benutzer abmeldet.
- Im Code für Ihre Anwendung muss die Autorität angegeben werden, an die Ihre Web-App die Anmeldung delegiert. Darüber hinaus empfiehlt es sich gegebenenfalls, die Tokenüberprüfung anzupassen (insbesondere in Partnerszenarien).
- Webanwendungen unterstützen alle Kontotypen. Weitere Informationen finden Sie unter [Unterstützte Kontotypen](v2-supported-account-types.md).

## <a name="recommended-reading"></a>Empfohlene Literatur

[!INCLUDE [recommended-topics](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="next-steps"></a>Nächste Schritte

# <a name="aspnet-core"></a>[ASP.NET Core](#tab/aspnetcore)

Fahren Sie mit dem nächsten Artikel in diesem Szenario fort: [App-Registrierung](./scenario-web-app-sign-user-app-registration.md?tabs=aspnetcore).

# <a name="aspnet"></a>[ASP.NET](#tab/aspnet)

Fahren Sie mit dem nächsten Artikel in diesem Szenario fort: [App-Registrierung](./scenario-web-app-sign-user-app-registration.md?tabs=aspnet).

# <a name="java"></a>[Java](#tab/java)

Fahren Sie mit dem nächsten Artikel in diesem Szenario fort: [App-Registrierung](./scenario-web-app-sign-user-app-registration.md?tabs=java).

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

Fahren Sie mit dem nächsten Artikel in diesem Szenario fort: [App-Registrierung](./scenario-web-app-sign-user-app-registration.md?tabs=nodejs).

# <a name="python"></a>[Python](#tab/python)

Fahren Sie mit dem nächsten Artikel in diesem Szenario fort: [App-Registrierung](./scenario-web-app-sign-user-app-registration.md?tabs=python).

---
