---
title: Azure Active Directory-Integration für Azure Red Hat OpenShift
description: Erfahren Sie, wie Sie eine Azure AD-Sicherheitsgruppe (Azure Active Directory) und einen Benutzer zum Testen von Apps in Ihrem Microsoft Azure Red Hat OpenShift-Cluster erstellen.
author: jimzim
ms.author: jzim
ms.service: azure-redhat-openshift
ms.topic: conceptual
ms.date: 05/13/2019
ms.openlocfilehash: f0bf28d61d4c9ad95a485fb4b60e370c16ace16c
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "100633324"
---
# <a name="azure-active-directory-integration-for-azure-red-hat-openshift"></a>Azure Active Directory-Integration für Azure Red Hat OpenShift

> [!IMPORTANT]
> Azure Red Hat OpenShift 3.11 wird zum 30. Juni 2022 eingestellt. Unterstützung für die Erstellung neuer Azure Red Hat OpenShift 3.11-Cluster wird bis zum 30. November 2020 bereitgestellt. Nach der Einstellung werden die verbleibenden Azure Red Hat OpenShift 3.11-Cluster abgeschaltet, um Sicherheitsrisiken zu vermeiden.
> 
> Führen Sie die Schritte in diesem Leitfaden aus, um [einen Azure Red Hat OpenShift 4-Cluster zu erstellen](tutorial-create-cluster.md).
> Wenn Sie spezielle Fragen haben, [kontaktieren Sie uns](mailto:arofeedback@microsoft.com).

Wenn Sie noch keinen Azure Active Directory-Mandanten erstellt haben, befolgen Sie die Anweisungen unter [Erstellen von Azure AD-Mandanten für Azure Red Hat OpenShift](howto-create-tenant.md), bevor Sie mit dieser Anleitung fortfahren.

Microsoft Azure Red Hat OpenShift benötigt Berechtigungen zum Ausführen von Tasks für Ihren Cluster. Wenn Ihre Organisation noch keine Azure AD-Benutzer, Azure AD-Sicherheitsgruppe oder Azure AD-App-Registrierung zur Verwendung als Dienstprinzipal besitzt, befolgen Sie die folgenden Anweisungen, um diese zu erstellen.

## <a name="create-a-new-azure-active-directory-user"></a>Erstellen eines neuen Azure Active Directory-Benutzers

