---
title: 'Service Fabric-Clustersicherheit: Clientrollen'
description: Dieser Artikel beschreibt die zwei Clientrollen und die für die Rollen bereitgestellten Berechtigungen.
ms.topic: conceptual
ms.date: 2/23/2018
ms.openlocfilehash: abca19e686d39338fcaa2e0b0c8126913135170b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "75451898"
---
# <a name="role-based-access-control-for-service-fabric-clients"></a>Rollenbasierte Zugriffssteuerung für Service Fabric-Clients
Azure Service Fabric unterstützt zwei unterschiedliche Zugriffsberechtigungstypen für Clients, die mit einem Service Fabric-Cluster verbunden sind: Administrator und Benutzer. Zugriffssteuerung ermöglicht es dem Clusteradministrator, den Zugriff auf bestimmte Clustervorgänge für verschiedene Gruppen von Benutzern einzuschränken, wodurch die Sicherheit des Clusters erhöht wird.  

**Administratoren** haben Vollzugriff auf Verwaltungsfunktionen (einschließlich Lese-/Schreibzugriff). **Benutzer** haben standardmäßig nur Lesezugriff auf Verwaltungsfunktionen (z.B. Abfragefunktionen) sowie die Möglichkeit, Anwendungen und Dienste aufzulösen.

Sie geben die beiden Clientrollen (Administrator und Client) zum Zeitpunkt der Clustererstellung an, indem Sie für jede separate Zertifikate bereitstellen. Ausführliche Informationen zum Einrichten eines sicheren Service Fabric-Clusters finden Sie unter [Service Fabric-Clustersicherheit](service-fabric-cluster-security.md) .

## <a name="default-access-control-settings"></a>Standardeinstellungen für die Zugriffssteuerung
Der Administrator-Zugriffssteuerungstyp hat vollen Zugriff auf die FabricClient-APIs. Mit ihm können alle Lese- und Schreibvorgänge für den Service Fabric-Cluster ausgeführt werden, einschließlich der folgenden:

### <a name="application-and-service-operations"></a>Anwendungs- und Dienstvorgänge
* **CreateService**: Diensterstellung                             
* **CreateServiceFromTemplate**: Diensterstellung aus Vorlage                             
* **UpdateService**: Dienstaktualisierungen                             
* **DeleteService**: Löschen des Diensts                             
* **ProvisionApplicationType**: Bereitstellung von Anwendungstypen                             
* **CreateApplication**: Anwendungserstellung                               
* **DeleteApplication**: Löschen der Anwendung                             
* **UpgradeApplication**: Starten oder Unterbrechen von Anwendungsupgrades                             
* **UnprovisionApplicationType**: Aufheben der Bereitstellung von Anwendungstypen                             
* **MoveNextUpgradeDomain**: Fortsetzen von Anwendungsupgrades mit einer expliziten Updatedomäne                             
* **ReportUpgradeHealth**: Fortsetzen von Anwendungsupgrades beim aktuellen Aktualisierungsvorgang                             
* **ReportHealth**: Integritätsbericht                             
* **PredeployPackageToNode**: API vor der Bereitstellung                            
* **CodePackageControl**: Neustarten von Codepaketen                             
* **RecoverPartition**: Wiederherstellen einer Partition                             
* **RecoverPartitions**: Wiederherstellen von Partitionen                             
* **RecoverServicePartitions**: Wiederherstellen von Dienstpartitionen                             
* **RecoverSystemPartitions**: Wiederherstellen von Systemdienstpartitionen                             

