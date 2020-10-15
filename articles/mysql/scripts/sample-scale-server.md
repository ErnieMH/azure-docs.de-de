---
title: 'CLI-Skript: Skalieren eines Servers – Azure Database for MySQL'
description: Dieses CLI-Beispielskript skaliert einen Azure-Datenbank für MySQL-Server nach Abfragen der Metriken auf eine andere Leistungsstufe.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc, devx-track-azurecli
ms.date: 12/02/2019
ms.openlocfilehash: 318e41089f096f22d17faaa77863fe6af54a3553
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87483738"
---
# <a name="monitor-and-scale-an-azure-database-for-mysql-server-using-azure-cli"></a>Überwachen und Skalieren eines Azure-Datenbank für MySQL-Servers mithilfe der Azure CLI
Dieses CLI-Beispielskript skaliert Compute- und Speicherressourcen für einen einzelnen Azure Database for MySQL-Server nach Abfragen der Metriken. Computeressourcen können hoch- oder herunterskaliert werden. Speicherressourcen können nur hochskaliert werden.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Befehlszeilenschnittelle lokal ausführen, ist für diesen Artikel Azure CLI Version 2.0 oder höher erforderlich. Überprüfen Sie die Version, indem Sie `az --version` ausführen. Informationen zum Installieren oder Aktualisieren Ihrer Version der Azure CLI finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Beispielskript
Aktualisieren Sie das Skript mit Ihrer Abonnement-ID.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh "Create and scale Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Verwenden Sie den folgenden Befehl, um die Ressourcengruppe und alle damit verbundenen Ressourcen zu entfernen, nachdem das Skript ausgeführt wurde. 
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Erläuterung des Skripts
In diesem Skript werden die Befehle verwendet, die in der folgenden Tabelle aufgeführt sind:

| **Befehl** | **Hinweise** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az mysql server create](/cli/azure/mysql/server#az-mysql-server-create) | Erstellt einen MySQL-Server, der die Datenbanken hostet. |
| [az mysql server update](/cli/azure/mysql/server#az-mysql-server-update) | Aktualisiert die Eigenschaften des MySQL-Servers. |
| [az monitor metrics list](/cli/azure/monitor/metrics#az-monitor-metrics-list) | Listet den Metrikwert für die Ressourcen auf. |
| [az group delete](/cli/azure/group#az-group-delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte
- Informieren Sie sich ausführlicher über [Compute- und Speicherressourcen in Azure Database for MySQL](../concepts-pricing-tiers.md).
- Probieren Sie weitere Skripts aus: [Azure CLI-Beispiele für Azure-Datenbank für MySQL](../sample-scripts-azure-cli.md)
- Lesen Sie weitere Informationen zur [Azure-Befehlszeilenschnittstelle](/cli/azure).
