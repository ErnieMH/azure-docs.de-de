---
title: Unterschiede zwischen MSAL für iOS und macOS | Azure
titleSuffix: Microsoft identity platform
description: Enthält eine Beschreibung der Unterschiede bei Verwendung der Microsoft Authentication Library (MSAL) unter iOS und macOS.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 08/28/2019
ms.author: marsma
ms.reviewer: oldalton
ms.custom: aaddev
ms.openlocfilehash: 41389bc5ed8580cd80dbc40e771c7f15241f5ae7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85479401"
---
# <a name="microsoft-authentication-library-for-ios-and-macos-differences"></a>Microsoft Authentication Library für iOS und macOS: Unterschiede

In diesem Artikel werden die Funktionalitätsunterschiede bei Verwendung der Microsoft Authentication Library (MSAL) unter iOS und macOS beschrieben.

> [!NOTE]
> Auf einem Macintosh werden von der MSAL nur macOS-Apps unterstützt.

## <a name="general-differences"></a>Allgemeine Unterschiede

Die MSAL für macOS umfasst eine Teilmenge der Funktionalität von iOS.

Folgendes wird von der MSAL für macOS nicht unterstützt:

- Verschiedene Browsertypen, z. B. `ASWebAuthenticationSession`, `SFAuthenticationSession` und `SFSafariViewController`.
- Die Authentifizierung per Broker über die Microsoft Authenticator-App wird für macOS nicht unterstützt.

Für die Keychainfreigabe zwischen Apps desselben Herausgebers gelten unter macOS 10.14 und früher stärkere Einschränkungen. Verwenden Sie [Zugriffssteuerungslisten](https://developer.apple.com/documentation/security/keychain_services/access_control_lists?language=objc), um die Pfade zu den Apps anzugeben, von denen die Keychain gemeinsam verwendet werden soll. Es kann sein, dass Benutzern zusätzliche Keychain-Eingabeaufforderungen angezeigt werden.

Unter macOS 10.15+ ist das Verhalten der MSAL für iOS und macOS identisch. Für die MSAL werden [Keychain-Zugriffsgruppen](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps?language=objc) für die Keychainfreigabe verwendet. 

### <a name="conditional-access-authentication-differences"></a>Unterschiede bei der Authentifizierung für bedingten Zugriff

Bei Szenarien mit bedingtem Zugriff werden weniger Benutzereingabeaufforderungen angezeigt, wenn Sie die MSAL für iOS verwenden. Der Grund ist, dass für iOS die Broker-App (Microsoft Authenticator) genutzt wird, sodass die Aufforderung zur Eingabe des Benutzers in einigen Fällen nicht erforderlich ist.

### <a name="project-setup-differences"></a>Unterschiede bei der Projekteinrichtung

**macOS**

- Stellen Sie beim Einrichten Ihres Projekts unter macOS sicher, dass Ihre Anwendung mit einem gültigen Entwicklungs- oder Produktionszertifikat signiert ist. Die MSAL funktioniert auch im nicht signierten Modus, weist aber in Bezug auf die Cachepersistenz ein anderes Verhalten auf. Die App sollte nur für Debugzwecke unsigniert ausgeführt werden. Wenn Sie die App unsigniert herausgeben, gilt Folgendes:
1. Unter Version 10.14 und früher fordert die MSAL den Benutzer bei jedem neuen Start der App zur Eingabe eines Keychainkennworts auf.
2. Unter Version 10.15 und höher fordert die MSAL den Benutzer bei jedem Tokenabruf zur Eingabe der Anmeldeinformationen auf. 

- Für macOS-Apps muss der AppDelegate-Aufruf nicht implementiert werden.

**iOS**

- Es müssen weitere Schritte zur Einrichtung Ihres Projekts ausgeführt werden, damit der Ablauf zur Authentifizierung per Broker unterstützt wird. Die Schritte sind im Tutorial beschrieben.
- Für iOS-Projekte müssen in der Datei „info.plist“ benutzerdefinierte Schemas registriert werden. Unter macOS ist dies nicht erforderlich.
