---
title: 'CLI: Verbinden einer App mit Redis'
description: Hier erfahren Sie, wie Sie die Azure CLI zum Automatisieren der Bereitstellung und Verwaltung Ihrer App Service-App verwenden. Dieses Beispiel zeigt, wie Sie eine Verbindung zwischen einer App und einer Azure Cache for Redis-Instanz herstellen.
author: msangapu-msft
tags: azure-service-management
ms.assetid: bc8345b2-8487-40c6-a91f-77414e8688e6
ms.devlang: azurecli
ms.topic: sample
ms.date: 12/11/2017
ms.author: msangapu
ms.custom: mvc, seodec18, devx-track-azurecli
ms.openlocfilehash: 24658525ca1248aa82732a31eb9a40df1403203c
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107782396"
---
# <a name="connect-an-app-service-app-to-an-azure-cache-for-redis-using-cli"></a>Herstellen einer Verbindung zwischen einer App Service-App und einer Azure Cache for Redis-Instanz mithilfe der Befehlszeilenschnittstelle

Dieses Beispielskript erstellt eine Azure Cache for Redis-Instanz und eine App Service-App. Anschließend wird die Azure Cache for Redis-Instanz mithilfe von App-Einstellungen mit der App verknüpft.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, benötigen Sie die Azure CLI-Version 2.0 oder höher. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-redis/connect-to-redis.sh "Azure Cache for Redis")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, eine App Service-App, eine Azure Cache for Redis-Instanz und alle dazugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [`az group create`](/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [`az appservice plan create`](/cli/azure/appservice/plan#az_appservice_plan_create) | Erstellt einen App Service-Plan. |
| [`az webapp create`](/cli/azure/webapp#az_webapp_create) | Erstellt eine App Service-App. |
| [`az redis create`](/cli/azure/redis#az_redis-create) | Erstellt eine neue Azure Cache for Redis-Instanz. |
| [`az redis list-keys`](/cli/azure/redis#az_redis_list_keys) | Listet die Zugriffsschlüssel für die Azure Cache for Redis-Instanz auf. |
| [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_set) | Erstellt oder aktualisiert eine App-Einstellung für eine App Service-App. App-Einstellungen werden als Umgebungsvariablen für Ihre App verfügbar gemacht. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure App Service](../samples-cli.md).
