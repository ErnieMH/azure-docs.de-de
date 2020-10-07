---
title: 'Schnellstart: Gewinnen von Erkenntnissen zu Bildern mithilfe der REST-API und Node.js – Visuelle Bing-Suche'
titleSuffix: Azure Cognitive Services
description: Hier wird beschrieben, wie Sie ein Bild mit der API für die visuelle Bing-Suche und Node.js hochladen und dann Erkenntnisse zum Bild abrufen.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 05/22/2020
ms.author: scottwhi
ms.custom: devx-track-js
ms.openlocfilehash: e96ff9173d920208e067988602323626767d9017
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91277091"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-nodejs"></a>Schnellstart: Gewinnen von Erkenntnissen zu Bildern mit der REST-API für die visuelle Bing-Suche und Node.js

Verwenden Sie diese Schnellstartanleitung, um die API für die visuelle Bing-Suche zum ersten Mal aufzurufen. Mit dieser einfachen JavaScript-Anwendung wird ein Bild in die API hochgeladen, und es werden die zurückgegebenen Informationen angezeigt. Die Anwendung ist zwar in JavaScript geschrieben, an sich ist die API aber ein RESTful-Webdienst, der mit den meisten Programmiersprachen kompatibel ist.

## <a name="prerequisites"></a>Voraussetzungen

* [Node.js](https://nodejs.org/en/download/)
* Das Anforderungsmodul für JavaScript. Dieses Modul kann mithilfe des Befehls `npm install request` installiert werden.
* Das Formulardatenmodul. Dieses Modul kann mithilfe des Befehls `npm install form-data` installiert werden. 

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>Initialisieren der Anwendung

1. Erstellen Sie eine JavaScript-Datei in Ihrer bevorzugten IDE oder Ihrem bevorzugten Editor, und legen Sie die folgenden Anforderungen fest:

    ```javascript
    var request = require('request');
    var FormData = require('form-data');
    var fs = require('fs');
    ```

2. Erstellen Sie Variablen für Ihren API-Endpunkt, den Abonnementschlüssel und den Pfad zu Ihrem Bild. Für den `baseUri`-Wert können Sie den globalen Endpunkt im folgenden Code oder den Endpunkt der [benutzerdefinierten Unterdomäne](../../../cognitive-services/cognitive-services-custom-subdomains.md) verwenden, der im Azure-Portal für Ihre Ressource angezeigt wird.

    ```javascript
    var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
    var subscriptionKey = 'your-api-key';
    var imagePath = "path-to-your-image";
    ```

3. Erstellen Sie eine Funktion mit dem Namen `requestCallback()`, um die Antwort der API auszugeben.

    ```javascript
    function requestCallback(err, res, body) {
        console.log(JSON.stringify(JSON.parse(body), null, '  '))
    }
    ```

## <a name="construct-and-send-the-search-request"></a>Erstellen und Senden der Suchanforderung

1. Wenn Sie ein lokales Bild hochladen, müssen die Formulardaten den Header `Content-Disposition` enthalten. Legen Sie den Parameter `name` auf „image“ und den Parameter `filename` auf den Dateinamen des Bilds fest. Der Inhalt des Formulars umfasst die Binärdaten des Bilds. Das hochzuladende Bild darf maximal 1 MB groß sein.

   ```
   --boundary_1234-abcd
   Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

   ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

   --boundary_1234-abcd--
   ```

2. Erstellen Sie ein neues `FormData`-Objekt mit `FormData()`, und fügen Sie Ihren Bildpfad an, indem Sie `fs.createReadStream()` verwenden.
    
    ```javascript
    var form = new FormData();
    form.append("image", fs.createReadStream(imagePath));
    ```

3. Verwenden Sie die Anforderungsbibliothek zum Hochladen des Bilds, und rufen Sie `requestCallback()` auf, um die Antwort auszugeben. Fügen Sie dem Anforderungsheader Ihren Abonnementschlüssel hinzu.

    ```javascript
    form.getLength(function(err, length){
      if (err) {
        return requestCallback(err);
      }
      var r = request.post(baseUri, requestCallback);
      r._form = form; 
      r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
    });
    ```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Single-Page-Web-App für die visuelle Suche](../tutorial-bing-visual-search-single-page-app.md)
