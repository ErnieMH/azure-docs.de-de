---
title: Erstellen von Aufgaben zum Vorbereiten und Durchführen von Aufträgen auf Computeknoten
description: Verwenden Sie Vorbereitungs- und Freigabeaufgaben auf Auftragsebene, um Datenübertragungen auf Azure Batch-Computeknoten zu minimieren und Knoten nach Abschluss des Auftrags zu bereinigen.
ms.topic: how-to
ms.date: 02/17/2020
ms.custom: seodec18, devx-track-csharp
ms.openlocfilehash: 5b1084cfdd5995b7983badcdce71460f7bdec3d5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "88919453"
---
# <a name="run-job-preparation-and-job-release-tasks-on-batch-compute-nodes"></a>Ausführen von Tasks zum Vorbereiten und Freigeben von Aufträgen auf Azure Batch-Computeknoten

 Für einen Azure Batch-Auftrag ist häufig ein bestimmtes Setup erforderlich, bevor die entsprechenden Aufgaben ausgeführt werden, sowie ein Wartungsschritt nach der Durchführung der Aufgaben. Sie müssen unter Umständen allgemeine Eingabedaten für die Aufgaben auf Ihre Computeknoten herunterladen oder Ausgabedaten der Aufgaben in Azure Storage hochladen, nachdem der Auftrag abgeschlossen wurde. Sie können Aufgaben vom Typ **Auftragsvorbereitung** und **Auftragsfreigabe** verwenden, um diese Vorgänge durchzuführen.

## <a name="what-are-job-preparation-and-release-tasks"></a>Was sind Aufgaben zur Auftragsvorbereitung und -freigabe?
Bevor die Aufgaben eines Auftrags ausgeführt werden, wird die Auftragsvorbereitungsaufgabe auf allen Computeknoten ausgeführt, für die die Ausführung von mindestens einer Aufgabe geplant ist. Nach Abschluss des Auftrags wird die Auftragsfreigabeaufgabe auf jedem Knoten im Pool ausgeführt, der mindestens eine Aufgabe ausgeführt hat. Wie bei normalen Batch-Aufgaben können Sie eine Befehlszeile angeben, die aufgerufen wird, wenn eine Aufgabe zur Auftragsvorbereitung oder -freigabe ausgeführt wird.

Aufgaben zur Auftragsvorbereitung und -freigabe verfügen über vertraute Batch-Aufgabenfunktionen, z.B. Herunterladen von Dateien ([Ressourcendateien][net_job_prep_resourcefiles]), Ausführen mit höheren Rechten, benutzerdefinierte Umgebungsvariablen, maximale Ausführungsdauer, Anzahl von Wiederholungsversuchen und Dateiaufbewahrungsdauer.

In den folgenden Abschnitten erfahren Sie, wie Sie die Klassen [JobPreparationTask][net_job_prep] und [JobReleaseTask][net_job_release] in der [Batch .NET][api_net]-Bibliothek verwenden.

> [!TIP]
> Aufgaben zur Auftragsvorbereitung und -freigabe sind insbesondere in Umgebungen mit einem gemeinsam genutzten Pool hilfreich, in denen ein Pool mit Computeknoten zwischen Auftragsausführungen beibehalten und von vielen verschiedenen Aufträgen genutzt wird.
> 
> 

## <a name="when-to-use-job-preparation-and-release-tasks"></a>Verwendungsmöglichkeiten für Auftragsvorbereitungs- und Auftragsfreigabeaufgaben
Auftragsvorbereitungs- und Auftragsfreigabeaufgaben eignen sich gut für die folgenden Situationen:

**Download von allgemeinen Aufgabendaten**

Batchaufträge erfordern oft einen gemeinsamen Satz von Daten als Eingabe für die Aufgaben des Auftrags. Bei täglich durchgeführten Berechnungen zur Risikoanalyse sind Marktdaten z.B. auftragsspezifisch, gelten aber trotzdem für alle Aufgaben im Auftrag. Diese Marktdaten, die oft mehrere GB groß sind, sollten auf jeden Computeknoten nur einmal heruntergeladen werden, sodass sie von jeder auf dem Knoten ausgeführten Aufgabe verwendet werden können. Verwenden Sie eine **Auftragsvorbereitungsaufgabe**, um diese Daten vor der Ausführung anderer Aufgaben im Auftrag auf jeden Knoten herunterzuladen.

**Löschen der Auftrags- und Aufgabenausgabe**

