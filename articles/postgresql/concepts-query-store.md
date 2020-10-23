---
title: Abfragespeicher – Azure Database for PostgreSQL (Einzelserver)
description: In diesem Artikel wird das Abfragespeicherfeature in Azure Database for PostgreSQL (Einzelserver) beschrieben.
author: sunilagarwal
ms.author: sunila
ms.service: postgresql
ms.topic: conceptual
ms.date: 07/01/2020
ms.openlocfilehash: 7b6c8faafac34ada664ddfadebf8d71a16c73fa7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91710531"
---
# <a name="monitor-performance-with-the-query-store"></a>Überwachen der Leistung mit dem Abfragespeicher

**Anwendungsbereich:** Azure Database for PostgreSQL – Einzelserverversionen 9.6 und höher

Das Abfragespeicherfeature in Azure Database for PostgreSQL bietet eine Möglichkeit, um die Abfrageleistung im Zeitverlauf nachzuverfolgen. Der Abfragespeicher vereinfacht das Beheben von Leistungsproblemen, da er es Ihnen ermöglicht, die am längsten ausgeführten und ressourcenintensivsten Abfragen schnell zu ermitteln. Der Abfragespeicher erfasst automatisch einen Verlauf der Abfragen und Laufzeitstatistiken und bewahrt diese auf, damit Sie sie überprüfen können. Er unterteilt die Daten nach Zeitfenstern, damit Sie Verwendungsmuster für Datenbanken erkennen können. Die Daten für alle Benutzer, Datenbanken und Abfragen werden in einer Datenbank namens **azure_sys** in der Azure Database for PostgreSQL-Instanz gespeichert.

> [!IMPORTANT]
> Nehmen Sie an der **azure_sys**-Datenbank oder ihren Schemas keine Änderungen vor. Andernfalls funktionieren der Abfragespeicher und die entsprechenden Leistungsfeatures nicht mehr ordnungsgemäß.

## <a name="enabling-query-store"></a>Aktivieren des Abfragespeichers
Der Abfragespeicher ist ein optionales Feature. Daher ist er auf einem Server nicht standardmäßig aktiviert. Der Speicher wird global für alle Datenbanken auf einem bestimmten Server aktiviert oder deaktiviert. Ein Aktivieren oder Deaktivieren für einzelne Datenbanken ist nicht möglich.

### <a name="enable-query-store-using-the-azure-portal"></a>Aktivieren des Abfragespeichers über das Azure-Portal
1. Melden Sie sich beim Azure-Portal an, und wählen Sie Ihren Azure Database for PostgreSQL-Server aus.
2. Wählen Sie im Bereich **Einstellungen** im Menü die Option **Serverparameter**.
3. Suchen Sie nach dem Parameter `pg_qs.query_capture_mode`.
4. Legen Sie für den Wert `TOP` fest und **Speichern**.

So aktivieren Sie Wartestatistiken in Ihrem Abfragespeicher: 
1. Suchen Sie nach dem Parameter `pgms_wait_sampling.query_capture_mode`.
1. Legen Sie für den Wert `ALL` fest und **Speichern**.


Alternativ können Sie diese Parameter über die Azure-Befehlszeilenschnittstelle festlegen.
```azurecli-interactive
az postgres server configuration set --name pg_qs.query_capture_mode --resource-group myresourcegroup --server mydemoserver --value TOP
az postgres server configuration set --name pgms_wait_sampling.query_capture_mode --resource-group myresourcegroup --server mydemoserver --value ALL
```

Es kann bis zu 20 Minuten dauern, bis der erste Datenbatch in der azure_sys-Datenbank gespeichert ist.

## <a name="information-in-query-store"></a>Informationen im Abfragespeicher
Der Abfragespeicher verfügt über zwei Speicher:
- Ein Speicher für Laufzeitstatistiken zum Aufbewahren der Informationen aus den Abfragespeicherstatistiken.
- Ein Speicher für Wartestatistiken zum Aufbewahren der Informationen aus den Wartestatistiken.

