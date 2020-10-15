---
title: Erstellen eines Azure Cosmos-Kontos mit VNET-Dienstendpunkten
description: Erstellen eines Azure Cosmos-Kontos mit VNET-Dienstendpunkten
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: 4d1a56c80cab58e98121ae35c98a086d16dfe02b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87432241"
---
# <a name="create-an-azure-cosmos-account-with-virtual-network-service-endpoints-using-azure-cli"></a>Erstellen eines Azure Cosmos-Kontos mit VNET-Dienstendpunkten mit der Azure-Befehlszeilenschnittstelle (Azure CLI)

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, ist es für diesen Befehl erforderlich, mindestens die Azure CLI-Version 2.9.1 auszuführen. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sei bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Beispielskript

Dieses Beispiel erstellt ein neues virtuelles Netzwerk mit einem Front- und Back-End-Subnetz und aktiviert Dienstendpunkte für `Microsoft.AzureCosmosDB`. Anschließend wird die Ressourcen-ID für dieses Subnetz abgerufen und auf das Azure Cosmos-Konto angewendet, und es werden Dienstendpunkte für das Konto aktiviert.

> [!NOTE]
> Dieses Beispiel veranschaulicht die Verwendung eines Core-API-Kontos (SQL). Wenn Sie dieses Beispiel für andere APIs verwenden möchten, wenden Sie die Parameter `enable-virtual-network` und `virtual-network-rules` im folgenden Skript auf Ihr API-spezifisches Skript an.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/common/service-endpoints.sh "Create an Azure Cosmos account with service endpoints.")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```azurecli-interactive
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create) | Erstellt ein virtuelles Azure-Netzwerk. |
| [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) | Erstellt ein Subnetz für ein virtuelles Azure-Netzwerk. |
| [az network vnet subnet show](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-show) | Gibt ein Subnetz für ein virtuelles Azure-Netzwerk zurück. |
| [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) | Erstellt ein Konto für Azure Cosmos DB. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure Cosmos DB-CLI finden Sie in der [Dokumentation zur Azure Cosmos DB-CLI](/cli/azure/cosmosdb).

Alle CLI-Skriptbeispiele für Azure Cosmos DB finden Sie im [GitHub-Repository zur Azure Cosmos DB-CLI](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
