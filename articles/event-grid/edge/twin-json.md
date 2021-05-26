---
title: Modulzwilling – Azure Event Grid IoT Edge | Microsoft-Dokumentation
description: Hier wird die Konfiguration über den Modulzwilling erläutert.
author: HiteshMadan
manager: rajarv
ms.author: himad
ms.reviewer: spelluru
ms.subservice: iot-edge
ms.date: 05/10/2021
ms.topic: article
ms.openlocfilehash: 27e5194b86ea5f76a346412c3239dac111c07d80
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110379781"
---
# <a name="module-twin-json-schema-azure-event-grid"></a>JSON-Schema des Modulzwillings (Azure Event Grid)

Event Grid in IoT Edge ist mit dem IoT Edge-Ökosystem integriert und unterstützt das Erstellen von Themen und Abonnements über den Modulzwilling. Zudem wird der aktuelle Status aller Themen und Ereignisabonnements an die gemeldeten Eigenschaften im Modulzwilling übermittelt.

> [!WARNING]
> Aufgrund von Einschränkungen im IoT Edge-Ökosystem wurden alle Arrayelemente im folgenden JSON-Beispiel als JSON-Zeichenfolgen codiert. Weitere Informationen finden Sie unter den Schlüsseln `EventSubscription.Filter.EventTypes` und `EventSubscription.Filter.AdvancedFilters` im unten stehenden Beispiel.

## <a name="desired-properties-json"></a>Gewünschte Eigenschaften – JSON

* Der Wert jedes Schlüssel-Wert-Paars im Abschnitt „topics“ weist dasselbe JSON-Schema auf, das auch beim Erstellen von Themen für `Topic.Properties` in der API verwendet wird.
* Der Wert jedes Schlüssel-Wert-Paars im Abschnitt **EventSubscriptions** weist genau dasselbe JSON-Schema auf, das auch beim Erstellen von Themen für `EventSubscription.Properties` in der API verwendet wird.
* Wenn Sie ein Thema löschen möchten, legen Sie seinen Wert in den gewünschten Eigenschaften auf `null` fest.
* Das Löschen von Ereignisabonnements über gewünschte Eigenschaften wird nicht unterstützt.

```json
{
    "topics": {
        "twinTopic1": {},
        "twinTopic2": {
            "inputSchema": "customEventSchema"
        }
    },
    "eventSubscriptions": {
        "twinTopic1WebhookSub": {
            "topic": "twinTopic1",
            "retryPolicy": {
                "eventExpiryInMinutes": 120,
                "maxDeliveryAttempts": 30
            },
            "destination": {
                "endpointType": "WebHook",
                "properties": {
                    "endpointUrl": "https://localhost:4438"
                }
            },
            "filter": {
                "subjectBeginsWith": "^",
                "subjectEndsWith": "$",
                "isSubjectCaseSensitive": false,
                "includedEventTypes": "[\"et1\",\"et2\"]",
                "advancedFilters": "[{\"value\":true,\"operatorType\":\"BoolEquals\",\"key\":\"data.b\"},{\"values\":[\"\\\"\",\"c\"],    \"operatorType\":\"StringContains\",\"key\":\"data.s\"}]"
            }
        },
        "twinTopic2EdgeHubSub": {
            "topic": "twinTopic2",
            "deliveryPolicy": {
                "approxBatchSizeInBytes": 200000,
                "maxEventsPerBatch": 25
            },
            "destination": {
                "endpointType": "EdgeHub",
                "properties": {
                    "outputName": "twinTopic2EdgeHubSub"
                }
            },
            "filter": {
                "advancedFilters": "[{\"value\":true,\"operatorType\":\"BoolEquals\",\"key\":\"dAt\\\"A.a\"},{\"values\":[\"\\\"\",    \"c\"],\"operatorType\":\"StringContains\",\"key\":\"dAt\\\"A.a\"}]"
            }
        }
    }
}
```

## <a name="reported-properties-json"></a>Gemeldete Eigenschaften – JSON

Der Abschnitt „Gemeldete Eigenschaften“ des Modulzwillings enthält die folgenden Informationen:

* Gruppe der Themen und Abonnements, die im Speicher des Moduls vorhanden sind
* Fehler, die beim Erstellen gewünschter Themen/Ereignisabonnements aufgetreten sind
* Aufgetretene Startfehler (z. B. JSON-Parsingfehler bei gewünschten Eigenschaften)

```json
{
    "topics": {
        "twinTopic1": {},
        "twinTopic2": {
            "inputSchema": "customEventSchema"
        }
    },
    "eventSubscriptions": {
        "twinTopic1WebhookSub": {
            "topic": "twinTopic1",
            "retryPolicy": {
                "eventExpiryInMinutes": 120,
                "maxDeliveryAttempts": 30
            },
            "destination": {
                "endpointType": "WebHook",
                "properties": {
                    "endpointUrl": "https://localhost:4438"
                }
            },
            "filter": {
                "subjectBeginsWith": "^",
                "subjectEndsWith": "$",
                "isSubjectCaseSensitive": false,
                "includedEventTypes": "[\"et1\",\"et2\"]",
                "advancedFilters": "[{\"value\":true,\"operatorType\":\"BoolEquals\",\"key\":\"data.b\"},{\"values\":[\"\\\"\",\"c\"],    \"operatorType\":\"StringContains\",\"key\":\"data.s\"}]"
            }
        },
        "twinTopic2EdgeHubSub": {
            "topic": "twinTopic2",
            "deliveryPolicy": {
                "approxBatchSizeInBytes": 200000,
                "maxEventsPerBatch": 25
            },
            "destination": {
                "endpointType": "EdgeHub",
                "properties": {
                    "outputName": "twinTopic2EdgeHubSub"
                }
            },
            "filter": {
                "advancedFilters": "[{\"value\":true,\"operatorType\":\"BoolEquals\",\"key\":\"dAt\\\"A.a\"},{\"values\":[\"\\\"\",    \"c\"],\"operatorType\":\"StringContains\",\"key\":\"dAt\\\"A.a\"}]"
            }
        }
    },
    "errors": {
        "bootupMessage": "",
        "bootupException": "",
        "topics": {},
        "eventSubscriptions": {
            "twinTopic1EventGridUserTopicSub": "HttpStatusCode='BadRequest' ErrorCode='InvalidDestination' Message='EventSubscription.Properties.Destination failed validation. Reason: EndpointUrl must target the /api/events API of Azure Event Grid in the cloud..'"
        }
    }
}
```
