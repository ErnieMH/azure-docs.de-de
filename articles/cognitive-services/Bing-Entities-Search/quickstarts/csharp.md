---
title: 'Schnellstart: Senden einer Suchanforderung an die REST-API mithilfe von C# – Bing-Entitätssuche'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie diese Schnellstartanleitung zum Senden einer Anforderung an die Bing-Entitätssuche-REST-API mit C#, und Sie erhalten eine JSON-Antwort.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 05/08/2020
ms.author: aahi
ms.custom: devx-track-csharp
ms.openlocfilehash: 131c282ab61f04ffa94921c2816886a35ea4b7ac
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "88936825"
---
# <a name="quickstart-send-a-search-request-to-the-bing-entity-search-rest-api-using-c"></a>Schnellstart: Senden einer Suchanforderung an die Bing-Entitätssuche-REST-API mithilfe von C#

Verwenden Sie diese Schnellstartanleitung, um die Bing-Entitätssuche-API zum ersten Mal aufzurufen und die JSON-Antwort anzuzeigen. Diese einfache C#-Anwendung sendet eine Nachrichtensuchabfrage an die API und zeigt die Antwort an. Der Quellcode für diese Anwendung ist auf [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Search/BingEntitySearchv7.cs) verfügbar.

Die Anwendung ist zwar in C# geschrieben, an sich ist die API aber ein RESTful-Webdienst, der mit den meisten Programmiersprachen kompatibel ist.


## <a name="prerequisites"></a>Voraussetzungen

- Eine beliebige Edition von [Visual Studio 2017 oder höher](https://www.visualstudio.com/downloads/).

- Das [Json.NET](https://www.newtonsoft.com/json)-Framework, das als NuGet-Paket verfügbar ist So installieren Sie das NuGet-Paket in Visual Studio

   1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf Ihr Projekt.
   2. Wählen Sie **NuGet-Pakete verwalten** aus.
   3. Suchen Sie nach *Newtonsoft.Json*, wählen Sie das Paket aus, und installieren Sie es anschließend.

- Unter Linux/macOS kann diese Anwendung mit [Mono](https://www.mono-project.com/) ausgeführt werden.


[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Erstellen und Initialisieren eines Projekts

1. Erstellen Sie in Visual Studio eine neue C#-Konsolenprojektmappe. Fügen Sie dann die folgenden Namespaces in die Hauptcodedatei ein:
    
    ```csharp
    using Newtonsoft.Json;
    using System;
    using System.Net.Http;
    using System.Text;
    ```

2. Erstellen Sie eine neue Klasse, und fügen Sie Variablen für den API-Endpunkt, Ihren Abonnementschlüssel und die Abfrage hinzu, die Sie durchsuchen möchten. Sie können den globalen Endpunkt im folgenden Code oder den Endpunkt der [benutzerdefinierten Unterdomäne](../../../cognitive-services/cognitive-services-custom-subdomains.md) verwenden, der im Azure-Portal für Ihre Ressource angezeigt wird.

    ```csharp
    namespace EntitySearchSample
    {
        class Program
        {
            static string host = "https://api.cognitive.microsoft.com";
            static string path = "/bing/v7.0/entities";
    
            static string market = "en-US";
    
            // NOTE: Replace this example key with a valid subscription key.
            static string key = "ENTER YOUR KEY HERE";
    
            static string query = "italian restaurant near me";
        //...
        }
    }
    ```

## <a name="send-a-request-and-get-the-api-response"></a>Senden einer Anforderung und Erhalten der API-Antwort

1. Erstellen Sie innerhalb der Klasse eine Funktion mit dem Namen `Search()`. In dieser Funktion erstellen Sie ein neues `HttpClient`-Objekt und fügen dem Header `Ocp-Apim-Subscription-Key` Ihren Abonnementschlüssel hinzu.

2. Erstellen Sie den URI für die Anforderung, indem Sie den Host und Pfad kombinieren. Fügen Sie dann Ihren Markt hinzu, und führen Sie die URL-Codierung für Ihre Abfrage durch.

3. Warten Sie, bis `client.GetAsync()` eine HTTP-Antwort erhält, und speichern Sie die JSON-Antwort, indem Sie auf `ReadAsStringAsync()` warten.

4. Formatieren Sie die JSON-Zeichenfolge mit `JsonConvert.DeserializeObject()`, und geben Sie sie in der Konsole aus.

      ```csharp
      async static void Search()
      {
       //...
       HttpClient client = new HttpClient();
       client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", key);

       string uri = host + path + "?mkt=" + market + "&q=" + System.Net.WebUtility.UrlEncode(query);

       HttpResponseMessage response = await client.GetAsync(uri);

       string contentString = await response.Content.ReadAsStringAsync();
       dynamic parsedJson = JsonConvert.DeserializeObject(contentString);
       Console.WriteLine(parsedJson);
      }
      ```

5. Rufen Sie in der `Main()`-Methode Ihrer Anwendung die `Search()`-Funktion auf.
    
    ```csharp
    static void Main(string[] args)
    {
        Search();
        Console.ReadLine();
    }
    ```


## <a name="example-json-response"></a>JSON-Beispielantwort

Es wird eine erfolgreiche Antwort im JSON-Format zurückgegeben, wie im folgenden Beispiel gezeigt: 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "italian restaurant near me",
    "askUserForLocation": true
  },
  "places": {
    "value": [
      {
        "_type": "LocalBusiness",
        "webSearchUrl": "https://www.bing.com/search?q=sinful+bakery&filters=local...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "https://www.contoso.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98112",
          "addressCountry": "US",
          "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
      },

      . . .
      {
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Pickles+and+Preserves...",
        "name": "Munson's Pickles and Preserves Farm",
        "url": "https://www.princi.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness",
            "Restaurant"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98101",
          "addressCountry": "US",
          "neighborhood": "Capitol Hill"
        },
        "telephone": "(800) 555-1212"
      },
      
      . . .
    ]
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Einzelseiten-Web-App](../tutorial-bing-entities-search-single-page-app.md)

* [Was ist die Bing-Entitätssuche-API?](../overview.md )
* [Referenz zur Bing-Entitätssuche-API](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference).
