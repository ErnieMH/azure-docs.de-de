---
title: 'Schnellstart: Erstellen eines privaten Azure-Endpunkts mit der Azure-Befehlszeilenschnittstelle'
description: In diesem Schnellstart erfahren Sie etwas über den privaten Azure-Endpunkt.
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: quickstart
ms.date: 09/16/2019
ms.author: allensu
ms.custom: devx-track-azurecli
ms.openlocfilehash: e7c098ba06086781306960f76978aac9e4fa06bc
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "87502663"
---
# <a name="quickstart-create-a-private-endpoint-using-azure-cli"></a>Schnellstart: Erstellen eines privaten Endpunkts mit Azure CLI

Ein privater Endpunkt ist der grundlegende Baustein für Private Link in Azure. Mit ihm können Azure-Ressourcen wie virtuelle Computer (VMs) privat mit Private Link-Ressourcen kommunizieren. In diesem Schnellstart erfahren Sie, wie Sie eine VM in einem virtuellen Netzwerk und einen Server in SQL-Datenbank mit einem privaten Endpunkt unter Verwendung der Azure-Befehlszeilenschnittstelle erstellen. Anschließend können Sie auf die VM zugreifen und sicher auf die Private Link-Ressource zugreifen (in diesem Beispiel auf einen privaten Server in SQL-Datenbank).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie stattdessen entscheiden, Azure CLI lokal zu installieren und zu verwenden möchten, müssen Sie für diese Schnellstartanleitung mindestens die Azure CLI-Version 2.0.28 verwenden. Führen Sie `az --version` aus, um die installierte Version zu ermitteln. Informationen zum Ausführen einer Installation oder eines Upgrades finden Sie unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Bevor Sie eine Ressource erstellen können, müssen Sie eine Ressourcengruppe zum Hosten des virtuellen Netzwerks erstellen. Erstellen Sie mit [az group create](/cli/azure/group) eine Ressourcengruppe. Bei diesem Beispiel wird am Standort *westcentralus* eine Ressourcengruppe namens *myResourceGroup* erstellt:

```azurecli-interactive
az group create --name myResourceGroup --location westcentralus
```

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Erstellen Sie mit [az network vnet create](/cli/azure/network/vnet) ein virtuelles Netzwerk. Bei diesem Beispiel wird ein virtuelles Standardnetzwerk mit dem Namen *myVirtualNetwork* und dem Subnetz *mySubnet* erstellt:

```azurecli-interactive
az network vnet create \
 --name myVirtualNetwork \
 --resource-group myResourceGroup \
 --subnet-name mySubnet
```

## <a name="disable-subnet-private-endpoint-policies"></a>Deaktivieren von Richtlinien für den privaten Endpunkt im Subnetz

