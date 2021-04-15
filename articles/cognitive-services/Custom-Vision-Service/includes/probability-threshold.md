---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: include
ms.date: 07/17/2019
ms.author: pafarley
ms.openlocfilehash: 07e7cc991f127bf4bb4f466c0108962786e45bce
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "96018359"
---
Beachten Sie den Schieberegler für den **Wahrscheinlichkeitsschwellenwert** im linken Bereich der Registerkarte **Leistung**. Dies ist das Maß an Vertrauen, das eine Vorhersage aufweisen muss, um als richtig betrachtet zu werden (zur Berechnung von Genauigkeit und Trefferquote). 

Wenn Sie Vorhersageaufrufe mit einem hohen Wahrscheinlichkeitsschwellenwert interpretieren, neigen sie dazu, Ergebnisse mit hoher Genauigkeit auf Kosten der Trefferquote zu liefern. Die erkannten Klassifizierungen sind richtig, aber viele bleiben unerkannt. Ein niedriger Wahrscheinlichkeitsschwellenwert bewirkt das Gegenteil. Die meisten der tatsächlichen Klassifizierungen werden erkannt, aber es gibt mehr falsch positive Ergebnisse innerhalb dieser Menge. In diesem Sinne empfiehlt es sich, den Wahrscheinlichkeitsschwellenwert gemäß den spezifischen Anforderungen Ihres Projekts festzulegen. Wenn Sie später auf der Clientseite Vorhersageergebnisse erhalten, sollten Sie denselben Wahrscheinlichkeitsschwellenwert wie hier verwenden.