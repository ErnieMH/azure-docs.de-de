---
title: Was ist eine BLEU-Bewertung? – Custom Translator
titleSuffix: Azure Cognitive Services
description: BLEU ist eine Messung der Unterschiede zwischen einer maschinellen Übersetzung und menschlichen Referenzübersetzungen desselben Ausgangssatzes.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 08/17/2020
ms.author: swmachan
ms.openlocfilehash: 61c1efb7337bee5fe329c9d548e23f3931e6ce3c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88510775"
---
# <a name="what-is-a-bleu-score"></a>Was ist eine BLEU-Bewertung?

[BLEU (Bilingual Evaluation Understudy)](https://en.wikipedia.org/wiki/BLEU) ist eine Messung der Unterschiede zwischen einer automatischen Übersetzung und mindestens einer menschlichen Referenzübersetzung desselben Ausgangssatzes.

## <a name="scoring-process"></a>Bewertungsprozess

Mit dem BLEU-Algorithmus werden aufeinander folgende Wortgruppen der automatischen Übersetzung mit den aufeinander folgenden Wortgruppen der Referenzübersetzung verglichen, und die Anzahl von Übereinstimmungen wird gezählt (mit Gewichtung). Diese Übereinstimmungen gelten unabhängig von der Position. Ein höherer Übereinstimmungsgrad weist auf eine höhere Ähnlichkeit mit der Referenzübersetzung hin und führt zu einem höheren Ergebnis. Die Verständlichkeit und die grammatische Korrektheit werden nicht berücksichtigt.

## <a name="how-bleu-works"></a>Wie funktioniert BLEU?

Die Stärke von BLEU besteht darin, dass diese Art der Bewertung gut mit der menschlichen Beurteilung korreliert. Es wird nicht auf einzelne Fehler bei der Satzbeurteilung geachtet, sondern der gesamte zu testende Korpus betrachtet, anstatt zu versuchen, eine präzise menschliche Beurteilung für jeden Satz zu erzielen.

Eine ausführlichere Beschreibung von BLEU-Bewertungen finden Sie [hier](https://youtu.be/-UqDljMymMg).

BLEU-Ergebnisse sind stark davon abhängig, welche Breite Ihre Domäne aufweist, wie konsistent die Testdaten mit den Trainings- und Optimierungsdaten sind und wie viele Daten für das Trainieren zur Verfügung stehen. Wenn Ihre Modelle anhand einer eng gefassten Domäne trainiert wurden und Ihre Trainingsdaten mit den Testdaten konsistent sind, können Sie eine hohe BLEU-Bewertung erwarten.

>[!NOTE]
>Ein Vergleich zwischen BLEU-Bewertungen ist nur sinnvoll, wenn die BLEU-Ergebnisse mit demselben Testdatensatz, dem gleichen Sprachpaar und derselben MT-Engine verglichen werden. Eine BLEU-Bewertung für einen anderen Testdatensatz fällt mit Sicherheit anders aus.
