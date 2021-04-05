---
title: Referenz zum App-Registrierungsportal | Azure
titleSuffix: Microsoft identity platform
description: Eine Beschreibung der Funktionen im App-Registrierungsportal von Microsoft.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 08/13/2019
ms.author: ryanwi
ms.reviewer: lenalepa
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: 6a33da602eaa9bee20f155eaa550e558e5dcbeca
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98755554"
---
# <a name="app-registration-reference"></a>Referenz zur App-Registrierung

Dieses Dokument enthält Kontext und Beschreibungen zu verschiedenen Features der [App-Registrierungen](https://aka.ms/appregistrations) im Azure-Portal.

## <a name="my-applications-or-converged-applications"></a>Eigene Anwendungen oder konvergente Anwendungen

Diese Liste enthält all Ihre Anwendungen, die für die Verwendung mit Microsoft Identity Platform registriert sind. Diese Anwendungen können Benutzer sowohl über persönliche Microsoft-Konten als auch über Geschäfts-, Schul- oder Unikonten von Azure Active Directory anmelden. Weitere Informationen zu Microsoft Identiy Platform finden Sie in der [Übersicht über Microsoft Identity Platform v2.0](./v2-overview.md). Diese Anwendungen können auch für die Integration in den Microsoft-Konto-Authentifizierungsendpunkt, `https://login.live.com`, verwendet werden.

## <a name="azure-ad-only-applications"></a>Nur Azure AD-Anwendungen

Diese Liste enthält all Ihre Anwendungen, die für die Verwendung mit dem Azure AD v1.0-Endpunkt registriert sind. Bei diesen Anwendungen kann die Benutzeranmeldung nur mithilfe von Geschäfts-, Schul- oder Unikonten aus Azure Active Directory erfolgen. Diese Liste enthält Anwendungen, die über den Abschnitt **App-Registrierungen** im [Azure-Portal](https://portal.azure.com) registriert wurden.

## <a name="live-sdk-applications"></a>Live SDK-Anwendungen

Diese Liste enthält all Ihre Anwendungen, die für die ausschließliche Verwendung mit dem Microsoft-Konto registriert sind. Sie können nicht mit Azure Active Directory verwendet werden. Hier finden Sie alle Anwendungen, die zuvor beim MSA-Entwicklerportal unter `https://account.live.com/developers/applications` registriert wurden. Alle Funktionen, die Sie zuvor unter `https://account.live.com/developers/applications` ausgeführt haben, können jetzt in [App-Registrierungen](https://aka.ms/appregistrations) ausgeführt werden.

## <a name="application-secrets"></a>Geheime Schlüssel für Anwendungen

Anwendungsgeheimnisse sind Anmeldeinformationen, mit denen Ihre Anwendung eine zuverlässige [Clientauthentifizierung](https://tools.ietf.org/html/rfc6749#section-2.3) bei Microsoft Identity Platform durchführen kann. In OAuth und OpenID Connect werden Anwendungsgeheimnisse gemeinhin als `client_secret` bezeichnet. Im v2.0-Protokoll muss jede Anwendung, die an einem adressierbaren Webspeicherort (nach `https`-Schema) ein Sicherheitstoken empfängt, ein Anwendungsgeheimnis verwenden, um sich beim Einlösen dieses Sicherheitstokens bei Microsoft Identity Platform zu identifizieren. Darüber hinaus wird jedem nativen Client, der auf einem Gerät Token empfängt, die Verwendung eines Anwendungsgeheimnisses zur Clientauthentifizierung untersagt. Dadurch wird die Speicherung von Geheimnissen in unsicheren Umgebungen verhindert.

Jede App kann immer nur zwei gültige Anwendungsgeheimnisse enthalten. Durch das Verwalten von zwei Geheimnissen haben Sie die Möglichkeit, in der gesamten Umgebung Ihrer Anwendung regelmäßige Schlüsselrollover durchzuführen. Nachdem Sie Ihre Anwendung vollständig zu einem neuen Geheimnis migriert haben, können Sie das alte Geheimnis löschen und ein neues bereitstellen.

Zurzeit sind nur zwei Arten von Anwendungsgeheimnissen im App-Registrierungsportal zulässig. Wenn Sie **Neues Kennwort generieren** auswählen, wird ein gemeinsames Geheimnis im entsprechenden Datenspeicher generiert und gespeichert, das Sie in Ihrer Anwendung verwenden können. Wenn Sie **Neues Schlüsselpaar generieren** auswählen, wird ein neues Paar aus öffentlichem und privatem Schlüssel erstellt, das heruntergeladen und für die Clientauthentifizierung bei Microsoft Identity Platform verwendet werden kann. Durch die Auswahl von **Öffentlichen Schlüssel hochladen** können Sie ein eigenes Paar aus öffentlichem und privatem Schlüssel verwenden.
Sie müssen ein Zertifikat hochladen, das einen öffentlichen Schlüssel enthält.

## <a name="profile"></a>Profil

Im Profilabschnitt des App-Registrierungsportals können Sie die Anmeldeseite für Ihre Anwendung anpassen. Zurzeit können Sie auf der Anmeldeseite das Anwendungslogo, die URL zu den Nutzungsbedingungen und die URL zur Datenschutzerklärung ändern. Das Logo muss ein transparentes Bild im Format 48 x 48 oder 50 x 50 Pixel in einer GIF-, PNG- oder JPEG-Datei von höchstens 15 KB sein. Ändern Sie die Werte nach Belieben, und sehen Sie sich das Ergebnis auf der Anmeldeseite an.

## <a name="live-sdk-support"></a>Live SDK-Unterstützung

Wenn Sie „Live SDK-Unterstützung“ aktivieren, werden alle von Ihnen erstellten geheimen Schlüssel für Anwendungen sowohl im Azure AD- als auch im Microsoft-Konto-Datenspeicher bereitgestellt. Dadurch kann Ihre Anwendung direkt in den Microsoft-Kontodienst (login.live.com) integriert werden. Wenn Sie eine App direkt über das Microsoft-Konto (und nicht über den v2.0-Endpunkt) erstellen möchten, sollten Sie sicherstellen, dass die Live SDK-Unterstützung aktiviert ist.

Wenn Sie die Live SDK-Unterstützung deaktivieren, wird das Anwendungsgeheimnis nur in den Azure AD-Datenspeicher geschrieben. Der Azure AD-Datenspeicher umfasst Vorschriften auf Unternehmensebene, die das Einhalten bestimmter Standards wie z. B. FISMA ermöglichen. Wenn Sie die Live SDK-Unterstützung aktivieren, kann Ihre Anwendung eventuell keine Kompatibilität mit einigen dieser Standards erzielen.

Wenn Sie ausschließlich den v2.0-Endpunkt verwenden möchten, können Sie die Live SDK-Unterstützung problemlos deaktivieren.