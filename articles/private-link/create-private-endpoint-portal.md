---
title: 'Schnellstart: Verwalten von privaten Endpunkten in Azure'
description: In diesem Schnellstart erfahren Sie, wie Sie über das Azure-Portal einen privaten Endpunkt erstellen.
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: quickstart
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: ef6d49c9046ba04bbac40ec9bf555e12d2faa8f6
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "84021703"
---
# <a name="quickstart-create-a-private-endpoint-using-azure-portal"></a>Schnellstart: Erstellen eines privaten Endpunkts mit dem Azure-Portal

Ein privater Endpunkt ist der grundlegende Baustein für Private Link in Azure. Mit ihm können Azure-Ressourcen wie virtuelle Computer (VMs) privat mit Private Link-Ressourcen kommunizieren. In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe des Azure-Portals einen virtuellen Computer in einem virtuellen Azure-Netzwerk und einen logischen SQL-Server mit einem privaten Azure-Endpunkt erstellen. Anschließend können Sie vom virtuellen Computer sicher auf SQL-Datenbank zugreifen.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.


## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

## <a name="create-a-vm"></a>Erstellen einer VM
In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und das Subnetz zum Hosten des virtuellen Computers, der für den Zugriff auf Ihre Private Link-Ressource verwendet wird (in diesem Beispiel ein SQL-Server in Azure).

## <a name="virtual-network-and-parameters"></a>Virtuelles Netzwerk und Parameter

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und das Subnetz zum Hosten des virtuellen Computers, der für den Zugriff auf Ihre Private Link-Ressource verwendet wird.

In den Schritten dieses Abschnitts müssen die folgenden Parameter wie folgt ersetzt werden:

| Parameter                   | Wert                |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroup |
| **\<virtual-network-name>** | myVirtualNetwork          |
| **\<region-name>**          | USA, Westen-Mitte    |
| **\<IPv4-address-space>**   | 10.1.0.0/16          |
| **\<subnet-name>**          | mySubnet        |
| **\<subnet-address-range>** | 10.1.0.0/24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

### <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

1. Wählen Sie oben links auf dem Bildschirm im Azure-Portal die Option **Ressource erstellen** > **Compute** > **Virtueller Computer** aus.

1. Geben Sie in **Virtuellen Computer erstellen – Grundlagen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **PROJEKTDETAILS** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie **myResourceGroup** aus. Diese haben Sie im vorherigen Abschnitt erstellt.  |
    | **INSTANZDETAILS** |  |
    | Name des virtuellen Computers | Geben Sie *myVm* ein. |
    | Region | Wählen Sie **WestCentralUS** aus. |
    | Verfügbarkeitsoptionen | Übernehmen Sie den Standardwert **Keine Infrastrukturredundanz erforderlich**. |
    | Image | Wählen Sie **Windows Server 2019 Datacenter** aus. |
    | Size | Übernehmen Sie den Standardwert **Standard DS1 v2**. |
    | **ADMINISTRATORKONTO** |  |
    | Username | Geben Sie einen Benutzernamen Ihrer Wahl ein. |
    | Kennwort | Geben Sie das gewünschte Kennwort ein. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) erfüllen.|
    | Kennwort bestätigen | Geben Sie das Kennwort erneut ein. |
    | **REGELN FÜR EINGEHENDE PORTS** |  |
    | Öffentliche Eingangsports | Übernehmen Sie den Standardwert **Keine**. |
    | **SPAREN SIE GELD** |  |
    | Windows-Lizenz bereits vorhanden? | Übernehmen Sie den Standardwert **Nein**. |
    |||

1. Klicken Sie auf **Weiter: Datenträger**.

1. Übernehmen Sie unter **Virtuellen Computer erstellen – Datenträger** die Standardwerte, und wählen Sie **Weiter: Netzwerk**.

1. Wählen Sie in **Virtuellen Computer erstellen – Netzwerk** diese Informationen aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Virtuelles Netzwerk | Übernehmen Sie den Standardwert **MyVirtualNetwork**.  |
    | Adressraum | Übernehmen Sie den Standardwert **10.1.0.0/24**.|
    | Subnet | Übernehmen Sie den Standardwert **mySubnet (10.1.0.0/24)** .|
    | Öffentliche IP-Adresse | Übernehmen Sie den Standardwert **(neu) myVm-ip**. |
    | Öffentliche Eingangsports | Wählen Sie **Ausgewählte Ports zulassen** aus. |
    | Eingangsports auswählen | Wählen Sie **HTTP** und **RDP** aus.|
    |||


