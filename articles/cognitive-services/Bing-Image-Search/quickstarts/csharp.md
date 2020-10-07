---
title: 'Schnellstart: Suchen nach Bildern mithilfe der Bing-Bildersuche-REST-API und C#'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie diese Schnellstartanleitung, um Bildsuchanforderungen mithilfe von C# an die Bing-Bildersuche-REST-API zu senden und JSON-Antworten zu empfangen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: quickstart
ms.date: 05/08/2020
ms.author: aahi
ms.custom: devx-track-csharp
ms.openlocfilehash: a5e957e21b431ede445fd64774fa4727eda7f803
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "88922241"
---
# <a name="quickstart-search-for-images-using-the-bing-image-search-rest-api-and-c"></a>Schnellstart: Suchen nach Bildern mithilfe der Bing-Bildersuche-REST-API und C#

In dieser Schnellstartanleitung erfahren Sie, wie Sie Suchanforderungen an die Bing-Bildersuche-API senden. Diese C#-Anwendung sendet eine Suchabfrage an die API und zeigt die URL des ersten Bilds in den Ergebnissen an. Die Anwendung ist zwar in C# geschrieben, an sich ist die API aber ein RESTful-Webdienst, der mit den meisten Programmiersprachen kompatibel ist.

Der Quellcode für dieses Beispiel ist auf [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Search/BingImageSearchv7Quickstart.cs) mit zusätzlichen Fehlerbehandlungen und Anmerkungen verfügbar.

