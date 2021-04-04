---
title: Upgraden der Konfiguration eines eigenständigen Clusters
description: Erfahren Sie, wie Sie die Konfiguration upgraden, die einen eigenständigen Service Fabric-Cluster ausführt.
ms.topic: conceptual
ms.date: 11/09/2018
ms.openlocfilehash: 3cb1d40f5b32415588d3fd5a108967cfb4c0e534
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91842613"
---
# <a name="upgrade-the-configuration-of-a-standalone-cluster"></a>Upgraden der Konfiguration eines eigenständigen Clusters 

Für alle modernen Systeme ist die Möglichkeit eines Upgrades der Schlüssel zum langfristigen Erfolg des Produkts. Ein Azure Service Fabric-Cluster ist eine Ressource, die sich in Ihrem Besitz befindet. In diesem Artikel erfahren Sie, wie Sie die Konfigurationseinstellungen Ihres eigenständigen Service Fabric-Clusters upgraden.

## <a name="customize-cluster-settings-in-the-clusterconfigjson-file"></a>Anpassen von Clustereinstellungen in der Datei „ClusterConfig.json“
Eigenständige Cluster werden über die Datei *ClusterConfig.json* konfiguriert. Weitere Informationen zu den verschiedenen Einstellungen finden Sie unter [Konfigurationseinstellungen für einen eigenständigen Windows-Cluster](service-fabric-cluster-manifest.md).

Sie können Einstellungen im Abschnitt `fabricSettings` unter dem Abschnitt mit den [Clustereigenschaften](./service-fabric-cluster-manifest.md#cluster-properties) in *ClusterConfig.json* hinzufügen, aktualisieren oder entfernen. 

Beispiel: Mit dem folgende JSON-Code wird die neue Einstellung *MaxDiskQuotaInMB* zum Abschnitt *Diagnostics* unter `fabricSettings` hinzugefügt:

```json
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

Nachdem Sie die Einstellungen in der Datei „ClusterConfig.json“ geändert haben, [testen Sie die Clusterkonfiguration](#test-the-cluster-configuration), und [aktualisieren Sie dann die Clusterkonfiguration](#upgrade-the-cluster-configuration), um die Einstellungen auf Ihren Cluster anzuwenden. 

## <a name="test-the-cluster-configuration"></a>Testen der Clusterkonfiguration
Bevor Sie das Konfigurationsupgrade initiieren, können Sie die neue JSON-Datei für die Clusterkonfiguration durch Ausführen des folgenden PowerShell-Skripts im eigenständigen Paket testen:

```powershell
TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File>
```

Oder verwenden Sie dieses Skript:

```powershell
TestConfiguration.ps1 -ClusterConfigFilePath <Path to the new Configuration File> -OldClusterConfigFilePath <Path to the old Configuration File> -FabricRuntimePackagePath <Path to the .cab file which you want to test the configuration against>
```

Einige Konfigurationen können nicht aktualisiert werden, z.B. Endpunkte, Clusternamen, Knoten-IP-Adressen usw. Die neue JSON-Datei für die Clusterkonfiguration wird anhand der alten getestet, und es werden Fehler im PowerShell-Fenster ausgegeben, wenn es Probleme gibt.

## <a name="upgrade-the-cluster-configuration"></a>Upgraden der Clusterkonfiguration
Um die Clusterkonfiguration upzugraden, führen Sie den Befehl [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade) aus. Das Konfigurationsupgrade wird Upgradedomäne für Upgradedomäne verarbeitet.

```powershell
Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path to Configuration File>
```

## <a name="upgrade-cluster-certificate-configuration"></a>Aktualisieren der Clusterzertifikatkonfiguration
Für die Authentifizierung zwischen Clusterknoten wird ein Clusterzertifikat verwendet. Der Zertifikatrollover sollte daher mit besonderer Vorsicht ausgeführt werden, da Fehler die Kommunikation zwischen Clusterknoten blockieren könnten.

Vier Optionen werden unterstützt:  

* Einzelnes Zertifikatupgrade: Der Upgradepfad lautet „Zertifikat A (primär) > Zertifikat B (primär) > Zertifikat C (primär) > ...“.

* Doppeltes Zertifikatupgrade: Der Upgradepfad lautet „Zertifikat A (primär) > Zertifikat A (primär) und B (sekundär) > Zertifikat B (primär) > Zertifikat B (primär) und C (sekundär) > Zertifikat C (primär) > ...“.

* Zertifikattypupgrade: Fingerabdruckbasierte Zertifikatkonfiguration <-> auf allgemeinem Namen basierende Zertifikatkonfiguration. Beispiel: Zertifikatfingerabdruck A (primär) und Fingerabdruck B (sekundär) -> allgemeiner Name des Zertifikats C.

* Fingerabdruckupgrade des Zertifikatausstellers: Der Upgradepfad lautet „Allgemeiner Name des Zertifikats=A,IssuerThumbprint=IT1 (primär) -> Allgemeiner Name des Zertifikats=A,IssuerThumbprint=IT1,IT2 (primär) -> Allgemeiner Name des Zertifikats=A,IssuerThumbprint=IT2 (primär)“.


## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich über das [Anpassen von Service Fabric-Clustereinstellungen und der Fabric-Upgraderichtlinie](service-fabric-cluster-fabric-settings.md).
* Machen Sie sich mit der Vorgehensweise zum [Skalieren Ihres Clusters](service-fabric-cluster-scale-in-out.md) vertraut.
* Machen Sie sich mit [Anwendungsupgrades](service-fabric-application-upgrade.md) vertraut.

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
