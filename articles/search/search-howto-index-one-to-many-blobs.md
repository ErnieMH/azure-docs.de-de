---
title: Indizieren von Blobs mit mehreren Dokumenten
titleSuffix: Azure Cognitive Search
description: Durchforsten Sie Azure-Blobs nach Textinhalten mithilfe des Blobindexers von Azure Cognitive Search, wobei jedes Blob ein einzelnes Suchindexdokument oder mehrere Suchindexdokumente ergeben kann.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 07/11/2020
ms.openlocfilehash: e5a69525c4bd0717c0561bc61ee3c52aa68e1c9d
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2020
ms.locfileid: "91533960"
---
# <a name="indexing-blobs-to-produce-multiple-search-documents"></a>Indizieren von Blobs zum Generieren mehrerer Suchdokumente
Standardmäßig behandelt ein Blobindexer die Inhalte eines Blobs als ein Suchdokument. Einige **parsingMode**-Werte unterstützen Szenarios, in denen sich aus einem einzigen Blob mehrere Suchdokumente ergeben können. Es gibt folgende unterschiedliche Typen von **parsingMode**, die es einem Indexer ermöglichen, mehr als ein Suchdokument aus einem Blob zu extrahieren:
+ `delimitedText`
+ `jsonArray`
+ `jsonLines`

## <a name="one-to-many-document-key"></a>1:n-Dokumentschlüssel
Jedes Dokument, das in einem Index der kognitiven Azure-Suche angezeigt wird, wird eindeutig durch einen Dokumentschlüssel identifiziert. 

Wenn kein Analysemodus angegeben wird und keine explizite Zuordnung für das Schlüsselfeld im Index vorhanden ist, wird in der kognitiven Azure-Suche automatisch die `metadata_storage_path`-Eigenschaft als Schlüssel [zugeordnet](search-indexer-field-mappings.md). Durch diese Zuordnung wird sichergestellt, dass jedes Blob als einzelnes Suchdokument angezeigt wird.

Wenn einer der oben aufgeführten Analysemodi verwendet wird, wird ein Blob mehreren Suchdokumenten zugeordnet, sodass ein Dokumentschlüssel, der nur auf Blobmetadaten basiert, ungeeignet ist. In der kognitiven Azure-Suche kann jedoch ein 1:n-Dokumentschlüssel für jede einzelne Entität generiert werden, die aus einem Blob extrahiert wird, um diese Einschränkung zu umgehen. Diese Eigenschaft mit dem Namen `AzureSearch_DocumentKey` wird jeder einzelnen Entität hinzugefügt, die aus dem Blob extrahiert wird. Der Wert dieser Eigenschaft ist für jede einzelne Entität _blobübergreifend_ garantiert eindeutig, und die Entitäten werden als separate Suchdokumente angezeigt.

Wenn keine expliziten Feldzuordnungen für das Schlüsselindexfeld angegeben werden, wird mit der Feldzuordnungsfunktion `base64Encode` standardmäßig `AzureSearch_DocumentKey` zugeordnet.

## <a name="example"></a>Beispiel
Angenommen, Ihre Indexdefinition weist folgende Felder auf:
+ `id`
+ `temperature`
+ `pressure`
+ `timestamp`

Und Ihr Blobcontainer enthält Blobs mit folgender Struktur:

_Blob1.json_

```json
    { "temperature": 100, "pressure": 100, "timestamp": "2019-02-13T00:00:00Z" }
    { "temperature" : 33, "pressure" : 30, "timestamp": "2019-02-14T00:00:00Z" }
```

_Blob2.json_

```json
    { "temperature": 1, "pressure": 1, "timestamp": "2018-01-12T00:00:00Z" }
    { "temperature" : 120, "pressure" : 3, "timestamp": "2013-05-11T00:00:00Z" }
```

Wenn Sie einen Indexer erstellen und **parsingMode** auf `jsonLines` festlegen, ohne explizite Feldzuordnungen für das Schlüsselfeld anzugeben, wird die folgende Zuordnung implizit angewendet

