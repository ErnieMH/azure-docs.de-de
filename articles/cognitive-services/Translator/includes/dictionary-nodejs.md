---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.custom: devx-track-javascript
ms.openlocfilehash: c6ca405a6e14f92ab0b09d8f277c99c0c51288bf
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87404621"
---
[!INCLUDE [Prerequisites](prerequisites-nodejs.md)]

[!INCLUDE [Set up and use environment variables](setup-env-variables.md)]

## <a name="create-a-project-and-import-required-modules"></a>Erstellen eines Projekts und Importieren der erforderlichen Module

Erstellen Sie in Ihrer bevorzugten IDE oder Ihrem bevorzugten Editor ein neues Projekt, oder erstellen Sie einen neuen Ordner auf dem Desktop. Kopieren Sie den folgenden Codeausschnitt in Ihrem Projekt bzw. Ihrem Ordner in die Datei `alt-translations.js`.

```javascript
const request = require('request');
const uuidv4 = require('uuid/v4');
```

> [!NOTE]
> Wenn Sie diese Module bisher nicht verwendet haben, müssen Sie sie vor der Ausführung Ihres Programms installieren. Führen Sie zum Installieren dieser Pakete `npm install request uuidv4` aus.

Diese Module sind zum Erstellen der HTTP-Anforderung und eines eindeutigen Bezeichners für den `'X-ClientTraceId'`-Header erforderlich.

## <a name="set-the-subscription-key-and-endpoint"></a>Festlegen des Abonnementschlüssels und Endpunkts

In diesem Beispiel werden der Translator-Abonnementschlüssel und der Endpunkt aus den Umgebungsvariablen `TRANSLATOR_TEXT_SUBSCRIPTION_KEY` und `TRANSLATOR_TEXT_ENDPOINT` gelesen. Wenn Sie mit Umgebungsvariablen nicht vertraut sind, können Sie `subscriptionKey` und `endpoint` als Zeichenfolge festlegen und die Bedingungsanweisungen auskommentieren.

Kopieren Sie diesen Code in Ihr Projekt:

```javascript
var key_var = 'TRANSLATOR_TEXT_SUBSCRIPTION_KEY';
if (!process.env[key_var]) {
    throw new Error('Please set/export the following environment variable: ' + key_var);
}
var subscriptionKey = process.env[key_var];
var endpoint_var = 'TRANSLATOR_TEXT_ENDPOINT';
if (!process.env[endpoint_var]) {
    throw new Error('Please set/export the following environment variable: ' + endpoint_var);
}
var endpoint = process.env[endpoint_var];
```

## <a name="configure-the-request"></a>Konfigurieren der Anforderung

Mit der über das Anforderungsmodul zur Verfügung gestellten `request()`-Methode können Sie die HTTP-Methode, die URL, Anforderungsparameter und den JSON-Text als `options`-Objekt übergeben. In diesem Codeausschnitt wird die Anforderung konfiguriert:

>[!NOTE]
> Weitere Informationen zu Endpunkten, Routen und Anforderungsparametern finden Sie unter [Translator 3.0: Wörterbuchsuche](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-dictionary-lookup).

```javascript
let options = {
    method: 'POST',
    baseUrl: endpoint,
    url: 'dictionary/lookup',
    qs: {
      'api-version': '3.0',
      'from': 'en',
      'to': 'es'
    },
    headers: {
      'Ocp-Apim-Subscription-Key': subscriptionKey,
      'Content-type': 'application/json',
      'X-ClientTraceId': uuidv4().toString()
    },
    body: [{
          'text': 'Elephants'
    }],
    json: true,
};
```

Eine Anforderung lässt sich am einfachsten authentifizieren, indem Sie den Abonnementschlüssel als `Ocp-Apim-Subscription-Key`-Header übergeben. Diese Vorgehensweise wird in diesem Beispiel verwendet. Alternativ können Sie den Abonnementschlüssel durch ein Zugriffstoken ersetzen und dieses als `Authorization`-Header übergeben, um Ihre Anforderung zu überprüfen.

Wenn Sie ein Cognitive Services-Abonnement mehrerer Dienste verwenden, müssen Sie auch `Ocp-Apim-Subscription-Region` in Ihre Anforderungsheader aufnehmen.

Weitere Informationen finden Sie unter [Authentifizierung](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="make-the-request-and-print-the-response"></a>Senden der Anforderung und Ausgeben der Antwort

Erstellen Sie als Nächstes mithilfe der `request()`-Methode die Anforderung. Sie verwendet das im vorherigen Abschnitt erstellte `options`-Objekt als erstes Argument und gibt dann die übersichtlicher gestaltete JSON-Antwort aus.

```javascript
request(options, function(err, res, body){
    console.log(JSON.stringify(body, null, 4));
});
```

>[!NOTE]
> In diesem Beispiel wird die HTTP-Anforderung im `options`-Objekt definiert. Das Anforderungsmodul unterstützt außerdem praktische Methoden wie `.post` und `.get`. Weitere Informationen finden Sie unter [Convenience methods](https://github.com/request/request#convenience-methods) (Praktische Methoden).

## <a name="put-it-all-together"></a>Korrektes Zusammenfügen

Das war's: Sie haben ein einfaches Programm erstellt, das Translator aufruft und eine JSON-Antwort zurückgibt. Führen Sie das Programm jetzt aus:

```console
node alt-translations.js
```

[Auf GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-NodeJS) finden Sie das vollständige Beispiel, falls Sie Ihren Code mit unserem Code vergleichen möchten.

## <a name="sample-response"></a>Beispiel für eine Antwort

```json
[
    {
        "displaySource": "elephants",
        "normalizedSource": "elephants",
        "translations": [
            {
                "backTranslations": [
                    {
                        "displayText": "elephants",
                        "frequencyCount": 1207,
                        "normalizedText": "elephants",
                        "numExamples": 5
                    }
                ],
                "confidence": 1.0,
                "displayTarget": "elefantes",
                "normalizedTarget": "elefantes",
                "posTag": "NOUN",
                "prefixWord": ""
            }
        ]
    }
]
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie den Abonnementschlüssel in Ihrem Programm hartcodiert haben, entfernen Sie ihn unbedingt, wenn Sie diese Schnellstartanleitung abgeschlossen haben.

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich anhand der API-Referenz mit den Möglichkeiten von Translator vertraut.

> [!div class="nextstepaction"]
> [API-Referenz](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