Azure stellt Ressourcen für ein Subnetz innerhalb eines virtuellen Netzwerks bereit. Daher müssen Sie das Subnetz erstellen oder aktualisieren, um die Netzwerkrichtlinien für den privaten Endpunkt im Subnetz zu deaktivieren. Aktualisieren Sie eine Subnetzkonfiguration mit dem Namen *mySubnet* mit [az network vnet subnet update](https://docs.microsoft.com/cli/azure/network/vnet/subnet?view=azure-cli-latest#az-network-vnet-subnet-update):

```azurecli-interactive
az network vnet subnet update \
 --name mySubnet \
 --resource-group myResourceGroup \
 --vnet-name myVirtualNetwork \
 --disable-private-endpoint-network-policies true
```

## <a name="create-the-vm"></a>Erstellen des virtuellen Computers

Erstellen Sie mit „az vm create“ einen virtuellen Computer. Geben Sie bei entsprechender Aufforderung ein Kennwort als Anmeldeinformation für den virtuellen Computer ein. In diesem Beispiel wird ein virtueller Computer mit dem Namen *myVm* erstellt:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm \
  --image Win2019Datacenter
```

Notieren Sie sich die öffentliche IP-Adresse der VM. Sie verwenden diese Adresse im nächsten Schritt zum Herstellen einer Verbindung mit dem virtuellen Computer über das Internet.

## <a name="create-a-server-in-sql-database"></a>Erstellen eines Servers in SQL-Datenbank

Erstellen Sie mit dem Befehl „az sql server create“ einen Server in SQL-Datenbank. Denken Sie daran, dass der Name Ihres Servers innerhalb von Azure eindeutig sein muss, und ersetzen Sie daher den Platzhalterwert in Klammern durch Ihren eigenen eindeutigen Wert:

```azurecli-interactive
# Create a server in the resource group
az sql server create \
    --name "myserver"\
    --resource-group myResourceGroup \
    --location WestUS \
    --admin-user "sqladmin" \
    --admin-password "CHANGE_PASSWORD_1"

# Create a database in the server with zone redundancy as false
az sql db create \
    --resource-group myResourceGroup  \
    --server myserver \
    --name mySampleDatabase \
    --sample-name AdventureWorksLT \
    --edition GeneralPurpose \
    --family Gen4 \
    --capacity 1
```

Die Server-ID ähnelt  ```/subscriptions/subscriptionId/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/myserver.``` Sie verwenden die Server-ID im nächsten Schritt.

## <a name="create-the-private-endpoint"></a>Erstellen des privaten Endpunkts

Erstellen Sie einen privaten Endpunkt für den logischen SQL-Server in Ihrem virtuellen Netzwerk:

```azurecli-interactive
az network private-endpoint create \  
    --name myPrivateEndpoint \  
    --resource-group myResourceGroup \  
    --vnet-name myVirtualNetwork  \  
    --subnet mySubnet \  
    --private-connection-resource-id "<server ID>" \  
    --group-ids sqlServer \  
    --connection-name myConnection  
 ```

## <a name="configure-the-private-dns-zone"></a>Konfigurieren der privaten DNS-Zone

Erstellen Sie eine private DNS-Zone für die SQL-Datenbank-Domäne, erstellen Sie eine Zuordnungsverknüpfung mit dem virtuellen Netzwerk, und erstellen Sie eine DNS-Zonengruppe, um den privaten Endpunkt der privaten DNS-Zone zuzuordnen. 

```azurecli-interactive
az network private-dns zone create --resource-group myResourceGroup \
   --name  "privatelink.database.windows.net"
az network private-dns link vnet create --resource-group myResourceGroup \
   --zone-name  "privatelink.database.windows.net"\
   --name MyDNSLink \
   --virtual-network myVirtualNetwork \
   --registration-enabled false
az network private-endpoint dns-zone-group create \
   --resource-group myResourceGroup \
   --endpoint-name myPrivateEndpoint \
   --name MyZoneGroup \
   --private-dns-zone "privatelink.database.windows.net" \
   --zone-name sql
```

## <a name="connect-to-a-vm-from-the-internet"></a>Herstellen einer Verbindung mit einem virtuellen Computer über das Internet

Stellen Sie wie folgt eine Verbindung mit dem virtuellen Computer *myVm* aus dem Internet her:

1. Geben Sie in der Suchleiste des Portals *myVm* ein.

1. Wählen Sie die Schaltfläche **Verbinden** aus. Nach dem Auswählen der Schaltfläche **Verbinden** wird **Verbindung mit virtuellem Computer herstellen** geöffnet.

1. Wählen Sie **RDP-Datei herunterladen** aus. Azure erstellt eine Remotedesktopprotokoll-Datei (*RDP*) und lädt sie auf Ihren Computer herunter.

1. Öffnen Sie die heruntergeladene RDP*-Datei.

    1. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.

    1. Geben Sie den Benutzernamen und das Kennwort ein, den/das Sie beim Erstellen des virtuellen Computers angegeben haben.

        > [!NOTE]
        > Unter Umständen müssen Sie **Weitere Optionen** > **Anderes Konto verwenden** auswählen, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.

1. Klicken Sie auf **OK**.

1. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wenn Sie eine Zertifikatwarnung erhalten, wählen Sie **Ja** oder **Weiter** aus.

1. Sobald der VM-Desktop angezeigt wird, minimieren Sie ihn, um zu Ihrem lokalen Desktop zurückzukehren.  

## <a name="access-sql-database-privately-from-the-vm"></a>Privates Zugreifen auf SQL-Datenbank über die VM

In diesem Abschnitt stellen Sie über den privaten Endpunkt eine Verbindung von der VM mit SQL-Datenbank her.

1. Öffnen Sie auf dem Remotedesktop von *myVM* PowerShell.
2. Geben Sie „nslookup myserver.database.windows.net“ ein.

   Sie erhalten eine Meldung wie die folgende:

    ```
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    myserver.privatelink.database.windows.net
    Address:  10.0.0.5
    Aliases:  myserver.database.windows.net
    ```

3. Installieren von SQL Server Management Studio
4. Geben Sie in „Mit Server verbinden“ diese Informationen ein, oder wählen Sie sie aus:

   - Servertyp: Wählen Sie „Datenbank-Engine“ aus.
   - Servername: Wählen Sie „myserver.database.windows.net“ aus.
   - Benutzername: Geben Sie einen während der Erstellung angegebenen Benutzernamen ein.
   - Password (Kennwort): Geben Sie ein während der Erstellung angegebenes Kennwort ein.
   - Kennwort speichern: Wählen Sie „Ja“ aus.

5. Wählen Sie **Verbinden**.
6. Durchsuchen Sie **Datenbanken** im linken Menü.
7. (Optional) Erstellen oder Abfragen von Informationen aus *mydatabase*
8. Schließen Sie die Remotedesktopverbindung mit *myVm*.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn die Ressourcengruppe und alle enthaltenen Ressourcen nicht mehr benötigt werden, können Sie sie mit „az group delete“ entfernen:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [Azure Private Link](private-link-overview.md).
