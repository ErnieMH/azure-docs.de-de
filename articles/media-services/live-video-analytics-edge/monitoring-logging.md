---
title: 'Überwachung und Protokollierung: Azure'
description: Dieser Artikel bietet eine Übersicht der Überwachung und Protokollierung von Live Video Analytics in IoT Edge.
ms.topic: reference
ms.date: 04/27/2020
ms.openlocfilehash: 8ae455a4157cd649f610620e486323ac2c0a5744
ms.sourcegitcommit: cc13f3fc9b8d309986409276b48ffb77953f4458
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97401048"
---
# <a name="monitoring-and-logging"></a>Überwachung und Protokollierung

In diesem Artikel erfahren Sie, wie Sie Ereignisse vom Modul Live Video Analytics in IoT Edge für die Remoteüberwachung empfangen können. 

Darüber hinaus erfahren Sie, wie Sie die Protokolle steuern können, die vom Modul generiert werden.

## <a name="taxonomy-of-events"></a>Taxonomie von Ereignissen

Live Video Analytics in IoT Edge gibt Ereignisse oder Telemetriedaten entsprechend der folgenden Taxonomie aus.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/telemetry-schema/taxonomy.png" alt-text="Taxonomie von Ereignissen":::

* Betriebsbezogen: Ereignisse, die als Teil der von einem Benutzer ausgeführten Aktionen oder während der Ausführung eines [Mediengraphs](media-graph-concept.md) generiert werden.
   
   * Volumen: Es wird ein geringes Volumen erwartet (mehrmals pro Minute oder sogar seltener).
   * Beispiele:

      Aufzeichnung gestartet (unten), Aufzeichnung beendet
      
      ```
      {
        "body": {
          "outputType": "assetName",
          "outputLocation": "sampleAssetFromEVR-LVAEdge-20200512T233309Z"
        },
        "applicationProperties": {
          "topic": "/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<my-resource-group>/providers/microsoft.media/mediaservices/<ams-account-name>",
          "subject": "/graphInstances/Sample-Graph-2/sinks/assetSink",
          "eventType": "Microsoft.Media.Graph.Operational.RecordingStarted",
          "eventTime": "2020-05-12T23:33:10.392Z",
          "dataVersion": "1.0"
        }
      }
      ```
