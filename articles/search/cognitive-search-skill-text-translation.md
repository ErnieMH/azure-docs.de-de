---
title: Die Qualifikation „Textübersetzung“ der kognitiven Suche
titleSuffix: Azure Cognitive Search
description: Wertet Text aus und gibt für jeden Datensatz Text zurück, der in einer KI-Anreicherungspipeline in Azure Cognitive Search in die angegebene Zielsprache übersetzt wurde.
manager: nitinme
author: careyjmac
ms.author: chalton
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 0953d750ee8b59e9889512bb64cfd276a0bbeb53
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97654863"
---
#   <a name="text-translation-cognitive-skill"></a>Die Qualifikation „Textübersetzung“ der kognitiven Suche

Die Qualifikation **Textübersetzung** wertet Text aus und gibt für jeden Datensatz Text zurück, der in die angegebene Zielsprache übersetzt wurde. Diese Qualifikation verwendet die in Cognitive Services verfügbare [Textübersetzungs-API Version 3.0](../cognitive-services/translator/reference/v3-0-translate.md).

Diese Funktion ist nützlich, wenn Sie davon ausgehen, dass nicht alle Ihre Dokumente in einer einzigen Sprache vorliegen. In diesem Fall können Sie den Text vor der Indizierung für die Suche in eine einzelne Sprache normalisieren, indem Sie ihn übersetzen.  Sie ist auch für Lokalisierungsanwendungsfälle nützlich, in denen Sie möglicherweise Kopien desselben Texts in mehreren Sprachen wünschen.

Die [Textübersetzungs-API 3.0](../cognitive-services/translator/reference/v3-0-reference.md) ist ein nicht regionaler Cognitive Service, was bedeutet, dass Ihre Daten nicht garantiert in der Region bleiben, in der sich Ihre Azure Cognitive Search-Instanz oder Ihre angefügte Cognitive Services-Ressource befindet.

