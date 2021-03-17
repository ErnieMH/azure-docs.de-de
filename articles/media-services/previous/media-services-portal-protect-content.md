---
title: Konfigurieren von Content Protection-Richtlinien über das Azure-Portal | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie mithilfe des Azure-Portals Content Protection-Richtlinien konfigurieren. Außerdem erfahren Sie, wie Sie die dynamische Verschlüsselung für Ihre Assets aktivieren.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: 270b3272-7411-40a9-ad42-5acdbba31154
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2021
ms.author: inhenkel
ms.openlocfilehash: c6620158965271ba99f80c2fbd36504fdbea7ecf
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "103010778"
---
# <a name="configure-content-protection-policies-by-using-the-azure-portal"></a>Konfigurieren von Content Protection-Richtlinien über das Azure-Portal

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

> [!NOTE]
> Um dieses Tutorial abzuschließen, benötigen Sie ein Azure-Konto. Ausführliche Informationen finden Sie unter [Einen Monat kostenlos testen](https://azure.microsoft.com/pricing/free-trial/).   > Media Services v2 werden derzeit keine neuen Features oder Funktionen hinzugefügt. <br/>Sehen Sie sich die neuste Version – [Media Services v3](../latest/index.yml) – an. Lesen Sie außerdem die [Hinweise zur Migration von v2 zu v3](../latest/migrate-v-2-v-3-migration-introduction.md).
>

 Mit Azure Media Services können Sie Ihre Medien ab dem Zeitpunkt sichern, an dem sie Ihren Computer durch Speicherung, Verarbeitung und Übermittlung verlassen. Mit Media Services können Sie Inhalte dynamisch mit AES (Advanced Encryption Standard) unter Verwendung von 128-Bit-Verschlüsselungsschlüsseln verschlüsselt übermitteln. Sie können es auch mit herkömmlicher Verschlüsselung (CENC) verwenden, indem Sie PlayReady und/oder Widevine Digital Rights Management (DRM) und Apple FairPlay verwenden. 

Media Services bietet einen Dienst zum Übermitteln von DRM-Lizenzen und unverschlüsselten AES-Schlüsseln an autorisierte Clients. Über das Azure-Portal können Sie eine einzelne Schlüssel-/Lizenzautorisierungsrichtlinie für alle Arten von Verschlüsselungen erstellen.

In diesem Artikel erfahren Sie, wie Sie eine Content Protection-Richtlinie über das Portal konfigurieren. Außerdem erfahren Sie, wie Sie die dynamische Verschlüsselung auf Ihre Assets anwenden.

## <a name="start-to-configure-content-protection"></a>Starten der Konfiguration von Content Protection
Führen Sie die folgenden Schritte aus, um Content Protection global über das Portal mithilfe Ihres Media Services-Kontos zu konfigurieren:

1. Wählen Sie im [Portal](https://portal.azure.com/) Ihr Media Services-Konto aus.

1. Wählen Sie **Einstellungen** > **Inhaltsschutz** aus.

    ![Inhaltsschutz](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a>Schlüssel-/Lizenzautorisierungsrichtlinie
Media Services unterstützt mehrere Methoden zur Authentifizierung von Benutzern, die Schlüssel oder Lizenzen anfordern. Sie müssen die Autorisierungsrichtlinie für den Inhaltsschlüssel konfigurieren. Ihr Client muss die Richtlinie erfüllen, bevor der Schlüssel bzw. die Lizenz an ihn übermittelt werden kann. Die Autorisierungsrichtlinie für Inhaltsschlüssel kann eine oder mehrere Autorisierungseinschränkungen aufweisen: offen oder Token.

Über das Portal können Sie eine einzelne Schlüssel-/Lizenzautorisierungsrichtlinie für alle Arten von Verschlüsselungen erstellen.

### <a name="open-authorization"></a>Offene Autorisierung
Bei Verwendung einer offenen Einschränkung übermittelt das System den Schlüssel an jeden Benutzer, der einen Schlüssel anfordert. Diese Einschränkung kann beim Testen hilfreich sein. 

### <a name="token-authorization"></a>Tokenautorisierung
Eine durch Token eingeschränkte Richtlinie gilt nur zusammen mit einem Token, das von einem Sicherheitstokendienst (Security Token Service, STS) ausgestellt wurde. Media Services unterstützt Token im Format „Simple Web Token“ (SWT) und „JSON Web Token“ (JWT). Media Services stellt keinen Sicherheitstokendienst (STS) bereit. Sie können einen benutzerdefinierten Sicherheitstokendienst erstellen oder Azure Access Control Service zum Ausstellen von Token verwenden. Der STS muss für die Erstellung eines mit dem angegebenen Schlüssel signierten Tokens und die Ausstellungsansprüche konfiguriert sein, die Sie in der Konfiguration der Tokeneinschränkung angegeben haben. Wenn das Token gültig ist und die Ansprüche im Token mit den für den Schlüssel (oder die Lizenz) konfigurierten Ansprüchen übereinstimmen, gibt der Schlüsselübermittlungsdienst von Media Services den angeforderten Schlüssel (oder die Lizenz) an den Client zurück.

Bei der Konfiguration der Richtlinie mit Tokeneinschränkung müssen die Parameter für den primären Verifizierungsschlüssel (primary verification key), den Aussteller (issuer) und die Zielgruppe (audience) angegeben werden. Der primäre Verifizierungsschlüssel enthält den Schlüssel, mit dem das Token signiert wurde. Der Aussteller ist der Sicherheitstokendienst, der das Token ausstellt. Audience (manchmal auch Scope) beschreibt den Verwendungszweck des Tokens oder die Ressource, auf die durch das Token Zugriff gewährt wird. Der Schlüsselübermittlungsdienst von Media Services überprüft, ob die Werte im Token mit den Werten in der Vorlage übereinstimmen.

![Schlüssel-/Lizenzautorisierungsrichtlinie](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-license-template"></a>PlayReady-Lizenzvorlage
Die PlayReady-Lizenzvorlage legt die Funktionen fest, die in Ihrer PlayReady-Lizenz aktiviert sind. Weitere Informationen zur PlayReady-Lizenzvorlage finden Sie unter [Media Services PlayReady-Lizenzvorlage – Übersicht](media-services-playready-license-template-overview.md).

### <a name="nonpersistent"></a>Nicht beständig
Wenn Sie eine Lizenz als nicht beständig konfigurieren, bleibt sie nur für die Dauer der Verwendung durch den Player im Arbeitsspeicher.  

![Nicht beständiger Inhaltsschutz](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Beständig
Wenn Sie die Lizenz als beständig konfigurieren, wird sie auf dem Client im beständigen Speicher gespeichert.

![Beständiger Inhaltsschutz](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-license-template"></a>Widevine-Lizenzvorlage
Die Widevine-Lizenzvorlage legt die Funktionen fest, die für Ihre Widevine-Lizenzen aktiviert sind.

### <a name="basic"></a>Basic
Bei Verwendung der Option **Basic** wird die Vorlage mit allen Standardwerten erstellt.

### <a name="advanced"></a>Fortgeschrittene
Weitere Informationen zur Widevine-Rechtevorlage finden Sie in der [Übersicht über die Widevine-Lizenzvorlage](media-services-widevine-license-template-overview.md).

![Erweiterter Inhaltsschutz](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>FairPlay-Konfiguration
Wählen Sie zum Aktivieren der FairPlay-Verschlüsselung **FairPlay-Konfiguration** aus. Wählen Sie dann das **App-Zertifikat** aus, und geben Sie den **geheimen Anwendungsschlüssel** ein. Weitere Informationen zur FairPlay-Konfiguration und zu Anforderungen finden Sie unter [Schützen von HLS-Inhalten mit Apple FairPlay oder Microsoft PlayReady](media-services-protect-hls-with-FairPlay.md).

![FairPlay-Konfiguration](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Anwenden der dynamischen Verschlüsselung auf Ihr Asset
Um die dynamische Verschlüsselung nutzen zu können, codieren Sie die Quelldatei in einen Satz von MP4-Dateien mit adaptiver Bitrate.

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Auswählen eines zu verschlüsselnden Assets
Wählen Sie **Einstellungen** > **Medienobjekte** aus, um alle Ihre Medienobjekte anzuzeigen.

![Option „Medienobjekte“](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>Verschlüsseln mit AES oder DRM
Wenn Sie für ein Medienobjekt die Option **Verschlüsseln** auswählen, werden zwei Optionen angezeigt: **AES** oder **DRM**. 

#### <a name="aes"></a>AES
Die AES-Verschlüsselung mit unverschlüsseltem Schlüssel ist für alle Streamingprotokolle aktiviert: Smooth Streaming, HLS und MPEG-DASH.

![Verschlüsselungskonfiguration](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM
1. Nachdem Sie **DRM** ausgewählt haben, werden verschiedene Content Protection-Richtlinien (die an dieser Stelle konfiguriert werden müssen) und eine Reihe von Streamingprotokollen angezeigt:

    a. **PlayReady and Widevine with MPEG-DASH** (PlayReady und Widevine mit MPEG-DASH): Verschlüsselt Ihren MPEG-DASH-Datenstrom dynamisch mit PlayReady- und Widevine-DRMs.

    b. **PlayReady and Widevine with MPEG-DASH + FairPlay with HLS** (PlayReady und Widevine mit MPEG-DASH + FairPlay mit HLS): Verschlüsselt Ihren MPEG-DASH-Datenstrom dynamisch mit PlayReady- und Widevine-DRMs. Bei dieser Option werden Ihre HLS-Datenströme auch mit FairPlay verschlüsselt.

    c. **PlayReady only with Smooth Streaming, HLS and MPEG-DASH** (Nur PlayReady mit Smooth Streaming, HLS und MPEG-DASH): Verschlüsselt Smooth Streaming-, HLS- und MPEG-DASH-Datenströme dynamisch mit PlayReady-DRM.

    d. **Widevine only with MPEG-DASH** (Nur Widevine mit MPEG-DASH): Verschlüsselt Ihren MPEG-DASH-Datenstrom dynamisch mit Widevine-DRM.
    
    e. **FairPlay only with HLS** (Nur FairPlay mit HLS): Verschlüsselt Ihren HLS-Datenstrom dynamisch mit FairPlay.

1. Wählen Sie zum Aktivieren der FairPlay-Verschlüsselung auf dem Blatt **Content Protection Global Settings** (Globale Content Protection-Einstellungen) die Option **FairPlay configuration** (FairPlay-Konfiguration) aus. Wählen Sie dann das **App-Zertifikat** aus, und geben Sie den **geheimen Anwendungsschlüssel** ein.

    ![Verschlüsselungstyp](./media/media-services-portal-content-protection/media-services-content-protection009.png)

1. Wählen Sie nach dem Auswählen der Verschlüsselung die Option **Übernehmen** aus.

>[!NOTE] 
>Wenn Sie beabsichtigen, HLS mit AES-Verschlüsselung in Safari abzuspielen, lesen Sie den Blogbeitrag zu [Verschlüsseltes HLS in Safari](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

## <a name="additional-notes"></a>Zusätzliche Hinweise

* Widevine ist ein von Google Inc. bereitgestellter Dienst, der den Vertragsbedingungen und der Datenschutzrichtlinie von Google, Inc. unterliegt.

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
