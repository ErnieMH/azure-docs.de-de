---
author: brjohnstmsft
ms.service: cognitive-search
ms.topic: include
ms.date: 06/13/2018
ms.author: brjohnst
ms.openlocfilehash: 998d0f1a84dc9cb2a07fb55286c1089787a263e1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "80272957"
---
| Datentyp | Features, die in Lambdaausdrücken mit `any` zulässig sind | Features, die in Lambdaausdrücken mit `all` zulässig sind |
|---|---|---|
| `Collection(Edm.ComplexType)` | Alles mit Ausnahme von `search.ismatch` und `search.ismatchscoring` | identisch |
| `Collection(Edm.String)` | Vergleiche mit `eq` oder `search.in` <br/><br/> Kombinieren von Unterausdrücken mit `or` | Vergleiche mit `ne` oder `not search.in()` <br/><br/> Kombinieren von Unterausdrücken mit `and` |
| `Collection(Edm.Boolean)` | Vergleiche mit `eq` oder `ne` | identisch |
| `Collection(Edm.GeographyPoint)` | Verwenden von `geo.distance` mit `lt` oder `le` <br/><br/> `geo.intersects` <br/><br/> Kombinieren von Unterausdrücken mit `or` | Verwenden von `geo.distance` mit `gt` oder `ge` <br/><br/> `not geo.intersects(...)` <br/><br/> Kombinieren von Unterausdrücken mit `and` |
| `Collection(Edm.DateTimeOffset)`, `Collection(Edm.Double)`, `Collection(Edm.Int32)`, `Collection(Edm.Int64)` | Vergleiche mit `eq`, `ne`, `lt`, `gt`, `le` oder `ge` <br/><br/> Kombinieren von Vergleichen mit anderen Unterausdrücken über `or` <br/><br/> Kombinieren von Vergleichen, ausgenommen `ne`, mit anderen Unterausdrücken über `and` <br/><br/> Ausdrücke über Kombinationen von `and` und `or` in [disjunktiver Normalform (DNF)](https://en.wikipedia.org/wiki/Disjunctive_normal_form) | Vergleiche mit `eq`, `ne`, `lt`, `gt`, `le` oder `ge` <br/><br/> Kombinieren von Vergleichen mit anderen Unterausdrücken über `and` <br/><br/> Kombinieren von Vergleichen, ausgenommen `eq`, mit anderen Unterausdrücken über `or` <br/><br/> Ausdrücke über Kombinationen von `and` und `or` in [konjunktiver Normalform (KNF)](https://en.wikipedia.org/wiki/Conjunctive_normal_form) |
