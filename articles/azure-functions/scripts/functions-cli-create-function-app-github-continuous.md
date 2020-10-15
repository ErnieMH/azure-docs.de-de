---
title: 'Erstellen einer Funktions-App mit GitHub-Bereitstellung: Azure CLI'
description: Erstellen einer Funktions-App und Bereitstellen von Funktionscode aus einem GitHub-Repository mit Azure Functions.
ms.date: 07/03/2018
ms.topic: sample
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 7f7c272fa95d2697d41cb751e39ef72b0da86a12
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87498517"
---
# <a name="create-a-function-app-in-azure-that-is-deployed-from-github"></a>Erstellen einer Funktions-App in Azure, die über GitHub bereitgestellt wird

Dieses Azure Functions-Beispielskript erstellt eine Funktions-App unter Verwendung des [Verbrauchsplans](../functions-scale.md#consumption-plan) (zusammen mit den dazugehörigen Ressourcen). Darüber hinaus konfiguriert das Skript den Funktionscode für Continuous Deployment über ein GitHub-Repository. 

In diesem Beispiel benötigen Sie Folgendes:

* Ein GitHub-Repository mit Funktionscode, für das Sie über Administratorberechtigungen verfügen.
* Ein [persönliches Zugriffstoken (PAT)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) für Ihr GitHub-Konto.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die Azure CLI dagegen lokal verwenden, müssen Sie Version 2.0 oder höher verwenden. Um die Azure CLI-Version zu ermitteln, führen Sie `az --version` aus. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Beispielskript

Dieses Beispiel erstellt eine Azure-Funktionen-App und stellt Funktionscode über GitHub bereit.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github-continuous/deploy-function-app-with-function-github-continuous.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft. Das Skript verwendet die folgenden Befehle:

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Erstellt das für die Funktions-App erforderliche Speicherkonto. |
| [az functionapp create](/cli/azure/functionapp#az-functionapp-create) | Erstellt eine Funktions-App im serverlosen [Verbrauchsplan](../functions-scale.md#consumption-plan) und ordnet sie einem Git- oder Mercurial-Repository zu. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere Azure Functions-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Functions](../functions-cli-samples.md).