* Diagnose: Ereignisse, die bei der Diagnose von Problemen und/oder Leistungsproblemen helfen.

   * Volumen: kann hoch sein (mehrmals pro Minute).
   * Beispiele:
   
      [SDP](https://en.wikipedia.org/wiki/Session_Description_Protocol)-Informationen von RTSP (unten) oder Lücken im eingehenden Videofeed.

      ```
      {
        "body": {
          "sdp": "SDP:\nv=0\r\no=- 1589326384077235 1 IN IP4 XXX.XX.XX.XXX\r\ns=Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\ni=media/lots_015.mkv\r\nt=0 0\r\na=tool:LIVE555 Streaming Media v2020.04.12\r\na=type:broadcast\r\na=control:*\r\na=range:npt=0-73.000\r\na=x-qt-text-nam:Matroska video+audio+(optional)subtitles, streamed by the LIVE555 Media Server\r\na=x-qt-text-inf:media/lots_015.mkv\r\nm=video 0 RTP/AVP 96\r\nc=IN IP4 0.0.0.0\r\nb=AS:500\r\na=rtpmap:96 H264/90000\r\na=fmtp:96 packetization-mode=1;profile-level-id=640028;sprop-parameter-sets=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\r\na=control:track1\r\n"
        },
        "applicationProperties": {
          "topic": "/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<my-resource-group>/providers/microsoft.media/mediaservices/<ams-account-name>",
          "subject": "/graphInstances/Sample-Graph-2/sources/rtspSource",
          "eventType": "Microsoft.Media.Graph.Diagnostics.MediaSessionEstablished",
          "eventTime": "2020-05-12T23:33:04.077Z",
          "dataVersion": "1.0"
        }
      }
      ```
* Analyse: Ereignisse, die im Rahmen der Videoanalyse generiert werden.

   * Volumen: kann hoch sein (mehrmals pro Minute oder sogar öfter).
   * Beispiele:
      
      Bewegung erkannt (unten), Rückschlussergebnis.

   ```      
   {
     "body": {
       "timestamp": 143039375044290,
       "inferences": [
         {
           "type": "motion",
           "motion": {
             "box": {
               "l": 0.48954,
               "t": 0.140741,
               "w": 0.075,
               "h": 0.058824
             }
           }
         }
       ]
     },
     "applicationProperties": {
       "topic": "/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<my-resource-group>/providers/microsoft.media/mediaservices/<ams-account-name>",
       "subject": "/graphInstances/Sample-Graph-2/processors/md",
       "eventType": "Microsoft.Media.Graph.Analytics.Inference",
       "eventTime": "2020-05-12T23:33:09.381Z",
       "dataVersion": "1.0"
     }
   }
   ```

Die Ereignisse, die vom Modul ausgegeben werden, werden an den [IoT Edge-Hub](../../iot-edge/iot-edge-runtime.md#iot-edge-hub) gesendet und können von dort aus an andere Ziele weitergeleitet werden. 

### <a name="timestamps-in-analytic-events"></a>Zeitstempel in Analyseereignissen

Wie oben gezeigt, sind Ereignisse, die im Rahmen der Videoanalyse generiert werden, mit einem Zeitstempel versehen. Wenn die [Livevideo-Aufzeichnung](video-recording-concept.md) im Rahmen der Graphtopologie erfolgt, können Sie anhand dieses Zeitstempels ermitteln, wo das jeweilige Ereignis im aufgezeichneten Video zu finden ist. Im Folgenden finden Sie die Richtlinien zum Zuordnen des Zeitstempels in einem Analyseereignis zur Zeitachse des Videos, das in einem [Azure Media Services-Asset](terminology.md#asset) aufgezeichnet wurde.

Extrahieren Sie zuerst den Wert `eventTime`. Verwenden Sie diesen Wert in einem [Zeitbereichsfilter](playback-recordings-how-to.md#time-range-filters), um einen passenden Teil der Aufzeichnung abzurufen. Angenommen, Sie möchten ein Video abrufen, das 30 Sekunden vor der `eventTime` beginnt und 30 Sekunden danach endet. Im obigen Beispiel mit einer `eventTime` von „2020-05-12T23:33:09.381Z“ sieht eine Anforderung für ein HLS-Manifest für das Fenster „+/- 30s“ wie folgt aus:

```
https://{hostname-here}/{locatorGUID}/content.ism/manifest(format=m3u8-aapl,startTime=2020-05-12T23:32:39Z,endTime=2020-05-12T23:33:39Z).m3u8
```

Die obige URL würde eine so genannte [Hauptwiedergabeliste](https://developer.apple.com/documentation/http_live_streaming/example_playlists_for_http_live_streaming) zurückgeben, die URLs für Medienwiedergabelisten enthält. Die Medienwiedergabeliste enthält Einträge, die den folgenden Beispielen vergleichbar sind:

```
...
#EXTINF:3.103011,no-desc
Fragments(video=143039375031270,format=m3u8-aapl)
...
```
Im obigen Beispiel wird über den Eintrag gemeldet, dass ein Videofragment verfügbar ist, das bei einem Zeitstempelwert von `143039375031270` beginnt. Der Wert `timestamp` im Analyseereignis verwendet dieselbe Zeitskala wie die Medienwiedergabeliste und kann zum Identifizieren des entsprechenden Videofragments und für die Suche nach dem richtigen Frame verwendet werden.

Weitere Informationen finden Sie in einem der vielen [Artikel](https://www.bing.com/search?q=frame+accurate+seeking+in+HLS) zur framegenauen Suche in HLS.

## <a name="controlling-events"></a>Steuern von Ereignissen

Sie können die folgenden Eigenschaften von Modulzwillingen verwenden, wie im [JSON-Schema des Modulzwillings](module-twin-configuration-schema.md) dokumentiert, um die betriebsbezogenen und Diagnoseereignisse zu steuern, die von Live Video Analytics im IoT Edge-Modul veröffentlicht werden.

`diagnosticsEventsOutputName`: Einschließen und Bereitstellen (beliebiger) Werte für diese Eigenschaft, um Diagnoseereignisse vom Modul abzurufen. Lassen Sie die Eigenschaft aus oder leer, um das Modul an der Veröffentlichung von Diagnoseereignissen zu hindern.
   
`operationalEventsOutputName`: Einschließen und Bereitstellen (beliebiger) Werte für diese Eigenschaft, um betriebsbezogene Ereignisse vom Modul abzurufen. Lassen Sie die Eigenschaft aus oder leer, um das Modul an der Veröffentlichung von betriebsbezogenen Ereignissen zu hindern.
   
Die Analyseereignisse werden von Knoten wie dem Verarbeitungsknoten für die Bewegungserkennung oder dem Verarbeitungsknoten der HTTP-Erweiterung erstellt, und die IoT-Hub-Senke wird verwendet, um sie an den IoT Edge-Hub zu senden. 

Sie können das [Routing aller oben aufgeführten Ereignisse](../../iot-edge/module-composition.md#declare-routes) über eine gewünschte Eigenschaft des $edgeHub-Modulzwillings (im Bereitstellungsmanifest) steuern:

```
 "$edgeHub": {
   "properties.desired": {
     "schemaVersion": "1.0",
     "routes": {
       "moduleToHub": "FROM /messages/modules/lvaEdge/outputs/* INTO $upstream"
     },
     "storeAndForwardConfiguration": {
       "timeToLiveSecs": 7200
     }
   }
 }
```

Im Beispiel oben ist IvaEdge der Name des Live Video Analytics in IoT Edge-Moduls, und die Routingregel folgt dem in [Deklarieren von Routen](../../iot-edge/module-composition.md#declare-routes) definierten Schema.

> [!NOTE]
> Um sicherzustellen, dass die Analyseereignisse den IoT Edge-Hub erreichen, muss ein IoT-Hub-Senkknoten vorhanden sein, der einem Verarbeitungsknoten für die Bewegungserkennung und/oder einem Verarbeitungsknoten der HTTP-Erweiterung im Datenstrom nachgelagert ist.

## <a name="event-schema"></a>Ereignisschema

Ereignisse haben auf dem Edge-Gerät ihren Ursprung und können auf dem Edge-Gerät oder in der Cloud verbraucht werden. Ereignisse, die von Live Video Analytics in IoT Edge generiert werden, entsprechen dem von Azure IoT Hub eingerichteten [Streaming-Messagingmuster](../../iot-hub/iot-hub-devguide-messages-construct.md) mit Systemeigenschaften, Anwendungseigenschaften und einem Textkörper.

### <a name="summary"></a>Zusammenfassung

Jedes Ereignis weist bei Beobachtung durch den IoT Hub eine Reihe gemeinsamer Eigenschaften auf, wie unten beschrieben.

|Eigenschaft   |Eigenschaftstyp| Datentyp   |BESCHREIBUNG|
|---|---|---|---|
|message-id |System |guid|  Eindeutige Ereignis-ID.|
|topic| applicationProperty |Zeichenfolge|    Azure Resource Manager-Pfad für das Media Services-Konto.|
|subject|   applicationProperty |Zeichenfolge|    Unterpfad zu der Entität, die das Ereignis ausgibt.|
|eventTime| applicationProperty|    Zeichenfolge| Die Uhrzeit, zu der das Ereignis generiert wurde.|
|eventType| applicationProperty |Zeichenfolge|    Ereignistypbezeichner (siehe unten).|
|body|body  |Objekt (object)|    Bestimmte Ereignisdaten.|
|dataVersion    |applicationProperty|   Zeichenfolge  |{Major}.{Minor}|

### <a name="properties"></a>Eigenschaften

#### <a name="message-id"></a>message-id

Global eindeutiger Ereignisbezeichner (GUID, Globally Unique Identifier).

#### <a name="topic"></a>topic

Stellt das dem Graph zugeordnete Azure Media Service-Konto dar.

`/subscriptions/{subId}/resourceGroups/{rgName}/providers/Microsoft.Media/mediaServices/{accountName}`

#### <a name="subject"></a>subject

Entität, die das Ereignis ausgibt:

`/graphInstances/{graphInstanceName}`<br/>
`/graphInstances/{graphInstanceName}/sources/{sourceName}`<br/>
`/graphInstances/{graphInstanceName}/processors/{processorName}`<br/>
`/graphInstances/{graphInstanceName}/sinks/{sinkName}`

Mit der Subject-Eigenschaft können generische Ereignisse ihrem Generierungsmodul zugeordnet werden. Beispielsweise wäre im Fall eines ungültigen RTSP-Benutzernamens oder Kennworts das generierte Ereignis `Microsoft.Media.Graph.Diagnostics.ProtocolError` auf dem `/graphInstances/myGraph/sources/myRtspSource`-Knoten.

#### <a name="event-types"></a>Ereignistypen

Ereignistypen werden einem Namespace gemäß dem folgenden Schema zugewiesen:

`Microsoft.Media.Graph.{EventClass}.{EventType}`

#### <a name="event-classes"></a>Ereignisklassen

|Klassenname|BESCHREIBUNG|
|---|---|
|Analytics  |Als Teil der Inhaltsanalyse erstellte Ereignisse.|
|Diagnose    |Ereignisse, die bei der Diagnose von Problemen und Leistungsproblemen unterstützend wirken.|
|Bei Betrieb    |Im Rahmen des Ressourcenbetriebs generierte Ereignisse.|

Die Ereignistypen sind für jede Ereignisklasse spezifisch.

Beispiele:

* Microsoft.Media.Graph.Analytics.Inference
* Microsoft.Media.Graph.Diagnostics.AuthorizationError
* Microsoft.Media.Graph.Operational.GraphInstanceStarted

### <a name="event-time"></a>Ereigniszeit

Die Ereigniszeit ist in der ISO8601-Zeichenfolge beschrieben. Dies ist die Zeit, zu der das Ereignis aufgetreten ist.

### <a name="azure-monitor-collection-using-telegraf"></a>Azure Monitor-Erfassung mithilfe von Telegraf

Diese Metriken werden vom Modul „Live Video Analytics in IoT Edge“ gemeldet:  

|Metrikname|type|Bezeichnung|BESCHREIBUNG|
|-----------|----|-----|-----------|
|lva_active_graph_instances|Maßstab|iothub, edge_device, module_name, graph_topology|Gesamtanzahl der aktiven Graphen pro Topologie|
|lva_received_bytes_total|Zähler|iothub, edge_device, module_name, graph_topology, graph_instance, graph_node|Gesamtanzahl der von einem Knoten empfangenen Bytes. Wird nur für RTSP-Quellen unterstützt.|
|lva_data_dropped_total|Zähler|iothub, edge_device, module_name, graph_topology, graph_instance, graph_node, data_kind|Zähler für alle gelöschten Daten (Ereignisse, Medien usw.)|

> [!NOTE]
> Ein [Prometheus-Endpunkt](https://prometheus.io/docs/practices/naming/) wird an Port **9600** des Containers verfügbar gemacht. Wenn Sie dem Modul „Live Video Analytics in IoT Edge“ den Namen „lvaEdge“ gegeben haben, kann durch Senden einer GET-Anforderung an http://lvaEdge:9600/metrics auf Metriken zugegriffen werden.   

Führen Sie die folgenden Schritte aus, um die Erfassung von Metriken aus dem Modul „Live Video Analytics in IoT Edge“ zu aktivieren:

1. Erstellen Sie auf dem Entwicklungscomputer einen Ordner, und navigieren Sie zu diesem Ordner.

1. Erstellen Sie in diesem Ordner eine Datei vom Typ `telegraf.toml` mit folgendem Inhalt:
    ```
    [agent]
        interval = "30s"
        omit_hostname = true

    [[inputs.prometheus]]
      metric_version = 2
      urls = ["http://edgeHub:9600/metrics", "http://edgeAgent:9600/metrics", "http://{LVA_EDGE_MODULE_NAME}:9600/metrics"]

    [[outputs.azure_monitor]]
      namespace_prefix = ""
      region = "westus"
      resource_id = "/subscriptions/{SUBSCRIPTON_ID}/resourceGroups/{RESOURCE_GROUP}/providers/Microsoft.Devices/IotHubs/{IOT_HUB_NAME}"
    ```
    > [!IMPORTANT]
    > Ersetzen Sie unbedingt die Variablen (durch `{ }` gekennzeichnet) in der Inhaltsdatei.

1. Erstellen Sie in diesem Ordner eine Datei vom Typ `.dockerfile` mit folgendem Inhalt:
    ```
        FROM telegraf:1.15.3-alpine
        COPY telegraf.toml /etc/telegraf/telegraf.conf
    ```

1. Nun können Sie mithilfe des Docker CLI-Befehls die **Docker-Datei erstellen** und das Image in Azure Container Registry veröffentlichen.
    1. Erfahren Sie, wie Sie [Docker-Images in Azure Container Registry pushen und pullen](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-docker-cli).  Weitere Informationen zu Azure Container Registry (ACR) finden Sie [hier](https://docs.microsoft.com/azure/container-registry/).


1. Fügen Sie nach Abschluss des Pushvorgangs an ACR den folgenden Knoten in der Bereitstellungsmanifestdatei hinzu:
    ```
    "telegraf": 
    {
      "settings": 
        {
            "image": "{ACR_LINK_TO_YOUR_TELEGRAF_IMAGE}"
        },
      "type": "docker",
      "version": "1.0",
      "status": "running",
      "restartPolicy": "always",
      "env": 
        {
            "AZURE_TENANT_ID": { "value": "{YOUR_TENANT_ID}" },
            "AZURE_CLIENT_ID": { "value": "{YOUR CLIENT_ID}" },
            "AZURE_CLIENT_SECRET": { "value": "{YOUR_CLIENT_SECRET}" }
        }
    ``` 
    > [!IMPORTANT]
    > Ersetzen Sie unbedingt die Variablen (durch `{ }` gekennzeichnet) in der Inhaltsdatei.


1. **Authentifizierung**
    1. Azure Monitor kann [vom Dienstprinzipal authentifiziert werden](https://github.com/influxdata/telegraf/blob/master/plugins/outputs/azure_monitor/README.md#azure-authentication).
        1. Das Azure Monitor-Telegraf-Plug-In stellt [verschiedene Methoden der Authentifizierung](https://github.com/influxdata/telegraf/blob/master/plugins/outputs/azure_monitor/README.md#azure-authentication) bereit. Die folgenden Umgebungsvariablen müssen so festgelegt werden, dass sie die Dienstprinzipalauthentifizierung verwenden:  
            •   AZURE_TENANT_ID: Gibt den Mandanten für die Authentifizierung an.  
            •   AZURE_CLIENT_ID: Gibt die zu verwendende App-Client-ID an.  
            •   AZURE_CLIENT_SECRET: Gibt das zu verwendende App-Geheimnis an.  
    >[!TIP]
    > Dem Dienstprinzipal kann die Rolle **Herausgeber von Überwachungsmetriken** zugewiesen werden.

1. Nach der Bereitstellung der Module werden in Azure Monitor Metriken unter einem einzelnen Namespace mit den Metriknamen angezeigt, die den von Prometheus ausgegebenen Metriknamen entsprechen. 
    1. Navigieren Sie in diesem Fall im Azure-Portal zu IoT Hub, und klicken Sie im linken Navigationsbereich auf den Link **Metriken**. Die Metriken sollten dort angezeigt werden.
## <a name="logging"></a>Protokollierung

Wie bei anderen IoT Edge-Modulen, können Sie außerdem [die Containerprotokolle auf dem Edge-Gerät untersuchen](../../iot-edge/troubleshoot.md#check-container-logs-for-issues). Die in den Protokollen erfassten Informationen können durch die Eigenschaften des [folgenden Modulzwillings](module-twin-configuration-schema.md) gesteuert werden:

* logLevel

   * Die zulässigen Werte sind Verbose, Information, Warning, Error, None.
   * Der Standardwert ist Information – die Protokolle enthalten Fehler, Warnungen und Informationen. Nachrichten.
   * Wenn Sie den Wert auf „Warning“ festlegen, enthalten die Protokolle Fehler- und Warnmeldungen
   * Wenn Sie den Wert auf „Error“ festlegen, enthalten die Protokolle nur Fehlermeldungen.
   * Wenn Sie den Wert auf „None“ festlegen, werden keine Protokolle generiert (dies ist nicht empfehlenswert).
   * „Verbose“ sollten Sie nur verwenden, wenn Sie die Protokolle mit dem Azure-Support teilen müssen, um ein Problem zu diagnostizieren.
* logCategories

   * Eine durch Trennzeichen getrennte Liste einer oder mehrerer der folgenden Ressourcen: Anwendung, MediaPipeline, Ereignisse.
   * Standardwert: Anwendung, Ereignisse.
   * Anwendung: Dies sind allgemeine Informationen aus dem Modul, wie z. B. Nachrichten zum Modulstart, Umgebungsfehlern und Aufrufen direkter Methoden.
   * Ereignisse: Dies sind alle Ereignisse, die weiter oben in diesem Artikel beschrieben sind.
   * MediaPipeline: Dies sind einige Detailprotokolle, die möglicherweise Einblick beim Troubleshooting von Problemen geben können, etwa bei Problemen beim Herstellen einer Verbindung mit einer RTSP-fähigen Kamera.
   
### <a name="generating-debug-logs"></a>Generieren von Debugprotokollen

In bestimmten Fällen müssen Sie möglicherweise ausführlichere als die oben beschriebenen Protokolle generieren, um den Azure-Support beim Beheben eines Problems zu unterstützen. Dies erreichen Sie in zwei Schritten.

Zunächst [verknüpfen Sie den Modulspeicher mit dem Gerätespeicher](../../iot-edge/how-to-access-host-storage-from-module.md#link-module-storage-to-device-storage), mithilfe von „createOptions“. Wenn Sie eine [Vorlage für ein Bereitstellungsmanifest](https://github.com/Azure-Samples/live-video-analytics-iot-edge-csharp/blob/master/src/edge/deployment.template.json) aus den Schnellstartanleitungen untersuchen, sehen Sie Folgendes:

```
"createOptions": {
   …
   "Binds": [
     "/var/local/mediaservices/:/var/lib/azuremediaservices/"
   ]
 }
```

Mit der Anweisung oben schreibt das Edge-Modul Protokolle unter dem (Geräte-) Speicherpfad „/var/local/mediaservices/“. Wenn Sie dem Modul die folgende gewünschte Eigenschaft hinzufügen:

`"debugLogsDirectory": "/var/lib/azuremediaservices/debuglogs/",`

schreibt das Modul Debugprotokolle in einem binären Format unter dem (Geräte-) Speicherpfad „/var/local/mediaservices/debuglogs/“, die Sie mit dem Azure-Support teilen können.

## <a name="faq"></a>Häufig gestellte Fragen

[Häufig gestellte Fragen](faq.md#monitoring-and-metrics)

## <a name="next-steps"></a>Nächste Schritte

[Fortlaufende Videoaufzeichnung](continuous-video-recording-tutorial.md)
