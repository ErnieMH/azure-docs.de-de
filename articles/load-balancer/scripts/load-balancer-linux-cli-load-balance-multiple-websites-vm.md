---
title: 'Lastenausgleich für mehrere Websites: Azure CLI – Azure Load Balancer'
description: Dieses Azure CLI-Skriptbeispiel zeigt, wie Sie einen Lastausgleich für mehrere Websites auf dem gleichen virtuellen Computer vornehmen.
documentationcenter: load-balancer
author: asudbring
ms.service: load-balancer
ms.devlang: azurecli
ms.topic: sample
ms.workload: infrastructure
ms.date: 04/20/2018
ms.author: allensu
ms.custom: devx-track-azurecli
ms.openlocfilehash: 77dfc42164419d86e174bc47297e2f596e757ffe
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107790093"
---
# <a name="azure-cli-script-example-load-balance-multiple-websites"></a>Azure CLI-Skriptbeispiele: Lastenausgleich für mehrere Websites

Dieses Azure CLI-Skriptbeispiel erstellt ein virtuelles Netzwerk mit zwei virtuellen Computern, die einer Verfügbarkeitsgruppe angehören. Ein Load Balancer leitet Datenverkehr für zwei separate IP-Adressen an die beiden virtuellen Computer. Nach der Ausführung des Skripts können Sie Webserversoftware auf den virtuellen Computern bereitstellen und mehrere Websites (jede mit eigener IP-Adresse) hosten.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript


[!code-azurecli-interactive[main](../../../cli_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.sh  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, ein virtuelles Netzwerk, einen Load Balancer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create) | Erstellt ein virtuelles Azure-Netzwerk und ein Subnetz. |
| [az network public-ip create](/cli/azure/network/public-ip#az_network_public_ip_create) | Erstellt eine öffentliche IP-Adresse mit einer statischen IP-Adresse und zugeordnetem DNS-Namen. |
| [az network lb create](/cli/azure/network/lb#az_network_lb_create) | Erstellt eine Azure Load Balancer-Instanz. |
| [az network lb probe create](/cli/azure/network/lb/probe#az_network_lb_probe_create) | Erstellt einen Lastenausgleichtest. Ein Lastenausgleichtest wird verwendet, um jeden virtuellen Computer in der Load Balancer-Gruppe zu überwachen. Falls eine VM nicht verfügbar ist, wird der Datenverkehr nicht an diese VM geroutet. |
| [az network lb rule create](/cli/azure/network/lb/rule#az_network_lb_rule_create) | Erstellt eine Lastenausgleichsregel. In diesem Beispiel wird eine Regel für den Port 80 erstellt. Wenn HTTP-Datenverkehr beim Load Balancer eingeht, wird dieser an Port 80 einer der VMs in der Load Balancer-Gruppe geroutet. |
| [az network lb frontend-ip create](/cli/azure/network/lb/frontend-ip#az_network_lb_frontend_ip_create) | Erstellt eine Front-End-IP-Adresse für den Load Balancer. |
| [az network lb address-pool create](/cli/azure/network/lb/address-pool#az_network_lb_address_pool_create) | Erstellt einen Back-End-Adresspool. |
| [az network nic create](/cli/azure/network/nic#az_network_nic_create) | Erstellt eine virtuelle Netzwerkkarte und verbindet diese mit dem virtuellen Netzwerk und dem Subnetz. |
| [az vm availability-set create](/cli/azure/network/lb/rule#az_network_lb_rule_create) | Erstellt eine Verfügbarkeitsgruppe. Verfügbarkeitsgruppen stellen die Verfügbarkeit von Anwendungen sicher, indem sie die virtuellen Computer auf physische Ressourcen verteilen. So wirken sich Ausfälle nicht auf die gesamte Gruppe aus. |
| [az network nic ip-config create](/cli/azure/network/nic/ip-config#az_network_nic_ip_config_create) | Erstellt eine IP-Konfiguration. Das Feature „Microsoft.Network/AllowMultipleIpConfigurationsPerNic“ muss für Ihr Abonnement aktiviert sein. Als primäre IP-Konfiguration kann pro NIC nur eine Konfiguration festgelegt werden. Verwenden Sie dazu das Flag „--make-primary“. |
| [az vm create](/cli/azure/vm/availability-set#az_vm_availability_set_create) | Erstellt den virtuellen Computer und verbindet diesen mit der Netzwerkkarte, dem virtuellen Netzwerk, dem Subnetz und der NSG. Dieser Befehl legt außerdem das zu verwendende VM-Image und die Administratoranmeldeinformationen fest.  |
| [az group delete](/cli/azure/vm/extension#az_vm_extension_set) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Zusätzliche Netzwerk-CLI-Skriptbeispiele finden Sie unter [Azure CLI Samples for networking](../cli-samples.md?toc=%2fazure%2fnetworking%2ftoc.json) (Azure CLI-Beispiele für Netzwerke).