---
title: 'Verbindungsrichtlinie: Azure Functions-Trigger für Cosmos DB'
description: Hier erfahren Sie, wie Sie die vom Azure Functions-Trigger für Cosmos DB verwendete Verbindungsrichtlinie konfigurieren.
author: ealsur
ms.service: cosmos-db
ms.topic: how-to
ms.date: 07/17/2019
ms.author: maquaran
ms.openlocfilehash: df30a0ddca58e6d7bd74184fa7287df6818cfc37
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84117143"
---
# <a name="how-to-configure-the-connection-policy-used-by-azure-functions-trigger-for-cosmos-db"></a>Konfigurieren der vom Azure Functions-Trigger für Cosmos DB verwendeten Verbindungsrichtlinie

In diesem Artikel wird die Konfiguration der Verbindungsrichtlinie beschrieben, wenn Sie mithilfe des Azure Functions-Triggers für Cosmos DB eine Verbindung mit Ihrem Azure Cosmos-Konto herstellen.

## <a name="why-is-the-connection-policy-important"></a>Warum ist die Verbindungsrichtlinie wichtig?

Es gibt zwei Verbindungsmodi: den direkten Modus und den Gatewaymodus. Weitere Informationen zu diesen Verbindungsmodi finden Sie im Artikel [Leistungstipps für Azure Cosmos DB und .NET](./performance-tips.md#networking). Standardmäßig wird ein **Gateway** zum Herstellen aller Verbindungen für den Azure Functions-Trigger für Cosmos DB verwendet. Dies ist jedoch unter Umständen nicht die optimale Option für Szenarien mit hohen Leistungsanforderungen.

## <a name="changing-the-connection-mode-and-protocol"></a>Ändern des Verbindungsmodus und des Protokolls

Für die Konfiguration der Clientverbindungsrichtlinie stehen zwei wichtige Konfigurationseinstellungen zur Verfügung: der **Verbindungsmodus** und das **Verbindungsprotokoll**. Sie können die Standardeinstellung für den Verbindungsmodus und das Verbindungsprotokoll ändern, die vom Azure Functions-Trigger für Cosmos DB und allen [Azure Cosmos DB-Bindungen](../azure-functions/functions-bindings-cosmosdb-v2-output.md) verwendet werden. Wenn Sie die Standardeinstellungen ändern möchten, müssen Sie in Ihrem Azure Functions-Projekt zur Datei `host.json` navigieren und die folgende [zusätzliche Einstellung](../azure-functions/functions-bindings-cosmosdb-v2-output.md#hostjson-settings) hinzufügen:

```js
{
  "cosmosDB": {
    "connectionMode": "Direct",
    "protocol": "Tcp"
  }
}
```

Dabei muss für `connectionMode` der gewünschte Verbindungsmodus (direkte Verbindung oder Gatewayverbindung) und für `protocol` das gewünschte Verbindungsprotokoll (TCP oder HTTPS) angegeben werden. 

Bei Azure Functions-Projekten mit Version 1 der Azure Functions-Runtime hat die Konfiguration einen geringfügig anderen Namen. Verwenden Sie `documentDB` anstelle von `cosmosDB`:

```js
{
  "documentDB": {
    "connectionMode": "Direct",
    "protocol": "Tcp"
  }
}
```

> [!NOTE]
> Wenn Sie als Azure Functions-Hostingplan den Verbrauchsplan verwenden, gilt für jede Instanz ein Grenzwert bei der Menge der Socket-Verbindungen, die aufrechterhalten werden können. Wenn Sie den direkten Modus oder den TCP-Modus verwenden, werden standardmäßig mehr Verbindungen erstellt, und unter Umständen wird der [Grenzwert des Verbindungsplans](../azure-functions/manage-connections.md#connection-limit) erreicht. In diesem Fall können Sie entweder den Gatewaymodus verwenden oder Azure Functions im [App Service-Modus](../azure-functions/functions-scale.md#app-service-plan) ausführen.

## <a name="next-steps"></a>Nächste Schritte

* [Verbindungsgrenzwerte in Azure Functions](../azure-functions/manage-connections.md#connection-limit)
* [Leistungstipps für Azure Cosmos DB](./performance-tips.md)
* [Codebeispiele](https://github.com/ealsur/serverless-recipes/tree/master/connectionmode)
