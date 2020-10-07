---
title: Hinzufügen von App-Rollen und Abrufen dieser Rollen aus einem Token | Azure
titleSuffix: Microsoft identity platform
description: Hier erfahren Sie, wie Sie App-Rollen in einer bei Azure Active Directory registrierten Anwendung hinzufügen, diesen Rollen Benutzer und Gruppen zuweisen und die Rollen im `roles`-Anspruch im Token empfangen.
services: active-directory
author: kalyankrishna1
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: how-to
ms.date: 07/15/2020
ms.author: kkrishna
ms.reviewer: kkrishna, jmprieur
ms.custom: aaddev
ms.openlocfilehash: be5cb1c1e6ff428b3c4d4305c915e07d3880839c
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91258386"
---
# <a name="how-to-add-app-roles-in-your-application-and-receive-them-in-the-token"></a>Gewusst wie: Hinzufügen von App-Rollen in Ihrer Anwendung und Empfangen der Rollen im Token

Die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) ist eine beliebte Methode zum Erzwingen der Autorisierung in Anwendungen. Bei der Verwendung der RBAC gewährt ein Administrator Berechtigungen für Rollen und nicht für einzelne Benutzer oder Gruppen. Der Administrator kann anschließend verschiedenen Benutzern und Gruppen Rollen zuweisen, um zu steuern, wer auf welche Inhalte und Funktionen zugreifen kann.

Entwickler können die RBAC mit Anwendungsrollen und Rollenansprüche verwenden und dadurch mit wenig Aufwand sicher die Autorisierung in ihren Apps erzwingen.

