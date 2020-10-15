---
title: Schemas für Diagnoseprotokolle von Azure Media Services – Azure
description: In diesem Artikel werden die Schemas für Diagnoseprotokolle von Azure Media Services veranschaulicht.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/31/2020
ms.author: inhenkel
ms.openlocfilehash: 8cfbe26458de630aaf411aade4a31cb4e9c72b17
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89295426"
---
# <a name="diagnostic-logs-schemas"></a>Schemas für Diagnoseprotokolle

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

[Azure Monitor](../../azure-monitor/overview.md) ermöglicht Ihnen die Überwachung von Metriken und Diagnoseprotokollen, die Ihnen zu verstehen helfen, wie sich Ihre Anwendungen verhalten. Sie können Media Services-Diagnoseprotokolle überwachen sowie Warnungen und Benachrichtigungen für die gesammelten Metriken und Protokolle erstellen. Sie können Protokolle an [Azure Storage](https://azure.microsoft.com/services/storage/) senden, sie an [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) streamen und in [Log Analytics](https://azure.microsoft.com/services/log-analytics/) exportieren oder Dienste von Drittanbietern nutzen.

Ausführliche Informationen dazu finden Sie unter [Azure Monitor-Metrik](../../azure-monitor/platform/data-platform.md) und [Azure Monitor-Diagnoseprotokolle](../../azure-monitor/platform/platform-logs-overview.md).

In diesem Artikel werden Schemas für Diagnoseprotokolle von Media Services beschrieben.

## <a name="top-level-diagnostic-logs-schema"></a>Schema der obersten Ebene für Diagnoseprotokolle

Eine detaillierte Beschreibung des Schemas der obersten Ebene für Diagnoseprotokolle finden Sie unter [Unterstützte Dienste, Schemas und Kategorien für Azure-Diagnoseprotokolle](../../azure-monitor/platform/resource-logs-schema.md).

## <a name="key-delivery-log-schema"></a>Protokollschema für Schlüsselübermittlung

### <a name="properties"></a>Eigenschaften

Diese Eigenschaften sind spezifisch für das Protokollschema für Schlüsselübermittlung.

|Name|BESCHREIBUNG|
|---|---|
|keyId|Die ID des angeforderten Schlüssels.|
|keyType|Es kann sich um einen der folgenden Werte handeln: „Clear“ (keine Verschlüsselung), „FairPlay“, „PlayReady“ oder „Widevine“.|
|policyName|Der Azure Resource Manager-Name der Richtlinie.|
|tokenType|Der Tokentyp.|
|statusMessage|Die Statusmeldung.|

### <a name="examples"></a>Beispiele

Eigenschaften des Schemas für Schlüsselübermittlungsanforderungen.

```json
{
    "time": "2019-01-11T17:59:10.4908614Z",
    "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000/RESOURCEGROUPS/SBKEY/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/SBDNSTEST",
    "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
    "operationVersion": "1.0",
    "category": "KeyDeliveryRequests",
    "resultType": "Succeeded",
    "resultSignature": "OK",
    "durationMs": 315,
    "identity": {
        "authorization": {
            "issuer": "http://testacs",
            "audience": "urn:test"
        },
        "claims": {
            "urn:microsoft:azure:mediaservices:contentkeyidentifier": "3321e646-78d0-4896-84ec-c7b98eddfca5",
            "iss": "http://testacs",
            "aud": "urn:test",
            "exp": "1547233138"
        }
    },
    "level": "Informational",
    "location": "uswestcentral",
    "properties": {
        "requestId": "b0243468-d8e5-4edf-a48b-d408e1661050",
        "keyType": "Clear",
        "keyId": "3321e646-78d0-4896-84ec-c7b98eddfca5",
        "policyName": "56a70229-82d0-4174-82bc-e9d3b14e5dbf",
        "tokenType": "JWT",
        "statusMessage": "OK"
    }
} 
```

```json
 {
    "time": "2019-01-11T17:59:33.4676382Z",
    "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-0000000000/RESOURCEGROUPS/SBKEY/PROVIDERS/MICROSOFT.MEDIA/MEDIASERVICES/SBDNSTEST",
    "operationName": "MICROSOFT.MEDIA/MEDIASERVICES/CONTENTKEYS/READ",
    "operationVersion": "1.0",
    "category": "KeyDeliveryRequests",
    "resultType": "Failed",
    "resultSignature": "Unauthorized",
    "durationMs": 2,
    "level": "Error",
    "location": "uswestcentral",
    "properties": {
        "requestId": "875af030-b77c-416b-b7e1-58f23ebec182",
        "keyType": "Clear",
        "keyId": "3321e646-78d0-4896-84ec-c7b98eddfca5",
        "policyName": "56a70229-82d0-4174-82bc-e9d3b14e5dbf",
        "tokenType": "None",
        "statusMessage": "No token present in authorization header or URL."
    }
} 
```

## <a name="additional-notes"></a>Zusätzliche Hinweise

* Widevine ist ein von Google Inc. bereitgestellter Dienst, der den Vertragsbedingungen und der Datenschutzrichtlinie von Google, Inc. unterliegt.

## <a name="next-steps"></a>Nächste Schritte

[Überwachen von Media Services-Metriken und -Diagnoseprotokollen](media-services-metrics-diagnostic-logs.md)
