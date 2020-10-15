---
title: 'Azure AD Connect-Bereitstellungs-Agent: Versionsverlauf | Microsoft-Dokumentation'
description: In diesem Artikel werden alle Versionen des Azure AD Connect-Bereitstellungs-Agent aufgeführt sowie neue Features und behobene Probleme beschrieben.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.topic: reference
ms.workload: identity
ms.date: 02/26/2020
ms.subservice: app-provisioning
ms.author: kenwith
ms.reviewer: celested
ms.openlocfilehash: 9e05d1a85f17800ddf4d77e4e4acba6396a8da47
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84781564"
---
# <a name="azure-ad-connect-provisioning-agent-version-release-history"></a>Azure AD Connect-Bereitstellungs-Agent: Verlauf der Versionsveröffentlichungen

In diesem Artikel werden die veröffentlichten Versionen und Features des Azure Active Directory Connect-Bereitstellungs-Agent aufgeführt. Das Azure AD-Team aktualisiert den Bereitstellungs-Agent regelmäßig mit neuen Features und Funktionen. Der Bereitstellungs-Agent wird automatisch aktualisiert, wenn eine neue Version veröffentlicht wird. 

Microsoft stellt direkte Unterstützung für die neueste Agent-Version und die unmittelbare Vorgängerversion bereit.

## <a name="11960"></a>1.1.96.0

### <a name="release-status"></a>Releasestatus

4\. Dezember 2019 Für den Download veröffentlicht

### <a name="new-features-and-improvements"></a>Neue Features und Verbesserungen

* Umfasst die Unterstützung der Synchronisierung von Benutzer-, Kontakt- und Gruppendaten auf dem lokalen Active Directory mit Azure AD für die [Azure AD Connect-Cloudbereitstellung](../cloud-provisioning/what-is-cloud-provisioning.md).


## <a name="11670"></a>1.1.67.0

### <a name="release-status"></a>Releasestatus

9\. September 2019: Veröffentlichung für automatisches Update

### <a name="new-features-and-improvements"></a>Neue Features und Verbesserungen

* Möglichkeit zum Konfigurieren zusätzlicher Ablaufverfolgung und Protokollierung für das Debuggen des Bereitstellungs-Agent
* Möglichkeit zum Abrufen nur jener Azure AD-Attribute, die in der Zuordnung konfiguriert sind, um die Synchronisierungsleistung zu verbessern

### <a name="fixed-issues"></a>Behobene Probleme

* Ein Fehler wurde behoben, aufgrund dessen der Agent nicht mehr reagierte, wenn Probleme durch Azure AD-Verbindungsfehler aufgetreten sind.
* Ein Fehler wurde behoben, der beim Lesen von Binärdaten aus Azure Active Directory Probleme verursacht hat.
* Ein Fehler wurde behoben, aufgrund dessen Agent die Vertrauensstellung mit dem Cloud-Hybrididentitätsdienst nicht erneuern konnte.

## <a name="11300"></a>1.1.30.0

### <a name="release-status"></a>Releasestatus

23. Januar 2019: Für den Download veröffentlicht

### <a name="new-features-and-improvements"></a>Neue Features und Verbesserungen

* Bereitstellungs-Agent und Connector-Architektur wurden für bessere Leistung, Stabilität und Zuverlässigkeit überarbeitet 
* Die Konfiguration des Bereitstellungs-Agent wurde durch Verwendung eines Installationsassistenten auf der Benutzeroberfläche vereinfacht 
* Unterstützung für automatische Agent-Updates hinzugefügt

