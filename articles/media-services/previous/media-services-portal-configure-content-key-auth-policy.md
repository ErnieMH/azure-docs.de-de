---
title: Konfigurieren einer Autorisierungsrichtlinie für Inhaltsschlüssel im Azure-Portal | Microsoft-Dokumentation
description: In diesem Artikel wird gezeigt, wie Sie eine Autorisierungsrichtlinie für einen Inhaltsschlüssel konfigurieren.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: ee82a3fa-c34b-48f2-a108-8ba321f1691e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2021
ms.author: inhenkel
ms.openlocfilehash: 68bd4bd2472e80a663294745368fb505767a230a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103008313"
---
# <a name="configure-a-content-key-authorization-policy"></a>Konfigurieren einer Autorisierungsrichtlinie für Inhaltsschlüssel

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

[!INCLUDE [media-services-selector-content-key-auth-policy](../../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a>Übersicht
 Mit Azure Media Services können Sie MPEG-DASH-, Smooth Streaming- und HTTP Live Streaming-Datenströme (HLS) übermitteln, die mit AES (Advanced Encryption Standard) mit 128-Bit-Verschlüsselungsschlüsseln oder [PlayReady DRM (Digital Rights Management)](https://www.microsoft.com/playready/overview/) geschützt sind. Media Services ermöglicht Ihnen auch, mit Widevine DRM verschlüsselte DASH-Streams zu übermitteln. PlayReady und Widevine sind gemäß der Common Encryption-Spezifikation (ISO/IEC 23001-7 CENC) verschlüsselt.

Media Services bietet einen Schlüssel-/Lizenzübermittlungsdienst, von dem die Clients AES-Schlüssel oder PlayReady/Widevine-Lizenzen zur Wiedergabe von verschlüsselten Inhalten abrufen können.

In diesem Artikel wird veranschaulicht, wie Sie das Azure-Portal zur Konfiguration der Autorisierungsrichtlinie für Inhaltsschlüssel verwenden. Der Schlüssel kann später verwendet werden, um Inhalte dynamisch zu verschlüsseln. Zur Zeit können Sie die Formate HLS, MPEG DASH und Smooth Streaming verschlüsseln. Progressive Downloads können nicht verschlüsselt werden.

Wenn ein Player einen Datenstrom anfordert, der für die dynamische Verschlüsselung konfiguriert ist, verwendet Media Services den konfigurierten Schlüssel, um Inhalte dynamisch mit der AES- oder DRM-Verschlüsselung zu verschlüsseln. Um den Stream zu entschlüsseln, fordert der Player den Schlüssel vom Schlüsselübermittlungsdienst an. Um zu ermitteln, ob der Benutzer berechtigt ist, den Schlüssel zu erhalten, wertet der Dienst die Autorisierungsrichtlinien aus, die Sie für den Schlüssel angegeben haben.

Wenn Sie mehrere Inhaltsschlüssel verwenden oder eine andere Schlüssel-/Lizenzübermittlungsdienst-URL als die des Schlüsselübermittlungsdiensts von Media Services angeben möchten, verwenden Sie das Media Services .NET-SDK oder REST-APIs. Weitere Informationen finden Sie unter

* [Konfigurieren einer Autorisierungsrichtlinie für Inhaltsschlüssel mit dem Media Services .NET-SDK](media-services-dotnet-configure-content-key-auth-policy.md)
* [Konfigurieren einer Autorisierungsrichtlinie für Inhaltsschlüssel mit der Media Services-REST-API](media-services-rest-configure-content-key-auth-policy.md)

### <a name="some-considerations-apply"></a>Zu berücksichtigende Aspekte
* Beim Erstellen Ihres Media Services-Kontos wird dem Konto ein Standard-Streamingendpunkt mit dem Status „Beendet“ hinzugefügt. Um mit dem Streamen der Inhalte zu beginnen und die dynamische Paketerstellung und dynamische Verschlüsselung zu nutzen, muss der Streamingendpunkt den Status „Wird ausgeführt“ aufweisen. 
* Ihr Medienobjekt muss einen Satz von MP4-Dateien bzw. Smooth Streaming-Dateien mit adaptiver Bitrate enthalten. Weitere Informationen finden Sie unter [Codieren von Medienobjekten](media-services-encode-asset.md).
* „ContentKeyAuthorizationPolicy“ und die zugehörigen Objekte (Richtlinienoptionen und Einschränkungen) werden vom Schlüsselübermittlungsdienst 15 Minuten lang zwischengespeichert. Sie können „ContentKeyAuthorizationPolicy“ erstellen und angeben, dass eine Tokeneinschränkung verwendet, getestet und anschließend die Richtlinie hinsichtlich der offenen Einschränkung aktualisiert werden soll. Dieser Vorgang dauert ungefähr 15 Minuten, bevor die Richtlinie zur offenen Version wechselt.
* Ein Media Services-Streamingendpunkt legt den Wert des CORS-Headers „Access-Control-Allow-Origin“ in der Preflightantwort als Platzhalter „\*“ fest. Dieser Wert funktioniert mit den meisten Playern gut, z.B Azure Media Player, Roku und JWPlayer sowie weitere. Einige Player, die dash.js nutzen, funktionieren jedoch nicht, da „XMLHttpRequest“ in ihrer dash.js mit dem Anmeldeinformationsmodus auf „include“ gesetzt ist und der Platzhalter „\*“ als Wert von „Access-Control-Allow-Origin“ nicht zugelassen wird. Als Problemumgehung für diese Einschränkung in dash.js kann Media Services, wenn Sie Ihren Client von einer einzigen Domäne aus hosten, diese Domäne im Preflightantwortheader angeben. Wenn Sie Hilfe benötigen, öffnen Sie ein Supportticket im Azure-Portal.

## <a name="configure-the-key-authorization-policy"></a>Konfigurieren der Schlüsselautorisierungsrichtlinie
Um die Schlüsselautorisierungsrichtlinie zu konfigurieren, wählen Sie die Seite **INHALTSSCHUTZ** aus.

Media Services unterstützt mehrere Möglichkeiten zur Authentifizierung von Benutzern, die Schlüssel anfordern. Autorisierungsrichtlinien für Inhaltsschlüssel können die Autorisierungseinschränkung „Open“, „Token“ oder „IP“ aufweisen. (IP kann mit REST oder dem .NET SDK konfiguriert werden.)

### <a name="open-restriction"></a>Open-Einschränkung
Bei Verwendung einer Open-Einschränkung übermittelt das System den Schlüssel an jeden, der einen Schlüssel anfordert. Diese Einschränkung kann zu Testzwecken nützlich sein.

![OpenPolicy][open_policy]

### <a name="token-restriction"></a>Token-Einschränkung
Zum Auswählen der durch „Token“ eingeschränkten Richtlinie wählen Sie die Schaltfläche **TOKEN** aus.

Eine durch Token eingeschränkte Richtlinie gilt nur zusammen mit einem Token, das von einem Sicherheitstokendienst (Security Token Service, STS) ausgestellt wurde. Media Services unterstützt Token im Format „Simple Web Token“ ([SWT](/previous-versions/azure/azure-services/gg185950(v=azure.100)#BKMK_2)) und „JSON Web Token“ (JWT). Weitere Informationen finden Sie unter [JWT-Authentifizierung](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Media Services stellt keinen Sicherheitstokendienst (STS) bereit. Sie können einen benutzerdefinierten STS zum Ausstellen von Token erstellen. Der STS muss für die Erstellung eines mit dem angegebenen Schlüssel signierten Tokens und die Ausstellungsansprüche konfiguriert sein, die Sie in der Konfiguration der Tokeneinschränkung angegeben haben. Der Schlüsselübermittlungsdienst von Media Services gibt den Verschlüsselungsschlüssel an den Client zurück, wenn das Token gültig ist und die Ansprüche im Token mit den für den Inhaltsschlüssel konfigurierten Ansprüchen übereinstimmen.

Bei der Konfiguration der Richtlinie mit Tokeneinschränkung müssen die Parameter für den primären Verifizierungsschlüssel (primary verification key), den Aussteller (issuer) und die Zielgruppe (audience) angegeben werden. Der primäre Verifizierungsschlüssel enthält den Schlüssel, mit dem das Token signiert wurde. Der Aussteller ist der Sicherheitstokendienst, der das Token ausstellt. Audience (manchmal auch Scope) beschreibt den Verwendungszweck des Tokens oder die Ressource, auf die durch das Token Zugriff gewährt wird. Der Schlüsselübermittlungsdienst von Media Services überprüft, ob die Werte im Token mit den Werten in der Vorlage übereinstimmen.

### <a name="playready"></a>PlayReady
Wenn Sie Inhalte mit PlayReady schützen, müssen Sie in Ihrer Autorisierungsrichtlinie u. a. eine XML-Zeichenfolge zur Definition der PlayReady-Lizenzvorlage angeben. Standardmäßig ist die folgende Richtlinie festgelegt:

```xml
<PlayReadyLicenseResponseTemplate xmlns:i="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
  <LicenseTemplates>
    <PlayReadyLicenseTemplate><AllowTestDevices>true</AllowTestDevices>
      <ContentKey i:type="ContentEncryptionKeyFromHeader" />
      <LicenseType>Nonpersistent</LicenseType>
      <PlayRight>
        <AllowPassingVideoContentToUnknownOutput>Allowed</AllowPassingVideoContentToUnknownOutput>
      </PlayRight>
    </PlayReadyLicenseTemplate>
  </LicenseTemplates>
</PlayReadyLicenseResponseTemplate>
```

Sie können die Schaltfläche **XML für Importrichtlinie** auswählen und einen anderen XML-Code angeben, der dem in [Media Services PlayReady-Lizenzvorlage – Übersicht](media-services-playready-license-template-overview.md) definierten XML-Schema entspricht.

## <a name="additional-notes"></a>Zusätzliche Hinweise

* Widevine ist ein von Google Inc. bereitgestellter Dienst, der den Vertragsbedingungen und der Datenschutzrichtlinie von Google, Inc. unterliegt.

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png
