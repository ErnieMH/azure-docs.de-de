---
title: Cognitive Services-Containerimagetags
titleSuffix: Azure Cognitive Services
description: Eine umfassende Liste aller Cognitive Services-Containerimagetags.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: reference
ms.date: 08/31/2020
ms.author: aahi
ms.openlocfilehash: 2a24433389e738bf5d0ecb7ecac6bf369c8ba183
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91369483"
---
# <a name="azure-cognitive-services-container-image-tags"></a>Azure Cognitive Services-Containerimagetags

Azure Cognitive Services bietet viele Containerimages. Die Containerregistrierungen und die entsprechenden Repositorys unterscheiden sich je nach Containerimage. Jeder Containerimagename bietet mehrere Tags. Ein Containerimagetag ist ein Mechanismus zur Versionsverwaltung für Containerimages. Dieser Artikel ist als umfassende Referenz für die Auflistung aller Cognitive Services-Containerimages und ihrer verfügbaren Tags gedacht.

> [!TIP]
> Achten Sie bei der Verwendung von [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) besonders auf die Groß-/Kleinschreibung der Containerregistrierung, des Repositorys, des Containerimagenamens und des entsprechenden Tags, da bei ihnen die **Groß-/Kleinschreibung berücksichtigt** wird.

## <a name="anomaly-detector"></a>Anomalieerkennung

Das Containerimage für die [Anomalieerkennung][ad-containers] finden Sie im Containerregistrierungssyndikat `mcr.microsoft.com`. Es befindet sich im Repository `azure-cognitive-services` und trägt den Namen `anomaly-detector`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/anomaly-detector`.

Dieses Containerimage verfügt über die folgenden Tags:

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest`                      |       |

## <a name="computer-vision"></a>Maschinelles Sehen

Das Containerimage [Maschinelles Sehen][cv-containers] für das Lesen von OCR befindet sich in der Containerregistrierung `containerpreview.azurecr.io`. Es befindet sich im Repository `microsoft` und trägt den Namen `cognitive-services-read`. Der vollqualifizierte Containerimagename lautet `containerpreview.azurecr.io/microsoft/cognitive-services-read`.

