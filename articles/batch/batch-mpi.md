---
title: Verwenden von Tasks mit mehreren Instanzen zum Ausführen von MPI-Anwendungen
description: Erfahren Sie, wie MPI-Anwendungen (Message Passing Interface) mithilfe des Tasktyps mit mehreren Instanzen in Azure Batch ausgeführt werden.
ms.topic: how-to
ms.date: 03/25/2021
ms.openlocfilehash: 02764f8dd8a6bb3e4224b8b44fe78ab7e15ba85d
ms.sourcegitcommit: 3f684a803cd0ccd6f0fb1b87744644a45ace750d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2021
ms.locfileid: "106219846"
---
# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-batch"></a>Verwenden von Tasks mit mehreren Instanzen zum Ausführen von MPI-Anwendungen (Message Passing Interface) in Batch

Tasks mit mehreren Instanzen ermöglichen die Ausführung eines Azure Batch-Tasks auf mehreren Computeknoten gleichzeitig. Diese Tasks machen High Performance Computing-Szenarien wie Message Passing Interface-Anwendungen (MPI) in Batch möglich. In diesem Artikel erfahren Sie, wie Tasks mit mehreren Instanzen mithilfe der [Batch-Bibliothek für .NET](/dotnet/api/microsoft.azure.batch) ausgeführt werden.

> [!NOTE]
> Die Beispiele in diesem Artikel konzentrieren sich auf Batch .NET, MS-MPI und Windows-Computeknoten, doch die vorgestellten Konzepte für mehrere Instanzen sind jedoch auch auf andere Plattformen und Technologien (z. B. Python und Intel MPI auf Linux-Knoten) anwendbar.

## <a name="multi-instance-task-overview"></a>Task mit mehreren Instanzen – Übersicht

In Batch wird normalerweise jeder Task auf einem einzelnen Computeknoten ausgeführt – Sie übermitteln mehrere Tasks an einen Auftrag, und Batch-Dienst plant die Ausführung der einzelnen Tasks auf einem Knoten. Indem Sie die **Einstellungen für mehrere Instanzen** eines Tasks konfigurieren, weisen Sie Batch aber an, stattdessen einen primären Task und mehrere Subtasks zu erstellen, die dann auf mehreren Knoten ausgeführt werden.

:::image type="content" source="media/batch-mpi/batch-mpi-01.png" alt-text="Diagram: Übersicht über die Einstellungen für mehrere Instanzen":::

Wenn Sie einen Task mit Einstellungen für mehrere Instanzen an einen Auftrag übermitteln, führt Batch mehrere für Tasks mit mehreren Instanzen eindeutige Schritte aus:

1. Der Batch-Dienst erstellt basierend auf den Einstellungen für mehrere Instanzen einen **primären** Task und mehrere **Subtasks**. Die Gesamtzahl der Tasks (primärer Task und alle Subtasks) entspricht der Anzahl der **Instanzen** (Computeknoten), die Sie in den Einstellungen für mehrere Instanzen festlegen.
2. Batch legt einen Computeknoten als **Master** fest und plant die Ausführung der primären Task auf dem Master. Es plant die Ausführung der Subtasks auf den restlichen Computeknoten, die dem Task mit mehreren Instanzen zugeordnet sind (eine Unteraufgabe pro Knoten).
3. Der Primärtask und alle Subtasks laden alle **gemeinsamen Ressourcendateien** herunter, die Sie in den Einstellungen für mehreren Instanzen angeben.
4. Nachdem die gemeinsamen Ressourcendateien heruntergeladen wurden, wird der in den Einstellungen für mehrere Instanzen angegebene **Koordinationsbefehl** vom Primärtask und von den Subtasks ausgeführt. Der Koordinationsbefehl wird in der Regel für die Vorbereitung von Knoten zum Ausführen des Tasks verwendet. Dies kann das Starten von Diensten im Hintergrund (z. B. `smpd.exe` von [Microsoft MPI](/message-passing-interface/microsoft-mpi)) und das Sicherstellen umfassen, dass die Knoten zum Verarbeiten von Nachrichten zwischen den Knoten bereit sind.
5. Der Primärtask führt den **Anwendungsbefehl** auf dem Masterknoten aus, *nachdem* der Koordinationsbefehl vom Primärtask und von allen Subtasks erfolgreich abgeschlossen wurde. Der Anwendungsbefehl ist die Befehlszeile des Tasks mit mehreren Instanzen und wird nur vom Primärtask ausgeführt. In einer [MS-MPI](/message-passing-interface/microsoft-mpi)-basierten Lösung führen Sie hier Ihre MPI-fähige Anwendung mit `mpiexec.exe` aus.

