---
title: Erstellen eines virtuellen Computers auf der Grundlage einer Momentaufnahme – CLI-Beispiel
description: Azure CLI-Skriptbeispiel – Erstellen eines virtuellen Computers aus einer Momentaufnahme
services: virtual-machines-linux
documentationcenter: virtual-machines
author: ramankumarlive
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: a6110ba2787cb99e20c099eb466e2dbd0c3df28e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87085298"
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-cli"></a>Erstellen eines virtuellen Computers aus einer Momentaufnahme über die Befehlszeilenschnittstelle

Dieses Skript erstellt einen virtuellen Computer aus einer Momentaufnahme eines Betriebssystemdatenträgers.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.sh "Create VM from snapshot")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um einen verwalteten Datenträger, einen virtuellen Computer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [az snapshot show](/cli/azure/snapshot) | Ruft die Momentaufnahme unter Verwendung des Momentaufnahmen- und Ressourcengruppennamens ab. Die ID-Eigenschaft des zurückgegebenen Objekts wird verwendet, um einen verwalteten Datenträger zu erstellen.  |
| [az disk create](/cli/azure/disk) | Erstellt verwaltete Datenträger aus einer Momentaufnahme unter Verwendung der Momentaufnahmen-ID, des Datenträgernamens, des Speichertyps und der Größe.  |
| [az vm create](/cli/azure/vm) | Erstellt einen virtuellen Computer unter Verwendung eines verwalteten Betriebssystemdatenträgers. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Zusätzliche VM-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Linux-VMs in Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