```http
    {
        "sourceFieldName" : "AzureSearch_DocumentKey",
        "targetFieldName": "id",
        "mappingFunction": { "name" : "base64Encode" }
    }
```

Mit diesem Setup enthält der Azure Cognitive Search-Index folgende Informationen (Base64-codierte ID aus Gründen der Übersichtlichkeit gekürzt).

| id | Temperatur | pressure | timestamp |
|----|-------------|----------|-----------|
| aHR0 ... YjEuanNvbjsx | 100 | 100 | 2019-02-13T00:00:00Z |
| aHR0 ... YjEuanNvbjsy | 33 | 30 | 2019-02-14T00:00:00Z |
| aHR0 ... YjIuanNvbjsx | 1 | 1 | 2018-01-12T00:00:00Z |
| aHR0 ... YjIuanNvbjsy | 120 | 3 | 2013-05-11T00:00:00Z |

## <a name="custom-field-mapping-for-index-key-field"></a>Benutzerdefinierte Feldzuordnung für das Indexschlüsselfeld

Angenommen, Ihr Blobcontainer enthält bei derselben Indexdefinition wie im vorherigen Beispiel Blobs mit folgender Struktur:

_Blob1.json_

```json
    recordid, temperature, pressure, timestamp
    1, 100, 100,"2019-02-13T00:00:00Z" 
    2, 33, 30,"2019-02-14T00:00:00Z" 
```

_Blob2.json_

```json
    recordid, temperature, pressure, timestamp
    1, 1, 1,"2018-01-12T00:00:00Z" 
    2, 120, 3,"2013-05-11T00:00:00Z" 
```

Wenn Sie einen Indexer mit **parsingMode** `delimitedText` erstellen, kommt es Ihnen möglicherweise natürlich vor, wie folgt eine Feldzuordnungsfunktion für die Schlüsselfelder einzurichten:

```http
    {
        "sourceFieldName" : "recordid",
        "targetFieldName": "id"
    }
```

Aus dieser Zuordnung ergeben sich jedoch _nicht_ 4 Dokumente, die im Index angezeigt werden, da das Feld `recordid` nicht _blobübergreifend_ eindeutig ist. Deshalb wird empfohlen, dass Sie die implizite Feldzuordnung verwenden, die von der Eigenschaft `AzureSearch_DocumentKey` auf das Schlüsselindexfeld für 1:n-Analysemodi angewendet wird.

Wenn Sie eine explizite Feldzuordnung einrichten möchten, sollten Sie sicherstellen, dass _sourceField_ für jede einzelne Entität **blobübergreifend** eindeutig ist.

> [!NOTE]
> Der von `AzureSearch_DocumentKey` verwendete Ansatz zur Sicherstellung der Eindeutigkeit jeder extrahierten Entität kann sich ändern, und Sie sollten sich deshalb für die Anforderungen Ihrer Anwendung nicht darauf verlassen.

## <a name="help-us-make-azure-cognitive-search-better"></a>Helfen Sie uns bei der Verbesserung der kognitiven Azure-Suche
Wenn Sie sich Features wünschen oder Verbesserungsvorschläge haben, stimmen Sie unter [UserVoice](https://feedback.azure.com/forums/263029-azure-search/) dafür ab. Wenn Sie Hilfe bei der Verwendung des vorhandenen Features benötigen, veröffentlichen Sie Ihre Frage in [Stack Overflow](https://stackoverflow.microsoft.com/questions/tagged/18870).

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie noch nicht mit der grundlegenden Struktur und dem Workflow der Blobindizierung vertraut sind, sollten Sie zunächst [Indizieren von Azure Blob Storage mit Azure Cognitive Search](search-howto-index-json-blobs.md) lesen. Weitere Informationen zum Analysemodus für die verschiedenen Blobinhaltstypen finden Sie in den folgenden Artikeln.

> [!div class="nextstepaction"]
> [Indizieren von CSV-Blobs](search-howto-index-csv-blobs.md)
> [Indizieren von JSON-Blobs](search-howto-index-json-blobs.md)
