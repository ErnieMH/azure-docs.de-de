---
title: Vordefinierte Dimension-Entitäten – LUIS
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erhalten Sie Informationen zur vordefinierten Entität „dimension“ in LUIS (Language Understanding Intelligent Service).
services: cognitive-services
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/14/2019
ms.openlocfilehash: 0b5f081dc8c0bc203ccafde4a513371d11e36879
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "91538346"
---
# <a name="dimension-prebuilt-entity-for-a-luis-app"></a>Vordefinierte Dimension-Entität für eine LUIS-App
Die vordefinierte dimension-Entität erkennt verschiedene Arten von Maßen, unabhängig von der LUIS-App-Kultur. Da diese Entität bereits trainiert wurde, müssen Sie den Anwendungsabsichten keine Beispieläußerungen mit Maßen hinzufügen. Die dimension-Entität wird in [vielen Kulturen](luis-reference-prebuilt-entities.md) unterstützt.

## <a name="types-of-dimension"></a>dimension-Typen

Die Entität „dimension“ wird über das GitHub-Repository [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml) verwaltet.

## <a name="resolution-for-dimension-entity"></a>Auflösung der dimension-Entität

Die folgenden Entitätsobjekte werden für die Abfrage zurückgegeben:

`10 1/2 miles of cable`

#### <a name="v3-response"></a>[V3-Antwort](#tab/V3)

Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `false` festgelegt:

```json
"entities": {
    "dimension": [
        {
            "number": 10.5,
            "units": "Mile"
        }
    ]
}
```
#### <a name="v3-verbose-response"></a>[Ausführliche V3-Antwort](#tab/V3-verbose)
Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `true` festgelegt:

```json
"entities": {
    "dimension": [
        {
            "number": 10.5,
            "units": "Mile"
        }
    ],
    "$instance": {
        "dimension": [
            {
                "type": "builtin.dimension",
                "text": "10 1/2 miles",
                "startIndex": 0,
                "length": 12,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

#### <a name="v2-response"></a>[V2-Antwort](#tab/V2)

Im folgenden Beispiel wird die Auflösung der Entität **builtin.dimension** veranschaulicht.

```json
{
    "entity": "10 1/2 miles",
    "type": "builtin.dimension",
    "startIndex": 0,
    "endIndex": 11,
    "resolution": {
    "unit": "Mile",
    "value": "10.5"
    }
}
```
* * *

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den [V3-Vorhersageendpunkt](luis-migration-api-v3.md).

Erfahren Sie mehr über die Entitäten [email](luis-reference-prebuilt-email.md), [number](luis-reference-prebuilt-number.md) und [ordinal](luis-reference-prebuilt-ordinal.md).
