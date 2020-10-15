---
title: Durchblättern von Suchergebnissen – Bing-Suche-APIs
titleSuffix: Azure Cognitive Services
description: Hier erfahren Sie, wie Sie Suchergebnisse der Bing-Suche-APIs durchblättern.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: 26CA595B-0866-43E8-93A2-F2B5E09D1F3B
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: aahi
ms.openlocfilehash: ea883bb294a8769b3c9be1e0eafc2e3e7c811b48
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "73481738"
---
# <a name="how-to-page-through-results-from-the-bing-search-apis"></a>Durchblättern der Suchergebnisse der Bing-Suche-APIs

Wenn Sie einen Aufruf an die Bing-Suche-APIs Web, Custom, Image, News oder Video senden, gibt Bing eine Teilmenge der Gesamtzahl der Ergebnisse zurück, die für die Abfrage relevant sein können. Um die geschätzte Gesamtzahl der verfügbaren Ergebnisse abzurufen, greifen Sie auf das `totalEstimatedMatches`-Feld des Antwortobjekts zu. 

Beispiel: 

```json
{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43CA...",
        "totalEstimatedMatches" : 262000,
        "value" : [...]
    }
}  
```

## <a name="paging-through-search-results"></a>Durchblättern von Suchergebnissen

Um durch die verfügbaren Ergebnisse zu blättern, verwenden Sie beim Senden Ihrer Abfrage die Abfrageparameter`count` und `offset`.  

> [!NOTE]
>
> * Das Durchblättern beschränkt sich bei den Bing-APIs Video, Image und News nur auf allgemeine Video- (`/video/search`), Nachrichten- (`/news/search`) und Bildsuchen (`/image/search`). Das Durchblättern von Trendthemen und Kategorien wird nicht unterstützt.  
> * Das Feld `TotalEstimatedMatches` gibt eine Schätzung der Gesamtzahl der Suchergebnisse für die aktuelle Abfrage an. Wenn Sie die Parameter `count` und `offset` festlegen, ändert sich diese Schätzung möglicherweise.

| Parameter | BESCHREIBUNG                                                                                                                                                                |
|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `count`   | Gibt die Anzahl der Ergebnisse an, die in der Antwort zurückgegeben werden sollen. Beachten Sie, dass sich der Standardwert von `count` und die maximale Zahl der Ergebnisse, die abgerufen werden können, bei den einzelnen APIs unterscheiden. Sie finden diese Werte in der Referenzdokumentation unter [Nächste Schritte](#next-steps). |
| `offset`  | Gibt die Anzahl der zu überspringenden Ergebnisse an. Der `offset`-Parameter ist nullbasiert und sollte kleiner sein als (`totalEstimatedMatches` - `count`).                                           |

Wenn Sie pro Seite z.B. 15 Artikel anzeigen möchten, legen Sie `count` auf 15 und `offset` auf 0 fest, um die erste Ergebnisseite abzurufen. Für jeden nachfolgenden API-Aufruf würden Sie `offset` um 15 heraufsetzen. Im Beispiel unten werden 15 Webseiten ab Offset 45 angefordert.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&count=15&offset=45&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```

Wenn Sie den Standardwert von `count` verwenden, müssen Sie in Ihren API-Aufrufen nur den Abfrageparameter `offset` angeben.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&offset=45&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```

Wenn Sie die Bing-Bildersuche- und Bing-Videosuche-APIs verwenden, können Sie mit dem Wert `nextOffset` doppelte Suchergebnisse vermeiden. Rufen Sie den Wert aus den Antwortobjekten `Images` oder `Videos` ab, und verwenden Sie ihn in Ihren Anforderungen mit dem `offset`-Parameter.  

> [!NOTE]
> Die Bing-Websuche-API gibt Suchergebnisse zurück, die Webseiten, Bilder, Videos und Nachrichten enthalten können. Wenn Sie durch die Suchergebnisse aus dem Bing-Websuche-API blättern, werden nur [Webseiten](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#webpage) angezeigt, keine anderen Antworttypen, wie etwa Bilder oder Nachrichten. Suchergebnisse in `WebPage`-Objekten enthalten möglicherweise Ergebnisse, die auch in anderen Antworttypen angezeigt werden.
>
> Wenn Sie den `responseFilter`-Abfrageparameter ohne Angabe von Filterwerten verwenden, verwenden Sie die Parameter `count` und `offset` nicht. 

## <a name="next-steps"></a>Nächste Schritte

* [Was sind die Bing-Websuche-APIs?](bing-api-comparison.md)
* [Referenz zur Bing-Websuche-API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference)
* [Bing-API v7 für die benutzerdefinierte Suche – Referenz](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference)
* [Bing-API v7 für die Nachrichtensuche – Referenz](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference)
* [Bing-API v7 für die Videosuche – Referenz](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference)
* [Bing-API v7 für die Bildsuche – Referenz](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)
