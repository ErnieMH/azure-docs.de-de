---
title: Erstellen einer Richtlinie für bedingten Zugriff – Azure Active Directory
description: Welche Optionen stehen für die Erstellung einer Richtlinie für bedingten Zugriff zur Verfügung, und was bedeuten Sie?
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8a79b046170a5a3f3574895490aa649fd02da082
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92016126"
---
# <a name="building-a-conditional-access-policy"></a>Erstellen einer Richtlinie für bedingten Zugriff

Wie im Artikel [Was ist bedingter Zugriff?](overview.md) erläutert, ist eine Richtlinie für bedingten Zugriff eine If-Then-Anweisung mit **Zuweisungen** und **Zugriffssteuerungen**. Eine Richtlinie für bedingten Zugriff führt Signale zusammen, um Entscheidungen zu treffen und Organisationsrichtlinien zu erzwingen.

Wie erstellt eine Organisation diese Richtlinien? Was ist erforderlich?

![Bedingter Zugriff (Signale + Entscheidungen + Erzwingung = Richtlinien)](./media/concept-conditional-access-policies/conditional-access-signal-decision-enforcement.png)

## <a name="assignments"></a>Zuweisungen

Im Teil „Zuweisungen“ wird das „Wer“, „Was“ und „Wo“ der Richtlinie für bedingten Zugriff gesteuert.

### <a name="users-and-groups"></a>Benutzer und Gruppen

[Benutzer und Gruppen](concept-conditional-access-users-groups.md) legen anhand der Zuweisungen fest, welche Personen in die Richtlinie einbezogen oder von der Richtlinie ausgeschlossen werden sollen. Diese Zuweisung kann alle Benutzer, bestimmte Benutzergruppen, Verzeichnisrollen oder externe Gastbenutzer einschließen. 

### <a name="cloud-apps-or-actions"></a>Cloud-Apps oder -aktionen

[Cloud-Apps oder -aktionen](concept-conditional-access-cloud-apps.md) können Cloudanwendungen oder Benutzeraktionen ein- oder ausschließen, die der Richtlinie unterliegen.

### <a name="conditions"></a>Bedingungen

Eine Richtlinie kann mehrere [Bedingungen](concept-conditional-access-conditions.md) enthalten.

#### <a name="sign-in-risk"></a>Anmelderisiko

Bei Organisationen mit [Azure AD Identity Protection](../identity-protection/overview-identity-protection.md) können die dort generierten Risikoerkennungen Ihre Richtlinien für den bedingten Zugriff beeinflussen.

#### <a name="device-platforms"></a>Geräteplattformen

Organisationen mit mehreren Betriebssystemplattformen für ihre Geräte möchten möglicherweise bestimmte Richtlinien auf unterschiedlichen Plattformen erzwingen. 

Die zum Berechnen der Geräteplattform verwendeten Informationen stammen aus nicht überprüften Quellen wie Benutzer-Agent-Zeichenfolgen, die geändert werden können.

#### <a name="locations"></a>Standorte

Standortdaten werden von IP-Geolocation-Daten bereitgestellt. Administratoren können bei Bedarf Standorte definieren und einige als vertrauenswürdig kennzeichnen (z.B. die Netzwerkstandorte ihrer Organisation).

#### <a name="client-apps"></a>Client-Apps

Standardmäßig gelten Richtlinien für bedingten Zugriff für Browser-Apps, mobile Apps und Desktopclients, die die moderne Authentifizierung unterstützen. 

Durch diese Zuweisungsbedingung können Richtlinien für bedingten Zugriff auf bestimmte Clientanwendungen abzielen, die keine moderne Authentifizierung verwenden. Zu diesen Anwendungen gehören Exchange ActiveSync-Clients, ältere Office-Anwendungen, die keine moderne Authentifizierung verwenden, sowie E-Mail-Protokolle wie IMAP, MAPI, POP und SMTP.

#### <a name="device-state"></a>Gerätestatus

Mit diesem Steuerelement können Geräte mit Hybrid-Azure AD-Einbindung oder Geräte, die in Intune als kompatibel gekennzeichnet sind, ausgeschlossen werden. Dieser Ausschluss kann zum Blockieren nicht verwalteter Geräte durchgeführt werden. 

## <a name="access-controls"></a>Zugriffssteuerung

Die Zugriffssteuerung der Richtlinie für bedingten Zugriff ist der Teil, der steuert, wie eine Richtlinie erzwungen wird.

