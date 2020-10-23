---
title: Paralleles Ausführen von Aufgaben zur Optimierung von Computeressourcen
description: Steigern der Effizienz und Reduzieren von Kosten durch Verringern der Serverknotenzahl und Ausführen paralleler Aufgaben auf jedem Knoten in einem Azure Batch-Pool
ms.topic: how-to
ms.date: 10/08/2020
ms.custom: H1Hack27Feb2017, devx-track-csharp
ms.openlocfilehash: 3c3a81aa624ccc67c0f9e8ec23e5ef9b8e61c724
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91850998"
---
# <a name="run-tasks-concurrently-to-maximize-usage-of-batch-compute-nodes"></a>Gleichzeitige Ausführung von Tasks zur optimalen Nutzung von Batch-Computeknoten

Wenn Sie mehrere Aufgaben gleichzeitig in jedem Computeknoten in Ihrem Azure Batch-Pool ausführen, wird die Maximierung der Ressourcenauslastung auf einer kleineren Anzahl von Knoten im Pool möglich. Bei einigen Workloads kann dies zu kürzeren Auftragszeiten und niedrigeren Kosten führen.

In manchen Szenarien profitieren Sie davon, dass alle Ressourcen eines Knotens einer einzigen Aufgabe zugewiesen werden können. Dagegen profitieren Sie in vielen Situationen davon, diese Ressourcen von mehreren Aufgaben nutzen zu lassen:

* **Minimieren des Datenübertragung** , wenn Aufgaben Daten gemeinsam nutzen können. In diesem Szenario können Sie die Gebühren für die Datenübertragung deutlich reduzieren, indem Sie freigegebene Daten auf eine kleinere Anzahl Knoten kopieren und Aufgaben parallel an jedem Knoten ausführen. Dies trifft besonders dann zu, wenn die zu jedem Knoten zu kopierenden Daten zwischen verschiedenen geografischen Regionen übertragen werden müssen.
* **Maximieren der Speicherauslastung** , wenn Aufgaben viel Speicherplatz beanspruchen – jedoch nur für kurze Zeiträumen und zu unterschiedlichen Zeiten während der Ausführung. Sie können weniger, aber dafür größere Computeknoten mit mehr Arbeitsspeicher einsetzen, um solche Spitzen effizient zu bewältigen. Auf diesen Knoten werden dann mehrere Aufgaben parallel ausgeführt, doch jede Aufgabe kann den reichlich vorhandenen Arbeitsspeicher zu verschiedenen Zeiten nutzen.
* **Verringern der Beschränkungen der Knotenanzahl** , wenn die Kommunikation zwischen Knoten in einem Pool erforderlich ist. Derzeit sind Pools, die für die Kommunikation zwischen Knoten konfiguriert sind, auf 50 Computeknoten beschränkt. Wenn jeder Knoten in einem Pool Aufgaben parallel ausführen kann, kann also eine größere Anzahl von Aufgaben gleichzeitig ausgeführt werden.
* **Replizieren eines lokalen Computeclusters**, z.B. beim ersten Auslagern einer Compute-Umgebung in Azure. Wenn Ihre aktuelle lokale Lösung mehrere Aufgaben pro Computeknoten ausführt, können Sie die maximale Anzahl von Knotenaufgaben erhöhen, um diese Konfiguration besser widerzuspiegeln.

## <a name="example-scenario"></a>Beispielszenario
Um die Vorteile der parallelen Aufgabenausführung zu veranschaulichen, nehmen wir an, dass für Ihre Aufgabenanwendung CPU- und Speicheranforderungen vorliegen, sodass die Knotengröße [Standard\_D1](../cloud-services/cloud-services-sizes-specs.md) geeignet ist. Um den Auftrag in der geforderten Zeit abzuschließen, sind jedoch 1.000 dieser Knoten nötig.

