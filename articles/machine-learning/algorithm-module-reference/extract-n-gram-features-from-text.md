---
title: Referenz des Moduls „Extract N-Gram Features from Text“
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Extract N-Gram-Modul im Azure Machine Learning-Designer verwenden, um Textdaten in Merkmale zu zerlegen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 09/01/2019
ms.openlocfilehash: c4d9c7c2cb7a0a86824a373f1b64044b6dcd6c20
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2020
ms.locfileid: "93420800"
---
# <a name="extract-n-gram-features-from-text-module-reference"></a>Referenz des Moduls „Extract N-Gram Features from Text“

In diesem Artikel wird ein Modul im Azure Machine Learning-Designer beschrieben. Verwenden Sie das Modul „Extract N-Gram Features from Text“ (N-Gramm-Merkmale aus Text extrahieren), um unstrukturierte Textdaten *in Merkmale zu zerlegen*. 

## <a name="configuration-of-the-extract-n-gram-features-from-text-module"></a>Konfiguration des Moduls „Extract N-Gram Features from Text“

Das Modul unterstützt die folgenden Szenarien für die Verwendung eines N-Gramm-Wörterbuchs:

* [Erstellen eines neuen N-Gramm-Wörterbuchs](#create-a-new-n-gram-dictionary) aus einer Spalte mit freiem Text.

* [Verwenden einer vorhandenen Sammlung von Textmerkmalen](#use-an-existing-n-gram-dictionary), um eine Spalte mit freiem Text in Merkmale zu zerlegen.

* [Bewerten oder Veröffentlichen eines Modells](#score-or-publish-a-model-that-uses-n-grams), das N-Gramme verwendet.

### <a name="create-a-new-n-gram-dictionary"></a>Erstellen eines neuen N-Gramm-Wörterbuchs

1.  Fügen Sie das Modul Extract N-Gram Features from Text Ihrer Pipeline hinzu, und verbinden Sie das Dataset, das den zu verarbeitenden Text enthält.

1.  Verwenden Sie **Text column** (Textspalte), um eine Spalte des Typs „string“ auszuwählen, die den zu extrahierenden Text enthält. Weil die Ergebnisse ausführlich sind, können Sie immer nur jeweils eine Spalte verarbeiten.

1. Legen Sie **Vocabulary mode** (Vokabularmodus) auf **Create** (Erstellen) fest, um anzugeben, dass Sie eine neue Liste von N-Gramm-Merkmalen erstellen. 

1. Legen Sie **N-Grams size** (N-Gramm-Größe) fest, um die maximale *Größe* der zu extrahierenden und zu speichernden N-Gramme anzugeben. 

    Wenn Sie z.B. 3 eingeben, werden Unigramme, Bigramme und Trigramme erstellt.

1. **Weighting function** (Gewichtungsfunktion) gibt an, wie der Dokumentmerkmalsvektor erstellt und Vokabular aus Dokumenten extrahiert wird.

    * **Binary Weight** (Binäre Gewichtung): Weist den extrahierten N-Grammen einen binären Wert für das Vorhandensein zu. Der Wert für jedes N-Gramm ist gleich 1, wenn es im Dokument vorhanden ist, andernfalls ist der Wert gleich 0.

    * **TF Weight** (TF-Gewichtung): Weist den extrahierten N-Grammen eine Begriffshäufigkeit (Term Frequency, TF) zu. Der Wert für jedes N-Gramm ist dessen Vorkommenshäufigkeit im Dokument.

    * **IDF Weight** (IDF-Gewichtung): Weist den extrahierten N-Grammen eine Bewertung für die inverse Dokumenthäufigkeit (Inverse Document Frequency, IDF) zu. Der Wert für jedes N-Gramm ist das Protokoll der Korpusgröße geteilt durch die Häufigkeit seines Vorkommens im gesamten Korpus.
    
      `IDF = log of corpus_size / document_frequency`
 
    *  **TF-IDF Weight** (TF-IDF-Gewichtung): Weist den extrahierten N-Grammen eine Bewertung für die Begriffshäufigkeit/inverse Dokumenthäufigkeit (TF/IDF) zu. Der Wert für jedes N-Gramm ist die TF-Bewertung multipliziert mit seiner IDF-Bewertung.

1. Legen Sie **Minimum word length** (Minimale Wortlänge) auf die minimale Anzahl von Buchstaben fest, die in jedem *Einzelwort* in einem N-Gramm verwendet werden können.

1. Verwenden Sie **Maximum word length** (Maximale Wortlänge), um die maximale Anzahl von Buchstaben festzulegen, die in jedem *Einzelwort* in einem N-Gramm verwendet werden können.

    Standardmäßig sind bis zu 25 Zeichen pro Wort oder Token zulässig.

1. Verwenden Sie **Minimum n-gram document absolute frequency** (Minimale absolute N-Gramm-Dokumenthäufigkeit), um die minimalen Vorkommen festzulegen, die erforderlich sind, damit ein N-Gramm in das N-Gramm-Wörterbuch aufgenommen werden kann. 

    Wenn Sie beispielsweise den Standardwert 5 verwenden, muss jedes N-Gramm mindestens fünf Mal im Korpus vorkommen, um in das N-Gramm-Wörterbuch aufgenommen zu werden. 

1.  Legen Sie **Maximum n-gram document ratio** (Maximales N-Gramm-Dokumentverhältnis) auf das maximale Verhältnis der Anzahl der Zeilen fest, die ein bestimmtes N-Gramm im Vergleich zur Anzahl der Zeilen im Gesamtkorpus enthalten darf.

    Ein Verhältnis von 1 gibt beispielsweise an, dass das N-Gramm dem N-Gramm-Wörterbuch selbst dann hinzugefügt werden kann, wenn in jeder Zeile ein bestimmtes N-Gramm vorhanden ist. In der Regel wird ein Wort, das in jeder Zeile vorkommt, als Füllwort betrachtet und entfernt. Um fachgebietsabhängige Füllwörter auszufiltern, verringern Sie dieses Verhältnis.

    > [!IMPORTANT]
    > Die Häufigkeit des Vorkommens bestimmter Wörter ist nicht einheitlich. Sie variiert von Dokument zu Dokument. Wenn Sie beispielsweise Kundenkommentare zu einem bestimmten Produkt analysieren, kann der Produktname sehr häufig vorkommen und ähnlich einem Füllwort sein, aber in anderen Kontexten als wichtiger Begriff gelten.

1. Wählen Sie die Option **Normalize n-gram feature vectors** (N-Gramm-Merkmalsvektoren normalisieren) aus, um die Merkmalsvektoren zu normalisieren. Wenn diese Option aktiviert ist, wird jeder N-Gramm-Merkmalsvektor durch seine L2-Norm dividiert.

1. Übermitteln Sie die Pipeline.

### <a name="use-an-existing-n-gram-dictionary"></a>Verwenden eines vorhandenen N-Gramm-Wörterbuchs

1.  Fügen Sie das Modul Extract N-Gram Features from Text Ihrer Pipeline hinzu, und verbinden Sie das Dataset, das den zu verarbeitenden Text enthält, mit dem **Dataset**-Port.

1.  Verwenden **Text column** (Textspalte), um die Textspalte auszuwählen, die den Text enthält, den Sie in Merkmale zerlegen möchten. Standardmäßig wählt das Modul alle Spalten des Typs **string** aus. Um optimale Ergebnisse zu erzielen, verarbeiten Sie jeweils eine einzelne Spalte.

1. Fügen Sie das gespeicherte Dataset hinzu, das ein zuvor generiertes N-Gramm-Wörterbuch enthält, und verbinden Sie es mit dem **Input vocabulary**-Port (Eingabevokabular). Sie können auch die Ausgabe **Result vocabulary** (Ergebnisvokabular) einer Upstreaminstanz des Moduls „Extract N-Gram Features from Text“ (N-Gramm-Merkmale aus Text extrahieren) verbinden.

1. Wählen Sie unter **Vocabulary mode** (Vokabularmodus) in der Dropdownliste die Aktualisierungsoption **ReadOnly** (Schreibgeschützt) aus.

   Die Option **ReadOnly** stellt den Eingabekorpus für das Eingabevokabular dar. Anstatt die Häufigkeiten von Begriffen aus dem neuen Textdataset (für die linke Eingabe) zu berechnen, werden die N-Gramm-Gewichtungen aus dem Eingabevokabular unverändert übernommen.

   > [!TIP]
   > Verwenden Sie diese Option, wenn Sie einen Textklassifizierer bewerten.

1.  Informationen zu allen anderen Optionen finden Sie in den Eigenschaftenbeschreibungen im [vorherigen Abschnitt](#create-a-new-n-gram-dictionary).

1.  Übermitteln Sie die Pipeline.

### <a name="score-or-publish-a-model-that-uses-n-grams"></a>Bewerten oder Veröffentlichen eines Modells, das N-Gramme verwendet

1.  Kopieren Sie das Modul **Extract N-Gram Features from Text** aus dem Trainingsdatenfluss in den Bewertungsdatenfluss.

1.  Verbinden Sie die Ausgabe **Result Vocabulary** (Ergebnisvokabular) aus dem Trainingsdatenfluss mit dem **Input Vocabulary** (Eingabevokabular) für den Bewertungsdatenfluss.

1.  Ändern Sie im Bewertungsworkflow das Modul „Extract N-Gram Features from Text“, und legen Sie den **Vocabulary mode**-Parameter auf **ReadOnly** fest. Belassen Sie alle anderen Parameter ungeändert.

1.  Um die Pipeline zu veröffentlichen, speichern Sie das **Ergebnisvokabular** als Dataset.

1.  Verbinden Sie dann das gespeicherte Dataset mit dem Modul „Extract N-Gram Features from Text“ in Ihrem Bewertungsgraphen.

## <a name="results"></a>Ergebnisse

Das Modul „Extract N-Gram Features from Text“ erstellt zwei Arten von Ausgabe: 

* **Result Dataset** (Ergebnisdataset): Diese Ausgabe ist eine Zusammenfassung des analysierten Texts in Kombination mit den N-Grammen, die extrahiert wurden. Spalten, die Sie nicht in der Option **Text column** (Textspalte) ausgewählt haben, werden an die Ausgabe übergeben. Für jede Textspalte, die Sie analysieren, generiert das Modul diese Spalten:

  * **Matrix von N-Gramm-Vorkommen**: Das Modul generiert eine Spalte für jedes im Gesamtkorpus gefundene N-Gramm und fügt in jeder Spalte eine Bewertung hinzu, um die Gewichtung des N-Gramms für diese Zeile anzugeben. 

* **Ergebnisvokabular**: Das Vokabular enthält das tatsächliche N-Gramm-Wörterbuch zusammen mit den Bewertungen für die Häufigkeit von Begriffen, die als Teil der Analyse generiert werden. Sie können das Dataset für die Wiederverwendung mit einer anderen Sammlung von Eingaben oder für eine spätere Aktualisierung speichern. Sie können das Vokabular auch für Modellierung und Bewertung wiederverwenden.

### <a name="result-vocabulary"></a>Ergebnisvokabular

Das Vokabular enthält das N-Gramm-Wörterbuch mit den Bewertungen für die Begriffshäufigkeit, die als Teil der Analyse generiert werden. Die DF- und die IDF-Bewertung werden unabhängig von anderen Optionen generiert.

+ **ID:** Ein Bezeichner, der für jedes eindeutige N-Gramm generiert wird.
+ **NGram**: Das N-Gramm. Leerzeichen oder andere Worttrennzeichen werden durch Unterstriche ersetzt.
+ **DF**: Die Bewertung für die Begriffshäufigkeit für das N-Gramm im ursprünglichen Korpus.
+ **IDF**: Die Bewertung für die inverse Dokumenthäufigkeit für das N-Gramm im ursprünglichen Korpus.

Sie können dieses Dataset manuell aktualisieren. Es könnte aber sein, dass Sie dabei Fehler einführen. Beispiel:

* Wenn das Modul doppelt vorhandene Zeilen mit demselben Schlüssel im Eingabevokabular findet, wird ein Fehler ausgelöst. Stellen Sie sicher, dass keine zwei Zeilen im Vokabular dasselbe Wort aufweisen.
* Das Eingabeschema der Vokabulardatasets muss genau übereinstimmen, einschließlich der Spaltennamen und Spaltentypen. 
* Die Spalte **ID** und die Spalte **DF** müssen den Typ „integer“ haben. 
* Die Spalte **IDF** muss den Typ „float“ haben.

> [!Note]
> Verbinden Sie die Datenausgabe nicht direkt mit dem Modul „Train Model“ (Modell trainieren). Sie müssen freie Textspalten entfernen, bevor Sie in das Modul „Train Model“ eingespeist werden. Andernfalls werden die freien Textspalten als kategorische Features (Kategoriemerkmale) behandelt.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich den [Satz der verfügbaren Module](module-reference.md) für Azure Machine Learning an.