Häufige Szenarien für die Verwendung des Abfragespeichers sind u.a.:
- Bestimmen der Häufigkeit zum Ausführen einer Abfrage in einem angegebenen Zeitfenster
- Vergleichen der durchschnittlichen Ausführungsdauer einer Abfrage für bestimmte Zeitfenster zum Anzeigen großer Abweichungen
- Identifizieren der am längsten ausgeführten Abfragen in der vergangenen X Stunden
- Identifizieren der wichtigsten N Abfragen, die auf Ressourcen warten
- Erhalten von Wartedetails einer bestimmten Abfrage

Um die Speicherverwendung zu minimieren, werden die Laufzeit-Ausführungsstatistiken im Speicher für Laufzeitstatistiken für ein festes, konfigurierbares Zeitfenster zusammengefasst. Die Informationen in diesem Speicher können durch Abfragen der Abfragespeicheransichten angezeigt werden.

## <a name="access-query-store-information"></a>Zugreifen auf Abfragespeicherinformationen

Abfragespeicherdaten werden in der Datenbank „azure_sys“ auf Ihrem Postgres-Server gespeichert. 

Die folgende Abfrage gibt Informationen über Abfragen im Abfragespeicher zurück:
```sql
SELECT * FROM query_store.qs_view; 
``` 

Diese Abfrage gibt Informationen über Wartestatistiken zurück:
```sql
SELECT * FROM query_store.pgms_wait_sampling_view;
```

## <a name="finding-wait-queries"></a>Suchen von Warteanfragen
Warteereignistypen kombinieren verschiedene Warteereignisse nach Ähnlichkeit in Buckets. Der Abfragespeicher enthält den Warteereignistyp, den spezifischen Warteereignisnamen und die entsprechende Abfrage. Die Möglichkeit zum Korrelieren dieser Warteinformationen mit den Abfragelaufzeitstatistiken bedeutet, dass Sie einen ausführlicheren Überblick darüber erhalten, welche Faktoren sich auf die Abfrageleistung auswirken.

Im Folgenden finden Sie einige Beispiele dafür, wie Sie mithilfe der Wartestatistiken im Abfragespeicher weitere Erkenntnisse zur Ihrer Workload erhalten:

| **Beobachtung** | **Aktion** |
|---|---|
|Lange Sperrwartevorgänge | Überprüfen Sie die Abfragetexte der betroffenen Abfragen, und identifizieren Sie die Zielentitäten. Suchen Sie im Abfragespeicher nach anderen Abfragen, die die gleiche Entität ändern, welche häufig ausgeführt wird bzw. eine lange Dauer aufweist. Nachdem Sie diese Abfragen ermittelt haben, ändern Sie ggf. die Anwendungslogik, um die Parallelität zu verbessern, oder verwenden Sie eine weniger restriktive Isolationsstufe.|
| Lange Puffer-E/A-Wartevorgänge | Suchen Sie die Abfragen mit einer hohen Anzahl an physischen Lesevorgängen im Abfragespeicher. Wenn diese mit Abfragen mit langen E/A-Wartevorgängen übereinstimmen, führen Sie ggf. einen Index für die zugrunde liegenden Entität ein, um Such- anstelle von Scanvorgängen durchzuführen. Dies verringert den E/A-Aufwand der Abfragen. Überprüfen Sie die **Leistungsempfehlungen** für Ihren Server im Portal, um festzustellen, ob Indexempfehlungen für diesen Server vorhanden sind, die die Abfragen optimieren.|
| Lange Arbeitsspeicher-Wartevorgänge | Suchen Sie die im Abfragespeicher die speicherintensivsten Abfragen. Diese Abfragen verzögern wahrscheinlich zusätzlich den Fortschritt der betroffen Abfragen. Überprüfen Sie die **Leistungsempfehlungen** für Ihren Server im Portal, um festzustellen, ob Indexempfehlungen vorhanden sind, die diese Abfragen optimieren.|