Dieses Containerimage verfügt über die folgenden Tags:

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest ( (2.0.013250001-amd64-preview)` | • Verringern Sie die Speicherauslastung für den Container weiter. |
|                                          | • Für das Einrichten von mehreren Pods ist externer Cache erforderlich. Richten Sie z. B. Redis als Cache ein. |
|                                          | • Beheben Sie das Problem fehlender Ergebnisse, wenn Redis-Cache eingerichtet ist und ResultExpirationPeriod=0 gilt.  |
|                                          | • Heben Sie die Größenbeschränkung für den Anforderungstext von 26 MB auf. Der Container kann jetzt Dateien akzeptieren, die größer als 26 MB sind.  |
|                                          | • Fügen Sie der Konsolenprotokollierung Zeitstempel und Buildversion hinzu.  |
| `1.1.013050001-amd64-preview`            | * Die Konfiguration „ReadEngineConfig:ResultExpirationPeriod“ für die Containerinitialisierung wurde hinzugefügt, um anzugeben, wann das System Erkennungsergebnisse bereinigen soll. |
|                                          | Die Einstellung wird in Stunden angegeben, und der Standardwert ist 48 Stunden.   |
|                                          |   Durch die Einstellung kann der Speicherverbrauch für die Speicherung der Ergebnisse reduziert werden. Dies gilt insbesondere dann, wenn für den Container ein In-Memory-Speicher verwendet wird.  |
|                                          |    * Beispiel 1. ReadEngineConfig:ResultExpirationPeriod=1: Das System löscht das Erkennungsergebnis 1 Stunde nach dem Prozess.   |
|                                          |    * Beispiel 2. ReadEngineConfig:ResultExpirationPeriod=0: Das System löscht das Erkennungsergebnis nach dem Abrufen des Ergebnisses.  |
|                                          | Es wurde ein interner Serverfehler mit dem Code 500 korrigiert, wenn ein ungültiges Imageformat an das System übermittelt wurde. Nun wird ein 400-Fehler zurückgegeben:   |
|                                          | `{`  |
|                                          | `"error": {`  |
|                                          |      `"code": "InvalidImageSize",`  |
|                                          |      `"message": "Image must be between 1024 and 209715200 bytes."`  |
|                                          |          `}`  |
|                                          | `}`  |
| `1.1.011580001-amd64-preview` |       |
| `1.1.009920003-amd64-preview` |       |
| `1.1.009910003-amd64-preview` |       |

## <a name="face"></a>Gesicht

Das Containerimage [Gesichtserkennung][fa-containers] befindet sich in der `containerpreview.azurecr.io`-Containerregistrierung. Es befindet sich im Repository `microsoft` und trägt den Namen `cognitive-services-face`. Der vollqualifizierte Containerimagename lautet `containerpreview.azurecr.io/microsoft/cognitive-services-face`.

Dieses Containerimage verfügt über die folgenden Tags:

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008710001-amd64-preview` |       |
| `1.1.007750002-amd64-preview` |       |
| `1.1.007360001-amd64-preview` |       |
| `1.1.006770001-amd64-preview` |       |
| `1.1.006490002-amd64-preview` |       |
| `1.0.005940002-amd64-preview` |       |
| `1.0.005550001-amd64-preview` |       |

## <a name="form-recognizer"></a>Formularerkennung

Das Containerimage [Formularerkennung][fr-containers] befindet sich in der `containerpreview.azurecr.io`-Containerregistrierung. Es befindet sich im Repository `microsoft` und trägt den Namen `cognitive-services-form-recognizer`. Der vollqualifizierte Containerimagename lautet `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer`.

Dieses Containerimage verfügt über die folgenden Tags:

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008640001-amd64-preview` |       |
| `1.1.008510001-amd64-preview` |       |

## <a name="language-understanding-luis"></a>Language Understanding (LUIS)

Das Containerimage [LUIS][lu-containers] befindet sich im `mcr.microsoft.com`-Containerregistrierungssyndikat. Es befindet sich im Repository `azure-cognitive-services` und trägt den Namen `luis`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/luis`.

Dieses Containerimage verfügt über die folgenden Tags:

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.010330004-amd64-preview` |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008710001-amd64-preview` |       |
| `1.1.008510001-amd64-preview` |       |
| `1.1.008010002-amd64-preview` |       |
| `1.1.007750002-amd64-preview` |       |
| `1.1.007360001-amd64-preview` |       |
| `1.1.007020001-amd64-preview` |       |

## <a name="custom-speech-to-text"></a>Benutzerdefinierte Spracherkennung

Das Containerimage [Benutzerdefinierte Spracherkennung][sp-cstt] befindet sich in der `containerpreview.azurecr.io`-Containerregistrierung. Es befindet sich im Repository `microsoft` und trägt den Namen `cognitive-services-custom-speech-to-text`. Der vollqualifizierte Containerimagename lautet `containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text`.

Dieses Containerimage verfügt über die folgenden Tags:

| Imagetags            | Notizen |
|-----------------------|:------|
| `latest`              |       |
| `2.5.0-amd64`         |       |
| `2.4.0-amd64-preview` |       |
| `2.3.1-amd64-preview` |       | 
| `2.3.0-amd64-preview` |       |
| `2.2.0-amd64-preview` |       |
| `2.1.1-amd64-preview` |       |
| `2.1.0-amd64-preview` |       |
| `2.0.2-amd64-preview` |       |
| `2.0.0-amd64-preview` |       |

## <a name="custom-text-to-speech"></a>Benutzerdefinierte Sprachsynthese

Das Containerimage [Benutzerdefinierte Sprachsynthese][sp-ctts] befindet sich in der `containerpreview.azurecr.io`-Containerregistrierung. Es befindet sich im Repository `microsoft` und trägt den Namen `cognitive-services-custom-text-to-speech`. Der vollqualifizierte Containerimagename lautet `containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech`.

Dieses Containerimage verfügt über die folgenden Tags:

| Imagetags            | Notizen |
|-----------------------|:------|
| `latest`              |       |
| `1.7.0-amd64`         |       |
| `1.6.0-amd64-preview` |       |
| `1.6.0-amd64-preview` |       |
| `1.5.0-amd64-preview` |       |
| `1.4.0-amd64-preview` |       |
| `1.3.0-amd64-preview` |       |

## <a name="speech-to-text"></a>Spracherkennung

Das Containerimage [Spracherkennung][sp-stt] befindet sich in der `containerpreview.azurecr.io`-Containerregistrierung. Es befindet sich im Repository `microsoft` und trägt den Namen `cognitive-services-speech-to-text`. Der vollqualifizierte Containerimagename lautet `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text`.
Images von Sprache-in-Text v2.5.0 werden in der Region *US Government Virginia* unterstützt. Verwenden Sie den Abrechnungsendpunkt sowie die API-Schlüssel der Region *US Government Virginia*, um es zu testen.

Dieses Containerimage verfügt über die folgenden Tags:

| Imagetags                  | Notizen                                    |
|-----------------------------|:-----------------------------------------|
| `latest`                    | Containerimage mit dem Gebietsschema `en-US`. |
| `2.5.0-amd64-ar-ae`         | Containerimage mit dem Gebietsschema `ar-AE`. |
| `2.5.0-amd64-ar-eg`         | Containerimage mit dem Gebietsschema `ar-EG`. |
| `2.5.0-amd64-ar-kw`         | Containerimage mit dem Gebietsschema `ar-KW`. |
| `2.5.0-amd64-ar-qa`         | Containerimage mit dem Gebietsschema `ar-QA`. |
| `2.5.0-amd64-ar-sa`         | Containerimage mit dem Gebietsschema `ar-SA`. |
| `2.5.0-amd64-ca-es`         | Containerimage mit dem Gebietsschema `ca-ES`. |
| `2.5.0-amd64-da-dk`         | Containerimage mit dem Gebietsschema `da-DK`. |
| `2.5.0-amd64-de-de`         | Containerimage mit dem Gebietsschema `de-DE`. |
| `2.5.0-amd64-en-au`         | Containerimage mit dem Gebietsschema `en-AU`. |
| `2.5.0-amd64-en-ca`         | Containerimage mit dem Gebietsschema `en-CA`. |
| `2.5.0-amd64-en-gb`         | Containerimage mit dem Gebietsschema `en-GB`. |
| `2.5.0-amd64-en-in`         | Containerimage mit dem Gebietsschema `en-IN`. |
| `2.5.0-amd64-en-nz`         | Containerimage mit dem Gebietsschema `en-NZ`. |
| `2.5.0-amd64-en-us`         | Containerimage mit dem Gebietsschema `en-US`. |
| `2.5.0-amd64-es-es`         | Containerimage mit dem Gebietsschema `es-ES`. |
| `2.5.0-amd64-es-mx`         | Containerimage mit dem Gebietsschema `es-MX`. |
| `2.5.0-amd64-fi-fi`         | Containerimage mit dem Gebietsschema `fi-FI`. |
| `2.5.0-amd64-fr-ca`         | Containerimage mit dem Gebietsschema `fr-CA`. |
| `2.5.0-amd64-fr-fr`         | Containerimage mit dem Gebietsschema `fr-FR`. |
| `2.5.0-amd64-gu-in`         | Containerimage mit dem Gebietsschema `gu-IN`. |
| `2.5.0-amd64-hi-in`         | Containerimage mit dem Gebietsschema `hi-IN`. |
| `2.5.0-amd64-it-it`         | Containerimage mit dem Gebietsschema `it-IT`. |
| `2.5.0-amd64-ja-jp`         | Containerimage mit dem Gebietsschema `ja-JP`. |
| `2.5.0-amd64-ko-kr`         | Containerimage mit dem Gebietsschema `ko-KR`. |
| `2.5.0-amd64-mr-in`         | Containerimage mit dem Gebietsschema `mr-IN`. |
| `2.5.0-amd64-nb-no`         | Containerimage mit dem Gebietsschema `nb-NO`. |
| `2.5.0-amd64-nl-nl`         | Containerimage mit dem Gebietsschema `nl-NL`. |
| `2.5.0-amd64-pl-pl`         | Containerimage mit dem Gebietsschema `pl-PL`. |
| `2.5.0-amd64-pt-br`         | Containerimage mit dem Gebietsschema `pt-BR`. |
| `2.5.0-amd64-pt-pt`         | Containerimage mit dem Gebietsschema `pt-PT`. |
| `2.5.0-amd64-ru-ru`         | Containerimage mit dem Gebietsschema `ru-RU`. |
| `2.5.0-amd64-sv-se`         | Containerimage mit dem Gebietsschema `sv-SE`. |
| `2.5.0-amd64-ta-in`         | Containerimage mit dem Gebietsschema `ta-IN`. |
| `2.5.0-amd64-te-in`         | Containerimage mit dem Gebietsschema `te-IN`. |
| `2.5.0-amd64-th-th`         | Containerimage mit dem Gebietsschema `th-TH`. |
| `2.5.0-amd64-tr-tr`         | Containerimage mit dem Gebietsschema `tr-TR`. |
| `2.5.0-amd64-zh-cn`         | Containerimage mit dem Gebietsschema `zh-CN`. |
| `2.5.0-amd64-zh-hk`         | Containerimage mit dem Gebietsschema `zh-HK`. |
| `2.5.0-amd64-zh-tw`         | Containerimage mit dem Gebietsschema `zh-TW`. |
| `2.4.0-amd64-ar-ae-preview` | Containerimage mit dem Gebietsschema `ar-AE`. |
| `2.4.0-amd64-ar-eg-preview` | Containerimage mit dem Gebietsschema `ar-EG`. |
| `2.4.0-amd64-ar-kw-preview` | Containerimage mit dem Gebietsschema `ar-KW`. |
| `2.4.0-amd64-ar-qa-preview` | Containerimage mit dem Gebietsschema `ar-QA`. |
| `2.4.0-amd64-ar-sa-preview` | Containerimage mit dem Gebietsschema `ar-SA`. |
| `2.4.0-amd64-ca-es-preview` | Containerimage mit dem Gebietsschema `ca-ES`. |
| `2.4.0-amd64-da-dk-preview` | Containerimage mit dem Gebietsschema `da-DK`. |
| `2.4.0-amd64-de-de-preview` | Containerimage mit dem Gebietsschema `de-DE`. |
| `2.4.0-amd64-en-au-preview` | Containerimage mit dem Gebietsschema `en-AU`. |
| `2.4.0-amd64-en-ca-preview` | Containerimage mit dem Gebietsschema `en-CA`. |
| `2.4.0-amd64-en-gb-preview` | Containerimage mit dem Gebietsschema `en-GB`. |
| `2.4.0-amd64-en-in-preview` | Containerimage mit dem Gebietsschema `en-IN`. |
| `2.4.0-amd64-en-nz-preview` | Containerimage mit dem Gebietsschema `en-NZ`. |
| `2.4.0-amd64-en-us-preview` | Containerimage mit dem Gebietsschema `en-US`. |
| `2.4.0-amd64-es-es-preview` | Containerimage mit dem Gebietsschema `es-ES`. |
| `2.4.0-amd64-es-mx-preview` | Containerimage mit dem Gebietsschema `es-MX`. |
| `2.4.0-amd64-fi-fi-preview` | Containerimage mit dem Gebietsschema `fi-FI`. |
| `2.4.0-amd64-fr-ca-preview` | Containerimage mit dem Gebietsschema `fr-CA`. |
| `2.4.0-amd64-fr-fr-preview` | Containerimage mit dem Gebietsschema `fr-FR`. |
| `2.4.0-amd64-gu-in-preview` | Containerimage mit dem Gebietsschema `gu-IN`. |
| `2.4.0-amd64-hi-in-preview` | Containerimage mit dem Gebietsschema `hi-IN`. |
| `2.4.0-amd64-it-it-preview` | Containerimage mit dem Gebietsschema `it-IT`. |
| `2.4.0-amd64-ja-jp-preview` | Containerimage mit dem Gebietsschema `ja-JP`. |
| `2.4.0-amd64-ko-kr-preview` | Containerimage mit dem Gebietsschema `ko-KR`. |
| `2.4.0-amd64-mr-in-preview` | Containerimage mit dem Gebietsschema `mr-IN`. |
| `2.4.0-amd64-nb-no-preview` | Containerimage mit dem Gebietsschema `nb-NO`. |
| `2.4.0-amd64-nl-nl-preview` | Containerimage mit dem Gebietsschema `nl-NL`. |
| `2.4.0-amd64-pl-pl-preview` | Containerimage mit dem Gebietsschema `pl-PL`. |
| `2.4.0-amd64-pt-br-preview` | Containerimage mit dem Gebietsschema `pt-BR`. |
| `2.4.0-amd64-pt-pt-preview` | Containerimage mit dem Gebietsschema `pt-PT`. |
| `2.4.0-amd64-ru-ru-preview` | Containerimage mit dem Gebietsschema `ru-RU`. |
| `2.4.0-amd64-sv-se-preview` | Containerimage mit dem Gebietsschema `sv-SE`. |
| `2.4.0-amd64-ta-in-preview` | Containerimage mit dem Gebietsschema `ta-IN`. |
| `2.4.0-amd64-te-in-preview` | Containerimage mit dem Gebietsschema `te-IN`. |
| `2.4.0-amd64-th-th-preview` | Containerimage mit dem Gebietsschema `th-TH`. |
| `2.4.0-amd64-tr-tr-preview` | Containerimage mit dem Gebietsschema `tr-TR`. |
| `2.4.0-amd64-zh-cn-preview` | Containerimage mit dem Gebietsschema `zh-CN`. |
| `2.4.0-amd64-zh-hk-preview` | Containerimage mit dem Gebietsschema `zh-HK`. |
| `2.4.0-amd64-zh-tw-preview` | Containerimage mit dem Gebietsschema `zh-TW`. |
| `2.3.1-amd64-ar-ae-preview` | Containerimage mit dem Gebietsschema `ar-AE`. |
| `2.3.1-amd64-ar-eg-preview` | Containerimage mit dem Gebietsschema `ar-EG`. |
| `2.3.1-amd64-ar-kw-preview` | Containerimage mit dem Gebietsschema `ar-KW`. |
| `2.3.1-amd64-ar-qa-preview` | Containerimage mit dem Gebietsschema `ar-QA`. |
| `2.3.1-amd64-ar-sa-preview` | Containerimage mit dem Gebietsschema `ar-SA`. |
| `2.3.1-amd64-ca-es-preview` | Containerimage mit dem Gebietsschema `ca-ES`. |
| `2.3.1-amd64-da-dk-preview` | Containerimage mit dem Gebietsschema `da-DK`. |
| `2.3.1-amd64-de-de-preview` | Containerimage mit dem Gebietsschema `de-DE`. |
| `2.3.1-amd64-en-au-preview` | Containerimage mit dem Gebietsschema `en-AU`. |
| `2.3.1-amd64-en-ca-preview` | Containerimage mit dem Gebietsschema `en-CA`. |
| `2.3.1-amd64-en-gb-preview` | Containerimage mit dem Gebietsschema `en-GB`. |
| `2.3.1-amd64-en-in-preview` | Containerimage mit dem Gebietsschema `en-IN`. |
| `2.3.1-amd64-en-nz-preview` | Containerimage mit dem Gebietsschema `en-NZ`. |
| `2.3.1-amd64-en-us-preview` | Containerimage mit dem Gebietsschema `en-US`. |
| `2.3.1-amd64-es-es-preview` | Containerimage mit dem Gebietsschema `es-ES`. |
| `2.3.1-amd64-es-mx-preview` | Containerimage mit dem Gebietsschema `es-MX`. |
| `2.3.1-amd64-fi-fi-preview` | Containerimage mit dem Gebietsschema `fi-FI`. |
| `2.3.1-amd64-fr-ca-preview` | Containerimage mit dem Gebietsschema `fr-CA`. |
| `2.3.1-amd64-fr-fr-preview` | Containerimage mit dem Gebietsschema `fr-FR`. |
| `2.3.1-amd64-gu-in-preview` | Containerimage mit dem Gebietsschema `gu-IN`. |
| `2.3.1-amd64-hi-in-preview` | Containerimage mit dem Gebietsschema `hi-IN`. |
| `2.3.1-amd64-it-it-preview` | Containerimage mit dem Gebietsschema `it-IT`. |
| `2.3.1-amd64-ja-jp-preview` | Containerimage mit dem Gebietsschema `ja-JP`. |
| `2.3.1-amd64-ko-kr-preview` | Containerimage mit dem Gebietsschema `ko-KR`. |
| `2.3.1-amd64-mr-in-preview` | Containerimage mit dem Gebietsschema `mr-IN`. |
| `2.3.1-amd64-nb-no-preview` | Containerimage mit dem Gebietsschema `nb-NO`. |
| `2.3.1-amd64-nl-nl-preview` | Containerimage mit dem Gebietsschema `nl-NL`. |
| `2.3.1-amd64-pl-pl-preview` | Containerimage mit dem Gebietsschema `pl-PL`. |
| `2.3.1-amd64-pt-br-preview` | Containerimage mit dem Gebietsschema `pt-BR`. |
| `2.3.1-amd64-pt-pt-preview` | Containerimage mit dem Gebietsschema `pt-PT`. |
| `2.3.1-amd64-ru-ru-preview` | Containerimage mit dem Gebietsschema `ru-RU`. |
| `2.3.1-amd64-sv-se-preview` | Containerimage mit dem Gebietsschema `sv-SE`. |
| `2.3.1-amd64-ta-in-preview` | Containerimage mit dem Gebietsschema `ta-IN`. |
| `2.3.1-amd64-te-in-preview` | Containerimage mit dem Gebietsschema `te-IN`. |
| `2.3.1-amd64-th-th-preview` | Containerimage mit dem Gebietsschema `th-TH`. |
| `2.3.1-amd64-tr-tr-preview` | Containerimage mit dem Gebietsschema `tr-TR`. |
| `2.3.1-amd64-zh-cn-preview` | Containerimage mit dem Gebietsschema `zh-CN`. |
| `2.3.1-amd64-zh-hk-preview` | Containerimage mit dem Gebietsschema `zh-HK`. |
| `2.3.1-amd64-zh-tw-preview` | Containerimage mit dem Gebietsschema `zh-TW`. |
| `2.3.0-amd64-ar-ae-preview` | Containerimage mit dem Gebietsschema `ar-AE`. |
| `2.3.0-amd64-ar-eg-preview` | Containerimage mit dem Gebietsschema `ar-EG`. |
| `2.3.0-amd64-ar-kw-preview` | Containerimage mit dem Gebietsschema `ar-KW`. |
| `2.3.0-amd64-ar-qa-preview` | Containerimage mit dem Gebietsschema `ar-QA`. |
| `2.3.0-amd64-ar-sa-preview` | Containerimage mit dem Gebietsschema `ar-SA`. |
| `2.3.0-amd64-ca-es-preview` | Containerimage mit dem Gebietsschema `ca-ES`. |
| `2.3.0-amd64-da-dk-preview` | Containerimage mit dem Gebietsschema `da-DK`. |
| `2.3.0-amd64-de-de-preview` | Containerimage mit dem Gebietsschema `de-DE`. |
| `2.3.0-amd64-en-au-preview` | Containerimage mit dem Gebietsschema `en-AU`. |
| `2.3.0-amd64-en-ca-preview` | Containerimage mit dem Gebietsschema `en-CA`. |
| `2.3.0-amd64-en-gb-preview` | Containerimage mit dem Gebietsschema `en-GB`. |
| `2.3.0-amd64-en-in-preview` | Containerimage mit dem Gebietsschema `en-IN`. |
| `2.3.0-amd64-en-nz-preview` | Containerimage mit dem Gebietsschema `en-NZ`. |
| `2.3.0-amd64-en-us-preview` | Containerimage mit dem Gebietsschema `en-US`. |
| `2.3.0-amd64-es-es-preview` | Containerimage mit dem Gebietsschema `es-ES`. |
| `2.3.0-amd64-es-mx-preview` | Containerimage mit dem Gebietsschema `es-MX`. |
| `2.3.0-amd64-fi-fi-preview` | Containerimage mit dem Gebietsschema `fi-FI`. |
| `2.3.0-amd64-fr-ca-preview` | Containerimage mit dem Gebietsschema `fr-CA`. |
| `2.3.0-amd64-fr-fr-preview` | Containerimage mit dem Gebietsschema `fr-FR`. |
| `2.3.0-amd64-gu-in-preview` | Containerimage mit dem Gebietsschema `gu-IN`. |
| `2.3.0-amd64-hi-in-preview` | Containerimage mit dem Gebietsschema `hi-IN`. |
| `2.3.0-amd64-it-it-preview` | Containerimage mit dem Gebietsschema `it-IT`. |
| `2.3.0-amd64-ja-jp-preview` | Containerimage mit dem Gebietsschema `ja-JP`. |
| `2.3.0-amd64-ko-kr-preview` | Containerimage mit dem Gebietsschema `ko-KR`. |
| `2.3.0-amd64-mr-in-preview` | Containerimage mit dem Gebietsschema `mr-IN`. |
| `2.3.0-amd64-nb-no-preview` | Containerimage mit dem Gebietsschema `nb-NO`. |
| `2.3.0-amd64-nl-nl-preview` | Containerimage mit dem Gebietsschema `nl-NL`. |
| `2.3.0-amd64-pl-pl-preview` | Containerimage mit dem Gebietsschema `pl-PL`. |
| `2.3.0-amd64-pt-br-preview` | Containerimage mit dem Gebietsschema `pt-BR`. |
| `2.3.0-amd64-pt-pt-preview` | Containerimage mit dem Gebietsschema `pt-PT`. |
| `2.3.0-amd64-ru-ru-preview` | Containerimage mit dem Gebietsschema `ru-RU`. |
| `2.3.0-amd64-sv-se-preview` | Containerimage mit dem Gebietsschema `sv-SE`. |
| `2.3.0-amd64-ta-in-preview` | Containerimage mit dem Gebietsschema `ta-IN`. |
| `2.3.0-amd64-te-in-preview` | Containerimage mit dem Gebietsschema `te-IN`. |
| `2.3.0-amd64-th-th-preview` | Containerimage mit dem Gebietsschema `th-TH`. |
| `2.3.0-amd64-tr-tr-preview` | Containerimage mit dem Gebietsschema `tr-TR`. |
| `2.3.0-amd64-zh-cn-preview` | Containerimage mit dem Gebietsschema `zh-CN`. |
| `2.3.0-amd64-zh-hk-preview` | Containerimage mit dem Gebietsschema `zh-HK`. |
| `2.3.0-amd64-zh-tw-preview` | Containerimage mit dem Gebietsschema `zh-TW`. |
| `2.2.0-amd64-ar-ae-preview` | Containerimage mit dem Gebietsschema `ar-AE`. |
| `2.2.0-amd64-ar-eg-preview` | Containerimage mit dem Gebietsschema `ar-EG`. |
| `2.2.0-amd64-ar-kw-preview` | Containerimage mit dem Gebietsschema `ar-KW`. |
| `2.2.0-amd64-ar-qa-preview` | Containerimage mit dem Gebietsschema `ar-QA`. |
| `2.2.0-amd64-ar-sa-preview` | Containerimage mit dem Gebietsschema `ar-SA`. |
| `2.2.0-amd64-ca-es-preview` | Containerimage mit dem Gebietsschema `ca-ES`. |
| `2.2.0-amd64-da-dk-preview` | Containerimage mit dem Gebietsschema `da-DK`. |
| `2.2.0-amd64-de-de-preview` | Containerimage mit dem Gebietsschema `de-DE`. |
| `2.2.0-amd64-en-au-preview` | Containerimage mit dem Gebietsschema `en-AU`. |
| `2.2.0-amd64-en-ca-preview` | Containerimage mit dem Gebietsschema `en-CA`. |
| `2.2.0-amd64-en-gb-preview` | Containerimage mit dem Gebietsschema `en-GB`. |
| `2.2.0-amd64-en-in-preview` | Containerimage mit dem Gebietsschema `en-IN`. |
| `2.2.0-amd64-en-nz-preview` | Containerimage mit dem Gebietsschema `en-NZ`. |
| `2.2.0-amd64-en-us-preview` | Containerimage mit dem Gebietsschema `en-US`. |
| `2.2.0-amd64-es-es-preview` | Containerimage mit dem Gebietsschema `es-ES`. |
| `2.2.0-amd64-es-mx-preview` | Containerimage mit dem Gebietsschema `es-MX`. |
| `2.2.0-amd64-fi-fi-preview` | Containerimage mit dem Gebietsschema `fi-FI`. |
| `2.2.0-amd64-fr-ca-preview` | Containerimage mit dem Gebietsschema `fr-CA`. |
| `2.2.0-amd64-fr-fr-preview` | Containerimage mit dem Gebietsschema `fr-FR`. |
| `2.2.0-amd64-gu-in-preview` | Containerimage mit dem Gebietsschema `gu-IN`. |
| `2.2.0-amd64-hi-in-preview` | Containerimage mit dem Gebietsschema `hi-IN`. |
| `2.2.0-amd64-it-it-preview` | Containerimage mit dem Gebietsschema `it-IT`. |
| `2.2.0-amd64-ja-jp-preview` | Containerimage mit dem Gebietsschema `ja-JP`. |
| `2.2.0-amd64-ko-kr-preview` | Containerimage mit dem Gebietsschema `ko-KR`. |
| `2.2.0-amd64-mr-in-preview` | Containerimage mit dem Gebietsschema `mr-IN`. |
| `2.2.0-amd64-nb-no-preview` | Containerimage mit dem Gebietsschema `nb-NO`. |
| `2.2.0-amd64-nl-nl-preview` | Containerimage mit dem Gebietsschema `nl-NL`. |
| `2.2.0-amd64-pl-pl-preview` | Containerimage mit dem Gebietsschema `pl-PL`. |
| `2.2.0-amd64-pt-br-preview` | Containerimage mit dem Gebietsschema `pt-BR`. |
| `2.2.0-amd64-pt-pt-preview` | Containerimage mit dem Gebietsschema `pt-PT`. |
| `2.2.0-amd64-ru-ru-preview` | Containerimage mit dem Gebietsschema `ru-RU`. |
| `2.2.0-amd64-sv-se-preview` | Containerimage mit dem Gebietsschema `sv-SE`. |
| `2.2.0-amd64-ta-in-preview` | Containerimage mit dem Gebietsschema `ta-IN`. |
| `2.2.0-amd64-te-in-preview` | Containerimage mit dem Gebietsschema `te-IN`. |
| `2.2.0-amd64-th-th-preview` | Containerimage mit dem Gebietsschema `th-TH`. |
| `2.2.0-amd64-tr-tr-preview` | Containerimage mit dem Gebietsschema `tr-TR`. |
| `2.2.0-amd64-zh-cn-preview` | Containerimage mit dem Gebietsschema `zh-CN`. |
| `2.2.0-amd64-zh-hk-preview` | Containerimage mit dem Gebietsschema `zh-HK`. |
| `2.2.0-amd64-zh-tw-preview` | Containerimage mit dem Gebietsschema `zh-TW`. |
| `2.1.1-amd64-en-us-preview` | Containerimage mit dem Gebietsschema `en-US`. |
| `2.1.1-amd64-ar-ae-preview` | Containerimage mit dem Gebietsschema `ar-AE`. |
| `2.1.1-amd64-ar-eg-preview` | Containerimage mit dem Gebietsschema `ar-EG`. |
| `2.1.1-amd64-ar-kw-preview` | Containerimage mit dem Gebietsschema `ar-KW`. |
| `2.1.1-amd64-ar-qa-preview` | Containerimage mit dem Gebietsschema `ar-QA`. |
| `2.1.1-amd64-ar-sa-preview` | Containerimage mit dem Gebietsschema `ar-SA`. |
| `2.1.1-amd64-ca-es-preview` | Containerimage mit dem Gebietsschema `ca-ES`. |
| `2.1.1-amd64-da-dk-preview` | Containerimage mit dem Gebietsschema `da-DK`. |
| `2.1.1-amd64-de-de-preview` | Containerimage mit dem Gebietsschema `de-DE`. |
| `2.1.1-amd64-en-au-preview` | Containerimage mit dem Gebietsschema `en-AU`. |
| `2.1.1-amd64-en-ca-preview` | Containerimage mit dem Gebietsschema `en-CA`. |
| `2.1.1-amd64-en-gb-preview` | Containerimage mit dem Gebietsschema `en-GB`. |
| `2.1.1-amd64-en-in-preview` | Containerimage mit dem Gebietsschema `en-IN`. |
| `2.1.1-amd64-en-nz-preview` | Containerimage mit dem Gebietsschema `en-NZ`. |
| `2.1.1-amd64-en-us-preview` | Containerimage mit dem Gebietsschema `en-US`. |
| `2.1.1-amd64-es-es-preview` | Containerimage mit dem Gebietsschema `es-ES`. |
| `2.1.1-amd64-es-mx-preview` | Containerimage mit dem Gebietsschema `es-MX`. |
| `2.1.1-amd64-fi-fi-preview` | Containerimage mit dem Gebietsschema `fi-FI`. |
| `2.1.1-amd64-fr-ca-preview` | Containerimage mit dem Gebietsschema `fr-CA`. |
| `2.1.1-amd64-fr-fr-preview` | Containerimage mit dem Gebietsschema `fr-FR`. |
| `2.1.1-amd64-gu-in-preview` | Containerimage mit dem Gebietsschema `gu-IN`. |
| `2.1.1-amd64-hi-in-preview` | Containerimage mit dem Gebietsschema `hi-IN`. |
| `2.1.1-amd64-it-it-preview` | Containerimage mit dem Gebietsschema `it-IT`. |
| `2.1.1-amd64-ja-jp-preview` | Containerimage mit dem Gebietsschema `ja-JP`. |
| `2.1.1-amd64-ko-kr-preview` | Containerimage mit dem Gebietsschema `ko-KR`. |
| `2.1.1-amd64-mr-in-preview` | Containerimage mit dem Gebietsschema `mr-IN`. |
| `2.1.1-amd64-nb-no-preview` | Containerimage mit dem Gebietsschema `nb-NO`. |
| `2.1.1-amd64-nl-nl-preview` | Containerimage mit dem Gebietsschema `nl-NL`. |
| `2.1.1-amd64-pl-pl-preview` | Containerimage mit dem Gebietsschema `pl-PL`. |
| `2.1.1-amd64-pt-br-preview` | Containerimage mit dem Gebietsschema `pt-BR`. |
| `2.1.1-amd64-pt-pt-preview` | Containerimage mit dem Gebietsschema `pt-PT`. |
| `2.1.1-amd64-ru-ru-preview` | Containerimage mit dem Gebietsschema `ru-RU`. |
| `2.1.1-amd64-sv-se-preview` | Containerimage mit dem Gebietsschema `sv-SE`. |
| `2.1.1-amd64-ta-in-preview` | Containerimage mit dem Gebietsschema `ta-IN`. |
| `2.1.1-amd64-te-in-preview` | Containerimage mit dem Gebietsschema `te-IN`. |
| `2.1.1-amd64-th-th-preview` | Containerimage mit dem Gebietsschema `th-TH`. |
| `2.1.1-amd64-tr-tr-preview` | Containerimage mit dem Gebietsschema `tr-TR`. |
| `2.1.1-amd64-zh-cn-preview` | Containerimage mit dem Gebietsschema `zh-CN`. |
| `2.1.1-amd64-zh-hk-preview` | Containerimage mit dem Gebietsschema `zh-HK`. |
| `2.1.1-amd64-zh-tw-preview` | Containerimage mit dem Gebietsschema `zh-TW`. |
| `2.1.0-amd64-ar-ae-preview` | Containerimage mit dem Gebietsschema `ar-AE`. |
| `2.1.0-amd64-ar-eg-preview` | Containerimage mit dem Gebietsschema `ar-EG`. |
| `2.1.0-amd64-ar-kw-preview` | Containerimage mit dem Gebietsschema `ar-KW`. |
| `2.1.0-amd64-ar-qa-preview` | Containerimage mit dem Gebietsschema `ar-QA`. |
| `2.1.0-amd64-ar-sa-preview` | Containerimage mit dem Gebietsschema `ar-SA`. |
| `2.1.0-amd64-ca-es-preview` | Containerimage mit dem Gebietsschema `ca-ES`. |
| `2.1.0-amd64-da-dk-preview` | Containerimage mit dem Gebietsschema `da-DK`. |
| `2.1.0-amd64-de-de-preview` | Containerimage mit dem Gebietsschema `de-DE`. |
| `2.1.0-amd64-en-au-preview` | Containerimage mit dem Gebietsschema `en-AU`. |
| `2.1.0-amd64-en-ca-preview` | Containerimage mit dem Gebietsschema `en-CA`. |
| `2.1.0-amd64-en-gb-preview` | Containerimage mit dem Gebietsschema `en-GB`. |
| `2.1.0-amd64-en-in-preview` | Containerimage mit dem Gebietsschema `en-IN`. |
| `2.1.0-amd64-en-nz-preview` | Containerimage mit dem Gebietsschema `en-NZ`. |
| `2.1.0-amd64-en-us-preview` | Containerimage mit dem Gebietsschema `en-US`. |
| `2.1.0-amd64-es-es-preview` | Containerimage mit dem Gebietsschema `es-ES`. |
| `2.1.0-amd64-es-mx-preview` | Containerimage mit dem Gebietsschema `es-MX`. |
| `2.1.0-amd64-fi-fi-preview` | Containerimage mit dem Gebietsschema `fi-FI`. |
| `2.1.0-amd64-fr-ca-preview` | Containerimage mit dem Gebietsschema `fr-CA`. |
| `2.1.0-amd64-fr-fr-preview` | Containerimage mit dem Gebietsschema `fr-FR`. |
| `2.1.0-amd64-gu-in-preview` | Containerimage mit dem Gebietsschema `gu-IN`. |
| `2.1.0-amd64-hi-in-preview` | Containerimage mit dem Gebietsschema `hi-IN`. |
| `2.1.0-amd64-it-it-preview` | Containerimage mit dem Gebietsschema `it-IT`. |
| `2.1.0-amd64-ja-jp-preview` | Containerimage mit dem Gebietsschema `ja-JP`. |
| `2.1.0-amd64-ko-kr-preview` | Containerimage mit dem Gebietsschema `ko-KR`. |
| `2.1.0-amd64-mr-in-preview` | Containerimage mit dem Gebietsschema `mr-IN`. |
| `2.1.0-amd64-nb-no-preview` | Containerimage mit dem Gebietsschema `nb-NO`. |
| `2.1.0-amd64-nl-nl-preview` | Containerimage mit dem Gebietsschema `nl-NL`. |
| `2.1.0-amd64-pl-pl-preview` | Containerimage mit dem Gebietsschema `pl-PL`. |
| `2.1.0-amd64-pt-br-preview` | Containerimage mit dem Gebietsschema `pt-BR`. |
| `2.1.0-amd64-pt-pt-preview` | Containerimage mit dem Gebietsschema `pt-PT`. |
| `2.1.0-amd64-ru-ru-preview` | Containerimage mit dem Gebietsschema `ru-RU`. |
| `2.1.0-amd64-sv-se-preview` | Containerimage mit dem Gebietsschema `sv-SE`. |
| `2.1.0-amd64-ta-in-preview` | Containerimage mit dem Gebietsschema `ta-IN`. |
| `2.1.0-amd64-te-in-preview` | Containerimage mit dem Gebietsschema `te-IN`. |
| `2.1.0-amd64-th-th-preview` | Containerimage mit dem Gebietsschema `th-TH`. |
| `2.1.0-amd64-tr-tr-preview` | Containerimage mit dem Gebietsschema `tr-TR`. |
| `2.1.0-amd64-zh-cn-preview` | Containerimage mit dem Gebietsschema `zh-CN`. |
| `2.1.0-amd64-zh-hk-preview` | Containerimage mit dem Gebietsschema `zh-HK`. |
| `2.1.0-amd64-zh-tw-preview` | Containerimage mit dem Gebietsschema `zh-TW`. |
| `2.0.3-amd64-ja-jp-preview` | Containerimage mit dem Gebietsschema `ja-JP`. |
| `2.0.2-amd64-ar-ae-preview` | Containerimage mit dem Gebietsschema `ar-AE`. |
| `2.0.2-amd64-ar-eg-preview` | Containerimage mit dem Gebietsschema `ar-EG`. |
| `2.0.2-amd64-ar-kw-preview` | Containerimage mit dem Gebietsschema `ar-KW`. |
| `2.0.2-amd64-ar-qa-preview` | Containerimage mit dem Gebietsschema `ar-QA`. |
| `2.0.2-amd64-ar-sa-preview` | Containerimage mit dem Gebietsschema `ar-SA`. |
| `2.0.2-amd64-ca-es-preview` | Containerimage mit dem Gebietsschema `ca-ES`. |
| `2.0.2-amd64-da-dk-preview` | Containerimage mit dem Gebietsschema `da-DK`. |
| `2.0.2-amd64-de-de-preview` | Containerimage mit dem Gebietsschema `de-DE`. |
| `2.0.2-amd64-en-au-preview` | Containerimage mit dem Gebietsschema `en-AU`. |
| `2.0.2-amd64-en-ca-preview` | Containerimage mit dem Gebietsschema `en-CA`. |
| `2.0.2-amd64-en-gb-preview` | Containerimage mit dem Gebietsschema `en-GB`. |
| `2.0.2-amd64-en-in-preview` | Containerimage mit dem Gebietsschema `en-IN`. |
| `2.0.2-amd64-en-nz-preview` | Containerimage mit dem Gebietsschema `en-NZ`. |
| `2.0.2-amd64-en-us-preview` | Containerimage mit dem Gebietsschema `en-US`. |
| `2.0.2-amd64-es-es-preview` | Containerimage mit dem Gebietsschema `es-ES`. |
| `2.0.2-amd64-es-mx-preview` | Containerimage mit dem Gebietsschema `es-MX`. |
| `2.0.2-amd64-fi-fi-preview` | Containerimage mit dem Gebietsschema `fi-FI`. |
| `2.0.2-amd64-fr-ca-preview` | Containerimage mit dem Gebietsschema `fr-CA`. |
| `2.0.2-amd64-fr-fr-preview` | Containerimage mit dem Gebietsschema `fr-FR`. |
| `2.0.2-amd64-gu-in-preview` | Containerimage mit dem Gebietsschema `gu-IN`. |
| `2.0.2-amd64-hi-in-preview` | Containerimage mit dem Gebietsschema `hi-IN`. |
| `2.0.2-amd64-it-it-preview` | Containerimage mit dem Gebietsschema `it-IT`. |
| `2.0.2-amd64-ja-jp-preview` | Containerimage mit dem Gebietsschema `ja-JP`. |
| `2.0.2-amd64-ko-kr-preview` | Containerimage mit dem Gebietsschema `ko-KR`. |
| `2.0.2-amd64-mr-in-preview` | Containerimage mit dem Gebietsschema `mr-IN`. |
| `2.0.2-amd64-nb-no-preview` | Containerimage mit dem Gebietsschema `nb-NO`. |
| `2.0.2-amd64-nl-nl-preview` | Containerimage mit dem Gebietsschema `nl-NL`. |
| `2.0.2-amd64-pl-pl-preview` | Containerimage mit dem Gebietsschema `pl-PL`. |
| `2.0.2-amd64-pt-br-preview` | Containerimage mit dem Gebietsschema `pt-BR`. |
| `2.0.2-amd64-pt-pt-preview` | Containerimage mit dem Gebietsschema `pt-PT`. |
| `2.0.2-amd64-ru-ru-preview` | Containerimage mit dem Gebietsschema `ru-RU`. |
| `2.0.2-amd64-sv-se-preview` | Containerimage mit dem Gebietsschema `sv-SE`. |
| `2.0.2-amd64-ta-in-preview` | Containerimage mit dem Gebietsschema `ta-IN`. |
| `2.0.2-amd64-te-in-preview` | Containerimage mit dem Gebietsschema `te-IN`. |
| `2.0.2-amd64-th-th-preview` | Containerimage mit dem Gebietsschema `th-TH`. |
| `2.0.2-amd64-tr-tr-preview` | Containerimage mit dem Gebietsschema `tr-TR`. |
| `2.0.2-amd64-zh-cn-preview` | Containerimage mit dem Gebietsschema `zh-CN`. |
| `2.0.2-amd64-zh-hk-preview` | Containerimage mit dem Gebietsschema `zh-HK`. |
| `2.0.2-amd64-zh-tw-preview` | Containerimage mit dem Gebietsschema `zh-TW`. |
| `2.0.1-amd64-ar-ae-preview` | Containerimage mit dem Gebietsschema `ar-AE`. |
| `2.0.1-amd64-ar-eg-preview` | Containerimage mit dem Gebietsschema `ar-EG`. |
| `2.0.1-amd64-ar-kw-preview` | Containerimage mit dem Gebietsschema `ar-KW`. |
| `2.0.1-amd64-ar-qa-preview` | Containerimage mit dem Gebietsschema `ar-QA`. |
| `2.0.1-amd64-ar-sa-preview` | Containerimage mit dem Gebietsschema `ar-SA`. |
| `2.0.1-amd64-ca-es-preview` | Containerimage mit dem Gebietsschema `ca-ES`. |
| `2.0.1-amd64-da-dk-preview` | Containerimage mit dem Gebietsschema `da-DK`. |
| `2.0.1-amd64-de-de-preview` | Containerimage mit dem Gebietsschema `de-DE`. |
| `2.0.1-amd64-en-au-preview` | Containerimage mit dem Gebietsschema `en-AU`. |
| `2.0.1-amd64-en-ca-preview` | Containerimage mit dem Gebietsschema `en-CA`. |
| `2.0.1-amd64-en-gb-preview` | Containerimage mit dem Gebietsschema `en-GB`. |
| `2.0.1-amd64-en-in-preview` | Containerimage mit dem Gebietsschema `en-IN`. |
| `2.0.1-amd64-en-nz-preview` | Containerimage mit dem Gebietsschema `en-NZ`. |
| `2.0.1-amd64-en-us-preview` | Containerimage mit dem Gebietsschema `en-US`. |
| `2.0.1-amd64-es-es-preview` | Containerimage mit dem Gebietsschema `es-ES`. |
| `2.0.1-amd64-es-mx-preview` | Containerimage mit dem Gebietsschema `es-MX`. |
| `2.0.1-amd64-fi-fi-preview` | Containerimage mit dem Gebietsschema `fi-FI`. |
| `2.0.1-amd64-fr-ca-preview` | Containerimage mit dem Gebietsschema `fr-CA`. |
| `2.0.1-amd64-fr-fr-preview` | Containerimage mit dem Gebietsschema `fr-FR`. |
| `2.0.1-amd64-gu-in-preview` | Containerimage mit dem Gebietsschema `gu-IN`. |
| `2.0.1-amd64-hi-in-preview` | Containerimage mit dem Gebietsschema `hi-IN`. |
| `2.0.1-amd64-it-it-preview` | Containerimage mit dem Gebietsschema `it-IT`. |
| `2.0.1-amd64-ja-jp-preview` | Containerimage mit dem Gebietsschema `ja-JP`. |
| `2.0.1-amd64-ko-kr-preview` | Containerimage mit dem Gebietsschema `ko-KR`. |
| `2.0.1-amd64-mr-in-preview` | Containerimage mit dem Gebietsschema `mr-IN`. |
| `2.0.1-amd64-nb-no-preview` | Containerimage mit dem Gebietsschema `nb-NO`. |
| `2.0.1-amd64-nl-nl-preview` | Containerimage mit dem Gebietsschema `nl-NL`. |
| `2.0.1-amd64-pl-pl-preview` | Containerimage mit dem Gebietsschema `pl-PL`. |
| `2.0.1-amd64-pt-br-preview` | Containerimage mit dem Gebietsschema `pt-BR`. |
| `2.0.1-amd64-pt-pt-preview` | Containerimage mit dem Gebietsschema `pt-PT`. |
| `2.0.1-amd64-ru-ru-preview` | Containerimage mit dem Gebietsschema `ru-RU`. |
| `2.0.1-amd64-sv-se-preview` | Containerimage mit dem Gebietsschema `sv-SE`. |
| `2.0.1-amd64-ta-in-preview` | Containerimage mit dem Gebietsschema `ta-IN`. |
| `2.0.1-amd64-te-in-preview` | Containerimage mit dem Gebietsschema `te-IN`. |
| `2.0.1-amd64-th-th-preview` | Containerimage mit dem Gebietsschema `th-TH`. |
| `2.0.1-amd64-tr-tr-preview` | Containerimage mit dem Gebietsschema `tr-TR`. |
| `2.0.1-amd64-zh-cn-preview` | Containerimage mit dem Gebietsschema `zh-CN`. |
| `2.0.1-amd64-zh-hk-preview` | Containerimage mit dem Gebietsschema `zh-HK`. |
| `2.0.1-amd64-zh-tw-preview` | Containerimage mit dem Gebietsschema `zh-TW`. |
| `2.0.0-amd64-ar-eg-preview` | Containerimage mit dem Gebietsschema `ar-EG`. |
| `2.0.0-amd64-ca-es-preview` | Containerimage mit dem Gebietsschema `ca-ES`. |
| `2.0.0-amd64-da-dk-preview` | Containerimage mit dem Gebietsschema `da-DK`. |
| `2.0.0-amd64-de-de-preview` | Containerimage mit dem Gebietsschema `de-DE`. |
| `2.0.0-amd64-en-au-preview` | Containerimage mit dem Gebietsschema `en-AU`. |
| `2.0.0-amd64-en-ca-preview` | Containerimage mit dem Gebietsschema `en-CA`. |
| `2.0.0-amd64-en-gb-preview` | Containerimage mit dem Gebietsschema `en-GB`. |
| `2.0.0-amd64-en-in-preview` | Containerimage mit dem Gebietsschema `en-IN`. |
| `2.0.0-amd64-en-nz-preview` | Containerimage mit dem Gebietsschema `en-NZ`. |
| `2.0.0-amd64-en-us-preview` | Containerimage mit dem Gebietsschema `en-US`. |
| `2.0.0-amd64-es-es-preview` | Containerimage mit dem Gebietsschema `es-ES`. |
| `2.0.0-amd64-es-mx-preview` | Containerimage mit dem Gebietsschema `es-MX`. |
| `2.0.0-amd64-fi-fi-preview` | Containerimage mit dem Gebietsschema `fi-FI`. |
| `2.0.0-amd64-fr-ca-preview` | Containerimage mit dem Gebietsschema `fr-CA`. |
| `2.0.0-amd64-fr-fr-preview` | Containerimage mit dem Gebietsschema `fr-FR`. |
| `2.0.0-amd64-hi-in-preview` | Containerimage mit dem Gebietsschema `hi-IN`. |
| `2.0.0-amd64-it-it-preview` | Containerimage mit dem Gebietsschema `it-IT`. |
| `2.0.0-amd64-ja-jp-preview` | Containerimage mit dem Gebietsschema `ja-JP`. |
| `2.0.0-amd64-ko-kr-preview` | Containerimage mit dem Gebietsschema `ko-KR`. |
| `2.0.0-amd64-nb-no-preview` | Containerimage mit dem Gebietsschema `nb-NO`. |
| `2.0.0-amd64-nl-nl-preview` | Containerimage mit dem Gebietsschema `nl-NL`. |
| `2.0.0-amd64-pl-pl-preview` | Containerimage mit dem Gebietsschema `pl-PL`. |
| `2.0.0-amd64-pt-br-preview` | Containerimage mit dem Gebietsschema `pt-BR`. |
| `2.0.0-amd64-pt-pt-preview` | Containerimage mit dem Gebietsschema `pt-PT`. |
| `2.0.0-amd64-ru-ru-preview` | Containerimage mit dem Gebietsschema `ru-RU`. |
| `2.0.0-amd64-sv-se-preview` | Containerimage mit dem Gebietsschema `sv-SE`. |
| `2.0.0-amd64-th-th-preview` | Containerimage mit dem Gebietsschema `th-TH`. |
| `2.0.0-amd64-tr-tr-preview` | Containerimage mit dem Gebietsschema `tr-TR`. |
| `2.0.0-amd64-zh-cn-preview` | Containerimage mit dem Gebietsschema `zh-CN`. |
| `2.0.0-amd64-zh-hk-preview` | Containerimage mit dem Gebietsschema `zh-HK`. |
| `2.0.0-amd64-zh-tw-preview` | Containerimage mit dem Gebietsschema `zh-TW`. |
| `1.2.0-amd64-de-de-preview` | Containerimage mit dem Gebietsschema `de-DE`. |
| `1.2.0-amd64-en-au-preview` | Containerimage mit dem Gebietsschema `en-AU`. |
| `1.2.0-amd64-en-ca-preview` | Containerimage mit dem Gebietsschema `en-CA`. |
| `1.2.0-amd64-en-gb-preview` | Containerimage mit dem Gebietsschema `en-GB`. |
| `1.2.0-amd64-en-in-preview` | Containerimage mit dem Gebietsschema `en-IN`. |
| `1.2.0-amd64-en-us-preview` | Containerimage mit dem Gebietsschema `en-US`. |
| `1.2.0-amd64-es-es-preview` | Containerimage mit dem Gebietsschema `es-ES`. |
| `1.2.0-amd64-es-mx-preview` | Containerimage mit dem Gebietsschema `es-MX`. |
| `1.2.0-amd64-fr-ca-preview` | Containerimage mit dem Gebietsschema `fr-CA`. |
| `1.2.0-amd64-fr-fr-preview` | Containerimage mit dem Gebietsschema `fr-FR`. |
| `1.2.0-amd64-it-it-preview` | Containerimage mit dem Gebietsschema `it-IT`. |
| `1.2.0-amd64-ja-jp-preview` | Containerimage mit dem Gebietsschema `ja-JP`. |
| `1.2.0-amd64-pt-br-preview` | Containerimage mit dem Gebietsschema `pt-BR`. |
| `1.2.0-amd64-zh-cn-preview` | Containerimage mit dem Gebietsschema `zh-CN`. |
| `1.1.3-amd64-de-de-preview` | Containerimage mit dem Gebietsschema `de-DE`. |
| `1.1.3-amd64-en-au-preview` | Containerimage mit dem Gebietsschema `en-AU`. |
| `1.1.3-amd64-en-ca-preview` | Containerimage mit dem Gebietsschema `en-CA`. |
| `1.1.3-amd64-en-gb-preview` | Containerimage mit dem Gebietsschema `en-GB`. |
| `1.1.3-amd64-en-in-preview` | Containerimage mit dem Gebietsschema `en-IN`. |
| `1.1.3-amd64-en-us-preview` | Containerimage mit dem Gebietsschema `en-US`. |
| `1.1.3-amd64-es-es-preview` | Containerimage mit dem Gebietsschema `es-ES`. |
| `1.1.3-amd64-es-mx-preview` | Containerimage mit dem Gebietsschema `es-MX`. |
| `1.1.3-amd64-fr-ca-preview` | Containerimage mit dem Gebietsschema `fr-CA`. |
| `1.1.3-amd64-fr-fr-preview` | Containerimage mit dem Gebietsschema `fr-FR`. |
| `1.1.3-amd64-it-it-preview` | Containerimage mit dem Gebietsschema `it-IT`. |
| `1.1.3-amd64-ja-jp-preview` | Containerimage mit dem Gebietsschema `ja-JP`. |
| `1.1.3-amd64-pt-br-preview` | Containerimage mit dem Gebietsschema `pt-BR`. |
| `1.1.3-amd64-zh-cn-preview` | Containerimage mit dem Gebietsschema `zh-CN`. |
| `1.1.2-amd64-de-de-preview` | Containerimage mit dem Gebietsschema `de-DE`. |
| `1.1.2-amd64-en-au-preview` | Containerimage mit dem Gebietsschema `en-AU`. |
| `1.1.2-amd64-en-ca-preview` | Containerimage mit dem Gebietsschema `en-CA`. |
| `1.1.2-amd64-en-gb-preview` | Containerimage mit dem Gebietsschema `en-GB`. |
| `1.1.2-amd64-en-in-preview` | Containerimage mit dem Gebietsschema `en-IN`. |
| `1.1.2-amd64-en-us-preview` | Containerimage mit dem Gebietsschema `en-US`. |
| `1.1.2-amd64-es-es-preview` | Containerimage mit dem Gebietsschema `es-ES`. |
| `1.1.2-amd64-es-mx-preview` | Containerimage mit dem Gebietsschema `es-MX`. |
| `1.1.2-amd64-fr-ca-preview` | Containerimage mit dem Gebietsschema `fr-CA`. |
| `1.1.2-amd64-fr-fr-preview` | Containerimage mit dem Gebietsschema `fr-FR`. |
| `1.1.2-amd64-it-it-preview` | Containerimage mit dem Gebietsschema `it-IT`. |
| `1.1.2-amd64-ja-jp-preview` | Containerimage mit dem Gebietsschema `ja-JP`. |
| `1.1.2-amd64-pt-br-preview` | Containerimage mit dem Gebietsschema `pt-BR`. |
| `1.1.2-amd64-zh-cn-preview` | Containerimage mit dem Gebietsschema `zh-CN`. |
| `1.1.1-amd64-de-de-preview` | Containerimage mit dem Gebietsschema `de-DE`. |
| `1.1.1-amd64-en-au-preview` | Containerimage mit dem Gebietsschema `en-AU`. |
| `1.1.1-amd64-en-ca-preview` | Containerimage mit dem Gebietsschema `en-CA`. |
| `1.1.1-amd64-en-gb-preview` | Containerimage mit dem Gebietsschema `en-GB`. |
| `1.1.1-amd64-en-in-preview` | Containerimage mit dem Gebietsschema `en-IN`. |
| `1.1.1-amd64-en-us-preview` | Containerimage mit dem Gebietsschema `en-US`. |
| `1.1.1-amd64-es-es-preview` | Containerimage mit dem Gebietsschema `es-ES`. |
| `1.1.1-amd64-es-mx-preview` | Containerimage mit dem Gebietsschema `es-MX`. |
| `1.1.1-amd64-fr-ca-preview` | Containerimage mit dem Gebietsschema `fr-CA`. |
| `1.1.1-amd64-fr-fr-preview` | Containerimage mit dem Gebietsschema `fr-FR`. |
| `1.1.1-amd64-it-it-preview` | Containerimage mit dem Gebietsschema `it-IT`. |
| `1.1.1-amd64-ja-jp-preview` | Containerimage mit dem Gebietsschema `ja-JP`. |
| `1.1.1-amd64-pt-br-preview` | Containerimage mit dem Gebietsschema `pt-BR`. |
| `1.1.1-amd64-zh-cn-preview` | Containerimage mit dem Gebietsschema `zh-CN`. |
| `1.1.0-amd64-de-de-preview` | Containerimage mit dem Gebietsschema `de-DE`. |
| `1.1.0-amd64-en-au-preview` | Containerimage mit dem Gebietsschema `en-AU`. |
| `1.1.0-amd64-en-ca-preview` | Containerimage mit dem Gebietsschema `en-CA`. |
| `1.1.0-amd64-en-gb-preview` | Containerimage mit dem Gebietsschema `en-GB`. |
| `1.1.0-amd64-en-in-preview` | Containerimage mit dem Gebietsschema `en-IN`. |
| `1.1.0-amd64-en-us-preview` | Containerimage mit dem Gebietsschema `en-US`. |
| `1.1.0-amd64-es-es-preview` | Containerimage mit dem Gebietsschema `es-ES`. |
| `1.1.0-amd64-es-mx-preview` | Containerimage mit dem Gebietsschema `es-MX`. |
| `1.1.0-amd64-fr-ca-preview` | Containerimage mit dem Gebietsschema `fr-CA`. |
| `1.1.0-amd64-fr-fr-preview` | Containerimage mit dem Gebietsschema `fr-FR`. |
| `1.1.0-amd64-it-it-preview` | Containerimage mit dem Gebietsschema `it-IT`. |
| `1.1.0-amd64-ja-jp-preview` | Containerimage mit dem Gebietsschema `ja-JP`. |
| `1.1.0-amd64-pt-br-preview` | Containerimage mit dem Gebietsschema `pt-BR`. |
| `1.1.0-amd64-zh-cn-preview` | Containerimage mit dem Gebietsschema `zh-CN`. |
| `1.0.0-amd64-de-de-preview` | Containerimage mit dem Gebietsschema `de-DE`. |
| `1.0.0-amd64-en-au-preview` | Containerimage mit dem Gebietsschema `en-AU`. |
| `1.0.0-amd64-en-ca-preview` | Containerimage mit dem Gebietsschema `en-CA`. |
| `1.0.0-amd64-en-gb-preview` | Containerimage mit dem Gebietsschema `en-GB`. |
| `1.0.0-amd64-en-in-preview` | Containerimage mit dem Gebietsschema `en-IN`. |
| `1.0.0-amd64-en-us-preview` | Containerimage mit dem Gebietsschema `en-US`. |
| `1.0.0-amd64-es-es-preview` | Containerimage mit dem Gebietsschema `es-ES`. |
| `1.0.0-amd64-es-mx-preview` | Containerimage mit dem Gebietsschema `es-MX`. |
| `1.0.0-amd64-fr-ca-preview` | Containerimage mit dem Gebietsschema `fr-CA`. |
| `1.0.0-amd64-fr-fr-preview` | Containerimage mit dem Gebietsschema `fr-FR`. |
| `1.0.0-amd64-it-it-preview` | Containerimage mit dem Gebietsschema `it-IT`. |
| `1.0.0-amd64-ja-jp-preview` | Containerimage mit dem Gebietsschema `ja-JP`. |
| `1.0.0-amd64-pt-br-preview` | Containerimage mit dem Gebietsschema `pt-BR`. |
| `1.0.0-amd64-zh-cn-preview` | Containerimage mit dem Gebietsschema `zh-CN`. |

## <a name="text-to-speech"></a>Text-zu-Sprache

Das Containerimage [Sprachsynthese][sp-tts] befindet sich in der `containerpreview.azurecr.io`-Containerregistrierung. Es befindet sich im Repository `microsoft` und trägt den Namen `cognitive-services-text-to-speech`. Der vollqualifizierte Containerimagename lautet `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech`.

Dieses Containerimage verfügt über die folgenden Tags:

| Imagetags                                  | Notizen                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaRUS`.         |
| `1.7.0-amd64-ar-eg-hoda`                    | Containerimage mit dem Gebietsschema `ar-EG` und der Stimme `ar-EG-Hoda`.            |
| `1.7.0-amd64-ar-sa-naayf`                   | Containerimage mit dem Gebietsschema `ar-SA` und der Stimme `ar-SA-Naayf`.           |
| `1.7.0-amd64-bg-bg-ivan`                    | Containerimage mit dem Gebietsschema `bg-BG` und der Stimme `bg-BG-Ivan`.            |
| `1.7.0-amd64-ca-es-herenarus`               | Containerimage mit dem Gebietsschema `ca-ES` und der Stimme `ca-ES-HerenaRUS`.       |
| `1.7.0-amd64-cs-cz-jakub`                   | Containerimage mit dem Gebietsschema `cs-CZ` und der Stimme `cs-CZ-Jakub`.           |
| `1.7.0-amd64-da-dk-hellerus`                | Containerimage mit dem Gebietsschema `da-DK` und der Stimme `da-DK-HelleRUS`.        |
| `1.7.0-amd64-de-at-michael`                 | Containerimage mit dem Gebietsschema `de-AT` und der Stimme `de-AT-Michael`.         |
| `1.7.0-amd64-de-ch-karsten`                 | Containerimage mit dem Gebietsschema `de-CH` und der Stimme `de-CH-Karsten`.         |
| `1.7.0-amd64-de-de-hedda`                   | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           |
| `1.7.0-amd64-de-de-heddarus`                | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           |
| `1.7.0-amd64-de-de-stefan-apollo`           | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Stefan-Apollo`.   |
| `1.7.0-amd64-el-gr-stefanos`                | Containerimage mit dem Gebietsschema `el-GR` und der Stimme `el-GR-Stefanos`.        |
| `1.7.0-amd64-en-au-catherine`               | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-Catherine`.       |
| `1.7.0-amd64-en-au-hayleyrus`               | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-HayleyRUS`.       |
| `1.7.0-amd64-en-ca-heatherrus`              | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-HeatherRUS`.      |
| `1.7.0-amd64-en-ca-linda`                   | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-Linda`.           |
| `1.7.0-amd64-en-gb-george-apollo`           | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-George-Apollo`.   |
| `1.7.0-amd64-en-gb-hazelrus`                | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-HazelRUS`.        |
| `1.7.0-amd64-en-gb-susan-apollo`            | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-Susan-Apollo`.    |
| `1.7.0-amd64-en-ie-sean`                    | Containerimage mit dem Gebietsschema `en-IE` und der Stimme `en-IE-Sean`.            |
| `1.7.0-amd64-en-in-heera-apollo`            | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Heera-Apollo`.    |
| `1.7.0-amd64-en-in-priyarus`                | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-PriyaRUS`.        |
| `1.7.0-amd64-en-in-ravi-apollo`             | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Ravi-Apollo`.     |
| `1.7.0-amd64-en-us-benjaminrus`             | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-BenjaminRUS`.     |
| `1.7.0-amd64-en-us-guy24krus`               | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Guy24kRUS`.       |
| `1.7.0-amd64-en-us-aria24krus`              | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Aria24kRUS`.      |
| `1.7.0-amd64-en-us-ariarus`                 | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaRUS`.         |
| `1.7.0-amd64-en-us-zirarus`                 | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-ZiraRUS`.         |
| `1.7.0-amd64-es-es-helenarus`               | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-HelenaRUS`.       |
| `1.7.0-amd64-es-es-laura-apollo`            | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Laura-Apollo`.    |
| `1.7.0-amd64-es-es-pablo-apollo`            | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Pablo-Apollo`.    |
| `1.7.0-amd64-es-mx-hildarus`                | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-HildaRUS`.        |
| `1.7.0-amd64-es-mx-raul-apollo`             | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-Raul-Apollo`.     |
| `1.7.0-amd64-fi-fi-heidirus`                | Containerimage mit dem Gebietsschema `fi-FI` und der Stimme `fi-FI-HeidiRUS`.        |
| `1.7.0-amd64-fr-ca-caroline`                | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-Caroline`.        |
| `1.7.0-amd64-fr-ca-harmonierus`             | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-HarmonieRUS`.     |
| `1.7.0-amd64-fr-ch-guillaume`               | Containerimage mit dem Gebietsschema `fr-CH` und der Stimme `fr-CH-Guillaume`.       |
| `1.7.0-amd64-fr-fr-hortenserus`             | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-HortenseRUS`.     |
| `1.7.0-amd64-fr-fr-julie-apollo`            | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Julie-Apollo`.    |
| `1.7.0-amd64-fr-fr-paul-apollo`             | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Paul-Apollo`.     |
| `1.7.0-amd64-he-il-asaf`                    | Containerimage mit dem Gebietsschema `he-IL` und der Stimme `he-IL-Asaf`.            |
| `1.7.0-amd64-hi-in-hemant`                  | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Hemant`.          |
| `1.7.0-amd64-hi-in-kalpana-apollo`          | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana-Apollo`.  |
| `1.7.0-amd64-hi-in-kalpana`                 | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana`.         |
| `1.7.0-amd64-hr-hr-matej`                   | Containerimage mit dem Gebietsschema `hr-HR` und der Stimme `hr-HR-Matej`.           |
| `1.7.0-amd64-hu-hu-szabolcs`                | Containerimage mit dem Gebietsschema `hu-HU` und der Stimme `hu-HU-Szabolcs`.        |
| `1.7.0-amd64-id-id-andika`                  | Containerimage mit dem Gebietsschema `id-ID` und der Stimme `id-ID-Andika`.          |
| `1.7.0-amd64-it-it-cosimo-apollo`           | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-Cosimo-Apollo`.   |
| `1.7.0-amd64-it-it-luciarus`                | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-LuciaRUS`.        |
| `1.7.0-amd64-ja-jp-ayumi-apollo`            | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ayumi-Apollo`.    |
| `1.7.0-amd64-ja-jp-harukarus`               | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-HarukaRUS`.       |
| `1.7.0-amd64-ja-jp-ichiro-apollo`           | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ichiro-Apollo`.   |
| `1.7.0-amd64-ko-kr-heamirus`                | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-HeamiRUS`.        |
| `1.7.0-amd64-ms-my-rizwan`                  | Containerimage mit dem Gebietsschema `ms-MY` und der Stimme `ms-MY-Rizwan`.          |
| `1.7.0-amd64-nb-no-huldarus`                | Containerimage mit dem Gebietsschema `nb-NO` und der Stimme `nb-NO-HuldaRUS`.        |
| `1.7.0-amd64-nl-nl-hannarus`                | Containerimage mit dem Gebietsschema `nl-NL` und der Stimme `nl-NL-HannaRUS`.        |
| `1.7.0-amd64-pl-pl-paulinarus`              | Containerimage mit dem Gebietsschema `pl-PL` und der Stimme `pl-PL-PaulinaRUS`.      |
| `1.7.0-amd64-pt-br-daniel-apollo`           | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-Daniel-Apollo`.   |
| `1.7.0-amd64-pt-br-heloisarus`              | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-HeloisaRUS`.      |
| `1.7.0-amd64-pt-pt-heliarus`                | Containerimage mit dem Gebietsschema `pt-PT` und der Stimme `pt-PT-HeliaRUS`.        |
| `1.7.0-amd64-ro-ro-andrei`                  | Containerimage mit dem Gebietsschema `ro-RO` und der Stimme `ro-RO-Andrei`.          |
| `1.7.0-amd64-ru-ru-ekaterinarus`            | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-EkaterinaRUS`.    |
| `1.7.0-amd64-ru-ru-irina-apollo`            | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Irina-Apollo`.    |
| `1.7.0-amd64-ru-ru-pavel-apollo`            | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Pavel-Apollo`.    |
| `1.7.0-amd64-sk-sk-filip`                   | Containerimage mit dem Gebietsschema `sk-SK` und der Stimme `sk-SK-Filip`.           |
| `1.7.0-amd64-sl-si-lado`                    | Containerimage mit dem Gebietsschema `sl-SI` und der Stimme `sl-SI-Lado`.            |
| `1.7.0-amd64-sv-se-hedvigrus`               | Containerimage mit dem Gebietsschema `sv-SE` und der Stimme `sv-SE-HedvigRUS`.       |
| `1.7.0-amd64-ta-in-valluvar`                | Containerimage mit dem Gebietsschema `ta-IN` und der Stimme `ta-IN-Valluvar`.        |
| `1.7.0-amd64-te-in-chitra`                  | Containerimage mit dem Gebietsschema `te-IN` und der Stimme `te-IN-Chitra`.          |
| `1.7.0-amd64-th-th-pattara`                 | Containerimage mit dem Gebietsschema `th-TH` und der Stimme `th-TH-Pattara`.         |
| `1.7.0-amd64-tr-tr-sedarus`                 | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-SedaRUS`.         |
| `1.7.0-amd64-vi-vn-an`                      | Containerimage mit dem Gebietsschema `vi-VN` und der Stimme `vi-VN-An`.              |
| `1.7.0-amd64-zh-cn-huihuirus`               | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-HuihuiRUS`.       |
| `1.7.0-amd64-zh-cn-kangkang-apollo`         | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Kangkang-Apollo`. |
| `1.7.0-amd64-zh-cn-yaoyao-apollo`           | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Yaoyao-Apollo`.   |
| `1.7.0-amd64-zh-hk-danny-apollo`            | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Danny-Apollo`.    |
| `1.7.0-amd64-zh-hk-tracy-apollo`            | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Tracy-Apollo`.    |
| `1.7.0-amd64-zh-hk-tracyrus`                | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-TracyRUS`.        |
| `1.7.0-amd64-zh-tw-hanhanrus`               | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-HanHanRUS`.       |
| `1.7.0-amd64-zh-tw-yating-apollo`           | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Yating-Apollo`.   |
| `1.7.0-amd64-zh-tw-zhiwei-apollo`           | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Zhiwei-Apollo`.   |
| `1.6.0-amd64-ar-eg-hoda-preview`            | Containerimage mit dem Gebietsschema `ar-EG` und der Stimme `ar-EG-Hoda`.            |
| `1.6.0-amd64-ar-sa-naayf-preview`           | Containerimage mit dem Gebietsschema `ar-SA` und der Stimme `ar-SA-Naayf`.           |
| `1.6.0-amd64-bg-bg-ivan-preview`            | Containerimage mit dem Gebietsschema `bg-BG` und der Stimme `bg-BG-Ivan`.            |
| `1.6.0-amd64-ca-es-herenarus-preview`       | Containerimage mit dem Gebietsschema `ca-ES` und der Stimme `ca-ES-HerenaRUS`.       |
| `1.6.0-amd64-cs-cz-jakub-preview`           | Containerimage mit dem Gebietsschema `cs-CZ` und der Stimme `cs-CZ-Jakub`.           |
| `1.6.0-amd64-da-dk-hellerus-preview`        | Containerimage mit dem Gebietsschema `da-DK` und der Stimme `da-DK-HelleRUS`.        |
| `1.6.0-amd64-de-at-michael-preview`         | Containerimage mit dem Gebietsschema `de-AT` und der Stimme `de-AT-Michael`.         |
| `1.6.0-amd64-de-ch-karsten-preview`         | Containerimage mit dem Gebietsschema `de-CH` und der Stimme `de-CH-Karsten`.         |
| `1.6.0-amd64-de-de-hedda-preview`           | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           |
| `1.6.0-amd64-de-de-heddarus-preview`        | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           |
| `1.6.0-amd64-de-de-stefan-apollo-preview`   | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Stefan-Apollo`.   |
| `1.6.0-amd64-el-gr-stefanos-preview`        | Containerimage mit dem Gebietsschema `el-GR` und der Stimme `el-GR-Stefanos`.        |
| `1.6.0-amd64-en-au-catherine-preview`       | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-Catherine`.       |
| `1.6.0-amd64-en-au-hayleyrus-preview`       | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-HayleyRUS`.       |
| `1.6.0-amd64-en-ca-heatherrus-preview`      | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-HeatherRUS`.      |
| `1.6.0-amd64-en-ca-linda-preview`           | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-Linda`.           |
| `1.6.0-amd64-en-gb-george-apollo-preview`   | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-George-Apollo`.   |
| `1.6.0-amd64-en-gb-hazelrus-preview`        | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-HazelRUS`.        |
| `1.6.0-amd64-en-gb-susan-apollo-preview`    | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-Susan-Apollo`.    |
| `1.6.0-amd64-en-ie-sean-preview`            | Containerimage mit dem Gebietsschema `en-IE` und der Stimme `en-IE-Sean`.            |
| `1.6.0-amd64-en-in-heera-apollo-preview`    | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Heera-Apollo`.    |
| `1.6.0-amd64-en-in-priyarus-preview`        | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-PriyaRUS`.        |
| `1.6.0-amd64-en-in-ravi-apollo-preview`     | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Ravi-Apollo`.     |
| `1.6.0-amd64-en-us-benjaminrus-preview`     | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-BenjaminRUS`.     |
| `1.6.0-amd64-en-us-guy24krus-preview`       | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Guy24kRUS`.       |
| `1.6.0-amd64-en-us-aria24krus-preview`      | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Aria24kRUS`.      |
| `1.6.0-amd64-en-us-ariarus-preview`         | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaRUS`.         |
| `1.6.0-amd64-en-us-zirarus-preview`         | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-ZiraRUS`.         |
| `1.6.0-amd64-es-es-helenarus-preview`       | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-HelenaRUS`.       |
| `1.6.0-amd64-es-es-laura-apollo-preview`    | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Laura-Apollo`.    |
| `1.6.0-amd64-es-es-pablo-apollo-preview`    | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Pablo-Apollo`.    |
| `1.6.0-amd64-es-mx-hildarus-preview`        | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-HildaRUS`.        |
| `1.6.0-amd64-es-mx-raul-apollo-preview`     | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-Raul-Apollo`.     |
| `1.6.0-amd64-fi-fi-heidirus-preview`        | Containerimage mit dem Gebietsschema `fi-FI` und der Stimme `fi-FI-HeidiRUS`.        |
| `1.6.0-amd64-fr-ca-caroline-preview`        | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-Caroline`.        |
| `1.6.0-amd64-fr-ca-harmonierus-preview`     | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-HarmonieRUS`.     |
| `1.6.0-amd64-fr-ch-guillaume-preview`       | Containerimage mit dem Gebietsschema `fr-CH` und der Stimme `fr-CH-Guillaume`.       |
| `1.6.0-amd64-fr-fr-hortenserus-preview`     | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-HortenseRUS`.     |
| `1.6.0-amd64-fr-fr-julie-apollo-preview`    | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Julie-Apollo`.    |
| `1.6.0-amd64-fr-fr-paul-apollo-preview`     | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Paul-Apollo`.     |
| `1.6.0-amd64-he-il-asaf-preview`            | Containerimage mit dem Gebietsschema `he-IL` und der Stimme `he-IL-Asaf`.            |
| `1.6.0-amd64-hi-in-hemant-preview`          | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Hemant`.          |
| `1.6.0-amd64-hi-in-kalpana-apollo-preview`  | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana-Apollo`.  |
| `1.6.0-amd64-hi-in-kalpana-preview`         | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana`.         |
| `1.6.0-amd64-hr-hr-matej-preview`           | Containerimage mit dem Gebietsschema `hr-HR` und der Stimme `hr-HR-Matej`.           |
| `1.6.0-amd64-hu-hu-szabolcs-preview`        | Containerimage mit dem Gebietsschema `hu-HU` und der Stimme `hu-HU-Szabolcs`.        |
| `1.6.0-amd64-id-id-andika-preview`          | Containerimage mit dem Gebietsschema `id-ID` und der Stimme `id-ID-Andika`.          |
| `1.6.0-amd64-it-it-cosimo-apollo-preview`   | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-Cosimo-Apollo`.   |
| `1.6.0-amd64-it-it-luciarus-preview`        | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-LuciaRUS`.        |
| `1.6.0-amd64-ja-jp-ayumi-apollo-preview`    | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ayumi-Apollo`.    |
| `1.6.0-amd64-ja-jp-harukarus-preview`       | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-HarukaRUS`.       |
| `1.6.0-amd64-ja-jp-ichiro-apollo-preview`   | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ichiro-Apollo`.   |
| `1.6.0-amd64-ko-kr-heamirus-preview`        | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-HeamiRUS`.        |
| `1.6.0-amd64-ms-my-rizwan-preview`          | Containerimage mit dem Gebietsschema `ms-MY` und der Stimme `ms-MY-Rizwan`.          |
| `1.6.0-amd64-nb-no-huldarus-preview`        | Containerimage mit dem Gebietsschema `nb-NO` und der Stimme `nb-NO-HuldaRUS`.        |
| `1.6.0-amd64-nl-nl-hannarus-preview`        | Containerimage mit dem Gebietsschema `nl-NL` und der Stimme `nl-NL-HannaRUS`.        |
| `1.6.0-amd64-pl-pl-paulinarus-preview`      | Containerimage mit dem Gebietsschema `pl-PL` und der Stimme `pl-PL-PaulinaRUS`.      |
| `1.6.0-amd64-pt-br-daniel-apollo-preview`   | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-Daniel-Apollo`.   |
| `1.6.0-amd64-pt-br-heloisarus-preview`      | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-HeloisaRUS`.      |
| `1.6.0-amd64-pt-pt-heliarus-preview`        | Containerimage mit dem Gebietsschema `pt-PT` und der Stimme `pt-PT-HeliaRUS`.        |
| `1.6.0-amd64-ro-ro-andrei-preview`          | Containerimage mit dem Gebietsschema `ro-RO` und der Stimme `ro-RO-Andrei`.          |
| `1.6.0-amd64-ru-ru-ekaterinarus-preview`    | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-EkaterinaRUS`.    |
| `1.6.0-amd64-ru-ru-irina-apollo-preview`    | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Irina-Apollo`.    |
| `1.6.0-amd64-ru-ru-pavel-apollo-preview`    | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Pavel-Apollo`.    |
| `1.6.0-amd64-sk-sk-filip-preview`           | Containerimage mit dem Gebietsschema `sk-SK` und der Stimme `sk-SK-Filip`.           |
| `1.6.0-amd64-sl-si-lado-preview`            | Containerimage mit dem Gebietsschema `sl-SI` und der Stimme `sl-SI-Lado`.            |
| `1.6.0-amd64-sv-se-hedvigrus-preview`       | Containerimage mit dem Gebietsschema `sv-SE` und der Stimme `sv-SE-HedvigRUS`.       |
| `1.6.0-amd64-ta-in-valluvar-preview`        | Containerimage mit dem Gebietsschema `ta-IN` und der Stimme `ta-IN-Valluvar`.        |
| `1.6.0-amd64-te-in-chitra-preview`          | Containerimage mit dem Gebietsschema `te-IN` und der Stimme `te-IN-Chitra`.          |
| `1.6.0-amd64-th-th-pattara-preview`         | Containerimage mit dem Gebietsschema `th-TH` und der Stimme `th-TH-Pattara`.         |
| `1.6.0-amd64-tr-tr-sedarus-preview`         | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-SedaRUS`.         |
| `1.6.0-amd64-vi-vn-an-preview`              | Containerimage mit dem Gebietsschema `vi-VN` und der Stimme `vi-VN-An`.              |
| `1.6.0-amd64-zh-cn-huihuirus-preview`       | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-HuihuiRUS`.       |
| `1.6.0-amd64-zh-cn-kangkang-apollo-preview` | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Kangkang-Apollo`. |
| `1.6.0-amd64-zh-cn-yaoyao-apollo-preview`   | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Yaoyao-Apollo`.   |
| `1.6.0-amd64-zh-hk-danny-apollo-preview`    | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Danny-Apollo`.    |
| `1.6.0-amd64-zh-hk-tracy-apollo-preview`    | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Tracy-Apollo`.    |
| `1.6.0-amd64-zh-hk-tracyrus-preview`        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-TracyRUS`.        |
| `1.6.0-amd64-zh-tw-hanhanrus-preview`       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-HanHanRUS`.       |
| `1.6.0-amd64-zh-tw-yating-apollo-preview`   | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Yating-Apollo`.   |
| `1.6.0-amd64-zh-tw-zhiwei-apollo-preview`   | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Zhiwei-Apollo`.   |
| `1.5.0-amd64-ar-eg-hoda-preview`            | Containerimage mit dem Gebietsschema `ar-EG` und der Stimme `ar-EG-Hoda`.            |
| `1.5.0-amd64-ar-sa-naayf-preview`           | Containerimage mit dem Gebietsschema `ar-SA` und der Stimme `ar-SA-Naayf`.           |
| `1.5.0-amd64-bg-bg-ivan-preview`            | Containerimage mit dem Gebietsschema `bg-BG` und der Stimme `bg-BG-Ivan`.            |
| `1.5.0-amd64-ca-es-herenarus-preview`       | Containerimage mit dem Gebietsschema `ca-ES` und der Stimme `ca-ES-HerenaRUS`.       |
| `1.5.0-amd64-cs-cz-jakub-preview`           | Containerimage mit dem Gebietsschema `cs-CZ` und der Stimme `cs-CZ-Jakub`.           |
| `1.5.0-amd64-da-dk-hellerus-preview`        | Containerimage mit dem Gebietsschema `da-DK` und der Stimme `da-DK-HelleRUS`.        |
| `1.5.0-amd64-de-at-michael-preview`         | Containerimage mit dem Gebietsschema `de-AT` und der Stimme `de-AT-Michael`.         |
| `1.5.0-amd64-de-ch-karsten-preview`         | Containerimage mit dem Gebietsschema `de-CH` und der Stimme `de-CH-Karsten`.         |
| `1.5.0-amd64-de-de-hedda-preview`           | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           |
| `1.5.0-amd64-de-de-heddarus-preview`        | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           |
| `1.5.0-amd64-de-de-stefan-apollo-preview`   | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Stefan-Apollo`.   |
| `1.5.0-amd64-el-gr-stefanos-preview`        | Containerimage mit dem Gebietsschema `el-GR` und der Stimme `el-GR-Stefanos`.        |
| `1.5.0-amd64-en-au-catherine-preview`       | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-Catherine`.       |
| `1.5.0-amd64-en-au-hayleyrus-preview`       | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-HayleyRUS`.       |
| `1.5.0-amd64-en-ca-heatherrus-preview`      | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-HeatherRUS`.      |
| `1.5.0-amd64-en-ca-linda-preview`           | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-Linda`.           |
| `1.5.0-amd64-en-gb-george-apollo-preview`   | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-George-Apollo`.   |
| `1.5.0-amd64-en-gb-hazelrus-preview`        | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-HazelRUS`.        |
| `1.5.0-amd64-en-gb-susan-apollo-preview`    | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-Susan-Apollo`.    |
| `1.5.0-amd64-en-ie-sean-preview`            | Containerimage mit dem Gebietsschema `en-IE` und der Stimme `en-IE-Sean`.            |
| `1.5.0-amd64-en-in-heera-apollo-preview`    | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Heera-Apollo`.    |
| `1.5.0-amd64-en-in-priyarus-preview`        | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-PriyaRUS`.        |
| `1.5.0-amd64-en-in-ravi-apollo-preview`     | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Ravi-Apollo`.     |
| `1.5.0-amd64-en-us-benjaminrus-preview`     | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-BenjaminRUS`.     |
| `1.5.0-amd64-en-us-guy24krus-preview`       | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Guy24kRUS`.       |
| `1.5.0-amd64-en-us-aria24krus-preview`      | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Aria24kRUS`.     |
| `1.5.0-amd64-en-us-ariarus-preview`         | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaRUS`.        |
| `1.5.0-amd64-en-us-zirarus-preview`         | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-ZiraRUS`.         |
| `1.5.0-amd64-es-es-helenarus-preview`       | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-HelenaRUS`.       |
| `1.5.0-amd64-es-es-laura-apollo-preview`    | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Laura-Apollo`.    |
| `1.5.0-amd64-es-es-pablo-apollo-preview`    | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Pablo-Apollo`.    |
| `1.5.0-amd64-es-mx-hildarus-preview`        | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-HildaRUS`.        |
| `1.5.0-amd64-es-mx-raul-apollo-preview`     | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-Raul-Apollo`.     |
| `1.5.0-amd64-fi-fi-heidirus-preview`        | Containerimage mit dem Gebietsschema `fi-FI` und der Stimme `fi-FI-HeidiRUS`.        |
| `1.5.0-amd64-fr-ca-caroline-preview`        | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-Caroline`.        |
| `1.5.0-amd64-fr-ca-harmonierus-preview`     | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-HarmonieRUS`.     |
| `1.5.0-amd64-fr-ch-guillaume-preview`       | Containerimage mit dem Gebietsschema `fr-CH` und der Stimme `fr-CH-Guillaume`.       |
| `1.5.0-amd64-fr-fr-hortenserus-preview`     | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-HortenseRUS`.     |
| `1.5.0-amd64-fr-fr-julie-apollo-preview`    | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Julie-Apollo`.    |
| `1.5.0-amd64-fr-fr-paul-apollo-preview`     | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Paul-Apollo`.     |
| `1.5.0-amd64-he-il-asaf-preview`            | Containerimage mit dem Gebietsschema `he-IL` und der Stimme `he-IL-Asaf`.            |
| `1.5.0-amd64-hi-in-hemant-preview`          | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Hemant`.          |
| `1.5.0-amd64-hi-in-kalpana-apollo-preview`  | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana-Apollo`.  |
| `1.5.0-amd64-hi-in-kalpana-preview`         | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana`.         |
| `1.5.0-amd64-hr-hr-matej-preview`           | Containerimage mit dem Gebietsschema `hr-HR` und der Stimme `hr-HR-Matej`.           |
| `1.5.0-amd64-hu-hu-szabolcs-preview`        | Containerimage mit dem Gebietsschema `hu-HU` und der Stimme `hu-HU-Szabolcs`.        |
| `1.5.0-amd64-id-id-andika-preview`          | Containerimage mit dem Gebietsschema `id-ID` und der Stimme `id-ID-Andika`.          |
| `1.5.0-amd64-it-it-cosimo-apollo-preview`   | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-Cosimo-Apollo`.   |
| `1.5.0-amd64-it-it-luciarus-preview`        | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-LuciaRUS`.        |
| `1.5.0-amd64-ja-jp-ayumi-apollo-preview`    | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ayumi-Apollo`.    |
| `1.5.0-amd64-ja-jp-harukarus-preview`       | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-HarukaRUS`.       |
| `1.5.0-amd64-ja-jp-ichiro-apollo-preview`   | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ichiro-Apollo`.   |
| `1.5.0-amd64-ko-kr-heamirus-preview`        | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-HeamiRUS`.        |
| `1.5.0-amd64-ms-my-rizwan-preview`          | Containerimage mit dem Gebietsschema `ms-MY` und der Stimme `ms-MY-Rizwan`.          |
| `1.5.0-amd64-nb-no-huldarus-preview`        | Containerimage mit dem Gebietsschema `nb-NO` und der Stimme `nb-NO-HuldaRUS`.        |
| `1.5.0-amd64-nl-nl-hannarus-preview`        | Containerimage mit dem Gebietsschema `nl-NL` und der Stimme `nl-NL-HannaRUS`.        |
| `1.5.0-amd64-pl-pl-paulinarus-preview`      | Containerimage mit dem Gebietsschema `pl-PL` und der Stimme `pl-PL-PaulinaRUS`.      |
| `1.5.0-amd64-pt-br-daniel-apollo-preview`   | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-Daniel-Apollo`.   |
| `1.5.0-amd64-pt-br-heloisarus-preview`      | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-HeloisaRUS`.      |
| `1.5.0-amd64-pt-pt-heliarus-preview`        | Containerimage mit dem Gebietsschema `pt-PT` und der Stimme `pt-PT-HeliaRUS`.        |
| `1.5.0-amd64-ro-ro-andrei-preview`          | Containerimage mit dem Gebietsschema `ro-RO` und der Stimme `ro-RO-Andrei`.          |
| `1.5.0-amd64-ru-ru-ekaterinarus-preview`    | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-EkaterinaRUS`.    |
| `1.5.0-amd64-ru-ru-irina-apollo-preview`    | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Irina-Apollo`.    |
| `1.5.0-amd64-ru-ru-pavel-apollo-preview`    | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Pavel-Apollo`.    |
| `1.5.0-amd64-sk-sk-filip-preview`           | Containerimage mit dem Gebietsschema `sk-SK` und der Stimme `sk-SK-Filip`.           |
| `1.5.0-amd64-sl-si-lado-preview`            | Containerimage mit dem Gebietsschema `sl-SI` und der Stimme `sl-SI-Lado`.            |
| `1.5.0-amd64-sv-se-hedvigrus-preview`       | Containerimage mit dem Gebietsschema `sv-SE` und der Stimme `sv-SE-HedvigRUS`.       |
| `1.5.0-amd64-ta-in-valluvar-preview`        | Containerimage mit dem Gebietsschema `ta-IN` und der Stimme `ta-IN-Valluvar`.        |
| `1.5.0-amd64-te-in-chitra-preview`          | Containerimage mit dem Gebietsschema `te-IN` und der Stimme `te-IN-Chitra`.          |
| `1.5.0-amd64-th-th-pattara-preview`         | Containerimage mit dem Gebietsschema `th-TH` und der Stimme `th-TH-Pattara`.         |
| `1.5.0-amd64-tr-tr-sedarus-preview`         | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-SedaRUS`.         |
| `1.5.0-amd64-vi-vn-an-preview`              | Containerimage mit dem Gebietsschema `vi-VN` und der Stimme `vi-VN-An`.              |
| `1.5.0-amd64-zh-cn-huihuirus-preview`       | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-HuihuiRUS`.       |
| `1.5.0-amd64-zh-cn-kangkang-apollo-preview` | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Kangkang-Apollo`. |
| `1.5.0-amd64-zh-cn-yaoyao-apollo-preview`   | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Yaoyao-Apollo`.   |
| `1.5.0-amd64-zh-hk-danny-apollo-preview`    | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Danny-Apollo`.    |
| `1.5.0-amd64-zh-hk-tracy-apollo-preview`    | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Tracy-Apollo`.    |
| `1.5.0-amd64-zh-hk-tracyrus-preview`        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-TracyRUS`.        |
| `1.5.0-amd64-zh-tw-hanhanrus-preview`       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-HanHanRUS`.       |
| `1.5.0-amd64-zh-tw-yating-apollo-preview`   | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Yating-Apollo`.   |
| `1.5.0-amd64-zh-tw-zhiwei-apollo-preview`   | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Zhiwei-Apollo`.   |
| `1.4.0-amd64-ar-eg-hoda-preview`            | Containerimage mit dem Gebietsschema `ar-EG` und der Stimme `ar-EG-Hoda`.            |
| `1.4.0-amd64-ar-sa-naayf-preview`           | Containerimage mit dem Gebietsschema `ar-SA` und der Stimme `ar-SA-Naayf`.           |
| `1.4.0-amd64-bg-bg-ivan-preview`            | Containerimage mit dem Gebietsschema `bg-BG` und der Stimme `bg-BG-Ivan`.            |
| `1.4.0-amd64-ca-es-herenarus-preview`       | Containerimage mit dem Gebietsschema `ca-ES` und der Stimme `ca-ES-HerenaRUS`.       |
| `1.4.0-amd64-cs-cz-jakub-preview`           | Containerimage mit dem Gebietsschema `cs-CZ` und der Stimme `cs-CZ-Jakub`.           |
| `1.4.0-amd64-da-dk-hellerus-preview`        | Containerimage mit dem Gebietsschema `da-DK` und der Stimme `da-DK-HelleRUS`.        |
| `1.4.0-amd64-de-at-michael-preview`         | Containerimage mit dem Gebietsschema `de-AT` und der Stimme `de-AT-Michael`.         |
| `1.4.0-amd64-de-ch-karsten-preview`         | Containerimage mit dem Gebietsschema `de-CH` und der Stimme `de-CH-Karsten`.         |
| `1.4.0-amd64-de-de-hedda-preview`           | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           |
| `1.4.0-amd64-de-de-heddarus-preview`        | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           |
| `1.4.0-amd64-de-de-stefan-apollo-preview`   | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Stefan-Apollo`.   |
| `1.4.0-amd64-el-gr-stefanos-preview`        | Containerimage mit dem Gebietsschema `el-GR` und der Stimme `el-GR-Stefanos`.        |
| `1.4.0-amd64-en-au-catherine-preview`       | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-Catherine`.       |
| `1.4.0-amd64-en-au-hayleyrus-preview`       | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-HayleyRUS`.       |
| `1.4.0-amd64-en-ca-heatherrus-preview`      | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-HeatherRUS`.      |
| `1.4.0-amd64-en-ca-linda-preview`           | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-Linda`.           |
| `1.4.0-amd64-en-gb-george-apollo-preview`   | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-George-Apollo`.   |
| `1.4.0-amd64-en-gb-hazelrus-preview`        | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-HazelRUS`.        |
| `1.4.0-amd64-en-gb-susan-apollo-preview`    | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-Susan-Apollo`.    |
| `1.4.0-amd64-en-ie-sean-preview`            | Containerimage mit dem Gebietsschema `en-IE` und der Stimme `en-IE-Sean`.            |
| `1.4.0-amd64-en-in-heera-apollo-preview`    | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Heera-Apollo`.    |
| `1.4.0-amd64-en-in-priyarus-preview`        | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-PriyaRUS`.        |
| `1.4.0-amd64-en-in-ravi-apollo-preview`     | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Ravi-Apollo`.     |
| `1.4.0-amd64-en-us-benjaminrus-preview`     | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-BenjaminRUS`.     |
| `1.4.0-amd64-en-us-guy24krus-preview`       | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Guy24kRUS`.       |
| `1.4.0-amd64-en-us-aria24krus-preview`      | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Aria24kRUS`.     |
| `1.4.0-amd64-en-us-ariarus-preview`         | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaRUS`.        |
| `1.4.0-amd64-en-us-zirarus-preview`         | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-ZiraRUS`.         |
| `1.4.0-amd64-es-es-helenarus-preview`       | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-HelenaRUS`.       |
| `1.4.0-amd64-es-es-laura-apollo-preview`    | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Laura-Apollo`.    |
| `1.4.0-amd64-es-es-pablo-apollo-preview`    | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Pablo-Apollo`.    |
| `1.4.0-amd64-es-mx-hildarus-preview`        | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-HildaRUS`.        |
| `1.4.0-amd64-es-mx-raul-apollo-preview`     | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-Raul-Apollo`.     |
| `1.4.0-amd64-fi-fi-heidirus-preview`        | Containerimage mit dem Gebietsschema `fi-FI` und der Stimme `fi-FI-HeidiRUS`.        |
| `1.4.0-amd64-fr-ca-caroline-preview`        | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-Caroline`.        |
| `1.4.0-amd64-fr-ca-harmonierus-preview`     | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-HarmonieRUS`.     |
| `1.4.0-amd64-fr-ch-guillaume-preview`       | Containerimage mit dem Gebietsschema `fr-CH` und der Stimme `fr-CH-Guillaume`.       |
| `1.4.0-amd64-fr-fr-hortenserus-preview`     | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-HortenseRUS`.     |
| `1.4.0-amd64-fr-fr-julie-apollo-preview`    | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Julie-Apollo`.    |
| `1.4.0-amd64-fr-fr-paul-apollo-preview`     | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Paul-Apollo`.     |
| `1.4.0-amd64-he-il-asaf-preview`            | Containerimage mit dem Gebietsschema `he-IL` und der Stimme `he-IL-Asaf`.            |
| `1.4.0-amd64-hi-in-hemant-preview`          | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Hemant`.          |
| `1.4.0-amd64-hi-in-kalpana-apollo-preview`  | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana-Apollo`.  |
| `1.4.0-amd64-hi-in-kalpana-preview`         | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana`.         |
| `1.4.0-amd64-hr-hr-matej-preview`           | Containerimage mit dem Gebietsschema `hr-HR` und der Stimme `hr-HR-Matej`.           |
| `1.4.0-amd64-hu-hu-szabolcs-preview`        | Containerimage mit dem Gebietsschema `hu-HU` und der Stimme `hu-HU-Szabolcs`.        |
| `1.4.0-amd64-id-id-andika-preview`          | Containerimage mit dem Gebietsschema `id-ID` und der Stimme `id-ID-Andika`.          |
| `1.4.0-amd64-it-it-cosimo-apollo-preview`   | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-Cosimo-Apollo`.   |
| `1.4.0-amd64-it-it-luciarus-preview`        | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-LuciaRUS`.        |
| `1.4.0-amd64-ja-jp-ayumi-apollo-preview`    | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ayumi-Apollo`.    |
| `1.4.0-amd64-ja-jp-harukarus-preview`       | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-HarukaRUS`.       |
| `1.4.0-amd64-ja-jp-ichiro-apollo-preview`   | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ichiro-Apollo`.   |
| `1.4.0-amd64-ko-kr-heamirus-preview`        | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-HeamiRUS`.        |
| `1.4.0-amd64-ms-my-rizwan-preview`          | Containerimage mit dem Gebietsschema `ms-MY` und der Stimme `ms-MY-Rizwan`.          |
| `1.4.0-amd64-nb-no-huldarus-preview`        | Containerimage mit dem Gebietsschema `nb-NO` und der Stimme `nb-NO-HuldaRUS`.        |
| `1.4.0-amd64-nl-nl-hannarus-preview`        | Containerimage mit dem Gebietsschema `nl-NL` und der Stimme `nl-NL-HannaRUS`.        |
| `1.4.0-amd64-pl-pl-paulinarus-preview`      | Containerimage mit dem Gebietsschema `pl-PL` und der Stimme `pl-PL-PaulinaRUS`.      |
| `1.4.0-amd64-pt-br-daniel-apollo-preview`   | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-Daniel-Apollo`.   |
| `1.4.0-amd64-pt-br-heloisarus-preview`      | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-HeloisaRUS`.      |
| `1.4.0-amd64-pt-pt-heliarus-preview`        | Containerimage mit dem Gebietsschema `pt-PT` und der Stimme `pt-PT-HeliaRUS`.        |
| `1.4.0-amd64-ro-ro-andrei-preview`          | Containerimage mit dem Gebietsschema `ro-RO` und der Stimme `ro-RO-Andrei`.          |
| `1.4.0-amd64-ru-ru-ekaterinarus-preview`    | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-EkaterinaRUS`.    |
| `1.4.0-amd64-ru-ru-irina-apollo-preview`    | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Irina-Apollo`.    |
| `1.4.0-amd64-ru-ru-pavel-apollo-preview`    | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Pavel-Apollo`.    |
| `1.4.0-amd64-sk-sk-filip-preview`           | Containerimage mit dem Gebietsschema `sk-SK` und der Stimme `sk-SK-Filip`.           |
| `1.4.0-amd64-sl-si-lado-preview`            | Containerimage mit dem Gebietsschema `sl-SI` und der Stimme `sl-SI-Lado`.            |
| `1.4.0-amd64-sv-se-hedvigrus-preview`       | Containerimage mit dem Gebietsschema `sv-SE` und der Stimme `sv-SE-HedvigRUS`.       |
| `1.4.0-amd64-ta-in-valluvar-preview`        | Containerimage mit dem Gebietsschema `ta-IN` und der Stimme `ta-IN-Valluvar`.        |
| `1.4.0-amd64-te-in-chitra-preview`          | Containerimage mit dem Gebietsschema `te-IN` und der Stimme `te-IN-Chitra`.          |
| `1.4.0-amd64-th-th-pattara-preview`         | Containerimage mit dem Gebietsschema `th-TH` und der Stimme `th-TH-Pattara`.         |
| `1.4.0-amd64-tr-tr-sedarus-preview`         | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-SedaRUS`.         |
| `1.4.0-amd64-vi-vn-an-preview`              | Containerimage mit dem Gebietsschema `vi-VN` und der Stimme `vi-VN-An`.              |
| `1.4.0-amd64-zh-cn-huihuirus-preview`       | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-HuihuiRUS`.       |
| `1.4.0-amd64-zh-cn-kangkang-apollo-preview` | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Kangkang-Apollo`. |
| `1.4.0-amd64-zh-cn-yaoyao-apollo-preview`   | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Yaoyao-Apollo`.   |
| `1.4.0-amd64-zh-hk-danny-apollo-preview`    | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Danny-Apollo`.    |
| `1.4.0-amd64-zh-hk-tracy-apollo-preview`    | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Tracy-Apollo`.    |
| `1.4.0-amd64-zh-hk-tracyrus-preview`        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-TracyRUS`.        |
| `1.4.0-amd64-zh-tw-hanhanrus-preview`       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-HanHanRUS`.       |
| `1.4.0-amd64-zh-tw-yating-apollo-preview`   | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Yating-Apollo`.   |
| `1.4.0-amd64-zh-tw-zhiwei-apollo-preview`   | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Zhiwei-Apollo`.   |
| `1.3.0-amd64-ar-eg-hoda-preview`            | Containerimage mit dem Gebietsschema `ar-EG` und der Stimme `ar-EG-Hoda`.            |
| `1.3.0-amd64-ar-sa-naayf-preview`           | Containerimage mit dem Gebietsschema `ar-SA` und der Stimme `ar-SA-Naayf`.           |
| `1.3.0-amd64-bg-bg-ivan-preview`            | Containerimage mit dem Gebietsschema `bg-BG` und der Stimme `bg-BG-Ivan`.            |
| `1.3.0-amd64-ca-es-herenarus-preview`       | Containerimage mit dem Gebietsschema `ca-ES` und der Stimme `ca-ES-HerenaRUS`.       |
| `1.3.0-amd64-cs-cz-jakub-preview`           | Containerimage mit dem Gebietsschema `cs-CZ` und der Stimme `cs-CZ-Jakub`.           |
| `1.3.0-amd64-da-dk-hellerus-preview`        | Containerimage mit dem Gebietsschema `da-DK` und der Stimme `da-DK-HelleRUS`.        |
| `1.3.0-amd64-de-at-michael-preview`         | Containerimage mit dem Gebietsschema `de-AT` und der Stimme `de-AT-Michael`.         |
| `1.3.0-amd64-de-ch-karsten-preview`         | Containerimage mit dem Gebietsschema `de-CH` und der Stimme `de-CH-Karsten`.         |
| `1.3.0-amd64-de-de-hedda-preview`           | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           |
| `1.3.0-amd64-de-de-heddarus-preview`        | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-HeddaRUS`.        |
| `1.3.0-amd64-de-de-stefan-apollo-preview`   | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Stefan-Apollo`.   |
| `1.3.0-amd64-el-gr-stefanos-preview`        | Containerimage mit dem Gebietsschema `el-GR` und der Stimme `el-GR-Stefanos`.        |
| `1.3.0-amd64-en-au-catherine-preview`       | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-Catherine`.       |
| `1.3.0-amd64-en-au-hayleyrus-preview`       | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-HayleyRUS`.       |
| `1.3.0-amd64-en-ca-heatherrus-preview`      | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-HeatherRUS`.      |
| `1.3.0-amd64-en-ca-linda-preview`           | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-Linda`.           |
| `1.3.0-amd64-en-gb-george-apollo-preview`   | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-George-Apollo`.   |
| `1.3.0-amd64-en-gb-hazelrus-preview`        | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-HazelRUS`.        |
| `1.3.0-amd64-en-gb-susan-apollo-preview`    | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-Susan-Apollo`.    |
| `1.3.0-amd64-en-ie-sean-preview`            | Containerimage mit dem Gebietsschema `en-IE` und der Stimme `en-IE-Sean`.            |
| `1.3.0-amd64-en-in-heera-apollo-preview`    | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Heera-Apollo`.    |
| `1.3.0-amd64-en-in-priyarus-preview`        | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-PriyaRUS`.        |
| `1.3.0-amd64-en-in-ravi-apollo-preview`     | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Ravi-Apollo`.     |
| `1.3.0-amd64-en-us-benjaminrus-preview`     | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-BenjaminRUS`.     |
| `1.3.0-amd64-en-us-guy24krus-preview`       | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Guy24kRUS`.       |
| `1.3.0-amd64-en-us-jessa24krus-preview`     | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Jessa24kRUS`.     |
| `1.3.0-amd64-en-us-jessarus-preview`        | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-JessaRUS`.        |
| `1.3.0-amd64-en-us-zirarus-preview`         | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-ZiraRUS`.         |
| `1.3.0-amd64-es-es-helenarus-preview`       | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-HelenaRUS`.       |
| `1.3.0-amd64-es-es-laura-apollo-preview`    | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Laura-Apollo`.    |
| `1.3.0-amd64-es-es-pablo-apollo-preview`    | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Pablo-Apollo`.    |
| `1.3.0-amd64-es-mx-hildarus-preview`        | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-HildaRUS`.        |
| `1.3.0-amd64-es-mx-raul-apollo-preview`     | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-Raul-Apollo`.     |
| `1.3.0-amd64-fi-fi-heidirus-preview`        | Containerimage mit dem Gebietsschema `fi-FI` und der Stimme `fi-FI-HeidiRUS`.        |
| `1.3.0-amd64-fr-ca-caroline-preview`        | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-Caroline`.        |
| `1.3.0-amd64-fr-ca-harmonierus-preview`     | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-HarmonieRUS`.     |
| `1.3.0-amd64-fr-ch-guillaume-preview`       | Containerimage mit dem Gebietsschema `fr-CH` und der Stimme `fr-CH-Guillaume`.       |
| `1.3.0-amd64-fr-fr-hortenserus-preview`     | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-HortenseRUS`.     |
| `1.3.0-amd64-fr-fr-julie-apollo-preview`    | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Julie-Apollo`.    |
| `1.3.0-amd64-fr-fr-paul-apollo-preview`     | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Paul-Apollo`.     |
| `1.3.0-amd64-he-il-asaf-preview`            | Containerimage mit dem Gebietsschema `he-IL` und der Stimme `he-IL-Asaf`.            |
| `1.3.0-amd64-hi-in-hemant-preview`          | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Hemant`.          |
| `1.3.0-amd64-hi-in-kalpana-preview`         | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana`.         |
| `1.3.0-amd64-hi-in-kalpana-preview`         | Containerimage mit dem Gebietsschema `hi-IN` und der Stimme `hi-IN-Kalpana`.         |
| `1.3.0-amd64-hr-hr-matej-preview`           | Containerimage mit dem Gebietsschema `hr-HR` und der Stimme `hr-HR-Matej`.           |
| `1.3.0-amd64-hu-hu-szabolcs-preview`        | Containerimage mit dem Gebietsschema `hu-HU` und der Stimme `hu-HU-Szabolcs`.        |
| `1.3.0-amd64-id-id-andika-preview`          | Containerimage mit dem Gebietsschema `id-ID` und der Stimme `id-ID-Andika`.          |
| `1.3.0-amd64-it-it-cosimo-apollo-preview`   | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-Cosimo-Apollo`.   |
| `1.3.0-amd64-it-it-luciarus-preview`        | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-LuciaRUS`.        |
| `1.3.0-amd64-ja-jp-ayumi-apollo-preview`    | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ayumi-Apollo`.    |
| `1.3.0-amd64-ja-jp-harukarus-preview`       | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-HarukaRUS`.       |
| `1.3.0-amd64-ja-jp-ichiro-apollo-preview`   | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ichiro-Apollo`.   |
| `1.3.0-amd64-ko-kr-heamirus-preview`        | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-HeamiRUS`.        |
| `1.3.0-amd64-ms-my-rizwan-preview`          | Containerimage mit dem Gebietsschema `ms-MY` und der Stimme `ms-MY-Rizwan`.          |
| `1.3.0-amd64-nb-no-huldarus-preview`        | Containerimage mit dem Gebietsschema `nb-NO` und der Stimme `nb-NO-HuldaRUS`.        |
| `1.3.0-amd64-nl-nl-hannarus-preview`        | Containerimage mit dem Gebietsschema `nl-NL` und der Stimme `nl-NL-HannaRUS`.        |
| `1.3.0-amd64-pl-pl-paulinarus-preview`      | Containerimage mit dem Gebietsschema `pl-PL` und der Stimme `pl-PL-PaulinaRUS`.      |
| `1.3.0-amd64-pt-br-daniel-apollo-preview`   | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-Daniel-Apollo`.   |
| `1.3.0-amd64-pt-br-heloisarus-preview`      | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-HeloisaRUS`.      |
| `1.3.0-amd64-pt-pt-heliarus-preview`        | Containerimage mit dem Gebietsschema `pt-PT` und der Stimme `pt-PT-HeliaRUS`.        |
| `1.3.0-amd64-ro-ro-andrei-preview`          | Containerimage mit dem Gebietsschema `ro-RO` und der Stimme `ro-RO-Andrei`.          |
| `1.3.0-amd64-ru-ru-ekaterinarus-preview`    | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-EkaterinaRUS`.    |
| `1.3.0-amd64-ru-ru-irina-apollo-preview`    | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Irina-Apollo`.    |
| `1.3.0-amd64-ru-ru-pavel-apollo-preview`    | Containerimage mit dem Gebietsschema `ru-RU` und der Stimme `ru-RU-Pavel-Apollo`.    |
| `1.3.0-amd64-sk-sk-filip-preview`           | Containerimage mit dem Gebietsschema `sk-SK` und der Stimme `sk-SK-Filip`.           |
| `1.3.0-amd64-sl-si-lado-preview`            | Containerimage mit dem Gebietsschema `sl-SI` und der Stimme `sl-SI-Lado`.            |
| `1.3.0-amd64-sv-se-hedvigrus-preview`       | Containerimage mit dem Gebietsschema `sv-SE` und der Stimme `sv-SE-HedvigRUS`.       |
| `1.3.0-amd64-ta-in-valluvar-preview`        | Containerimage mit dem Gebietsschema `ta-IN` und der Stimme `ta-IN-Valluvar`.        |
| `1.3.0-amd64-te-in-chitra-preview`          | Containerimage mit dem Gebietsschema `te-IN` und der Stimme `te-IN-Chitra`.          |
| `1.3.0-amd64-th-th-pattara-preview`         | Containerimage mit dem Gebietsschema `th-TH` und der Stimme `th-TH-Pattara`.         |
| `1.3.0-amd64-tr-tr-sedarus-preview`         | Containerimage mit dem Gebietsschema `tr-TR` und der Stimme `tr-TR-SedaRUS`.         |
| `1.3.0-amd64-vi-vn-an-preview`              | Containerimage mit dem Gebietsschema `vi-VN` und der Stimme `vi-VN-An`.              |
| `1.3.0-amd64-zh-cn-huihuirus-preview`       | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-HuihuiRUS`.       |
| `1.3.0-amd64-zh-cn-kangkang-apollo-preview` | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Kangkang-Apollo`. |
| `1.3.0-amd64-zh-cn-yaoyao-apollo-preview`   | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Yaoyao-Apollo`.   |
| `1.3.0-amd64-zh-hk-danny-apollo-preview`    | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Danny-Apollo`.    |
| `1.3.0-amd64-zh-hk-tracy-apollo-preview`    | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-Tracy-Apollo`.    |
| `1.3.0-amd64-zh-hk-tracyrus-preview`        | Containerimage mit dem Gebietsschema `zh-HK` und der Stimme `zh-HK-TracyRUS`.        |
| `1.3.0-amd64-zh-tw-hanhanrus-preview`       | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-HanHanRUS`.       |
| `1.3.0-amd64-zh-tw-yating-apollo-preview`   | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Yating-Apollo`.   |
| `1.3.0-amd64-zh-tw-zhiwei-apollo-preview`   | Containerimage mit dem Gebietsschema `zh-TW` und der Stimme `zh-TW-Zhiwei-Apollo`.   |
| `1.2.0-amd64-de-de-hedda-preview`           | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           |
| `1.2.0-amd64-de-de-heddarus-preview`        | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-HeddaRUS`.        |
| `1.2.0-amd64-de-de-stefan-apollo-preview`   | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Stefan-Apollo`.   |
| `1.2.0-amd64-en-au-catherine-preview`       | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-Catherine`.       |
| `1.2.0-amd64-en-au-hayleyrus-preview`       | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-HayleyRUS`.       |
| `1.2.0-amd64-en-gb-george-apollo-preview`   | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-George-Apollo`.   |
| `1.2.0-amd64-en-gb-hazelrus-preview`        | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-HazelRUS`.        |
| `1.2.0-amd64-en-gb-susan-apollo-preview`    | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-Susan-Apollo`.    |
| `1.2.0-amd64-en-in-heera-apollo-preview`    | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Heera-Apollo`.    |
| `1.2.0-amd64-en-in-priyarus-preview`        | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-PriyaRUS`.        |
| `1.2.0-amd64-en-in-ravi-apollo-preview`     | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Ravi-Apollo`.     |
| `1.2.0-amd64-en-us-benjaminrus-preview`     | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-BenjaminRUS`.     |
| `1.2.0-amd64-en-us-guy24krus-preview`       | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Guy24kRUS`.       |
| `1.2.0-amd64-en-us-jessa24krus-preview`     | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Jessa24kRUS`.     |
| `1.2.0-amd64-en-us-jessarus-preview`        | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-JessaRUS`.        |
| `1.2.0-amd64-en-us-zirarus-preview`         | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-ZiraRUS`.         |
| `1.2.0-amd64-es-es-helenarus-preview`       | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-HelenaRUS`.       |
| `1.2.0-amd64-es-es-laura-apollo-preview`    | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Laura-Apollo`.    |
| `1.2.0-amd64-es-es-pablo-apollo-preview`    | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Pablo-Apollo`.    |
| `1.2.0-amd64-es-mx-hildarus-preview`        | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-HildaRUS`.        |
| `1.2.0-amd64-es-mx-raul-apollo-preview`     | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-Raul-Apollo`.     |
| `1.2.0-amd64-fr-ca-caroline-preview`        | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-Caroline`.        |
| `1.2.0-amd64-fr-ca-harmonierus-preview`     | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-HarmonieRUS`.     |
| `1.2.0-amd64-fr-fr-hortenserus-preview`     | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-HortenseRUS`.     |
| `1.2.0-amd64-fr-fr-julie-apollo-preview`    | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Julie-Apollo`.    |
| `1.2.0-amd64-fr-fr-paul-apollo-preview`     | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Paul-Apollo`.     |
| `1.2.0-amd64-it-it-cosimo-apollo-preview`   | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-Cosimo-Apollo`.   |
| `1.2.0-amd64-it-it-luciarus-preview`        | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-LuciaRUS`.        |
| `1.2.0-amd64-ja-jp-ayumi-apollo-preview`    | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ayumi-Apollo`.    |
| `1.2.0-amd64-ja-jp-harukarus-preview`       | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-HarukaRUS`.       |
| `1.2.0-amd64-ja-jp-ichiro-apollo-preview`   | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ichiro-Apollo`.   |
| `1.2.0-amd64-ko-kr-heamirus-preview`        | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-HeamiRUS`.        |
| `1.2.0-amd64-pt-br-daniel-apollo-preview`   | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-Daniel-Apollo`.   |
| `1.2.0-amd64-pt-br-heloisarus-preview`      | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-HeloisaRUS`.      |
| `1.2.0-amd64-zh-cn-huihuirus-preview`       | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-HuihuiRUS`.       |
| `1.2.0-amd64-zh-cn-kangkang-apollo-preview` | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Kangkang-Apollo`. |
| `1.2.0-amd64-zh-cn-yaoyao-apollo-preview`   | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Yaoyao-Apollo`.   |
| `1.1.0-amd64-de-de-hedda-preview`           | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Hedda`.           |
| `1.1.0-amd64-de-de-heddarus-preview`        | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-HeddaRUS`.        |
| `1.1.0-amd64-de-de-stefan-apollo-preview`   | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-Stefan-Apollo`.   |
| `1.1.0-amd64-en-au-catherine-preview`       | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-Catherine`.       |
| `1.1.0-amd64-en-au-hayleyrus-preview`       | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-HayleyRUS`.       |
| `1.1.0-amd64-en-gb-george-apollo-preview`   | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-George-Apollo`.   |
| `1.1.0-amd64-en-gb-hazelrus-preview`        | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-HazelRUS`.        |
| `1.1.0-amd64-en-gb-susan-apollo-preview`    | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-Susan-Apollo`.    |
| `1.1.0-amd64-en-in-heera-apollo-preview`    | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Heera-Apollo`.    |
| `1.1.0-amd64-en-in-priyarus-preview`        | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-PriyaRUS`.        |
| `1.1.0-amd64-en-in-ravi-apollo-preview`     | Containerimage mit dem Gebietsschema `en-IN` und der Stimme `en-IN-Ravi-Apollo`.     |
| `1.1.0-amd64-en-us-benjaminrus-preview`     | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-BenjaminRUS`.     |
| `1.1.0-amd64-en-us-guy24krus-preview`       | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Guy24kRUS`.       |
| `1.1.0-amd64-en-us-jessa24krus-preview`     | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Jessa24kRUS`.     |
| `1.1.0-amd64-en-us-jessarus-preview`        | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-JessaRUS`.        |
| `1.1.0-amd64-en-us-zirarus-preview`         | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-ZiraRUS`.         |
| `1.1.0-amd64-es-es-helenarus-preview`       | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-HelenaRUS`.       |
| `1.1.0-amd64-es-es-laura-apollo-preview`    | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Laura-Apollo`.    |
| `1.1.0-amd64-es-es-pablo-apollo-preview`    | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-Pablo-Apollo`.    |
| `1.1.0-amd64-es-mx-hildarus-preview`        | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-HildaRUS`.        |
| `1.1.0-amd64-es-mx-raul-apollo-preview`     | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-Raul-Apollo`.     |
| `1.1.0-amd64-fr-ca-caroline-preview`        | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-Caroline`.        |
| `1.1.0-amd64-fr-ca-harmonierus-preview`     | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-HarmonieRUS`.     |
| `1.1.0-amd64-fr-fr-hortenserus-preview`     | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-HortenseRUS`.     |
| `1.1.0-amd64-fr-fr-julie-apollo-preview`    | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Julie-Apollo`.    |
| `1.1.0-amd64-fr-fr-paul-apollo-preview`     | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-Paul-Apollo`.     |
| `1.1.0-amd64-it-it-cosimo-apollo-preview`   | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-Cosimo-Apollo`.   |
| `1.1.0-amd64-it-it-luciarus-preview`        | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-LuciaRUS`.        |
| `1.1.0-amd64-ja-jp-ayumi-apollo-preview`    | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ayumi-Apollo`.    |
| `1.1.0-amd64-ja-jp-harukarus-preview`       | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-HarukaRUS`.       |
| `1.1.0-amd64-ja-jp-ichiro-apollo-preview`   | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-Ichiro-Apollo`.   |
| `1.1.0-amd64-ko-kr-heamirus-preview`        | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-HeamiRUS`.        |
| `1.1.0-amd64-pt-br-daniel-apollo-preview`   | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-Daniel-Apollo`.   |
| `1.1.0-amd64-pt-br-heloisarus-preview`      | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-HeloisaRUS`.      |
| `1.1.0-amd64-zh-cn-huihuirus-preview`       | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-HuihuiRUS`.       |
| `1.1.0-amd64-zh-cn-kangkang-apollo-preview` | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Kangkang-Apollo`. |
| `1.1.0-amd64-zh-cn-yaoyao-apollo-preview`   | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Yaoyao-Apollo`.   |
| `1.0.0-amd64-en-us-benjaminrus-preview`     | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-BenjaminRUS`.     |
| `1.0.0-amd64-en-us-guy24krus-preview`       | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Guy24kRUS`.       |
| `1.0.0-amd64-en-us-jessa24krus-preview`     | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-Jessa24kRUS`.     |
| `1.0.0-amd64-en-us-jessarus-preview`        | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-JessaRUS`.        |
| `1.0.0-amd64-en-us-zirarus-preview`         | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-ZiraRUS`.         |
| `1.0.0-amd64-zh-cn-huihuirus-preview`       | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-HuihuiRUS`.       |
| `1.0.0-amd64-zh-cn-kangkang-apollo-preview` | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Kangkang-Apollo`. |
| `1.0.0-amd64-zh-cn-yaoyao-apollo-preview`   | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-Yaoyao-Apollo`.   |

## <a name="neural-text-to-speech"></a>Text-zu-Sprache (neuronal)

Das [Text-zu-Sprache (neuronal)][sp-ntts]-Containerimage befindet sich in der `containerpreview.azurecr.io`-Containerregistrierung. Es befindet sich im Repository `microsoft` und trägt den Namen `cognitive-services-neural-text-to-speech`. Der vollqualifizierte Containerimagename lautet `containerpreview.azurecr.io/microsoft/cognitive-services-neural-text-to-speech`.

Dieses Containerimage verfügt über die folgenden Tags:

| Imagetags                                  | Notizen                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaNeural`.      |
| `1.2.0-amd64-de-de-katjaneural-preview`     | Containerimage mit dem Gebietsschema `de-DE` und der Stimme `de-DE-KatjaNeural`.     |
| `1.2.0-amd64-en-au-natashaneural-preview`   | Containerimage mit dem Gebietsschema `en-AU` und der Stimme `en-AU-NatashaNeural`.   |
| `1.2.0-amd64-en-ca-claraneural-preview`     | Containerimage mit dem Gebietsschema `en-CA` und der Stimme `en-CA-ClaraNeural`.     |
| `1.2.0-amd64-en-gb-libbyneural-preview`     | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-LibbyNeural`.     |
| `1.2.0-amd64-en-gb-mianeural-preview`       | Containerimage mit dem Gebietsschema `en-GB` und der Stimme `en-GB-MiaNeural`.       |
| `1.2.0-amd64-en-us-arianeural-preview`      | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-AriaNeural`.      |
| `1.2.0-amd64-en-us-guyneural-preview`       | Containerimage mit dem Gebietsschema `en-US` und der Stimme `en-US-GuyNeural`.       |
| `1.2.0-amd64-es-es-elviraneural-preview`    | Containerimage mit dem Gebietsschema `es-ES` und der Stimme `es-ES-ElviraNeural`.    |
| `1.2.0-amd64-es-mx-dalianeural-preview`     | Containerimage mit dem Gebietsschema `es-MX` und der Stimme `es-MX-DaliaNeural`.     |
| `1.2.0-amd64-fr-ca-sylvieneural-preview`    | Containerimage mit dem Gebietsschema `fr-CA` und der Stimme `fr-CA-SylvieNeural`.    |
| `1.2.0-amd64-fr-fr-deniseneural-preview`    | Containerimage mit dem Gebietsschema `fr-FR` und der Stimme `fr-FR-DeniseNeural`.    |
| `1.2.0-amd64-it-it-elsaneural-preview`      | Containerimage mit dem Gebietsschema `it-IT` und der Stimme `it-IT-ElsaNeural`.      |
| `1.2.0-amd64-ja-jp-nanamineural-preview`    | Containerimage mit dem Gebietsschema `ja-JP` und der Stimme `ja-JP-NanamiNeural`.    |
| `1.2.0-amd64-ko-kr-sunhineural-preview`     | Containerimage mit dem Gebietsschema `ko-KR` und der Stimme `ko-KR-SunHiNeural`.     |
| `1.2.0-amd64-pt-br-franciscaneural-preview` | Containerimage mit dem Gebietsschema `pt-BR` und der Stimme `pt-BR-FranciscaNeural`. |
| `1.2.0-amd64-zh-cn-xiaoxiaoneural-preview`  | Containerimage mit dem Gebietsschema `zh-CN` und der Stimme `zh-CN-XiaoxiaoNeural`.  |

## <a name="key-phrase-extraction"></a>Schlüsselwortextraktion

Das Containerimage [Schlüsselbegriffserkennung][ta-kp] befindet sich im `mcr.microsoft.com`-Containerregistrierungssyndikat. Es befindet sich im Repository `azure-cognitive-services` und trägt den Namen `keyphrase`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/keyphrase`.

Dieses Containerimage verfügt über die folgenden Tags:

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008510001-amd64-preview` |       |
| `1.1.007750002-amd64-preview` |       |
| `1.1.007360001-amd64-preview` |       |
| `1.1.006770001-amd64-preview` |       |

## <a name="language-detection"></a>Spracherkennung

Das Containerimage [Sprachenerkennung][ta-la] befindet sich im `mcr.microsoft.com`-Containerregistrierungssyndikat. Es befindet sich im Repository `azure-cognitive-services` und trägt den Namen `language`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/language`.

Dieses Containerimage verfügt über die folgenden Tags:

| Imagetags                    | Notizen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008510001-amd64-preview` |       |
| `1.1.007750002-amd64-preview` |       |
| `1.1.007360001-amd64-preview` |       |
| `1.1.006770001-amd64-preview` |       |

## <a name="sentiment-analysis"></a>Standpunktanalyse

Das Containerimage [Standpunktanalyse][ta-se] befindet sich im `mcr.microsoft.com`-Containerregistrierungssyndikat. Es befindet sich im Repository `azure-cognitive-services` und trägt den Namen `sentiment`. Der vollqualifizierte Containerimagename lautet `mcr.microsoft.com/azure-cognitive-services/sentiment`.

Dieses Containerimage verfügt über die folgenden Tags:

| Imagetags | Notizen                                         |
|------------|:----------------------------------------------|
| `latest`   |                                               |
| `3.0-en`   | Standpunktanalyse v3 (Englisch)               |
| `3.0-es`   | Standpunktanalyse v3 (Spanisch)               |
| `3.0-fr`   | Standpunktanalyse v3 (Französisch)                |
| `3.0-it`   | Standpunktanalyse v3 (Italienisch)               |
| `3.0-de`   | Standpunktanalyse v3 (Deutsch)                |
| `3.0-zh`   | Standpunktanalyse v3 (Chinesisch, vereinfacht)  |
| `3.0-zht`  | Standpunktanalyse v3 (Chinesisch, traditionell) |
| `3.0-ja`   | Standpunktanalyse v3 (Japanisch)              |
| `3.0-pt`   | Standpunktanalyse v3 (Portugiesisch)            |
| `3.0-nl`   | Standpunktanalyse v3 (Niederländisch)                 |
| `1.1.009301-amd64-preview`    | Standpunktanalyse v2      |
| `1.1.008510001-amd64-preview` |       |
| `1.1.007750002-amd64-preview` |       |
| `1.1.007360001-amd64-preview` |       |
| `1.1.006770001-amd64-preview` |       |

[ad-containers]: ../anomaly-Detector/anomaly-detector-container-howto.md
[cv-containers]: ../computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: ../face/face-how-to-install-containers.md
[fr-containers]: ../form-recognizer/form-recognizer-container-howto.md
[lu-containers]: ../luis/luis-container-howto.md
[sp-stt]: ../speech-service/speech-container-howto.md?tabs=stt
[sp-cstt]: ../speech-service/speech-container-howto.md?tabs=cstt
[sp-tts]: ../speech-service/speech-container-howto.md?tabs=tts
[sp-ctts]: ../speech-service/speech-container-howto.md?tabs=ctts
[ta-kp]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=keyphrase
[ta-la]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=language
[ta-se]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=sentiment
