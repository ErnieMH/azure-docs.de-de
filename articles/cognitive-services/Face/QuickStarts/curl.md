---
title: 'Schnellstart: Erkennen von Gesichtern in einem Bild mit der Azure-REST-API und cURL'
titleSuffix: Azure Cognitive Services
description: In diesem Schnellstart verwenden Sie die Azure-Gesichtserkennungs-REST-API mit cURL, um Gesichter in einem Bild zu erkennen.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 11/23/2020
ms.author: pafarley
ms.openlocfilehash: 922b261ffeb6cb7a7170a50a3ba5e5ca91a10a11
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/24/2020
ms.locfileid: "96011998"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-face-rest-api-and-curl"></a>Schnellstart: Erkennen von Gesichtern in einem Bild mit der Gesichtserkennungs-REST-API und cURL

In dieser Schnellstartanleitung verwenden Sie die Azure-Gesichtserkennungs-REST-API mit cURL, um menschliche Gesichter in einem Bild zu erkennen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/cognitive-services/) erstellen, bevor Sie beginnen. 

## <a name="prerequisites"></a>Voraussetzungen

* Azure-Abonnement – [Erstellen eines kostenlosen Kontos](https://azure.microsoft.com/free/cognitive-services/)
* Sobald Sie über Ihr Azure-Abonnement verfügen, sollten Sie über <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesFace"  title="Erstellen einer Gesichtserkennungsressource"  target="_blank"> im Azure-Portal eine Gesichtserkennungsressource <span class="docon docon-navigate-external x-hidden-focus"></span></a> erstellen, um Ihren Schlüssel und Endpunkt abzurufen. Klicken Sie nach Abschluss der Bereitstellung auf **Zu Ressource wechseln**.
    * Sie benötigen den Schlüssel und Endpunkt der von Ihnen erstellten Ressource, um Ihre Anwendung mit der Gesichtserkennungs-API zu verbinden. Der Schlüssel und der Endpunkt werden weiter unten in der Schnellstartanleitung in den Code eingefügt.
    * Sie können den kostenlosen Tarif (`F0`) verwenden, um den Dienst zu testen, und später für die Produktion auf einen kostenpflichtigen Tarif upgraden.

## <a name="write-the-command"></a>Schreiben des Befehls
 
Der Befehl zum Aufrufen der Gesichtserkennungs-API sowie zum Abrufen von Gesichtsattributdaten aus einem Bild sieht in etwa wie folgt aus. Kopieren Sie den Code zunächst in einen Text-Editor. Sie müssen bestimmte Teile des Befehls ändern, bevor Sie ihn ausführen können.

:::code language="shell" source="~/cognitive-services-quickstart-code/curl/face/detect.sh" id="detection_model_2":::

### <a name="subscription-key"></a>Abonnementschlüssel
Ersetzen Sie `<Subscription Key>` durch Ihren gültigen Abonnementschlüssel für die Gesichtserkennungs-API.

### <a name="face-endpoint-url"></a>Endpunkt-URL der Gesichtserkennungs-API

Die URL `https://<My Endpoint String>.com/face/v1.0/detect` gibt den Endpunkt der Azure-Gesichtserkennungs-API an, der abgefragt werden soll. Der erste Teil dieser URL muss ggf. entsprechend des Endpunkts Ihres Abonnementschlüssels geändert werden.

[!INCLUDE [subdomains-note](../../../../includes/cognitive-services-custom-subdomains-note.md)]

### <a name="image-source-url"></a>URL der Bildquelle
Die Quell-URL gibt das Bild an, das als Eingabe verwendet werden soll. Sie können den Wert ändern, um auf ein beliebiges Bild zu verweisen, das Sie analysieren möchten.

```
https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg
``` 

## <a name="run-the-command"></a>Führen Sie den folgenden Befehl aus:

Wenn Sie die gewünschten Änderungen vorgenommen haben, öffnen Sie eine Eingabeaufforderung, und geben Sie den neuen Befehl ein. Die Gesichtserkennungsinformationen sollten daraufhin als JSON-Daten im Konsolenfenster angezeigt werden. Beispiel:

```json
[
  {
    "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f",
    "faceRectangle": {
      "top": 131,
      "left": 177,
      "width": 162,
      "height": 162
    }
  }
]  
```

## <a name="extract-face-attributes"></a>Extrahieren von Gesichtsattributen
 
Verwenden Sie zum Extrahieren von Gesichtsattributen das Erkennungsmodell 1, und fügen Sie den Abfrageparameter `returnFaceAttributes` hinzu. Der Befehl sollte jetzt wie in der folgenden Abbildung aussehen. Fügen Sie wie zuvor Ihren Abonnementschlüssel und den Endpunkt für die Gesichtserkennung ein.

:::code language="shell" source="~/cognitive-services-quickstart-code/curl/face/detect.sh" id="detection_model_1":::

Die zurückgegebenen Gesichtserkennungsinformationen enthalten jetzt Gesichtsattribute. Beispiel:

```json
[
  {
    "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f",
    "faceRectangle": {
      "top": 131,
      "left": 177,
      "width": 162,
      "height": 162
    },
    "faceAttributes": {
      "smile": 0,
      "headPose": {
        "pitch": 0,
        "roll": 0.1,
        "yaw": -32.9
      },
      "gender": "female",
      "age": 22.9,
      "facialHair": {
        "moustache": 0,
        "beard": 0,
        "sideburns": 0
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0,
        "disgust": 0,
        "fear": 0,
        "happiness": 0,
        "neutral": 0.986,
        "sadness": 0.009,
        "surprise": 0.005
      },
      "blur": {
        "blurLevel": "low",
        "value": 0.06
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.67
      },
      "noise": {
        "noiseLevel": "low",
        "value": 0
      },
      "makeup": {
        "eyeMakeup": true,
        "lipMakeup": true
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0,
        "invisible": false,
        "hairColor": [
          {
            "color": "brown",
            "confidence": 1
          },
          {
            "color": "black",
            "confidence": 0.87
          },
          {
            "color": "other",
            "confidence": 0.51
          },
          {
            "color": "blond",
            "confidence": 0.08
          },
          {
            "color": "red",
            "confidence": 0.08
          },
          {
            "color": "gray",
            "confidence": 0.02
          }
        ]
      }
    }
  }
]
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen cURL-Befehl geschrieben, der den Azure-Gesichtserkennungsdienst aufruft, um Gesichter in einem Bild zu erkennen und deren Attribute zurückzugeben. Sehen Sie sich als Nächstes die Referenzdokumentation zur Gesichtserkennungs-API an, um mehr zu erfahren.

> [!div class="nextstepaction"]
> [Gesichtserkennungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
