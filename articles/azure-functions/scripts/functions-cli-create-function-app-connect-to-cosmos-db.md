---
title: 'Erstellen einer Funktions-App mit Azure Cosmos DB: Azure CLI'
description: 'Azure CLI-Skriptbeispiel: Erstellen einer Azure-Funktion, die eine Verbindung mit Azure Cosmos DB herstellt'
ms.topic: sample
ms.date: 07/03/2018
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 440767159ec1321d9b157f53408dbff8f9706eec
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87498564"
---
# <a name="create-an-azure-function-that-connects-to-an-azure-cosmos-db"></a>Erstellen einer Azure-Funktion, die eine Verbindung mit Azure Cosmos DB herstellt

Dieses Azure Functions-Beispielskript erstellt eine Funktions-App und stellt eine Verbindung zwischen der Funktion und einer Azure Cosmos DB-Datenbank her. Die erstellte App-Einstellung mit der Verbindung kann mit einem [Azure Cosmos DB-Trigger oder einer Azure Cosmos DB-Bindung](../functions-bindings-cosmosdb.md) verwendet werden.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Verwenden Sie im Fall der lokalen Befehlszeilenschnittstelle mindestens die Azure CLI-Version 2.0. Führen Sie `az --version` aus, um die Version zu finden. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Beispielskript

In diesem Beispiel werden eine Azure-Funktionen-App erstellt und ein Cosmos DB-Endpunkt sowie ein Zugriffsschlüssel zu den App-Einstellungen hinzugefügt.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh "Create an Azure Function that connects to an Azure Cosmos DB")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript benutzt die folgenden Befehle: Jeder Befehl in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Erstellen Sie eine Ressourcengruppe mit einem Speicherort. |
| [az storage accounts create](/cli/azure/storage/account#az-storage-account-create) | Speicherkonto erstellen |
| [az functionapp create](/cli/azure/functionapp#az-functionapp-create) | Erstellt eine Funktions-App im serverlosen [Verbrauchsplan](../functions-scale.md#consumption-plan). |
| [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) | Erstellen Sie eine Azure Cosmos DB-Datenbank. |
| [az cosmosdb show](/cli/azure/cosmosdb#az-cosmosdb-show)| Ruft die Verbindung des Datenbankkontos ab. |
| [az cosmosdb list-keys](/cli/azure/cosmosdb#az-cosmosdb-list-keys)| Ruft die Schlüssel für die Datenbank ab. |
| [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) | Legt die Verbindungszeichenfolge als App-Einstellung in der Funktions-App fest. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere Azure Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions](../functions-cli-samples.md).




