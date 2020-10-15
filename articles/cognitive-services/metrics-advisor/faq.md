---
title: 'Metrics Advisor: Häufig gestellte Fragen'
titleSuffix: Azure Cognitive Services
description: Häufig gestellte Fragen zum Metrics Advisor-Dienst
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: conceptual
ms.date: 09/30/2020
ms.author: mbullwin
ms.openlocfilehash: 42b23876761afa213b07f07b3a61e125dcf0824b
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92046807"
---
# <a name="metrics-advisor-frequently-asked-questions"></a>Metrics Advisor: Häufig gestellte Fragen

### <a name="what-is-the-cost-of-my-instance"></a>Welche Kosten fallen für meine Instanz an?

Für die Verwendung Ihrer Instanz in der Vorschau fallen derzeit keine Kosten an.

### <a name="why-is-the-demo-website-readonly"></a>Weshalb ist die Demowebsite schreibgeschützt?

Die [Demowebsite](https://anomaly-detector.azurewebsites.net/) ist öffentlich verfügbar. Diese Instanz ist schreibgeschützt, um das versehentliche Hochladen von Daten zu verhindern.

### <a name="why-cant-i-create-the-resource-the-pricing-tier-is-unavailable-and-it-says-you-have-already-created-1-s0-for-this-subscription"></a>Warum kann ich die Ressource nicht erstellen? Der Tarif ist nicht verfügbar, und „You have already created 1 S0 for this subscription“ (Sie haben bereits 1 S0 für dieses Abonnement erstellt) wird angezeigt.

:::image type="content" source="media/pricing.png" alt-text="Meldung, wenn eine F0-Ressource bereits vorhanden ist":::

In der öffentlichen Vorschau darf nur eine Instanz von Metrics Advisor unter einem Abonnement in einer Region erstellt werden.

Wenn Sie bereits über eine Instanz verfügen, die mit demselben Abonnement in derselben Region erstellt wurde, können Sie eine andere Region oder ein anderes Abonnement zum Erstellen einer neuen Instanz verwenden. Oder löschen Sie eine vorhandene Instanz, um eine neue Instanz zu erstellen.

Wenn Sie die vorhandene Instanz bereits gelöscht haben, der Fehler jedoch weiterhin angezeigt wird, warten Sie nach dem Löschen der Ressource ca. 20 Minuten, bevor Sie eine neue Instanz erstellen.

## <a name="basic-concepts"></a>Grundlegende Konzepte

### <a name="what-is-multi-dimensional-time-series-data"></a>Was sind mehrdimensionale Zeitreihendaten?

Weitere Informationen finden Sie in der Definition für [Multi-dimensional metric](glossary.md#multi-dimensional-metric) (Mehrdimensionale Metrik) im Glossar.

### <a name="how-much-data-is-needed-for-metrics-advisor-to-start-anomaly-detection"></a>Wie viele Daten sind erforderlich, damit der Metrics Advisor die Anomalieerkennung startet?

Mindestens ein Datenpunkt kann eine Anomalieerkennung auslösen. Dies bringt jedoch nicht die beste Genauigkeit mit sich. Der Dienst geht von einem Fenster vorheriger Datenpunkte aus und verwendet den Wert, den Sie während der Datenfeederstellung als Regel zum Füllen von Lücken angegeben haben.

Es wird empfohlen, dass Sie vor dem Zeitstempel, für den Sie die Ermittlung durchführen möchten, über einige Daten verfügen.
Ausgehend von der Granularität der Daten variiert die empfohlene Datenmenge wie unten beschrieben.

| Granularität | Empfohlene Datenmenge für die Erkennung |
| ----------- | ------------------------------------- |
| Weniger als 5 Minuten | 4 Tage an Daten |
| 5 Minuten bis 1 Tag | 28 Tage an Daten |
| Mehr als 1 Tag, bis zu 31 Tage | 4 Jahre an Daten |
| Mehr als 31 Tage | 48 Jahre an Daten |

### <a name="why-metrics-advisor-doesnt-detect-anomalies-from-historical-data"></a>Warum erkennt Metrics Advisor keine Anomalien in Verlaufsdaten?

Metrics Advisor dient zum Erkennen von Livestreamingdaten. Es gibt eine Einschränkung der maximalen Länge der Verlaufsdaten, die der Dienst rückwirkend prüft und für die er eine Anomalieerkennung ausführt. Dies bedeutet, dass nur Datenpunkte nach einem bestimmten frühesten Zeitstempel Anomalieerkennungsergebnisse aufweisen. Der früheste Zeitstempel hängt von der Granularität der Daten ab.

Ausgehend von der Granularität der Daten weisen die Längen der Verlaufsdaten die Anomalieerkennungsergebnisse wie folgt auf.

| Granularität | Maximale Länge der Verlaufsdaten für die Anomalieerkennung |
| ----------- | ------------------------------------- |
| Weniger als 5 Minuten | Onboardingzeit – 13 Stunden |
| 5 Minuten bis weniger als 1 Stunde | Onboardingzeit – 4 Tage  |
| 1 Stunde bis weniger als 1 Tag | Onboardingzeit – 14 Tage  |
| 1 Tag | Onboardingzeit – 28 Tage  |
| Mehr als 1 Tag, weniger als 31 Tage | Onboardingzeit – 2 Jahre  |
| Mehr als 31 Tage | Onboardingzeit – 24 Jahre   |

### <a name="more-concepts-and-technical-terms"></a>Weitere Konzepte und technische Begriffe

Weitere Informationen finden Sie auch im [Glossar](glossary.md).

###  <a name="how-do-i-write-a-valid-query-for-ingesting-my-data"></a>Wie schreibe ich eine gültige Abfrage zum Erfassen meiner Daten?  

Damit die Daten vom Metrics Advisor erfasst werden können, müssen Sie eine Abfrage erstellen, die die Dimensionen der Daten zu einem einzelnen Zeitstempel zurückgibt. Der Metrics Advisor führt diese Abfrage mehrmals aus, um die Daten von jedem Zeitstempel abzurufen. 

Beachten Sie, dass die Abfrage zu einem bestimmten Zeitstempel höchstens einen Datensatz für jede Dimensionskombination zurückgeben sollte. Alle zurückgegebenen Datensätze müssen denselben Zeitstempel aufweisen. Es dürfen keine doppelten Datensätze von der Abfrage zurückgegeben werden.

Angenommen, Sie erstellen die folgende Abfrage für eine tägliche Metrik: 
 
`select timestamp, city, category, revenue from sampledata where Timestamp >= @StartTime and Timestamp < dateadd(DAY, 1, @StartTime)`

Achten Sie darauf, dass Sie die richtige Granularität für Ihre Zeitreihe verwenden. Für eine stündliche Metrik verwenden Sie Folgendes: 

`select timestamp, city, category, revenue from sampledata where Timestamp >= @StartTime and Timestamp < dateadd(hour, 1, @StartTime)`

Beachten Sie, dass diese Abfragen nur Daten zu einem einzigen Zeitstempel zurückgeben und alle Dimensionskombinationen enthalten, die vom Metrics Advisor erfasst werden. 

:::image type="content" source="media/query-result.png" alt-text="Meldung, wenn eine F0-Ressource bereits vorhanden ist" lightbox="media/query-result.png":::


### <a name="how-do-i-detect-spikes--dips-as-anomalies"></a>Wie erkenne ich Spitzen und Einbrüche als Anomalien?

Wenn Sie harte Schwellenwerte vordefiniert haben, können Sie „Hard threshold“ (Harter Schwellenwert) unter [anomaly detection configurations](how-tos/configure-metrics.md#anomaly-detection-methods) (Anomalieerkennungskonfiguration) manuell festlegen.
Wenn keine Schwellenwerte vorhanden sind, können Sie die intelligente Erkennung verwenden, die von KI unterstützt wird. Ausführliche Informationen finden Sie unter [Optimieren der Erkennungskonfiguration](how-tos/configure-metrics.md#tune-the-detecting-configuration).

### <a name="how-do-i-detect-inconformity-with-regular-seasonal-patterns-as-anomalies"></a>Wie erkenne ich eine fehlende Übereinstimmung mit regulären (saisonalen) Mustern als Anomalien?

Die intelligente Erkennung ist in der Lage, das Muster Ihrer Daten einschließlich saisonaler Muster zu lernen. Anschließend werden die Datenpunkte, die den regulären Mustern nicht entsprechen, als Anomalien erkannt. Ausführliche Informationen finden Sie unter [Optimieren der Erkennungskonfiguration](how-tos/configure-metrics.md#tune-the-detecting-configuration).

### <a name="how-do-i-detect-flat-lines-as-anomalies"></a>Wie erkenne ich fehlende Schwankungen als Anomalien?

Wenn Ihre Daten normalerweise recht instabil sind und stark schwanken und Sie benachrichtigt werden möchten, wenn die Daten zu stabil sind oder sogar überhaupt keine Schwankungen mehr aufweisen, kann „Change Threshold“ (Änderungsschwellenwert) so konfiguriert werden, dass solche Datenpunkte erkannt werden, wenn die Änderung zu gering ist.
Ausführliche Informationen finden Sie unter [Anomalieerkennungskonfiguration](how-tos/configure-metrics.md#anomaly-detection-methods).

## <a name="next-steps"></a>Nächste Schritte
- [Übersicht über Metrics Advisor](overview.md)
- [Ausprobieren der Demowebsite](quickstarts/explore-demo.md)
- [Verwenden des Webportals](quickstarts/web-portal.md)