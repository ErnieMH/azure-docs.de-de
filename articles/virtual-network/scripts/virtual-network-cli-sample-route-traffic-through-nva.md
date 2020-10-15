---
title: 'Weiterleiten von Datenverkehr per virtuellem Netzwerkgerät: Azure CLI-Skriptbeispiel'
description: 'Weiterleiten von Datenverkehr über eine virtuelle Firewallnetzwerkappliance: Azure CLI-Skriptbeispiel'
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: kumud
ms.custom: devx-track-azurecli
ms.openlocfilehash: 964fda8168867c115502c7262dc1d41e55075866
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91317649"
---
# <a name="route-traffic-through-a-network-virtual-appliance---azure-cli-script-sample"></a>Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät – Azure CLI-Beispielskript

In diesem Skriptbeispiel wird ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetz erstellt. Es wird auch ein virtueller Computer mit IP-Weiterleitung erstellt, der zur Weiterleitung von Datenverkehr zwischen den zwei Subnetzen aktiviert ist. Nach dem Ausführen des Skripts können Sie dem virtuellen Computer Netzwerksoftware bereitstellen, z.B. eine Firewallanwendung.

Sie können das Skript über Azure [Cloud Shell](https://shell.azure.com/bash) oder eine lokale Installation der Azure CLI ausführen. Wenn Sie die CLI lokal verwenden, muss für dieses Skript Version 2.0.28 oder höher ausgeführt werden. Führen Sie `az --version` aus, um die installierte Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI](/cli/azure/install-azure-cli). Wenn Sie die CLI lokal ausführen, müssen Sie auch `az login` ausführen, um eine Verbindung mit Azure herzustellen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen:

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, ein virtuelles Netzwerk und Netzwerksicherheitsgruppen zu erstellen. Jeder Befehl in der folgenden Tabelle ist mit der zugehörigen Dokumentation verknüpft:

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az network vnet create](/cli/azure/network/vnet) | Erstellt ein virtuelles Azure-Netzwerk und ein Front-End-Subnetz. |
| [az network subnet create](/cli/azure/network/vnet/subnet) | Erstellt Back-End- und DMZ-Subnetz. |
| [az network public-ip create](/cli/azure/network/public-ip) | Erstellt eine öffentliche IP-Adresse für den Zugriff auf den virtuellen Computer aus dem Internet. |
| [az network nic create](/cli/azure/network/nic) | Erstellt eine virtuelle Netzwerkschnittstelle und aktiviert die IP-Weiterleitung dafür. |
| [az network nsg create](/cli/azure/network/nsg) | Erstellt eine Netzwerksicherheitsgruppe (NSG). |
| [az network nsg rule create](/cli/azure/network/nsg/rule) | Erstellt NSG-Regeln, die eingehende HTTP- und HTTPS-Ports für den virtuellen Computer ermöglichen. |
| [az network vnet subnet update](/cli/azure/network/vnet/subnet)| Ordnet die NSGs und Routentabellen Subnetzen zu. |
| [az network route-table create](/cli/azure/network/route-table#az-network-route-table-create)| Erstellt eine Routingtabelle für alle Routen. |
| [az network route-table route create](/cli/azure/network/route-table/route#az-network-route-table-route-create)| Erstellt die Routen zum Weiterleiten von Datenverkehr zwischen Subnetzen und dem Internet über den virtuellen Computer. |
| [az vm create](/cli/azure/vm) | Erstellt einen virtuellen Computer und fügt ihm eine NIC an. Dieser Befehl legt außerdem das zu verwendende VM-Image und die Administratoranmeldeinformationen fest. |
| [az group delete](/cli/azure/group) | Löscht eine Ressourcengruppe und alle darin enthaltenen Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere CLI-Skriptbeispiele für virtuelle Netzwerke finden Sie unter [Azure CLI-Beispiele für virtuelle Netzwerke](../cli-samples.md).
