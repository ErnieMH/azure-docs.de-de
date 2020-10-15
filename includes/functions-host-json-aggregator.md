---
title: include file
description: include file
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/19/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 9c51ce726545d1c64d69c86c36fc69ea43c3b882
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "76279308"
---
Gibt an, wie viele Funktionsaufrufe aggregiert werden, wenn [Metriken für Application Insights berechnet werden](../articles/azure-functions/functions-monitoring.md#configure-the-aggregator). 

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    }
}
```

|Eigenschaft |Standard  | BESCHREIBUNG |
|---------|---------|---------| 
|batchSize|1000|Maximale Anzahl der zu aggregierenden Anforderungen.| 
|flushTimeout|00:00:30|Maximal zu aggregierender Zeitraum.| 

Funktionsaufrufe werden aggregiert, wenn der erste der beiden Grenzwerte erreicht wird.
