---
title: 'Löschen einer Azure Cache for Redis-Instanz: Azure CLI'
description: In diesem Azure CLI-Codebeispiel wird gezeigt, wie Sie mithilfe des Befehls „az redis delete“ eine Azure Cache for Redis-Instanz löschen.
author: yegu-ms
ms.author: yegu
tags: azure-service-management
ms.service: cache
ms.devlang: azurecli
ms.topic: sample
ms.date: 08/30/2017
ms.custom: devx-track-azurecli
ms.openlocfilehash: b1e044c6f61e51de1d8f2ba07f64918307109e46
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87494722"
---
# <a name="delete-an-azure-cache-for-redis"></a>Löschen einer Azure Cache for Redis-Instanz

In diesem Szenario erfahren Sie, wie Sie eine Azure Cache for Redis-Instanz löschen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/redis-cache/delete-cache/delete-cache.sh "Azure Cache for Redis")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um eine Azure Cache for Redis-Instanz zu löschen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az redis delete](https://docs.microsoft.com/cli/azure/redis) | Löscht eine Azure Cache for Redis-Instanz. |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Zusätzliche Azure Cache for Redis-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure Cache for Redis](../cli-samples.md).
