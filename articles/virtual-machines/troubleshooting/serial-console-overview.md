---
title: Serielle Azure-Konsole | Microsoft-Dokumentation
description: Über die serielle Azure-Konsole können Sie eine Verbindung mit Ihrem virtuellen Computer herstellen, wenn SSH und RDP nicht verfügbar sind.
services: virtual-machines
documentationcenter: ''
author: asinn826
manager: borisb
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm
ms.workload: infrastructure-services
ms.date: 02/10/2020
ms.author: alsin
ms.openlocfilehash: 28c5a3085d84b25deb7c5ee09a9c9cc4d7a06819
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87374064"
---
# <a name="azure-serial-console"></a>Serielle Azure-Konsole

Die serielle Konsole im Azure-Portal ermöglicht den Zugriff auf eine textbasierte Konsole für virtuelle Computer (VMs) und VM-Skalierungsgruppeninstanzen, die unter Linux oder Windows ausgeführt werden. Diese serielle Verbindung erfolgt über den seriellen ttyS0- oder COM1-Port des virtuellen Computers oder der VM-Skalierungsgruppeninstanz und ermöglicht Zugriff unabhängig vom Zustand des Netzwerks oder Betriebssystems. Auf die serielle Konsole kann nur über das Azure-Portal und von Benutzern zugegriffen werden, die mindestens über die Zugriffsrolle „Mitwirkender“ für die VM oder VM-Skalierungsgruppeninstanzen verfügen.

Die serielle Konsole funktioniert auf die gleiche Weise für VMs und VM-Skalierungsgruppeninstanzen. Deshalb beziehen sich alle Äußerungen bezüglich VMs in dieser Dokumentation, sofern nicht anders angegeben, implizit auch auf VM-Skalierungsgruppeninstanzen.

Die serielle Konsole ist in den globalen Azure-Regionen allgemein und in Azure Government als Public Preview verfügbar. Sie ist noch nicht in der Cloud „Azure China“ verfügbar.

## <a name="prerequisites-to-access-the-azure-serial-console"></a>Voraussetzungen für den Zugriff auf die serielle Azure-Konsole
Für den Zugriff auf die serielle Konsole auf dem virtuellen Computer oder in der VM-Skalierungsgruppeninstanz gelten folgende Voraussetzungen:

- Die Startdiagnose muss für den virtuellen Computer aktiviert sein.
- Auf dem virtuellen Computer muss ein Benutzerkonto vorhanden sein, bei dem die Kennwortauthentifizierung verwendet wird. Einen kennwortbasierten Benutzer können Sie mit der Funktion [Kennwort zurücksetzen](../extensions/vmaccess.md#reset-password) der Erweiterung für den Zugriff auf virtuelle Computer erstellen. Wählen Sie im Abschnitt **Support + Problembehandlung** **Kennwort zurücksetzen** aus.
- Das Azure-Konto, über das auf die serielle Konsole zugegriffen wird, muss die Rolle [Mitwirkender für virtuelle Computer](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) für den virtuellen Computer sowie für das Speicherkonto der [Startdiagnose](boot-diagnostics.md) aufweisen.
- Klassische Bereitstellungen werden nicht unterstützt. Der virtuelle Computer oder die VM-Skalierungsgruppeninstanz muss das Azure Resource Manager-Bereitstellungsmodell verwenden.

> [!NOTE]
> Zurzeit ist die serielle Konsole nicht mit verwalteten Speicherkonten für die Startdiagnose kompatibel. Stellen Sie zum Verwenden der seriellen Konsole sicher, dass Sie ein benutzerdefiniertes Speicherkonto verwenden.

## <a name="get-started-with-the-serial-console"></a>Erste Schritte mit der seriellen Konsole
Auf die serielle Konsole für VMs und VM-Skalierungsgruppen kann nur über das Azure-Portal zugegriffen werden:

### <a name="serial-console-for-virtual-machines"></a>Serielle Konsole für virtuelle Computer
Für den Zugriff auf die serielle Konsole für VMs müssen Sie lediglich auf **Serielle Konsole** im Abschnitt **Support + Problembehandlung** des Azure-Portals klicken.
  1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).

  1. Navigieren Sie zu **Alle Ressourcen**, und wählen Sie eine VM aus. Die Übersichtsseite für die VM wird geöffnet.

  1. Scrollen Sie nach unten zum Abschnitt **Support + Problembehandlung**, und wählen Sie **Serielle Konsole** aus. Ein neuer Bereich mit der seriellen Konsole wird geöffnet, und die Verbindung wird hergestellt.

     ![Fenster der seriellen Linux-Konsole](./media/virtual-machines-serial-console/virtual-machine-linux-serial-console-connect.gif)