### <a name="cluster-operations"></a>Clustervorgänge
* **ProvisionFabric**: MSI- und/oder Clustermanifest-Bereitstellung                             
* **UpgradeFabric**: Starten von Clusterupgrades                             
* **UnprovisionFabric**: Aufheben der MSI- und/oder Clustermanifest-Bereitstellung                         
* **MoveNextFabricUpgradeDomain**: Fortsetzen von Clusterupgrades mit einer expliziten Updatedomäne                             
* **ReportFabricUpgradeHealth**: Fortsetzen von Clusterupgrades beim aktuellen Aktualisierungsvorgang                             
* **StartInfrastructureTask**: Starten von Infrastrukturaufgaben                             
* **FinishInfrastructureTask**: Abschließen von Infrastrukturaufgaben                             
* **InvokeInfrastructureCommand**: Befehle zur Infrastrukturaufgabenverwaltung                              
* **ActivateNode**: Aktivieren eines Knotens                             
* **DeactivateNode**: Deaktivieren eines Knotens                             
* **DeactivateNodesBatch**: Deaktivieren mehrerer Knoten                             
* **RemoveNodeDeactivations**: Zurücksetzen der Deaktivierung mehrerer Knoten                             
* **GetNodeDeactivationStatus**: Überprüfung des Deaktivierungsstatus                             
* **NodeStateRemoved**: Bericht zur Entfernung des Knotenstatus                             
* **ReportFault**: Berichtsfehler                             
* **FileContent**: Imagespeicherclient-Dateiübertragung (außerhalb des Clusters)                             
* **FileDownload**: Imagespeicherclient-Dateidownloadinitiierung (außerhalb des Clusters)                             
* **InternalList**: Imagespeicherclient-Dateiauflistungsvorgang (intern)                             
* **Delete**: Imagespeicherclient-Löschvorgang                              
* **Upload**: Imagespeicherclient-Uploadvorgang                             
* **NodeControl**: Starten, Stoppen und Neustarten von Knoten                             
* **MoveReplicaControl**: Verschieben von Replikaten von einem Knoten auf einen anderen                             

### <a name="miscellaneous-operations"></a>Verschiedene Vorgänge
* **Ping**: Clientpingvorgänge                             
* **Query**: Alle Abfragen zulässig
* **NameExists**: Überprüfung auf vorhandene URI-Namen                             

Die Zugriffssteuerung vom Typ „Benutzer“ ist standardmäßig auf die folgenden Vorgänge beschränkt: 

* **EnumerateSubnames**: Benennen der URI-Enumeration                             
* **EnumerateProperties**: Benennen der Eigenschaftenenumeration                             
* **PropertyReadBatch**: Benennen der Lesevorgänge für Eigenschaften                             
* **GetServiceDescription**: Dienstbenachrichtigungen mit langen Abrufzeiten und Lesen von Dienstbeschreibungen                             
* **ResolveService**: Dienstauflösungen auf Konfliktbasis                             
* **ResolveNameOwner**: Auflösen des Namens-URI-Besitzers                             
* **ResolvePartition**: Auflösen von Systemdiensten                             
* **ServiceNotifications**: Dienstbenachrichtigungen auf Ereignisbasis                             
* **GetUpgradeStatus**: Abrufen des Anwendungsupgradestatus                             
* **GetFabricUpgradeStatus**: Abrufen des Clusterupgradestatus                             
* **InvokeInfrastructureQuery**: Abfragen von Infrastrukturaufgaben                             
* **List**: Imagespeicherclient-Dateiauflistungsvorgang                             
* **ResetPartitionLoad**: Zurücksetzen der Last für eine Failovereinheit                             
* **ToggleVerboseServicePlacementHealthReporting**: Umschalten von ausführlichen Integritätsberichten zur Dienstplatzierung                             

Die Zugriffssteuerung des Typs „Admin“ kann auch auf die zuvor genannten Vorgänge zugreifen.

## <a name="changing-default-settings-for-client-roles"></a>Ändern der Standardeinstellungen für Clientrollen
In der Manifestdatei des Clusters können bei Bedarf Administratorfunktionen für den Client bereitgestellt werden. Sie können die Standardeinstellungen ändern, indem Sie während der [Clustererstellung](service-fabric-cluster-creation-via-portal.md) die Option **Fabric-Einstellungen** auswählen und die zuvor beschriebenen Einstellungen in den Feldern **name**, **admin**, **user** und **value** angeben.

## <a name="next-steps"></a>Nächste Schritte
[Service Fabric-Clustersicherheit](service-fabric-cluster-security.md)

[Service Fabric-Clustererstellung](service-fabric-cluster-creation-via-portal.md)

