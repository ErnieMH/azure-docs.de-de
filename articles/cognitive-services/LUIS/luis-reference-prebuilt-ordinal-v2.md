---
title: 'Vorgefertigte Ordinal-V2-Entität: LUIS'
titleSuffix: Azure Cognitive Services
description: Dieser Artikel enthält ordinale V2 vorgefertigte Entitätsinformationen in Language Understanding (LUIS).
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/27/2019
ms.openlocfilehash: 3fd80389dfa54cb8386c13e0e05a1e71ac4bd956
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2020
ms.locfileid: "91541933"
---
# <a name="ordinal-v2-prebuilt-entity-for-a-luis-app"></a>Ordinale V2 vorgefertigte Entität für eine LUIS-Applikation
Ordinale V2-Anzahl erweitert [Ordnungszahl](luis-reference-prebuilt-ordinal.md), um relative Verweise bereitzustellen, z. B. `next`, `last` und `previous`. Diese werden nicht mithilfe der ordinalen vorgefertigten Entität extrahiert.

## <a name="resolution-for-prebuilt-ordinal-v2-entity"></a>Auflösung für vorgefertigte ordinale V2-Entität

Die folgenden Entitätsobjekte werden für die Abfrage zurückgegeben:

`what is the second to last choice in the list`

#### <a name="v3-response"></a>[V3-Antwort](#tab/V3)

Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `false` festgelegt:

```json
"entities": {
    "ordinalV2": [
        {
            "offset": -1,
            "relativeTo": "end"
        }
    ]
}
```

#### <a name="v3-verbose-response"></a>[Ausführliche V3-Antwort](#tab/V3-verbose)

Beim folgenden JSON-Code wurde der `verbose`-Parameter auf `true` festgelegt:

```json
"entities": {
    "ordinalV2": [
        {
            "offset": -1,
            "relativeTo": "end"
        }
    ],
    "$instance": {
        "ordinalV2": [
            {
                "type": "builtin.ordinalV2.relative",
                "text": "the second to last",
                "startIndex": 8,
                "length": 18,
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

Das folgende Beispiel zeigt die Auflösung der **builtin.ordinalV2**-Entität.

```json
"entities": [
    {
        "entity": "the second to last",
        "type": "builtin.ordinalV2.relative",
        "startIndex": 8,
        "endIndex": 25,
        "resolution": {
            "offset": "-1",
            "relativeTo": "end"
        }
    }
]
```
* * *

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über den [V3-Vorhersageendpunkt](luis-migration-api-v3.md).

Erfahren Sie mehr zu den [Prozentsatz](luis-reference-prebuilt-percentage.md)-, [Telefonnummer](luis-reference-prebuilt-phonenumber.md)- und [Temperaturentitäten](luis-reference-prebuilt-temperature.md).