Anstatt Standard\_D1-Knoten mit einem CPU-Kern zu verwenden, können Sie [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md)-Knoten mit je 16 Kernen verwenden und so die parallele Ausführung von Aufgaben ermöglichen. Daher könnte *etwa ein Sechzehntel der Knoten* verwendet werden – statt 1.000 Knoten wären nur 63 erforderlich. Wenn große Anwendungsdateien oder Verweisdaten für jeden Knoten erforderlich sind, werden Auftragsdauer und Effizienz außerdem erneut verbessert, da die Daten nur auf 63 Knoten kopiert werden.

## <a name="enable-parallel-task-execution"></a>Aktivieren der parallelen Aufgabenausführung
Sie konfigurieren die Computeknoten für die parallele Aufgabenausführung auf der Pool-Ebene. Bei der Batch-.NET-Bibliothek legen Sie die Eigenschaft [CloudPool.TaskSlotsPerNode][maxtasks_net] fest, wenn Sie einen Pool erstellen. Bei Verwendung der Batch-REST-API legen Sie das [taskSlotsPerNode][rest_addpool]-Element im Anforderungstext fest, wenn Sie einen Pool erstellen.

In Azure Batch können Sie die Anzahl von Aufgabenslots pro Knoten auf die bis zu vierfache Anzahl der Knotenkerne festlegen. Ist der Pool beispielsweise mit Knoten der Größe „Groß“ (vier Kerne) konfiguriert, kann für „ `taskSlotsPerNode` ” 16 festgelegt werden. Unabhängig von der Anzahl von Kernen, über die der Knoten verfügt, können maximal 256 Aufgabenslots pro Knoten existieren. Ausführliche Informationen zur Anzahl der Kerne für jede Knotengröße finden Sie unter [Größen für Clouddienste](../cloud-services/cloud-services-sizes-specs.md). Weitere Informationen zu den Grenzen des Dienstes finden Sie in [Kontingente und Einschränkungen für den Azure Batch-Dienst](batch-quota-limit.md).

> [!TIP]
> Berücksichtigen Sie unbedingt den Wert `taskSlotsPerNode`, wenn Sie für Ihren Pool eine [Formel für das automatische Skalieren][enable_autoscaling] erstellen. Beispielsweise könnte eine Formel zum Auswerten von `$RunningTasks` erheblich von einer Steigerung der Aufgaben pro Knoten betroffen sein. Weitere Informationen finden Sie unter [Automatisches Skalieren von Computeknoten in einem Azure Batch-Pool](batch-automatic-scaling.md) .
>
>

> [!NOTE]
> Sie können das Element `taskSlotsPerNode` und die [TaskSlotsPerNode][maxtasks_net]-Eigenschaft nur zum Zeitpunkt der Poolerstellung festlegen. Nach der Erstellung eines Pools können sie nicht mehr geändert werden.
>
>

## <a name="distribution-of-tasks"></a>Verteilung von Aufgaben
Wenn die Computeknoten in einem Pool Aufgaben parallel ausführen können, müssen Sie angeben, wie die Aufgaben auf die Knoten innerhalb des Pools verteilt werden sollen.

Mithilfe der [CloudPool.TaskSchedulingPolicy][task_schedule]-Eigenschaft können Sie angeben, dass Aufgaben gleichmäßig über alle Knoten im Pool zugewiesen werden sollen („Verteilen“). Oder Sie können angeben, dass jedem Knoten so viele Aufgaben wie möglich zugewiesen werden sollen, bevor sie einem anderen Knoten im Pool zugewiesen werden („Packen“).

