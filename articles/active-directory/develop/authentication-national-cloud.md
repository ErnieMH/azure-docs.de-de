---
title: Azure AD-Authentifizierung und nationale Clouds | Azure
titleSuffix: Microsoft identity platform
description: Hier erhalten Sie Informationen zu App-Registrierungs- und Authentifizierungsendpunkten für nationale Clouds.
services: active-directory
author: negoe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 08/28/2019
ms.author: negoe
ms.reviewer: negoe,celested
ms.custom: aaddev,references_regions
ms.openlocfilehash: d27498748f03fb101d165351ec80c77d57f78f1a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88141498"
---
# <a name="national-clouds"></a>Nationale Clouds

Nationale Clouds (auch als Sovereign Clouds bezeichnet) sind physisch isolierte Azure-Instanzen. Diese Regionen von Azure sollen dafür sorgen, dass Anforderungen an Datenresidenz, Datenhoheit und Compliance innerhalb geografischer Grenzen erfüllt werden.

Neben der globalen Cloud wird Azure Active Directory (Azure AD) in den folgenden nationalen Clouds bereitgestellt:  

- Azure Government
- Azure Deutschland
- Azure China 21Vianet

Nationale Clouds sind eindeutig und von der globalen Azure-Umgebung getrennt. Es ist wichtig, beim Entwickeln von Anwendungen für diese Umgebungen die wichtigsten Unterschiede zu kennen. Zu den Unterschieden gehören das Registrieren von Anwendungen, Abrufen von Token und Konfigurieren von Endpunkten.

## <a name="app-registration-endpoints"></a>App-Registrierungsendpunkte

Es gibt für jede nationale Cloud ein separates Azure-Portal. Um Anwendungen mit der Microsoft Identity Platform in eine nationale Cloud zu integrieren, müssen Sie Ihre Anwendung jeweils separat im speziellen Azure-Portal für die Umgebung registrieren.

Die folgende Tabelle enthält die Basis-URLs für die Azure AD-Endpunkte, die zum Registrieren einer Anwendung für die einzelnen nationalen Clouds verwendet werden.

| Nationale Cloud | Azure AD-Portalendpunkt |
|----------------|--------------------------|
| Azure AD für US-Regierungsbehörden | `https://portal.azure.us` |
| Azure AD Deutschland | `https://portal.microsoftazure.de` |
| Azure AD China, betrieben von 21Vianet | `https://portal.azure.cn` |
| Azure AD (globaler Dienst) |`https://portal.azure.com` |

## <a name="azure-ad-authentication-endpoints"></a>Azure AD-Authentifizierungsendpunkte

Alle nationalen Clouds authentifizieren Benutzer in jeder Umgebung separat und verfügen über separate Authentifizierungsendpunkte.

Die folgende Tabelle enthält die Basis-URLs für die Azure AD-Endpunkte, die zum Abrufen von Token für die einzelnen nationalen Clouds verwendet werden.

| Nationale Cloud | Azure AD-Authentifizierungsendpunkt |
|----------------|-------------------------|
| Azure AD für US-Regierungsbehörden | `https://login.microsoftonline.us` |
| Azure AD Deutschland| `https://login.microsoftonline.de` |
| Azure AD China, betrieben von 21Vianet | `https://login.chinacloudapi.cn` |
| Azure AD (globaler Dienst)| `https://login.microsoftonline.com` |

Anforderungen an die Autorisierungs- oder Tokenendpunkte von Azure AD können mit der entsprechenden regionsspezifischen Basis-URL generiert werden. Z.B. für Azure Deutschland:

  - Der allgemeine Autorisierungsendpunkt ist `https://login.microsoftonline.de/common/oauth2/v2.0/authorize`.
  - Der allgemeine Tokenendpunkt ist `https://login.microsoftonline.de/common/oauth2/v2.0/token`.

Ersetzen Sie bei Anwendungen mit einem Mandanten in den oben genannten URLs „common“ durch die ID oder den Namen Ihres Mandanten. z. B. `https://login.microsoftonline.de/contoso.com`.

## <a name="microsoft-graph-api"></a>Microsoft Graph-API

Informationen zum Aufrufen der Microsoft Graph-APIs in der Umgebung der nationalen Cloud finden Sie unter [Microsoft Graph in nationalen Cloudbereitstellungen](/graph/deployments).

> [!IMPORTANT]
> Bestimmte Dienste und Funktionen, die in bestimmten Regionen des globalen Diensts vorhanden sind, sind möglicherweise nicht in allen nationalen Clouds verfügbar. Um herauszufinden, welche Dienste verfügbar sind, wechseln Sie zu [Verfügbare Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=all&regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia,china-non-regional,china-east,china-east-2,china-north,china-north-2,germany-non-regional,germany-central,germany-northeast).

Wie Sie eine Anwendung mit Microsoft Identity Platform erstellen können, erfahren Sie im [Tutorial zur Microsoft Authentication Library (MSAL)](msal-national-cloud.md). Speziell mit dieser App wird ein Benutzer angemeldet und ein Zugriffstoken zum Aufrufen der Microsoft Graph-API abgerufen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen:

- [Azure Government](../../azure-government/index.yml)
- [Azure China 21Vianet](/azure/china/)
- [Azure Deutschland](../../germany/index.yml)
- [Azure AD-Authentifizierungsgrundlagen](./authentication-vs-authorization.md)