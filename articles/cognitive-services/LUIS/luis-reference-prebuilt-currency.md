---
title: 'Vordefinierte Währungsentität: LUIS'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erhalten Sie Informationen zur vordefinierten Entität „currency“ in LUIS (Language Understanding Intelligent Service).
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/14/2019
ms.openlocfilehash: c89de0ba74d04c510f3d5ba537f3a6dcc4819169
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2020
ms.locfileid: "91534335"
---
# <a name="currency-prebuilt-entity-for-a-luis-app"></a>Vordefinierte Währungsentität für eine LUIS-App
Die vordefinierte currency-Entität erkennt Währungen in vielen Bezeichnungen und Ländern/Regionen, unabhängig von der LUIS-App-Kultur. Da diese Entität bereits trainiert wurde, müssen Sie den Anwendungsabsichten keine Beispieläußerungen mit Währungen hinzufügen. Die currency-Entität wird in [vielen Kulturen](luis-reference-prebuilt-entities.md) unterstützt.

## <a name="types-of-currency"></a>Arten von Währung
Die Währung wird über das GitHub-Repository [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L26) verwaltet.

## <a name="resolution-for-currency-entity"></a>Auflösung der currency-Entität

#### <a name="v3-response"></a>[V3-Antwort](#tab/V3)

Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `false` festgelegt:

```json
"entities": {
    "money": [
        {
            "number": 10.99,
            "units": "Dollar"
        }
    ]
}
```
#### <a name="v3-verbose-response"></a>[Ausführliche V3-Antwort](#tab/V3-verbose)
Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `true` festgelegt:

```json
"entities": {
    "money": [
        {
            "number": 10.99,
            "unit": "Dollar"
        }
    ],
    "$instance": {
        "money": [
            {
                "type": "builtin.currency",
                "text": "$10.99",
                "startIndex": 23,
                "length": 6,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor"
            }
        ]
    }
}
```

#### <a name="v2-response"></a>[V2-Antwort](#tab/V2)

Im folgenden Beispiel wird die Auflösung der Entität **builtin.currency** veranschaulicht.

```json
"entities": [
    {
        "entity": "$10.99",
        "type": "builtin.currency",
        "startIndex": 23,
        "endIndex": 28,
        "resolution": {
        "unit": "Dollar",
        "value": "10.99"
        }
    }
]
```
* * *

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den [V3-Vorhersageendpunkt](luis-migration-api-v3.md).

Erfahren Sie mehr über die Entitäten [datetimeV2](luis-reference-prebuilt-datetimev2.md), [dimension](luis-reference-prebuilt-dimension.md) und [email](luis-reference-prebuilt-email.md).