Ein Beispiel dafür, wie wertvoll diese Eigenschaft ist: Sehen Sie sich den Pool aus [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md)-Knoten (im vorigen Beispiel) an, der mit dem [CloudPool.TaskSlotsPerNode][maxtasks_net]-Wert 16 konfiguriert wurde. Bei Konfiguration von [CloudPool.TaskSchedulingPolicy][task_schedule] mit einem [ComputeNodeFillType][fill_type] der Art *Pack* wird die Auslastung aller 16 Kerne für die einzelnen Knoten maximiert und ein [Pool mit automatischer Skalierung](batch-automatic-scaling.md) ermöglicht, der nicht verwendete Knoten (Knoten, denen keine Aufgaben zugewiesen sind) aus dem Pool löscht. Dies minimiert die Ressourcenverwendung und spart Geld.

## <a name="variable-slots-per-task"></a>Variable Slots pro Aufgabe
Aufgaben können mit der [CloudTask.RequiredSlots][taskslots_net]-Eigenschaft definiert werden, um anzugeben, wie viele Slots zur Ausführung auf einem Computeknoten erforderlich sind. Der Standardwert ist 1. Sie können variable Aufgabenslots festlegen, wenn Ihre Aufgaben unterschiedlich viele Ressourcen auf einem Computeknoten nutzen. So kann auf jedem Computeknoten eine angemessene Anzahl an Aufgaben gleichzeitig ausgeführt werden, ohne dass Systemressourcen wie CPU oder Arbeitsspeicher übermäßig beansprucht werden.

Ein Beispiel: Für einen Pool mit der Eigenschaft `taskSlotsPerNode = 8` können Sie CPU-intensive Aufgaben, die mehrere Kerne benötigen, mit `requiredSlots = 8` übermitteln und für andere Aufgaben `requiredSlots = 1` verwenden. Wenn diese gemischte Workload im Pool geplant wird, werden die CPU-intensiven Aufgaben exklusiv auf dem Computeknoten ausgeführt. Andere Aufgaben (bis zu acht) können gleichzeitig auf anderen Knoten ausgeführt werden. So können Sie die Workload auf mehrere Computeknoten verteilen und die Effizienz der Ressourcennutzung verbessern.

> [!TIP]
> Bei variablen Aufgabenslots kann es passieren, dass umfangreiche Aufgaben, die mehr Slots erfordern, vorübergehend nicht geplant werden können, weil auf keinem Computeknoten genügend Slots vorhanden sind. Dies gilt auch dann, wenn einige andere Knoten noch über ungenutzte Slots verfügen. Sie können die Auftragspriorität für diese Aufgaben erhöhen, um ihre Chance zu verbessern, verfügbare Slots auf Knoten zu erhalten.
>
> Der Batch-Dienst gibt ebenfalls [TaskScheduleFailEvent](batch-task-schedule-fail-event.md) aus,wenn die Ausführung einer Aufgabe nicht geplant werden kann. Die Planung wird so lange wiederholt, bis die erforderlichen Slots zur Verfügung stehen. Sie können auf dieses Ereignis lauschen, um mögliche Probleme mit nicht mehr reagierenden Aufgabenplanungen zu erkennen, und entsprechende Maßnahmen ergreifen.
>

> [!NOTE]
> Geben Sie für die `requiredSlots`-Eigenschaft der Aufgabe keinen größeren Wert an als für die `taskSlotsPerNode`-Eigenschaft des Pools. Dies würde dazu führen, dass die Aufgabe niemals ausgeführt werden kann. Zurzeit überprüft der Batch-Dienst diese Werte nicht, wenn Sie Aufgaben übermitteln, weil zum Zeitpunkt der Übermittlung möglicherweise noch keine Poolgrenze für den Auftrag festgelegt wurde oder weil der Auftrag durch Deaktivieren und erneutes Aktivieren in einen anderen Pool verschoben werden kann.
>

## <a name="batch-net-example"></a>Beispiel für Batch .NET
Die folgenden Codeausschnitte der [Batch .NET][api_net]-API zeigen, wie Sie einen Pool mit mehreren Aufgabenslots pro Knoten erstellen und eine Aufgabe mit den erforderlichen Slots übermitteln.

