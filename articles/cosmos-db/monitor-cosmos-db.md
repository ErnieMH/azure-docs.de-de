---
title: Überwachen von Azure Cosmos DB | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Leistung und Verfügbarkeit von Azure Cosmos DB überwachen.
author: bwren
services: cosmos-db
ms.service: cosmos-db
ms.topic: how-to
ms.date: 08/24/2020
ms.author: bwren
ms.custom: subject-monitoring
ms.openlocfilehash: 12bf87e16bf4506f2015dd75fb360f8de8399902
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88797818"
---
# <a name="monitoring-azure-cosmos-db"></a>Überwachen von Azure Cosmos DB

Wenn Sie über unternehmenskritische Anwendungen und Geschäftsprozesse verfügen, die auf Azure-Ressourcen beruhen, sollten Sie Verfügbarkeit, Leistung und Betrieb dieser Ressourcen überwachen. In diesem Artikel wird das Überwachen von Daten beschrieben, die von Azure Cosmos-Datenbanken generiert wurden. Außerdem wird erläutert, wie Sie die Funktionen von Azure Monitor nutzen können, um diese Daten zu analysieren und Warnungen dafür zu erstellen.

Sie können Ihre Daten mit clientseitigen und serverseitigen Metriken überwachen. Wenn Sie serverseitige Metriken verwenden, können Sie die in Azure Cosmos DB gespeicherten Daten mit den folgenden Optionen überwachen:

