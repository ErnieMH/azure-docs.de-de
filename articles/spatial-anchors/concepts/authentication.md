---
title: Authentifizierung und Autorisierung
description: Lernen Sie die verschiedenen Möglichkeiten kennen, mit denen eine App oder ein Dienst sich bei Azure Spatial Anchors authentifizieren kann, und erfahren Sie, auf welchen Ebenen Sie den Zugriff auf Azure Spatial Anchors steuern können.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 10/08/2020
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.custom: devx-track-csharp
ms.openlocfilehash: 133b565bc54feaf49a2fec9dd0056ca8e7ef43f7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91857723"
---
# <a name="authentication-and-authorization-to-azure-spatial-anchors"></a>Authentifizierung und Autorisierung bei Azure Spatial Anchors

In diesem Abschnitt wird erläutert, welche verschiedenen Möglichkeiten Ihnen zum Authentifizieren bei Azure Spatial Anchors aus Ihrer App oder Ihrem Webdienst heraus zur Verfügung stehen. Zudem wird beleuchtet, auf welche Weise Sie die rollenbasierte Zugriffssteuerung in Azure Active Directory (Azure AD) verwenden können, um den Zugriff auf Ihre Spatial Anchors-Konten zu steuern.

## <a name="overview"></a>Übersicht

![Abbildung der Übersicht über die Authentifizierung bei Azure Spatial Anchors](./media/spatial-anchors-authentication-overview.png)

Um auf ein bestimmtes Azure Spatial Anchors-Konto zuzugreifen, müssen Clients zuerst ein Zugriffstoken aus dem Azure Mixed Reality-Sicherheitstokendienst (Security Token Service, STS) abrufen. Aus dem STS abgerufene Token sind 24 Stunden lang gültig und enthalten Informationen, mit denen Spatial Anchors-Dienste Autorisierungsentscheidungen für ein Konto treffen können. Diese Informationen stellen sicher, dass nur autorisierte Prinzipale auf dieses Konto zugreifen können.

Zugriffstoken können im Austausch gegen Kontoschlüssel oder von Azure AD ausgestellte Token abgerufen werden.

Kontoschlüssel ermöglichen Ihnen einen schnellen Einstieg in den Azure Spatial Anchors-Dienst; bevor Sie jedoch Ihre Anwendung in die Produktion bereitstellen, empfiehlt es sich, Ihre App für die Verwendung der Azure AD-basierten Authentifizierung zu aktualisieren.

Azure AD-Authentifizierungstoken können auf zwei Arten abgerufen werden:

- Wenn Sie eine Unternehmensanwendung erstellen und Ihr Unternehmen Azure AD als Identitätssystem verwendet, können Sie die benutzerbasierte Azure AD-Authentifizierung in Ihrer App verwenden und den Benutzern in Ihrer Organisation den Zugriff auf Ihre Spatial Anchors-Konten direkt oder über Ihre vorhandenen Azure AD-Sicherheitsgruppen gewähren.
- Andernfalls empfiehlt es sich, Azure AD-Token aus dem Webdienst abzurufen, der Ihre App unterstützt. Die Verwendung eines unterstützenden Webdiensts ist die empfohlene Authentifizierungsmethode für Produktionsanwendungen, da hierbei die Anmeldeinformationen für den Zugriff auf Azure Spatial Anchors nicht in Ihre Clientanwendung eingebettet werden müssen.

## <a name="account-keys"></a>Kontoschlüssel

Die Verwendung von Kontoschlüsseln für den Zugriff auf Ihr Azure Spatial Anchors-Konto ist die einfachste Möglichkeit für den Einstieg. Sie finden Ihre Kontoschlüssel im Azure-Portal. Navigieren Sie zu Ihrem Konto, und klicken Sie auf die Registerkarte „Schlüssel“.

![Screenshot der Seite „Schlüssel“ mit hervorgehobener Schaltfläche „Kopieren“ für den „Primärschlüssel“](../../../includes/media/spatial-anchors-get-started-create-resource/view-account-key.png)

Zwei Schlüssel stehen zur Verfügung, und beide sind gleichzeitig für den Zugriff auf das Spatial Anchors-Konto gültig. Es wird empfohlen, den Schlüssel, den Sie für den Zugriff auf das Konto verwenden, regelmäßig zu aktualisieren. Da zwei separate gültige Schlüssel verfügbar sind, sind solche Updates ohne Ausfallzeiten möglich: Sie müssen nur den primären und den sekundären Schlüssel abwechselnd aktualisieren.

Das SDK bietet integrierte Unterstützung für die Authentifizierung mit Kontoschlüssel, Sie müssen einfach nur die AccountKey-Eigenschaft in Ihrem cloudSession-Objekt festlegen.

# <a name="c"></a>[C#](#tab/csharp)

```csharp
this.cloudSession.Configuration.AccountKey = @"MyAccountKey";
```

# <a name="objc"></a>[ObjC](#tab/objc)

```objc
_cloudSession.configuration.accountKey = @"MyAccountKey";
```

# <a name="swift"></a>[Swift](#tab/swift)

```swift
_cloudSession!.configuration.accountKey = "MyAccountKey"
```

# <a name="java"></a>[Java](#tab/java)

```java
mCloudSession.getConfiguration().setAccountKey("MyAccountKey");
```

# <a name="c-ndk"></a>[C++ NDK](#tab/cpp)

```cpp
auto configuration = cloudSession_->Configuration();
configuration->AccountKey(R"(MyAccountKey)");
```

# <a name="c-winrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
auto configuration = m_cloudSession.Configuration();
configuration.AccountKey(LR"(MyAccountKey)");
```

---

Sobald das geschehen ist, verarbeitet das SDK den Austausch des Kontoschlüssels gegen ein Zugriffstoken und die notwendige Zwischenspeicherung der Token für Ihre App.

> [!WARNING]
> Die Verwendung von Kontoschlüsseln wird für ein schnelles Onboarding empfohlen, allerdings nur in den Phasen der Entwicklung und der Prototyperstellung. Es wird dringend angeraten, die Anwendung nicht mit einem eingebetteten Kontoschlüssel in die Produktion auszuliefern. Verwenden Sie stattdessen die benutzer- oder dienstbasierten Azure AD-Authentifizierungsverfahren, die im Folgenden erläutert werden.

## <a name="azure-ad-user-authentication"></a>Azure AD-Benutzerauthentifizierung

Für Anwendungen, die an Azure Active Directory-Benutzer gerichtet sind, wird empfohlen, ein Azure AD-Token für den Benutzer zu verwenden, das Sie mithilfe der [MSAL-Bibliothek](../../active-directory/develop/msal-overview.md) abrufen können. Befolgen Sie die Schritte im [Schnellstart zum Registrieren einer App](../../active-directory/develop/quickstart-register-app.md), einschließlich der folgenden:

1. Konfiguration im Azure-Portal
    1.    Registrieren Sie Ihre Anwendung in Azure AD als **native Anwendung**. Im Rahmen der Registrierung müssen Sie festlegen, ob Ihre Anwendung mehrinstanzenfähig sein soll oder nicht, und Sie müssen die Umleitungs-URLs angeben, die für Ihre Anwendung zugelassen sind.
        1.  Wechseln Sie zur Registerkarte **API-Berechtigungen**.
        2.  Wählen Sie **Berechtigung hinzufügen** aus.
            1.  Wählen Sie auf der Registerkarte **Von meiner Organisation verwendete APIs** die Option **Mixed Reality Resource Provider** (Mixed Reality-Ressourcenanbieter) aus.
            2.  Wählen Sie **Delegierte Berechtigungen** aus.
            3.  Aktivieren Sie das Kontrollkästchen **mixedreality.signin** unter **mixedreality**.
            4.  Wählen Sie **Berechtigungen hinzufügen** aus.
        3.  Wählen Sie **Administratoreinwilligung erteilen** aus.
    2.    Gewähren Sie Ihrer Anwendung bzw. den Benutzern Zugriff auf Ihre Ressource:
        1.    Navigieren Sie im Azure-Portal zu Ihrer Spatial Anchors-Ressource.
        2.    Wechseln Sie zur Registerkarte **Zugriffssteuerung (IAM)** .
        3.    Klicken Sie auf **Rollenzuweisung hinzufügen**.
            1.    [Wählen Sie eine Rolle aus](#role-based-access-control).
            2.    Geben Sie im Feld **Auswählen** die Namen der Benutzer, Gruppen und/oder Anwendungen ein, denen Sie Zugriff gewähren möchten.
            3.    Klicken Sie auf **Speichern**.
2. Gehen Sie im Code folgendermaßen vor:
    1.    Achten Sie darauf, die **Anwendungs-ID** und den **Umleitungs-URI** Ihrer eigenen Azure AD-Anwendung als Parameter für **Client-ID** und **Umleitungs-URI** in MSAL zu verwenden.
    2.    Legen Sie die Informationen zum Mandanten fest:
        1.    Wenn Ihre Anwendung **Nur meine Organisation** unterstützt, ersetzen Sie diesen Wert durch die **Mandanten-ID** oder den **Mandantennamen** (z.B. „contoso.microsoft.com“).
        2.    Wenn Ihre Anwendung **Konten in einem beliebigen Organisationsverzeichnis** unterstützt, ersetzen Sie diesen Wert durch **Organisationen**.
        3.    Wenn Ihre Anwendung **Alle Microsoft-Kontobenutzer** unterstützt, ersetzen Sie diesen Wert durch **Allgemein**.
    3.    Legen Sie in der Tokenanforderung den **Bereich** auf „https://sts.mixedreality.azure.com//.default“ fest. Dieser Bereich informiert Azure AD darüber, dass Ihre Anwendung ein Token für den Sicherheitstokendienst (Security Token Service, STS) Mixed Reality anfordert.

Damit sollte Ihre Anwendung in der Lage sein, von MSAL ein Azure AD-Token abzurufen. Sie können dieses Azure AD-Token als **authenticationToken** im config-Objekt Ihrer Cloudsitzung festlegen.

# <a name="c"></a>[C#](#tab/csharp)

```csharp
this.cloudSession.Configuration.AuthenticationToken = @"MyAuthenticationToken";
```

# <a name="objc"></a>[ObjC](#tab/objc)

```objc
_cloudSession.configuration.authenticationToken = @"MyAuthenticationToken";
```

# <a name="swift"></a>[Swift](#tab/swift)

```swift
_cloudSession!.configuration.authenticationToken = "MyAuthenticationToken"
```

# <a name="java"></a>[Java](#tab/java)

```java
mCloudSession.getConfiguration().setAuthenticationToken("MyAuthenticationToken");
```

# <a name="c-ndk"></a>[C++ NDK](#tab/cpp)

```cpp
auto configuration = cloudSession_->Configuration();
configuration->AuthenticationToken(R"(MyAuthenticationToken)");
```

# <a name="c-winrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
auto configuration = m_cloudSession.Configuration();
configuration.AuthenticationToken(LR"(MyAuthenticationToken)");
```

---

## <a name="azure-ad-service-authentication"></a>Azure AD-Dienstauthentifizierung

Die empfohlene Option für die Bereitstellung von Apps in die Produktion, die Azure Spatial Anchors verwenden, besteht darin, einen Back-End-Dienst als Broker für Authentifizierungsanforderungen zu nutzen. Das allgemeine Schema sollte der Beschreibung im folgenden Diagramm entsprechen:

![Übersicht über die Authentifizierung bei Azure Spatial Anchors](./media/spatial-anchors-aad-authentication.png)

Hier wird davon ausgegangen, dass Ihre App einen eigenen Mechanismus (z.B. Microsoft-Konto, PlayFab, Facebook, Google ID, benutzerdefinierter Benutzername/Kennwort usw.) für die Authentifizierung beim Back-End-Dienst verwendet. Sobald Ihre Benutzer bei Ihrem Back-End-Dienst authentifiziert sind, kann der Dienst ein Azure AD-Token abrufen, dieses gegen ein Zugriffstoken für Azure Spatial Anchors austauschen und es dann an die Clientanwendung zurückgeben.

Das Azure AD-Zugriffstoken wird mithilfe der [MSAL-Bibliothek](../../active-directory/develop/msal-overview.md) abgerufen. Befolgen Sie die Schritte im [Schnellstart zum Registrieren einer App](../../active-directory/develop/quickstart-register-app.md), einschließlich der folgenden:

