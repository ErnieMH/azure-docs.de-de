---
title: Remoteverbindung mit Azure Virtual Machines ist nicht möglich, da die NIC deaktiviert ist | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein Problem behandeln, bei dem für RDP ein Fehler auftritt, weil die Netzwerkschnittstelle auf der Azure-VM deaktiviert ist | Microsoft-Dokumentation
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/12/2018
ms.author: genli
ms.openlocfilehash: 0e79efc9de43fc0a3044e9ae1e3959f63bb6e69f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90090255"
---
#  <a name="cannot-remote-desktop-to-a-vm-because-the-network-interface-is-disabled"></a>Remotedesktopverbindung mit einer VM nicht möglich, weil die Netzwerkschnittstelle deaktiviert ist

In diesem Artikel erfahren Sie, wie Sie ein Problem beim Herstellen einer Remotedesktopverbindung mit Azure Virtual Machines (VMs) unter Windows beheben, wenn die Netzwerkschnittstelle deaktiviert ist.


## <a name="symptoms"></a>Symptome

Sie können keine RDP-Verbindung oder eine andere Art von Verbindung mit anderen Ports einer VM in Azure herstellen, weil die Netzwerkschnittstelle auf der VM deaktiviert ist.

![Screenshot: VM mit getrennter Netzwerkschnittstelle](./media/troubleshoot-rdp-nic-disabled/disconnected.png)

![Screenshot: VM mit deaktivierter Netzwerkschnittstelle](./media/troubleshoot-rdp-nic-disabled/disabled.png)


## <a name="solution"></a>Lösung

Erstellen Sie eine Momentaufnahme des Betriebssystemdatenträgers des betroffenen virtuellen Computers als Sicherung, bevor Sie die unten angegebenen Schritte ausführen. Weitere Informationen finden Sie unter [Erstellen einer Momentaufnahme eines Datenträgers](../windows/snapshot-copy-managed-disk.md).

Verwenden Sie zum Aktivieren der Schnittstelle für die VM die serielle Konsole, oder [setzen Sie die Netzwerkschnittstelle für die VM zurück](#reset-network-interface).

### <a name="use-serial-control"></a>Verwenden der seriellen Konsole

1. Stellen Sie eine Verbindung mit der [seriellen Konsole her, und öffnen Sie eine CMD-Instanz](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console). Wenn die serielle Konsole auf Ihrer VM nicht aktiviert ist, helfen Ihnen die Informationen unter [Zurücksetzen der Netzwerkschnittstelle](#reset-network-interface) weiter.
2. Überprüfen Sie den Status der Netzwerkschnittstelle:

    ```console
    netsh interface show interface
    ```

    Notieren Sie sich den Namen der deaktivierten Netzwerkschnittstelle.

3. Aktivieren Sie die Netzwerkschnittstelle:

    ```console
    netsh interface set interface name="interface Name" admin=enabled
    ```

    Führen Sie beispielsweise den folgenden Befehl aus, wenn die Netzwerkschnittstelle den Namen „Ethernet 2“ hat:

    ```console
    netsh interface set interface name="Ethernet 2" admin=enabled
    ```

4.  Überprüfen Sie den Status der Netzwerkschnittstelle erneut, um sicherzustellen, dass sie aktiviert ist.

    ```console
    netsh interface show interface
    ```

    Sie müssen die VM an diesem Punkt nicht neu starten. Die VM ist jetzt wieder erreichbar.

5.  Stellen Sie eine Verbindung mit dem virtuellen Computer her, und prüfen Sie, ob das Problem behoben wurde.

## <a name="reset-network-interface"></a>Zurücksetzen der Netzwerkschnittstelle

Ändern Sie zum Zurücksetzen der Netzwerkschnittstelle die IP-Adresse in eine andere, im Subnetz verfügbare IP-Adresse. Verwenden Sie hierzu das Azure-Portal oder Azure PowerShell. Weitere Informationen finden Sie unter [Zurücksetzen der Netzwerkschnittstelle](reset-network-interface.md).
