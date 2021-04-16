---
title: Der Clusterressourcen-Manager von Service Fabric – Verschiebungskosten
description: Erfahren Sie mehr über die Verschiebungskosten für Service Fabric-Dienste und wie diese für alle architektonischen Anforderungen (einschließlich dynamischer Konfigurationen) festgelegt werden können.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.custom: devx-track-csharp
ms.openlocfilehash: 0fdcfb02851d56ed996ae4bf32671ab545782733
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "89005342"
---
# <a name="service-movement-cost"></a>Kosten von Dienstverschiebungen
Ein Faktor bei den Überlegungen im Cluster Resource Manager von Service Fabric zu Veränderungen an einem Cluster sind die Kosten, die mit diesen Änderungen verbunden sind. Die „Kosten“ werden dabei gegen die mögliche Verbesserung des Clusters abgewogen. Die Kosten werden berücksichtigt, wenn Dienste zum Lastenausgleich, zur Defragmentierung und aufgrund anderer Anforderungen verschoben werden. Ziel ist es, die Anforderungen auf die am wenigsten störende und kostengünstigste Weise zu erfüllen.

Das Verschieben von Diensten kostet zumindest CPU-Zeit und Netzwerkbandbreite. Für zustandsbehaftete Dienste muss eine Kopie des Zustands der Dienste erstellt werden. Dies erfordert zusätzlichen Speicherplatz im Arbeitsspeicher und auf dem Datenträger. Durch Minimieren der Kosten von Lösungen, die vom Cluster Resource Manager in Azure Service Fabric bereitgestellt werden, kann sichergestellt werden, dass die Ressourcen des Clusters nicht unnötigerweise verbraucht werden. Sie möchten jedoch auch alle Lösungen kennen, die die Zuordnung von Ressourcen im Cluster erheblich verbessern würden.

Der Cluster Resource Manager bietet zwei Möglichkeiten, die Kosten zu berechnen und zu beschränken und gleichzeitig den Cluster möglichst optimal zu verwalten. Bei der ersten Möglichkeit wird einfach jede einzelne Verschiebung gezählt, die erforderlich ist. Wenn zwei Lösungen mit nahezu identischem Lastenausgleich (Bewertung) generiert werden, wird im Cluster Resource Manager die Lösung mit den geringsten Kosten (Gesamtzahl der Verschiebungen) bevorzugt.

Diese Strategie funktioniert sehr gut. Aber wie bei standardmäßigen oder statischen Lasten ist es in einem komplexen System unwahrscheinlich, dass alle Verschiebungen gleich sind. Einige sind wahrscheinlich viel teurer.

## <a name="setting-move-costs"></a>Festlegen der Verschiebungskosten 
Sie können die Standardverschiebungskosten für einen Dienst bei dessen Erstellung angeben:

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -DefaultMoveCost Medium
```

C#: 

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
//set up the rest of the ServiceDescription
serviceDescription.DefaultMoveCost = MoveCost.Medium;
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Sie können MoveCost für einen Dienst auch nach dessen Erstellung dynamisch angeben oder aktualisieren: 

PowerShell: 

```posh
Update-ServiceFabricService -Stateful -ServiceName "fabric:/AppName/ServiceName" -DefaultMoveCost High
```

C#:

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.DefaultMoveCost = MoveCost.High;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/AppName/ServiceName"), updateDescription);
```

## <a name="dynamically-specifying-move-cost-on-a-per-replica-basis"></a>Dynamisches Angeben der Verschiebungskosten pro Replikat

In sämtlichen vorangegangenen Codeausschnitten wird MoveCost direkt für einen gesamten Dienst außerhalb des Diensts angegeben. Verschiebungskosten erweisen sich jedoch als besonders nützlich, wenn sich die Verschiebungskosten eines bestimmten Dienstobjekts im Lauf seiner Lebensdauer ändern. Da die entsprechenden Kosten für die Verschiebung zu einem bestimmten Zeitpunkt wahrscheinlich am besten den jeweiligen Diensten zu entnehmen sind, liegt eine API für Dienste zum Erfassen der individuellen Verschiebungskosten während der Laufzeit vor. 

C#:

```csharp
this.Partition.ReportMoveCost(MoveCost.Medium);
```