1. Klicken Sie auf **Überprüfen + erstellen**. Sie werden zur Seite **Überprüfen und erstellen** weitergeleitet, auf der Azure Ihre Konfiguration überprüft.

1. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus.

## <a name="create-a-logical-sql-server"></a>Erstellen eines logischen SQL-Servers

In diesem Abschnitt erstellen Sie einen logischen SQL-Server in Azure. 

1. Wählen Sie oben links auf dem Bildschirm im Azure-Portal die Option **Ressource erstellen** > **Datenbanken** > **SQL-Datenbank** aus.

1. Geben Sie unter **SQL-Datenbank erstellen – Grundlagen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **Datenbankdetails** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie **myResourceGroup** aus. Diese haben Sie im vorherigen Abschnitt erstellt.|
    | **INSTANZDETAILS** |  |
    | Datenbankname  | Geben Sie *mydatabase* ein. Wenn dieser Name vergeben ist, erstellen Sie einen eindeutigen Namen. |
    |||
5. Wählen Sie unter **Server** die Option **Neu erstellen** aus. 
6. Geben Sie bei **Neuer Server** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    |Servername  | Geben Sie *myserver* ein. Wenn dieser Name vergeben ist, erstellen Sie einen eindeutigen Namen.|
    | Serveradministratoranmeldung| Geben Sie einen Administratornamen Ihrer Wahl ein. |
    | Kennwort | Geben Sie das gewünschte Kennwort ein. Das Kennwort muss mindestens acht Zeichen lang sein und die festgelegten Anforderungen erfüllen. |
    | Standort | Wählen Sie eine Azure-Region aus, in der sich Ihr SQL Server befinden soll. |
    
7. Klicken Sie auf **OK**. 
8. Klicken Sie auf **Überprüfen + erstellen**. Sie werden zur Seite **Überprüfen und erstellen** weitergeleitet, auf der Azure Ihre Konfiguration überprüft. 
9. Wenn die Meldung „Überprüfung erfolgreich“ angezeigt wird, wählen Sie **Erstellen** aus. 
10. Wenn die Meldung „Überprüfung erfolgreich“ angezeigt wird, wählen Sie „Erstellen“ aus. 

## <a name="create-a-private-endpoint"></a>Erstellen eines privaten Endpunkts

In diesem Abschnitt erstellen Sie einen SQL-Server und fügen ihm einen privaten Endpunkt hinzu. 

1. Wählen Sie oben links auf dem Bildschirm im Azure-Portal die Option **Ressource erstellen** > **Netzwerk** > **Private Link-Center (Vorschau)** aus.
2. Wählen Sie unter **Privat Link-Center – Übersicht** bei der Option **Build a private connection to a service** (Private Verbindung mit einem Dienst herstellen) **Start** aus.
1. Geben Sie unter **Privaten Endpunkt erstellen (Vorschau) – Grundlagen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | **Projektdetails** | |
    | Subscription | Wählen Sie Ihr Abonnement aus. |
    | Resource group | Wählen Sie **myResourceGroup** aus. Diese haben Sie im vorherigen Abschnitt erstellt.|
    | **INSTANZDETAILS** |  |
    | Name | Geben Sie *myPrivateEndpoint* ein. Wenn dieser Name vergeben ist, erstellen Sie einen eindeutigen Namen. |
    |Region|Wählen Sie **WestCentralUS** aus.|
    |||
5. Klicken Sie auf **Weiter: Ressource** aus.
6. Geben Sie unter **Privaten Endpunkt erstellen – Ressource** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    |Verbindungsmethode  | Wählen Sie das Herstellen einer Verbindung mit einer Azure-Ressource im eigenen Verzeichnis aus.|
    | Subscription| Wählen Sie Ihr Abonnement aus. |
    | Ressourcentyp | Wählen Sie **Microsoft.Sql/servers** aus. |
    | Resource |Wählen Sie *myServer* aus.|
    |Zielunterressource |Wählen Sie *sqlServer* aus.|
    |||
7. Klicken Sie auf **Weiter: Konfiguration** aus.
8. Geben Sie unter **Privaten Endpunkt erstellen (Vorschau) – Konfiguration** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    |**NETZWERK**| |
    | Virtuelles Netzwerk| Wählen Sie *MyVirtualNetwork* aus. |
    | Subnet | Wählen Sie *mySubnet* aus. |
    |**PRIVATE DNS-INTEGRATION**||
    |Integration in eine private DNS-Zone |Wählen Sie **Ja** aus. |
    |Private DNS-Zone |Wählen Sie *(New)privatelink.database.windows.net* aus. |
    |||