In einer Umgebung mit einem gemeinsam genutzten Pool, in der die Computeknoten eines Pools zwischen Aufträgen nicht außer Betrieb genommen werden, müssen Sie Auftragsdaten zwischen den Ausführungen unter Umständen löschen. Es kann sein, dass Sie auf den Knoten Speicherplatz sparen oder die Sicherheitsrichtlinien Ihrer Organisation einhalten müssen. Verwenden Sie eine **Auftragsfreigabeaufgabe** , um Daten zu löschen, die von einer Auftragsvorbereitungsaufgabe heruntergeladen oder während der Aufgabenausführung generiert wurden.

**Protokollaufbewahrung**

Eventuell wollen Sie eine Kopie der von den Aufgaben generierten Protokolle oder die von fehlerhaften Anwendungen generierten Absturzabbilddateien aufbewahren. Verwenden Sie in solchen Fällen eine **Auftragsfreigabeaufgabe**, um diese Daten zu komprimieren und in ein [Azure Storage][azure_storage]-Konto hochzuladen.

> [!TIP]
> Eine andere Möglichkeit zum Beibehalten von Protokollen und anderen Auftrags- und Aufgabenausgabedaten ist die Verwendung der Bibliothek [Azure Batch File Conventions](batch-task-output.md) (Azure Batch-Dateikonventionen).
>
>

## <a name="job-preparation-task"></a>Auftragsvorbereitungsaufgabe


Vor der Ausführung der Aufgaben eines Auftrags führt Batch die Auftragsvorbereitungsaufgabe auf jedem Computeknoten aus, der zum Ausführen einer Aufgabe eingeplant ist. Standardmäßig wartet Batch, bis die Auftragsvorbereitungsaufgabe abgeschlossen ist, bevor die geplanten Aufgaben auf dem Knoten ausgeführt werden. Sie können den Dienst allerdings auch dahingehend konfigurieren, dass er nicht wartet. Wenn der Knoten neu gestartet wird, wird die Auftragsvorbereitungsaufgabe erneut ausgeführt. Sie können dieses Verhalten auch deaktivieren. Wenn Sie einen Auftrag mit einer Auftragsvorbereitungsaufgabe haben und eine Auftrags-Manager-Aufgabe konfiguriert ist, wird die Auftragsvorbereitungsaufgabe vor der Auftrags-Manager-Aufgabe ausgeführt, genauso wie bei allen anderen Tasks. Die Auftragsvorbereitungsaufgabe wird immer zuerst ausgeführt.

Die Auftragsvorbereitungsaufgabe wird nur auf Knoten ausgeführt, die zum Ausführen einer Aufgabe eingeplant sind. Das verhindert die unnötige Ausführung von Auftragsvorbereitungsaufgaben an Knoten, denen keine Aufgaben zugewiesen sind.  Dies kann eintreten, wenn die Anzahl der Aufgaben für einen Auftrag geringer ist, als die Anzahl der Knoten in einem Pool. Dies gilt auch, wenn die [gleichzeitige Aufgabenausführung](batch-parallel-node-tasks.md) aktiviert ist. Einige Knoten bleiben unbeschäftigt, wenn die Gesamtzahl von Aufgaben geringer als die Gesamtzahl der möglichen zeitgleich ausgeführten Aufgaben ist. Indem Sie die Auftragsvorbereitungsaufgabe nicht auf Knoten im Leerlauf ausführen, können Sie bei den Gebühren für Datenübertragungen sparen.

> [!NOTE]
> [JobPreparationTask][net_job_prep_cloudjob] unterscheidet sich vom [CloudPool.StartTask][pool_starttask] insofern, dass „JobPreparationTask“ beim Start jedes Auftrags ausgeführt wird, während „StartTask“ nur ausgeführt wird, wenn ein Computeknoten einem Pool erstmals hinzugefügt oder neu gestartet wird.
>


>## <a name="job-release-task"></a>Auftragsfreigabeaufgabe

Nachdem ein Auftrag als abgeschlossen markiert wurde, wird die Auftragsfreigabeaufgabe auf jedem Knoten im Pool ausgeführt, der mindestens eine Aufgabe ausgeführt hat. Um einen Auftrag als abgeschlossen zu markieren, geben Sie eine Terminate-Anforderung aus. Der Batch-Dienst legt anschließend den Status des Auftrags auf *Wird beendet* fest, beendet alle aktiven bzw. ausgeführten Aufgaben im Zusammenhang mit dem Auftrag und führt die Auftragsfreigabeaufgabe aus. Danach wird der Status des Auftrags in *Abgeschlossen* geändert.