## <a name="configuration-options"></a>Konfigurationsoptionen
Wenn der Abfragespeicher aktiviert ist, speichert er Daten in Aggregationsfenstern von 15 Minuten mit bis zu 500 unterschiedlichen Abfragen pro Fenster. 

Die folgenden Optionen stehen für die Konfiguration der Abfragespeicherparameter zur Verfügung.

| **Parameter** | **Beschreibung** | **Standard** | **Bereich**|
|---|---|---|---|
| pg_qs.query_capture_mode | Legt fest, welche Anweisungen nachverfolgt werden. | none | none, top, all |
| pg_qs.max_query_text_length | Legt die maximale Abfragelänge fest, die gespeichert werden kann. Längere Abfragen werden abgeschnitten. | 6000 | 100 – 10.000 |
| pg_qs.retention_period_in_days | Legt den Aufbewahrungszeitraum fest. | 7 | 1 – 30 |
| pg_qs.track_utility | Legt fest, ob Dienstprogrammbefehle nachverfolgt werden. | on | on, off |

Die folgenden Optionen gelten speziell für Wartestatistiken.

| **Parameter** | **Beschreibung** | **Standard** | **Bereich**|
|---|---|---|---|
| pgms_wait_sampling.query_capture_mode | Legt fest, welche Anweisungen in Bezug auf Wartestatistiken nachverfolgt werden. | none | none, all|
| Pgms_wait_sampling.history_period | Legt die Häufigkeit in Millisekunden fest, mit der Stichproben von Wartezeitereignissen erfasst werden. | 100 | 1 – 600000 |

> [!NOTE] 
> **pg_qs.query_capture_mode** ersetzt **pgms_wait_sampling.query_capture_mode**. Wenn für pg_qs.query_capture_mode NONE festgelegt ist, hat die Einstellung pgms_wait_sampling.query_capture_mode keine Auswirkungen.


Verwenden Sie das [Azure-Portal](howto-configure-server-parameters-using-portal.md) oder die [Azure-Befehlszeilenschnittstelle](howto-configure-server-parameters-using-cli.md) zum Abrufen oder Festlegen eines anderen Werts für einen Parameter.

## <a name="views-and-functions"></a>Ansichten und Funktionen
Mithilfe der folgenden Ansichten und Funktionen können Sie den Abfragespeicher anzeigen und verwalten. In der öffentlichen PostgreSQL-Rolle kann jeder diese Ansichten verwenden, um die Daten im Abfragespeicher anzuzeigen. Diese Ansichten sind nur in der **azure_sys**-Datenbank verfügbar.

Abfragen werden normalisiert, indem ihre Struktur nach dem Entfernen von Literalen und Konstanten untersucht wird. Wenn zwei Abfragen mit Ausnahme von Literalwerten identisch sind, haben sie denselben Hash.

### <a name="query_storeqs_view"></a>query_store.qs_view
In dieser Ansicht werden alle Daten im Abfragespeicher zurückgegeben. Es gibt eine Zeile für jede einzelne Datenbank-ID, Benutzer-ID und Abfrage-ID. 

