---
title: Media Encoder Standard-Schema | Microsoft Docs
description: In diesem Artikel werden einige Elemente und Typen des XML-Schemas beschrieben, auf dem Media Encoder Standard-Voreinstellungen basieren.
author: IngridAtMicrosoft
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2021
ms.author: inhenkel
ms.openlocfilehash: f82e0c3f76dba05c3404b11e07c7130119ce0b9d
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "103015657"
---
# <a name="media-encoder-standard-schema"></a>Media Encoder Standard-Schema

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

In diesem Artikel werden einige Elemente und Typen des XML-Schemas beschrieben, auf dem [Media Encoder Standard-Voreinstellungen](media-services-mes-presets-overview.md) basieren. Der Artikel enthält eine Beschreibung der Elemente und der dazugehörigen gültigen Werte.  

## <a name="preset-root-element"></a><a name="Preset"></a> Voreinstellung (Stammelement)
Dient zum Definieren einer Voreinstellung für die Codierung.  

### <a name="elements"></a>Elemente

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Codieren** |[Codieren](media-services-mes-schema.md#Encoding) |Stammelement, mit dem angegeben wird, dass die Eingabequellen codiert werden sollen. |
| **Ausgaben** |[Ausgaben](media-services-mes-schema.md#Output) |Auflistung der gewünschten Ausgabedateien. |
| **StretchMode**<br/>minOccurs="0"<br/>default="AutoSize|xs:string|Festlegen der Größe des Ausgabevideoframes, des Abstands, der Pixel oder des Anzeigeseitenverhältnisses. **StretchMode** kann einen der folgenden Werte aufweisen: **None**, **AutoSize** (Standard) oder **AutoFit**.<br/><br/>**None**: Befolgen Sie strikt die Ausgabeauflösung (z.B. die **Breite** und **Höhe** in der Voreinstellung), ohne das Pixelseitenverhältnis oder das Anzeigeseitenverhältnis des Eingabevideos zu berücksichtigen. Empfohlen wird die Verwendung beispielsweise für [Zuschnitte](media-services-crop-video.md), in denen das Ausgabevideo ein anderes Seitenverhältnis aufweist als das Eingabevideo. <br/><br/>**AutoSize**: Die Ausgabeauflösung passt in das Fenster (Breite * Höhe), das durch die Voreinstellung festgelegt wurde. Der Encoder erzeugt jedoch ein Ausgabevideo mit einem quadratischen Pixelseitenverhältnis (1:1). Daher kann entweder die Ausgabebreite oder -höhe überschrieben werden, um das Anzeigeseitenverhältnis der Eingabe ohne Abstand anzupassen. Wenn die Eingabe beispielsweise 1920x1080 ausweist und die Codierungsvoreinstellung 1280x1280 erfordert, wird der Höhenwert in der Voreinstellung überschrieben, und die Ausgabe erfolgt im Format 1280x720, sodass das Eingabeseitenverhältnis von 16:9 beibehalten wird. <br/><br/>**AutoFit**: Passen Sie bei Bedarf den Abstand des Ausgabevideos an (im Letterbox- oder Pillarboxformat), um die gewünschte Ausgabeauflösung zu erzielen. Achten Sie dabei darauf, dass der aktive Videobereich in der Ausgabe das gleiche Seitenverhältnis wie in der Eingabe aufweist. Angenommen, die Eingabe ist 1920x1080 und die Codierungsvoreinstellung fordert 1280x1280. Dann hat das Ausgabevideo das Format 1280x1280, doch es enthält ein inneres Rechteck mit dem aktiven Video (1280x720) mit einem Seitenverhältnis von 16:9 und mit 280 Pixel hohen Bereichen oben und unten im Letterboxformat. Ein weiteres Beispiel: Wenn die Eingabe 1440x1080 beträgt und die Codierungsvoreinstellung 1280x720 verlangt, liegt die Ausgabe bei 1280x720, was ein inneres Rechteck von 960x720 im Seitenverhältnis 4:3 und 160 Pixel breiten Bereichen links und rechts im Pillarboxformat enthält. 

### <a name="attributes"></a>Attributes

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Version**<br/><br/> Erforderlich |**xs: decimal** |Die voreingestellte Version. Es gelten die folgenden Einschränkungen: xs:fractionDigits value="1" und xs:minInclusive value="1", z.B. **version="1.0"**. |

## <a name="encoding"></a><a name="Encoding"></a> Codieren
Enthält eine Sequenz der unten angegebenen Elemente:  

### <a name="elements"></a>Elemente

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **H264Video** |[H264Video](media-services-mes-schema.md#H264Video) |Einstellungen für die H.264-Videocodierung |
| **AACAudio** |[AACAudio](media-services-mes-schema.md#AACAudio) |Einstellungen für die AAC-Codierung von Audiodaten |
| **BmpImage** |[BmpImage](media-services-mes-schema.md#BmpImage) |Einstellungen für BMP-Bild |
| **PngImage** |[PngImage](media-services-mes-schema.md#PngImage) |Einstellungen für PNG-Bild |
| **JpgImage** |[JpgImage](media-services-mes-schema.md#JpgImage) |Einstellungen für JPG-Bild |

## <a name="h264video"></a><a name="H264Video"></a> H264Video
### <a name="elements"></a>Elemente

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **TwoPass**<br/><br/> minOccurs="0" |**xs:boolean** |Derzeit wird nur die Einwegverschlüsselung unterstützt. |
| **KeyFrameInterval**<br/><br/> minOccurs="0"<br/><br/> **default="00:00:02"** |**xs:time** |Bestimmt den festen Abstand zwischen IDR-Frames in Sekunden. Wird auch als GOP-Dauer bezeichnet. Siehe **SceneChangeDetection** um festzulegen, ob der Encoder von diesem Wert abweichen kann. |
| **SceneChangeDetection**<br/><br/> minOccurs="0"<br/><br/> default="false" |**xs: boolean** |Bei der Einstellung auf „true“, versucht der Codierer, eine Szenenänderung im Video zu erkennen, und fügt ein IDR-Bild ein. |
| **Komplexität**<br/><br/> minOccurs="0"<br/><br/> default="Balanced" |**xs:string** |Steuert den Kompromiss zwischen Codiergeschwindigkeit und Videoqualität. Dies kann einer der folgenden Werte sein: **Speed**, **Balanced** oder **Quality**.<br/><br/> Standardeinstellung: **Balanced** |
| **SyncMode**<br/><br/> minOccurs="0" | |Dieses Feature wird in einer zukünftigen Version verfügbar gemacht. |
| **H264Layers**<br/><br/> minOccurs="0" |[H264Layers](media-services-mes-schema.md#H264Layers) |Auflistung von Ausgabevideoebenen |

### <a name="attributes"></a>Attributes

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Condition** |**xs:string** | Wenn die Eingabe kein Video enthält, können Sie den Encoder zwingen, eine monochrome Videospur hinzuzufügen. Verwenden Sie zu diesem Zweck Condition="InsertBlackIfNoVideoBottomLayerOnly" (Video nur für die unterste Bitrate hinzufügen) oder Condition="InsertBlackIfNoVideo" (Video für alle Ausgabebitraten hinzufügen). Weitere Informationen dazu finden Sie in [diesem Artikel](media-services-advanced-encoding-with-mes.md#no_video).|

## <a name="h264layers"></a><a name="H264Layers"></a> H264Layers

Wenn Sie eine Eingabe an den Encoder senden, die nur Audiodaten und keine Videodaten enthält, besteht das Ausgabemedienobjekt standardmäßig nur aus Dateien mit Audiodaten. Einige Player können derartige Ausgabedatenströme möglicherweise nicht verarbeiten. Mit der Attributeinstellung **InsertBlackIfNoVideo** von H264Video können Sie den Encoder zwingen, der Ausgabe in diesem Szenario eine Videospur hinzuzufügen. Weitere Informationen dazu finden Sie in [diesem Artikel](media-services-advanced-encoding-with-mes.md#no_video).
              
### <a name="elements"></a>Elemente

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **H264Layer**<br/><br/> minOccurs="0" maxOccurs="unbounded" |[H264Layer](media-services-mes-schema.md#H264Layer) |Auflistung von H264-Ebenen |

## <a name="h264layer"></a><a name="H264Layer"></a> H264Layer
> [!NOTE]
> Videogrenzwerte basieren auf den Werten, die in der Tabelle mit den [„H264-Levels“](https://en.wikipedia.org/wiki/H.264/MPEG-4_AVC#Levels) beschrieben sind.  
> 
> 

### <a name="elements"></a>Elemente

| Name | Typ | BESCHREIBUNG |
| --- | --- | --- |
| **Profil**<br/><br/> minOccurs="0"<br/><br/> default="Auto" |**xs: string** |Dies kann einer der folgenden **xs: string**-Werte sein: **Auto**, **Baseline**, **Main**, **High**. |
| **Level**<br/><br/> minOccurs="0"<br/><br/> default="Auto" |**xs: string** | |
| **Bitrate**<br/><br/> minOccurs="0" |**xs:int** |Bitrate für diese Videoebene in KBit/s |
| **MaxBitrate**<br/><br/> minOccurs="0" |**xs: int** |Maximale Bitrate für diese Videoebene in KBit/s |
| **BufferWindow**<br/><br/> minOccurs="0"<br/><br/> default="00:00:05" |**xs: time** |Länge des Videopuffers |
| **Width**<br/><br/> minOccurs="0" |**xs: int** |Breite des Ausgabevideobilds in Pixel.<br/><br/> Derzeit müssen sowohl „Width“ als auch „Height“ angeben werden. „Width“ und „Height“ müssen gerade Zahlen sein. |
| **Height**<br/><br/> minOccurs="0" |**xs:int** |Höhe des Ausgabevideobilds in Pixel.<br/><br/> Derzeit müssen sowohl „Width“ als auch „Height“ angeben werden. „Width“ und „Height“ müssen gerade Zahlen sein.|
| **BFrames**<br/><br/> minOccurs="0" |**xs: int** |Anzahl von B-Bildern zwischen Referenzbildern. |
| **ReferenceFrames**<br/><br/> minOccurs="0"<br/><br/> default="3" |**xs:int** |Anzahl von Referenzbildern in einer Bildgruppe (GOP). |
| **EntropyMode**<br/><br/> minOccurs="0"<br/><br/> default="Cabac" |**xs: string** |Dies kann einer der folgenden Werte sein: **Cabac** und **Cavlc**. |
| **FrameRate**<br/><br/> minOccurs="0" |Rationale Zahl |Bestimmt die Bildfrequenz des Ausgabevideos. Verwenden Sie den Standardwert "0/1", damit der Codierer die gleiche Bildfrequenz wie beim Eingabevideo nutzen kann. Zulässige Werte sind allgemeine Videobildfrequenzen. Es ist aber die Eingabe einer beliebigen gültigen rationalen Zahl zulässig. Für „1/1“ ergibt sich beispielsweise 1 Bild/Sekunde, also ein gültiger Wert.<br/><br/> - 12/1 (12 Bilder/Sekunde)<br/><br/> - 15/1 (15 Bilder/Sekunde)<br/><br/> - 24/1 (24 Bilder/Sekunde)<br/><br/> - 24.000/1.001 (23,976 Bilder/Sekunde)<br/><br/> - 25/1 (25 Bilder/Sekunde)<br/><br/>  - 30/1 (30 Bilder/Sekunde)<br/><br/> - 30.000/1.001 (29,97 Bilder/Sekunde) <br/> <br/>**HINWEIS** Wenn Sie eine benutzerdefinierte Voreinstellung für die Mehrfachbitraten-Codierung erstellen, **muss** auf allen Ebenen der Voreinstellung der gleiche Wert für FrameRate verwendet werden.|
| **AdaptiveBFrame**<br/><br/> minOccurs="0" |**xs: boolean** |Kopieren vom Azure Media Encoder |
| **Slices**<br/><br/> minOccurs="0"<br/><br/> default="0" |**xs:int** |Bestimmt, in wie viele Slices ein Bild aufgeteilt wird. Es wird empfohlen, die Standardeinstellung zu verwenden. |

## <a name="aacaudio"></a><a name="AACAudio"></a> AACAudio
 Enthält eine Sequenz der unten angegebenen Elemente und Gruppen.  

 Weitere Informationen zu AAC finden Sie unter [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding).  

### <a name="elements"></a>Elemente

| Name | Typ | BESCHREIBUNG |
| --- | --- | --- |
| **Profil**<br/><br/> minOccurs="0"<br/><br/> default="AACLC" |**xs: string** |Dies kann einer der folgenden Werte sein: **AACLC**, **HEAACV1** oder **HEAACV2**. |

### <a name="attributes"></a>Attributes

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Condition** |**xs: string** |Um zu erzwingen, dass der Encoder ein Asset erstellt, das bei einer Eingabe ohne Audio eine stille Audiospur enthält, geben Sie den Wert „InsertSilenceIfNoAudio“ an.<br/><br/> Wenn Sie eine Eingabe an den Encoder senden, die keine Audiodaten, sondern nur Videodaten enthält, besteht das Ausgabemedienobjekt standardmäßig nur aus Dateien mit Videodaten. Einige Player können derartige Ausgabedatenströme möglicherweise nicht verarbeiten. Mit dieser Einstellung können Sie den Encoder zwingen, der Ausgabe in diesem Szenario eine stille Audiospur hinzuzufügen. |

### <a name="groups"></a>Gruppen

| Referenz | Beschreibung |
| --- | --- |
| [AudioGroup](media-services-mes-schema.md#AudioGroup)<br/><br/> minOccurs="0" |In der Beschreibung von [AudioGroup](media-services-mes-schema.md#AudioGroup) finden Sie Informationen zur richtigen Anzahl von Kanälen, zur Samplingrate und zur Bitrate, die für die einzelnen Profile festgelegt werden kann. |

## <a name="audiogroup"></a><a name="AudioGroup"></a> AudioGroup
Ausführliche Informationen dazu, welche Werte für die einzelnen Profile gelten, finden Sie unten in der Tabelle mit den „Audiocodec-Details“.  

### <a name="elements"></a>Elemente

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Channels**<br/><br/> minOccurs="0" |**xs: int** |Gibt die Anzahl von codierten Audiokanälen an. Gültige Optionen sind: 1, 2, 5, 6, 8.<br/><br/> Standardeinstellung: 2. |
| **SamplingRate**<br/><br/> minOccurs="0" |**xs: int** |Audiosamplingrate in Hz |
| **Bitrate**<br/><br/> minOccurs="0" |**xs: int** |Bitrate für die Codierung der Audiodaten in KBit/s |

### <a name="audio-codec-details"></a>Audiocodec-Details

Audiocodec|Details  
-----------------|---  
**AACLC** |1:<br/><br/> - 11025 : 8 &lt;= Bitrate &lt; 16<br/><br/> - 12000 : 8 &lt;= Bitrate &lt; 16<br/><br/> - 16000 : 8 &lt;= Bitrate &lt;32<br/><br/>- 22050 : 24 &lt;= Bitrate &lt; 32<br/><br/> - 24000 : 24 &lt;= Bitrate &lt; 32<br/><br/> - 32000 : 32 &lt;= Bitrate &lt;= 192<br/><br/> - 44100 : 56 &lt;= Bitrate &lt;= 288<br/><br/> - 48000 : 56 &lt;= Bitrate &lt;= 288<br/><br/> - 88200 : 128 &lt;= Bitrate &lt;= 288<br/><br/> - 96000 : 128 &lt;= Bitrate &lt;= 288<br/><br/> 2:<br/><br/> - 11025 : 16 &lt;= Bitrate &lt; 24<br/><br/> - 12000 : 16 &lt;= Bitrate &lt; 24<br/><br/> - 16000 : 16 &lt;= Bitrate &lt; 40<br/><br/> - 22050 : 32 &lt;= Bitrate &lt; 40<br/><br/> - 24000 : 32 &lt;= Bitrate &lt; 40<br/><br/> - 32000 : 40 &lt;= Bitrate &lt;= 384<br/><br/> - 44100 : 96 &lt;= Bitrate &lt;= 576<br/><br/> - 48000 : 96 &lt;= Bitrate &lt;= 576<br/><br/> - 88200 : 256 &lt;= Bitrate &lt;= 576<br/><br/> - 96000 : 256 &lt;= Bitrate &lt;= 576<br/><br/> 5/6:<br/><br/> - 32000 : 160 &lt;= Bitrate &lt;= 896<br/><br/> - 44100 : 240 &lt;= Bitrate &lt;= 1024<br/><br/> - 48000 : 240 &lt;= Bitrate &lt;= 1024<br/><br/> - 88200 : 640 &lt;= Bitrate &lt;= 1024<br/><br/> - 96000 : 640 &lt;= Bitrate &lt;= 1024<br/><br/> 8:<br/><br/> - 32000 : 224 &lt;= Bitrate &lt;= 1024<br/><br/> - 44100 : 384 &lt;= Bitrate &lt;= 1024<br/><br/> - 48000 : 384 &lt;= Bitrate &lt;= 1024<br/><br/> - 88200 : 896 &lt;= Bitrate &lt;= 1024<br/><br/> - 96000 : 896 &lt;= Bitrate &lt;= 1024  
**HEAACV1** |1:<br/><br/> - 22050: Bitrate = 8<br/><br/> - 24000 : 8 &lt;= Bitrate &lt;= 10<br/><br/> - 32000 : 12 &lt;= Bitrate &lt;= 64<br/><br/> - 44100 : 20 &lt;= Bitrate &lt;= 64<br/><br/> - 48000 : 20 &lt;= Bitrate &lt;= 64<br/><br/> - 88200: Bitrate = 64<br/><br/> 2:<br/><br/> - 32000 : 16 &lt;= Bitrate &lt;= 128<br/><br/> - 44100 : 16 &lt;= Bitrate &lt;= 128<br/><br/> - 48000 : 16 &lt;= Bitrate &lt;= 128<br/><br/> - 88200 : 96 &lt;= Bitrate &lt;= 128<br/><br/> - 96000 : 96 &lt;= Bitrate &lt;= 128<br/><br/> 5/6:<br/><br/> - 32000 : 64 &lt;= Bitrate &lt;= 320<br/><br/> - 44100 : 64 &lt;= Bitrate &lt;= 320<br/><br/> - 48000 : 64 &lt;= Bitrate &lt;= 320<br/><br/> - 88200 : 256 &lt;= Bitrate &lt;= 320<br/><br/> - 96000 : 256 &lt;= Bitrate &lt;= 320<br/><br/> 8:<br/><br/> - 32000 : 96 &lt;= Bitrate &lt;= 448<br/><br/> - 44100 : 96 &lt;= Bitrate &lt;= 448<br/><br/> - 48000 : 96 &lt;= Bitrate &lt;= 448<br/><br/> - 88200 : 384 &lt;= Bitrate &lt;= 448<br/><br/> - 96000 : 384 &lt;= Bitrate &lt;= 448  
**HEAACV2** |2:<br/><br/> - 22050 : 8 &lt;= Bitrate &lt;= 10<br/><br/> - 24000 : 8 &lt;= Bitrate &lt;= 10<br/><br/> - 32000 : 12 &lt;= Bitrate &lt;= 64<br/><br/> - 44100 : 20 &lt;= Bitrate &lt;= 64<br/><br/> - 48000 : 20 &lt;= Bitrate &lt;= 64<br/><br/> - 88200 : 64 &lt;= Bitrate &lt;= 64  
  
## <a name="clip"></a><a name="Clip"></a> Clip
### <a name="attributes"></a>Attributes

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **StartTime** |**xs:duration** |Gibt die Startzeit einer Darstellung an. Der Wert von „StartTime“ muss mit den absoluten Zeitstempeln des Eingabevideos übereinstimmen. Wenn beispielsweise das erste Bild des Eingabevideos den Zeitstempel 12:00:10.000 hat, sollte „StartTime“ mindestens 12:00:10.000 betragen. |
| **Duration** |**xs:duration** |Gibt die Dauer einer Darstellung an (z.B. die Einblendung einer Überlagerung im Video). |

## <a name="output"></a><a name="Output"></a> Ausgabe
### <a name="attributes"></a>Attributes

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **FileName** |**xs:string** |Der Name der Ausgabedatei.<br/><br/> Sie können die Makros in der folgenden Tabelle verwenden, um die Ausgabedateinamen zu erstellen. Beispiel:<br/><br/> **"Outputs": [      {       "FileName": "{Basename}*{Resolution}*{Bitrate}.mp4",       "Format": {         "Type": "MP4Format"       }     }   ]** |

### <a name="macros"></a>Makros

| Makro | Beschreibung |
| --- | --- |
| **{Basename}** |Wenn Sie die VoD-Codierung durchführen, umfasst {Basename} die ersten 32 Zeichen der AssetFile.Name-Eigenschaft für die primäre Datei im Eingabeasset.<br/><br/> Wenn das Eingabeasset ein Live-Archiv ist, wird {Basename} aus den trackName-Attributen im Servermanifest abgeleitet. Wenn Sie einen Subclipauftrag mit TopBitrate übermitteln, z.B. „<VideoStream\>TopBitrate</VideoStream\>“, und die Ausgabedatei Videodaten enthält, umfasst {Basename} die ersten 32 Zeichen des trackName-Elements der Videoebene mit der höchsten Bitrate.<br/><br/> Falls Sie stattdessen zum Übermitteln eines Subclipauftrags alle Eingabebitraten verwenden, z.B. „<VideoStream\>*</VideoStream\>“, und die Ausgabedatei Videodaten enthält, umfasst {Basename} die ersten 32 Zeichen des trackName-Elements der entsprechenden Videoebene. |
| **{Codec}** |Wird „H264“ für Videodaten und „AAC“ für Audiodaten zugeordnet. |
| **{Bitrate}** |Gibt die Ziel-Videobitrate an, wenn die Ausgabedatei Video- und Audiodaten enthält, bzw. die Ziel-Audiobitrate, wenn die Ausgabedatei nur Audiodaten enthält. Als Wert wird die Bitrate in KBit/s verwendet. |
| **{Channel}** |Audiokanalanzahl, wenn die Datei Audiodaten enthält. |
| **{Width}** |Breite des Videos in der Ausgabedatei in Pixel, wenn die Datei Videodaten enthält. |
| **{Height}** |Höhe des Videos in der Ausgabedatei in Pixel, wenn die Datei Videodaten enthält. |
| **{Extension}** |Erbt von der „Type“-Eigenschaft für die Ausgabedatei. Der Name der Ausgabedatei hat eine der folgenden Erweiterungen: „mp4“, „ts“, „jpg“, „png“ oder „bmp“. |
| **{Index}** |Für Miniaturansichten obligatorisch. Sollte nur einmal vorhanden sein. |

## <a name="video-complex-type-inherits-from-codec"></a><a name="Video"></a> Video (komplexer Typ erbt vom Codec)
### <a name="attributes"></a>Attributes

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Starten** |**xs:string** | |
| **Schritt** |**xs:string** | |
| **Bereich** |**xs:string** | |
| **PreserveResolutionAfterRotation** |**xs:boolean** |Eine ausführliche Beschreibung finden Sie im Abschnitt [PreserveResolutionAfterRotation](media-services-mes-schema.md#PreserveResolutionAfterRotation). |

### <a name="preserveresolutionafterrotation"></a><a name="PreserveResolutionAfterRotation"></a> PreserveResolutionAfterRotation
Es wird empfohlen, das **PreserveResolutionAfterRotation**-Flag zusammen mit Auflösungswerten zu verwenden, die als Prozentsätze ausgedrückt werden (Width="100%" , Height="100%").  

Standardmäßig sind die Einstellungen für die Codierungsauflösung (Width, Height) in den Voreinstellungen des Media Encoder Standard (MES) für Videos mit einer Drehung von 0 Grad bestimmt. Wenn Ihr Eingabevideo beispielsweise über eine Auflösung von 1.280 x 720 und eine Drehung von 0 Grad verfügt, wird mit den Standardvoreinstellungen sichergestellt, dass die Ausgabe die gleiche Auflösung aufweist.  

![MESRoation1](./media/media-services-shemas/media-services-mes-roation1.png) 

Wenn das Eingangsvideo mit einer Drehung ungleich Null aufgenommen wurde (z. B. mit einem vertikal gehaltenen Smartphone oder Tablet), bedeutet dies aber Folgendes: MES wendet standardmäßig die Einstellungen für die Codierauflösung („Width“, „Height“) auf die Eingangsvideodaten an und gleicht anschließend die Drehung aus. Betrachten Sie z. B. die folgende Abbildung. Für die Voreinstellung wird Breite = „100%“ und Höhe = „100%“ verwendet. Das interpretiert MES so, dass die Ausgabe 1280 Pixel breit und 720 Pixel hoch ist. Nach der Drehung des Videos wird das Bild dann so verkleinert, dass es in das Fenster passt, sodass sich links und rechts leere Bereiche ergeben.  

![MESRoation2](./media/media-services-shemas/media-services-mes-roation2.png) 

Alternativ können Sie das **PreserveResolutionAfterRotation**-Flag verwenden und auf TRUE festlegen (Standardeinstellung ist FALSE). Wenn also Breite = „100%“ und Höhe = „100%“ voreingestellt und „PreserveResolutionAfterRotation“ auf TRUE festgelegt ist, wird für Eingabevideodaten mit einer Breite von 1280 Pixeln und einer Höhe von 720 Pixeln mit einer Drehung von 90 Grad eine Ausgabe mit 0 Grad Drehung und einer Breite von 720 Pixeln und einer Höhe von 1280 Pixeln erzeugt. Sehen Sie sich die folgende Abbildung an:  

![MESRoation3](./media/media-services-shemas/media-services-mes-roation3.png) 

## <a name="formatgroup-group"></a><a name="FormatGroup"></a> FormatGroup (Gruppe)
### <a name="elements"></a>Elemente

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **BmpFormat** |**BmpFormat** | |
| **PngFormat** |**PngFormat** | |
| **JpgFormat** |**JpgFormat** | |

## <a name="bmplayer"></a><a name="BmpLayer"></a> BmpLayer
### <a name="element"></a>Element

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Width**<br/><br/> minOccurs="0" |**xs:int** | |
| **Height**<br/><br/> minOccurs="0" |**xs:int** | |

### <a name="attributes"></a>Attributes

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Condition** |**xs:string** | |

## <a name="pnglayer"></a><a name="PngLayer"></a> PngLayer
### <a name="element"></a>Element

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Width**<br/><br/> minOccurs="0" |**xs:int** | |
| **Height**<br/><br/> minOccurs="0" |**xs:int** | |

### <a name="attributes"></a>Attributes

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Condition** |**xs:string** | |

## <a name="jpglayer"></a><a name="JpgLayer"></a> JpgLayer
### <a name="element"></a>Element

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Width**<br/><br/> minOccurs="0" |**xs:int** | |
| **Height**<br/><br/> minOccurs="0" |**xs:int** | |
| **Qualität**<br/><br/> minOccurs="0" |**xs:int** |Gültige Werte: 1 (am schlechtesten) bis 100 (am besten) |

### <a name="attributes"></a>Attributes

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Condition** |**xs:string** | |

## <a name="pnglayers"></a><a name="PngLayers"></a> PngLayers
### <a name="elements"></a>Elemente

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **PngLayer**<br/><br/> minOccurs="0" maxOccurs="unbounded" |[PngLayer](media-services-mes-schema.md#PngLayer) | |

## <a name="bmplayers"></a><a name="BmpLayers"></a> BmpLayers
### <a name="elements"></a>Elemente

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **BmpLayer**<br/><br/> minOccurs="0" maxOccurs="unbounded" |[BmpLayer](media-services-mes-schema.md#BmpLayer) | |

## <a name="jpglayers"></a><a name="JpgLayers"></a> JpgLayers
### <a name="elements"></a>Elemente

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **JpgLayer**<br/><br/> minOccurs="0" maxOccurs="unbounded" |[JpgLayer](media-services-mes-schema.md#JpgLayer) | |

## <a name="bmpimage-complex-type-inherits-from-video"></a><a name="BmpImage"></a> BmpImage (komplexer Typ erbt vom Video)
### <a name="elements"></a>Elemente

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs="0" |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG-Ebenen |

## <a name="jpgimage-complex-type-inherits-from-video"></a><a name="JpgImage"></a> JpgImage (komplexer Typ erbt vom Video)
### <a name="elements"></a>Elemente

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs="0" |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG-Ebenen |

## <a name="pngimage-complex-type-inherits-from-video"></a><a name="PngImage"></a> PngImage (komplexer Typ erbt vom Video)
### <a name="elements"></a>Elemente

| Name | Typ | Beschreibung |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs="0" |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG-Ebenen |

## <a name="examples"></a>Beispiele
Beispiele für XML-Voreinstellungen, die basierend auf diesem Schema erstellt werden, finden Sie unter [Task Presets for MES (Media Encoder Standard)](media-services-mes-presets-overview.md) (Aufgabenvoreinstellungen für MES (Media Encoder Standard)).

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

