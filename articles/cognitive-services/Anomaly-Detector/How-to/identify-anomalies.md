---
title: Verwenden der Anomalieerkennungs-API für Zeitreihendaten
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie Anomalien in Ihren Daten entweder als Batch oder beim Streaming von Daten erkennen können.
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: mbullwin
ms.openlocfilehash: 74f891ba7f5b400b5782565e670539167f4e2464
ms.sourcegitcommit: e7152996ee917505c7aba707d214b2b520348302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/20/2020
ms.locfileid: "97703431"
---
# <a name="how-to-use-the-anomaly-detector-api-on-your-time-series-data"></a>Gewusst wie: Verwenden der Anomalieerkennungs-API für Zeitreihendaten  

Die [Anomalieerkennungs-API](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect) bietet zwei Methoden zur Erkennung von Anomalien. Sie können Anomalien entweder als Batch über Ihre Zeitreihen oder beim Generieren Ihrer Daten erkennen, indem Sie den Anomaliestatus des letzten Datenpunkts erkennen. Das Erkennungsmodell gibt die Anomalieergebnisse zusammen mit dem erwarteten Wert der einzelnen Datenpunkte und den oberen und unteren Grenzen der Anomalieerkennung zurück. Mit diesen Werten können Sie den Bereich der Normalwerte und Anomalien in den Daten visualisieren.

## <a name="anomaly-detection-modes"></a>Anomalieerkennungsmodi 

Die Anomalieerkennungs-API stellt Erkennungsmodi bereit: Batch und Streaming.

> [!NOTE]
> Die folgenden Anforderungs-URLs müssen mit dem entsprechenden Endpunkt für Ihr Abonnement kombiniert werden. Beispiel: `https://<your-custom-subdomain>.api.cognitive.microsoft.com/anomalydetector/v1.0/timeseries/entire/detect`


### <a name="batch-detection"></a>Batcherkennung

Verwenden Sie den folgenden Anforderungs-URI mit Ihren Zeitreihendaten, um Anomalien in einem Batch von Datenpunkten über einen bestimmten Zeitbereich zu erkennen: 

`/timeseries/entire/detect`. 

Indem Sie Ihre Zeitreihendaten auf einmal senden, generiert die API ein Modell, das die gesamte Zeitreihe verwendet und analysiert damit jeden Datenpunkt.  

### <a name="streaming-detection"></a>Streamingerkennung

Verwenden Sie den folgenden Anforderungs-URI mit Ihrem neuesten Datenpunkt, um kontinuierlich Anomalien beim Streaming der Daten zu erkennen: 

`/timeseries/last/detect'`. 

Indem Sie neue Datenpunkte während der Generierung senden, können Sie Ihre Daten in Echtzeit überwachen. Es wird ein Modell mit den von Ihnen gesendeten Datenpunkten erstellt, und die API ermittelt, ob der letzte Punkt in der Zeitreihe eine Anomalie darstellt.

## <a name="adjusting-lower-and-upper-anomaly-detection-boundaries"></a>Anpassen der unteren und oberen Grenzen der Anomalieerkennung

Standardmäßig werden die obere und untere Grenze für die Anomalieerkennung mit `expectedValue`, `upperMargin` und `lowerMargin` berechnet. Wenn Sie andere Grenzen benötigen, wird empfohlen, `marginScale` auf `upperMargin` oder `lowerMargin` anzuwenden. Die Grenzen werden wie folgt berechnet:

|Grenze  |Berechnung  |
|---------|---------|
|`upperBoundary` | `expectedValue + (100 - marginScale) * upperMargin`        |
|`lowerBoundary` | `expectedValue - (100 - marginScale) * lowerMargin`        |

Die folgenden Beispiele zeigen ein Ergebnis der Anomalieerkennungs-API bei verschiedenen Empfindlichkeiten.

### <a name="example-with-sensitivity-at-99"></a>Beispiel mit einer Empfindlichkeit von 99

![Standardempfindlichkeit](../media/sensitivity_99.png)

### <a name="example-with-sensitivity-at-95"></a>Beispiel mit einer Empfindlichkeit von 95

![Empfindlichkeit 99](../media/sensitivity_95.png)

### <a name="example-with-sensitivity-at-85"></a>Beispiel mit einer Empfindlichkeit von 85

![Empfindlichkeit 85](../media/sensitivity_85.png)

## <a name="next-steps"></a>Nächste Schritte

* [Was ist die Anomalieerkennungs-API?](../overview.md)
* [Schnellstart: Verwenden der Anomalieerkennungs-Clientbibliothek](../quickstarts/client-libraries.md)
