---
title: Einrichten der Anmeldung für eine Azure AD-Organisation
titleSuffix: Azure AD B2C
description: Einrichten der Anmeldung für eine bestimmte Azure Active Directory-Organisation in Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/07/2020
ms.author: mimart
ms.subservice: B2C
ms.custom: fasttrack-edit, project-no-code
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 05c4d36f266fb526a1d0232cc32f0408e4322c80
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97654386"
---
# <a name="set-up-sign-in-for-a-specific-azure-active-directory-organization-in-azure-active-directory-b2c"></a>Einrichten der Anmeldung für eine bestimmte Azure Active Directory-Organisation in Azure Active Directory B2C

In diesem Artikel erfahren Sie, wie Sie die Anmeldung für Benutzer einer bestimmten Azure AD-Organisation über einen Benutzerflow in Azure AD B2C aktivieren.

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

::: zone pivot="b2c-custom-policy"

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

::: zone-end

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

## <a name="register-an-azure-ad-app"></a>Registrieren einer Azure AD-App

Um die Anmeldung für Benutzer von einer bestimmten Azure AD-Organisation zu aktivieren, müssen Sie eine Anwendung im Azure AD-Mandanten der Organisation registrieren.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD-Organisationsmandanten (z. B. „contoso.com“) enthält. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD-Mandanten enthält.
1. Klicken Sie links oben im Azure-Portal auf **Alle Dienste**, suchen Sie nach **App-Registrierungen**, und wählen Sie dann diese Option aus.
1. Wählen Sie **Neue Registrierung** aus.
1. Geben Sie einen **Namen** für Ihre Anwendung ein. Beispiel: `Azure AD B2C App`.
1. Übernehmen Sie für diese Anwendung die Standardauswahl von **Nur Konten in diesem Organisationsverzeichnis**.
1. Übernehmen Sie den Wert **Web** als **Umleitungs-URI**, geben Sie die folgende URL in Kleinbuchstaben ein, und ersetzen Sie dabei `your-B2C-tenant-name` durch den Namen Ihres Azure AD B2C-Mandanten.

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Beispiel: `https://fabrikam.b2clogin.com/fabrikam.onmicrosoft.com/oauth2/authresp`.

1. Wählen Sie **Registrieren**. Notieren Sie sich die **Anwendungs-ID (Client)** zur Verwendung in einem späteren Schritt.
1. Wählen Sie **Zertifikate & Geheimnisse** und dann **Neuer geheimer Clientschlüssel** aus.
1. Geben Sie eine **Beschreibung** für das Geheimnis ein, wählen Sie ein Ablaufdatum aus, und wählen Sie dann **Hinzufügen** aus. Notieren Sie sich den **Wert** des Geheimnisses zur Verwendung in einem späteren Schritt.

### <a name="configuring-optional-claims"></a>Konfigurieren optionaler Ansprüche

Wenn Sie die Ansprüche `family_name` und `given_name` von Azure AD erhalten möchten, können Sie optionale Ansprüche für Ihre Anwendung im Azure-Portal oder im Anwendungsmanifest konfigurieren. Weitere Informationen finden Sie unter [Bereitstellen optionaler Ansprüche für Ihre Azure AD-App](../active-directory/develop/active-directory-optional-claims.md).

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Suchen Sie nach **Azure Active Directory**, und wählen Sie diese Option aus.
1. Wählen Sie im Abschnitt **Verwalten** die Option **App-Registrierungen** aus.
1. Wählen Sie in der Liste die Anwendung aus, für die Sie optionale Ansprüche konfigurieren möchten.
1. Wählen Sie im Abschnitt **Verwalten** die Option **Tokenkonfiguration** aus.
1. Wählen Sie **Optionalen Anspruch hinzufügen** aus.
1. Wählen Sie als **Tokentyp** die Option **ID** aus.
1. Wählen Sie die hinzuzufügenden optionalen Ansprüche (`family_name` und `given_name`) aus.
1. Klicken Sie auf **Hinzufügen**.

::: zone pivot="b2c-user-flow"

