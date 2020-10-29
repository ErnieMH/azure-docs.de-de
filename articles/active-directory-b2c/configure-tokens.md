---
title: Konfigurieren von Token – Azure Active Directory B2C | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Lebensdauer der Token und die Kompatibilitätseinstellungen in Azure Active Directory B2C konfigurieren.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 10/15/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 67bc9d6b35d4841999721a00592a6bbe23bff10f
ms.sourcegitcommit: f88074c00f13bcb52eaa5416c61adc1259826ce7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2020
ms.locfileid: "92340222"
---
# <a name="configure-tokens-in-azure-active-directory-b2c"></a>Konfigurieren von Token in Azure Active Directory B2C

In diesem Artikel erfahren Sie, wie Sie die [Lebensdauer und Kompatibilität eines Tokens](tokens-overview.md) in Azure Active Directory B2C (Azure AD B2C) konfigurieren.

## <a name="prerequisites"></a>Voraussetzungen

[Erstellen Sie einen Benutzerflow](tutorial-create-user-flows.md), damit sich Benutzer bei Ihrer Anwendung registrieren und anmelden können.

## <a name="configure-jwt-token-lifetime"></a>Konfigurieren der Lebensdauer von JWT-Token

Sie können die Lebensdauer des Tokens für den beliebigen Benutzerflow konfigurieren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält. Wählen Sie im Hauptmenü den **Verzeichnis- und Abonnementfilter** aus, und wählen Sie das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
3. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C** , und wählen Sie dann diese Option aus.
4. Wählen Sie **Benutzerflows (Richtlinien)** aus.
5. Öffnen Sie den Benutzerflow, den Sie zuvor erstellt haben.
6. Wählen Sie **Eigenschaften** aus.
7. Passen Sie unter **Tokenlebensdauer** die folgenden Eigenschaften an die Anforderungen Ihrer Anwendung an:

    ![Eigenschafteneinstellungen für die Tokengültigkeitsdauer im Azure-Portal](./media/configure-tokens/token-lifetime.png)

8. Klicken Sie auf **Speichern** .

> [!NOTE]
> Bei Single-Page-Webanwendungen, die den Autorisierungscodeflow mit PKCE verwenden, hat das Aktualisierungstoken immer eine Lebensdauer von 24 Stunden. [Weitere Informationen zu Sicherheitsaspekten von Aktualisierungstoken im Browser](../active-directory/develop/reference-third-party-cookies-spas.md#security-implications-of-refresh-tokens-in-the-browser)

## <a name="configure-jwt-token-compatibility"></a>Konfigurieren der Kompatibilität von JWT-Token

1. Wählen Sie **Benutzerflows (Richtlinien)** aus.
2. Öffnen Sie den Benutzerflow, den Sie zuvor erstellt haben.
3. Wählen Sie **Eigenschaften** aus.
4. Passen Sie unter **Tokenkompatibilitätseinstellungen** die folgenden Eigenschaften an die Anforderungen Ihrer Anwendung an:

    ![Eigenschafteneinstellungen für die Tokenkompatibilität im Azure-Portal](./media/configure-tokens/token-compatibility.png)

5. Klicken Sie auf **Speichern** .

## <a name="provide-optional-claims-to-your-app"></a>Bereitstellen optionaler Ansprüche für Ihre App

Die Anwendungsansprüche sind Werte, die an die Anwendung zurückgegeben werden. Aktualisieren Sie den Benutzerflow, sodass dieser die gewünschten Ansprüche enthält.

1. Wählen Sie **Benutzerflows (Richtlinien)** aus.
1. Öffnen Sie den Benutzerflow, den Sie zuvor erstellt haben.
1. Wählen Sie **Anwendungsansprüche** aus.
1. Wählen Sie die Ansprüche und Attribute aus, die an die Anwendung zurückgesendet werden sollen.
1. Klicken Sie auf **Speichern** .


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum [Anfordern von Zugriffstoken](access-tokens.md).



