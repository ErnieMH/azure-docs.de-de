---
title: 'Videowiedergabe mit Live Video Analytics: Azure'
description: In diesem Artikel erfahren Sie mehr über die auszuführenden Schritte, um ein Medienobjekt mithilfe der vorhandenen Streamingfunktionen von Azure Media Services wiederzugeben.
ms.topic: conceptual
ms.date: 04/27/2020
ms.openlocfilehash: 9669194f3dff8f64d54cb15c8cb6fee0d7a2667b
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110368385"
---
# <a name="video-playback-with-live-video-analytics"></a>Videowiedergabe mit Live Video Analytics

[!INCLUDE [redirect to Azure Video Analyzer](./includes/redirect-video-analyzer.md)]

## <a name="suggested-pre-reading"></a>Empfohlene Lektüre zur Vorbereitung 

* [Übersicht zu Live Video Analytics in IoT Edge](overview.md)
* [Terminologie zu Live Video Analytics in IoT Edge](terminology.md)
* [Mediengraph: Konzepte](media-graph-concept.md)

## <a name="overview"></a>Übersicht  

Sie können [Mediengraphen](media-graph-concept.md) verwenden, um Video in einem [Medienobjekt](terminology.md#asset) von Azure Media Services aufzuzeichnen. In diesem Artikel erfahren Sie mehr über die auszuführenden Schritte, um ein Medienobjekt mithilfe der vorhandenen Streamingfunktionen von Azure Media Services wiederzugeben.

## <a name="streaming-endpoint"></a>Streamingendpunkt 

Sie können Azure Media Services verwenden, um das Medienobjekt mithilfe von Branchenstandards für HTTP-basierte Medienstreamingprotokolle wie HTTP Live Streaming (HLS) und MPEG-DASH an Videoplayer zu [streamen](terminology.md#streaming). Diese Konvertierung von Medien aus aufgezeichneten Inhalten in Streamingformate erfolgt durch einen [Streamingendpunkt](../latest/stream-streaming-endpoint-concept.md). Das ist eine Ressource, die Sie in Ihrem Azure Media Service-Konto bereitstellen müssen.

## <a name="streaming-policy"></a>Streamingrichtlinie 

Azure Media Services bietet Ihnen verschiedene Methoden zum Schützen Ihrer Videostreams, wie im Artikel [Schützen Sie Ihren Inhalt mit der dynamischen Verschlüsselung von Media Services](../latest/drm-content-protection-concept.md) beschrieben. Dies sind die allgemeinen Optionen für den Schutz von Inhalten:

* **Unverschlüsseltes Streaming**: Es wird während des Streamings keine Verschlüsselung angewendet.
* **Verwenden von AES-128 (Advanced Encryption Standard)** : Sie müssen eine Methode implementieren, um die Ausgabe von Schlüsseln zum Entschlüsseln des Videos nur an authentifizierte Viewer sicherzustellen.
* **Verwenden von DRM-Systemen (Digital Rights Management, Verwaltung digitaler Rechte)** : Sie können die Nutzung, Bearbeitung und Übermittlung von Video an Geräte steuern, die diese Richtlinien durchsetzen.

Um den Schutz von Inhalten zu erreichen, können Sie eine [Streamingrichtlinie](../latest/stream-streaming-policy-concept.md) in Ihrem Media Service-Konto erstellen und sie für das Streaming aller Medienobjekte verwenden (unter der Annahme, dass für alle Medienobjekte die gleichen Sicherheitsanforderungen gelten). Sie können auch jede der vordefinierten Richtlinien (z. B. Predefined_ClearStreamingOnly) verwenden.

## <a name="streaming-locator"></a>Streaminglocator  

Nachdem Sie einen Streamingendpunkt in Ihrem Media Services-Konto gestartet haben und die Streaming-Richtlinie definiert wurde, können Sie mit dem Streamen von aufgezeichneten Medien aus einem Medienobjekt mithilfe der HLS- oder DASH-Protokolle fortfahren. Webplayer und mobile Apps benötigen eine URL, die auf den HLS- oder DASH-Datenstrom verweist. Sie können diese URL mit dem [Streaminglocator](../latest/stream-streaming-locators-concept.md) erstellen. Wie im Artikel [Erstellen eines Streaminglocators und von URLs](../latest/create-streaming-locator-build-url.md) erläutert und dort im Beispiel gezeigt, setzt sich die Streaming-URL aus dem Streamingendpunkt, der Streamingrichtlinie und dem Streaminglocator zusammen.

## <a name="content-recorded-using-file-sink"></a>Mithilfe der Dateisenke aufgezeichnete Inhalte  

Wie unter [Media Graph-Dateisenke](media-graph-concept.md#file-sink) beschrieben, können Sie Mediengraphen verwenden, um Videos im lokalen Dateisystem des Edge-Geräts mithilfe einer Dateisenke in Ihrem Mediengraph aufzuzeichnen. Die Dateisenke generiert [MP4](https://developer.mozilla.org/docs/Web/Media/Formats/Containers#MP4)-Dateien, und Sie können das HTML5-Element [&lt;Video&gt;](https://developer.mozilla.org/docs/Web/HTML/Element/video) zum Wiedergeben solcher Inhalte verwenden. 

## <a name="next-steps"></a>Nächste Schritte

[Azure IoT Edge](../../iot-edge/index.yml)
<!--
## Next steps

[Playback recording](playback-recording-how-to.md)
-->
