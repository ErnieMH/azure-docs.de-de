---
title: Einrichten der Registrierung und Anmeldung mit einem Google-Konto
titleSuffix: Azure AD B2C
description: Bereitstellen von Registrierung und Anmeldung für Kunden mit Google-Konten in Ihren Anwendungen mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 72e4de1473766d50512453ae38b6033ff0c5b73d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85388033"
---
# <a name="set-up-sign-up-and-sign-in-with-a-google-account-using-azure-active-directory-b2c"></a>Einrichten der Registrierung und Anmeldung mit einem Google-Konto mithilfe von Azure Active Directory B2C

## <a name="create-a-google-application"></a>Erstellen einer Google-Anwendung

Um ein Google-Konto als [Identitätsanbieter](authorization-code-flow.md) in Azure Active Directory B2C (Azure AD B2C) verwenden zu können, müssen Sie in Ihrer Google-Entwicklerkonsole eine Anwendung erstellen. Wenn Sie noch über kein Google-Konto verfügen, können Sie sich unter [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp) registrieren.

1. Melden Sie sich bei der [Google Developers Console](https://console.developers.google.com/) mit den Anmeldeinformationen für Ihr Google-Konto an.
1. Wählen Sie in der oberen linken Ecke der Seite die Projektliste aus, und wählen Sie dann **Neues Projekt** aus.
1. Geben Sie einen **Projektnamen** ein, und wählen Sie **Erstellen** aus.
1. Stellen Sie sicher, dass Sie das neue Projekt verwenden, indem Sie oben links auf dem Bildschirm in der Dropdownliste „Projekt“ Ihr Projekt auswählen und dann **Öffnen** auswählen.
1. Wählen Sie im linken Menü die Option **OAuth-Zustimmungsbildschirm** aus, wählen Sie **Extern** aus, und wählen Sie dann **Erstellen** aus.
Geben Sie einen **Namen** für Ihre Anwendung ein. Geben Sie im Abschnitt *Autorisierte Domänen* die Zeichenfolge **b2clogin.com** ein, und wählen Sie **Speichern** aus.
1. Wählen Sie im linken Menü die Option **Credentials** (Anmeldeinformationen) und anschließend **Create credentials** > **Oauth client ID** (Anmeldeinformationen erstellen > OAuth-Client-ID) aus.
1. Wählen Sie unter **Anwendungstyp** die Option **Webanwendung** aus.
1. Geben Sie einen **Namen** für die Anwendung an, und geben Sie `https://your-tenant-name.b2clogin.com` unter **Authorized JavaScript origins** (Autorisierte JavaScript-Quellen) und `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` unter **Authorized redirect URIs** (Autorisierte Umleitungs-URIs) ein. Ersetzen Sie `your-tenant-name` durch den Namen Ihres Mandanten. Bei der Eingabe Ihres Mandantennamens dürfen Sie nur Kleinbuchstaben verwenden, auch wenn der Mandant in Azure AD B2C Großbuchstaben enthält.
1. Klicken Sie auf **Erstellen**.
1. Kopieren Sie die Werte für **Client-ID** und **Clientgeheimnis**. Sie benötigen beide Angaben, um Google als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. Das **Clientgeheimnis** ist eine wichtige Anmeldeinformation.

## <a name="configure-a-google-account-as-an-identity-provider"></a>Konfigurieren eines Google-Kontos als Identitätsanbieter

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als globaler Administrator Ihres Azure AD B2C-Mandanten an.
1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält, indem Sie im oberen Menü auf den **Verzeichnis- und Abonnementfilter** klicken und das entsprechende Verzeichnis auswählen.
1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **Azure AD B2C**, und klicken Sie darauf.
1. Wählen Sie **Identitätsanbieter** und dann **Google** aus.
1. Geben Sie einen **Namen** ein. Beispiel: *Google*.
1. Geben Sie für die **Client-ID** die Client-ID der Google-Anwendung ein, die Sie zuvor erstellt haben.
1. Geben Sie für **Geheimer Clientschlüssel** den zuvor notierten geheimen Clientschlüssel ein.
1. Wählen Sie **Speichern** aus.