1. Klicken Sie auf **Überprüfen + erstellen**. Sie werden zur Seite **Überprüfen und erstellen** weitergeleitet, auf der Azure Ihre Konfiguration überprüft. 
2. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus. 
 
## <a name="connect-to-a-vm-using-remote-desktop-rdp"></a>Herstellen einer Verbindung mit einem virtuellen Computer mithilfe von Remotedesktop (RDP)


Stellen Sie nach der Erstellung von **myVm** über das Internet eine Verbindung mit diesem virtuellen Computer her: 

1. Geben Sie in der Suchleiste des Portals *myVm* ein.

1. Wählen Sie die Schaltfläche **Verbinden** aus. Nach dem Auswählen der Schaltfläche **Verbinden** wird **Verbindung mit virtuellem Computer herstellen** geöffnet.

1. Wählen Sie **RDP-Datei herunterladen** aus. Azure erstellt eine Remotedesktopprotokoll-Datei (*RDP*) und lädt sie auf Ihren Computer herunter.

1. Öffnen Sie die Datei *downloaded.rdp*.

    1. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus.

    1. Geben Sie den Benutzernamen und das Kennwort ein, den/das Sie beim Erstellen des virtuellen Computers angegeben haben.

        > [!NOTE]
        > Unter Umständen müssen Sie **Weitere Optionen** > **Anderes Konto verwenden** auswählen, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben.

1. Klicken Sie auf **OK**.

1. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wenn Sie eine Zertifikatwarnung erhalten, wählen Sie **Ja** oder **Weiter** aus.

1. Sobald der VM-Desktop angezeigt wird, minimieren Sie ihn, um zu Ihrem lokalen Desktop zurückzukehren.  

## <a name="access-sql-database-privately-from-the-vm"></a>Privates Zugreifen auf SQL-Datenbank über den virtuellen Computer

1. Öffnen Sie auf dem Remotedesktop von *myVM* PowerShell.

2. Geben Sie `nslookup myserver.database.windows.net` ein. 

    Sie erhalten eine Meldung wie die folgende:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    myserver.privatelink.database.windows.net
    Address:  10.0.0.5
    Aliases:   myserver.database.windows.net
    ```
3. Installieren Sie [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017).

4. Geben Sie unter **Mit Server verbinden** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | Wert |
    | ------- | ----- |
    | Servertyp| Wählen Sie **Datenbank-Engine** aus.|
    | Servername| Wählen Sie *myserver.database.windows.net* aus. |
    | Benutzername | Geben Sie einen Benutzernamen als username@servername ein, der während der Erstellung des SQL-Servers angegebenes Kennwort angegeben wird. |
    |Kennwort |Geben Sie ein während der Erstellung des SQL-Servers angegebenes Kennwort ein. |
    |Kennwort speichern|Wählen Sie **Ja** aus.|
    |||
1. Wählen Sie **Verbinden**.
2. Durchsuchen Sie Datenbanken im linken Menü.
3. (Optional) Erstellen Sie oder fragen Sie Informationen aus „mydatabase“ ab.
4. Schließen Sie die Remotedesktopverbindung mit *myVm*. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 
Wenn Sie Ihre Arbeit mit dem privaten Endpunkt, dem SQL-Server und dem virtuellen Computer abgeschlossen haben, löschen Sie die Ressourcengruppe und alle darin enthaltenen Ressourcen: 
1. Geben Sie oben im Portal im Feld **Suchen** die Zeichenfolge *myResourceGroup* ein, und wählen Sie in den Suchergebnissen den Eintrag *myResourceGroup* aus. 
2. Wählen Sie die Option **Ressourcengruppe löschen**. 
3. Geben Sie „myResourceGroup“ für **RESSOURCENGRUPPENNAMEN EINGEBEN** ein, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie einen virtuellen Computer in einem virtuellen Netzwerk, einen logischen SQL-Server und einen privaten Endpunkt für den privaten Zugriff erstellt. Sie haben aus dem Internet eine Verbindung mit einem virtuellen Computer hergestellt und über Private Link sicher mit SQL-Datenbank kommuniziert. Weitere Informationen zu privaten Endpunkten finden Sie unter [Was ist privater Endpunkt in Azure?](private-endpoint-overview.md).
