---
title: Diagnoseprotokollierung für Azure Analysis Services | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Protokollierung zum Überwachen Ihres Azure Analysis Services-Servers einrichten.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: d5537079341823275ba521c9d44139a0e0305286
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92014935"
---
# <a name="setup-diagnostic-logging"></a>Einrichten der Diagnoseprotokollierung

Die Überwachung der Leistung Ihrer Server ist ein wesentlicher Bestandteil jeder Analysis Services-Lösung. Azure Analysis Services ist in Azure Monitor integriert. Mit [Azure Monitor-Ressourcenprotokollen](../azure-monitor/platform/platform-logs-overview.md) können Sie Protokolle überwachen und an [Azure Storage](https://azure.microsoft.com/services/storage/) senden, diese in [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) streamen und in [Azure Monitor-Protokolle](../azure-monitor/overview.md) exportieren.

![Ressourcenprotokollierung für Storage, Event Hubs oder Azure Monitor-Protokolle](./media/analysis-services-logging/aas-logging-overview.png)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="whats-logged"></a>Protokollierte Daten

Sie können die Kategorien **Modul**, **Dienst** und **Metriken** auswählen.

### <a name="engine"></a>Engine

Bei Auswahl von **Modul** werden alle [xEvents](/analysis-services/instances/monitor-analysis-services-with-sql-server-extended-events) protokolliert. Einzelne Ereignisse können nicht ausgewählt werden. 

|xEvent-Kategorien |Ereignisname  |
|---------|---------|
|Sicherheitsüberwachung    |   Audit Login      |
|Sicherheitsüberwachung    |   Audit Logout      |
|Sicherheitsüberwachung    |   Audit Server Starts And Stops      |
|Statusberichte     |   Progress Report Begin      |
|Statusberichte     |   Progress Report End      |
|Statusberichte     |   Progress Report Current      |
|Abfragen     |  Query Begin       |
|Abfragen     |   Query End      |
|Befehle     |  Command Begin       |
|Befehle     |  Command End       |
|Fehler und Warnungen     |   Fehler      |
|Entdecken     |   Discover End      |
|Benachrichtigung     |    Benachrichtigung     |
|Sitzung     |  Session Initialize       |
|Locks    |  Deadlock       |
|Abfrageverarbeitung     |   VertiPaq SE Query Begin      |
|Abfrageverarbeitung     |   VertiPaq SE Query End      |
|Abfrageverarbeitung     |   VertiPaq SE Query Cache Match      |
|Abfrageverarbeitung     |   Direct Query Begin      |
|Abfrageverarbeitung     |  Direct Query End       |

### <a name="service"></a>Dienst

|Vorgangsname  |Auftreten bei folgendem Vorgang  |
|---------|---------|
|ResumeServer     |    Fortsetzen eines Servers     |
|SuspendServer    |   Anhalten eines Servers      |
|DeleteServer     |    Löschen eines Servers     |
|RestartServer    |     Benutzer startet einen Server über SSMS oder PowerShell neu.    |
|GetServerLogFiles    |    Benutzer exportiert Serverprotokoll über PowerShell.     |
|ExportModel     |   Benutzer exportiert ein Modell im Portal mithilfe von „Öffnen“ in Visual Studio.     |

### <a name="all-metrics"></a>Alle Metriken

Die Kategorie „Metriken“ protokolliert dieselben [Servermetriken](analysis-services-monitor.md#server-metrics) in der AzureMetrics-Tabelle. Wenn Sie die Abfrage [horizontal skalieren](analysis-services-scale-out.md) und Metriken für jedes Lesereplikat trennen müssen, verwenden Sie stattdessen die AzureDiagnostics-Tabelle, wobei **OperationName** gleich **LogMetric** ist.

## <a name="setup-diagnostics-logging"></a>Einrichten der Diagnoseprotokollierung

### <a name="azure-portal"></a>Azure-Portal

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) > „Server“ im linken Navigationsbereich auf **Diagnoseeinstellungen** und dann auf **Diagnose aktivieren**.

    ![Aktivieren der Ressourcenprotokollierung für Azure Cosmos DB im Azure-Portal](./media/analysis-services-logging/aas-logging-turn-on-diagnostics.png)

2. Geben Sie in **Diagnoseeinstellungen** die folgenden Optionen an: 

    * **Name**: Geben Sie einen Namen für die zu erstellenden Protokolle ein.

    * **In einem Speicherkonto archivieren**. Sie benötigen ein vorhandenes Speicherkonto, mit dem eine Verbindung hergestellt werden kann, um diese Option verwenden zu können. Siehe [Erstellen Sie ein Speicherkonto](../storage/common/storage-account-create.md). Befolgen Sie die Anweisungen zum Erstellen eines allgemeinen Resource Manager-Kontos, und wählen Sie dann Ihr Speicherkonto aus, indem Sie zu dieser Seite im Portal zurückwechseln. Es dauert möglicherweise einige Minuten, bis neu erstellte Speicherkonten im Dropdownmenü angezeigt werden.
    * **An einen Event Hub streamen**. Sie benötigen einen vorhandenen Event Hub-Namespace und einen Event Hub. mit dem eine Verbindung hergestellt werden kann, um diese Option verwenden zu können. Weitere Informationen finden Sie unter [Erstellen eines Event Hubs-Namespace und eines Event Hubs mithilfe des Azure-Portals](../event-hubs/event-hubs-create.md). Kehren Sie anschließend auf diese Seite im Portal zurück, um den Event Hub-Namespace und den Richtliniennamen auszuwählen.
    * **An Azure Monitor senden (Log Analytics-Arbeitsbereich)** . Für diese Option können Sie entweder einen vorhandenen Arbeitsbereich verwenden oder im Portal [eine neue Arbeitsbereichsressource erstellen](../azure-monitor/learn/quick-create-workspace.md). Weitere Informationen zum Anzeigen Ihrer Protokolle finden Sie unter [Anzeigen von Protokollen im Log Analytics-Arbeitsbereich](#view-logs-in-log-analytics-workspace) in diesem Artikel.

    * **Modul:** Wählen Sie diese Option aus, um xEvents zu protokollieren. Wenn die Archivierung in einem Speicherkonto erfolgt, können Sie die Beibehaltungsdauer für die Ressourcenprotokolle auswählen. Protokolle werden nach Ablauf des Aufbewahrungszeitraums automatisch gelöscht.
    * **Dienst**. Wählen Sie diese Option aus, um Ereignisse auf Dienstebene zu protokollieren. Wenn Sie auf einem Speicherkonto archivieren, können Sie die Beibehaltungsdauer für die Ressourcenprotokolle auswählen. Protokolle werden nach Ablauf des Aufbewahrungszeitraums automatisch gelöscht.
    * **Metriken:** Wählen Sie diese Option aus, um ausführliche Daten in [Metriken](analysis-services-monitor.md#server-metrics) zu speichern. Wenn Sie auf einem Speicherkonto archivieren, können Sie die Beibehaltungsdauer für die Ressourcenprotokolle auswählen. Protokolle werden nach Ablauf des Aufbewahrungszeitraums automatisch gelöscht.

3. Klicken Sie auf **Speichern**.

    Möglicherweise wird eine solche Fehlermeldung angezeigt: „Fehler beim Aktualisieren der Diagnose für \<workspace name>. Das Abonnement „\<subscription id>“ ist nicht für die Verwendung von microsoft.insights registriert.“ Befolgen Sie die Anweisungen zur [Problembehandlung bei der Azure-Diagnose](../azure-monitor/platform/resource-logs.md), um das Konto zu registrieren, und wiederholen Sie dann dieses Verfahren.

    Wenn Sie ändern möchten, wie die Ressourcenprotokolle zu einem zukünftigen Zeitpunkt gespeichert werden, können Sie zum Ändern der Einstellungen zu dieser Seite zurückkehren.

### <a name="powershell"></a>PowerShell

Hier sind die grundlegenden Befehle zum Einstieg aufgeführt. Detaillierte Hilfe zum Einrichten der Protokollierung in einem Speicherkonto mithilfe von PowerShell finden Sie im Tutorial weiter unten in diesem Artikel.

Verwenden Sie die folgenden Befehle, um die Metrik- und Ressourcenprotokollierung mit PowerShell zu aktivieren:

- Verwenden Sie den folgenden Befehl, um das Speichern von Ressourcenprotokollen in einem Speicherkonto zu aktivieren:

   ```powershell
   Set-AzDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   Die Speicherkonto-ID ist die Ressourcen-ID für das Speicherkonto, an das die Protokolle gesendet werden sollen.

- Verwenden Sie den folgenden Befehl, um das Streamen von Ressourcenprotokollen an einen Event Hub zu aktivieren:

   ```powershell
   Set-AzDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   Die Azure Service Bus-Regel-ID ist eine Zeichenfolge mit dem folgenden Format:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- Verwenden Sie den folgenden Befehl, um das Senden von Ressourcenprotokollen an einen Log Analytics-Arbeitsbereich zu aktivieren:

   ```powershell
   Set-AzDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- Sie können die Ressourcen-ID mit dem folgenden Befehl aus Ihrem Log Analytics-Arbeitsbereich abrufen:

   ```powershell
   (Get-AzOperationalInsightsWorkspace).ResourceId
   ```

Sie können diese Parameter miteinander kombinieren, um mehrere Ausgabeoptionen zu aktivieren.

### <a name="rest-api"></a>REST-API

Informieren Sie sich darüber, wie Sie [Diagnoseeinstellungen mithilfe der Azure Monitor-REST-API ändern](/rest/api/monitor/). 

### <a name="resource-manager-template"></a>Resource Manager-Vorlage

Informieren Sie sich darüber, wie Sie [Diagnoseeinstellungen beim Erstellen von Ressourcen mithilfe einer Resource Manager-Vorlage aktivieren](../azure-monitor/samples/resource-manager-diagnostic-settings.md). 

## <a name="manage-your-logs"></a>Verwalten Ihrer Protokolle

Die Protokolle stehen in der Regel einige Stunden nach dem Einrichten der Protokollierung zur Verfügung. Die Verwaltung der Protokolle im Speicherkonto ist Ihre Aufgabe:

* Verwenden Sie zum Schützen der Protokolle standardmäßige Azure-Zugriffssteuerungsmethoden, indem Sie einschränken, wer darauf zugreifen kann.
* Löschen Sie Protokolle, die im Speicherkonto nicht mehr aufbewahrt werden sollen.
* Legen Sie eine Beibehaltungsdauer fest, sodass ältere Protokolle aus dem Speicherkonto gelöscht werden.

## <a name="view-logs-in-log-analytics-workspace"></a>Anzeigen von Protokollen im Log Analytics-Arbeitsbereich

Metriken und Serverereignisse werden in Ihren Log Analytics-Arbeitsbereich in xEvents integriert, sodass eine parallele Analyse erfolgt. Der Log Analytics-Arbeitsbereich kann zudem so konfiguriert werden, dass Ereignisse von anderen Azure-Diensten empfangen werden, sodass die Diagnoseprotokollierungsdaten Ihrer Architektur ganzheitlich angezeigt werden.

Öffnen Sie zum Anzeigen Ihrer Diagnosedaten im Log Analytics-Arbeitsbereich im linken Menü **Protokolle**.

![Protokollsuchoptionen im Azure-Portal](./media/analysis-services-logging/aas-logging-open-log-search.png)

Erweitern Sie im Abfrage-Generator **LogManagement** > **AzureDiagnostics**. AzureDiagnostics umfasst Modul- und Dienstereignisse. Beachten Sie, dass direkt eine Abfrage erstellt wird. Das Feld EventClass\_s enthält xEvent-Namen, die Ihnen vertraut vorkommen können, wenn Sie xEvents für die lokale Protokollierung verwendet haben. Wenn Sie auf **EventClass\_s** oder einen der Ereignisnamen klicken, wird im Log Analytics-Arbeitsbereich eine Abfrage erstellt. Speichern Sie die Abfragen, um sie zu einem späteren Zeitpunkt wiederverwenden zu können.

### <a name="example-queries"></a>Beispielabfragen

#### <a name="example-1"></a>Beispiel 1

Die folgende Abfrage gibt Werte für die Dauer für jedes Abfrageende-/Aktualisierungsendeereignis für eine Modelldatenbank und einen Server zurück. Beim horizontalen Hochskalieren werden die Ergebnisse nach Replikat aufgeteilt, da die Replikatnummer in „ServerName_s“ enthalten ist. Die Gruppierung nach RootActivityId_g verringert die aus der REST-API der Azure-Diagnose abgerufene Zeilenanzahl und hilft, die unter [Log Analytics-Ratengrenzwerte](https://dev.loganalytics.io/documentation/Using-the-API/Limits) beschriebenen Grenzwerte einzuhalten.

```Kusto
let window = AzureDiagnostics
   | where ResourceProvider == "MICROSOFT.ANALYSISSERVICES" and Resource =~ "MyServerName" and DatabaseName_s =~ "MyDatabaseName" ;
window
| where OperationName has "QueryEnd" or (OperationName has "CommandEnd" and EventSubclass_s == 38)
| where extract(@"([^,]*)", 1,Duration_s, typeof(long)) > 0
| extend DurationMs=extract(@"([^,]*)", 1,Duration_s, typeof(long))
| project  StartTime_t,EndTime_t,ServerName_s,OperationName,RootActivityId_g,TextData_s,DatabaseName_s,ApplicationName_s,Duration_s,EffectiveUsername_s,User_s,EventSubclass_s,DurationMs
| order by StartTime_t asc
```

#### <a name="example-2"></a>Beispiel 2

Die folgende Abfrage gibt Arbeitsspeicher- und CPU-Verbrauch für einen Server zurück. Beim horizontalen Hochskalieren werden die Ergebnisse nach Replikat aufgeteilt, da die Replikatnummer in „ServerName_s“ enthalten ist.

```Kusto
let window = AzureDiagnostics
   | where ResourceProvider == "MICROSOFT.ANALYSISSERVICES" and Resource =~ "MyServerName";
window
| where OperationName == "LogMetric" 
| where name_s == "memory_metric" or name_s == "qpu_metric"
| project ServerName_s, TimeGenerated, name_s, value_s
| summarize avg(todecimal(value_s)) by ServerName_s, name_s, bin(TimeGenerated, 1m)
| order by TimeGenerated asc 
```

#### <a name="example-3"></a>Beispiel 3

Die folgende Abfrage gibt die Leistungsindikatoren der pro Sekunde gelesenen Zeilen der Analysis Services-Engine für einen Server zurück.

```Kusto
let window =  AzureDiagnostics
   | where ResourceProvider == "MICROSOFT.ANALYSISSERVICES" and Resource =~ "MyServerName";
window
| where OperationName == "LogMetric" 
| where parse_json(tostring(parse_json(perfobject_s).counters))[0].name == "Rows read/sec" 
| extend Value = tostring(parse_json(tostring(parse_json(perfobject_s).counters))[0].value) 
| project ServerName_s, TimeGenerated, Value
| summarize avg(todecimal(Value)) by ServerName_s, bin(TimeGenerated, 1m)
| order by TimeGenerated asc 
```

Ihnen stehen Hunderte von Abfragen zur Verwendung zur Verfügung. Weitere Informationen zu Abfragen finden Sie unter [Erste Schritte mit Azure Monitor-Protokollabfragen](../azure-monitor/log-query/get-started-queries.md).


## <a name="turn-on-logging-by-using-powershell"></a>Aktivieren der Protokollierung mit PowerShell

In diesem kurzen Tutorial erstellen Sie ein Speicherkonto im gleichen Abonnement und der gleichen Ressourcengruppe, in denen sich Ihr Analysis Services-Server befindet. Anschließend verwenden Sie „Set-AzDiagnosticSetting“, um die Diagnoseprotokollierung zu aktivieren, damit Ausgaben an das neue Speicherkonto gesendet werden.

### <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie die folgenden Ressourcen:

* Einen vorhandenen Azure Analysis Services-Server. Anweisungen zum Erstellen einer Serverressource finden Sie unter [Erstellen eines Servers im Azure-Portal](analysis-services-create-server.md) oder [Erstellen eines Azure Analysis Services-Servers mithilfe von PowerShell](analysis-services-create-powershell.md).

### <a name="aconnect-to-your-subscriptions"></a></a>Herstellen von Verbindungen mit Ihren Abonnements

Starten Sie eine Azure PowerShell-Sitzung, und melden Sie sich mit dem folgenden Befehl bei Ihrem Azure-Konto an:  

```powershell
Connect-AzAccount
```

Geben Sie im Popup-Browserfenster den Benutzernamen und das Kennwort Ihres Azure-Kontos ein. Azure PowerShell ruft alle Abonnements ab, die diesem Konto zugeordnet sind, und verwendet standardmäßig das erste Abonnement.

Wenn Sie über mehrere Abonnements verfügen, müssen Sie unter Umständen ein bestimmtes Abonnement angeben, das zum Erstellen der Azure Key Vault-Instanz verwendet wurde. Geben Sie Folgendes ein, um die Abonnements für Ihr Konto anzuzeigen:

```powershell
Get-AzSubscription
```

Geben Sie anschließend Folgendes ein, um das Abonnement anzugeben, das dem protokollierten Azure Analysis Services-Konto zugeordnet ist:

```powershell
Set-AzContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> Wenn Ihrem Konto mehrere Abonnements zugeordnet sind, müssen Sie unbedingt das Abonnement angeben.
>
>

### <a name="create-a-new-storage-account-for-your-logs"></a>Erstellen eines neuen Speicherkontos für Ihre Protokolle

Sie können ein vorhandenes Speicherkonto für Ihre Protokolle verwenden, sofern es sich im gleichen Abonnement wie der Server befindet. Für dieses Tutorial erstellen Sie ein neues Speicherkonto für Analysis Services-Protokolle. Der Einfachheit halber speichern Sie die Details zum Speicherkonto in einer Variable mit dem Namen **sa**.

Zudem verwenden Sie dieselbe Ressourcengruppe, in der auch der Analysis Services-Server enthalten ist. Ersetzen Sie die Werte für `awsales_resgroup`, `awsaleslogs` und `West Central US` durch Ihre eigenen Werte:

```powershell
$sa = New-AzStorageAccount -ResourceGroupName awsales_resgroup `
-Name awsaleslogs -Type Standard_LRS -Location 'West Central US'
```

### <a name="identify-the-server-account-for-your-logs"></a>Identifizieren des Serverkontos für Ihre Protokolle

Legen Sie den Kontonamen auf die Variable **account** fest, wobei ResourceName der Name des Kontos ist.

```powershell
$account = Get-AzResource -ResourceGroupName awsales_resgroup `
-ResourceName awsales -ResourceType "Microsoft.AnalysisServices/servers"
```

### <a name="enable-logging"></a>Aktivieren der Protokollierung

Verwenden Sie zum Aktivieren der Protokollierung das Cmdlet „Set-AzDiagnosticSetting“ zusammen mit den Variablen für das neue Speicherkonto, das Serverkonto und die Kategorie. Führen Sie den folgenden Befehl aus, und legen Sie das Flag **-Enabled** auf **$true** fest:

```powershell
Set-AzDiagnosticSetting  -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories Engine
```

Die Ausgabe sollte in etwa wie das folgende Beispiel aussehen:

```powershell
StorageAccountId            : 
/subscriptions/a23279b5-xxxx-xxxx-xxxx-47b7c6d423ea/resourceGroups/awsales_resgroup/providers/Microsoft.Storage/storageAccounts/awsaleslogs
ServiceBusRuleId            :
EventHubAuthorizationRuleId :
Metrics                    
    TimeGrain       : PT1M
    Enabled         : False
    RetentionPolicy
    Enabled : False
    Days    : 0


Logs                       
    Category        : Engine
    Enabled         : True
    RetentionPolicy
    Enabled : False
    Days    : 0


    Category        : Service
    Enabled         : False
    RetentionPolicy
    Enabled : False
    Days    : 0


WorkspaceId                 :
Id                          : /subscriptions/a23279b5-xxxx-xxxx-xxxx-47b7c6d423ea/resourcegroups/awsales_resgroup/providers/microsoft.analysisservic
es/servers/awsales/providers/microsoft.insights/diagnosticSettings/service
Name                        : service
Type                        :
Location                    :
Tags                        :
```

Diese Ausgabe bestätigt, dass die Protokollierung für den Server jetzt aktiviert ist und Informationen im Speicherkonto gespeichert werden.

Sie können außerdem eine Aufbewahrungsrichtlinie für Ihre Protokolle festlegen, sodass ältere Protokolle automatisch gelöscht werden. Richten Sie beispielsweise eine Aufbewahrungsrichtlinie ein, bei der das Flag **-RetentionEnabled** auf **$true** und der Parameter **-RetentionInDays** auf **90** festgelegt sind. Protokolle, die älter als 90 Tage sind, werden dann automatisch gelöscht.

```powershell
Set-AzDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories Engine`
  -RetentionEnabled $true -RetentionInDays 90
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die [Azure Monitor-Ressourcenprotokollierung](../azure-monitor/platform/platform-logs-overview.md).

Weitere Informationen finden Sie in der PowerShell-Hilfe unter [Set-AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting).