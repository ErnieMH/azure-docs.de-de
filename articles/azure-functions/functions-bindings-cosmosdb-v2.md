---
title: Azure Cosmos DB-Bindungen für Functions 2.x oder höher
description: Grundlegendes zur Verwendung von Azure Cosmos DB-Triggern und -Bindungen in Azure Functions.
author: craigshoemaker
ms.topic: reference
ms.date: 02/24/2017
ms.author: cshoe
ms.openlocfilehash: 2c6efd14bd974de1b01b1725b9810f153df74bf8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85482172"
---
# <a name="azure-cosmos-db-trigger-and-bindings-for-azure-functions-2x-and-higher-overview"></a>Übersicht über Azure Cosmos DB-Trigger und -Bindungen für Azure Functions 2.x oder höher

> [!div class="op_single_selector" title1="Wählen Sie die von Ihnen verwendete Version der Azure Functions-Runtime aus: "]
> * [Version 1](functions-bindings-cosmosdb.md)
> * [Version 2 oder höher](functions-bindings-cosmosdb-v2.md)

In dieser Gruppe von Artikeln wird die Verwendung von [Azure Cosmos DB](../cosmos-db/serverless-computing-database.md)-Bindungen in Azure Functions 2.x oder höher erläutert. Azure Functions unterstützt Trigger sowie Ein- und Ausgabebindungen für Azure Cosmos DB.

| Aktion | type |
|---------|---------|
| Ausführen einer Funktion, wenn ein Azure Cosmos DB-Dokument erstellt oder geändert wird | [Trigger](./functions-bindings-cosmosdb-v2-trigger.md) |
| Lesen eines Azure Cosmos DB-Dokuments | [Eingabebindung](./functions-bindings-cosmosdb-v2-input.md) |
| Speichern der Änderungen an einem Azure Cosmos DB-Dokument  |[Ausgabebindung](./functions-bindings-cosmosdb-v2-output.md) |

> [!NOTE]
> Dieser Verweis gilt für [Azure Functions Version 2.x oder höher](functions-versions.md).  Informationen zur Verwendung dieser Bindungen in Functions 1.x finden Sie unter [Azure Cosmos DB-Bindungen für Azure Functions](functions-bindings-cosmosdb.md).
>
> Diese Bindung hatte ursprünglich die Bezeichnung „DocumentDB“. In der Functions-Version 2.x oder höher wurden der Trigger, die Bindungen und das Paket jeweils mit dem Namen „Cosmos DB“ versehen.

## <a name="supported-apis"></a>Unterstützte APIs

[!INCLUDE [SQL API support only](../../includes/functions-cosmosdb-sqlapi-note.md)]

## <a name="add-to-your-functions-app"></a>Hinzufügen zu Ihrer Funktions-App

### <a name="functions-2x-and-higher"></a>Functions 2.x und höher

Das Arbeiten mit Triggern und Bindungen erfordert, dass Sie auf das entsprechende Paket verweisen. Das NuGet-Paket wird für .NET-Klassenbibliotheken verwendet, während das Erweiterungspaket für alle anderen Anwendungstypen verwendet wird.

| Sprache                                        | Hinzufügen nach...                                   | Bemerkungen 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | Installieren des [NuGet-Paket], Version 3.x | |
| C#-Skript, Java, JavaScript, Python, PowerShell | Registrieren des [Erweiterungspaket]          | Die [Azure-Tools-Erweiterung] wird zur Verwendung mit Visual Studio Code empfohlen. |
| C#-Skript (nur online im Azure-Portal)         | Hinzufügen einer Bindung                            | Informationen zum Aktualisieren vorhandener Bindungserweiterungen, ohne Ihre Funktions-App erneut veröffentlichen zu müssen, finden Sie unter [Aktualisieren Ihrer Erweiterungen]. |

[NuGet-Paket]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB
[core tools]: ./functions-run-local.md
[Erweiterungspaket]: ./functions-bindings-register.md#extension-bundles
[Aktualisieren Ihrer Erweiterungen]: ./install-update-binding-extensions-manual.md
[Azure-Tools-Erweiterung]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>Functions 1.x

Functions 1.x-Apps enthalten automatisch einen Verweis auf das NuGet-Paket, Version 2.x, [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs).

## <a name="next-steps"></a>Nächste Schritte

- [Ausführen einer Funktion, wenn ein Azure Cosmos DB-Dokument erstellt oder geändert wird (Trigger)](./functions-bindings-cosmosdb-v2-trigger.md)
- [Lesen eines Azure Cosmos DB-Dokuments (Eingabebindung)](./functions-bindings-cosmosdb-v2-input.md)
- [Speichern der Änderungen an einem Azure Cosmos DB-Dokument (Ausgabebindung)](./functions-bindings-cosmosdb-v2-output.md)