1.    Konfiguration im Azure-Portal:
    1.    Registrieren Ihrer Anwendung bei Azure AD:
        1.    Navigieren Sie im Azure-Portal zu **Azure Active Directory**, und wählen Sie **App-Registrierungen** aus.
        2.    Wählen Sie **Registrierung einer neuen Anwendung** aus.
        3.    Geben Sie den Namen Ihrer Anwendung ein, wählen Sie **Web-App/API** als Anwendungstyp aus, und geben Sie die Authentifizierungs-URL für Ihren Dienst ein. Klicken Sie dann auf **Erstellen**.
        4.    Klicken Sie in dieser Anwendung auf **Einstellungen**, und wählen Sie die Registerkarte **Zertifikate und Geheimnisse** aus. Erstellen Sie einen neuen geheimen Clientschlüssel, wählen Sie eine Dauer aus, und klicken Sie auf **Hinzufügen**. Sie müssen den Wert des Geheimnisses speichern, da Sie ihn in den Code Ihres Webdiensts einschließen müssen.
    2.    Gewähren Sie Ihrer Anwendung bzw. den Benutzern Zugriff auf Ihre Ressource:
        1.    Navigieren Sie im Azure-Portal zu Ihrer Spatial Anchors-Ressource.
        2.    Wechseln Sie zur Registerkarte **Zugriffssteuerung (IAM)** .
        3.    Klicken Sie auf **Rollenzuweisung hinzufügen**.
        1.    [Wählen Sie eine Rolle aus](#role-based-access-control).
        2.    Geben Sie im Feld **Auswählen** die Namen der Anwendungen ein, die Sie erstellt haben und denen Sie Zugriff gewähren möchten. Wenn Sie möchten, dass die Benutzer Ihrer App unterschiedliche Rollen im Spatial Anchors-Konto einnehmen, müssen Sie mehrere Anwendungen in Azure AD erstellen und jeder eine separate Rolle zuweisen. Implementieren Sie dann Ihre Autorisierungslogik, damit die richtige Rolle für Ihre Benutzer verwendet wird.
        3.    Hinweis: Es empfiehlt sich, in der Auswahl **Rollenzuweisung hinzufügen** die Option **Zugriff zuweisen zu** auf „Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal“ festzulegen.
    3.    Klicken Sie auf **Speichern**.
2.    Gehen Sie in Ihrem Code folgendermaßen vor (Hinweis: Sie können das Dienstbeispiel aus GitHub verwenden):
    1.    Stellen Sie sicher, dass Sie die Anwendungs-ID, das Anwendungsgeheimnis und den Umleitungs-URI Ihrer eigenen Azure AD-Anwendung als Parameter für Client-ID, Geheimnis und Umleitungs-URI in MSAL verwenden.
    2.    Legen Sie die Mandanten-ID im authority-Parameter in MSAL auf Ihre eigene Azure ADD-Mandanten-ID fest.
    3.    Legen Sie in der Tokenanforderung den **Bereich** auf „https://sts.mixedreality.azure.com//.default“ fest.

Damit kann Ihr Back-End-Dienst ein Azure AD-Token abrufen. Der Dienst kann dieses Token gegen ein MR-Token austauschen, das er dann an den Client zurückgibt. Der Abruf eines MR-Tokens mithilfe eines Azure AD-Tokens erfolgt über einen REST-Aufruf. Im Folgenden sehen Sie einen Beispielaufruf:

```
GET https://sts.mixedreality.azure.com/Accounts/35d830cb-f062-4062-9792-d6316039df56/token HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni<truncated>FL8Hq5aaOqZQnJr1koaQ
Host: sts.mixedreality.azure.com
Connection: Keep-Alive

HTTP/1.1 200 OK
Date: Sun, 24 Feb 2019 08:00:00 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 1153
Accept: application/json
MS-CV: 05JLqWeKFkWpbdY944yl7A.0
{"AccessToken":"eyJhbGciOiJSUzI1NiIsImtpZCI6IjI2MzYyMTk5ZTI2NjQxOGU4ZjE3MThlM2IyMThjZTIxIiwidHlwIjoiSldUIn0.eyJqdGkiOiJmMGFiNWIyMy0wMmUxLTQ1MTQtOWEzNC0xNzkzMTA1NTc4NzAiLCJjYWkiOiIzNWQ4MzBjYi1mMDYyLTQwNjItOTc5Mi1kNjMxNjAzOWRmNTYiLCJ0aWQiOiIwMDAwMDAwMC0wMDAwLTAwMDAtMDAwMC0wMDAwMDAwMDAwMDAiLCJhaWQiOiIzNWQ4MzBjYi1mMDYyLTQwNjItOTc5Mi1kNjMxNjAzOWRmNTYiLCJhYW8iOi0xLCJhcHIiOiJlYXN0dXMyIiwicmlkIjoiL3N1YnNjcmlwdGlvbnMvNzIzOTdlN2EtNzA4NC00ODJhLTg3MzktNjM5Y2RmNTMxNTI0L3Jlc291cmNlR3JvdXBzL3NhbXBsZV9yZXNvdXJjZV9ncm91cC9wcm92aWRlcnMvTWljcm9zb2Z0Lk1peGVkUmVhbGl0eS9TcGF0aWFsQW5jaG9yc0FjY291bnRzL2RlbW9fYWNjb3VudCIsIm5iZiI6MTU0NDU0NzkwMywiZXhwIjoxNTQ0NjM0MzAzLCJpYXQiOjE1NDQ1NDc5MDMsImlzcyI6Imh0dHBzOi8vbXJjLWF1dGgtcHJvZC50cmFmZmljbWFuYWdlci5uZXQvIiwiYXVkIjoiaHR0cHM6Ly9tcmMtYW5jaG9yLXByb2QudHJhZmZpY21hbmFnZXIubmV0LyJ9.BFdyCX9UJj0i4W3OudmNUiuaGgVrlPasNM-5VqXdNAExD8acFJnHdvSf6uLiVvPiQwY1atYyPbOnLYhEbIcxNX-YAfZ-xyxCKYb3g_dbxU2w8nX3zDz_X3XqLL8Uha-rkapKbnNgxq4GjM-EBMCill2Svluf9crDmO-SmJbxqIaWzLmlUufQMWg_r8JG7RLseK6ntUDRyDgkF4ex515l2RWqQx7cw874raKgUO4qlx0cpBAB8cRtGHC-3fA7rZPM7UQQpm-BC3suXqRgROTzrKqfn_g-qTW4jAKBIXYG7iDefV2rGMRgem06YH_bDnpkgUa1UgJRRTckkBuLkO2FvA"}
```

Der Autorisierungsheader ist hierbei als `Bearer <Azure_AD_token>` formatiert.

Die Antwort enthält das MR-Token im Klartext.

Dieses MR-Token wird an den Client zurückgegeben. Ihre Client-App kann dieses dann als Zugriffstoken in der config-Datei für die Cloudsitzung festlegen.

# <a name="c"></a>[C#](#tab/csharp)

```csharp
this.cloudSession.Configuration.AccessToken = @"MyAccessToken";
```

# <a name="objc"></a>[ObjC](#tab/objc)

```objc
_cloudSession.configuration.accessToken = @"MyAccessToken";
```

# <a name="swift"></a>[Swift](#tab/swift)

```swift
_cloudSession!.configuration.accessToken = "MyAccessToken"
```

# <a name="java"></a>[Java](#tab/java)

```java
mCloudSession.getConfiguration().setAccessToken("MyAccessToken");
```

# <a name="c-ndk"></a>[C++ NDK](#tab/cpp)

```cpp
auto configuration = cloudSession_->Configuration();
configuration->AccessToken(R"(MyAccessToken)");
```

# <a name="c-winrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
auto configuration = m_cloudSession.Configuration();
configuration.AccessToken(LR"(MyAccessToken)");
```

---

## <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

Damit Sie die Zugriffsebene für Anwendungen, Dienste oder Azure AD-Benutzer Ihres Diensts besser steuern können, wurden folgende Rollen für Sie erstellt, die Sie nach Bedarf für Ihre Azure Spatial Anchors-Konten zuweisen können:

- **Spatial Anchors-Kontobesitzer**: Anwendungen oder Benutzer mit dieser Rolle können Raumanker erstellen, abfragen und löschen. Wenn Sie die Authentifizierung bei Ihrem Konto über Kontoschlüssel durchführen, wird die Rolle **Spatial Anchors-Kontobesitzer** dem authentifizierten Prinzipal zugewiesen.
- **Spatial Anchors-Kontomitwirkender**: Anwendungen oder Benutzer mit dieser Rolle können Raumanker erstellen und abfragen, aber nicht löschen.
- **Spatial Anchors-Kontoleser**: Anwendungen oder Benutzer mit dieser Rolle können Raumanker nur abfragen, sie aber weder erstellen noch löschen oder Metadaten für Raumanker aktualisieren. Diese Rolle wird in der Regel für Anwendungen verwendet, bei denen einige Benutzer die Umgebung kuratieren, während andere nur Anker zurückrufen können, die zuvor in dieser Umgebung platziert waren.

## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie Ihre erste App mit Azure Spatial Anchors.

> [!div class="nextstepaction"]
> [Unity (HoloLens)](../quickstarts/get-started-unity-hololens.md)

> [!div class="nextstepaction"]
> [Unity (iOS)](../quickstarts/get-started-unity-ios.md)

> [!div class="nextstepaction"]
> [Unity (Android)](../quickstarts/get-started-unity-android.md)

> [!div class="nextstepaction"]
> [iOS](../quickstarts/get-started-ios.md)

> [!div class="nextstepaction"]
> [Android](../quickstarts/get-started-android.md)

> [!div class="nextstepaction"]
> [HoloLens](../quickstarts/get-started-hololens.md)

> [!div class="nextstepaction"]
> [Xamarin (Android)](../quickstarts/get-started-xamarin-android.md)

> [!div class="nextstepaction"]
> [Xamarin (iOS)](../quickstarts/get-started-xamarin-ios.md)
