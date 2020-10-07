---
title: Datenkonvertierung – LUIS
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Äußerungen vor der Vorhersage in Language Understanding (LUIS) geändert werden können.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.openlocfilehash: b305be693f59b65a62570f656a0132f4f03cf099
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2020
ms.locfileid: "91541797"
---
# <a name="convert-data-format-of-utterances"></a>Konvertieren des Datenformats von Äußerungen
LUIS bietet die folgenden Konvertierungen einer Benutzeräußerung vor der Vorhersage:

* Spracherkennung mit [Cognitive Services – Speech Service](../Speech-Service/overview.md)

## <a name="speech-to-text"></a>Spracherkennung

Die Spracherkennung wird als Integration mit LUIS bereitgestellt.

### <a name="intent-conversion-concepts"></a>Konzepte zur Absichtsumsetzung
Die Sprache-Absichts-Umsetzung in LUIS ermöglicht es Ihnen, gesprochene Äußerungen an einen Endpunkt zu senden und als Antwort eine LUIS-Vorhersage zu erhalten. Dieser Vorgang stellt eine Integration des [Sprachverständnis](https://docs.microsoft.com/azure/cognitive-services/Speech)-Diensts in LUIS dar. Erfahren Sie mehr über die Sprache-Absichts-Umsetzung in einem [Tutorial](../speech-service/how-to-recognize-intents-from-speech-csharp.md).

### <a name="key-requirements"></a>Schlüsselanfoderungen
Sie brauchen für diese Integration keinen Schlüssel für eine **Bing-Sprach-API** zu erstellen. Ein im Azure-Portal erstellter **Language Understanding**-Schlüssel funktioniert bei dieser Integration. Verwenden Sie nicht den LUIS-Starterschlüssel.

### <a name="pricing-tier"></a>Preisstufe
Für diese Integration gilt ein anderes [Preismodell](luis-limits.md#key-limits) als für die üblichen Language Understanding-Tarife.

### <a name="quota-usage"></a>Kontingentverbrauch
Informationen finden Sie unter [Schlüsselgrenzwerte](luis-limits.md#key-limits).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Extrahieren von Daten](luis-concept-data-extraction.md)