* **Überwachen über das Azure Cosmos DB-Portal:** Sie können die Überwachung mit den Metriken durchführen, die über die Registerkarte **Metriken** im Azure Cosmos-Konto verfügbar sind. Die Metriken auf dieser Registerkarte sind unter anderem Durchsatz, Speicher, Verfügbarkeit, Latenz, Konsistenz und Metriken auf Systemebene. Standardmäßig haben diese Metriken eine Beibehaltungsdauer von 7 Tagen. Weitere Informationen finden Sie im Abschnitt [Von Azure Cosmos DB gesammelte Überwachungsdaten](#monitoring-from-azure-cosmos-db) dieses Artikels.

* **Überwachung mit Metriken in Azure Monitor:** Sie können die Metriken Ihres Azure Cosmos-Kontos überwachen und Dashboards über das Azure Monitor-Menü erstellen. Azure Monitor sammelt die Azure Cosmos DB-Metriken standardmäßig, Sie müssen also nichts explizit konfigurieren. Dieses Metriken werden mit einer Granularität von einer Minute erfasst. Die Granularität kann allerdings basierend auf den von Ihnen ausgewählten Metriken variieren. Standardmäßig haben diese Metriken eine Beibehaltungsdauer von 30 Tagen. Die meisten Metriken, die über die vorherigen Optionen verfügbar sind, sind auch in diesen Metriken verfügbar. Bei den Dimensionswerten für die Metriken, wie den Containernamen, wird keine Groß-/Kleinschreibung berücksichtigt. Daher müssen Sie bei Zeichenfolgenvergleichen für diese Dimensionswerte Vergleiche verwenden, bei denen keine Groß-/Kleinschreibung berücksichtigt wird. Weitere Informationen finden Sie im Abschnitt [Analysieren von Metrikdaten](#analyze-metric-data) in diesem Artikel.

* **Überwachen mit Diagnoseprotokollen in Azure Monitor:** Sie können die Protokolle Ihres Azure Cosmos-Kontos überwachen und Dashboards über das Azure Monitor-Menü erstellen. Telemetriedaten wie Ereignisse und Ablaufverfolgungsdaten, die mit einer zweiten Granularität erfolgen, werden als Protokolle gespeichert. Wenn der Durchsatz eines Containers sich beispielsweise ändert und sich die Eigenschaften eines Cosmos-Kontos ändern, werden diese Ereignisse in Protokollen erfasst. Sie können diese Protokolle analysieren, indem Sie Abfragen für die gesammelten Daten ausführen. Weitere Informationen finden Sie im Abschnitt [Analysieren von Protokolldaten](#analyze-log-data) in diesem Artikel.

* **Programmgesteuerte Überwachung mit SDKs:** Sie können Ihr Azure Cosmos-Konto programmgesteuert überwachen, indem Sie die SDKs von .NET, Java, Python, Node.js sowie die Header in REST-API verwenden. Weitere Informationen finden Sie im Abschnitt [Programmgesteuertes Überwachen von Azure Cosmos DB](#monitor-cosmosdb-programmatically) dieses Artikels.

Die folgende Abbildung zeigt unterschiedliche Optionen, die zum Überwachen des Azure Cosmos DB-Kontos über das Azure-Portal verfügbar sind:

:::image type="content" source="media/monitor-cosmos-db/monitoring-options-portal.png" alt-text="Im Azure-Portal verfügbare Überwachungsoptionen" border="false":::

Wenn Sie Azure Cosmos DB verwenden, können Sie auf der Clientseite die Details für die Anforderungsgebühren, die Aktivitäts-ID, die Informationen zu Ausnahmen und der Stapelüberwachung, den HTTP-Status bzw. Unterstatuscode und auch die Diagnosezeichenfolge zum Debuggen von Problemen zu erfassen. Diese Informationen sind auch erforderlich, wenn Sie sich an das Azure Cosmos DB-Supportteam wenden müssen.  

## <a name="what-is-azure-monitor"></a>Was ist Azure Monitor?

Azure Cosmos DB erstellt Überwachungsdaten mit [Azure Monitor](../azure-monitor/overview.md), einem vollständigen Überwachungsdienst in Azure, der sämtliche Features für das Überwachen Ihrer Azure-Ressourcen sowie der Ressourcen in anderen Clouds und lokaler Ressourcen bereitstellt.

Wenn Sie mit der Überwachung von Azure-Diensten noch nicht vertraut sind, beginnen Sie mit dem Artikel [Überwachen von Azure-Ressourcen mit Azure Monitor](../azure-monitor/insights/monitor-azure-resource.md), in dem die folgenden Punkte beschrieben werden:

* Was ist Azure Monitor?
* Kosten für die Überwachung
* In Azure gesammelte Überwachungsdaten
* Konfigurieren der Datensammlung
* Standardtools in Azure zum Analysieren von Überwachungsdaten sowie zum Generieren von Warnungen

Die folgenden Abschnitte basieren auf diesem Artikel, indem sie die spezifischen, von Azure Cosmos DB gesammelten Daten beschreiben, und Beispiele für die Konfiguration der Datensammlung und die Analyse dieser Daten mit Azure-Tools bereitstellen.

## <a name="azure-monitor-for-azure-cosmos-db"></a>Azure Monitor für Azure Cosmos DB

Azure Monitor für Azure Cosmos DB basiert auf der [Arbeitsmappenfunktion von Azure Monitor](../azure-monitor/platform/workbooks-overview.md) und verwendet dieselben Überwachungsdaten, die für Cosmos DB gesammelt wurden, wie in den folgenden Abschnitten beschrieben. Verwenden Sie eine Übersicht über Gesamtleistung, Fehler, Kapazität und Betriebsintegrität aller Ihrer Azure Cosmos DB-Ressourcen in Azure Monitor über eine vereinheitlichte interaktive Oberfläche, und nutzen Sie die anderen Features von Azure Monitor zur ausführlichen Analyse und zum Generieren von Warnungen. Weitere Informationen finden Sie im Artikel [Informationen zu Azure Monitor für Azure Cosmos DB](../azure-monitor/insights/cosmosdb-insights-overview.md).

> [!NOTE]
> Stellen Sie beim Erstellen von Containern sicher, dass Sie nicht zwei Container mit demselben Namen, aber unterschiedlicher Groß-/Kleinschreibung erstellen. Der Grund dafür ist, dass bei einigen Teilen der Azure-Plattform die Groß-/Kleinschreibung nicht beachtet wird, und dies kann zu Verwechslungen/Kollisionen von Telemetriedaten und Aktionen für Container mit solchen Namen führen.

## <a name="monitor-data-collected-from-azure-cosmos-db-portal"></a><a id="monitoring-from-azure-cosmos-db"></a> Überwachen von über das Azure Cosmos DB-Portal gesammelte Daten

Azure Cosmos DB sammelt dieselben Arten von Überwachungsdaten wie andere Azure-Ressourcen, die unter [Überwachen von Daten von Azure-Ressourcen](../azure-monitor/insights/monitor-azure-resource.md#monitoring-data) beschrieben sind. Eine ausführliche Referenz zu den Protokollen und Metriken, die von Azure Cosmos DB erstellt werden, finden Sie unter [Überwachen von Azure Cosmos DB-Daten – Referenz](monitor-cosmos-db-reference.md).

Die Seite **Übersicht** im Azure-Portal für jede Azure Cosmos-Datenbank enthält eine kurze Übersicht über die Datenbanknutzung einschließlich ihrer Anforderung und der Nutzung der stündlichen Abrechnung. Dies sind hilfreiche Informationen, die aber nur einen kleinen Teil der verfügbaren Überwachungsdaten ausmachen. Einige dieser Daten werden automatisch gesammelt und stehen für die Analyse zur Verfügung, sobald Sie die Datenbank erstellen, während Sie mit einer gewissen Konfiguration zusätzliche Datensammlungen aktivieren können.

:::image type="content" source="media/monitor-cosmos-db/overview-page.png" alt-text="Im Azure-Portal verfügbare Überwachungsoptionen":::

## <a name="analyzing-metric-data"></a><a id="analyze-metric-data"></a> Analysieren von Metrikdaten

Azure Cosmos DB bietet ein benutzerdefiniertes Erlebnis für die Arbeit mit Metriken. Ausführliche Informationen zur Verwendung dieser Erfahrung und zur Analyse verschiedener Azure Cosmos DB-Szenarien finden Sie unter [Überwachen und Debuggen von Azure Cosmos DB-Metriken aus Azure Monitor](cosmos-db-azure-monitor-metrics.md).

Sie können Metriken für Azure Cosmos DB mit Metriken aus anderen Azure-Diensten mit dem Metrik-Explorer analysieren, indem Sie **Metriken** aus dem Menü **Azure Monitor** öffnen. Ausführliche Informationen zur Verwendung dieses Tools finden Sie unter [Erste Schritte mit dem Azure-Metrik-Explorer](../azure-monitor/platform/metrics-getting-started.md). Alle Metriken für Azure Cosmos DB befinden sich im Namespace **Cosmos DB-Standardmetriken**. Sie können die folgenden Dimensionen mit diesen Metriken verwenden, wenn Sie einen Filter zu einem Diagramm hinzufügen:

* CollectionName
* DatabaseName
* OperationType
* Region
* StatusCode

### <a name="view-operation-level-metrics-for-azure-cosmos-db"></a>Anzeigen von Metriken auf Vorgangsebene für Azure Cosmos DB

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Wählen Sie auf der Navigationsleiste auf der linken Seite die Option **Monitor** und anschließend **Metrik** aus.

   :::image type="content" source="./media/monitor-cosmos-db/monitor-metrics-blade.png" alt-text="Im Azure-Portal verfügbare Überwachungsoptionen":::

1. Klicken Sie im Bereich **Metriken** auf **Ressource auswählen**. Wählen Sie dann das erforderliche **Abonnement** und die **Ressourcengruppe** aus. Wählen Sie unter **Ressourcentyp** die Option **Azure Cosmos DB-Konten** aus. Wählen Sie dann eins der vorhandenen Azure Cosmos-Konten und anschließend **Anwenden** aus.

   :::image type="content" source="./media/monitor-cosmos-db/select-cosmosdb-account.png" alt-text="Im Azure-Portal verfügbare Überwachungsoptionen":::

1. Als Nächstes können Sie eine Metrik aus der Liste der verfügbaren Metriken auswählen. Sie können spezifische Metriken für Anforderungseinheiten, Speicher, Wartezeit, Verfügbarkeit, Cassandra usw. auswählen. Ausführliche Informationen zu allen verfügbaren Metriken in dieser Liste finden Sie im Artikel [Metriken nach Kategorie](monitor-cosmos-db-reference.md). In diesem Beispiel wählen Sie **Anforderungseinheiten** und als Aggregationswert **Mittelw.** aus.

   Zusätzlich zu diesen Angaben können Sie auch **Zeitbereich** und **Zeitgranularität** für die Metriken auswählen. Sie können Metriken maximal für die letzten 30 Tage anzeigen.  Nach Anwendung des Filters wird ein darauf basierendes Diagramm angezeigt. Sie sehen die durchschnittliche Anzahl der pro Minute verbrauchten Anforderungseinheiten für den ausgewählten Zeitraum.  

   :::image type="content" source="./media/monitor-cosmos-db/metric-types.png" alt-text="Im Azure-Portal verfügbare Überwachungsoptionen":::

### <a name="add-filters-to-metrics"></a>Hinzufügen von Filtern zu Metriken

Sie können Metriken und das angezeigte Diagramm auch nach bestimmten Werten für **CollectionName**, **DatabaseName**, **OperationType**, **Region** und **StatusCode** filtern. Wählen Sie zum Filtern der Metriken **Filter hinzufügen** und dann die erforderliche Eigenschaft aus, etwa **OperationType**. Wählen Sie anschließend einen Wert aus, beispielsweise **Query**. Im Diagramm werden die Anforderungseinheiten angezeigt, die für den Abfragevorgang im ausgewählten Zeitraum verbraucht wurden. Die über eine gespeicherte Prozedur ausgeführten Vorgänge werden nicht protokolliert und sind daher nicht unter der Metrik „OperationType“ verfügbar.

:::image type="content" source="./media/monitor-cosmos-db/add-metrics-filter.png" alt-text="Im Azure-Portal verfügbare Überwachungsoptionen":::

Sie können Metriken mit der Option **Apply splitting** (Aufteilung anwenden) gruppieren. Beispielsweise können Sie die Anforderungseinheiten nach Vorgangstyp gruppieren und das Diagramm für alle Vorgänge auf einmal anzeigen, wie in der folgenden Abbildung dargestellt:

:::image type="content" source="./media/monitor-cosmos-db/apply-metrics-splitting.png" alt-text="Im Azure-Portal verfügbare Überwachungsoptionen":::

## <a name="analyzing-log-data"></a><a id="analyze-log-data"></a> Analysieren von Protokolldaten

Daten in Azure Monitor-Protokollen werden in Tabellen gespeichert, wobei jede Tabelle ihren eigenen Satz eindeutiger Eigenschaften hat. In Azure Cosmos DB werden Daten in den folgenden Tabellen gespeichert.

| Tabelle | BESCHREIBUNG |
|:---|:---|
| AzureDiagnostics | Allgemeine Tabelle, die von mehreren Diensten verwendet wird, um Ressourcenprotokolle zu speichern. Ressourcenprotokolle von Azure Cosmos DB können mit `MICROSOFT.DOCUMENTDB` identifiziert werden.   |
| AzureActivity    | Allgemeine Tabelle, die alle Datensätze aus dem Aktivitätsprotokoll speichert.

> [!IMPORTANT]
> Wenn Sie **Protokolle** im Menü von Azure Cosmos DB auswählen, wird Log Analytics geöffnet, wobei der Abfragebereich auf die aktuelle Azure Cosmos-Datenbank festgelegt ist. Dies bedeutet, dass Protokollabfragen nur Daten aus dieser Ressource umfassen. Wenn Sie eine Abfrage ausführen möchten, die Daten aus anderen Datenbanken oder Daten aus anderen Azure-Diensten enthält, wählen Sie im Menü **Azure Monitor** die Option **Protokolle** aus. Ausführliche Informationen finden Sie unter [Protokollabfragebereich und Zeitbereich in Azure Monitor Log Analytics](../azure-monitor/log-query/scope.md).

### <a name="azure-cosmos-db-log-analytics-queries-in-azure-monitor"></a>Azure Cosmos DB Log Analytics-Abfragen in Azure Monitor

Hier sind einige Abfragen, die Sie in die Suchleiste **Protokollsuche** eingeben können, um die Überwachung Ihrer Azure Cosmos-Container zu vereinfachen. Diese Abfragen arbeiten mit der [neuen Sprache](../log-analytics/log-analytics-log-search-upgrade.md).

Die folgenden Abfragen sind Abfragen, mit denen Sie Ihre Azure Cosmos-Datenbanken überwachen können:

* Das folgende Beispiel zeigt eine Abfrage für alle Diagnoseprotokolle von Azure Cosmos DB für einen angegebenen Zeitraum:

    ```Kusto
    AzureDiagnostics 
    | where ResourceProvider=="Microsoft.DocumentDb" and Category=="DataPlaneRequests"

    ```

* So fragen Sie alle Vorgänge gruppiert nach Ressource ab:

    ```Kusto
    AzureActivity 
    | where ResourceProvider=="Microsoft.DocumentDb" and Category=="DataPlaneRequests" 
    | summarize count() by Resource

    ```

* So fragen Sie die gesamte Benutzeraktivität gruppiert nach Ressource ab:

    ```Kusto
    AzureActivity 
    | where Caller == "test@company.com" and ResourceProvider=="Microsoft.DocumentDb" and Category=="DataPlaneRequests" 
    | summarize count() by Resource
    ```

## <a name="monitor-azure-cosmos-db-programmatically"></a><a id="monitor-cosmosdb-programmatically"></a> Programmgesteuertes Überwachen von Azure Cosmos DB

Die im Portal für Konten verfügbaren Metriken, z. B. für die Speichernutzung von Konten und die Gesamtzahl der Anforderungen, stehen über die SQL-APIs nicht zur Verfügung. Sie können jedoch mithilfe der SQL-APIs die Nutzungsdaten auf Sammlungsebene abrufen. Gehen Sie zum Abrufen von Daten auf Sammlungsebene wie folgt vor:

* Führen Sie zur Verwendung der REST-API [einen GET-Befehl für die Sammlung](https://msdn.microsoft.com/library/mt489073.aspx)aus. Die Kontingent- und Nutzungsinformationen für die Sammlung werden in den Headern „x-ms-resource-quota“ und „x-ms-resource-usage“ in der Antwort zurückgegeben.

* Verwenden Sie für das .NET SDK die Methode [DocumentClient.ReadDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.readdocumentcollectionasync.aspx), die eine [ResourceResponse](https://msdn.microsoft.com/library/dn799209.aspx) mit mehreren Nutzungseigenschaften zurückgibt, z.B. **CollectionSizeUsage**, **DatabaseUsage**, **DocumentUsage** sowie weitere Eigenschaften.

Für den Zugriff auf weitere Metriken verwenden Sie das [Azure Monitor SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights). Verfügbare Metrikdefinitionen können folgendermaßen aufgerufen werden:

```http
https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/providers/microsoft.insights/metricDefinitions?api-version=2018-01-01
```

Verwenden Sie zum Abrufen einzelner Metriken das folgende Format:

```http
https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroup}/providers/Microsoft.DocumentDb/databaseAccounts/{DocumentDBAccountName}/providers/microsoft.insights/metrics?timespan={StartTime}/{EndTime}&interval={AggregationInterval}&metricnames={MetricName}&aggregation={AggregationType}&`$filter={Filter}&api-version=2018-01-01
```

Weitere Informationen finden Sie im Artikel [Azure Monitoring-Rest-API](../azure-monitor/platform/rest-api-walkthrough.md).

## <a name="next-steps"></a>Nächste Schritte

* Eine Referenz zu den Protokollen und Metriken, die von Azure Cosmos DB erstellt werden, finden Sie unter [Überwachen von Azure Cosmos DB-Daten – Referenz](monitor-cosmos-db-reference.md).
* Ausführliche Informationen zur Überwachung von Azure-Ressourcen finden Sie unter [Überwachen von Azure-Ressourcen mit Azure Monitor](../azure-monitor/insights/monitor-azure-resource.md).
