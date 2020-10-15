---
title: Microsoft Azure Media Services-Szenarien und datencenterübergreifende Featureverfügbarkeit | Microsoft-Dokumentation
description: Dieses Thema enthält eine Übersicht über Microsoft Azure Media Services-Szenarien und über die datencenterübergreifende Verfügbarkeit von Features und Diensten.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 99604092ea901458062abd6423d74ad8f1f2c8d7
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92014408"
---
# <a name="scenarios-and-availability-of-media-services-features-across-datacenters"></a>Szenarien und datencenterübergreifende Verfügbarkeit von Media Services-Features

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

> [!NOTE]
> Media Services v2 werden derzeit keine neuen Features oder Funktionen hinzugefügt. <br/>Sehen Sie sich die neuste Version – [Media Services v3](../latest/index.yml) – an. Lesen Sie außerdem die [Hinweise zur Migration von v2 zu v3](../latest/migrate-from-v2-to-v3.md).

Mit Microsoft Azure Media Services (AMS) können Sie Video- oder Audioinhalte auf sichere Weise hochladen, speichern, codieren, verpacken und bedarfsgesteuert oder als Livestream auf verschiedenen Clients (wie Fernsehern, PCs und mobilen Geräten) bereitstellen.

AMS wird in mehreren Datencentern weltweit betrieben. Diese Rechenzentren sind in geografische Regionen unterteilt, sodass Sie auswählen können, wo Sie Ihre Anwendungen erstellen möchten. Sie können die [Liste der Regionen und Standorte](https://azure.microsoft.com/regions/)überprüfen. 

Dieses Thema enthält allgemeine Szenarien für die Übermittlung von Inhalten ([live](#live_scenarios) oder bedarfsgesteuert). Außerdem finden Sie hier Details zur datencenterübergreifenden Verfügbarkeit von Medienfeatures und -diensten.

## <a name="overview"></a>Übersicht

### <a name="prerequisites"></a>Voraussetzungen

Um mit Azure Media Services loszulegen, sollten Sie Folgendes haben:

* Ein Azure-Konto. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Einen Monat kostenlos testen](https://azure.microsoft.com).
* Ein Azure Media Services-Konto. Weitere Informationen finden Sie unter [Erstellen von Konten](media-services-portal-create-account.md).
* Der Streamingendpunkt, von dem aus Sie die Inhalte streamen möchten, muss sich im Status **Wird ausgeführt** befinden.

    Beim Erstellen Ihres AMS-Kontos wird dem Konto ein **Standard**-Streamingendpunkt mit dem Zustand **Beendet** hinzugefügt. Um mit dem Streamen der Inhalte zu beginnen und die dynamische Paketerstellung und dynamische Verschlüsselung zu nutzen, muss sich der Streamingendpunkt im Zustand **Wird ausgeführt** befinden.

### <a name="commonly-used-objects-when-developing-against-the-ams-odata-model"></a>Häufig verwendete Objekte bei der Entwicklung mit dem AMS-OData-Modell

Die folgende Abbildung zeigt einige der am häufigsten verwendeten Objekte beim Entwickeln mit dem Media Services OData-Modell.

Klicken Sie auf Bild, um es in voller Größe anzeigen.  

[![Diagramm mit einigen der am häufigsten verwendeten Objekte bei der Entwicklung anhand des Azure Media Services-Objektdatenmodells.](./media/media-services-overview/media-services-overview-object-model-small.png)](./media/media-services-overview/media-services-overview-object-model.png#lightbox)

Sie können das gesamte Modell [hier](https://media.windows.net/API/$metadata?api-version=2.15) anzeigen.  

## <a name="protect-content-in-storage-and-deliver-streaming-media-in-the-clear-non-encrypted"></a>Schützen von Inhalte im Speicher und Übermitteln von Streamingmedien ohne Verschlüsselung

![VoD-Workflow](./media/scenarios-and-availability/scenarios-and-availability01.png)

1. Laden Sie eine Mediendatei mit hoher Qualität in ein Medienobjekt hoch.

    Es wird empfohlen, eine Speicherverschlüsselung auf Medienobjekte anzuwenden, um Ihre Inhalte beim Hochladen und während der Speicherung zu schützen.
2. Codieren Sie das Medienobjekt in einen Satz von MP4-Dateien mit adaptiver Bitrate.

    Es wird empfohlen, eine Speicherverschlüsselung das Ausgabemedienobjekt anzuwenden, um Ihre Inhalte während der Speicherung zu schützen.
3. Konfigurieren Sie eine Übermittlungsrichtlinie für Medienobjekte (wird zur dynamischen Paketerstellung verwendet).

    Wenn Ihr Medienobjekt speicherverschlüsselt ist, **müssen** Sie die Übermittlungsrichtlinien für Medienobjekte konfigurieren.
4. Veröffentlichen Sie das Medienobjekt durch Erstellen eines OnDemand-Locators.
5. Streamen Sie die veröffentlichten Inhalte.

Weitere Informationen zur Verfügbarkeit in Datencentern finden Sie im Abschnitt [Verfügbarkeit](#availability).

## <a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a>Schützen von Inhalten im Speicher, Übermitteln dynamisch verschlüsselter Streamingmedien

![Schützen mit PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

1. Laden Sie eine Mediendatei mit hoher Qualität in ein Medienobjekt hoch. Wenden Sie die Speicherverschlüsselung auf das Medienobjekt an.
2. Codieren Sie das Medienobjekt in einen Satz von MP4-Dateien mit adaptiver Bitrate. Wenden Sie die Speicherverschlüsselung auf das Ausgabemedienobjekt an.
3. Erstellen Sie einen Inhaltsverschlüsselungsschlüssel für das Medienobjekt, das während der Wiedergabe dynamisch verschlüsselt werden soll.
4. Konfigurieren Sie Autorisierungsrichtlinien für Inhaltsschlüssel.
5. Konfigurieren Sie Übermittlungsrichtlinien für Medienobjekte (wird zur dynamischen Paketerstellung und zur dynamischen Verschlüsselung verwendet).
6. Veröffentlichen Sie das Medienobjekt durch Erstellen eines OnDemand-Locators.
7. Streamen Sie die veröffentlichten Inhalte.

Weitere Informationen zur Verfügbarkeit in Datencentern finden Sie im Abschnitt [Verfügbarkeit](#availability).

## <a name="use-media-analytics-to-derive-actionable-insights-from-your-videos"></a>Gewinnen nützlicher Erkenntnisse aus Ihren Videos mithilfe von Media Analytics

Media Analytics ist eine Sammlung aus Sprach- und Bildanalysekomponenten, mit denen Organisationen und Unternehmen anhand von Videodateien Erkenntnisse gewinnen, aus denen sich umsetzbare Maßnahmen ableiten lassen. Weitere Informationen finden Sie unter [Azure Media Services Analytics – Übersicht](./legacy-components.md).

1. Laden Sie eine Mediendatei mit hoher Qualität in ein Medienobjekt hoch.
2. Verarbeiten Sie Ihre Videos mit einem der Media Analytics-Dienste, die im Übersichtsabschnitt des Artikels [Media Analytics auf der Media Services-Plattform](./legacy-components.md) beschrieben werden.
3. Media Analytics-Medienprozessoren generieren MP4- oder JSON-Dateien. Wenn ein Medienprozessor eine MP4-Datei erstellt hat, können Sie die Datei progressiv herunterladen. Wenn ein Medienprozessor eine JSON-Datei erstellt hat, können Sie die Datei aus Azure Blob Storage herunterladen.

Weitere Informationen zur Verfügbarkeit in Datencentern finden Sie im Abschnitt [Verfügbarkeit](#availability).

## <a name="deliver-progressive-download"></a>Bereitstellen eines progressiven Downloads

1. Laden Sie eine Mediendatei mit hoher Qualität in ein Medienobjekt hoch.
2. Codieren Sie das Medienobjekt in eine einzelne MP4-Datei.
3. Veröffentlichen Sie das Medienobjekt durch Erstellen eines OnDemand- oder SAS-Locators.

    Wenn Sie einen SAS-Locator verwenden, wird der Inhalt aus Azure Blob Storage heruntergeladen. In diesem Fall müssen keine Streamingendpunkte im Status „Gestartet“ vorhanden sein.
4. Führen Sie einen progressiven Download der Inhalte durch.

## <a name="delivering-live-streaming-events"></a><a id="live_scenarios"></a>Bereitstellen von Livestreaming-Ereignissen 

1. Erfassen Sie Liveinhalte über verschiedene Livestreaming-Protokolle (wie etwa RTMP oder Smooth Streaming).
2. Optional: Codieren Sie Ihren Datenstrom als Datenstrom mit adaptiver Bitrate.
3. Zeigen Sie eine Vorschau Ihres Livestreams an.
4. Übermitteln Sie Inhalte über gängige Streamingprotokolle (wie MPEG DASH, Smooth oder HLS) direkt an die Kunden oder zur weiteren Verteilung an ein Content Delivery Network (CDN).

    Oder

    Zeichnen Sie die erfassten Inhalte auf, und speichern Sie sie, damit sie später gestreamt werden können (Video-on-Demand).

Beim Livestreaming haben Sie die Wahl zwischen folgenden Routen:

### <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Arbeiten mit Kanälen, die Livedatenströme mit Mehrfachbitraten von lokalen Encodern empfangen (Pass-Through)

Das folgende Diagramm zeigt die Hauptkomponenten der AMS-Plattform, die am **Pass-Through-Workflow** beteiligt sind:

![Liveworkflow](./media/scenarios-and-availability/media-services-live-streaming-current.png)

Weitere Informationen finden Sie unter [Arbeiten mit Kanälen, die Livedatenströme mit mehreren Bitraten von lokalen Encodern empfangen](media-services-live-streaming-with-onprem-encoders.md).

### <a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Arbeiten mit Kanälen, die zum Ausführen von Livecodierung mit Azure Media Services aktiviert wurden

Das folgende Diagramm zeigt die Hauptkomponenten der AMS-Plattform, die in Livestreaming-Workflows beteiligt sind, wenn ein Kanal zum Ausführen der Livecodierung mit Media Services aktiviert ist.

![Liveworkflow](./media/scenarios-and-availability/media-services-live-streaming-new.png)

Weitere Informationen finden Sie unter [Arbeiten mit Kanälen, die zum Ausführen von Live Encoding mit Azure Media Services aktiviert wurden](media-services-manage-live-encoder-enabled-channels.md).

Weitere Informationen zur Verfügbarkeit in Datencentern finden Sie im Abschnitt [Verfügbarkeit](#availability).

## <a name="consuming-content"></a>Nutzung von Inhalten

Azure Media Services stellt die Tools zur Verfügung, die zum Erstellen leistungsstarker, dynamischer Clientplayeranwendungen für die gängigsten Plattformen erforderlich sind, darunter: iOS-Geräte, Android-Geräte, Windows, Windows Phone, Xbox und Set-Top-Boxen. 

## <a name="enabling-azure-cdn"></a>Aktivieren von Azure CDN

Von Media Services wird die Integration mit Azure CDN unterstützt. Informationen zum Aktivieren von Azure CDN finden Sie unter [Verwalten von Streamingendpunkten in Media Services-Konten](media-services-portal-manage-streaming-endpoints.md).

## <a name="scaling-a-media-services-account"></a><a id="scaling"></a>Skalieren eines Media Services-Kontos

AMS-Kunden können Streamingendpunkte, Medienverarbeitung und Speicherung in ihren AMS-Konten skalieren.

* Media Services-Kunden haben die Wahl zwischen einem **Standard**-Streamingendpunkt und einem **Premium**-Streamingendpunkt. Ein **Standard**-Streamingendpunkt ist für die meisten Streamingworkloads geeignet. Er bietet die gleichen Features wie ein **Premium**-Streamingendpunkt und skaliert die ausgehende Bandbreite automatisch. 

    **Premium**-Streamingendpunkte eignen sich für komplexere Workloads und bieten eine dedizierte und skalierbare Bandbreitenkapazität. Kunden mit einem **Premium**-Streamingendpunkt erhalten standardmäßig eine einzelne Streamingeinheit (Streaming Unit, SU). Der Streamingendpunkt kann durch Hinzufügen von SUs skaliert werden. Jede SU stellt zusätzliche Bandbreitenkapazität für die Anwendung bereit. Weitere Informationen zur Skalierung von **Premium**-Streamingendpunkten finden Sie im Thema [Skalieren von Streamingendpunkten mithilfe des Azure-Portals](media-services-portal-scale-streaming-endpoints.md).

* Ein Media Services-Konto ist dem Typ einer reservierten Einheit zugeordnet, der die Geschwindigkeit bestimmt, mit der Medienverarbeitungsaufgaben erfolgen. Sie können zwischen den folgenden reservierten Einheitentypen wählen: **S1**, **S2** oder **S3**. Derselbe Codierungsauftrag wird bei Verwendung der reservierten Einheit **S2** beispielsweise schneller ausgeführt als mit dem Typ **S1**.

    Neben der Art der reservierten Einheit können Sie angeben, dass für Ihr Konto **reservierte Einheiten** (Reserved Units, RUs) bereitgestellt werden sollen. Die Anzahl bereitgestellter RUs bestimmt die Anzahl von Medienaufgaben, die gleichzeitig unter einem bestimmten Konto verarbeitet werden können.

    >[!NOTE]
    >RUs dienen der Parallelisierung der gesamten Medienverarbeitung, einschließlich der Indizierung von Aufträgen mit Azure Media Indexer. Allerdings erfolgt die Indizierung von Aufträgen im Gegensatz zur Codierung mit schnelleren reservierten Einheiten nicht schneller.

    Weitere Informationen finden Sie unter [Skalieren der Medienverarbeitung](media-services-portal-scale-media-processing.md).
* Außerdem können Sie Ihr Media Services-Konto skalieren, indem Sie Speicherkonten hinzufügen. Jedes Speicherkonto ist auf 500 TB beschränkt. Um den Speicher über die Standardbeschränkungen hinaus zu erweitern, können Sie mehrere Speicherkonten mit einem einzelnen Media Services-Konto verknüpfen. Weitere Informationen finden Sie unter [Verwalten von Media Services-Medienobjekten für mehrere Speicherkonten](./media-services-managing-multiple-storage-accounts.md).

## <a name="availability-of-media-services-features-across-datacenters"></a><a id="availability"></a> Datencenterübergreifende Verfügbarkeit von Media Services-Features

Dieser Abschnitt enthält Details zur datencenterübergreifenden Verfügbarkeit von Media Services-Features.

### <a name="ams-accounts"></a>AMS-Konten

#### <a name="availability"></a>Verfügbarkeit

Verwenden Sie [Azure-Produkte nach Region](https://azure.microsoft.com/global-infrastructure/services/?products=media-services&regions=all), um zu bestimmen, ob Media Services in einem bestimmten Rechenzentrum verfügbar ist.

### <a name="streaming-endpoints"></a>Streamingendpunkte 

Media Services-Kunden haben die Wahl zwischen einem **Standard**-Streamingendpunkt und einem **Premium**-Streamingendpunkt. Weitere Informationen finden Sie im [Abschnitt zur Skalierung](#scaling).

#### <a name="availability"></a>Verfügbarkeit

|Name|Status|Rechenzentren
|---|---|---|
|Standard|Allgemein verfügbar|All|
|Premium|Allgemein verfügbar|All|

### <a name="live-encoding"></a>Live Encoding

#### <a name="availability"></a>Verfügbarkeit

Verfügbar in allen Datencentern außer: „Deutschland“, „Brasilien, Süden“, „Indien, Westen“, „Indien, Süden“ und „Indien, Mitte“. 

### <a name="encoding-media-processors"></a>Medienprozessoren für die Codierung

AMS bietet zwei On-Demand-Encoder: **Media Encoder Standard** und **Media Encoder Premium Workflow**. Weitere Informationen finden Sie unter [Azure On-Demand Media Encoder – Überblick und Vergleich](media-services-encode-asset.md). 

#### <a name="availability"></a>Verfügbarkeit

|Medienprozessorname|Status|Rechenzentren
|---|---|---|
|Media Encoder Standard|Allgemein verfügbar|All|
|Media Encoder Premium Workflow|Allgemein verfügbar|Alle mit Ausnahme von China|

### <a name="analytics-media-processors"></a>Analytics-Medienprozessoren

Media Analytics ist eine Sammlung aus Sprach- und Bildanalysekomponenten, mit denen Organisationen und Unternehmen anhand von Videodateien Erkenntnisse gewinnen, aus denen sich umsetzbare Maßnahmen ableiten lassen. Weitere Informationen finden Sie unter [Azure Media Services Analytics – Übersicht](./legacy-components.md).

> [!NOTE]
> Einige Analytics-Medienprozessoren werden außer Betrieb genommen. Die Daten zur Einstellung finden Sie unter dem Thema [Legacykomponenten](legacy-components.md).

#### <a name="availability"></a>Verfügbarkeit

|Medienprozessorname|Status|Rechenzentren
|---|---|---|
|Azure Media Face Detector|Vorschau|All|
|Azure Media Indexer|Allgemein verfügbar|All|
|Azure Media Motion Detector|Vorschau|All|
|Azure Media OCR|Vorschau|All|
|Azure Media Redactor|Allgemein verfügbar|All|
|Azure Media Video Thumbnails|Vorschau|All|

### <a name="protection"></a>Schutz

Microsoft Azure Media Services ermöglicht die Sicherung Ihrer Medien ab dem Zeitpunkt, an dem sie Ihren Computer durch Speicherung, Verarbeitung und Übermittlung verlassen. Weitere Informationen finden Sie unter [Schützen von Inhalten – Übersicht](media-services-content-protection-overview.md).

#### <a name="availability"></a>Verfügbarkeit

|Verschlüsselung|Status|Rechenzentren|
|---|---|---| 
|Storage|Allgemein verfügbar|All|
|AES-128-Schlüssel|Allgemein verfügbar|All|
|FairPlay|Allgemein verfügbar|All|
|PlayReady|Allgemein verfügbar|All|
|Widevine|Allgemein verfügbar|Alle außer Deutschland, US-Regierung und China.

### <a name="reserved-units-rus"></a>Reservierte Einheiten (RUs)

Die Anzahl der bereitgestellten reservierten Einheiten bestimmt die Anzahl der Medienaufgaben, die gleichzeitig unter einem angegebenen Konto verarbeitet werden können. 

Weitere Informationen finden Sie im [Abschnitt zur Skalierung](#scaling).

#### <a name="availability"></a>Verfügbarkeit

In allen Datencentern verfügbar.

### <a name="reserved-unit-ru-type"></a>Art der reservierten Einheit (RU)

Ein Media Services-Konto ist einem RU-Typ zugeordnet, der die Geschwindigkeit bestimmt, mit der Medienverarbeitungsaufgaben ausgeführt werden. Sie können zwischen den folgenden reservierten Einheitentypen wählen: S1, S2 oder S3.

Weitere Informationen finden Sie im [Abschnitt zur Skalierung](#scaling).

#### <a name="availability"></a>Verfügbarkeit

|Name des RU-Typs|Status|Rechenzentren
|---|---|---|
|S1|Allgemein verfügbar|All|
|S2|Allgemein verfügbar|Alle außer „Brasilien, Süden“ und „Indien, Westen“|
|S3|Allgemein verfügbar|Alle außer „Indien, Westen“|

## <a name="additional-notes"></a>Zusätzliche Hinweise

* Widevine ist ein von Google Inc. bereitgestellter Dienst, der den Vertragsbedingungen und der Datenschutzrichtlinie von Google, Inc. unterliegt.

## <a name="next-steps"></a>Nächste Schritte

Überprüfen Sie die Media Services-Lernpfade.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]