---
title: Weiterleiten von Datenverkehr für Hochverfügbarkeit von Anwendungen – Azure CLI – Traffic Manager
description: 'Azure CLI-Skriptbeispiel: Weiterleiten von Datenverkehr für Hochverfügbarkeit von Anwendungen'
services: traffic-manager
documentationcenter: traffic-manager
author: duongau
manager: twooley
tags: azure-infrastructure
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: traffic-manager
ms.date: 04/26/2018
ms.author: duau
ms.openlocfilehash: 86151efdc6d2b17c9eef722f2dc3c6306d5aa1b8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89400224"
---
# <a name="route-traffic-for-high-availability-of-applications-using-azure-cli"></a>Weiterleiten von Datenverkehr für Hochverfügbarkeit von Anwendungen mit Azure CLI

Dieses Skript erstellt eine Ressourcengruppe, zwei App Service-Pläne, zwei Web-Apps, ein Traffic Manager-Profil und zwei Traffic Manager-Endpunkte. Traffic Manager leitet Datenverkehr zur Anwendung an eine Region weiter, die als primäre Region gilt, und an die sekundäre Region, wenn die Anwendung in der primären Region nicht verfügbar ist. Vor dem Ausführen des Skripts müssen Sie die Werte von MyWebApp, MyWebAppL1 und MyWebAppL2 in Werte ändern, die innerhalb von Azure eindeutig sind. Nach dem Ausführen des Skripts können Sie in der primären Region mit der URL „mywebapp.trafficmanager.net“ auf die App zugreifen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/traffic-manager/direct-traffic-for-increased-application-availability/direct-traffic-for-increased-application-availability.sh "Route traffic for high availability")]


## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, die App Service-App und alle zugehörigen Ressourcen entfernt werden.

```azurecli
az group delete --name myResourceGroup1 --yes
az group delete --name myResourceGroup2 --yes
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, eine Web-App, ein Traffic Manager-Profil und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan) | Erstellt einen App Service-Plan. Dies ist wie eine Serverfarm für die Azure-Web-App. |
| [az webapp web create](https://docs.microsoft.com/cli/azure/webapp#az-webapp-create) | Erstellt eine Azure-Web-App im App Service-Plan. |
| [az network traffic-manager profile create](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile) | Erstellt ein Azure Traffic Manager-Profil. |
| [az network traffic-manager endpoint create](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint) | Fügt einem Azure Traffic Manager-Profil einen Endpunkt hinzu. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in den [Azure CLI Samples for networking](../cli-samples.md) (Azure CLI-Beispiele für Netzwerke).
