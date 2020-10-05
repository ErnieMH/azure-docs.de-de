---
title: Entwerfen von Hybrididentitäten – Anforderungen für die mehrstufige Authentifizierung in Azure | Microsoft-Dokumentation
description: Mit der Steuerung für bedingten Zugriff überprüft Azure AD die besonderen Bedingungen, die Sie beim Authentifizieren des Benutzers und vor dem Gewähren des Zugriffs auf die Anwendung auswählen.
documentationcenter: ''
services: active-directory
author: billmath
manager: billmath
editor: ''
ms.assetid: 9c59fda9-47d0-4c7e-b3e7-3575c29beabe
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7d8ddf372e234bab242e4b28ba53dce7dd68cc89
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90976059"
---
# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>Ermitteln der Anforderungen an die Multi-Factor Authentication für Ihre Hybrid-Identitätslösung
In dieser Welt mit ihrem hohen Mobilitätsgrad, in der Benutzer mit allen Geräten auf Daten und Anwendungen in der Cloud zugreifen können, ist der Schutz dieser Daten zu einer sehr wichtigen Aufgabe geworden.  Jeden Tag kann man neue Artikel über Sicherheitsverletzungen lesen.  Es gibt zwar keinen absoluten Schutz vor diesen Sicherheitsverletzungen, aber die Multi-Factor Authentication bietet eine zusätzliche Sicherheitsebene als Schutz vor Verletzungen dieser Art.
Beginnen Sie, indem Sie die Anforderungen des Unternehmens in Bezug auf die Multi-Factor Authentication auswerten. Hierbei geht es um die Daten, die vom Unternehmen geschützt werden sollen.  Diese Auswertung ist wichtig, um die technischen Anforderungen zum Einrichten und Aktivieren der Unternehmensbenutzer für die Multi-Factor Authentication zu definieren.

Beantworten Sie die folgenden Fragen:

* Sollen in Ihrem Unternehmen Microsoft-Apps geschützt werden? 
* Wie werden diese Apps veröffentlicht?
* Wird von Ihrem Unternehmen der Remotezugriff bereitgestellt, damit die Mitarbeiter auf lokale Apps zugreifen können?

Wenn ja: Welche Art von Remotezugriff? Außerdem müssen Sie auswerten, wo sich die Benutzer befinden sollen, die auf diese Anwendungen zugreifen. Diese Auswertung ist ein weiterer wichtiger Schritt zur Definition der Multi-Factor Authentication-Strategie. Beantworten Sie die folgenden Fragen:

* Wo werden sich die Benutzer befinden?
* Können Sie sich an beliebigen Orten befinden?
* Möchte Ihr Unternehmen Einschränkungen in Bezug auf den Standort des Benutzers festlegen?

Nachdem Sie diese Anforderungen verstanden haben, ist es wichtig, auch die Anforderungen des Benutzers an die Multi-Factor Authentication auszuwerten. Diese Auswertung ist wichtig, weil hierbei die Anforderungen für die Einführung der Multi-Factor Authentication festgelegt werden. Beantworten Sie die folgenden Fragen:

* Sind die Benutzer mit der Multi-Factor Authentication vertraut?
* Ist für einige Verwendungen eine zusätzliche Authentifizierung erforderlich?  
  * Wenn ja: Gilt dies immer, für den Eingang aus externen Netzwerken, beim Zugreifen auf bestimmte Anwendungen oder unter anderen Bedingungen?
* Müssen die Benutzer darin geschult werden, wie die Multi-Factor Authentication eingerichtet und implementiert wird?
* Für welche wichtigen Szenarien möchte Ihr Unternehmen die Multi-Factor Authentication für die Benutzer ermöglichen?

Nach der Beantwortung der obigen Fragen wissen Sie, ob lokal bereits eine Multi-Factor Authentication implementiert wurde. Diese Auswertung ist wichtig, um die technischen Anforderungen zum Einrichten und Aktivieren der Unternehmensbenutzer für die Multi-Factor Authentication zu definieren. Beantworten Sie die folgenden Fragen:

* Müssen in Ihrem Unternehmen privilegierte Konten per MFA geschützt werden?
* Muss Ihr Unternehmen MFA für bestimmte Anwendungen aus Compliance-Gründen aktivieren?
* Muss Ihr Unternehmen MFA für alle betroffenen Benutzer dieser Anwendungen oder nur für Administratoren aktivieren?
* Muss MFA immer aktiviert sein, oder ist dies nur der Fall, wenn die Benutzer von außerhalb Ihres Unternehmensnetzwerks angemeldet sind?

## <a name="next-steps"></a>Nächste Schritte
[Definieren einer Strategie zur Hybrididentitätsübernahme](plan-hybrid-identity-design-considerations-identity-adoption-strategy.md)

## <a name="see-also"></a>Weitere Informationen
[Überlegungen zum Entwurf – Übersicht](plan-hybrid-identity-design-considerations-overview.md)