### <a name="create-pool"></a>Erstellen eines Pools
Dieser Codeausschnitt zeigt eine Anforderung zum Erstellen eines Pools aus vier Knoten mit vier zulässigen Aufgabenslots pro Knoten. Er gibt eine Richtlinie für die Aufgabenplanung vor, die besagt, dass jeder Knoten mit Aufgaben gefüllt werden soll, bevor diese den anderen Knoten im Pool zugewiesen werden. Weitere Informationen zum Hinzufügen von Pools mit der Batch .NET-API finden Sie unter [BatchClient.PoolOperations.CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicatedComputeNodes: 4
        virtualMachineSize: "standard_d1_v2",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));

pool.TaskSlotsPerNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

### <a name="create-task-with-required-slots"></a>Erstellen einer Aufgabe mit erforderlichen Slots
Dieser Codeausschnitt erstellt eine Aufgabe mit einem nicht standardmäßigen `requiredSlots`-Wert. Diese Aufgabe wird nur ausgeführt, wenn auf dem Computeknoten genügend freie Slots verfügbar sind.
```csharp
CloudTask task = new CloudTask(taskId, taskCommandLine)
{
    RequiredSlots = 2
};
```

### <a name="list-compute-nodes-with-counts-for-running-tasks-and-slots"></a>Auflisten von Computeknoten mit der Anzahl ausgeführter Aufgaben und Slots
Dieser Codeausschnitt listet alle Computeknoten im Pool auf und gibt die Anzahl der ausgeführten Aufgaben und Aufgabenslots pro Knoten aus.
```csharp
ODATADetailLevel nodeDetail = new ODATADetailLevel(selectClause: "id,runningTasksCount,runningTaskSlotsCount");
IPagedEnumerable<ComputeNode> nodes = batchClient.PoolOperations.ListComputeNodes(poolId, nodeDetail);

await nodes.ForEachAsync(node =>
{
    Console.WriteLine(node.Id + " :");
    Console.WriteLine($"RunningTasks = {node.RunningTasksCount}, RunningTaskSlots = {node.RunningTaskSlotsCount}");

}).ConfigureAwait(continueOnCapturedContext: false);
```

### <a name="list-task-counts-for-the-job"></a>Auflisten der Anzahl von Aufgaben für den Auftrag
Dieser Codeausschnitt ruft die Anzahl von Aufgaben für den Auftrag ab. Diese Zahl umfasst sowohl Aufgaben als auch Aufgabenslots pro Aufgabenzustand.
```csharp
TaskCountsResult result = await batchClient.JobOperations.GetJobTaskCountsAsync(jobId);

Console.WriteLine("\t\tActive\tRunning\tCompleted");
Console.WriteLine($"TaskCounts:\t{result.TaskCounts.Active}\t{result.TaskCounts.Running}\t{result.TaskCounts.Completed}");
Console.WriteLine($"TaskSlotCounts:\t{result.TaskSlotCounts.Active}\t{result.TaskSlotCounts.Running}\t{result.TaskSlotCounts.Completed}");
```

