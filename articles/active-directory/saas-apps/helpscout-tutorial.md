---
title: 'Tutorial: Azure Active Directory-Integration mit Help Scout | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Help Scout konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 10/24/2019
ms.author: jeedes
ms.openlocfilehash: 7c5e8210bc8b805d72149fd2ef3335c1d637a58f
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2020
ms.locfileid: "92445221"
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a>Tutorial: Azure Active Directory-Integration mit Help Scout

In diesem Tutorial erfahren Sie, wie Sie Help Scout in Azure Active Directory (Azure AD) integrieren.
Die Integration von Help Scout in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Help Scout hat.
* Sie können es Ihren Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Help Scout anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Help Scout konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein Help Scout-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Help Scout unterstützt **SP- und IDP-initiiertes** einmaliges Anmelden.
* Help Scout unterstützt die **Just-in-Time** -Benutzerbereitstellung.

## <a name="adding-help-scout-from-the-gallery"></a>Hinzufügen von Help Scout aus dem Katalog

Zum Konfigurieren der Integration von Help Scout in Azure AD müssen Sie Help Scout über den Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen** , und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Help Scout** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **Help Scout** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Help Scout mithilfe eines Testbenutzers namens **B.Simon** .
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Help Scout eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Help Scout müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    * **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    * **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für Help Scout](#configure-help-scout-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    * **[Erstellen eines Help Scout-Testbenutzers](#create-help-scout-test-user)** , um eine Entsprechung von B. Simon in Help Scout zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit Help Scout die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Help Scout** die Option **Einmaliges Anmelden** .

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

1. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten** , um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP** -initiierten Modus konfigurieren möchten:

    ![Screenshot: Seite „Grundlegende SAML-Konfiguration“ zum Eingeben des Bezeichners sowie einer Antwort-URL und zum Klicken auf „Speichern“](common/idp-intiated.png)

    a. Der **Bezeichner** ist der **Benutzergruppen-URI (ID der Dienstanbieterentität)** aus Help Scout und beginnt mit `urn:`.

    b. Die **Antwort-URL** ist die **Postback-URL (Assertionsverbraucherdienst-URL)** aus Help Scout und beginnt mit `https://`. 

    > [!NOTE]
    > Die Werte in diesen URLs dienen nur Demonstrationszwecken. Aktualisieren Sie diese Werte mit der tatsächlichen Antwort-URL und dem tatsächlichen Bezeichner. Diese Werte finden Sie auf der Registerkarte **Einmaliges Anmelden** im Abschnitt „Authentifizierung“ (wird im weiteren Verlauf des Tutorials noch näher erläutert).

1. Klicken Sie auf **Zusätzliche URLs festlegen** , und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![Screenshot: Option „Zusätzliche URLs festlegen“ zum Eingeben einer Anmelde-URL](common/metadata-upload-additional-signon.png)

    Geben Sie im Textfeld **Anmelde-URL** die URL folgendermaßen ein: `https://secure.helpscout.net/members/login/`.

1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen** , um das Ihrer Anforderung entsprechende **Zertifikat (Base64)** aus den angegebenen Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/certificatebase64.png)

1. Kopieren Sie im Abschnitt **Help Scout einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt wird ein Testbenutzer namens B. Simon im Azure-Portal erstellt.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory** , **Benutzer** und dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](common/users.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Schaltfläche „Neuer Benutzer“](common/new-user.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Dialogfeld „Benutzer“](common/user-properties.png)

    a. Geben Sie **B.Simon** in das Feld **Name** ein.
  
    b. Geben Sie im Feld **Benutzername** Folgendes ein: **B.Simon\@ihreunternehmensdomäne.erweiterung** .  
    Zum Beispiel, B.Simon@contoso.com

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen** , und notieren Sie sich den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen** .

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Help Scout gewähren.

