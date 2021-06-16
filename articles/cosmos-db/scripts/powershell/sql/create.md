---
title: PowerShell-Skript zum Erstellen einer Datenbank und eines Containers für die SQL-API in Azure Cosmos DB
description: 'Azure PowerShell-Skript: Azure Cosmos DB – Erstellen einer Datenbank und eines Containers für die SQL-API'
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 05/13/2020
ms.author: mjbrown
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 10bcccd4e8f9556c854b8a728d42ef728f1010ec
ms.sourcegitcommit: df574710c692ba21b0467e3efeff9415d336a7e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110679828"
---
# <a name="create-a-database-and-container-for-azure-cosmos-db---sql-api"></a>Erstellen einer Datenbank und eines Containers für Azure Cosmos DB – SQL-API
[!INCLUDE[appliesto-sql-api](../../../includes/appliesto-sql-api.md)]

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

Für dieses Beispiel ist mindestens Azure PowerShell Az 5.4.0 erforderlich. Führen Sie `Get-Module -ListAvailable Az` aus, um die installierten Versionen zu ermitteln.
Wenn Sie die Installation ausführen müssen, finden Sie unter [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps) Informationen dazu.

Führen Sie zum Anmelden bei Azure [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) aus.

## <a name="sample-script"></a>Beispielskript

Dieses Skript erstellt ein Cosmos-Konto für die SQL-API (Core-API) in zwei Regionen mit Konsistenz auf Sitzungsebene, eine Datenbank und einen Container mit einem Partitionsschlüssel, benutzerdefinierter Indizierungsrichtlinie, einer Richtlinie für eindeutige Schlüssel, TTL, dediziertem Durchsatz und einer Konfliktlösungsrichtlinie vom Typ „Letzter Schreiber gewinnt“ mit einem benutzerdefinierten Konfliktauflösungspfads, der im Fall von `multipleWriteLocations=true` verwendet wird.

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/sql/ps-sql-create.ps1 "Create an account, database, and container for SQL API")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
|**Azure Cosmos DB**| |
| [New-AzCosmosDBAccount](/powershell/module/az.cosmosdb/new-azcosmosdbaccount) | Erstellt ein Cosmos DB-Konto. |
| [New-AzCosmosDBSqlDatabase](/powershell/module/az.cosmosdb/new-azcosmosdbsqldatabase) | Erstellt eine Cosmos DB-SQL-Datenbank. |
| [New-AzCosmosDBSqlUniqueKey](/powershell/module/az.cosmosdb/new-azcosmosdbsqluniquekey) | Erstellt ein „PSSqlUniqueKey“-Objekt, das als Parameter für „New-AzCosmosDBSqlUniqueKeyPolicy“ verwendet wird. |
| [New-AzCosmosDBSqlUniqueKeyPolicy](/powershell/module/az.cosmosdb/new-azcosmosdbsqluniquekeypolicy) | Erstellt ein „PSSqlUniqueKeyPolicy“-Objekt, das als Parameter für „New-AzCosmosDBSqlContainer“ verwendet wird. |
| [New-AzCosmosDBSqlCompositePath](/powershell/module/az.cosmosdb/new-azcosmosdbsqlcompositepath) | Erstellt ein „PSCompositePath“-Objekt, das als Parameter für „New-AzCosmosDBSqlIndexingPolicy“ verwendet wird. |
| [New-AzCosmosDBSqlIncludedPathIndex](/powershell/module/az.cosmosdb/new-azcosmosdbsqlincludedpathindex) | Erstellt ein „PSIndexes“-Objekt, das als Parameter für „New-AzCosmosDBSqlIncludedPath“ verwendet wird. |
| [New-AzCosmosDBSqlIncludedPath](/powershell/module/az.cosmosdb/new-azcosmosdbsqlincludedpath) | Erstellt ein „PSIncludedPath“-Objekt, das als Parameter für „New-AzCosmosDBSqlIndexingPolicy“ verwendet wird. |
| [New-AzCosmosDBSqlIndexingPolicy](/powershell/module/az.cosmosdb/new-azcosmosdbsqlindexingpolicy) | Erstellt ein „PSSqlIndexingPolicy“-Objekt, das als Parameter für „New-AzCosmosDBSqlContainer“ verwendet wird. |
| [New-AzCosmosDBSqlConflictResolutionPolicy](/powershell/module/az.cosmosdb/new-azcosmosdbsqlconflictresolutionpolicy) | Erstellt ein „PSSqlConflictResolutionPolicy“-Objekt, das als Parameter für „New-AzCosmosDBSqlContainer“ verwendet wird. |
| [New-AzCosmosDBSqlContainer](/powershell/module/az.cosmosdb/new-azcosmosdbsqlcontainer) | Erstellt einen neuen Cosmos DB-SQL-Container. |
|**Azure-Ressourcengruppen**| |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/).