### <a name="serial-console-for-virtual-machine-scale-sets"></a>Serielle Konsole für Virtual Machine Scale Sets
Die serielle Konsole ist für VM-Skalierungsgruppen verfügbar und in jeder Instanz innerhalb der Skalierungsgruppe zugänglich. Sie müssen zur individuellen Instanz einer VM-Skalierungsgruppe navigieren, um die Schaltfläche **Serielle Konsole** zu finden. Wenn die Startdiagnose nicht für Ihre VM-Skalierungsgruppe aktiviert ist, stellen Sie sicher, dass Sie Ihr VM-Skalierungsgruppenmodell aktualisieren, um die Startdiagnose zu aktivieren, und führen Sie das Upgrades für alle Instanzen auf das neue Modell durch, um auf die serielle Konsole zugreifen zu können.
  1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).

  1. Navigieren Sie zu **Alle Ressourcen**, und wählen Sie eine VM-Skalierungsgruppe aus. Die Übersichtsseite für die VM-Skalierungsgruppe wird geöffnet.

  1. Navigieren Sie zu **Instanzen**.

  1. Wählen Sie eine VM-Skalierungsgruppeninstanz aus.

  1. Klicken Sie im Abschnitt **Support + Problembehandlung** auf **Serielle Konsole**. Ein neuer Bereich mit der seriellen Konsole wird geöffnet, und die Verbindung wird hergestellt.

     ![Serielle Konsole für Linux-VM-Skalierungsgruppe](./media/virtual-machines-serial-console/vmss-start-console.gif)


### <a name="tls-12-in-serial-console"></a>TLS 1.2 in der seriellen Konsole
Die serielle Konsole verwendet an jedem Punkt TLS 1.2, um die gesamte Kommunikation im Dienst abzusichern. Die serielle Konsole weist eine Abhängigkeit von einem vom Benutzer verwalteten Speicherkonto zur Startdiagnose auf. Außerdem muss TLS 1.2 für das Speicherkonto separat konfiguriert werden. Anweisungen dazu finden Sie [hier](../../storage/common/transport-layer-security-configure-minimum-version.md).

## <a name="advanced-uses-for-serial-console"></a>Erweiterte Nutzung der seriellen Konsole
Neben dem Konsolenzugriff auf Ihren virtuellen Computer können Sie die serielle Azure-Konsole für Folgendes verwenden:
* Senden eines [SysRq-Befehls an den virtuellen Computer](./serial-console-nmi-sysrq.md)
* Senden eines [nicht maskierbaren Interrupts an den virtuellen Computer](./serial-console-nmi-sysrq.md)
* Ordnungsgemäßer [Neustart oder erzwungenes Aus- und Einschalten des virtuellen Computers](./serial-console-power-options.md)


## <a name="next-steps"></a>Nächste Schritte
Zusätzliche Dokumentation zur seriellen Konsole ist über die Seitenleiste verfügbar.
- Weitere Informationen zur [seriellen Konsole für virtuelle Linux-Computer](./serial-console-linux.md)
- Weitere Informationen zur [seriellen Konsole für virtuelle Windows-Computer](./serial-console-windows.md)
