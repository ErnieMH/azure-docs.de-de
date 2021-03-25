---
title: CloudSimple-Wartung und -Updates
titleSuffix: Azure VMware Solution by CloudSimple
description: Beschreibt den CloudSimple-Dienstprozess für planmäßige Wartung und Updates.
author: sharaths-cs
ms.author: dikamath
ms.date: 03/09/2021
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 1cbd708f6c1f5159655099d5d72900ab0c608147
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102522307"
---
# <a name="cloudsimple-maintenance-and-updates"></a>CloudSimple-Wartung und -Updates

Die private Cloudumgebung ist so konzipiert, dass sie keinen Single Point of Failure aufweist.

* ESXi-Cluster sind mit vSphere-Hochverfügbarkeit (High-Availability, HA) konfiguriert. Die Cluster sind so bemessen, dass sie mindestens einen Ersatzknoten für die Ausfallsicherheit aufweisen.
* Redundanter Primärspeicher wird von vSAN bereitgestellt. Dafür werden mindestens drei Knoten benötigt, um Schutz vor einem einzigen Ausfall zu bieten. vSAN kann konfiguriert werden, um eine höhere Resilienz für größere Cluster zu gewährleisten.
* vCenter-, PSC- und NSX Manager-VMs sind mit einem RAID-10-Speicher konfiguriert, um sie vor Speicherausfällen zu schützen. Die VMs sind durch vSphere-Hochverfügbarkeit gegen Knoten-/Netzausfälle geschützt.
* ESXi-Hosts haben redundante Lüfter und NICs.
* Tor- und Spine-Switches sind paarweise für Hochverfügbarkeit konfiguriert, um Resilienz zu gewährleisten.

CloudSimple überwacht kontinuierlich die folgenden VMs auf Betriebszeit und Verfügbarkeit und bietet Verfügbarkeits-SLAs:

* ESXi-Hosts
* vCenter
* PSC
* NSX Manager

CloudSimple überwacht auch die folgenden Komponenten ständig auf Fehler:

* Festplatten
* Physische NIC-Ports
* Server
* Lüfter
* Power
* Switches
* Switchports

Wenn ein Datenträger oder Knoten ausfällt, wird automatisch ein neuer Knoten zu dem betroffenen VMware-Cluster hinzugefügt, um ihn sofort wieder in den ordnungsgemäßen Zustand zu versetzen.

CloudSimple sichert, wartet und aktualisiert die folgenden VMware-Elemente in den privaten Clouds:

* ESXi
* VCenter-Plattformdienste
* Controller
* vSAN
* NSX

## <a name="back-up-and-restore"></a>Sichern und Wiederherstellen

Die CloudSimple-Sicherung umfasst:

* Nächtliche inkrementelle Sicherungen von vCenter-, PSC- und DVS-Regeln.
* Native vCenter-APIs zur Sicherung von Komponenten auf Anwendungsebene.
* Automatische Sicherung vor Updates oder Upgrades der VMware-Verwaltungssoftware.
* vCenter-Datenverschlüsselung an der Quelle vor der Datenübertragung über einen mit TLS1.2 verschlüsselten Kanal an Azure. Die Daten werden in einem Azure-Blob gespeichert, wo sie über Regionen hinweg repliziert werden.

Sie können eine Wiederherstellung anfordern, indem Sie eine [Supportanfrage](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) stellen.

## <a name="maintenance"></a>Wartung

CloudSimple führt mehrere Arten von geplanten Wartungsmaßnahmen durch.

### <a name="backendinternal-maintenance"></a>Wartung von Back-End-/internen Komponenten

Diese Wartung beinhaltet in der Regel die Rekonfiguration physischer Objekte oder die Installation von Softwarepatches. Es hat keinen Einfluss auf die normale Nutzung der Objekte. Da sich redundante NICs in jedem physischen Rack befinden, sind der normale Netzwerkverkehr und der Betrieb der privaten Cloud nicht beeinträchtigt. Möglicherweise treten Leistungseinbußen nur dann auf, wenn Ihre Organisation davon ausgeht, die gesamte redundante Bandbreite während des Wartungsintervalls zu nutzen.

### <a name="cloudsimple-portal-maintenance"></a>CloudSimple-Portalwartung

Wenn die Steuerungsebene oder Infrastruktur von CloudSimple aktualisiert wird, sind begrenzte Ausfallzeiten erforderlich. Derzeit können die Wartungsintervalle einmal pro Monat stattfinden. Es wird erwartet, dass die Frequenz mit der Zeit abnimmt. CloudSimple benachrichtigt Sie, wenn eine Portalwartung stattfindet, und hält das Intervall so kurz wie möglich. Während eines Portal-Wartungsintervalls funktionieren die folgenden Dienste weiterhin ohne Beeinträchtigung:

* VMware-Verwaltungsebene und Anwendungen
* vCenter-Zugriff
* Alle Netzwerk und Speicher
* Der gesamte Azure-Datenverkehr

### <a name="vmware-infrastructure-maintenance"></a>VMware-Infrastrukturverwaltung

Gelegentlich ist es notwendig, Änderungen an der Konfiguration der VMware-Infrastruktur vorzunehmen.  Derzeit können diese Intervalle alle 1-2 Monate auftreten, aber es wird erwartet, dass die Häufigkeit mit der Zeit abnimmt. Diese Art der Wartung kann in der Regel ohne Unterbrechung der normalen Nutzung der CloudSimple-Dienste durchgeführt werden. Während eines VMware-Wartungsintervalls funktionieren die folgenden Dienste weiterhin ohne Beeinträchtigung:

* VMware-Verwaltungsebene und Anwendungen
* vCenter-Zugriff
* Alle Netzwerk und Speicher
* Der gesamte Azure-Datenverkehr

## <a name="updates-and-upgrades"></a>Updates und Upgrades

CloudSimple kümmert sich um die Lebenszyklusverwaltung der VMware-Software (ESXi, vCenter, PSC und NSX) in der privaten Cloud.

Softwareupdates umfassen:

* **Patches**. Sicherheitspatches oder Fehlerbehebungen, die von VMware freigegeben werden.
* **Updates**. Ändern der Nebenversion einer VMware-Stack-Komponente.
* **Upgrades**. Ändern der Hauptversion einer VMware-Stack-Komponente.

CloudSimple testet einen wichtigen Sicherheitspatch, sobald er von VMware verfügbar ist. 

Bis die nächsten geplanten Updates bereitgestellt werden, werden dokumentierte VMware-Problemumgehungen implementiert, anstatt einen entsprechenden Patch zu installieren. 

## <a name="next-steps"></a>Nächste Schritte

[Sichern von Workload-VMs mit Veeam](backup-workloads-veeam.md)