Stellen Sie im [Azure-Portal](https://portal.azure.com) sicher, dass Ihr Mandant unter Ihrem Benutzernamen oben rechts im Portal angezeigt wird:

![Screenshot des Portals mit Anzeige des Mandanten oben rechts](./media/howto-create-tenant/tenant-callout.png) Wenn der falsche Mandant angezeigt wird, klicken Sie oben rechts auf Ihren Benutzernamen. Klicken Sie dann auf **Verzeichnis wechseln**, und wählen Sie den richtigen Mandanten aus der Liste **Alle Verzeichnisse** aus.

Erstellen Sie einen neuen Azure Active Directory-Benutzer vom Typ „Besitzer“, um sich bei Ihrem Azure Red Hat OpenShift-Cluster anzumelden.

1. Wechseln Sie zum Blatt [Benutzer – Alle Benutzer](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UsersManagementMenuBlade/AllUsers).
2. Klicken Sie zum Öffnen des Bereichs **Benutzer** auf **Neuer Benutzer**.
3. Geben Sie einen **Namen** für diesen Benutzer ein.
4. Erstellen Sie einen **Benutzernamen**, der auf dem Namen des erstellten Mandanten basiert, und fügen Sie am Ende `.onmicrosoft.com` an. Beispiel: `yourUserName@yourTenantName.onmicrosoft.com`. Notieren Sie sich diesen Benutzernamen. Sie benötigen ihn zur Anmeldung bei Ihrem Cluster.
5. Klicken Sie zum Öffnen des Bereichs „Verzeichnisrolle“ auf **Verzeichnisrolle**, und wählen Sie **Besitzer** aus. Klicken Sie dann am unteren Rand des Bereichs auf **OK**.
6. Klicken Sie im Bereich **Benutzer** auf **Kennwort anzeigen**, und notieren Sie das temporäre Kennwort. Nachdem Sie sich zum ersten Mal angemeldet haben, werden Sie zum Zurücksetzen des Kennworts aufgefordert.
7. Klicken Sie im unteren Bereich auf **Erstellen**, um den Benutzer zu erstellen.

## <a name="create-an-azure-ad-security-group"></a>Erstellen einer Azure AD-Sicherheitsgruppe

Die Mitgliedschaften in einer Azure AD-Sicherheitsgruppe werden mit der OpenShift-Gruppe „osa-customer-admins“ synchronisiert, um Clusteradministratorzugriff zu gewähren. Wenn dies nicht angegeben wird, wird kein Clusteradministratorzugriff gewährt.

1. Öffnen Sie das Blatt [Azure Active Directory-Gruppen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/GroupsManagementMenuBlade/AllGroups).
2. Klicken Sie auf **+Neue Gruppe**.
3. Geben Sie einen Gruppennamen und eine Beschreibung an.
4. Legen Sie für **Gruppentyp** die Option **Sicherheit** fest.
5. Legen Sie den **Mitgliedschaftstyp** auf **Zugewiesen** fest.

    Fügen Sie dieser Sicherheitsgruppe den Azure AD-Benutzer hinzu, den Sie in einem vorherigen Schritt erstellt haben.

6. Klicken Sie auf **Mitglieder**, um den Bereich **Mitglieder auswählen** zu öffnen.
7. Wählen Sie in der Liste der Mitglieder den Azure AD-Benutzer aus, den Sie zuvor erstellt haben.
8. Klicken Sie im Portal im unteren Bereich der Seite zuerst auf **Auswählen** und anschließend auf **Erstellen**, um die Sicherheitsgruppe zu erstellen.

    Notieren Sie sich den Wert der Gruppen-ID.

9. Wenn die Gruppe erstellt wurde, wird diese in der Liste aller Gruppen angezeigt. Klicken Sie auf die neue Gruppe.
10. Kopieren Sie die **Objekt-ID**, die auf der Seite angezeigt wird. Im Tutorial [Erstellen eines Azure Red Hat OpenShift-Clusters](tutorial-create-cluster.md) bezeichnen wir diesen Wert als `GROUPID`.

> [!IMPORTANT]
> Um diese Gruppe mit der OpenShift-Gruppe „osa-customer-admins“ zu synchronisieren, erstellen Sie den Cluster mithilfe der Azure CLI. Im Azure-Portal fehlt zurzeit ein Feld zum Festlegen dieser Gruppe.

## <a name="create-an-azure-ad-app-registration"></a>Erstellen einer Azure AD-App-Registrierung

Sie können automatisch einen Client für die Azure Active Directory-App-Registrierung als Teil der Erstellung des Clusters erstellen, indem Sie das `--aad-client-app-id`-Flag im `az openshift create`-Befehl auslassen. In diesem Tutorial erfahren Sie, wie Sie der Vollständigkeit halber die Azure AD-App-Registrierung erstellen.

Wenn Ihre Organisation noch keine Azure Active Directory-App-Registrierung (Azure AD) zur Verwendung als Dienstprinzipal besitzt, befolgen Sie die folgenden Anweisungen, um diese zu erstellen.

1. Öffnen Sie das Blatt [App-Registrierungen](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview), und klicken Sie auf **Neue Registrierung**.
2. Geben Sie im Bereich **Anwendung registrieren** einen Namen für Ihre Anwendungsregistrierung ein.
3. Stellen Sie sicher, dass unter **Unterstützte Kontotypen** die Option **Nur Konten in diesem Organisationsverzeichnis** ausgewählt ist. Dies ist die sicherste Option.
4. Sobald wir den URI des Clusters wissen, fügen wir einen Umleitungs-URI hinzu. Klicken Sie auf die Schaltfläche **Registrieren**, um die Azure AD-Anwendungsregistrierung zu erstellen.
5. Kopieren Sie die **Anwendungs-ID (Client-ID)** , die auf der Seite angezeigt wird. Im Tutorial [Erstellen eines Azure Red Hat OpenShift-Clusters](tutorial-create-cluster.md) bezeichnen wir diesen Wert als `APPID`.

![Screenshot der Seite „App-Objekt“](./media/howto-create-tenant/get-app-id.png)

### <a name="create-a-client-secret"></a>Erstellen eines Clientgeheimnisses

Generieren Sie ein Clientgeheimnis zur Authentifizierung Ihrer App bei Azure Active Directory.

1. Klicken Sie im Abschnitt **Verwalten** der Seite „App-Registrierungen“ auf **Zertifikate und Geheimnisse**.
2. Klicken Sie im Bereich **Zertifikate und Geheimnisse** auf **Neuer geheimer Clientschlüssel**.  Der Bereich **Geheimen Clientschlüssel hinzufügen** wird angezeigt.
3. Geben Sie eine **Beschreibung** ein.
4. Legen Sie **Expires** (Läuft ab) auf die von Ihnen gewünschte Dauer fest (z. B. **In 2 Jahren**).
5. Klicken Sie auf **Hinzufügen**. Daraufhin wird der Schlüsselwert im Abschnitt **Geheime Clientschlüssel** der Seite angezeigt.
6. Notieren Sie sich den Schlüsselwert. Im Tutorial [Erstellen eines Azure Red Hat OpenShift-Clusters](tutorial-create-cluster.md) bezeichnen wir diesen Wert als `SECRET`.

![Screenshot des Bereichs „Zertifikate und Geheimnisse“](./media/howto-create-tenant/create-key.png)

Weitere Informationen zu Azure-Anwendungsobjekten finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory](../active-directory/develop/app-objects-and-service-principals.md).