### <a name="grant"></a>Erteilen

[Erteilen](concept-conditional-access-grant.md) bietet Administratoren eine Möglichkeit zur Richtlinienerzwingung, bei der sie den Zugriff blockieren oder gewähren können.

#### <a name="block-access"></a>Zugriff blockieren

„Zugriff blockieren“ bewirkt genau dies. Der Zugriff wird unter den angegebenen Zuweisungen blockiert. Das Blockieren des Zugriffs ist ein leistungsstarkes Steuerelement, das nur mit entsprechenden Kenntnissen eingesetzt werden sollte.

#### <a name="grant-access"></a>Gewähren von Zugriff

Das Steuerelement zum Gewähren des Zugriffs kann die Erzwingung von mindestens einem weiteren Steuerelementen auslösen. 

- Multi-Factor Authentication erforderlich
- Als kompatibel markierte Geräte (Intune) erforderlich
- Geräte mit Hybrid-Azure AD-Einbindung erforderlich
- Genehmigte Client-App erforderlich
- App-Schutzrichtlinie erforderlich

Administratoren können mithilfe der folgenden Optionen auswählen, ob eins der obigen Steuerelemente oder alle ausgewählten Steuerelemente erforderlich sind. Als Standardwert für mehrere Steuerelemente werden alle als erforderlich ausgewählt.

- Alle ausgewählten Steuerelemente erforderlich (Steuerelement UND Steuerelement)
- Eins der ausgewählten Steuerelemente erforderlich (Steuerelement ODER Steuerelement)

### <a name="session"></a>Sitzung

[Sitzungssteuerelemente](concept-conditional-access-session.md) können die Umgebung einschränken. 

- Verwenden von App-erzwungenen Einschränkungen
   - Funktioniert derzeit nur mit Exchange Online und SharePoint Online.
      - Übergibt Geräteinformationen, um das Gewähren von vollständigem oder eingeschränktem Zugriff auf die Umgebung zu steuern.
- App-Steuerung für bedingten Zugriff verwenden
   - Verwendet Signale von Microsoft Cloud App Security, um Folgendes auszuführen: 
      - Blockiert das Herunterladen, Ausschneiden, Kopieren und Drucken von sensiblen Dokumenten.
      - Überwacht das Verhalten riskanter Sitzungen.
      - Erzwingt das Bezeichnen von sensiblen Dateien.
- Anmeldehäufigkeit
   - Möglichkeit zum Ändern der standardmäßigen Anmeldehäufigkeit für die moderne Authentifizierung.
- Persistente Browsersitzung
   - Benutzer können angemeldet bleiben, nachdem sie ihr Browserfenster geschlossen und erneut geöffnet haben.

## <a name="simple-policies"></a>Einfache Richtlinien

Eine Richtlinie für bedingten Zugriff muss mindestens Folgendes enthalten, um erzwungen werden zu können:

- **Name** der Richtlinie.
- **Zuweisungen**
   - **Benutzer und/oder Gruppen**, auf die die Richtlinie angewendet werden soll.
   - **Cloud-Apps oder Aktionen**, auf die die Richtlinie angewendet werden soll.
- **Steuerelemente**
   - Steuerelemente für **Gewähren** oder **Blockieren**

![Leere Richtlinie für bedingten Zugriff](./media/concept-conditional-access-policies/conditional-access-blank-policy.png)

Der Artikel [Allgemeine Richtlinien für bedingten Zugriff](concept-conditional-access-policy-common.md) enthält einige Richtlinien, die für die meisten Organisationen nützlich sein könnten.

## <a name="next-steps"></a>Nächste Schritte

[Erstellen der Richtlinie für bedingten Zugriff](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-enable-azure-mfa?toc=/azure/active-directory/conditional-access/toc.json&bc=/azure/active-directory/conditional-access/breadcrumb/toc.json#create-a-conditional-access-policy)

[Simulieren des Anmeldeverhaltens mit dem Was-wäre-wenn-Tool für den bedingten Zugriff](troubleshoot-conditional-access-what-if.md)

[Planen einer cloudbasierten Azure Multi-Factor Authentication-Bereitstellung](../authentication/howto-mfa-getstarted.md)

[Verwalten der Gerätekonformität mit Intune](/intune/device-compliance-get-started)

[Microsoft Cloud App Security und bedingter Zugriff](/cloud-app-security/proxy-intro-aad)
