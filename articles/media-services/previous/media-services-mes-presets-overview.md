---
title: Aufgabenvoreinstellungen für Media Encoder Standard (MES) | Microsoft-Dokumentation
description: Das Thema bietet eine Übersicht über die vom Dienst definierten Beispielvoreinstellungen für Media Encoder Standard (MES).
author: IngridAtMicrosoft
manager: femila
editor: johndeu
services: media-services
documentationcenter: ''
ms.assetid: f243ed1c-ac9c-4300-a5f7-f092cf9853b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2021
ms.author: inhenkel
ms.openlocfilehash: 74e557aa02f5ab83828262209653bf8a78fdd2d0
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "103011645"
---
# <a name="sample-presets-for-media-encoder-standard-mes"></a>Beispielvoreinstellungen für Media Encoder Standard (MES)

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

**Media Encoder Standard** bietet eine Reihe vom System vordefinierter Codierungsvoreinstellungen, die Sie beim Erstellen von Codierungsaufträgen verwenden können. Es wird empfohlen, die Voreinstellung „Adaptives Streaming“ zu verwenden, wenn Sie ein Video für das Streamen mit Media Services codieren möchten. Bei Angabe dieser Voreinstellung wird von Media Encoder Standard [automatisch eine Bitrate generiert](media-services-autogen-bitrate-ladder-with-mes.md). 

### <a name="creating-custom-presets-from-samples"></a>Erstellen benutzerdefinierter Voreinstellungen anhand von Beispielen
Media Services unterstützt die vollständige Anpassung aller Werte in Voreinstellungen zum Erfüllen Ihrer spezifischen Codierungsanforderungen. Wenn Sie eine Codierungsvoreinstellung anpassen müssen, sollten Sie mit einer der Systemvoreinstellungen beginnen, die in diesem Abschnitt als Vorlage für Ihre benutzerdefinierte Konfiguration angegeben sind. Erläuterungen zur Bedeutung der einzelnen Elemente in diesen Voreinstellungen sowie gültige Werte für jedes Element finden Sie im Thema [Media Encoder Standard schema](media-services-mes-schema.md) (Media Encoder Standard-Schema).  
  
> [!NOTE]
>  Wenn Sie eine Voreinstellung für 4k-Codierungen verwenden, sollten Sie den reservierten Einheitentyp `S3` erhalten. Weitere Informationen finden Sie unter [Skalieren der Codierung](./media-services-scale-media-processing-overview.md).  

#### <a name="video-rotation-default-setting-in-presets"></a>Standardeinstellung für die Videodrehung in den Voreinstellungen:
Bei der Arbeit mit Media Encoder Standard ist die Videodrehung standardmäßig aktiviert. Wenn Ihr Video im Hochformat auf einem Mobilgerät aufgezeichnet wurde, wird es vor der Codierung mit diesen Voreinstellungen in das Querformat gedreht.
 