## <a name="configure-azure-ad-as-an-identity-provider"></a>Konfigurieren von Azure AD als Identitätsanbieter

1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das den Azure AD B2C-Mandanten enthält. Wählen Sie im Hauptmenü den **Verzeichnis- und Abonnementfilter** aus, und wählen Sie das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
1. Wählen Sie **Identitätsanbieter** und dann **Neuer OpenID Connect-Anbieter** aus.
1. Geben Sie einen **Namen** ein. Geben Sie beispielsweise *Contoso Azure AD* ein.
1. Geben Sie für **Metadaten-URL** die folgende URL ein, und ersetzen Sie dabei `{tenant}` durch den Domänennamen Ihres Azure AD-Mandanten.

    ```
    https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration
    ```

    Beispiel: `https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0/.well-known/openid-configuration`.
    Beispiel: `https://login.microsoftonline.com/contoso.com/v2.0/.well-known/openid-configuration`.

1. Geben Sie für **Client-ID** die zuvor notierte Anwendungs-ID ein.
1. Geben Sie im Feld **Geheimer Clientschlüssel** den zuvor notierten geheimen Clientschlüssel ein.
1. Geben Sie für den **Bereich** das `openid profile` ein.
1. Übernehmen Sie die Standardwerte für **Antworttyp** und **Antwortmodus**.
1. (Optional) Geben Sie als **Domänenhinweis** die Zeichenfolge `contoso.com` ein. Weitere Informationen finden Sie unter [Einrichten einer direkten Anmeldung mit Azure Active Directory B2C](direct-signin.md#redirect-sign-in-to-a-social-provider).
1. Wählen Sie unter **Zuordnung von Identitätsanbieteransprüchen** die folgenden Ansprüche aus:

    - **Benutzer-ID**: *oid*
    - **Anzeigename**: *name*
    - **Vorname**: *given_name*
    - **Nachname**: *family_name*
    - **E-Mail**: *preferred_username*

1. Wählen Sie **Speichern** aus.

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="create-a-policy-key"></a>Erstellen eines Richtlinienschlüssels

Sie müssen den von Ihnen erstellten Anwendungsschlüssel in Ihrem Azure AD B2C-Mandanten speichern.

1. Stellen Sie sicher, dass Sie das Verzeichnis verwenden, das Ihren Azure AD B2C-Mandanten enthält. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Wählen Sie links oben im Azure-Portal die Option **Alle Dienste** aus, suchen Sie nach **Azure AD B2C**, und wählen Sie dann diese Option aus.
1. Wählen Sie unter **Richtlinien** die Option **Identity Experience Framework** aus.
1. Wählen Sie **Richtlinienschlüssel** und dann **Hinzufügen** aus.
1. Klicken Sie unter **Optionen** auf `Manual`.
1. Geben Sie einen **Namen** für den Richtlinienschlüssel ein. Beispiel: `ContosoAppSecret`.  Das Präfix `B2C_1A_` wird dem Namen Ihres Schlüssels bei der Erstellung automatisch hinzugefügt. Im XML-Code im folgenden Abschnitt lautet der entsprechende Verweis darauf daher *B2C_1A_ContosoAppSecret*.
1. Geben Sie im Feld **Geheimnis** den zuvor notierten geheimen Clientschlüssel ein.
1. Wählen Sie für **Schlüsselverwendung** die Option `Signature` aus.
1. Klicken Sie auf **Erstellen**.

## <a name="add-a-claims-provider"></a>Hinzufügen eines Anspruchsanbieters

Wenn Sie möchten, dass sich Benutzer mithilfe von Azure AD anmelden, müssen Sie Azure AD als Anspruchsanbieter definieren, mit dem Azure AD B2C über einen Endpunkt kommunizieren kann. Der Endpunkt bietet eine Reihe von Ansprüchen, mit denen Azure AD B2C überprüft, ob ein bestimmter Benutzer authentifiziert wurde.

Sie können Azure AD als Anspruchsanbieter definieren, indem Sie Azure AD in der Erweiterungsdatei Ihrer Richtlinie dem Element **ClaimsProvider** hinzufügen.

1. Öffnen Sie die Datei *TrustFrameworkExtensions.xml*.
2. Suchen Sie nach dem Element **ClaimsProviders**. Falls das Element nicht vorhanden sein sollte, fügen Sie es unter dem Stammelement hinzu.
3. Fügen Sie ein neues **ClaimsProvider**-Element wie folgt hinzu:
    ```xml
    <ClaimsProvider>
      <Domain>Contoso</Domain>
      <DisplayName>Login using Contoso</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="OIDC-Contoso">
          <DisplayName>Contoso Employee</DisplayName>
          <Description>Login with your Contoso account</Description>
          <Protocol Name="OpenIdConnect"/>
          <Metadata>
            <Item Key="METADATA">https://login.microsoftonline.com/tenant-name.onmicrosoft.com/v2.0/.well-known/openid-configuration</Item>
            <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
            <Item Key="response_types">code</Item>
            <Item Key="scope">openid profile</Item>
            <Item Key="response_mode">form_post</Item>
            <Item Key="HttpBinding">POST</Item>
            <Item Key="UsePolicyInRedirectUri">false</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_ContosoAppSecret"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="oid"/>
            <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid"/>
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
            <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" AlwaysUseDefaultValue="true" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" PartnerClaimType="iss" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin"/>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

4. Ändern Sie unter dem Element **ClaimsProvider** den Wert für **Domain** in einen eindeutigen Wert, der eine Unterscheidung von anderen Identitätsanbietern ermöglicht. Beispiel: `Contoso`. Am Ende dieser Domäneneinstellung wird nicht `.com` angefügt.
5. Aktualisieren Sie unter dem Element **ClaimsProvider** den Wert für **DisplayName** in einen Anzeigenamen für den Anspruchsanbieter. Dieser Wert wird derzeit nicht verwendet.

### <a name="update-the-technical-profile"></a>Aktualisieren des technischen Profils

Um ein Token vom Azure AD-Endpunkt zu erhalten, müssen Sie die Protokolle definieren, die Azure AD B2C zur Kommunikation mit Azure AD verwenden soll. Dies erfolgt im **TechnicalProfile**-Element von **ClaimsProvider**.

1. Aktualisieren Sie die ID des **TechnicalProfile**-Elements. Diese ID wird als Verweis auf dieses technische Profil aus anderen Teilen der Richtlinie verwendet, z. B. `OIDC-Contoso`.
1. Aktualisieren Sie den Wert für **DisplayName**. Dieser Wert wird auf dem Anmeldebildschirm auf der Anmeldeschaltfläche angezeigt.
1. Aktualisieren Sie den Wert von **Beschreibung**.
1. Azure AD verwendet das OpenID Connect-Protokoll. Stellen Sie daher sicher, dass der Wert für **Protokoll**`OpenIdConnect` lautet.
1. Legen Sie den Wert von **METADATA** auf `https://login.microsoftonline.com/tenant-name.onmicrosoft.com/v2.0/.well-known/openid-configuration` fest, wobei `tenant-name` der Name Ihres Azure AD-Mandanten ist. Zum Beispiel, `https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0/.well-known/openid-configuration`
1. Legen Sie **client_id** auf die Anwendungs-ID aus der Anwendungsregistrierung fest.
1. Aktualisieren Sie unter **CryptographicKeys** den Wert von **StorageReferenceId** auf den Namen des zuvor erstellten Richtlinienschlüssels. Beispiel: `B2C_1A_ContosoAppSecret`.

### <a name="upload-the-extension-file-for-verification"></a>Hochladen der Erweiterungsdatei zur Überprüfung

Nun haben Sie Ihre Richtlinie so konfiguriert, dass Azure AD B2C mit Ihrem Azure AD-Verzeichnis kommunizieren kann. Versuchen Sie, die Erweiterungsdatei Ihrer Richtlinie hochzuladen, um sich zu vergewissern, dass soweit keine Probleme vorliegen.

1. Wählen Sie in Ihrem Azure AD B2C-Mandanten auf der Seite **Benutzerdefinierte Richtlinien** die Option **Richtlinie hochladen** aus.
1. Aktivieren Sie **Richtlinie überschreiben, sofern vorhanden**, navigieren Sie dann zur Datei *TrustFrameworkExtensions.xml*, und wählen Sie die Datei aus.
1. Klicken Sie auf **Hochladen**.

## <a name="register-the-claims-provider"></a>Registrieren des Anspruchsanbieters

Der Identitätsanbieter wurde nun eingerichtet, aber er ist noch auf keiner der Registrierungs-/Anmeldeseiten verfügbar. Um ihn verfügbar zu machen, erstellen Sie ein Duplikat einer vorhandenen User Journey-Vorlage und ändern diese dann so, dass sie auch den Azure AD-Identitätsanbieter aufweist:

1. Öffnen Sie die Datei *TrustFrameworkBase.xml* aus dem Starter Pack.
1. Suchen und kopieren Sie den gesamten Inhalt des **UserJourney**-Elements, das `Id="SignUpOrSignIn"` enthält.
1. Öffnen Sie die Datei *TrustFrameworkExtensions.xml*, und suchen Sie nach dem **UserJourneys**-Element. Wenn das Element nicht vorhanden ist, fügen Sie ein solches hinzu.
1. Fügen Sie den gesamten Inhalt des kopierten **UserJourney**-Element als untergeordnetes Element des **UserJourneys**-Elements ein.
1. Benennen Sie die ID der User Journey um. Beispiel: `SignUpSignInContoso`.

### <a name="display-the-button"></a>Anzeigen der Schaltfläche

Das **ClaimsProviderSelection**-Element entspricht einer Schaltfläche für einen Identitätsanbieter auf einer Registrierungs-/Anmeldeseite. Wenn Sie ein **ClaimsProviderSelection**-Element für Azure AD hinzufügen, wird eine neue Schaltfläche angezeigt, wenn ein Benutzer zu der Seite gelangt.

1. Suchen Sie nach dem **OrchestrationStep**-Element, das `Order="1"` in der User Journey enthält, die Sie in der Datei *TrustFrameworkExtensions.xml* erstellt haben.
1. Fügen Sie unter **ClaimsProviderSelections** das folgende Element hinzu. Legen Sie den Wert von **TargetClaimsExchangeId** auf einen geeigneten Wert (z.B. auf `ContosoExchange`) fest:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Verknüpfen der Schaltfläche mit einer Aktion

Nachdem Sie eine Schaltfläche implementiert haben, müssen Sie sie mit einer Aktion verknüpfen. In diesem Fall soll bei der Aktion Azure AD B2C mit Azure AD kommunizieren, um ein Token zu empfangen. Dies gelingt Ihnen, indem Sie das technische Profil für Ihren Azure AD-Anspruchsanbieter verknüpfen:

1. Suchen Sie nach dem **OrchestrationStep**-Element, das `Order="2"` in der User Journey enthält.
1. Fügen Sie das folgende **ClaimsExchange**-Element hinzu, um sicherzustellen, dass Sie für **Id** den gleichen Wert verwenden, den Sie für **TargetClaimsExchangeId** verwendet haben:

    ```xml
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="OIDC-Contoso" />
    ```

    Ändern Sie den Wert von **TechnicalProfileReferenceId** in die **Id** des technischen Profils, das Sie zuvor erstellt haben. Beispiel: `OIDC-Contoso`.

1. Speichern Sie die Datei *TrustFrameworkExtensions.xml*, und laden Sie die Datei zur Überprüfung erneut hoch.

::: zone-end

::: zone pivot="b2c-user-flow"

## <a name="add-azure-ad-identity-provider-to-a-user-flow"></a>Hinzufügen von Azure AD als Identitätsanbieter zu einem Benutzerflow 

1. Wählen Sie in Ihrem Azure AD B2C-Mandanten die Option **Benutzerflows** aus.
1. Klicken Sie auf den Benutzerflow, dem Sie Azure AD als Identitätsanbieter hinzufügen möchten.
1. Wählen Sie unter **Soziales Netzwerk als Identitätsanbieter** die Option **Contoso Azure AD** aus.
1. Wählen Sie **Speichern** aus.
1. Um die Richtlinie zu testen, wählen Sie **Benutzerflow ausführen** aus.
1. Wählen Sie für **Anwendung** die Webanwendung *testapp1* aus, die Sie zuvor registriert haben. Als **Antwort-URL** sollte `https://jwt.ms` angezeigt werden.
1. Klicken Sie auf **Benutzerflow ausführen**.

::: zone-end

::: zone pivot="b2c-custom-policy"


## <a name="update-and-test-the-relying-party-file"></a>Aktualisieren und Testen der Datei der vertrauenden Seite

Aktualisieren Sie als Nächstes die Datei der vertrauenden Seite, mit der die erstellte User Journey initiiert wird.

1. Erstellen Sie in Ihrem Arbeitsverzeichnis eine Kopie der Datei *SignUpOrSignIn.xml*, und benennen Sie sie um. Benennen Sie die Datei z. B. in *SignUpSignInContoso.xml* um.
1. Öffnen Sie die neue Datei, und aktualisieren Sie den Wert des Attributs **PolicyId** für **TrustFrameworkPolicy** mit einem eindeutigen Wert. Beispiel: `SignUpSignInContoso`.
1. Aktualisieren Sie den Wert von **PublicPolicyUri** mit dem URI für die Richtlinie. Beispiel: `http://contoso.com/B2C_1A_signup_signin_contoso`.
1. Aktualisieren Sie den Wert des Attributs **ReferenceId** im Element **DefaultUserJourney**,damit dieser der Kennung der User Journey entspricht, die Sie zuvor erstellt haben. Beispielsweise *SignUpSignInContoso*.
1. Speichern Sie die Änderungen, und laden Sie die Datei hoch.
1. Wählen Sie unter **Benutzerdefinierte Richtlinien** die neue Richtlinie in der Liste aus.
1. Wählen Sie in der Dropdownliste **Anwendung auswählen** die Azure AD B2C-Anwendung aus, die Sie zuvor erstellt haben. Zum Beispiel *testapp1*.
1. Kopieren Sie den **Endpunkt für sofortige Ausführung**, und öffnen Sie ihn in einem privaten Browserfenster, z. B. in Google Chrome im Inkognitomodus oder in Microsoft Edge in einem InPrivate-Fenster. Durch das Öffnen in einem privaten Browserfenster können Sie die vollständige User Journey testen, indem Sie keine aktuell zwischengespeicherten Azure AD-Anmeldeinformationen verwenden.
1. Wählen Sie die Schaltfläche für die Azure AD-Anmeldung (z. B. *Contoso-Mitarbeiter*) aus, und geben Sie dann die Anmeldeinformationen für einen Benutzer in Ihrem Azure AD-Organisationsmandanten ein. Sie werden aufgefordert, die Anwendung zu autorisieren und dann Informationen für Ihr Profil einzugeben.

Wenn der Anmeldevorgang erfolgreich verlaufen ist, wird der Browser an `https://jwt.ms` umgeleitet, wodurch der Inhalt des von Azure AD B2C zurückgegebenen Tokens angezeigt wird.

## <a name="next-steps"></a>Nächste Schritte

Beim Arbeiten mit benutzerdefinierten Richtlinien benötigen Sie möglicherweise manchmal zusätzliche Informationen, wenn Sie Probleme mit einer Richtlinie während der Entwicklung beheben.

Um das Diagnostizieren von Problemen zu erleichtern, können Sie die Richtlinie vorübergehend in den „Entwicklermodus“ versetzen und Protokolle mit Azure Application Insights erfassen. Informationen hierzu finden Sie unter [Azure Active Directory B2C: mithilfe von Application Insights](troubleshoot-with-application-insights.md).

::: zone-end