---
title: Verwalten des Zwillingsgraphen mit Beziehungen
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie Sie einen Graphen von digitalen Zwillingen verwalten, indem Sie sie über Beziehungen verbinden.
author: baanders
ms.author: baanders
ms.date: 4/10/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 8c698cdf5b26cb1682eec2828922517cf4272275
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92048439"
---
# <a name="manage-a-graph-of-digital-twins-using-relationships"></a>Verwalten eines Graphen von digitalen Zwillingen mithilfe von Beziehungen

Das Herzstück von Azure Digital Twins ist der [Zwillingsgraph](concepts-twins-graph.md), der Ihre gesamte Umgebung darstellt. Der Zwillingsgraph setzt sich aus einzelnen digitalen Zwillingen zusammen, die über **Beziehungen** verbunden sind.

Sobald Sie über eine funktionierende [Azure Digital Twins-Instanz](how-to-set-up-instance-portal.md) verfügen und [Authentifizierungscode](how-to-authenticate-client.md) in Ihrer Client-App eingerichtet haben, können Sie die [**DigitalTwins-APIs**](how-to-use-apis-sdks.md) verwenden, um digitale Zwillinge und ihre Beziehungen in einer Azure Digital Twins-Instanz zu erstellen, zu ändern und zu löschen. Sie können auch das [.NET (C#) SDK](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core) oder die [Azure Digital Twins-CLI](how-to-use-cli.md) verwenden.

Dieser Artikel konzentriert sich auf die Verwaltung von Beziehungen und den Graphen als Ganzes. Für die Arbeit mit einzelnen digitalen Zwillingen finden Sie weitere Informationen unter [ *Verwalten digitaler Zwillinge*](how-to-manage-twin.md).

[!INCLUDE [visualizing with Azure Digital Twins explorer](../../includes/digital-twins-visualization.md)]

## <a name="create-relationships"></a>Erstellen von Beziehungen

Beziehungen beschreiben, wie verschiedene digitale Zwillinge miteinander verbunden sind, was die Grundlage des Zwillingsgraphen bildet.

Beziehungen werden mit dem `CreateRelationship`-Aufruf erstellt. 

Sie müssen Folgendes angeben, um eine Beziehung zu erstellen:
* Die ID des Quellzwillings (der Zwilling, von dem die Beziehung ausgeht)
* Die ID des Zielzwillings (der Zwilling, bei dem die Beziehung eintrifft)
* Ein Beziehungsname
* Eine Beziehungs-ID

Die Beziehungs-ID muss innerhalb des angegebenen Quellzwillings eindeutig sein. Sie muss nicht global eindeutig sein.
Für den Zwilling *foo* muss z. B. jede bestimmte Beziehungs-ID eindeutig sein. Ein weiterer Zwilling *bar* kann jedoch eine ausgehende Beziehung aufweisen, die derselben ID einer *foo*-Beziehung entspricht. 

Im folgenden Codebeispiel wird veranschaulicht, wie Sie Ihrer Azure Digital Twins-Instanz eine Beziehung hinzufügen.

```csharp
public async static Task CreateRelationship(DigitalTwinsClient client, string srcId, string targetId)
{
    var relationship = new BasicRelationship
    {
        TargetId = targetId,
        Name = "contains"
    };

    try
    {
        string relId = $"{srcId}-contains->{targetId}";
        await client.CreateRelationshipAsync(srcId, relId, JsonSerializer.Serialize(relationship));
        Console.WriteLine("Created relationship successfully");
    }
    catch (RequestFailedException rex) {
        Console.WriteLine($"Create relationship error: {rex.Status}:{rex.Message}");
    }
}
```

Weitere Informationen zur Hilfsklasse `BasicRelationship` finden Sie unter [ *Verwenden der Azure Digital Twins-APIs und SDKs*](how-to-use-apis-sdks.md).

### <a name="create-multiple-relationships-between-twins"></a>Erstellen mehrerer Beziehungen zwischen Zwillingen

Zwischen Zwillingen können beliebig viele Beziehungen vorhanden sein. 

Das bedeutet, Sie können gleichzeitig verschiedene Arten von Beziehungen zwischen zwei Zwillingen ausdrücken. Ein Beispiel: *Zwilling A* kann sowohl eine *gespeicherte* als auch eine *erstellte* Beziehung mit *Zwilling B* haben.

Auf Wunsch können sogar mehrere Instanzen der gleichen Art von Beziehung zwischen den gleichen beiden Zwillingen erstellt werden. In diesem Beispiel bedeutet das, dass *Zwilling A* über zwei individuelle *gespeicherte* Beziehungen mit *Zwilling B* verfügen kann.

## <a name="list-relationships"></a>Auflisten von Beziehungen

Sie können Folgendes verwenden, um auf die Liste der **ausgehenden** Beziehungen zuzugreifen, die von einem bestimmten Zwilling im Graphen stammen:

```csharp
await client.GetRelationshipsAsync(id);
```

Dies gibt ein `Azure.Pageable<T>` oder `Azure.AsyncPageable<T>` zurück, je nachdem, ob Sie die synchrone oder asynchrone Version des Aufrufs verwenden.

Im Folgenden finden Sie ein vollständiges Beispiel, das eine Liste von Beziehungen abruft:

```csharp
public async Task<List<BasicRelationship>> FindOutgoingRelationshipsAsync(string dtId)
{
    // Find the relationships for the twin
    try
    {
        // GetRelationshipsAsync will throw if an error occurs
        AsyncPageable<string> relsJson = client.GetRelationshipsAsync(dtId);
        List<BasicRelationship> results = new List<BasicRelationship>();
        await foreach (string relJson in relsJson)
        {
            var rel = System.Text.Json.JsonSerializer.Deserialize<BasicRelationship>(relJson);
            results.Add(rel);
        }
        return results;
    }
    catch (RequestFailedException ex)
    {
        Log.Error($"**_ Error {ex.Status}/{ex.ErrorCode} retrieving relationships for {dtId} due to {ex.Message}");
        return null;
    }
}
```

Sie können die abgerufenen Beziehungen verwenden, um zu anderen Zwillingen in Ihrem Graphen zu navigieren. Lesen Sie dazu das Feld `target` aus der zurückgegebenen Beziehung, und verwenden Sie es als ID für Ihren nächsten Aufruf von `GetDigitalTwin`. 

### <a name="find-incoming-relationships-to-a-digital-twin"></a>Suchen von eingehenden Beziehungen zu einem digitalen Zwilling

Azure Digital Twins verfügt auch über eine API, um alle **eingehenden** Beziehungen für einen bestimmten Zwilling zu finden. Dies ist oft nützlich bei der umgekehrten Navigation oder beim Löschen eines Zwillings.

Das Codebeispiel oben hat sich auf die Suche nach ausgehenden Beziehungen von einem Zwilling konzentriert. Das folgende Beispiel ist ähnlich strukturiert, findet aber stattdessen *eingehende* Beziehungen vom Zwilling.

Beachten Sie, dass die `IncomingRelationship`-Aufrufe nicht den vollständigen Text der Beziehung zurückgeben.

```csharp
async Task<List<IncomingRelationship>> FindIncomingRelationshipsAsync(string dtId)
{
    // Find the relationships for the twin
    try
    {
        // GetRelationshipsAsync will throw an error if a problem occurs
        AsyncPageable<IncomingRelationship> incomingRels = client.GetIncomingRelationshipsAsync(dtId);

        List<IncomingRelationship> results = new List<IncomingRelationship>();
        await foreach (IncomingRelationship incomingRel in incomingRels)
            results.Add(incomingRel);
    }
    catch (RequestFailedException ex)
    {
        Log.Error($"*** Error {ex.Status}/{ex.ErrorCode} retrieving incoming relationships for {dtId} due to {ex.Message}");
    }
}
```

## <a name="delete-relationships"></a>Löschen von Beziehungen

Sie können Beziehungen mit `DeleteRelationship(source, relId);` löschen.

Der erste Parameter gibt den Quellzwilling an (der Zwilling, von dem die Beziehung ausgeht). Der andere Parameter ist die Beziehungs-ID. Sie benötigen sowohl die Zwillings-ID als auch die Beziehungs-ID, da Beziehungs-IDs nur innerhalb des Bereichs eines Zwillings eindeutig sind.

## <a name="create-a-twin-graph"></a>Erstellen eines Zwillingsgraphen 

Der folgende Codeausschnitt verwendet die Beziehungsvorgänge aus diesem Artikel, um einen Zwillingsgraphen aus digitalen Zwillingen und Beziehungen zu erstellen.

```csharp
static async Task CreateTwins()
{
    // Create twins - see utility functions below 
    await CreateRoom("Room01", 68, 50, false, "");
    await CreateRoom("Room02", 70, 66, true, "EId-00124");
    await CreateFloorOrBuilding("Floor01", makeFloor:true);

    // Create relationships
    await AddRelationship("Floor01", "contains", "Floor-to-Room01", "Room01");
    await AddRelationship("Floor01", "contains", "Floor-to-Room02", "Room02");
}

static async Task<bool> AddRelationship(string source, string relationship, string id, string target)
{
    var relationship = new BasicRelationship
    {
        TargetId = target,
        Name = relationship
    };

    try
    {
        string relId = $"{source}-contains->{target}";
        await client.CreateRelationshipAsync(source, relId, JsonSerializer.Serialize(relationship));
        Console.WriteLine("Created relationship successfully");
        return true;
    }
    catch (RequestFailedException rex) {
        Console.WriteLine($"Create relationship error: {rex.Status}:{rex.Message}");
        return false;
    }
}

static async Task<bool> CreateRoom(string id, double temperature, double humidity)
{
    BasicDigitalTwin twin = new BasicDigitalTwin();
    twin.Metadata = new DigitalTwinMetadata();
    twin.Metadata.ModelId = "dtmi:com:contoso:Room;2";
    // Initialize properties
    Dictionary<string, object> props = new Dictionary<string, object>();
    props.Add("Temperature", temperature);
    props.Add("Humidity", humidity);
    twin.CustomProperties = props;
    
    try
    {
        client.CreateDigitalTwin(id, JsonSerializer.Serialize<BasicDigitalTwin>(twin)); 
        return true;       
    }
    catch (ErrorResponseException e)
    {
        Console.WriteLine($"**_ Error creating twin {id}: {e.Response.StatusCode}"); 
        return false;
    }
}

static async Task<bool> CreateFloorOrBuilding(string id, bool makeFloor=true)
{
    string type = "dtmi:com:contoso:Building;3";
    if (makeFloor==true)
        type = "dtmi:com:contoso:Floor;2";
    BasicDigitalTwin twin = new BasicDigitalTwin();
    twin.Metadata = new DigitalTwinMetadata();
    twin.Metadata.ModelId = type;
    // Initialize properties
    Dictionary<string, object> props = new Dictionary<string, object>();
    props.Add("AverageTemperature", 0);
    twin.CustomProperties = props;
    
    try
    {
        client.CreateDigitalTwin(id, JsonSerializer.Serialize<BasicDigitalTwin>(twin));  
        return true;      
    }
    catch (ErrorResponseException e)
    {
        Console.WriteLine($"_*_ Error creating twin {id}: {e.Response.StatusCode}"); 
        return false;
    }
}
```

### <a name="create-a-twin-graph-from-a-spreadsheet"></a>Erstellen eines Zwillingsgraphen aus einem Arbeitsblatt

In praktischen Anwendungsfällen werden Zwillingshierarchien oft aus Daten erstellt, die in einer anderen Datenbank oder vielleicht in einem Arbeitsblatt gespeichert sind. In diesem Abschnitt wird veranschaulicht, wie ein Arbeitsblatt analysiert werden kann.

Betrachten Sie die folgende Datentabelle, in der eine Reihe von digitalen Zwillingen und die zu erstellenden Beziehungen beschrieben werden.

| Modell    | id | Parent | Beziehungsname | Sonstige Daten |
| --- | --- | --- | --- | --- |
| floor    | Floor01 | | | … |
| room    | Room10 | Floor01 | contains | … |
| room    | Room11 | Floor01 | contains | … |
| room    | Room12 | Floor01 | contains | … |
| floor    | Floor02 | | | … |
| room    | Room21 | Floor02 | contains | … |
| room    | Room22 | Floor02 | contains | … |

Der folgende Code verwendet die [Microsoft Graph-API](/graph/overview), um ein Arbeitsblatt zu lesen und aus den Ergebnissen einen Azure Digital Twins-Zwillingsgraphen zu konstruieren.

```csharp
var range = msftGraphClient.Me.Drive.Items["BuildingsWorkbook"].Workbook.Worksheets["Building"].usedRange;
JsonDocument data = JsonDocument.Parse(range.values);
List<BasicRelationship> RelationshipRecordList = new List<BasicRelationship>();
foreach (JsonElement row in data.RootElement.EnumerateArray())
{
    string type = row[0].GetString();
    string id = row[1].GetString();
    string relSource = row[2].GetString();
    string relName = row[3].GetString();
    // Parse spreadsheet extra data into a JSON string to initialize the digital twin
    // Left out for compactness
    Dictionary<string, object> initData = new Dictionary<string, object>() { ... };

    if (relSource != null)
    {
        BasicRelationship br = new BasicRelationship()
        {
            SourceId = relSource,
            TargetId = id,
            Name = relName
        };
        RelationshipRecordList.Add(br);
    }

    BasicDigitalTwin twin = new BasicDigitalTwin();
    twin.CustomProperties = initData;
    // Set the type of twin to be created
    switch (type)
    {
        case "room":
            twin.Metadata = new DigitalTwinMetadata() { ModelId = "dtmi:com:contoso:Room;2" };
            break;
        case "floor":
            twin.Metadata = new DigitalTwinMetadata() { ModelId = "dtmi:com:contoso:Floor;2" };
            break;
        ... handle additional types
    }
    try
    {
        client.CreateDigitalTwin(id, JsonSerializer.Serialize<BasicDigitalTwin>(twin));
    }
    catch (RequestFailedException e)
    {
        Log.Error($"Error {e.Status}: {e.Message}");
    }
    foreach (BasicRelationship rec in RelationshipRecordList)
    { 
        try { 
            client.CreateRelationship(rec.SourceId, Guid.NewGuid().ToString(), JsonSerializer.Serialize<BasicRelationship>(rec));
        }
        catch (RequestFailedException e)
        {
            Log.Error($"Error {e.Status}: {e.Message}");
        }
    }
}
```
## <a name="manage-relationships-with-cli"></a>Verwalten von Beziehungen mit der CLI

Zwillinge und ihre Beziehungen können auch über die Azure Digital Twins-Befehlszeilenschnittstelle (CLI) verwaltet werden. Die Befehle finden Sie unter [*Vorgehensweise: Verwenden der Azure Digital Twins-Befehlszeilenschnittstelle*](how-to-use-cli.md).

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Abfrage eines Azure Digital Twins-Zwillingsgraphen:
* [*Konzepte: Abfragesprache*](concepts-query-language.md)
* [*Verwenden Abfragen des Zwillingsgraphen*](how-to-query-graph.md)