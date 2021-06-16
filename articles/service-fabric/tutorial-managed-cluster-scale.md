---
title: Aufskalieren eines verwalteten Service Fabric-Clusters
description: In diesem Tutorial erfahren Sie, wie Sie Knotentypen eines verwalteten Service Fabric-Clusters aufskalieren.
ms.topic: tutorial
ms.date: 5/10/2021
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 9e2bd57925ddb78dcfe23742b35c1490584558f8
ms.sourcegitcommit: df574710c692ba21b0467e3efeff9415d336a7e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110666875"
---
# <a name="tutorial-scale-out-a-service-fabric-managed-cluster"></a>Tutorial: Aufskalieren eines verwalteten Service Fabric-Clusters

In dieser Tutorialreihe wird Folgendes behandelt:

> [!div class="checklist"]
> * [Bereitstellen eines verwalteten Service Fabric-Clusters.](tutorial-managed-cluster-deploy.md)
> * Aufskalieren eines verwalteten Service Fabric-Clusters
> * [Hinzufügen und Entfernen von Knoten in einem verwalteten Service Fabric-Cluster](tutorial-managed-cluster-add-remove-node-type.md)
> * [Bereitstellen einer Anwendung in einem verwalteten Service Fabric-Cluster](tutorial-managed-cluster-deploy-app.md)

In diesem Teil der Reihe wird Folgendes behandelt:

> [!div class="checklist"]
> * Skalieren eines verwalteten Service Fabric-Clusterknotens

## <a name="prerequisites"></a>Voraussetzungen

* Ein verwalteter Service Fabric-Cluster (siehe [*Bereitstellen eines verwalteten Clusters*](tutorial-managed-cluster-deploy.md)).
* [Azure PowerShell 4.7.0](/powershell/azure/release-notes-azureps#azservicefabric) oder höher (siehe [*Installieren von Azure PowerShell*](/powershell/azure/install-az-ps)).

## <a name="scale-a-service-fabric-managed-cluster"></a>Skalieren eines verwalteten Service Fabric-Clusters
Ändern Sie die Instanzanzahl, um die Anzahl der Knoten für den Knotentyp zu erhöhen oder zu verringern, die Sie skalieren möchten. Sie finden Knotentypnamen in der ARM-Vorlage (Azure Resource Manager) aus der Clusterbereitstellung oder in Service Fabric Explorer.  

> [!NOTE]
> Wenn der Knotentyp auf einen primären Typ festgelegt ist, sind Sie nicht in der Lage, unter 3 Knoten für einen Cluster der SKU „Basic“ und unter 5 Knoten für einen Cluster der SKU „Standard“ zu skalieren.

```powershell
$resourceGroup = "myResourceGroup"
$clusterName = "mysfcluster"
$nodeTypeName = "FE"
$instanceCount = "7"

Set-AzServiceFabricManagedNodeType -ResourceGroupName $resourceGroup -ClusterName $clusterName -name $nodeTypeName -InstanceCount $instanceCount -Verbose
```

Das Upgrade des Clusters wird automatisch ausgeführt, und nach einigen Minuten werden die zusätzlichen Knoten angezeigt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schritt haben wir einen Knotentyp auf einem verwalteten Service Fabric-Cluster skaliert. Weitere Informationen zum Hinzufügen und Entfernen von Knotentypen finden Sie im folgenden Artikel:

> [!div class="nextstepaction"]
> [Hinzufügen und Entfernen von Knoten in einem verwalteten Service Fabric-Cluster](tutorial-managed-cluster-add-remove-node-type.md)