|**Name**   |**Typ** | **Referenzen**  | **Beschreibung**|
|---|---|---|---|
|runtime_stats_entry_id |BIGINT | | Die ID aus der Tabelle runtime_stats_entries|
|user_id    |oid    |pg_authid.oid  |Die OID des Benutzers, der die Anweisung ausgeführt hat|
|db_id  |oid    |pg_database.oid    |Die OID der Datenbank, in der die Anweisung ausgeführt wurde|
|query_id   |BIGINT  || Interner Hash, der von der Analysestruktur der Anweisung berechnet wurde|
|query_sql_text |Varchar(10000)  || Der Text einer repräsentativen Anweisung. Unterschiedliche Abfragen mit der gleichen Struktur werden gruppiert; dieser Text ist der Text für die erste Abfrage im Cluster.|
|plan_id    |BIGINT |   |ID des Plans, der dieser Abfrage entspricht, noch nicht verfügbar|
|start_time |timestamp  ||  Abfragen werden nach Zeitrahmen zusammengefasst. Die Zeitspanne eines Zeitrahmens beträgt standardmäßig 15 Minuten. Dies ist die Startzeit des Zeitrahmens für diesen Eintrag.|
|end_time   |timestamp  ||  Dies ist die Endzeit des Zeitrahmens für diesen Eintrag.|
|calls  |BIGINT  || Häufigkeit der Abfrageausführung|
|total_time |double precision   ||  Gesamte Abfrageausführungsdauer in Millisekunden|
|min_time   |double precision   ||  Mindestwert der Abfrageausführungsdauer in Millisekunden|
|max_time   |double precision   ||  Höchstwert der Abfrageausführungsdauer in Millisekunden|
|mean_time  |double precision   ||  Durchschnittliche Abfrageausführungsdauer in Millisekunden|
|stddev_time|   double precision    ||  Standardabweichung der Abfrageausführungsdauer in Millisekunden |
|rows   |BIGINT ||  Gesamtanzahl der Zeilen, die abgerufen wurden oder von der Anweisung betroffen sind|
|shared_blks_hit|   BIGINT  ||  Gesamtanzahl der freigegebenen Blockcachetreffer der Anweisung|
|shared_blks_read|  BIGINT  ||  Gesamtanzahl der freigegebenen Blöcke, die von der Anweisung gelesen wurden|
|shared_blks_dirtied|   BIGINT   || Gesamtanzahl der freigegebenen Blöcke, die von der Anweisung geändert wurden |
|shared_blks_written|   BIGINT  ||  Gesamtanzahl der freigegebenen Blöcke, die von der Anweisung geschrieben wurden|
|local_blks_hit|    BIGINT ||   Gesamtanzahl der lokalen Blockcachetreffer der Anweisung|
|local_blks_read|   BIGINT   || Gesamtanzahl der lokalen Blöcke, die von der Anweisung gelesen wurden|
|local_blks_dirtied|    BIGINT  ||  Gesamtanzahl der lokalen Blöcke, die von der Anweisung geändert wurden|
|local_blks_written|    BIGINT  ||  Gesamtanzahl der lokalen Blöcke, die von der Anweisung geschrieben wurden|
|temp_blks_read |BIGINT  || Gesamtanzahl der temporären Blöcke, die von der Anweisung gelesen wurden|
|temp_blks_written| BIGINT   || Gesamtanzahl der temporären Blöcke, die von der Anweisung geschrieben wurden|
|blk_read_time  |double precision    || Gesamte Zeit, die die Anweisung zum Lesen von Blöcken benötigt hat, in Millisekunden (wenn track_io_timing aktiviert ist, andernfalls Null)|
|blk_write_time |double precision    || Gesamte Zeit, die die Anweisung zum Schreiben von Blöcken benötigt hat, in Millisekunden (wenn track_io_timing aktiviert ist, andernfalls Null)|
    
### <a name="query_storequery_texts_view"></a>query_store.query_texts_view
Diese Ansicht gibt alle Abfragetextdaten im Abfragespeicher zurück. Für jeden einzelnen query_text gibt es eine Zeile.

|**Name**|  **Typ**|   **Beschreibung**|
|---|---|---|
|query_text_id  |BIGINT     |ID der Tabelle query_texts|
|query_sql_text |Varchar(10000)     |Der Text einer repräsentativen Anweisung. Unterschiedliche Abfragen mit der gleichen Struktur werden gruppiert; dieser Text ist der Text für die erste Abfrage im Cluster.|

### <a name="query_storepgms_wait_sampling_view"></a>query_store.pgms_wait_sampling_view
Diese Ansicht gibt Warteereignisdaten im Abfragespeicher zurück. Es gibt eine Zeile für jede einzelne Datenbank-ID, Benutzer-ID und jedes Ereignis.