Details zur Erstellung einer neuen Azure AD-Anwendung finden Sie unter [Registrieren einer App mit dem Azure AD v1.0-Endpunkt](../active-directory/develop/quickstart-register-app.md).

## <a name="add-api-permissions"></a>Hinzufügen von API-Berechtigungen

[//]: # (Wechseln Sie nicht zu Microsoft Graph. Die Verwendung mit Microsoft Graph ist nicht möglich.)
1. Klicken Sie im Bereich **Verwalten** auf **API-Berechtigungen**.
2. Klicken Sie auf **Berechtigung hinzufügen**, und wählen Sie **Azure Active Directory Graph** und dann **Delegierte Berechtigungen** aus.
> [!NOTE]
> Vergewissern Sie sich, dass Sie die Kachel „Azure Active Directory Graph“ und nicht die Kachel „Microsoft Graph“ ausgewählt haben.

3. Erweitern Sie **Benutzer** in der folgenden Liste, und aktivieren Sie die Berechtigung **User.Read**. Wenn **User.Read** standardmäßig aktiviert ist, stellen Sie sicher, dass es sich um die **Azure Active Directory Graph**-Berechtigung **User.Read** handelt.
4. Scrollen Sie nach oben, und wählen Sie **Anwendungsberechtigungen** aus.
5. Erweitern Sie **Verzeichnis** in der folgenden Liste, und aktivieren Sie **Directory.ReadAll**.
6. Klicken Sie auf **Berechtigungen hinzufügen**, um die Änderungen zu übernehmen.
7. Der Bereich der API-Berechtigungen sollte nun sowohl *User.Read* als auch *Directory.ReadAll* anzeigen. Bitte beachten Sie die Warnung in der Spalte **Administratoreinwilligung erforderlich** neben *Directory.ReadAll*.
8. Wenn Sie der *Azure-Abonnementadministrator* sind, klicken Sie unten auf **Administratorzustimmung für *Abonnementname* erteilen**. Wenn Sie nicht der *Azure-Abonnementadministrator* sind, fordern Sie die Zustimmung von Ihrem Administrator an.

![Screenshot des Bereichs der API-Berechtigungen. Die Berechtigungen „User.Read“ und „Directory.ReadAll“ sind hinzugefügt, die Zustimmung des Administrators ist für „Directory.ReadAll“ erforderlich.](./media/howto-aad-app-configuration/permissions-required.png)

> [!IMPORTANT]
> Die Synchronisierung der Clusteradministratorengruppe funktioniert nur, nachdem die Zustimmung erteilt ist. Daraufhin wird ein grüner Kreis mit einem Häkchen und die Meldung „Gewährt für *Abonnementname*“ in der Spalte *Administratoreinwilligung erforderlich* angezeigt.

Weitere Informationen zum Verwalten von Administratoren und anderen Rollen finden Sie unter [Hinzufügen oder Ändern von Azure-Abonnementadministratoren](../cost-management-billing/manage/add-change-subscription-administrator.md).

## <a name="resources"></a>Ressourcen

* [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory](../active-directory/develop/app-objects-and-service-principals.md)
* [Schnellstart: Registrieren einer App mit dem Azure AD v1.0-Endpunkt](../active-directory/develop/quickstart-register-app.md)

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie alle [Azure Red Hat OpenShift-Voraussetzungen](howto-setup-environment.md) erfüllt haben, können Sie Ihren ersten Cluster erstellen!

Lesen Sie dieses Tutorial:
> [!div class="nextstepaction"]
> [Erstellen eines Azure Red Hat OpenShift-Clusters](tutorial-create-cluster.md)