---
title: Azure Media Services-Fehlercodes für Liveereignisse | Microsoft-Dokumentation
description: In diesem Artikel werden Fehlercodes für Liveereignisse aufgelistet.
author: IngridAtMicrosoft
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: error-reference
ms.date: 08/31/2020
ms.author: inhenkel
ms.openlocfilehash: 13277c3d46d5b68aa5705699f45a2bf9e97a78d8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89291533"
---
# <a name="media-services-live-event-error-codes"></a>Azure Media Services-Fehlercodes für Liveereignisse

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

In den Tabellen in diesem Abschnitt sind die [Liveereignis](live-events-outputs-concept.md)-Fehlercodes aufgeführt.

## <a name="liveeventconnectionrejected"></a>LiveEventConnectionRejected

Wenn Sie die [Event Grid](../../event-grid/index.yml)-Ereignisse für ein Liveereignis abonnieren, wird möglicherweise einer der folgenden Fehler aus dem [LiveEventConnectionRejected](media-services-event-schemas.md#liveeventconnectionrejected)-Ereignis ausgegeben.

| Ergebniscode | BESCHREIBUNG |
| ----------- | ----------- |
| MPE_RTMP_APPID_AUTH_FAILURE | Falsche Erfassungs-URL |
| MPE_INGEST_ENCODER_CONNECTION_DENIED | Encoder-IP-Adresse ist nicht in der konfigurierten Liste zugelassener IP-Adressen enthalten |
| MPE_INGEST_RTMP_SETDATAFRAME_NOT_RECEIVED | Der RTMP-Encoder hat den Befehl „setDataFrame“ nicht gesendet. |
| MPE_INGEST_CODEC_NOT_SUPPORTED | Der angegebene Codec wird nicht unterstützt. |
| MPE_INGEST_DESCRIPTION_INFO_NOT_RECEIVED |Die Medienbeschreibungsinformationen wurden nicht empfangen, bevor die eigentlichen Mediendaten übermittelt wurden.|
| MPE_INGEST_MEDIA_QUALITIES_EXCEEDED |Die Anzahl von Qualitätsangaben für Audio und Video hat die maximal zulässige Anzahl überschritten.|
| MPE_INGEST_BITRATE_AGGREGATED_EXCEEDED |Die Gesamtbitrate (eingehend) in einem Liveereignis oder Kanaldienst hat den maximal zulässigen Wert überschritten.|
| MPE_RTMP_FLV_TAG_TIMESTAMP_INVALID | Der Zeitstempel für das Video- oder Audio-FLVTag vom RTMP-Encoder ist ungültig. |
| MPE_INGEST_FRAMERATE_EXCEEDED | Die eingehenden, vom Encoder erfassten Streams mit Frameraten haben den maximal zulässigen Wert von 30 BpS für die Codierung von Liveereignissen/Kanälen überschritten.|
| MPE_INGEST_VIDEO_RESOLUTION_NOT_SUPPORTED | Die eingehenden, vom Encoder erfassten Streams haben die folgenden zulässigen Auflösungen überschritten: 1920 x 1088 für die Codierung von Liveereignissen/Kanälen und 4096 x 2160 für Pass-Through-Liveereignisse/-Kanäle.|
| MPE_INGEST_RTMP_TOO_LARGE_UNPROCESSED_FLV | Das Liveereignis hat eine große Menge von Audiodaten gleichzeitig oder eine große Menge von Videodaten ohne Keyframes erhalten. Wir haben den Encoder getrennt, um ihm die Möglichkeit zu geben, einen Wiederholungsversuch mit korrekten Daten auszuführen. |

## <a name="liveeventencoderdisconnected"></a>LiveEventEncoderDisconnected

Möglicherweise wird einer der folgenden Fehler aus dem [LiveEventEncoderDisconnected](media-services-event-schemas.md#liveeventencoderdisconnected)-Ereignis ausgegeben.

|Ergebniscode|BESCHREIBUNG|
|---|---|
|MPE_RTMP_SESSION_IDLE_TIMEOUT|RTMP-Sitzungstimeout nach einem Leerlauf mit dem zulässigen Zeitlimit.|
|MPE_RTMP_FLV_TAG_TIMESTAMP_INVALID|Der Zeitstempel für das Video- oder Audio-FLVTag vom RTMP-Encoder ist ungültig.|
|MPE_CAPACITY_LIMIT_REACHED|Der Encoder sendet die Daten zu schnell.|
|Unbekannte Fehlercodes|Diese Fehlercodes können von zu wenig Arbeitsspeicher bis zu doppelten Einträgen in der Hashzuordnung reichen.|


## <a name="see-also"></a>Weitere Informationen

[Fehlercodes für den Streamingendpunkt (Ursprung)](streaming-endpoint-error-codes.md)

## <a name="next-steps"></a>Nächste Schritte

[Tutorial: Livestreaming mit Media Services](stream-live-tutorial-with-api.md)