> [!NOTE]
> Wenn Sie den Umfang erweitern, indem Sie die Verarbeitungsfrequenz erhöhen oder weitere Dokumente oder KI-Algorithmen hinzufügen, müssen Sie [eine kostenpflichtige Cognitive Services-Ressource anfügen](cognitive-search-attach-cognitive-services.md). Gebühren fallen beim Aufrufen von APIs in Cognitive Services sowie für die Bildextraktion im Rahmen der Dokumententschlüsselungsphase in Azure Cognitive Search an. Für die Textextraktion aus Dokumenten fallen keine Gebühren an.
>
> Die Ausführung integrierter Qualifikationen wird nach dem bestehenden [nutzungsbasierten Preis für Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) berechnet. Die Preise für die Bildextraktion sind in der [Preisübersicht für Azure Cognitive Search](https://azure.microsoft.com/pricing/details/search/) angegeben.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.TranslationSkill

## <a name="data-limits"></a>Datengrenzwerte
Die maximale Größe eines Datensatzes beträgt 50.000 Zeichen (gemessen durch [`String.Length`](/dotnet/api/system.string.length)). Wenn Sie Ihre Daten teilen müssen, bevor Sie sie an die Übersetzungsqualifikation senden, denken Sie daran, die [Qualifikation „Text teilen“](cognitive-search-skill-textsplit.md) zu verwenden.

## <a name="skill-parameters"></a>Skillparameter

Bei den Parametern wird zwischen Groß- und Kleinschreibung unterschieden.

| Eingaben | BESCHREIBUNG |
|---------------------|-------------|
| defaultToLanguageCode | (Erforderlich) Der Sprachcode, in den Dokumente übersetzt werden für Dokumente, in denen die Zielprache nicht explizit angegeben ist. <br/> Siehe die [vollständige Liste der unterstützten Sprachen](../cognitive-services/translator/language-support.md). |
| defaultFromLanguageCode | (Optional) Der Sprachcode, aus dem Dokumente übersetzt werden für Dokumente, in denen die Ausgangssprache nicht explizit angegeben ist.  Wenn defaultFromLanguageCode nicht angegeben wird, wird die von der Textübersetzungs-API bereitgestellte automatische Spracherkennung verwendet, um die Ausgangssprache zu bestimmen. <br/> Siehe die [vollständige Liste der unterstützten Sprachen](../cognitive-services/translator/language-support.md). |
| suggestedFrom | (Optional) Der Sprachcode, aus dem Dokumente übersetzt werden, wenn weder die fromLanguageCode-Eingabe noch der defaultFromLanguageCode-Parameter bereitgestellt wird, und die automatische Spracherkennung nicht erfolgreich ist.  Wenn die suggestedFrom-Sprache nicht festgelegt wird, wird Englisch (en) als suggestedFrom-Sprache verwendet. <br/> Siehe die [vollständige Liste der unterstützten Sprachen](../cognitive-services/translator/language-support.md). |

## <a name="skill-inputs"></a>Skilleingaben

| Eingabename     | BESCHREIBUNG |
|--------------------|-------------|
| text | Der zu übersetzende Text.|
| toLanguageCode    | Eine Zeichenfolge, die die Sprache angibt, in die der Text übersetzt werden sollte. Wenn diese Eingabe nicht angegeben wird, wird der Text mit defaultToLanguageCode übersetzt. <br/>Siehe die [vollständige Liste der unterstützten Sprachen](../cognitive-services/translator/language-support.md).|
| fromLanguageCode  | Eine Zeichenfolge, die die aktuelle Sprache des Texts angibt. Wenn dieser Parameter nicht angegeben wird, wird defaultFromLanguageCode (oder die automatische Spracherkennung, wenn defaultFromLanguageCode nicht angegeben wird) verwendet, um den Text zu übersetzen. <br/>Siehe die [vollständige Liste der unterstützten Sprachen](../cognitive-services/translator/language-support.md).|

## <a name="skill-outputs"></a>Skillausgaben

| Ausgabename    | BESCHREIBUNG |
|--------------------|-------------|
| translatedText | Das Zeichenfolgenergebnis der Textübersetzung von translatedFromLanguageCode in translatedToLanguageCode.|
| translatedToLanguageCode  | Eine Zeichenfolge, die den Sprachcode angibt, in den der Text übersetzt wurde. Nützlich, wenn Sie in mehrere Sprachen übersetzen und verfolgen möchten, welcher Text in welcher Sprache vorliegt.|
| translatedFromLanguageCode    | Eine Zeichenfolge, die den Sprachcode angibt, aus dem der Text übersetzt wurde. Nützlich, wenn Sie sich für die Option zur automatischen Spracherkennung entschieden haben, da diese Ausgabe das Ergebnis dieser Erkennung liefert.|

##  <a name="sample-definition"></a>Beispieldefinition

```json
 {
    "@odata.type": "#Microsoft.Skills.Text.TranslationSkill",
    "defaultToLanguageCode": "fr",
    "suggestedFrom": "en",
    "context": "/document",
    "inputs": [
      {
        "name": "text",
        "source": "/document/text"
      }
    ],
    "outputs": [
      {
        "name": "translatedText",
        "targetName": "translatedText"
      },
      {
        "name": "translatedFromLanguageCode",
        "targetName": "translatedFromLanguageCode"
      },
      {
        "name": "translatedToLanguageCode",
        "targetName": "translatedToLanguageCode"
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
          "text": "We hold these truths to be self-evident, that all men are created equal."
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
}
```


##  <a name="sample-output"></a>Beispielausgabe

```json
{
  "values": [
    {
      "recordId": "1",
      "data":
        {
          "translatedText": "Nous tenons ces vérités pour évidentes, que tous les hommes sont créés égaux.",
          "translatedFromLanguageCode": "en",
          "translatedToLanguageCode": "fr"
        }
    },
    {
      "recordId": "2",
      "data":
        {
          "translatedText": "Nous sommes très heureux d'être avec vous.",
          "translatedFromLanguageCode": "es",
          "translatedToLanguageCode": "fr"
        }
    }
  ]
}
```


## <a name="errors-and-warnings"></a>Fehler und Warnungen
Wenn Sie einen nicht unterstützten Sprachcode für die Ausgangs- oder Zielsprache angeben, wird ein Fehler generiert, und der Text wird nicht übersetzt.
Wenn Ihr Text leer ist, wird eine Warnung angezeigt.
Wenn Ihr Text mehr als 50.000 Zeichen umfasst, werden nur die ersten 50.000 Zeichen übersetzt und eine Warnung ausgegeben.

## <a name="see-also"></a>Weitere Informationen

+ [Integrierte Qualifikationen](cognitive-search-predefined-skills.md)
+ [Definieren eines Skillsets](cognitive-search-defining-skillset.md)