> [!NOTE]
> Beim Löschen eines Auftrags wird die Auftragsfreigabeaufgabe ebenfalls ausgeführt. Wurde ein Auftrag jedoch bereits zuvor beendet, wird die Freigabeaufgabe kein zweites Mal ausgeführt, wenn der Auftrag später gelöscht wird.

Auftragsfreigabeaufgaben können maximal 15 Minuten lang ausgeführt werden, bevor Sie vom Batch-Dienst beendet werden. Weitere Informationen finden Sie in der [Referenzdokumentation zur REST-API](/rest/api/batchservice/job/add#jobreleasetask).
> 
> 

## <a name="job-prep-and-release-tasks-with-batch-net"></a>Aufgaben zur Auftragsvorbereitung und -freigabe mit Batch .NET
Zum Verwenden einer Auftragsvorbereitungsaufgabe weisen Sie der Eigenschaft [CloudJob.JobPreparationTask][net_job_prep] des Auftrags ein [JobPreparationTask][net_job_prep_cloudjob]-Objekt zu. Auf ähnliche Weise initialisieren Sie einen [JobReleaseTask][net_job_release], den Sie der Eigenschaft [CloudJob.JobReleaseTask][net_job_prep_cloudjob] zuweisen, um die Freigabeaufgabe des Auftrags festzulegen.

In diesem Codeausschnitt ist `myBatchClient` eine Instanz von [BatchClient][net_batch_client], und `myPool` ist ein vorhandener Pool im Batch-Konto.

```csharp
// Create the CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify the command lines for the job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign the job preparation task to the job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign the job release task to the job
myJob.JobReleaseTask =
    new JobReleaseTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

Wie oben erwähnt, wird die Freigabeaufgabe ausgeführt, wenn ein Auftrag beendet oder gelöscht wird. Beenden Sie einen Auftrag mit [JobOperations.TerminateJobAsync][net_job_terminate]. Löschen Sie einen Auftrag mit [JobOperations.DeleteJobAsync][net_job_delete]. Normalerweise beenden oder löschen Sie einen Auftrag, wenn dessen Aufgaben abgeschlossen sind oder wenn eine Zeitüberschreitung erreicht wird, die Sie festgelegt haben.

```csharp
// Terminate the job to mark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsync("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a>Codebeispiel auf GitHub
Sehen Sie sich das Beispielprojekt [JobPrepRelease][job_prep_release_sample] auf GitHub an, um Aufgaben zur Auftragsvorbereitung und -freigabe in Aktion zu erleben. Diese Konsolenanwendung führt folgende Schritte aus:

1. Sie erstellt einen Pool mit zwei Knoten.
2. Sie erstellt einen Auftrag mit Auftragsvorbereitungs-, Auftragsfreigabe- und Standardaufgaben.
3. Sie führt die Auftragsvorbereitungsaufgabe aus, die zunächst die Knoten-ID in eine Textdatei im „freigegebenen“ Verzeichnis des Knotens schreibt.
4. Sie führt auf jedem Knoten eine Aufgabe aus, welche die Aufgaben-ID in dieselbe Textdatei schreibt.
5. Nach dem Abschluss aller Aufgaben (oder Erreichen des Timeouts) gibt sie den Inhalt der Textdatei jedes Knotens an die Konsole aus.
6. Sie führt die Auftragsfreigabeaufgabe aus, um die Datei aus dem Knoten zu löschen, wenn der Auftrag abgeschlossen ist.
7. Sie gibt die Exitcodes der Auftragsvorbereitungs- und Auftragsfreigabeaufgaben für jeden Knoten aus, auf dem diese ausgeführt wurden.
8. Sie hält die Ausführung an, um auf die Löschbestätigung für den Auftrag und/oder den Pool zu warten.

Die Ausgabe der Beispielanwendung sieht in etwa wie folgt aus:

```
Attempting to create pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob to reach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER to exit...
```

> [!NOTE]
> Die tatsächliche Ausgabe kann sich aufgrund der variablen Erstellungs- und Startzeit von Knoten in einem neuen Pool unterscheiden. (Einige Knoten sind schneller für Aufgaben bereit als andere.) Aufgrund der schnellen Ausführung der Aufgaben werden unter Umständen alle Aufgaben des Auftrags von einem einzelnen Knoten des Pools ausgeführt. In diesem Fall werden Sie feststellen, dass die Aufgaben zur Auftragsvorbereitung und -freigabe für den Knoten, die keine Aufgaben ausgeführt hat, nicht vorhanden sind.
> 
> 

### <a name="inspect-job-preparation-and-release-tasks-in-the-azure-portal"></a>Überprüfen von Aufgaben zur Auftragsvorbereitung und -freigabe im Azure-Portal
Wenn Sie die Beispielanwendung ausführen, können Sie die Eigenschaften des Auftrags und die zugehörigen Aufgaben im [Azure-Portal][portal] anzeigen und sogar die durch die Aufgaben des Auftrags geänderte, freigegebene Textdatei herunterladen.

Der folgende Screenshot zeigt das Blatt **Vorbereitungstasks** im Azure-Portal nach der Ausführung der Beispielanwendung. Navigieren Sie nach Abschluss der Aufgaben (aber noch vor dem Löschen des Auftrags und Pools) zu den Eigenschaften für *JobPrepReleaseSampleJob*, und klicken Sie auf **Vorbereitungstasks** oder **Freigabetasks**, um die entsprechenden Eigenschaften anzuzeigen.

![Aufgabenvorbereitungseigenschaften im Azure-Portal][1]

## <a name="next-steps"></a>Nächste Schritte
### <a name="application-packages"></a>Anwendungspakete
Neben der Aufgabe zur Auftragsvorbereitung können Sie auch das Batch-Feature [Anwendungspakete](batch-application-packages.md) verwenden, um Computeknoten für die Aufgabenausführung vorzubereiten. Dieses Feature ist besonders bei der Bereitstellung von Anwendungen hilfreich, für die kein Installationsprogramm ausgeführt werden muss, sowie bei Anwendungen, die mehr als 100 Dateien umfassen oder eine strenge Versionskontrolle erfordern.

### <a name="installing-applications-and-staging-data"></a>Installieren von Anwendungen und Bereitstellen von Daten (Staging)
Dieser Beitrag im MSDN-Forum enthält eine Übersicht über mehrere Methoden zum Vorbereiten der Knoten für die Ausführung von Aufgaben:

[Installing applications and staging data on Batch compute nodes][forum_post] (Installieren von Anwendungen und Bereitstellen von Daten auf Batch-Computeknoten)

Der Beitrag wurde von einem Mitglied des Azure Batch-Teams geschrieben und enthält Beschreibungen mehrerer Verfahren, die Sie zum Bereitstellen von Anwendungen und Daten auf Computeknoten verwenden können.

[api_net]: /dotnet/api/microsoft.azure.batch
[api_net_listjobs]: /dotnet/api/microsoft.azure.batch.joboperations
[api_rest]: /rest/api/batchservice/
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: /dotnet/api/microsoft.azure.batch.batchclient
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: /dotnet/api/microsoft.azure.batch.jobpreparationtask
[net_job_prep_cloudjob]: /dotnet/api/microsoft.azure.batch.cloudjob
[net_job_prep_resourcefiles]: /dotnet/api/microsoft.azure.batch.jobpreparationtask
[net_job_delete]: /previous-versions/azure/mt281411(v=azure.100)
[net_job_terminate]: /previous-versions/azure/mt188985(v=azure.100)
[net_job_release]: /dotnet/api/microsoft.azure.batch.jobreleasetask
[net_job_release_cloudjob]: /dotnet/api/microsoft.azure.batch.cloudjob
[pool_starttask]: /dotnet/api/microsoft.azure.batch.cloudpool

[net_list_certs]: /dotnet/api/microsoft.azure.batch.certificateoperations
[net_list_compute_nodes]: /dotnet/api/microsoft.azure.batch.pooloperations
[net_list_job_schedules]: /dotnet/api/microsoft.azure.batch.jobscheduleoperations
[net_list_jobprep_status]: /dotnet/api/microsoft.azure.batch.joboperations
[net_list_jobs]: /dotnet/api/microsoft.azure.batch.joboperations
[net_list_nodefiles]: /dotnet/api/microsoft.azure.batch.joboperations
[net_list_pools]: /dotnet/api/microsoft.azure.batch.pooloperations
[net_list_schedule_jobs]: /dotnet/api/microsoft.azure.batch.jobscheduleoperations
[net_list_task_files]: /dotnet/api/microsoft.azure.batch.cloudtask
[net_list_tasks]: /dotnet/api/microsoft.azure.batch.joboperations

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png
