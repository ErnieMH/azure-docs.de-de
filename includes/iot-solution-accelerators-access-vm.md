---
title: include file
description: include file
services: iot-accelerators
author: dominicbetts
ms.service: iot-accelerators
ms.topic: include
ms.date: 08/16/2018
ms.author: dobett
ms.custom: include file, devx-track-azurecli
ms.openlocfilehash: 817c41a969f03ad04d372c516a16ef6b770f3e18
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92756060"
---
## <a name="access-the-virtual-machine"></a>Zugriff auf den virtuellen Computer

Bei den nachstehenden Schritten wird die Azure-Befehlszeilenschnittstelle in der Azure Cloud Shell verwendet. Wenn es Ihnen lieber ist, können Sie auf Ihrem Entwicklungscomputer die [Azure-Befehlszeilenschnittstelle installieren](/cli/azure/install-azure-cli) und die Befehle lokal ausführen.

Die nachstehenden Schritte zeigen, wie Sie den virtuellen Azure-Computer so konfigurieren, das **SSH** -Zugriff zulässig ist. Bei den gezeigten Schritten wird angenommen, dass der für den Solution Accelerator gewählte Name **Contoso-Simulation** lautet. Ersetzen Sie diesen Wert durch den Namen Ihrer Bereitstellung:

1. Listen Sie die Inhalte der Ressourcengruppe auf, in der die Ressourcen des Solution Accelerators enthalten sind:

    ```azurecli-interactive
    az resource list -g contoso-simulation -o table
    ```

    Notieren Sie sich den Namen des virtuellen Computers, die öffentliche IP-Adresse und die Netzwerksicherheitsgruppe. Sie benötigen diese Werte zu einem späteren Zeitpunkt.

1. Aktualisieren Sie die Netzwerksicherheitsgruppe, um SSH-Zugriff zu ermöglichen. Bei dem folgenden Befehl wird angenommen, dass der Name der Netzwerksicherheitsgruppe **Contoso-Simulation-Nsg** lautet. Ersetzen Sie diesen Wert durch den Namen Ihrer Netzwerksicherheitsgruppe:

    ```azurecli-interactive
    az network nsg rule update --name SSH --nsg-name contoso-simulation-nsg -g contoso-simulation --access Allow -o table
    ```

    Aktivieren Sie SSH-Zugriff nur während der Test- und Entwicklungsphase. Wenn Sie SSH aktivieren, [sollten Sie es so bald wie möglich wieder deaktivieren](https://docs.microsoft.com/azure/security/fundamentals/network-best-practices#disable-rdpssh-access-to-virtual-machines).

1. Aktualisieren Sie das Kennwort für das **Azureuser** -Konto auf dem virtuellen Computer in ein Ihnen bekanntes Kennwort. Wählen Sie Ihr eigenes Kennwort aus, wenn Sie den folgenden Befehl ausführen:

    ```azurecli-interactive
    az vm user update --name vm-vikxv --username azureuser --password YOURSECRETPASSWORD  -g contoso-simulation
    ```

1. Suchen Sie die öffentliche IP-Adresse Ihres virtuellen Computers. Bei dem folgenden Befehl wird angenommen, dass der Name des virtuellen Computers **Vm-Vikxv** lautet. Ersetzen Sie diesen Wert durch den Namen des Computers, den Sie sich zuvor notiert haben:

    ```azurecli-interactive
    az vm list-ip-addresses --name vm-vikxv -g contoso-simulation -o table
    ```

    Notieren Sie sich die öffentliche IP-Adresse Ihres virtuellen Computers.
