---
title: Übermittlung und Wiederholung – Azure Event Grid IoT Edge | Microsoft-Dokumentation
description: Übermittlung und Wiederholung in Event Grid in IoT Edge.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.subservice: iot-edge
ms.date: 05/10/2021
ms.topic: article
ms.openlocfilehash: 541f85181546dcb9998509841e463fdb0687a5d6
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110370427"
---
# <a name="delivery-and-retry"></a>Übermittlung und Wiederholung

Event Grid bietet permanente Übermittlung. Es versucht, für jedes übereinstimmende Abonnement jede Nachricht mindestens einmal sofort zu übermitteln. Wenn der Endpunkt eines Abonnenten den Empfang eines Ereignisses nicht bestätigt, oder wenn ein Fehler auftritt, versucht Event Grid die Übermittlung erneut auf Grundlage eines festen **Wiederholungszeitplans** und einer festen **Wiederholungsrichtlinie**.  Standardmäßig übermittelt das Event Grid-Modul immer nur ein Ereignis gleichzeitig an den Abonnenten. Die Nutzlast ist jedoch ein Array mit einem einzelnen Ereignis. Sie können das Modul mehr als ein Ereignis gleichzeitig übermitteln lassen, indem Sie die Ausgabebatchverarbeitungs-Funktion aktivieren. Ausführlichere Informationen zu dieser Funktion finden Sie unter [Ausgabebatchverarbeitung](delivery-output-batching.md).  

> [!IMPORTANT]
>Es gibt keine Persistenzunterstützung für Ereignisdaten. Dies bedeutet, dass das erneute Bereitstellen oder Neustarten des Event Grid-Moduls dazu führt, dass alle Ereignisse verloren gehen, die noch nicht übermittelt wurden.

## <a name="retry-schedule"></a>Wiederholungszeitplan

Event Grid wartet nach der Zustellung einer Nachricht bis zu 60 Sekunden auf eine Antwort. Wenn der Endpunkt des Abonnenten die Antwort nicht bestätigt (ACK), wird die Nachricht in eine der Backoff-Warteschlangen für nachfolgende Wiederholungen eingereiht.

Es gibt zwei vorkonfigurierte Backoff-Warteschlangen, die den Zeitplan bestimmen, nach dem ein Wiederholungsversuch durchgeführt wird. Sie lauten wie folgt:

| Zeitplan | BESCHREIBUNG |
| ---------| ------------ |
| 1 Minute | Für Nachrichten, die hier aufgenommen werden, wird jede Minute ein Wiederholungsversuch unternommen.
| 10 Minuten | Für Nachrichten, die hier aufgenommen werden, wird alle 10 Minuten ein Wiederholungsversuch unternommen.

### <a name="how-it-works"></a>Funktionsweise

1. Die Nachricht geht beim Event Grid-Modul ein. Es wird versucht, sie sofort zu übermitteln.
1. Wenn die Übermittlung fehlschlägt, wird die Nachricht in die 1-Minute-Warteschlange eingereiht und nach einer Minute erneut versucht.
1. Wenn die Übermittlung weiterhin fehlschlägt, wird die Nachricht in die 10-Minuten-Warteschlange eingereiht und alle 10 Minuten erneut versucht.
1. Die Übermittlung wird so oft versucht, bis sie erfolgreich ist oder die Limits der Wiederholungsrichtlinien erreicht werden.

## <a name="retry-policy-limits"></a>Wiederholungsrichtlinienlimits

Es gibt zwei Konfigurationen, die die Wiederholungsrichtlinien regeln. Sie lauten wie folgt:

* Maximale Anzahl von Versuchen
* Gültigkeitsdauer des Ereignisses (TTL)

Ein Ereignis wird gelöscht, wenn eins dieser Limits der Wiederholungsrichtlinie erreicht wird. Der Wiederholungszeitplan selbst wurde im Abschnitt „Wiederholungszeitplan“ beschrieben. Die Konfiguration dieser Limits kann entweder für alle Abonnenten oder pro Abonnement erfolgen. Im folgenden Abschnitt werden beide Möglichkeiten detaillierter beschrieben.

## <a name="configuring-defaults-for-all-subscribers"></a>Konfigurieren von Standardwerten für alle Abonnenten

Es gibt zwei Eigenschaften, `brokers__defaultMaxDeliveryAttempts` und `broker__defaultEventTimeToLiveInSeconds`, die als Teil der Event Grid-Bereitstellung konfiguriert werden können, die die Standardeinstellungen für Wiederholungsrichtlinien für alle Abonnenten kontrolliert.

| Eigenschaftenname | BESCHREIBUNG |
| ---------------- | ------------ |
| `broker__defaultMaxDeliveryAttempts` | Die maximale Anzahl der Versuche zur Übermittlung eines Ereignisses. Der Standardwert ist 30.
| `broker__defaultEventTimeToLiveInSeconds` | Ereignisgültigkeitsdauer (Time-to-Live, TTL) in Sekunden, nach der ein Ereignis gelöscht wird, wenn es nicht übermittelt wird. Der Standardwert ist **7200 Sekunden**.

## <a name="configuring-defaults-per-subscriber"></a>Konfigurieren von Standardwerten pro Abonnent

Sie können auch Limits für Wiederholungsrichtlinien pro Abonnement angeben.
Informationen zum Konfigurieren von Standardeinstellungen pro Abonnent finden Sie in unserer [API-Dokumentation](api.md). Standardeinstellungen auf Abonnementebene setzen die Konfigurationen auf Modulebene außer Kraft.

## <a name="examples"></a>Beispiele

Im folgenden Beispiel wird die Wiederholungsrichtlinie im Event Grid-Modul auf „maxNumberOfAttempts = 3“ und eine Ereignisgültigkeitsdauer (TTL) von 30 Minuten festgelegt.

```json
{
  "Env": [
    "broker__defaultMaxDeliveryAttempts=3",
    "broker__defaultEventTimeToLiveInSeconds=1800"
  ],
  "HostConfig": {
    "PortBindings": {
      "4438/tcp": [
        {
          "HostPort": "4438"
        }
      ]
    }
  }
}
```

Im folgenden Beispiel wird ein Webhookabonnement auf „maxNumberOfAttempts = 3“ und eine Ereignisgültigkeitsdauer (TTL) von 30 Minuten festgelegt.

```json
{
 "properties": {
  "destination": {
   "endpointType": "WebHook",
   "properties": {
    "endpointUrl": "<your_webhook_url>",
    "eventDeliverySchema": "eventgridschema"
   }
  },
  "retryPolicy": {
   "eventExpiryInMinutes": 30,
   "maxDeliveryAttempts": 3
  }
 }
}
```