|**Name**|  **Typ**|   **Referenzen**| **Beschreibung**|
|---|---|---|---|
|user_id    |oid    |pg_authid.oid  |Die OID des Benutzers, der die Anweisung ausgeführt hat|
|db_id  |oid    |pg_database.oid    |Die OID der Datenbank, in der die Anweisung ausgeführt wurde|
|query_id   |BIGINT     ||Interner Hash, der von der Analysestruktur der Anweisung berechnet wurde|
|event_type |text       ||Der Typ des Ereignisses, auf das das Back-End wartet|
|Ereignis  |text       ||Der Warteereignisname, wenn das Back-End derzeit wartet|
|calls  |Integer        ||Nummer des gleichen erfassten Ereignisses|


### <a name="functions"></a>Functions
Query_store.qs_reset() gibt „void“ zurück

`qs_reset` verwirft alle Statistiken, die bisher vom Abfragespeicher gesammelt werden. Diese Funktion kann nur von der Serveradministratorrolle ausgeführt werden.

Query_store.staging_data_reset() gibt „void“ zurück

`staging_data_reset` verwirft alle Statistiken, die vom Abfragespeicher im Arbeitsspeicher erfasst werden (d.h. die Daten im Arbeitsspeicher, für die noch kein Flushvorgang in die Datenbank durchgeführt wurde). Diese Funktion kann nur von der Serveradministratorrolle ausgeführt werden.


## <a name="azure-monitor"></a>Azure Monitor
Azure Database for PostgreSQL ist in die [Azure Monitor-Diagnoseeinstellungen](../azure-monitor/platform/diagnostic-settings.md) integriert. Mit Diagnoseeinstellungen können Sie Ihre Postgres-Protokolle im JSON-Format an [Azure Monitor-Protokolle](../azure-monitor/log-query/log-query-overview.md) (Analyse und Warnungen), Event Hubs (Streaming) und Azure Storage (Archivierung) senden.

>[!IMPORTANT]
> Dieses Diagnosefeature steht nur in den Tarifen „Universell“ und „Arbeitsspeicheroptimiert“ zur Verfügung.

### <a name="configure-diagnostic-settings"></a>Konfigurieren von Diagnoseeinstellungen
Sie können die Diagnoseeinstellungen für Ihren Postgres-Server über Azure-Portal, die CLI, die REST-API und PowerShell aktivieren. Die zu konfigurierenden Protokollkategorien sind **QueryStoreRuntimeStatistics** und **QueryStoreWaitStatistics**. 

So aktivieren Sie Ressourcenprotokolle über das Azure-Portal:

1. Wechseln Sie im Portal im Navigationsmenü Ihres Postgres-Servers zu „Diagnoseeinstellungen“.
2. Wählen Sie „Diagnoseeinstellung hinzufügen“ aus.
3. Benennen Sie die Einstellung.
4. Wählen Sie Ihren bevorzugten Endpunkt aus (Speicherkonto, Event Hub, Log Analytics).
5. Wählen Sie die Protokolltypen **QueryStoreRuntimeStatistics** und **QueryStoreWaitStatistics** aus.
6. Speichern Sie die Einstellungen.

Informationen zum Aktivieren dieser Einstellung über PowerShell, die CLI oder die REST-API finden Sie im [Artikel zu den Diagnoseeinstellungen](../azure-monitor/platform/diagnostic-settings.md).

### <a name="json-log-format"></a>JSON-Protokollformat
In den folgenden Tabellen werden die Felder für die beiden Protokolltypen beschrieben. Je nach dem ausgewählten Ausgabeendpunkt können die enthaltenen Felder und ihre Reihenfolge variieren.