## <a name="batch-rest-example"></a>Beispiel für Batch REST
Dieser [Batch-REST][api_rest]-API-Ausschnitt zeigt eine Anforderung zum Erstellen eines Pools aus zwei großen Knoten mit maximal vier Aufgaben pro Knoten. Weitere Informationen zum Hinzufügen von Pools mit der REST-API finden Sie unter [Add a pool to an account][rest_addpool] (Hinzufügen eines Pools zu einem Konto).

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  },
  "targetDedicatedComputeNodes":2,
  "taskSlotsPerNode":4,
  "enableInterNodeCommunication":true,
}
```

Dieser Codeausschnitt zeigt eine Anforderung zum Hinzufügen einer Aufgabe mit einem nicht standardmäßigen `requiredSlots`-Wert. Diese Aufgabe wird nur ausgeführt, wenn auf dem Computeknoten genügend freie Slots verfügbar sind.
```json
{
  "id": "taskId",
  "commandLine": "bash -c 'echo hello'",
  "userIdentity": {
    "autoUser": {
      "scope": "task",
      "elevationLevel": "nonadmin"
    }
  },
  "requiredSLots": 2
}
```

## <a name="code-sample"></a>Codebeispiel
Das [ParallelNodeTasks][parallel_tasks_sample]-Projekt auf GitHub veranschaulicht die Verwendung der [CloudPool.TaskSlotsPerNode][maxtasks_net]-Eigenschaft.

Diese C#-Konsolenanwendung verwendet die [Batch-Bibliothek für .NET][api_net] zum Erstellen eines Pools mit einem oder mehreren Serverknoten. Sie führt eine konfigurierbare Anzahl an Aufgaben auf diesen Knoten aus, um eine variable Auslastung zu simulieren. Die Ausgabe der Anwendung gibt an, welcher Knoten welche Aufgabe ausgeführt hat. Die Anwendung liefert auch eine Zusammenfassung der Aufgabenparameter und der -dauer. Die Zusammenfassung der Ausgabe von zwei verschiedenen Ausführungen der Beispielanwendung wird unten angezeigt.

```
Nodes: 1
Node size: large
Task slots per node: 1
Max slots per task: 1
Tasks: 32
Duration: 00:30:01.4638023
```

Die erste Ausführung der Beispielanwendung zeigt, dass die Aufgabe bei Nutzung eines einzigen Knotens im Pool und einer Aufgabe pro Knoten mehr als 30 Minuten dauert.

```
Nodes: 1
Node size: large
Task slots per node: 4
Max slots per task: 1
Tasks: 32
Duration: 00:08:48.2423500
```

Die zweite Ausführung des Beispiels zeigt eine deutliche Verringerung der Aufgabendauer. Das liegt daran, dass der Pool mit vier Aufgaben pro Knoten konfiguriert wurde, was die parallele Aufgabenausführung in etwa einem Viertel der Zeit ermöglicht.

> [!NOTE]
> In der Aufgabedauer in den oben aufgeführten Zusammenfassungen ist die Erstellung des Pools nicht berücksichtigt. Jede der oben aufgeführten Aufgaben wurde an bereits erstellte Pools übermittelt, deren Serverknoten sich zum Übermittlungszeitpunkt im *Idle* -Status befanden.
>
>

## <a name="next-steps"></a>Nächste Schritte
### <a name="batch-explorer-heat-map"></a>Heat Map für Batch Explorer
Der [Batch Explorer][batch_labs] ist ein kostenloses eigenständiges Clienttool mit zahlreichen Features, das Sie beim Erstellen, Debuggen und Überwachen von Azure Batch-Anwendungen unterstützt. Der Batch Explorer enthält ein *Wärmebild*-Feature, das die Visualisierung der Taskausführung ermöglicht. Wenn Sie die Beispielanwendung [ParallelTasks][parallel_tasks_sample] ausführen, können Sie die Heat Map-Funktion für eine einfache Visualisierung der Ausführung paralleler Aufgaben auf jedem Knoten verwenden.


[api_net]: /dotnet/api/microsoft.azure.batch
[api_rest]: /rest/api/batchservice/
[batch_labs]: https://azure.github.io/BatchExplorer/
[cloudpool]: /dotnet/api/microsoft.azure.batch.cloudpool
[enable_autoscaling]: /rest/api/batchservice/pool/enableautoscale
[fill_type]: /dotnet/api/microsoft.azure.batch.common.computenodefilltype
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: /dotnet/api/microsoft.azure.batch.cloudpool
[rest_addpool]: /rest/api/batchservice/pool/add
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: /dotnet/api/microsoft.azure.batch.pooloperations
[task_schedule]: /dotnet/api/microsoft.azure.batch.cloudpool
[taskslots_net]: /dotnet/api/microsoft.azure.batch.cloudtask.requiredslots
