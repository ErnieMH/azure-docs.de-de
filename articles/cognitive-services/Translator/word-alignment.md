---
title: 'Wortausrichtung: Translator'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie zum Empfangen von Informationen zur Wortausrichtung die Translate-Methode, und binden Sie den optionalen Parameter includeAlignment ein.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 05/26/2020
ms.author: lajanuar
ms.openlocfilehash: 8368ca0ca4c3f2f0ab3cb88a5d54295d71451636
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "98898018"
---
# <a name="how-to-receive-word-alignment-information"></a>Empfangen von Informationen zur Wortausrichtung

## <a name="receiving-word-alignment-information"></a>Empfangen von Informationen zur Wortausrichtung
Verwenden Sie zum Empfangen von Informationen zur Wortausrichtung die Translate-Methode, und binden Sie den optionalen Parameter includeAlignment ein.

## <a name="alignment-information-format"></a>Format der Informationen zur Ausrichtung
Die Ausrichtung wird als Zeichenfolgenwert des folgenden Formats für jedes Wort der Quelle zurückgegeben. Die Informationen für jedes Wort sind durch ein Leerzeichen getrennt. Dies gilt auch für Sprachen (Skripts), die keine Leerzeichen als Trennung enthalten, z.B. Chinesisch:

[[SourceTextStartIndex]\:[SourceTextEndIndex]–[TgtTextStartIndex]\:[TgtTextEndIndex]] *

Beispiel für Ausrichtungszeichenfolge: „0:0-7:10 1:2-11:20 3:4-0:3 3:4-4:6 5:5-21:21“.

Anders ausgedrückt: Der Doppelpunkt trennt Start- und Endindex, der Bindestrich trennt die Sprachen, und Leerzeichen trennen die Wörter. Ein Wort kann mit Null, einem oder mehreren Wörtern in der anderen Sprache übereinstimmen, und die ausgerichteten Wörter sind ggf. nicht zusammenhängend. Wenn keine Informationen für die Ausrichtung verfügbar sind, ist das Ausrichtungselement leer. In diesem Fall gibt die Methode keinen Fehler zurück.

## <a name="restrictions"></a>Beschränkungen
Die Ausrichtung wird derzeit nur für eine Teilmenge der Sprachpaare zurückgegeben:
* aus dem Englischen in eine andere Sprache
* aus einer beliebigen anderen Sprache ins Englische mit Ausnahme von Chinesisch (vereinfacht) und Chinesisch (traditionell) sowie aus dem Lettischen ins Englische
* aus dem Japanischen ins Koreanische oder aus dem Koreanischen ins Japanische Sie erhalten keine Ausrichtungsinformationen, wenn es sich bei dem Satz um eine vordefinierte Übersetzung handelt. Beispiele für eine vordefinierte Übersetzung im Englischen sind „This is a test“, „I love you“ und andere häufig verwendete Sätze.

## <a name="example"></a>Beispiel

JSON-Beispiel

```json
[
  {
    "translations": [
      {
        "text": "Kann ich morgen Ihr Auto fahren?",
        "to": "de",
        "alignment": {
          "proj": "0:2-0:3 4:4-5:7 6:10-25:30 12:15-16:18 17:19-20:23 21:28-9:14 29:29-31:31"
        }
      }
    ]
  }
]
```
