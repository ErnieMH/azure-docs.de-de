---
title: Häufig gestellte Fragen zur Textanalyse-API
titleSuffix: Azure Cognitive Services
description: In diesem Artikel finden Sie Antworten zu häufig gestellten Fragen zu Konzepten, Code und Szenarios der Textanalyse-API für Azure Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 03/29/2021
ms.author: aahi
ms.openlocfilehash: 324ff6f29aef76ef5baded89db92bb4e4b5ec362
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108755547"
---
# <a name="frequently-asked-questions-faq-about-the-text-analytics-api"></a>Häufig gestellte Fragen (FAQ) zur Textanalyse-API

 In diesem Artikel finden Sie Antworten zu häufig gestellten Fragen zu Konzepten, Code und Szenarios der Textanalyse-API für Azure Cognitive Services.

## <a name="what-is-the-maximum-size-and-number-of-requests-i-can-make-to-the-api"></a>Wie viele Anforderungen darf ich maximal an die API senden, und wie groß dürfen die Anforderungen maximal sein?

Informationen zur Größe und Anzahl von Anforderungen, die Sie pro Minute und Sekunde senden können, finden Sie im Artikel [Datengrenzwerte](concepts/data-limits.md).

## <a name="can-text-analytics-identify-sarcasm"></a>Kann die Textanalyse Sarkasmus erkennen?

Die Analyse dient der Einordnung in die Kategorien „positiv“ und „negativ“ und kann keine Stimmungslagen erkennen.

In der Stimmungsanalyse ist immer ein gewisses Maß an Ungenauigkeit vorhanden. Das Modell ist jedoch bestens geeignet, wenn es keine versteckte Bedeutungen oder unterschwelligen Botschaften im Inhalt gibt. Ironie, Sarkasmus, Humor und ähnlich komplexe Inhalte basieren auf kulturellem Kontext und Normen, um Absicht zu vermitteln. Diese Art von Inhalt gehört für die Analyse zu den anspruchsvollsten. In der Regel liegt der größte Unterschied zwischen dem Ergebnis durch das Analysetool und einer subjektiven Bewertung durch einen Menschen bei Inhalten mit komplexen Bedeutungen.

## <a name="can-i-add-my-own-training-data-or-models"></a>Kann ich meine eigenen Trainingsdaten oder Modelle hinzufügen?

Nein, die Modelle sind vortrainiert. Die einzigen Vorgänge zum Hochladen von Daten sind die Bewertung, Schlüsselbegriffserkennung und Spracherkennung. Benutzerdefinierte Modelle werden nicht gehostet. Wenn Sie benutzerdefinierte Machine Learning-Modelle erstellen und hosten möchten, informieren Sie sich über die [Machine Learning-Funktionen in Microsoft R Server](/r-server/r/concept-what-is-the-microsoftml-package).

## <a name="can-i-request-additional-languages"></a>Kann ich weitere Sprachen anfordern?

Die Stimmungsanalyse und Schlüsselbegriffserkennung sind für eine [bestimmte Auswahl von Sprachen](./language-support.md) verfügbar. Die Verarbeitung natürlicher Sprache ist komplex und erfordert umfangreiche Tests, bevor neue Funktionen veröffentlicht werden können. Aus diesem Grund wird die Unterstützung nicht frühzeitig angekündigt, sodass niemand sich auf eine Funktion verlässt, die mehr Entwicklungszeit benötigt. 

Sie können mit dem [Feedbacktool](https://feedback.azure.com/forums/932041-azure-cognitive-services?category_id=395749) für bestimmte Sprachen stimmen, damit die Entscheidung leichter fällt, welche Sprache als nächstes priorisiert wird. 

## <a name="why-does-key-phrase-extraction-return-some-words-but-not-others"></a>Wieso werden manche Worte von der Schlüsselbegriffserkennung zurückgegeben und andere nicht?

Bei der Schlüsselbegriffserkennung werden unbedeutende Worte und alleinstehende Adjektive entfernt. Kombinationen aus Adjektiven und Substantiven wie „spektakuläre Aussicht“ oder „nebliges Wetter“ werden zusammen zurückgegeben.

Im Allgemeinen besteht die Ausgabe aus den Substantiven und Objekten des Satzes. Die Ausgabe wird in der Reihenfolge der Wichtigkeit aufgeführt, wobei der erste Begriff der wichtigste ist. Die Wichtigkeit wird an der Häufigkeit, mit der ein bestimmtes Konzept auftritt, oder an der Beziehung des Elements zu anderen Elementen im Text gemessen.

## <a name="why-does-output-vary-given-identical-inputs"></a>Wieso variiert die Ausgabe bei identischen Eingaben?

Verbesserungen an Modellen und Algorithmen werden angekündigt, wenn es sich dabei um eine größere Änderung handelt. Bei kleinen Updates werden sie unangekündigt per Slipstream in den Dienst integriert. Im Laufe der Zeit fällt Ihnen möglicherweise auf, dass die gleiche Texteingabe in verschiedenen Stimmungswerten oder Schlüsselbegriffsausgaben resultiert. Das ist eine normale und beabsichtigte Folge der Verwendung verwalteter Machine Learning-Ressourcen in der Cloud.

## <a name="service-availability-and-redundancy"></a>Dienstverfügbarkeit und Redundanz

### <a name="is-text-analytics-service-zone-resilient"></a>Ist der Textanalyse-Dienst zonenresilient?

Ja. Der Textanalyse-Dienst ist standardmäßig zonenresilient.

### <a name="how-do-i-configure-the-text-analytics-service-to-be-zone-resilient"></a>Wie konfiguriere ich den Textanalyse-Dienst so, dass er zonenresilient ist?

Es ist keine Kundenkonfiguration erforderlich, um Zonenresilienz zu ermöglichen. Zonenresilienz für Textanalyse-Ressourcen ist standardmäßig verfügbar und wird vom Dienst selbst verwaltet.

## <a name="next-steps"></a>Nächste Schritte

Haben Sie eine Frage zu einem fehlenden Feature bzw. einer fehlenden Funktion? Erwägen Sie, sie mit dem [Feedbacktool](https://feedback.azure.com/forums/932041-azure-cognitive-services?category_id=395749) anzufordern oder dafür zu stimmen.

## <a name="see-also"></a>Siehe auch

 * [StackOverflow: Text Analytics API (StackOverflow: Textanalyse-API)](https://stackoverflow.com/questions/tagged/text-analytics-api)   
 * [StackOverflow: Cognitive Services](https://stackoverflow.com/questions/tagged/microsoft-cognitive)
