---
title: 'Tutorial: Registrieren einer Anwendung'
titleSuffix: Azure AD B2C
description: In diesem Tutorial erfahren Sie, wie Sie eine Webanwendungen in Azure Active Directory B2C mithilfe des Azure-Portals registrieren.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: tutorial
ms.date: 04/10/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 0fd062bd0e58ecc714e4f450c93384e47e743b65
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87922012"
---
# <a name="tutorial-register-a-web-application-in-azure-active-directory-b2c"></a>Tutorial: Registrieren einer Webanwendung in Azure Active Directory B2C

Bevor Ihre [Anwendungen](application-types.md) mit Azure Active Directory B2C (Azure AD B2C) interagieren können, müssen sie in einem von Ihnen verwalteten Mandanten registriert werden. In diesem Tutorial erfahren Sie, wie Sie eine Webanwendung mithilfe des Azure-Portals registrieren.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Registrieren einer Webanwendung
> * Erstellen eines Clientgeheimnisses

Wenn Sie stattdessen eine native App (z. B. eine iOS- oder Android-App oder eine Mobil- oder Desktop-App) verwenden, lesen Sie die Informationen zum [Registrieren einer nativen Clientanwendung](add-native-application.md).

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie Ihren eigenen [Azure AD B2C-Mandanten](tutorial-create-tenant.md) noch nicht erstellt haben, erstellen Sie jetzt einen. Sie können einen vorhandenen Azure AD B2C-Mandanten verwenden.

## <a name="register-a-web-application"></a>Registrieren einer Webanwendung

