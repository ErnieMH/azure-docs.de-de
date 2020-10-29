---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Claromentis | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Claromentis konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/04/2019
ms.author: jeedes
ms.openlocfilehash: 92b068ee9b8aaf4c462002354bbb6490f4888a80
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2020
ms.locfileid: "92455900"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-claromentis"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Claromentis

In diesem Tutorial erfahren Sie, wie Sie Claromentis in Azure Active Directory (Azure AD) integrieren. Die Integration von Claromentis in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf Claromentis hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Claromentis anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein Claromentis-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Claromentis unterstützt **SP- und IDP-initiiertes** einmaliges Anmelden.
* Claromentis unterstützt die **Just-In-Time** -Benutzerbereitstellung.

## <a name="adding-claromentis-from-the-gallery"></a>Hinzufügen von Claromentis aus dem Katalog

Zum Konfigurieren der Integration von Claromentis in Azure AD müssen Sie Claromentis aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen** , und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Claromentis** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **Claromentis** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on-for-claromentis"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Claromentis

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Claromentis mithilfe eines Testbenutzers mit dem Namen **B. Simon** . Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Claromentis eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Claromentis die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    * **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    * **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für Claromentis](#configure-claromentis-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    * **[Erstellen eines Claromentis-Testbenutzers](#create-claromentis-test-user)** , um eine Entsprechung von B. Simon in Claromentis zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Claromentis** zum Abschnitt **Verwalten** , und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration** , um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Geben Sie im Abschnitt **Grundlegende SAML-Konfiguration** die Werte in die folgenden Felder ein, wenn Sie die Anwendung im **IDP** -initiierten Modus konfigurieren möchten:

    a. Geben Sie im Textfeld **Bezeichner** den Bezeichnerwert entsprechend den Anforderungen Ihrer Organisation ein.

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://<customer_site_url>/custom/loginhandler/simplesaml/www/module.php/saml/sp/saml2-acs.php/claromentis`

1. Klicken Sie auf **Zusätzliche URLs festlegen** , und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein:

    ```https
    https://<customer_site_url>/login
    https://<customer_site_url>/login?no_auto=0
    ```

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächlichen Werte für Bezeichner, Antwort-URL und Anmelde-URL. Darauf wird später im Tutorial eingegangen.

1. Navigieren Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** zu **Verbundmetadaten-XML** , und wählen Sie **Herunterladen** aus, um das Zertifikat herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

1. Kopieren Sie im Abschnitt **Claromentis einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer mit dem Namen B. Simon.

1. Wählen Sie im linken Bereich des Microsoft Azure-Portals **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
1. Führen Sie unter den Eigenschaften für **Benutzer** die folgenden Schritte aus:
   1. Geben Sie im Feld **Name** die Zeichenfolge `B.Simon` ein.  
   1. Geben Sie im Feld **Benutzername** die Zeichenfolge username@companydomain.extension ein. Beispiel: `B.Simon@contoso.com`.
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen** , und notieren Sie sich den Wert aus dem Feld **Kennwort** .
   1. Klicken Sie auf **Erstellen** .

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Claromentis gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen**  > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste die Option **Claromentis** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten** , und wählen Sie **Benutzer und Gruppen** aus.

   ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer hinzufügen“](common/add-assign-user.png)

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen** .
1. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** die entsprechende Rolle für den Benutzer in der Liste aus, und klicken Sie dann im unteren Bildschirmbereich auf die Schaltfläche **Auswählen** .
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen** .

## <a name="configure-claromentis-sso"></a>Konfigurieren des einmaligen Anmeldens für Claromentis

1. Melden Sie sich in einem anderen Webbrowserfenster bei der Claromentis-Website als Administrator an.

1. Klicken Sie auf das **Anwendungssymbol** , und wählen Sie **Admin** aus.

    ![Screenshot der „Claromentis“-Website mit ausgewähltem „Admin“-Symbol](./media/claromentis-tutorial/config1.png)

1. Wählen Sie die Registerkarte **Custom Login Handler** (Benutzerdefinierter Anmeldehandler) aus.

    ![Screenshot der Seite „Administration“ mit ausgewählter Option „Custom Login Handler“](./media/claromentis-tutorial/config2.png)

1. Wählen Sie **SAML Config** (SAML-Konfiguration) aus.

    ![Screenshot der Konfigurationsseite für SAML](./media/claromentis-tutorial/config3.png)

1. Scrollen Sie auf der Registerkarte **SAML Config** (SAML-Konfiguration) zum Abschnitt **Config** (Konfiguration), und führen Sie die folgenden Schritte aus:

    ![Screenshot des „Config“-Abschnitts der Seite, auf der Sie die in diesem Schritt beschriebenen Informationen eingeben können](./media/claromentis-tutorial/config4.png)

    a. Geben Sie in das Textfeld **Technical Contact Name** (Name des technischen Ansprechpartners) den Namen des technischen Ansprechpartners ein.

    b. Geben Sie in das Textfeld **Technical Contact Email** (E-Mail-Adresse des technischen Ansprechpartners) die E-Mail-Adresse des technischen Ansprechpartners ein.

    c. Geben Sie in das Textfeld **Auth Admin Password** (Kennwort des Authentifizierungsadministrators) das Kennwort ein.

1. Scrollen Sie nach unten bis zu **Auth Sources** (Authentifizierungsquellen), und führen Sie die folgenden Schritte aus:

    ![Screenshot des Abschnitts „Auth Sources“, in dem Sie die in diesem Schritt beschriebenen Informationen eingeben können](./media/claromentis-tutorial/config5.png)

    a. Fügen Sie in das Textfeld **IDP** den Wert für den **Azure AD-Bezeichner** ein, den Sie aus dem Azure-Portal kopiert haben.

    b. Geben Sie den Wert der Entitäts-ID in das Textfeld **Entity ID** (Entitäts-ID) ein.

    c. Laden Sie die **Verbundmetadaten-XML-Datei** hoch, die Sie aus dem Azure-Portal heruntergeladen haben.

    d. Klicken Sie auf **Speichern** .

1. Sie werden nun feststellen, dass im Abschnitt **Identity Provider** (Identitätsanbieter) im Abschnitt **SAML Config** (SAML-Konfiguration) alle URLs aufgefüllt wurden.

    ![Screenshot der Seite „Identity Provider“, auf der URLs anzugeben sind](./media/claromentis-tutorial/config6.png)

    a. Kopieren Sie den Wert im Feld **Identifier (Entity ID)** (Bezeichner (Entitäts-ID)), und fügen Sie ihn im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** in das Textfeld **Bezeichner** ein.

    b. Kopieren Sie den Wert im Feld **Reply URL** (Antwort-URL), und fügen Sie ihn im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** in das Textfeld **Antwort-URL** ein.

    c. Kopieren Sie den Wert im Feld **Sign On URL** (Anmelde-URL), und fügen Sie ihn im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** in das Textfeld **Anmelde-URL** ein.

### <a name="create-claromentis-test-user"></a>Erstellen eines Claromentis-Testbenutzers

In diesem Abschnitt wird in Claromentis ein Benutzer namens B. Simon erstellt. Claromentis unterstützt die Just-In-Time-Benutzerbereitstellung (standardmäßig aktiviert). Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist ein Benutzer noch nicht in Claromentis vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Claromentis“ klicken, sollten Sie automatisch bei der Claromentis-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](./tutorial-list.md)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Was ist der bedingte Zugriff in Azure Active Directory?](../conditional-access/overview.md)

- [Claromentis mit Azure AD ausprobieren](https://aad.portal.azure.com/)