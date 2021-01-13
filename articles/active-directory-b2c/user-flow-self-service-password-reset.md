---
title: Self-Service-Kennwortzurücksetzung in Azure Active Directory B2C | Microsoft-Dokumentation
description: Veranschaulicht das Einrichten der Self-Service-Kennwortzurücksetzung für Ihre Kunden in Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 07/30/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 0c9edaf3356ea4c1a521a89f2ec60a4b6ba1a5ef
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87481494"
---
# <a name="set-up-self-service-password-reset-for-your-customers"></a>Einrichten der Self-Service-Kennwortzurücksetzung für Ihre Kunden

Das Feature für die Self-Service-Kennwortzurücksetzung ermöglicht Ihren Kunden, die für lokale Konten registriert sind, das eigenständige Zurücksetzen ihrer Kennwörter. Dadurch wird die Belastung für Ihre Supportmitarbeiter erheblich reduziert, insbesondere, wenn Ihre Anwendung Millionen von Kunden besitzt, die sie regelmäßig verwenden. Derzeit wird nur die Verwendung einer verifizierten E-Mail-Adresse als Wiederherstellungsmethode unterstützt.

> [!NOTE]
> Dieser Artikel gilt für die Self-Service-Kennwortzurücksetzung im Kontext des Standardbenutzerflows **Anmeldung**, bei dem **Anmeldung mit lokalem Konto** als Identitätsanbieter verwendet wird. Informationen zu vollständig anpassbaren Benutzerflows zur Kennwortzurücksetzung, die in Ihrer App aufgerufen werden, finden Sie in [diesem Artikel](user-flow-overview.md).
>
>

Standardmäßig ist für Ihr Verzeichnis die Self-Service-Kennwortzurücksetzung nicht aktiviert. Verwenden Sie die folgenden Schritte zum Aktivieren der Funktion:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) als Abonnementadministrator an. Dies ist dasselbe Geschäfts-, Schul- oder Unikonto bzw. dasselbe Microsoft-Konto, mit dem Sie Ihr Verzeichnis erstellt haben.
2. Öffnen Sie **Azure Active Directory** (auf der Navigationsleiste auf der linken Seite).
3. Scrollen Sie auf dem Blatt "Optionen" nach unten, und wählen Sie **Kennwortzurücksetzung** aus.
4. Legen Sie **Self-Service-Kennwortzurücksetzung aktiviert** auf **Alle** fest.
5. Klicken Sie oben auf der Seite auf **Speichern**. Sie haben es geschafft!

Zum Testen verwenden Sie das Feature „Jetzt ausführen“ für einen beliebigen Benutzerflow für die Anmeldung mit lokalen Konten als Identitätsanbieter. Klicken Sie auf der lokalen Kontoanmeldeseite (auf der Sie eine E-Mail-Adresse und ein Kennwort oder einen Benutzernamen und ein Kennwort eingeben) auf **Sie können nicht auf Ihr Konto zugreifen?** , um den Vorgang zu überprüfen.

> [!NOTE]
> Die Seiten der Self-Service-Kennwortrücksetzung können mithilfe des [Features für Unternehmensbranding](../active-directory/fundamentals/customize-branding.md)angepasst werden.
>
>