1. Wählen Sie im Azure-Portal nacheinander die Optionen **Unternehmensanwendungen** , **Alle Anwendungen** und **Help Scout** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Wählen Sie in der Anwendungsliste **Help Scout** aus.

    ![Help Scout-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen** , und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen** .

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen** .

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen** .

## <a name="configure-help-scout-sso"></a>Konfigurieren des einmaligen Anmeldens für Help Scout

1. Wenn Sie die Konfiguration in Help Scout automatisieren möchten, müssen Sie die **Browsererweiterung „Meine Apps“ für die sichere Anmeldung** installieren, indem Sie auf **Erweiterung installieren** klicken.

    ![Erweiterung „Meine Apps“](common/install-myappssecure-extension.png)

1. Klicken Sie nach dem Hinzufügen der Erweiterung zum Browser auf **Help Scout einrichten** , um zur Anwendung Help Scout weitergeleitet zu werden. Geben Sie dort die Administratoranmeldeinformationen ein, um sich bei Help Scout anzumelden. Die Browsererweiterung konfiguriert die Anwendung automatisch für Sie und automatisiert die Schritte 3 bis 7.

    ![Einrichtungskonfiguration](common/setup-sso.png)

1. Wenn Sie Help Scout manuell einrichten möchten, melden Sie sich in einem neuen Webbrowserfenster bei der Help Scout-Unternehmenswebsite als Administrator an, und führen Sie die folgenden Schritte aus:

1. Klicken Sie im oberen Menü auf **Verwalten** , und wählen Sie anschließend im Dropdownmenü die Option **Unternehmen** aus.

    ![Screenshot: Menü „Verwalten“ mit ausgewählter Option „Unternehmen“](./media/helpscout-tutorial/settings1.png)

1. Wählen Sie im linken Navigationsbereich **Authentication** (Authentifizierung) aus.

    ![Screenshot: Navigationsbereich „Authentication“ (Authentifizierung) ausgewählt](./media/helpscout-tutorial/settings2.png)

1. Dadurch gelangen Sie zum Abschnitt mit den SAML-Einstellungen. Führen Sie dort die folgenden Schritte aus:

    ![Screenshot: Registerkarte „Einmaliges Anmelden“, auf der Sie die angegebenen Informationen eingeben](./media/helpscout-tutorial/settings3.png)

    a. Kopieren Sie den Wert für **Post-back URL (Assertion Consumer Service URL)** (Postback-URL (Assertionsverbraucherdienst-URL)), und fügen Sie ihn im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** in das Feld **Antwort-URL** ein.

    b. Kopieren Sie den Wert für **Audience URI (Service Provider Entity ID)** (Benutzergruppen-URI (ID der Dienstanbieterentität)), und fügen Sie ihn im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** in das Feld **Bezeichner** ein.

1. Aktivieren Sie **SAML aktivieren** , und führen Sie die folgenden Schritte aus:

    ![Screenshot: Registerkarte „Einmaliges Anmelden“, auf der Sie SAML aktivieren und andere Informationen hinzufügen](./media/helpscout-tutorial/settings4.png)

    a. Fügen Sie in das Textfeld **Einfacher Anmeldungs-URL** den Wert der **Anmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    b. Klicken Sie auf **Zertifikat hochladen** , um das aus dem Azure-Portal heruntergeladene **Zertifikat (Base64)** hochzuladen.

    c. Geben Sie die E-Mail-Domäne(n) Ihrer Organisation (Beispiel: `contoso.com`) in das Textfeld **Email Domains** (E-Mail-Domänen) ein. Mehrere Domänen können jeweils durch ein Komma getrennt werden. Help Scout-Benutzer oder -Administratoren, die diese spezielle Domäne auf der [Anmeldeseite von Help Scout](https://secure.helpscout.net/members/login/) eingeben, werden zur Authentifizierung mit ihren Anmeldeinformationen an den Identitätsanbieter weitergeleitet.

    d. Abschließend können Sie noch **Force SAML Sign-on** (SAML-Anmeldung erzwingen) aktivieren, wenn sich Benutzer ausschließlich mit dieser Methode bei Help Scout anmelden sollen. Wenn die Benutzer weiterhin die Möglichkeit haben sollen, sich mit ihren Help Scout-Anmeldeinformationen anzumelden, lassen Sie die Einstellung deaktiviert. Auch bei aktivierter Einstellung kann sich ein Kontobesitzer jederzeit mit seinem Kontokennwort bei Help Scout anmelden.

    e. Klicken Sie auf **Speichern** .

### <a name="create-help-scout-test-user"></a>Erstellen eines Help Scout-Testbenutzers.

In diesem Abschnitt wird in Help Scout ein Benutzer namens B. Simon erstellt. Help Scout unterstützt die Just-in-Time-Benutzerbereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist ein Benutzer noch nicht in Help Scout vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt.

### <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Help Scout“ klicken, sollten Sie automatisch bei der Help Scout-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](./tutorial-list.md)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Was ist bedingter Zugriff?](../conditional-access/overview.md)

- [Help Scout mit Azure AD ausprobieren](https://aad.portal.azure.com/)