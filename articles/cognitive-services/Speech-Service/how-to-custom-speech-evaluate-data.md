---
title: Bewerten der Genauigkeit für Custom Speech – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: In diesem Dokument erfahren Sie, wie Sie die Qualität unseres Spracherkennungsmodells oder Ihres benutzerdefinierten Modells quantitativ messen können. Zum Testen der Genauigkeit sind Audio- und menschenmarkierte Transkriptionsdaten erforderlich, und 30 Minuten bis 5 Stunden an repräsentativem Audio müssen bereitgestellt werden.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: erhopf
ms.openlocfilehash: cadbe79bbe0af2b5cebacb3d0c7c4e910fc7dbb8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85856832"
---
# <a name="evaluate-custom-speech-accuracy"></a>Bewerten der Custom Speech-Genauigkeit

In diesem Dokument erfahren Sie, wie Sie die Qualität des Spracherkennungsmodells von Microsoft oder Ihres benutzerdefinierten Modells quantitativ messen können. Zum Testen der Genauigkeit sind Audio- und menschenmarkierte Transkriptionsdaten erforderlich, und 30 Minuten bis 5 Stunden an repräsentativem Audio müssen bereitgestellt werden.

## <a name="what-is-word-error-rate-wer"></a>Was ist die Wort-Fehler-Rate (Word Error Rate, WER)?

Der Branchenstandard zur Messung der Modellgenauigkeit ist die *Wort-Fehler-Rate* (WER). Zur Berechnung der WER wird die Anzahl von bei der Erkennung falsch identifizierten Wörtern ermittelt und durch die Gesamtanzahl von Wörtern im menschenmarkierten Transkript (unten als „N“ zu sehen) dividiert. Diese Zahl wird dann mit 100 % multipliziert, um die WER zu berechnen.

![WER-Formel](./media/custom-speech/custom-speech-wer-formula.png)

Falsch identifizierte Wörter fallen in drei Kategorien:

* Einfügung (I): Wörter, die fälschlicherweise im Hypothesentranskript hinzugefügt werden
* Löschung (D): Wörter, die im Hypothesentranskript nicht erkannt werden
* Ersetzung (S): Wörter, die zwischen Verweis und Hypothese ersetzt wurden

Hier sehen Sie ein Beispiel:

![Beispiel für falsch identifizierte Wörter](./media/custom-speech/custom-speech-dis-words.png)

## <a name="resolve-errors-and-improve-wer"></a>Beheben von Fehlern und Verbessern der WER

Sie können die WER aus den Ergebnissen der maschinellen Erkennung verwenden, um die Qualität des mit Ihrer App, Ihrem Tool oder Ihrem Produkt verwendeten Modells zu bewerten. Ein WER von 5 %–10 % gilt als gute Qualität und ist einsatzbereit. Eine WER von 20 % ist akzeptabel, Sie sollten aber weiteres Training in Betracht ziehen. Eine WER von 30 % oder mehr weist auf schlechte Qualität hin und erfordert Anpassung und Training.

Die Verteilung der Fehler spielt eine wichtige Rolle. Wenn viele Löschfehler auftreten, liegt es in der Regel an einem schwachen Audiosignal. Um dieses Problem zu beheben, müssen Sie Audiodaten näher an der Quelle erfassen. Einfügefehler bedeuten, dass der Ton in einer lauten Umgebung aufgenommen wurde und Übersprechen vorhanden sein kann, was zu Erkennungsproblemen führt. Ersetzungsfehler treten häufig auf, wenn eine unzureichende Stichprobe domänenspezifischer Begriffe entweder als menschenmarkierte Transkriptionen oder zugehöriger Text bereitgestellt wurde.

Durch die Analyse einzelner Dateien können Sie feststellen, welche Art von Fehlern vorliegen und welche Fehler nur in einer bestimmten Datei auftreten. Das Verständnis von Problemen auf Dateiebene hilft Ihnen bei gezielten Verbesserungen.

## <a name="create-a-test"></a>Erstellen eines Tests

Wenn Sie die Qualität des Basismodells für Spracherkennung von Microsoft oder eines von Ihnen trainierten benutzerdefinierten Modells testen möchten, können Sie zwei Modelle nebeneinander vergleichen, um die Genauigkeit zu bewerten. Der Vergleich umfasst WER und Erkennungsergebnisse. In der Regel wird ein benutzerdefiniertes Modell mit dem Basismodell von Microsoft verglichen.

Parallele Auswertung von Modellen:

1. Melden Sie sich beim [Custom Speech-Portal](https://speech.microsoft.com/customspeech) an.
2. Navigieren Sie zu **Spracherkennung > Custom Speech > [Projektname] > Testen**.
3. Klicken Sie auf **Test hinzufügen**.
4. Wählen Sie **Bewerten der Genauigkeit** aus. Geben Sie einen Namen und eine Beschreibung für den Test ein, und wählen Sie Ihr Audio- und menschenmarkiertes Transkriptionsdataset aus.
5. Wählen Sie bis zu zwei Modelle aus, die Sie testen möchten.
6. Klicken Sie auf **Erstellen**.

Nachdem der Test erfolgreich erstellt wurde, können Sie die Ergebnisse nebeneinander vergleichen.

## <a name="side-by-side-comparison"></a>Gegenüberstellung

Sobald der Test abgeschlossen ist, was durch den Statuswechsel zu *Erfolgreich* angezeigt wird, erhalten Sie eine WER-Nummer für beide Modelle in Ihrem Test. Klicken Sie auf den Testnamen, um die Seite mit den Testdetails anzuzeigen. Diese Detailseite listet alle Äußerungen in Ihrem Dataset auf und zeigt die Erkennungsergebnisse der beiden Modelle neben der Transkription aus dem übermittelten Dataset an. Zur einfacheren Untersuchung der Gegenüberstellung können Sie verschiedene Fehlertypen wie Einfügungen, Löschungen und Ersetzungen aktivieren oder deaktivieren. Indem Sie sich die Audiodaten anhören und mit den Erkennungsergebnissen in den einzelnen Spalten vergleichen, die die menschenmarkierte Transkription sowie die Ergebnisse für zwei Spracherkennungsmodelle enthalten, können Sie entscheiden, welches Modell Ihre Anforderungen erfüllt und wo weiteres Training und Verbesserungen erforderlich sind.

## <a name="next-steps"></a>Nächste Schritte

* [Trainieren Ihres Modells](how-to-custom-speech-train-model.md)
* [Bereitstellen Ihres Modells](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Vorbereiten und Testen Ihrer Daten](how-to-custom-speech-test-data.md)
* [Überprüfen Ihrer Daten](how-to-custom-speech-inspect-data.md)
