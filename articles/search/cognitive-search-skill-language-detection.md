---
title: Der kognitive Skill „Spracherkennung“
titleSuffix: Azure Cognitive Search
description: Bewertet unstrukturierten Text und gibt für jeden Datensatz einen Sprachbezeichner mit einer Punktzahl zurück, die die Stärke der Analyse in einer KI-Anreicherungspipeline in der kognitiven Azure-Suche angibt.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/17/2020
ms.openlocfilehash: 087989638193bb59001ed33c4ee253d61682d8bf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88935992"
---
#   <a name="language-detection-cognitive-skill"></a>Der kognitive Skill „Spracherkennung“

Der Skill **Spracherkennung** erkennt die Sprache von Eingabetexten und meldet einen einzigen Sprachcode für jedes Dokument, das auf Anforderung gesendet wird. Der Sprachcode ist mit einem Wert kombiniert, der die Stärke der Analyse angibt. Bei dieser Qualifikation werden die Machine Learning-Modelle verwendet, die in Cognitive Services über die [Textanalyse](../cognitive-services/text-analytics/overview.md) bereitgestellt werden.

Diese Funktion ist besonders nützlich, wenn Sie die Sprache des Texts als Eingabe für andere Skills (z.B. den [Skill „Standpunktanalyse“](cognitive-search-skill-sentiment.md) oder den [Skill „Text teilen“](cognitive-search-skill-textsplit.md)) benötigen.

„Spracherkennung“ nutzt die Bibliotheken von Bing zur Verarbeitung natürlicher Sprache, wodurch die Anzahl von [unterstützten Sprachen und Regionen](../cognitive-services/text-analytics/language-support.md) überschritten wird, die für „Textanalyse“ aufgeführt sind. Die genaue Sprachenliste wird nicht veröffentlicht. Sie enthält aber alle verbreiteten Sprachen, plus Varianten, Dialekte sowie einige Regional- und Kultursprachen. Bei Inhalten in einer seltener verwendeten Sprache können Sie [die Sprachenerkennungs-API ausprobieren](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7), um zu sehen, ob sie einen Code zurückgibt. Die Antwort bei Sprachen, die nicht erkannt werden können, lautet `unknown`.

> [!NOTE]
> Wenn Sie den Umfang erweitern, indem Sie die Verarbeitungsfrequenz erhöhen oder weitere Dokumente oder KI-Algorithmen hinzufügen, müssen Sie [eine kostenpflichtige Cognitive Services-Ressource anfügen](cognitive-search-attach-cognitive-services.md). Gebühren fallen beim Aufrufen von APIs in Cognitive Services sowie für die Bildextraktion im Rahmen der Dokumententschlüsselungsphase in Azure Cognitive Search an. Für die Textextraktion aus Dokumenten fallen keine Gebühren an.
>
> Die Ausführung integrierter Qualifikationen wird nach dem bestehenden [nutzungsbasierten Preis für Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) berechnet. Die Preise für die Bildextraktion sind in der [Preisübersicht für Azure Cognitive Search](https://azure.microsoft.com/pricing/details/search/) angegeben.


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.LanguageDetectionSkill

## <a name="data-limits"></a>Datengrenzwerte
Die maximale Größe eines Datensatzes beträgt 50.000 Zeichen (gemessen durch [`String.Length`](/dotnet/api/system.string.length)). Wenn Sie Ihre Daten teilen müssen, bevor Sie sie an den Skill für Sprachenerkennung senden, können Sie den [Skill „Text teilen“](cognitive-search-skill-textsplit.md) verwenden.

## <a name="skill-inputs"></a>Skilleingaben

Bei den Parametern wird zwischen Groß- und Kleinschreibung unterschieden.

| Eingaben     | BESCHREIBUNG |
|--------------------|-------------|
| `text` | Der zu analysierende Text|

## <a name="skill-outputs"></a>Skillausgaben

| Ausgabename    | BESCHREIBUNG |
|--------------------|-------------|
| `languageCode` | Der für die Sprache identifizierte ISO-6391-Sprachcode. Beispiel: „en“. |
| `languageName` | Der Name der Sprache. Beispiel: „Englisch“. |
| `score` | Ein Wert zwischen 0 und 1 ein. Die Wahrscheinlichkeit, dass die Sprache korrekt identifiziert ist. Der Wert kann kleiner als 1 sein, wenn im Satz verschiedene Sprachen kombiniert sind.  |

##  <a name="sample-definition"></a>Beispieldefinition

```json
 {
    "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill",
    "inputs": [
      {
        "name": "text",
        "source": "/document/text"
      }
    ],
    "outputs": [
      {
        "name": "languageCode",
        "targetName": "myLanguageCode"
      },
      {
        "name": "languageName",
        "targetName": "myLanguageName"
      },
      {
        "name": "score",
        "targetName": "myLanguageScore"
      }

    ]
  }
```

##  <a name="sample-input"></a>Beispieleingabe

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "Glaciers are huge rivers of ice that ooze their way over land, powered by gravity and their own sheer weight. "
           }
      },
      {
        "recordId": "2",
        "data":
           {
             "text": "Estamos muy felices de estar con ustedes."
           }
      }
    ]
```


##  <a name="sample-output"></a>Beispielausgabe

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
            {
              "languageCode": "en",
              "languageName": "English",
              "score": 1,
            }
      },
      {
        "recordId": "2",
        "data":
            {
              "languageCode": "es",
              "languageName": "Spanish",
              "score": 1,
            }
      }
    ]
}
```


## <a name="error-cases"></a>Auftretende Fehler
Wenn der Text in einer nicht unterstützten Sprache ausgedrückt wird, tritt ein Fehler auf, und es wird keine Sprachen-ID zurückgegeben.

## <a name="see-also"></a>Weitere Informationen

+ [Integrierte Qualifikationen](cognitive-search-predefined-skills.md)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)