## <a name="available-presets"></a>Verfügbare Vorgaben: 

 [H264 Multiple Bitrate 1080p Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-1080p-Audio-5.1.md) erzeugt einen Satz von acht MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 6.000KBit/s und 400KBit/s sowie AAC 5.1-Audio.  
  
 [H264 Multiple Bitrate 1080p](media-services-mes-preset-H264-Multiple-Bitrate-1080p.md) erzeugt einen Satz von acht MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 6.000KBit/s und 400KBit/s sowie Stereo-AAC-Audio.  
  
 [H264 Multiple Bitrate 16x9 für iOS](media-services-mes-preset-H264-Multiple-Bitrate-16x9-for-iOS.md) erzeugt einen Satz von acht MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 8.500KBit/s und 200KBit/s sowie Stereo-AAC-Audio.  
  
 [H264 Multiple Bitrate 16x9 SD Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD-Audio-5.1.md) erzeugt einen Satz von fünf MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 1.900KBit/s und 400KBit/s sowie AAC 5.1-Audio.  
  
 [H264 Multiple Bitrate 16x9 SD](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD.md) erzeugt einen Satz von fünf MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 1.900KBit/s und 400KBit/s sowie Stereo-AAC-Audio.  
  
 [H264 Multiple Bitrate 4K Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4K-Audio-5.1.md) erzeugt einen Satz von zwölf MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 20.000KBit/s und 1.000KBit/s sowie AAC 5.1-Audio.  
  
 [H264 Multiple Bitrate 4K](media-services-mes-preset-H264-Multiple-Bitrate-4K.md) erzeugt einen Satz von 12 MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 20.000KBit/s und 1.000KBit/s sowie Stereo-AAC-Audio.  
  
 [H264 Multiple Bitrate 4x3 für iOS](media-services-mes-preset-H264-Multiple-Bitrate-4x3-for-iOS.md) erzeugt einen Satz von acht MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 8.500KBit/s und 200KBit/s sowie Stereo-AAC-Audio.  
  
 [H264 Multiple Bitrate 4x3 SD Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD-Audio-5.1.md) erzeugt einen Satz von fünf MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 1.600KBit/s und 400KBit/s sowie AAC 5.1-Audio.  
  
 [H264 Multiple Bitrate 4x3 SD](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD.md) erzeugt einen Satz von fünf MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 1.600KBit/s und 400KBit/s sowie Stereo-AAC-Audio.  
  
 [H264 Multiple Bitrate 720p Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-720p-Audio-5.1.md) erzeugt einen Satz von sechs MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 3.400KBit/s und 400KBit/s sowie AAC 5.1-Audio.  
  
 [H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) erzeugt einen Satz von sechs MP4-Dateien mit GOP-Ausrichtung mit Werten zwischen 3.400KBit/s und 400KBit/s sowie Stereo-AAC-Audio.  
  
 [H264 Single Bitrate 1080p Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-1080p-Audio-5.1.md) erzeugt eine einzelne MP4-Datei mit einer Bitrate von 6.750KBit/s und AAC 5.1-Audio.  
  
 [H264 Single Bitrate 1080p](media-services-mes-preset-H264-Single-Bitrate-1080p.md) erzeugt eine einzelne MP4-Datei mit einer Bitrate von 6.750KBit/s und Stereo-AAC-Audio.  
  
 [H264 Single Bitrate 4K Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-4K-Audio-5.1.md) erzeugt eine einzelne MP4-Datei mit einer Bitrate von 18.000KBit/s und AAC 5.1-Audio.  
  
 [H264 Single Bitrate 4K](media-services-mes-preset-H264-Single-Bitrate-4K.md) erzeugt eine einzelne MP4-Datei mit einer Bitrate von 18.000KBit/s und Stereo-AAC-Audio.  
  
 [H264 Single Bitrate 4x3 SD Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-4x3-SD-Audio-5.1.md) erzeugt eine einzelne MP4-Datei mit einer Bitrate von 1.800 KBit/s und AAC 5.1-Audio.  
  
 [H264 Single Bitrate 4x3 SD](media-services-mes-preset-H264-Single-Bitrate-4x3-SD.md) erzeugt eine einzelne MP4-Datei mit einer Bitrate von 1.800 KBit/s und Stereo-AAC-Audio.  
  
 [H264 Single Bitrate 16x9 SD Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-16x9-SD-Audio-5.1.md) erzeugt eine einzelne MP4-Datei mit einer Bitrate von 2.200KBit/s und AAC 5.1-Audio.  
  
 [H264 Single Bitrate 16x9 SD](media-services-mes-preset-H264-Single-Bitrate-16x9-SD.md) erzeugt eine einzelne MP4-Datei mit einer Bitrate von 2.200KBit/s und Stereo-AAC-Audio.  
  
 [H264 Single Bitrate 720p Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-720p-Audio-5.1.md) erzeugt eine einzelne MP4-Datei mit einer Bitrate von 4.500KBit/s und AAC 5.1-Audio.  
  
 [H264 Single Bitrate 720p für Android](media-services-mes-preset-H264-Single-Bitrate-720p-for-Android.md) erzeugt eine einzelne MP4-Datei mit einer Bitrate von 2.000KBit/s und Stereo-AAC.  
  
 [H264 Single Bitrate 720p](media-services-mes-preset-H264-Single-Bitrate-720p.md) erzeugt eine einzelne MP4-Datei mit einer Bitrate von 4.500KBit/s und Stereo-AAC-Audio.  
  
 [H264 Single Bitrate High Quality SD für Android](media-services-mes-preset-H264-Single-Bitrate-High-Quality-SD-for-Android.md) erzeugt eine einzelne MP4-Datei mit einer Bitrate von 500KBit/s und Stereo-AAC-Audio.  
  
 [H264 Single Bitrate Low Quality SD für Android](media-services-mes-preset-H264-Single-Bitrate-Low-Quality-SD-for-Android.md) erzeugt eine einzelne MP4-Datei mit einer Bitrate von 56KBit/s und Stereo-AAC-Audio.  
  
 Weitere Informationen im Zusammenhang mit Media Services-Encodern finden Sie unter [Azure On-Demand Media Encoder – Überblick und Vergleich](./media-services-encode-asset.md).