Zum Registrieren einer Anwendung in Ihrem Azure AD B2C-Mandanten können Sie unsere neue einheitliche Benutzeroberfläche für **App-Registrierungen** oder unsere alte Benutzeroberfläche für **Anwendungen (Legacy)** verwenden. [Weitere Informationen zur neuen Oberfläche](https://aka.ms/b2cappregtraining)

#### <a name="app-registrations"></a>[App-Registrierungen](#tab/app-reg-ga/)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie auf der Symbolleiste des Portals das Symbol **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Suchen Sie im Azure-Portal nach **Azure AD B2C**, und wählen Sie diese Option dann aus.
1. Wählen Sie **App-Registrierungen** aus, und wählen Sie dann **Registrierung einer neuen Anwendung** aus.
1. Geben Sie unter **Name** einen Namen für die Anwendung ein. Beispiel: *webapp1*.
1. Wählen Sie unter **Unterstützte Kontotypen** die Option **Konten in einem Organisationsverzeichnis oder ein beliebiger Identitätsanbieter (zur Authentifizierung von Benutzern bei Azure AD B2C)** aus.
1. Wählen Sie unter **Umleitungs-URI** die Option **Web** aus, und geben Sie `https://jwt.ms` in das URL-Textfeld ein.

    Die Umleitungs-URI ist der Endpunkt, an den der Benutzer vom Autorisierungsserver (in diesem Fall Azure AD B2C) nach Abschluss seiner Interaktion mit dem Benutzer gesendet wird und an den bei erfolgreicher Autorisierung ein Zugriffstoken oder Autorisierungscode gesendet wird. In einer Produktionsanwendung handelt es sich in der Regel um einen öffentlich zugänglichen Endpunkt, an dem Ihre App ausgeführt wird, etwa um `https://contoso.com/auth-response`. Für Testzwecke wie in diesem Tutorial können Sie ihn auf `https://jwt.ms` festlegen. Dabei handelt es sich um eine Microsoft-Webanwendung, die den decodierten Inhalt eines Tokens anzeigt (der Inhalt des Tokens verlässt niemals Ihren Browser). Während der App-Entwicklung können Sie den Endpunkt hinzufügen, an dem die Anwendung lokal lauscht, etwa `https://localhost:5000`. Sie können Umleitungs-URIs in Ihren registrierten Anwendungen jederzeit hinzufügen und ändern.

    Für Umleitungs-URIs gelten die folgenden Einschränkungen:

    * Die Antwort-URL muss mit dem Schema `https` beginnen.
    * Bei der Antwort-URL muss die Groß-/Kleinschreibung beachtet werden. Die Groß-/Kleinschreibung muss der Groß-/Kleinschreibung des URL-Pfads Ihrer ausgeführten Anwendung entsprechen. Wenn Ihre Anwendung z. B. als Teil des Pfads `.../abc/response-oidc` enthält, geben Sie in der Antwort-URL nicht `.../ABC/response-oidc` an. Weil der Webbrowser bei Pfaden die Groß-/Kleinschreibung beachtet, werden Cookies, die `.../abc/response-oidc` zugeordnet sind, möglicherweise ausgeschlossen, wenn eine Umleitung an die anders geschriebene (nicht übereinstimmende) URL `.../ABC/response-oidc` erfolgt.

1. Aktivieren Sie unter **Berechtigungen** das Kontrollkästchen *Administratoreinwilligung für openid- und offline_access-Berechtigungen erteilen*.
1. Wählen Sie **Registrieren**.

Aktivieren Sie nach Abschluss der Anwendungsregistrierung den Ablauf zur impliziten Gewährung:

1. Wählen Sie im linken Menü unter **Verwalten** die Option **Authentifizierung** aus.
1. Aktivieren Sie unter **Implizite Genehmigung** die Kontrollkästchen **Zugriffstoken** und **ID-Token**.
1. Wählen Sie **Speichern** aus.

#### <a name="applications-legacy"></a>[Anwendungen (Legacy)](#tab/applications-legacy/)

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie auf der Symbolleiste des Portals das Symbol **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Suchen Sie im Azure-Portal nach **Azure AD B2C**, und wählen Sie diese Option dann aus.
1. Wählen Sie **Anwendungen (Legacy)** und dann **Hinzufügen** aus.
1. Geben Sie einen Namen für die Anwendung ein. Beispiel: *webapp1*.
1. Wählen Sie für **Web-App/Web-API einschließen** und n**Impliziten Ablauf zulassen** die Option **Ja**.
1. Geben Sie für **Antwort-URLs** einen Endpunkt ein, an den Azure AD B2C von Ihrer App angeforderte Token zurückgibt. Sie können sie so einrichten, dass sie lokal bei `https://localhost:44316` lauschen. Wenn Sie die Portnummer noch nicht kennen, können Sie einen Platzhalterwert eingeben und diesen später ändern.

    Zu Testzwecken wie in diesem Tutorial können Sie `https://jwt.ms` festlegen. Damit zeigen Sie den Inhalt eines Tokens für die Überprüfung an. Legen Sie in diesem Tutorial **Antwort-URL** auf `https://jwt.ms` fest.

    Für Antwort-URLs gelten die folgenden Einschränkungen:

    * Die Antwort-URL muss mit dem Schema `https` beginnen.
    * Bei der Antwort-URL muss die Groß-/Kleinschreibung beachtet werden. Die Groß-/Kleinschreibung muss der Groß-/Kleinschreibung des URL-Pfads Ihrer ausgeführten Anwendung entsprechen. Wenn Ihre Anwendung z. B. als Teil des Pfads `.../abc/response-oidc` enthält, geben Sie in der Antwort-URL nicht `.../ABC/response-oidc` an. Weil der Webbrowser bei Pfaden die Groß-/Kleinschreibung beachtet, werden Cookies, die `.../abc/response-oidc` zugeordnet sind, möglicherweise ausgeschlossen, wenn eine Umleitung an die anders geschriebene (nicht übereinstimmende) URL `.../ABC/response-oidc` erfolgt.

1. Wählen Sie **Erstellen** aus, um die Anwendungsregistrierung abzuschließen.

* * *

## <a name="create-a-client-secret"></a>Erstellen eines Clientgeheimnisses

Wenn Ihre Anwendung einen Autorisierungscode für ein Zugriffstoken austauscht, müssen Sie ein Anwendungsgeheimnis erstellen.


#### <a name="app-registrations"></a>[App-Registrierungen](#tab/app-reg-ga/)

1. Wählen Sie auf der Seite **Azure AD B2C – App-Registrierungen** die von Ihnen erstellte Anwendung aus, etwa *webapp1*.
1. Wählen Sie im linken Menü unter **Verwalten** die Option **Zertifikate und Geheimnisse** aus.
1. Wählen Sie **Neuer geheimer Clientschlüssel**.
1. Geben Sie im Feld **Beschreibung** eine Beschreibung für das Clientgeheimnis ein. Beispielsweise *clientsecret1*.
1. Wählen Sie unter **Läuft ab** einen Zeitraum aus, für den das Geheimnis gültig ist, und wählen Sie dann **Hinzufügen** aus.
1. Notieren Sie den **Wert** des Geheimnisses. Sie verwenden diesen Wert als Anwendungsgeheimnis im Code Ihrer Anwendung.

#### <a name="applications-legacy"></a>[Anwendungen (Legacy)](#tab/applications-legacy/)

1. Wählen Sie auf der Seite **Azure AD B2C – Anwendungen** die von Ihnen erstellte Anwendung aus, z.B. *webapp1*.
1. Wählen Sie **Schlüssel** und dann **Schlüssel generieren** aus.
1. Wählen Sie **Speichern**, um den Schlüssel anzuzeigen. Notieren Sie sich den Wert für **App-Schlüssel**. Sie verwenden diesen Wert als Anwendungsgeheimnis im Code Ihrer Anwendung.

* * *

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Registrieren einer Webanwendung
> * Erstellen eines Clientgeheimnisses

Als Nächstes erfahren Sie, wie Sie Benutzerflows erstellen, damit sich die Benutzer registrieren, anmelden und ihre Profile verwalten können.

> [!div class="nextstepaction"]
> [Erstellen von Benutzerflows in Azure Active Directory B2C >](tutorial-create-user-flows.md)