Ein weiterer Ansatz ist die Verwendung von Azure AD-Gruppen und Gruppenansprüchen wie unter [WebApp-GroupClaims-DotNet](https://github.com/Azure-Samples/WebApp-GroupClaims-DotNet) gezeigt. Azure AD-Gruppen und Anwendungsrollen schließen sich keinesfalls gegenseitig aus. Sie können zusammen verwendet werden, um eine noch differenziertere Zugriffssteuerung zu ermöglichen.

## <a name="declare-roles-for-an-application"></a>Deklarieren von Rollen für eine Anwendung

Diese Anwendungsrollen werden im [Azure-Portal](https://portal.azure.com) im Registrierungsmanifest der Anwendung definiert.  Wenn sich ein Benutzer bei der Anwendung anmeldet, gibt Azure AD für den Benutzer einen `roles`-Anspruch für jede einzeln zugewiesene Rolle und für die Gruppenmitgliedschaft aus.  Die Zuweisung von Benutzern und Gruppen zu Rollen kann über die Benutzeroberfläche des Portals oder programmgesteuert mit [Microsoft Graph](/graph/azuread-identity-access-management-concept-overview) erfolgen.

### <a name="declare-app-roles-using-azure-portal"></a>Deklarieren von App-Rollen mithilfe des Azure-Portals

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie auf der Symbolleiste im Portal das Symbol **Verzeichnis + Abonnement** aus.
1. Wählen Sie in der Liste **Favoriten** oder **Alle Verzeichnisse** den Azure Active Directory-Mandanten aus, bei dem Sie Ihre Anwendung registrieren möchten.
1. Suchen Sie im Azure-Portal nach **Azure Active Directory**, und wählen Sie es aus.
1. Wählen Sie im Bereich **Azure Active Directory** die Option **App-Registrierungen** aus, um eine Liste aller Anwendungen anzuzeigen.
1. Wählen Sie die Anwendung aus, in der Sie App-Rollen definieren möchten. Wählen Sie anschließend **Manifest** aus.
1. Bearbeiten Sie das Manifest, indem Sie die `appRoles`-Einstellung suchen und alle Anwendungsrollen hinzufügen.

     > [!NOTE]
     > Für jede Anwendungsrollendefinition in diesem Manifest muss im Kontext des Manifests eine andere gültige GUID für die `id`-Eigenschaft angegeben sein.
     >
     > Die `value`-Eigenschaft der einzelnen Anwendungsrollendefinition muss exakt mit den Zeichenfolgen übereinstimmen, die im Code in der Anwendung verwendet werden. Die `value`-Eigenschaft darf keine Leerzeichen enthalten. Ist dies der Fall, erhalten Sie einen Fehler, wenn Sie das Manifest speichern.

1. Speichern Sie das Manifest.

### <a name="examples"></a>Beispiele

Das folgende Beispiel zeigt den `appRoles`-Wert, den Sie `users` zuweisen können.

> [!NOTE]
>`id` muss eine eindeutige GUID sein.

```Json
"appId": "8763f1c4-f988-489c-a51e-158e9ef97d6a",
"appRoles": [
    {
      "allowedMemberTypes": [
        "User"
      ],
      "displayName": "Writer",
      "id": "d1c2ade8-98f8-45fd-aa4a-6d06b947c66f",
      "isEnabled": true,
      "description": "Writers Have the ability to create tasks.",
      "value": "Writer"
    }
  ],
"availableToOtherTenants": false,
```

> [!NOTE]
>`displayName` darf Leerzeichen enthalten.

Sie können App-Rollen mit Ausrichtung auf `users`, `applications` oder auf beides festlegen. Wenn App-Rollen für `applications` verfügbar sind, werden sie als Anwendungsberechtigungen im Abschnitt **Verwalten** > **API-Berechtigungen > Berechtigung hinzufügen > Meine APIs > API auswählen > Anwendungsberechtigungen** angezeigt. Das folgende Beispiel zeigt eine App-Rolle für `Application`.

```Json
"appId": "8763f1c4-f988-489c-a51e-158e9ef97d6a",
"appRoles": [
    {
      "allowedMemberTypes": [
        "Application"
      ],
      "displayName": "ConsumerApps",
      "id": "47fbb575-859a-4941-89c9-0f7a6c30beac",
      "isEnabled": true,
      "description": "Consumer apps have access to the consumer data.",
      "value": "Consumer"
    }
  ],
"availableToOtherTenants": false,
```

Die Anzahl der definierten Rollen wirkt sich auf die Grenzwerte des Anwendungsmanifests aus. Diese wurden ausführlich auf der Seite [Grenzwerte für das Manifest](./reference-app-manifest.md#manifest-limits) erläutert.

### <a name="assign-users-and-groups-to-roles"></a>Zuweisen von Benutzern und Gruppen zu Rollen

Nachdem Sie App-Rollen in Ihrer Anwendung hinzugefügt haben, können Sie diesen Rollen Benutzer und Gruppen zuweisen.

1. Wählen Sie im Bereich **Azure Active Directory** im Navigationsmenü **Azure Active Directory** auf der linken Seite die Option **Unternehmensanwendungen**.
1. Wählen Sie **Alle Anwendungen**, um eine Liste mit Ihren Anwendungen anzuzeigen.

     Falls die gewünschte Anwendung hier nicht angezeigt wird, können Sie oben in der Liste **Alle Anwendungen** die verschiedenen Filter verwenden, um den Inhalt der Liste einzugrenzen. Sie können auch in der Liste nach unten scrollen, um nach Ihrer Anwendung zu suchen.

1. Wählen Sie die Anwendung aus, in der Sie Benutzer oder eine Sicherheitsgruppe zu Rollen zuweisen möchten.
1. Wählen Sie in der Anwendung im Navigationsmenü auf der linken Seite den Bereich **Benutzer und Gruppen** aus.
1. Wählen Sie oben in der Liste **Benutzer und Gruppen** die Schaltfläche **Benutzer hinzufügen** aus, um den Bereich **Zuweisung hinzufügen** zu öffnen.
1. Wählen Sie im Bereich **Zuweisung hinzufügen** den Selektor **Benutzer und Gruppen** aus.

     Eine Liste mit Benutzern und Sicherheitsgruppen wird mit einem Textfeld angezeigt, mit dem bestimmte Benutzer oder Gruppen gesucht werden können. Auf diesem Bildschirm können Sie mehrere Benutzer und Gruppen auf einmal auswählen.

1. Nachdem Sie mit dem Auswählen der Benutzer und Gruppen fertig sind, können Sie unten die Schaltfläche **Auswählen** verwenden, um mit dem nächsten Teil fortzufahren.
1. Wählen Sie im Bereich **Zuweisung hinzufügen** den Selektor **Benutzer** aus. Alle weiter oben im App-Manifest deklarierten Rollen werden angezeigt.
1. Wählen Sie eine Rolle und dann die Schaltfläche **Auswählen** aus.
1. Wählen Sie unten die Schaltfläche **Zuweisen** aus, um die Zuweisungen von Benutzern und Gruppen zur App abzuschließen.
1. Vergewissern Sie sich, dass die hinzugefügten Benutzer und Gruppen in der aktualisierten Liste **Benutzer und Gruppen** angezeigt werden.

### <a name="receive-roles-in-tokens"></a>Empfangen von Rollen in Token

Wenn sich Benutzer, denen verschiedene App-Rollen zugewiesen sind, bei der Anwendung anmelden, enthalten deren Token die zugewiesenen Rollen im `roles`-Anspruch.

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen von Autorisierung zu einer ASP.NET Core-Web-App mit App-Rollen und Rollenansprüchen](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/5-WebApp-AuthZ/5-1-Roles)
- [Implementieren der Autorisierung in Ihren Anwendungen mit Microsoft Identity Platform (Video)](https://www.youtube.com/watch?v=LRoc-na27l0)
- [Azure Active Directory, now with Group Claims and Application Roles](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-now-with-Group-Claims-and-Application/ba-p/243862) (Azure Active Directory jetzt mit Gruppenansprüchen und Anwendungsrollen)
- [Azure Active Directory-App-Manifest](./reference-app-manifest.md)
- [ Azure AD-AD-Zugriffstoken](access-tokens.md)
- [Azure AD `id_tokens`](id-tokens.md)
