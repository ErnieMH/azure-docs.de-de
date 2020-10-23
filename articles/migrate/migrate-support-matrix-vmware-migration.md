---
title: Unterstützung der VMware-Migration in Azure Migrate
description: Erfahren Sie etwas über die Unterstützung der Migration virtueller VMware-Computer in Azure Migrate.
ms.topic: conceptual
ms.date: 06/08/2020
ms.openlocfilehash: 87733cac23d0336e4b9319f2a325e8d844e6e5b2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91651954"
---
# <a name="support-matrix-for-vmware-migration"></a>Unterstützungsmatrix für die VMware-Migration

Dieser Artikel enthält eine Zusammenfassung der Unterstützungseinstellungen und Einschränkungen bei der Migration virtueller VMware-Computer mithilfe von [Azure Migrate: Servermigration](migrate-services-overview.md#azure-migrate-server-migration-tool). Informationen zur Bewertung von virtuellen VMware-Computern bei der Migration zu Azure finden Sie in der [Unterstützungsmatrix für die Bewertung](migrate-support-matrix-vmware.md).


## <a name="migration-options"></a>Migrationsoptionen

Es gibt mehrere Möglichkeiten, virtuelle VMware-Computer zu migrieren:

- **Verwendung der Migration ohne Agent**: Migrieren von VMs, ohne dass darauf Software installiert werden muss Bei der Migration ohne Agent stellen Sie die [Azure Migrate-Appliance](migrate-appliance.md) bereit.
- **Verwendung der Agent-basierten Migration**: Installieren eines Agents für die Replikation auf der VM Bei der Agent-basierten Migration stellen Sie eine [Replikationsappliance](migrate-replication-appliance.md) bereit.

In [diesem Artikel](server-migrate-overview.md) finden Sie Informationen zur Verwendung dieser beiden Methoden.

## <a name="migration-limitations"></a>Einschränkungen bei der Migration

- Sie können bis zu 10 VMs gleichzeitig für die Replikation auswählen. Wenn Sie weitere Computer migrieren möchten, replizieren Sie sie in Gruppen von 10.
- Bei der VMware-Migration ohne Agent können Sie bis zu 300 Replikationsvorgänge gleichzeitig ausführen.

## <a name="agentless-migration"></a>Migration ohne Agent 

In diesem Abschnitt werden die Anforderungen für die Migration ohne Agent zusammengefasst.

### <a name="vmware-requirements-agentless"></a>VMware-Anforderungen (ohne Agent)

In der Tabelle werden die VMware-Hypervisor-Anforderungen zusammengefasst.

**VMware** | **Details**
--- | ---
**VMware vCenter Server** | Version 5.5, 6.0, 6.5, 6.7 und 7.0
**VMware vSphere ESXI-Host** | Version 5.5, 6.0, 6.5, 6.7 und 7.0
**vCenter Server-Berechtigungen** | Bei der Migration ohne Agent wird die [Migrate-Appliance](migrate-appliance.md) verwendet. Für die Appliance sind folgende Berechtigungen in vCenter Server erforderlich:<br/><br/> - **Datastore.Browse:** Ermöglicht das Durchsuchen von VM-Protokolldateien zur Fehlerbehebung bei der Erstellung und Löschung von Momentaufnahmen.<br/><br/> - **Datastore.FileManagement**: Ermöglicht Lese-/Schreib-/Lösch-/Umbenennungsvorgänge im Datenspeicherbrowser, um Probleme bei der Erstellung und Löschung von Momentaufnahmen zu beheben.<br/><br/> - **VirtualMachine.Config.ChangeTracking**: Ermöglicht die Aktivierung oder Deaktivierung der Änderungsverfolgung von VM-Datenträgern, um geänderte Datenblöcke zwischen Momentaufnahmen abzurufen.<br/><br/> - **VirtualMachine.Config.DiskLease**: Ermöglicht Datenträger-Leasevorgänge für eine VM, damit der Datenträger mit dem VMware vSphere Virtual Disk Development Kit (VDDK) gelesen werden kann.<br/><br/> - **VirtualMachine.Provisioning.DiskAccess**: (insbesondere für vSphere 6.0 und höher) ermöglicht das Öffnen eines Datenträgers auf einem virtuellen Computer für den zufälligen Lesezugriff auf den Datenträger mit dem VDDK.<br/><br/> - **VirtualMachine.Provisioning.DiskRandomRead**: Ermöglicht das Öffnen eines Datenträgers auf einer VM, damit der Datenträger mit dem VDDK gelesen werden kann.<br/><br/> - **VirtualMachine.Provisioning.DiskRandomAccess**: Ermöglicht das Öffnen eines Datenträgers auf einer VM, damit der Datenträger mit dem VDDK gelesen werden kann.<br/><br/> - **VirtualMachine.Provisioning.GetVmFiles**: Ermöglicht Lesevorgänge für Dateien, die einer VM zugeordnet sind, um die Protokolle herunterzuladen und ggf. Fehler zu beheben.<br/><br/> - **VirtualMachine.State.\*** : Ermöglicht das Erstellen und Verwalten von VM-Momentaufnahmen zur Replikation.



### <a name="vm-requirements-agentless"></a>- **VirtualMachine.Interact.PowerOff:** Ermöglicht das Ausschalten der VM während der Migration zu Azure.

VM-Anforderungen (ohne Agent)

In der Tabelle werden die Migrationsanforderungen ohne Agent für VMware-VMs zusammengefasst. | **Unterstützung**
--- | ---
**Details** | **Unterstützte Betriebssysteme**
Sie können alle [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)- und [Linux](../virtual-machines/linux/endorsed-distros.md)-Betriebssysteme migrieren, die von Azure unterstützt werden. | **Windows-VMs in Azure** 
Möglicherweise müssen Sie vor der Migration [einige Änderungen](prepare-for-migration.md#verify-required-changes-before-migrating) an VMs vornehmen. | **Linux-VMs in Azure**<br/><br/> Einige VMs erfordern möglicherweise Änderungen, damit sie in Azure ausgeführt werden können.<br/> Bei diesen Linux-Betriebssystemen führt Azure Migrate diese Änderungen automatisch durch:<br/> – Red Hat Enterprise Linux 7.8, 7.7, 7.6, 7.5, 7.4, 7.0 und 6.x</br> – Cent OS 7.7, 7.6, 7.5, 7.4 und 6.x<br/> - SUSE Linux Enterprise Server 12 SP1+ <br/>– SUSE Linux Enterprise Server 15 SP1<br/> – Ubuntu 19.04, 19.10, 14.04LTS, 16.04LTS und 18.04LTS <br/> – Debian 7, 8<br/> Oracle Linux 7.7, 7.7-CI
Bei anderen Betriebssystemen nehmen Sie die [erforderlichen Änderungen](prepare-for-migration.md#verify-required-changes-before-migrating) manuell vor. | **Linux-Start**<br/> Wenn sich „/boot“ in einer dedizierten Partition befindet, sollte diese auf dem Betriebssystemdatenträger und nicht auf mehrere Datenträger verteilt vorhanden sein.
Wenn „/boot“ Teil der Stammpartition („/“) ist, sollte sich diese auf dem Betriebssystemdatenträger befinden und nicht auf andere Datenträger erstrecken. | **UEFI-Start** Unterstützt. 
UEFI-basierte VMs werden zu Azure-VMs der 2. Generation migriert. | **Datenträgergröße**
2 TB für den Betriebssystemdatenträger (BIOS-Start); 4 TB für den Betriebssystemdatenträger (UEFI-Start); 8 TB für Datenträger |  **Einschränkungen für Datenträger**
Bis zu 60 Datenträger pro virtuellem Computer | **Verschlüsselte Datenträger/Volumes**
Die Migration von VMs mit verschlüsselten Datenträgern/Volumes wird nicht unterstützt. | **Freigegebener Datenträgercluster**
Wird nicht unterstützt. | **Unabhängige Datenträger**
Wird nicht unterstützt. | **RDM-Datenträger/Pass-Through-Datenträger**
Wenn VMs über RDM- oder Pass-Through-Datenträger verfügen, werden diese nicht in Azure repliziert. | **NFS**
NFS-Volumes, die als Volumes auf den VMs bereitgestellt sind, werden nicht repliziert. | **iSCSI-Ziele**
Virtuelle Computer mit iSCSI-Zielen werden bei der Migration ohne Agent nicht unterstützt. | **Multipfad-E/A**
Wird nicht unterstützt. | **Storage vMotion** Wird nicht unterstützt.
Die Replikation funktioniert nicht, wenn auf einem virtuellen Computer Storage vMotion verwendet wird. | **Kombinierte Netzwerkadapter**
Wird nicht unterstützt. | **IPv6**
Wird nicht unterstützt. | **Zieldatenträger**
Virtuelle Computer können nur zu verwalteten Datenträgern (HDD Standard, SSD Standard, SSD Premium) in Azure migriert werden. | **Gleichzeitige Replikation** 300 virtuelle Computer pro vCenter Server.


### <a name="appliance-requirements-agentless"></a>Bei mehr virtuellen Computern sind diese in Batches von 300 zu migrieren.

Applianceanforderungen (ohne Agent) Bei der Migration ohne Agent wird die [Azure Migrate-Appliance](migrate-appliance.md) verwendet.

- Sie können die Appliance mithilfe einer in vCenter Server importierten OVA-Vorlage oder mit einem [PowerShell-Skript](deploy-appliance-script.md) als VMware-VM bereitstellen.
- Erfahren Sie etwas über die [Anforderungen für die Appliance](migrate-appliance.md#appliance---vmware) für VMware.
- Informationen zu den URLs, auf die die Appliance Zugriff benötigt, finden Sie unter [URLs für die öffentliche Cloud](migrate-appliance.md#public-cloud-urls) und [URLs für Azure Government-Clouds](migrate-appliance.md#government-cloud-urls).

### <a name="port-requirements-agentless"></a>In Azure Government müssen Sie die Appliance [mithilfe des Skripts](deploy-appliance-script-government.md) bereitstellen.

Portanforderungen (ohne Agent) | **Device**
--- | ---
**Connection** | Appliance
Ausgehende Verbindungen an Port 443 zum Hochladen von replizierten Daten in Azure und zur Kommunikation mit Azure Migrate-Diensten zum Orchestrieren der Replikation und der Migration. | vCenter-Server
Eingehende Verbindungen an Port 443 zum Orchestrieren der Replikation durch die Appliance – Erstellen von Momentaufnahmen, Kopieren von Daten, Freigeben von Momentaufnahmen. | vSphere/ESXI-Host

## <a name="agent-based-migration"></a>Eingehend an TCP-Port 902 zum Replizieren der Daten aus Momentaufnahmen durch die Appliance 


Agent-basierte Migration


### <a name="vmware-requirements-agent-based"></a>In diesem Abschnitt werden die Anforderungen für die Agent-basierte Migration zusammengefasst.

VMware-Anforderungen (Agent-basiert)

In dieser Tabelle sind die Unterstützung und Einschränkungen der Bewertung für VMware-Virtualisierungsserver zusammengefasst. | **VMware-Anforderungen**
--- | ---
**Details** | **VMware vCenter Server**
Version 5.5, 6.0, 6.5 oder 6.7 | **VMware vSphere ESXI-Host**
Version 5.5, 6.0, 6.5 oder 6.7 | **vCenter Server-Berechtigungen**

### <a name="vm-requirements-agent-based"></a>Ein schreibgeschütztes Konto für vCenter Server.

VM-Anforderungen (Agent-basiert)

Die Tabelle enthält eine Übersicht über die Unterstützung für virtuelle VMware-Computer, die mit der Agent-basierten Migration migriert werden sollen. | **Unterstützung**
--- | ---
**Details** | **Computerworkload**
Azure Migrate unterstützt die Migration beliebiger Workloads (u. a. Active Directory- und SQL Server-Workloads), die auf einem unterstützten Computer ausgeführt werden. | **Betriebssysteme** Die neuesten Informationen finden Sie im Abschnitt zur [Betriebssystemunterstützung](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines) für Site Recovery.
Azure Migrate bietet identische VM-Betriebssystemunterstützung. | **Linux-Dateisystem/-Gastspeicher** Die neuesten Informationen finden Sie im Abschnitt zur [Linux-Dateisystemunterstützung](../site-recovery/vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) für Site Recovery.
Azure Migrate hat die gleiche Linux-Dateisystemunterstützung. | **Netzwerk/Speicher** Die neuesten Informationen finden Sie in den Bedingungen für [Netzwerk](../site-recovery/vmware-physical-azure-support-matrix.md#network) und [Speicher](../site-recovery/vmware-physical-azure-support-matrix.md#storage) für Site Recovery.
Für Azure Migrate gelten die gleichen Netzwerk-/Speicheranforderungen. | **Anforderungen für Azure** Die neuesten Informationen finden Sie in den Bedingungen für [Azure-Netzwerk](../site-recovery/vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover), [Speicher](../site-recovery/vmware-physical-azure-support-matrix.md#azure-storage) und [Compute](../site-recovery/vmware-physical-azure-support-matrix.md#azure-compute) für Site Recovery.
Für Azure Migrate gelten die gleichen Anforderungen für die VMware-Migration. | **Mobilitätsdienst**
Der Mobilitätsdienst-Agent muss auf jeder VM installiert sein, die Sie migrieren möchten. | **UEFI-Start** Unterstützt. 
UEFI-basierte VMs werden zu Azure-VMs der 2. Generation migriert.         | **UEFI: Sicherer Start**
Nicht unterstützt für die Migration. | **Zieldatenträger**
Virtuelle Computer können nur zu verwalteten Datenträgern (HDD Standard, SSD Standard, SSD Premium) in Azure migriert werden. | **Datenträgergröße**
2 TB für den Betriebssystemdatenträger (BIOS-Start); 4 TB für den Betriebssystemdatenträger (UEFI-Start); 8 TB für Datenträger |  **Einschränkungen für Datenträger**
Bis zu 63 Datenträger pro virtuellem Computer | **Verschlüsselte Datenträger/Volumes**
Die Migration von VMs mit verschlüsselten Datenträgern/Volumes wird nicht unterstützt. | **Freigegebener Datenträgercluster**
Wird nicht unterstützt. | **Unabhängige Datenträger**
Unterstützt. | **Pass-Through-Datenträger**
Unterstützt. | **NFS**
NFS-Volumes, die als Volumes auf den VMs bereitgestellt sind, werden nicht repliziert. | **iSCSI-Ziele**
Unterstützt. | **Multipfad-E/A**
Wird nicht unterstützt. | **Storage vMotion**
Unterstützt | **Kombinierte Netzwerkadapter**
Wird nicht unterstützt. | **IPv6**




### <a name="appliance-requirements-agent-based"></a>Wird nicht unterstützt.

Applianceanforderungen (Agent-basiert) Wenn Sie die Replikationsappliance mit der OVA-Vorlage im Azure Migrate-Hub einrichten, wird die Appliance unter Windows Server 2016 ausgeführt und erfüllt die Supportanforderungen.

- Stellen Sie bei der manuellen Einrichtung der Replikationsappliance auf einem physischen Server sicher, dass sie die Anforderungen erfüllt.
- Erfahren Sie etwas über die [Anforderungen für die Replikationsappliance](migrate-replication-appliance.md#appliance-requirements) für VMware. MySQL muss auf der Appliance installiert sein.
- Erfahren Sie etwas über die [Installationsoptionen ](migrate-replication-appliance.md#mysql-installation).
- Informationen zu den URLs, auf die die Replikationsappliance Zugriff benötigt, finden Sie unter [URLs für die öffentliche Cloud](migrate-replication-appliance.md#url-access) und [URLs für Azure Government-Clouds](migrate-replication-appliance.md#azure-government-url-access).

### <a name="port-requirements-agent-based"></a>Überprüfen Sie die [URLs](migrate-replication-appliance.md#port-access), auf die die Replikationsappliance zugreifen können muss.

Portanforderungen (Agent-basiert) | **Device**
--- | ---
**Connection** | VMs<br/><br/> Der Mobilitätsdienst auf virtuellen Computern kommuniziert mit der lokalen Replikationsappliance (Konfigurationsserver) über den HTTPS-Port 443 für eingehenden Datenverkehr, um die Replikationsverwaltung auszuführen. Virtuelle Computer senden Replikationsdaten an den Prozessserver (der auf dem Konfigurationsserver ausgeführt wird) über Port HTTPS 9443 für eingehenden Datenverkehr.
Dieser Port kann geändert werden. | Replikationsappliance
Die Replikationsappliance orchestriert die Replikation mit Azure über Port HTTPS 443 für ausgehenden Datenverkehr. | Prozessserver<br/> Der Prozessserver empfängt Replikationsdaten, optimiert und verschlüsselt sie und sendet sie über den ausgehenden Port 443 an Azure Storage.

## <a name="azure-vm-requirements"></a>Der Prozessserver wird standardmäßig auf der Replikationsappliance ausgeführt.

Azure-VM-Anforderungen 

Alle lokalen VMs, die (mit Agent-basierter Migration oder Migration ohne Agent) in Azure repliziert werden, müssen die in dieser Tabelle zusammengefassten Anforderungen für Azure-VMs erfüllen. | **Komponente** 
--- | --- | ---
**Anforderungen** | Gastbetriebssystem<br/> Überprüft die unterstützten Betriebssysteme der virtuellen VMware-Computer für die Migration. 
Sie können alle Workloads migrieren, die unter einem unterstützten Betriebssystem ausgeführt werden. | Architektur des Gastbetriebssystems 
64 Bit. | Größe des Betriebssystem-Datenträgers 
Bis zu 2.048 GB. | Anzahl von Betriebssystem-Datenträgern 
1 | Anzahl von Datenträgern für Daten 
Maximal 64. | Datenträgergröße
Bis zu 8.095 GB | Netzwerkadapter
Es werden mehrere Adapter unterstützt. | Freigegebene VHD 
Wird nicht unterstützt. | Fiber-Channel-Datenträger 
Wird nicht unterstützt. | BitLocker<br/><br/> Wird nicht unterstützt.
BitLocker muss deaktiviert sein, bevor Sie den Computer migrieren. | Name des virtuellen Computers<br/><br/> 1 bis 63 Zeichen.<br/><br/> Ist auf Buchstaben, Zahlen und Bindestriche beschränkt. 
Der Computername muss mit einem Buchstaben oder einer Ziffer beginnen und enden. | Herstellen einer Verbindung nach der Migration: Windows<br/><br/> So stellen Sie nach der Migration eine Verbindung mit Azure-VMs unter Windows her:<br/><br/> – Aktivieren Sie vor der Migration RDP auf dem lokalen virtuellen Computer.<br/><br/> Stellen Sie sicher, dass TCP- und UDP-Regeln für das Profil **Öffentlich** hinzugefügt werden und dass RDP unter **Windows-Firewall** > **Zugelassene Apps** für alle Profile zugelassen ist.<br/><br/> Aktivieren Sie bei Site-to-Site-VPN-Zugriff das RDP unter **Windows-Firewall** -> **Zugelassene Apps und Features** für **private Netzwerke und Domänennetzwerke**. Achten Sie außerdem darauf, dass die SAN-Richtlinie des Betriebssystems auf **OnlineAll** festgelegt ist.
[Weitere Informationen](prepare-for-migration.md) | Herstellen einer Verbindung nach der Migration: Linux<br/><br/> So stellen Sie nach der Migration per SSH eine Verbindung mit Azure-VMs her:<br/><br/> Überprüfen Sie vor der Migration auf dem lokalen Computer, ob der Secure Shell-Dienst auf „Start“ festgelegt ist und ob die Firewallregeln eine SSH-Verbindung zulassen.<br/><br/> Lassen Sie nach dem Failover auf der Azure-VM eingehende Verbindungen am SSH-Port für die Netzwerksicherheitsgruppen-Regeln auf der VM, für die ein Failover ausgeführt wurde, sowie für das Azure-Subnetz zu, mit dem die VM verbunden ist.  


## <a name="next-steps"></a>Fügen Sie außerdem der VM eine öffentliche IP-Adresse hinzu.

Nächste Schritte