## <a name="impact-of-move-cost"></a>Auswirkungen von Verschiebungskosten
MoveCost hat fünf Ebenen: Zero, Low, Medium, High und VeryHigh. Es gelten die folgenden Regeln:

* MoveCosts stehen zueinander in einem Verhältnis, mit Ausnahme von Zero und VeryHigh. 
* Zero bedeutet, dass das Verschieben keine Kosten generiert und die Bewertung der Lösung nicht negativ beeinflussen sollte.
* Das Umstellen der Kosten auf High oder VeryHigh bietet *keine* Garantie, dass das Replikat *nie* verschoben wird.
* Replikate mit Verschiebungskosten von VeryHigh werden nur verschoben, wenn eine Einschränkungsverletzung im Cluster vorliegt, die nicht auf andere Weise behoben werden kann (selbst wenn zum Beheben der Verletzung viele andere Replikate verschoben werden müssen).



<center>

![Verschiebungskosten als Faktor bei der Auswahl der zu verschiebenden Replikate][Image1]
</center>

MoveCost hilft Ihnen dabei, Lösungen zu finden, die insgesamt die geringsten Unterbrechungen verursachen, am leichtesten umzusetzen sind und dabei das beste Gleichgewicht versprechen. Die Kosten für einen Dienst können von Vielem abhängen. Die gängigsten Faktoren bei der Berechnung der Kosten von Verschiebungen sind:

- Die Menge von Zuständen oder Daten, die ein Dienst verschieben soll.
- Die Kosten der Trennung von Clients. Die Verschiebung eines primären Replikats ist normalerweise kostenintensiver als die Verschiebung eines sekundären Replikats.
- Die Kosten für Unterbrechungen einer sich in der Ausführung befindenden Operation. Einige Operationen auf Datenspeicherebene oder Operationen, die als Antwort auf einen Clientaufruf ausgeführt werden, sind kostenaufwendig. Ab einem bestimmten Punkt werden sie nicht mehr unnötigerweise freiwillig abgebrochen. Sie erhöhen daher während des Vorgangs die Kosten für das Verschieben dieses Dienstobjekts, um die Wahrscheinlichkeit zu verringern, dass es verschoben wird. Wenn der Vorgang abgeschlossen ist, können Sie die Kosten wieder auf den normalen Wert zurückstufen.

> [!IMPORTANT]
> Die Nutzung von VeryHigh-Verschiebungskosten sollte sorgfältig erwogen werden, da dabei erheblich die Fähigkeit des Clusterressourcen-Managers beeinträchtigt wird, eine global-optimale Platzierungslösung im Cluster zu ermitteln. Replikate mit Verschiebungskosten von VeryHigh werden nur verschoben, wenn eine Einschränkungsverletzung im Cluster vorliegt, die nicht auf andere Weise behoben werden kann (selbst wenn zum Beheben der Verletzung viele andere Replikate verschoben werden müssen).

## <a name="enabling-move-cost-in-your-cluster"></a>Aktivieren von Verschiebungskosten in Ihrem Cluster
Damit die differenzierten MoveCosts berücksichtigt werden können, muss MoveCost in Ihrem Cluster aktiviert werden. Ohne diese Einstellung wird der Standardmodus für das Zählen von Verschiebungen zur Berechnung von MoveCost verwendet. MoveCost-Berichte werden dagegen ignoriert.


ClusterManifest.xml

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="UseMoveCostReports" Value="true" />
        </Section>
```

Über „ClusterConfig.json“ für eigenständige Bereitstellungen bzw. über „Template.json“ für in Azure gehostete Cluster:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "UseMoveCostReports",
          "value": "true"
      }
    ]
  }
]
```

## <a name="next-steps"></a>Nächste Schritte
- Der Clusterressourcen-Manager von Service Fabric verwendet Metriken, um den Ressourcenverbrauch und die Kapazität im Cluster zu verwalten. Weitere Informationen zu Metriken und deren Konfiguration finden Sie unter [Verwalten von Ressourcenverbrauch und Auslastung in Service Fabric mit Metriken](service-fabric-cluster-resource-manager-metrics.md).
- Informationen darüber, wie der Clusterressourcen-Manager die Auslastung im Cluster verwaltet und verteilt, finden Sie unter [Lastenausgleich für Service Fabric-Cluster](service-fabric-cluster-resource-manager-balancing.md).

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png