## <a name="prerequisites"></a>Voraussetzungen
* Eine beliebige Edition von [Visual Studio 2017 oder höher](https://www.visualstudio.com/downloads/).
* Das [Json.NET](https://www.newtonsoft.com/json)-Framework, das als NuGet-Paket verfügbar ist
* Unter Linux/macOS kann diese Anwendung mit [Mono](https://www.mono-project.com/) ausgeführt werden.

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Erstellen und Initialisieren eines Projekts

1. Erstellen Sie eine neue Konsolenprojektmappe in Visual Studio mit der Bezeichnung `BingSearchApisQuickStart`. Fügen Sie der Hauptcodedatei dann die folgenden Namespaces hinzu:

    ```csharp
    using System;
    using System.Net;
    using System.IO;
    using System.Collections.Generic;
    using Newtonsoft.Json.Linq;
    ```

2. Erstellen Sie Variablen für den API-Endpunkt, Ihren Abonnementschlüssel und einen Suchbegriff. Für `uriBase` können Sie den globalen Endpunkt im folgenden Code oder den Endpunkt der [benutzerdefinierten Unterdomäne](../../../cognitive-services/cognitive-services-custom-subdomains.md) verwenden, der im Azure-Portal für Ihre Ressource angezeigt wird.

    ```csharp
    //...
    namespace BingSearchApisQuickstart
    {
        class Program
        {
        // Replace the this string with your valid access key.
        const string subscriptionKey = "enter your key here";
        const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/images/search";
        const string searchTerm = "tropical ocean";
    //...
    ```

## <a name="create-a-struct-to-format-the-bing-image-search-response"></a>Erstellen einer Struktur zum Formatieren der Bing-Bildersuche-Antwort

Definieren Sie eine `SearchResult`-Struktur, die die Bildersuchergebnisse und JSON-Headerinformationen enthält.

```csharp
    namespace BingSearchApisQuickstart
    {
        class Program
        {
        //...
            struct SearchResult
            {
                public String jsonResult;
                public Dictionary<String, String> relevantHeaders;
            }
//...
```

## <a name="create-a-method-to-send-search-requests"></a>Erstellen einer Methode zum Senden von Suchanforderungen

Erstellen Sie eine Methode mit der Bezeichnung `BingImageSearch`, um die API aufzurufen, und legen Sie den Rückgabetyp auf die zuvor erstellte `SearchResult`-Struktur fest.

```csharp
//...
namespace BingSearchApisQuickstart
{
    //...
    class Program
    {
        //...
        static SearchResult BingImageSearch(string searchTerm)
        {
        }
//...
```

## <a name="create-and-handle-an-image-search-request"></a>Erstellen und Verarbeiten einer Bildersuchanforderung

Führen Sie in der `BingImageSearch`-Methode die folgenden Schritte aus:

1. Erstellen Sie den URI für die Suchanforderung. Formatieren Sie den Suchbegriff `SearchTerm`, bevor Sie ihn an die Zeichenfolge anhängen.

    ```csharp
    static SearchResult BingImageSearch(string SearchTerm){

        var uriQuery = uriBase + "?q=" + Uri.EscapeDataString(SearchTerm);
    //...
    ```

2. Senden Sie die Webanforderung, und rufen Sie die Antwort als JSON-Zeichenfolge ab.

    ```csharp
    WebRequest request = WebRequest.Create(uriQuery);
    request.Headers["Ocp-Apim-Subscription-Key"] = subscriptionKey;
    HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
    string json = new StreamReader(response.GetResponseStream()).ReadToEnd();
    ```

3. Erstellen Sie das Suchergebnisobjekt, und extrahieren Sie die Bing-HTTP-Header. Geben Sie dann `searchResult` zurück.

    ```csharp
    // Create the result object for return
    var searchResult = new SearchResult()
    {
        jsonResult = json,
        relevantHeaders = new Dictionary<String, String>()
    };

    // Extract Bing HTTP headers
    foreach (String header in response.Headers)
    {
        if (header.StartsWith("BingAPIs-") || header.StartsWith("X-MSEdge-"))
            searchResult.relevantHeaders[header] = response.Headers[header];
    }
    return searchResult;
    ```

## <a name="process-and-view-the-response"></a>Verarbeiten und Anzeigen der Antwort

1. Rufen Sie in der Main-Methode `BingImageSearch()` auf, und speichern Sie die zurückgegebene Antwort. Deserialisieren Sie dann den JSON-Code in ein Objekt.

    ```csharp
    SearchResult result = BingImageSearch(searchTerm);
    //deserialize the JSON response from the Bing Image Search API
    dynamic jsonObj = Newtonsoft.Json.JsonConvert.DeserializeObject(result.jsonResult);
    ```

2. Rufen Sie das erste von `jsonObj` zurückgegebene Bild ab, und drucken Sie den Titel und eine URL des Bilds aus.
    ```csharp
    var firstJsonObj = jsonObj["value"][0];
    Console.WriteLine("Title for the first image result: " + firstJsonObj["name"]+"\n");
    //After running the application, copy the output URL into a browser to see the image.
    Console.WriteLine("URL for the first image result: " + firstJsonObj["webSearchUrl"]+"\n");
    ```  


## <a name="example-json-response"></a>JSON-Beispielantwort

Antworten der Bing-Bildersuche-API werden im JSON-Format zurückgegeben. Diese Beispielantwort wurde gekürzt, damit nur ein Ergebnis angezeigt wird.

```json
{
"_type":"Images",
"instrumentation":{
    "_type":"ResponseInstrumentation"
},
"readLink":"images\/search?q=tropical ocean",
"webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=tropical ocean&FORM=OIIARP",
"totalEstimatedMatches":842,
"nextOffset":47,
"value":[
    {
        "webSearchUrl":"https:\/\/www.bing.com\/images\/search?view=detailv2&FORM=OIIRPO&q=tropical+ocean&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&simid=608027248313960152",
        "name":"My Life in the Ocean | The greatest WordPress.com site in ...",
        "thumbnailUrl":"https:\/\/tse3.mm.bing.net\/th?id=OIP.fmwSKKmKpmZtJiBDps1kLAHaEo&pid=Api",
        "datePublished":"2017-11-03T08:51:00.0000000Z",
        "contentUrl":"https:\/\/mylifeintheocean.files.wordpress.com\/2012\/11\/tropical-ocean-wallpaper-1920x12003.jpg",
        "hostPageUrl":"https:\/\/mylifeintheocean.wordpress.com\/",
        "contentSize":"897388 B",
        "encodingFormat":"jpeg",
        "hostPageDisplayUrl":"https:\/\/mylifeintheocean.wordpress.com",
        "width":1920,
        "height":1200,
        "thumbnail":{
        "width":474,
        "height":296
        },
        "imageInsightsToken":"ccid_fmwSKKmK*mid_8607ACDACB243BDEA7E1EF78127DA931E680E3A5*simid_608027248313960152*thid_OIP.fmwSKKmKpmZtJiBDps1kLAHaEo",
        "insightsMetadata":{
        "recipeSourcesCount":0,
        "bestRepresentativeQuery":{
            "text":"Tropical Beaches Desktop Wallpaper",
            "displayText":"Tropical Beaches Desktop Wallpaper",
            "webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=Tropical+Beaches+Desktop+Wallpaper&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&FORM=IDBQDM"
        },
        "pagesIncludingCount":115,
        "availableSizesCount":44
        },
        "imageId":"8607ACDACB243BDEA7E1EF78127DA931E680E3A5",
        "accentColor":"0050B2"
    }]
}
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Einseitige Web-App für die Bing-Bildersuche](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Weitere Informationen

* [Was ist die Bing-Bildersuche-API?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Interaktive Onlinedemo testen](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)
* [Details zu den Preisen für die Bing-Suche-APIs](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)
* [Dokumentation zu Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services)
* [Referenz zur Bing-Bildersuche-API](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)
