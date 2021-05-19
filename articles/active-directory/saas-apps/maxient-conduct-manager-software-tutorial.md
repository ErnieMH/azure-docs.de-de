---
title: 'Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Maxient Conduct Manager Software | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Maxient Conduct Manager Software konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/18/2019
ms.author: jeedes
ms.openlocfilehash: 2790ae787831fb5dfa81656d32473c98ac4bb4bd
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108739941"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-maxient-conduct-manager-software"></a>Tutorial: Integration des einmaligen Anmeldens (Single Sign-On, SSO) von Azure Active Directory mit Maxient Conduct Manager Software

In diesem Tutorial erfahren Sie, wie Sie Maxient Conduct Manager Software in Azure Active Directory (Azure AD) integrieren. Die Integration von Maxient Conduct Manager Software in Azure AD ermöglicht Folgendes:

* Verwenden von Azure AD, um Ihre Benutzer für die Maxient Conduct Manager-Software zu authentifizieren.
* Sie können es Ihren Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Maxient Conduct Manager Software anzumelden.


Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* SSO-fähiges Maxient Conduct Manager Software-Abonnement

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren Sie Ihr Azure AD für die Verwendung mit der Maxient Conduct Manager-Software.


* Maxient Conduct Manager Software unterstützt **SP- und IDP-initiiertes** einmaliges Anmelden.

> [!NOTE]
> Der Bezeichner dieser Anwendung ist ein fester Zeichenfolgenwert, daher kann in einem Mandanten nur eine Instanz konfiguriert werden.

## <a name="adding-maxient-conduct-manager-software-from-the-gallery"></a>Hinzufügen von Maxient Conduct Manager Software über den Katalog

Um die Integration von Maxient Conduct Manager Software in Azure AD zu konfigurieren, müssen Sie Maxient Conduct Manager Software über den Katalog Ihrer Liste mit den verwalteten SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Maxient Conduct Manager Software** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **Maxient Conduct Manager Software** aus, und fügen Sie die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.


## <a name="configure-and-test-azure-ad-single-sign-on-for-maxient-conduct-manager-software"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Maxient Conduct Manager Software

Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Maxient Conduct Manager Software. Damit einmaliges Anmelden funktioniert, muss eine Verbindung zwischen Azure AD und der Maxient Conduct Manager Software hergestellt werden.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Maxient Conduct Manager Software zu konfigurieren:

1. **[Konfigurieren des einmaligen Anmeldens für Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzer die Authentifizierung für die Verwendung von Maxient Conduct Manager-Software zu ermöglichen.
   - **[Legen Sie „Benutzerzuweisung erforderlich?“ auf Nein fest](#set-user-assignment-required-to-no)** , damit sich jeder in Ihrer Einrichtung authentifizieren kann.
1. **[Testen von Azure AD Setup mit Maxient](#test-with-maxient)** , um zu überprüfen, ob die Konfiguration funktioniert und die richtigen Attribute freigegeben werden.

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **Maxient Conduct Manager Software** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Bearbeitungs- bzw. Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

1. Im Abschnitt **Grundlegende SAML-Konfiguration** ist die Anwendung im **IDP-initiierten** Modus vorkonfiguriert, und die erforderlichen URLs sind bereits mit Azure vorausgefüllt. Der Benutzer muss die Konfiguration speichern, indem er auf die Schaltfläche **Speichern** klickt.

1. Klicken Sie auf **Zusätzliche URLs festlegen**, und führen Sie den folgenden Schritt aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://cm.maxient.com/<SCHOOLCODE>`

    > [!NOTE]
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an Ihren Maxient-Implementierungs-/Supportmitarbeiter, um den Wert zu erhalten.

1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf die Schaltfläche „Kopieren“, um die **App-Verbundmetadaten-URL** zu kopieren, und speichern Sie sie auf Ihrem Computer.  Diese URL müssen Sie Ihrem Maxient-Implementierungs-/Supportmitarbeiter angeben.

    ![Downloadlink für das Zertifikat](common/copy-metadataurl.png)

<a name="set-user-assignment-required-to-no"></a>
    
### <a name="set-user-assignment-required-to-no"></a>Legen Sie „Benutzerzuweisung erforderlich?“ fest. auf Nein

Es ist wichtig zu beachten, dass dieser Schritt **ERFORDERLICH** ist, damit Maxient ordnungsgemäß funktioniert.  Maxient nutzt das Azure AD-System, um Benutzer zu *authentifizieren*. Die *Autorisierung* von Benutzern wird innerhalb des Maxient-Systems für die jeweilige Funktion ausgeführt, die diese durchführen möchten. Maxient verwendet keine Attribute aus Ihrem Verzeichnis, um diese Entscheidungen zu treffen.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste die Option **Maxient Conduct Manager Software** aus.
1. Ändern Sie auf der Übersichtsseite der App die Einstellung „Benutzerzuweisung erforderlich“ auf Nein.

## <a name="test-with-maxient"></a>Testen mit Maxient 

Wenn nicht bereits ein Supportticket bei einem Implementierungs-/Supportmitarbeiter von Maxient geöffnet wurde, senden Sie eine E-Mail mit dem Betreff „Campus-basierte Authentifizierung/Azure Setup – \<\<School Name\>\>“ an [support@maxient.com](mailto:support@maxient.com). Geben Sie im Textkörper der E-Mail die **App-Verbundmetadaten-URL** an. Ein Maxient-Mitarbeiter wird mit einem Testlink antworten, um zu überprüfen, ob die richtigen Attribute freigegeben werden.  
    
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Liste mit den Tutorials zur Integration von SaaS-Apps in Azure Active Directory](./tutorial-list.md)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Was ist der bedingte Zugriff in Azure Active Directory?](../conditional-access/overview.md)

- [Testen von Maxient Conduct Manager Software mit Azure AD](https://aad.portal.azure.com/)