> [!NOTE]
> Obwohl er sich funktional unterscheidet, ist der „Task mit mehreren Instanzen“ kein eindeutiger Tasktyp wie [StartTask](/dotnet/api/microsoft.azure.batch.starttask) oder [JobPreparationTask](/dotnet/api/microsoft.azure.batch.jobpreparationtask). Der Task mit mehreren Instanzen ist einfach ein standardmäßiger Batch-Task ([CloudTask](/dotnet/api/microsoft.azure.batch.cloudtask) in Batch .NET), dessen Einstellungen für mehrere Instanzen konfiguriert wurden. In diesem Artikel bezeichnen wir sie als **Task mit mehreren Instanzen**.

## <a name="requirements-for-multi-instance-tasks"></a>Anforderungen für Tasks mit mehreren Instanzen

Tasks mit mehreren Instanzen erfordern einen Pool, in dem die **Kommunikation zwischen Knoten** aktiviert und die **gleichzeitige Ausführung von Tasks deaktiviert** ist. Um die gleichzeitige Ausführung von Tasks zu deaktivieren, legen Sie die Eigenschaft [CloudPool.TaskSlotsPerNode](/dotnet/api/microsoft.azure.batch.cloudpool) auf 1 fest.

> [!NOTE]
> Batch [begrenzt](batch-quota-limit.md#pool-size-limits) die Größe eines Pools, für den Kommunikation zwischen den Knoten aktiviert wurde.

In diesem Codeausschnitt wird veranschaulicht, wie ein Pool für Tasks mit mehreren Instanzen mithilfe der Batch-Bibliothek für .NET erstellt wird.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicatedComputeNodes: 3
        virtualMachineSize: "standard_d1_v2",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.TaskSlotsPerNode = 1;
```

> [!NOTE]
> Wenn Sie versuchen, einen Task mit mehreren Instanzen in einem Pool auszuführen, in dem die Kommunikation zwischen den Knoten deaktiviert ist oder der einen höheren *taskSlotsPerNode*-Wert als 1 aufweist, wird der Task nie geplant – er bleibt auf unbestimmte Zeit im aktiven Zustand.

### <a name="use-a-starttask-to-install-mpi"></a>Verwenden eines StartTask-Elements für die MPI-Installation

Zum Ausführen von MPI-Anwendungen mit einem Task mit mehreren Instanzen müssen Sie zunächst eine MPI-Implementierung (z.B. MS-MPI oder Intel MPI) auf den Computeknoten im Pool installieren. Dies ist der ideale Zeitpunkt, um einen [StartTask](/dotnet/api/microsoft.azure.batch.starttask) zu verwenden, der immer ausgeführt wird, wenn ein Knoten mit einem Pool verknüpft oder neu gestartet wird. Mit diesem Codeausschnitt wird ein StartTask erstellt, mit dem das MS-MPI-Setup-Paket als [Ressourcendatei](/dotnet/api/microsoft.azure.batch.resourcefile) angegeben wird. Die Befehlszeile des Starttask wird ausgeführt, nachdem die Ressourcendatei auf den Knoten heruntergeladen wird. In diesem Fall führt die Befehlszeile eine unbeaufsichtigte Installation von MS-MPI durch.

```csharp
// Create a StartTask for the pool which we use for installing MS-MPI on
// the nodes as they join the pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin)),
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit the fully configured pool to the Batch service to actually create
// the pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a>Remotezugriff auf den direkten Speicher (Remote Direct Memory Access, RDMA)

Wenn Sie für die Computeknoten in Ihrem Batch-Pool eine [für RDMA geeignete Größe](../virtual-machines/sizes-hpc.md?toc=/azure/virtual-machines/windows/toc.json) wie z.B. A9 auswählen, kann die MPI-Anwendung das hochleistungsfähige RDMA-Netzwerk (Remote Direct Memory Access) mit geringer Latenz von Azure nutzen.

Suchen Sie unter [Größen für virtuelle Computer in Azure](../virtual-machines/sizes.md) (für VirtualMachineConfiguration-Pools) bzw. [Größen für Cloud Services (klassisch)](../cloud-services/cloud-services-sizes-specs.md) (für CloudServicesConfiguration-Pools) nach den mit „RDMA-fähig“ gekennzeichneten Größen.

> [!NOTE]
> Zum Nutzen von RDMA auf [Linux-Computeknoten](batch-linux-nodes.md) müssen Sie **Intel MPI** auf den Knoten verwenden.

## <a name="create-a-multi-instance-task-with-batch-net"></a>Erstellen eines Tasks mit mehreren Instanzen mithilfe von Batch .NET

Nachdem wie die Poolanforderungen und die MPI-Paketinstallation jetzt behandelt haben, erstellen wir den Task mit mehreren Instanzen. In diesem Ausschnitt erstellen wir einen Standard-[CloudTask](/dotnet/api/microsoft.azure.batch.cloudtask) und konfigurieren dann seine [MultiInstanceSettings](/dotnet/api/microsoft.azure.batch.cloudtask)-Eigenschaft. Wie bereits erwähnt, ist der Task mit mehreren Instanzen kein eigener Tasktyp, sondern ein standardmäßiger Batch-Task, der mit Einstellungen für mehrere Instanzen konfiguriert ist.

```csharp
// Create the multi-instance task. Its command line is the "application command"
// and will be executed *only* by the primary, and only after the primary and
// subtasks execute the CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure the task's MultiInstanceSettings. The CoordinationCommandLine will be executed by
// the primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit the task to the job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on the nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a>Primärer Task und Subtasks

Beim Erstellen der Einstellungen für mehrere Instanzen für einen Task geben Sie die Anzahl von Computeknoten an, auf denen der Task ausgeführt werden soll. Wenn Sie den Task an einen Auftrag übermitteln, erstellt der Batch-Dienst einen **Primärtask** und eine ausreichende Anzahl von **Subtasks**, die zusammen der festgelegten Anzahl von Knoten entsprechen.

Diesen Tasks wird eine ganzzahlige ID im Bereich von 0 bis *numberOfInstances* - 1 zugewiesen. Der Task mit der ID 0 ist der primäre Task, und alle anderen IDs sind Subtasks. Wenn Sie z. B. für einen Task die folgenden Einstellungen für mehrere Instanzen erstellen, erhält der Primärtask die ID 0, und die Subtasks erhalten die IDs 1 bis 9.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Masterknoten

Wenn Sie einen Task mit mehreren Instanzen übermitteln, legt Batch einen Computeknoten als Masterknoten fest und plant die Ausführung des primären Tasks auf dem Masterknoten. Die Subtasks werden auf den restlichen Knoten ausgeführt, die dem Task mit mehreren Instanzen zugeordnet sind.

## <a name="coordination-command"></a>Koordinationsbefehl

Der **Koordinationsbefehl** wird vom Primärtask und den Subtasks ausgeführt.

Der Aufruf des Koordinationsbefehls führt zu einer Blockierung – Batch führt den Anwendungsbefehl erst dann aus, wenn der Koordinationsbefehl für alle Subtasks erfolgreich zurückgegeben wurde. Der Koordinationsbefehl sollte daher alle erforderlichen Hintergrunddienste starten, sicherstellen, dass sie einsatzbereit sind, und dann beendet werden. Dieser Koordinationsbefehl für eine Lösung mit MS-MPI Version 7 startet z. B. den SMPD-Dienst auf dem Knoten und wird dann beendet:

`cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d`

Beachten Sie die Verwendung von `start` in diesem Koordinationsbefehl. Dies ist erforderlich, da die `smpd.exe` -Anwendung nicht sofort nach der Ausführung zurückgegeben wird. Ohne die Verwendung des start-Befehls würde dieser Koordinationsbefehl nicht zurückgegeben und daher die Ausführung des Anwendungsbefehls blockieren.

## <a name="application-command"></a>Anwendungsbefehl

Nachdem der Primärtask und die Subtasks die Ausführung des Koordinationsbefehls abgeschlossen haben, wird die Befehlszeile des Tasks mit mehreren Instanzen *nur* vom Primärtask ausgeführt. Wir bezeichnen dies zur Unterscheidung vom Koordinationsbefehl als **Anwendungsbefehl** .

Verwenden Sie bei MS-MPI-Anwendungen den Anwendungsbefehl, um Ihre MPI-fähige Anwendung mit `mpiexec.exe` auszuführen. Hier sehen Sie ist z. B. einen Anwendungsbefehl für eine Lösung mit MS-MPI Version 7:

`cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe`

> [!NOTE]
> Da die Datei `mpiexec.exe` von MS-MPI standardmäßig die Variable `CCP_NODES` verwendet (siehe [Umgebungsvariablen](#environment-variables)), wird sie in der obigen Beispielbefehlszeile der Anwendung ausgeschlossen.

## <a name="environment-variables"></a>Umgebungsvariablen

Batch erstellt mehrere [Umgebungsvariablen](batch-compute-node-environment-variables.md) für Tasks mit mehreren Instanzen auf den Computeknoten, die einem Task mit mehreren Instanzen zugeordnet sind. Ihre Anwendungs- und Koordinationsbefehlszeilen sowie die Skripts und Programme, die sie ausführen, können auf diese Umgebungsvariablen verweisen.

Die folgenden Umgebungsvariablen werden vom Batch-Dienst für die Verwendung von Tasks mit mehreren Instanzen erstellt:

- `CCP_NODES`
- `AZ_BATCH_NODE_LIST`
- `AZ_BATCH_HOST_LIST`
- `AZ_BATCH_MASTER_NODE`
- `AZ_BATCH_TASK_SHARED_DIR`
- `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Weitere Informationen zu diesen und anderen Umgebungsvariablen der Batch-Computeknoten, ihren Inhalten und ihrer Sichtbarkeit finden Sie unter [Compute node environment variables](batch-compute-node-environment-variables.md) (Computeknoten-Umgebungsvariablen).

> [!TIP]
> Das [Batch-Linux-MPI-Codebeispiel](https://github.com/Azure-Samples/azure-batch-samples/tree/master/Python/Batch/article_samples/mpi) enthält ein Beispiel dafür, wie einige dieser Umgebungsvariablen verwendet werden können.

## <a name="resource-files"></a>Ressourcendateien

Es gibt zwei Sätze von Ressourcendateien, die bei Tasks mit mehreren Instanzen berücksichtigt werden müssen: **gemeinsame Ressourcendateien**, die von *allen* Tasks (Primärtask und Subtasks) heruntergeladen werden, und die **Ressourcendateien** für den Task mit mehreren Instanzen selbst, die *nur vom Primärtask* heruntergeladen werden.

In den Einstellungen für mehrere Instanzen eines Tasks können Sie eine oder mehrere **gemeinsame Ressourcendateien** angeben. Diese gemeinsamen Ressourcendateien werden vom Primärtask und von den Subtasks aus [Azure Storage](../storage/common/storage-introduction.md) in das **freigegebene Verzeichnis** des Tasks aller Knoten heruntergeladen. Sie können mithilfe der `AZ_BATCH_TASK_SHARED_DIR` -Umgebungsvariablen über Anwendungs- und Koordinationsbefehlszeilen auf das freigegebene Verzeichnis eines Tasks zugreifen. Da der Pfad `AZ_BATCH_TASK_SHARED_DIR` ist bei allen dem Task mit mehreren Instanzen zugeordneten Knoten identisch ist, können Sie für den Primärtask und alle Subtasks denselben Koordinationsbefehl verwenden. Batch wird das Verzeichnis nicht im Sinne eines Remotzugriffs „freigeben“, aber Sie können es wie im Tipp zu Umgebungsvariablen erwähnt als Bereitstellungs- oder Freigabepunkt verwenden.

Ressourcendateien, die Sie für den Task mit mehreren Instanzen angeben, werden standardmäßig in das Arbeitsverzeichnis des Tasks (`AZ_BATCH_TASK_WORKING_DIR`) heruntergeladen. Im Gegensatz zu gemeinsamen Ressourcendateien lädt wie erwähnt nur der Primärtask die für den Task mit mehreren Instanzen angegebenen Ressourcendateien herunter.

> [!IMPORTANT]
> Verwenden Sie immer die Umgebungsvariablen `AZ_BATCH_TASK_SHARED_DIR` und `AZ_BATCH_TASK_WORKING_DIR`, um in Ihren Befehlszeilen auf diese Verzeichnisse zu verweisen. Versuchen Sie nicht, die Pfade manuell zu erstellen.

## <a name="task-lifetime"></a>Gültigkeitsdauer von Tasks

Die Gültigkeitsdauer des Primärtasks steuert die Gültigkeitsdauer des gesamten Tasks mit mehreren Instanzen. Wenn der Primärtask beendet wird, werden auch alle Subtasks beendet. Der Exitcode des Primärtasks ist der Exitcode des Tasks und wird daher verwendet, um den Erfolg oder Misserfolg des Tasks zu Wiederholungszwecken zu bestimmen.

Wenn beispielsweise einer der Subtasks fehlschlägt und mit einem Rückgabecode ungleich NULL beendet wird, schlägt der gesamte Task mit mehreren Instanzen fehl. Der Task mit mehreren Instanzen wird dann beendet und wiederholt, bis die Grenze für erneute Versuche erreicht ist.

Wenn Sie einen Task mit mehreren Instanzen löschen, werden der Primärtask und alle Subtasks ebenfalls vom Batch-Dienst gelöscht. Alle Subtaskverzeichnisse und die Dateien darin werden genau wie bei einem Standardtask von den Computeknoten gelöscht.

[TaskConstraints](/dotnet/api/microsoft.azure.batch.taskconstraints) für einen Task mit mehreren Instanzen, z.B. die [MaxTaskRetryCount](/dotnet/api/microsoft.azure.batch.taskconstraints.maxtaskretrycount)-, [MaxWallClockTime](/dotnet/api/microsoft.azure.batch.taskconstraints.maxwallclocktime)- und [RetentionTime](/dotnet/api/microsoft.azure.batch.taskconstraints.retentiontime)-Eigenschaften werden berücksichtigt, da sie für einen Standardtask angegeben sind und für den Primärtask sowie alle Subtasks gelten. Wenn Sie die RetentionTime-Eigenschaft jedoch nach dem Hinzufügen des Tasks mit mehreren Instanzen zum Auftrag ändern, wird diese Änderung nur für den Primärtask übernommen. Alle Subtasks verwenden weiterhin die ursprüngliche RetentionTime-Eigenschaft.

In der Liste der aktuellen Tasks eines Computeknotens wird die ID eines Subtasks angezeigt, wenn der aktuelle Task Teil eines Tasks mit mehreren Instanzen war.

## <a name="obtain-information-about-subtasks"></a>Abrufen von Informationen zu Subtasks

Zum Abrufen von Informationen zu Subtasks mithilfe der Batch-Bibliothek für .NET rufen Sie die [CloudTask.ListSubtasks](/dotnet/api/microsoft.azure.batch.cloudtask.listsubtasks)-Methode auf. Diese Methode gibt Informationen zu allen Subtasks sowie zum Computeknoten zurück, auf dem die Tasks ausgeführt wurden. Anhand dieser Informationen können Sie u. a. das Stammverzeichnis, die Pool-ID, den aktuellen Zustand und den Exitcode der einzelnen Subtasks bestimmen. Diese Informationen können Sie in Kombination mit der [PoolOperations.GetNodeFile](/dotnet/api/microsoft.azure.batch.pooloperations.getnodefile)-Methode zum Abrufen der Dateien des Subtasks verwenden. Beachten Sie, dass diese Methode keine Informationen für den Primärtask (ID 0) zurückgibt.

> [!NOTE]
> Sofern nicht anders angegeben, gelten Batch .NET-Methoden, die auf den [CloudTask](/dotnet/api/microsoft.azure.batch.cloudtask) mit mehreren Instanzen selbst angewendet werden, *nur* für den Primärtask. Wenn Sie beispielsweise die [CloudTask.ListNodeFiles](/dotnet/api/microsoft.azure.batch.cloudtask.listnodefiles)-Methode für einen Task mit mehreren Instanzen aufrufen, werden nur die Dateien des Primärtasks zurückgegeben.

Der folgende Codeausschnitt zeigt das Abrufen von Informationen zu Subtasks sowie das Anfordern von Dateiinhalten von den Knoten, auf denen sie ausgeführt werden.

```csharp
// Obtain the job and the multi-instance task from the Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain the list of subtasks for the task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over the subtasks and print their stdout and stderr
// output if the subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == SubtaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a>Codebeispiel

Im Codebeispiel [MultiInstanceTasks](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks) auf GitHub wird veranschaulicht, wie Sie einen Task mit mehreren Instanzen zum Ausführen einer [MS-MPI](/message-passing-interface/microsoft-mpi)-Anwendung auf Batch-Computeknoten verwenden. Führen Sie die folgenden Schritte aus, um das Beispiel auszuführen.

### <a name="preparation"></a>Vorbereitung

1. Laden Sie das [MS-MPI SDK und die Redist-Installationsprogramme](/message-passing-interface/microsoft-mpi) herunter, und installieren Sie sie. Nach der Installation können Sie überprüfen, ob die MS-MPI-Umgebungsvariablen festgelegt wurden.
1. Erstellen Sie eine *Freigabe* version des MPI-Beispielprogramms [MPIHelloWorld](https://github.com/Azure-Samples/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld). Dies ist das Programm, das vom Task mit mehreren Instanzen auf Computeknoten ausgeführt wird.
1. Erstellen Sie eine ZIP-Datei, die `MPIHelloWorld.exe` (in Schritt 2 erstellt) und `MSMpiSetup.exe` (in Schritt 1 heruntergeladen) enthält. Im nächsten Schritt laden Sie diese ZIP-Datei als Anwendungspaket hoch.
1. Verwenden Sie das [Azure-Portal](https://portal.azure.com) zum Erstellen einer Batch-[Anwendung](batch-application-packages.md) mit dem Namen „MPIHelloWorld“, und geben Sie die im vorherigen Schritt als Version „1.0“ des Anwendungspakets erstellte ZIP-Datei an. Weitere Informationen finden Sie unter [Hochladen und Verwalten von Anwendungen](batch-application-packages.md#upload-and-manage-applications).

> [!TIP]
> Durch die Erstellung einer *Releaseversion* von `MPIHelloWorld.exe` wird sichergestellt, dass Sie keine weiteren Abhängigkeiten (z. B. `msvcp140d.dll` oder `vcruntime140d.dll`) in Ihr Anwendungspaket einbinden müssen.

### <a name="execution"></a>Ausführung

1. Laden Sie die [ZIP-Datei „azure-batch-samples“](https://github.com/Azure/azure-batch-samples/archive/master.zip) von GitHub herunter.
1. Öffnen Sie die **Projektmappe** „MultiInstanceTasks“ in Visual Studio 2019. Die Lösungsdatei `MultiInstanceTasks.sln` befindet sich hier:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`
1. Geben Sie die Anmeldeinformationen für Ihr Batch- und Storage-Konto in `AccountSettings.settings` im Projekt **Microsoft.Azure.Batch.Samples.Common** ein.
1. **Erstellen Sie die Lösung MultiInstanceTasks, und führen Sie sie aus**, damit die MPI-Beispielanwendung auf den Computeknoten in einem Batch-Pool ausgeführt wird.
1. *Optional*: Verwenden Sie das [Azure-Portal](https://portal.azure.com) oder den [Batch Explorer](https://azure.github.io/BatchExplorer/), um Beispielpool, -auftrag und -task („MultiInstanceSamplePool“, „MultiInstanceSampleJob“, „MultiInstanceSampleTask“) vor dem Löschen der Ressourcen zu untersuchen.

> [!TIP]
> Sie können [Visual Studio Community](https://visualstudio.microsoft.com/vs/community/) kostenlos herunterladen, falls Sie Visual Studio noch nicht erworben haben.

Die Ausgabe der Datei `MultiInstanceTasks.exe` sieht in etwa wie folgt aus:

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] to job [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks to complete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich ausführlicher über die [MPI-Unterstützung für Linux in Azure Batch](https://docs.microsoft.com/archive/blogs/windowshpc/introducing-mpi-support-for-linux-on-azure-batch).
- Erfahren Sie, wie Sie [Pools mit Linux-Computeknoten](batch-linux-nodes.md) zur Verwendung in Ihren Azure Batch-MPI-Lösungen erstellen.