#### <a name="querystoreruntimestatistics"></a>QueryStoreRuntimeStatistics
|**Feld** | **Beschreibung** |
|---|---|
| TimeGenerated [UTC] | Zeitstempel für den Aufzeichnungsbeginn des Protokolls in UTC |
| resourceId | Der Azure-Ressourcen-URI für den Postgres-Server |
| Category | `QueryStoreRuntimeStatistics` |
| Vorgangsname | `QueryStoreRuntimeStatisticsEvent` |
| LogicalServerName_s | Der Name des Postgres-Servers | 
| runtime_stats_entry_id_s | Die ID aus der Tabelle runtime_stats_entries |
| user_id_s | Die OID des Benutzers, der die Anweisung ausgeführt hat |
| db_id_s | Die OID der Datenbank, in der die Anweisung ausgeführt wurde |
| query_id_s | Interner Hash, der von der Analysestruktur der Anweisung berechnet wurde |
| end_time_s | Die Endzeit des Zeitrahmens für diesen Eintrag |
| calls_s | Häufigkeit der Abfrageausführung |
| total_time_s | Gesamte Abfrageausführungsdauer in Millisekunden |
| min_time_s | Mindestwert der Abfrageausführungsdauer in Millisekunden |
| max_time_s | Höchstwert der Abfrageausführungsdauer in Millisekunden |
| mean_time_s | Durchschnittliche Abfrageausführungsdauer in Millisekunden |
| ResourceGroup | Die Ressourcengruppe | 
| SubscriptionId | Ihre Abonnement-ID |
| ResourceProvider | `Microsoft.DBForPostgreSQL` | 
| Resource | Der Name des Postgres-Servers |
| ResourceType | `Servers` | 


#### <a name="querystorewaitstatistics"></a>QueryStoreWaitStatistics
|**Feld** | **Beschreibung** |
|---|---|
| TimeGenerated [UTC] | Zeitstempel für den Aufzeichnungsbeginn des Protokolls in UTC |
| resourceId | Der Azure-Ressourcen-URI für den Postgres-Server |
| Category | `QueryStoreWaitStatistics` |
| Vorgangsname | `QueryStoreWaitEvent` |
| user_id_s | Die OID des Benutzers, der die Anweisung ausgeführt hat |
| db_id_s | Die OID der Datenbank, in der die Anweisung ausgeführt wurde |
| query_id_s | Der interne Hash der Abfrage |
| calls_s | Nummer des gleichen erfassten Ereignisses |
| event_type_s | Der Typ des Ereignisses, auf das das Back-End wartet |
| event_s | Der Warteereignisname, wenn das Back-End derzeit wartet |
| start_time_t | Die Startzeit des Ereignisses |
| end_time_s | Die Endzeit des Ereignisses | 
| LogicalServerName_s | Der Name des Postgres-Servers | 
| ResourceGroup | Die Ressourcengruppe | 
| SubscriptionId | Ihre Abonnement-ID |
| ResourceProvider | `Microsoft.DBForPostgreSQL` | 
| Resource | Der Name des Postgres-Servers |
| ResourceType | `Servers` | 

## <a name="limitations-and-known-issues"></a>Einschränkungen und bekannte Probleme
- Wenn ein PostgreSQL-Server über den Parameter „default_transaction_read_only“ verfügt, kann der Abfragespeicher keine Daten erfassen.
- Die Abfragespeicherfunktionalität kann unterbrochen werden, wenn lange Unicodeabfragen (mindestens 6000 Bytes) festgestellt werden.
- [Lesereplikate](concepts-read-replicas.md) replizieren Abfragespeicherdaten des primären Servers. Das bedeutet, dass der Abfragespeicher eines Lesereplikats keine Statistikdaten zu Abfragen bereitstellt, die für das Lesereplikat ausgeführt werden.


## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Szenarien, in denen der Abfragespeicher hilfreich ist](concepts-query-store-scenarios.md).
- Erfahren Sie mehr über die [bewährten Methoden für die Verwendung des Abfragespeichers](concepts-query-store-best-practices.md).
