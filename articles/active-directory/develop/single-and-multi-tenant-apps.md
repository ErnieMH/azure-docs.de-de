---
title: Einzel- und mehrinstanzenfähige Apps in Azure AD
titleSuffix: Microsoft identity platform
description: Erfahren Sie mehr über die Features und Unterschiede zwischen einzel- und mehrinstanzenfähige Apps in Azure AD.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/24/2020
ms.author: ryanwi
ms.reviewer: justhu
ms.custom: aaddev
ms.openlocfilehash: 0868d87d977b15a552b04d5dbd6d19de6931f0ae
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91395905"
---
# <a name="tenancy-in-azure-active-directory"></a>Mandanten in Azure Active Directory

Azure Active Directory (Azure AD) organisiert Objekten wie Benutzer und Apps in Gruppen, die als *Mandanten* bezeichnet werden. Mandanten erlauben einem Administrator, Richtlinien für Benutzer innerhalb des Unternehmens und die Apps festzulegen, die dem Unternehmen gehören, um die Sicherheits- und Betriebsrichtlinien einzuhalten. 

## <a name="who-can-sign-in-to-your-app"></a>Wer kann sich bei Ihrer App anmelden?

Wenn es um die Entwicklung von Apps geht, können Entwickler bei der App-Registrierung im [Azure-Portal](https://portal.azure.com) auswählen, ob ihre App als einzel- oder mehrinstanzenfähige App konfiguriert werden soll.
* Einzelinstanzenfähige Apps sind nur in dem Mandanten verfügbar, in dem sie registriert wurden. Dieser wird auch als Basismandant bezeichnet.
* Mehrinstanzenfähige Apps sind für Benutzer in ihrem Basismandanten und in anderen Mandanten verfügbar.

Im Azure-Portal können Sie Ihre App als einzel- oder mehrinstanzenfähig konfigurieren, indem Sie die Zielgruppe wie folgt festlegen.

| Zielgruppe | Einzel-/mehrinstanzenfähig | Wer kann sich anmelden? | 
|----------|--------| ---------|
| Nur Konten in diesem Verzeichnis | Einzelner Mandant | Alle Benutzer- und Gastkonten in Ihrem Verzeichnis können Ihre Anwendung oder API verwenden.<br>*Verwenden Sie diese Option, wenn Ihre Zielgruppe sich innerhalb Ihrer Organisation befindet.* |
| Konten in einem beliebigen Azure AD-Verzeichnis | Mehrinstanzenfähig | Alle Benutzer und Gäste mit einem Geschäfts-, Schul- oder Unikonto von Microsoft können Ihre Anwendung oder API verwenden. Dazu gehören auch Bildungseinrichtungen und Unternehmen, die Microsoft 365 verwenden.<br>*Verwenden Sie diese Option, wenn Ihre Zielgruppe Kunden aus dem Unternehmens- oder Bildungsbereich sind.* |
| Konten in beliebigen Azure AD-Verzeichnissen und persönliche Microsoft-Konten (z.B. Skype, Xbox, Outlook.com) | Mehrinstanzenfähig | Alle Benutzer mit einem Geschäfts-, Schul- oder Unikonto bzw. einem persönlichen Microsoft-Konto können Ihre Anwendung oder API verwenden. Dazu gehören Bildungseinrichtungen und Unternehmen, die Microsoft 365 nutzen, sowie persönliche Konten, mit denen die Anmeldung bei Diensten wie Xbox und Skype erfolgt.<br>*Verwenden Sie diese Option, um die breiteste Auswahl an Microsoft-Konten als Zielgruppe zu verwenden.* | 

## <a name="best-practices-for-multi-tenant-apps"></a>Bewährte Methoden für mehrinstanzenfähige Apps

Das Erstellen guter mehrinstanzenfähiger Apps kann aufgrund der Vielzahl unterschiedlicher Richtlinien, die IT-Administratoren in ihren Mandanten festlegen können, eine Herausforderung darstellen. Wenn Sie eine mehrinstanzenfähige App erstellen möchten, wenden Sie die folgenden bewährten Methoden an:

* Testen Sie Ihre App in einem Mandanten, für den [Richtlinien für bedingten Zugriff](../azuread-dev/conditional-access-dev-guide.md) konfiguriert sind.
* Befolgen Sie das Prinzip des geringstmöglichen Benutzerzugriffs, um sicherzustellen, dass Ihre App nur Berechtigungen anfordert, die sie tatsächlich benötigt. 
* Geben Sie geeignete Namen und Beschreibungen für alle Berechtigungen an, die Sie als Teil Ihrer App bereitstellen. Auf diese Weise können sich Benutzer und Administratoren besser informieren, welchen Berechtigungen sie zustimmen, wenn sie versuchen, die APIs Ihrer App zu verwenden. Weitere Informationen finden Sie im Abschnitt zu den bewährten Methoden im [Berechtigungsleitfaden](v2-permissions-and-consent.md).

## <a name="next-steps"></a>Nächste Schritte

* [Konvertieren einer App in eine mehrinstanzenfähige App](howto-convert-app-to-be-multi-tenant.md)
