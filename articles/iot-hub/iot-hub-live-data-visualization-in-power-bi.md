---
title: Datenvisualisierung in Echtzeit von Daten aus Azure IoT Hub – Power BI
description: Verwenden Sie Power BI, um Temperatur- und Luftfeuchtigkeitsdaten visuell darzustellen, die der Sensor gesammelt und an Ihren Azure IoT Hub gesendet hat.
author: robinsh
keywords: Datenvisualisierung in Echtzeit, Visualisierung von Livedaten, Visualisierung von Sensordaten
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 6/08/2020
ms.author: robinsh
ms.openlocfilehash: 82caf13618fe8483ab8d3a622c6c0d51ab05a206
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102177333"
---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a>Verwenden von Power BI zum Visualisieren von Sensordaten in Azure IoT Hub in Echtzeit

![Lückenloses Diagramm](./media/iot-hub-live-data-visualization-in-power-bi/end-to-end-diagram.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Lerninhalt

Sie erfahren, wie Sie mit Power BI von Azure IoT Hub empfangene Sensordaten in Echtzeit visualisieren. Wenn Sie versuchen möchten, die Daten in Ihrem IoT Hub mit einer Web-App visuell darzustellen, helfen Ihnen die Informationen unter [Visualisieren von Sensordaten in Azure IoT Hub in Echtzeit mit einer Web-App](iot-hub-live-data-visualization-in-web-apps.md) weiter.

## <a name="what-you-do"></a>Aufgaben

* Bereiten Sie Ihren IoT Hub für den Datenzugriff vor, indem Sie eine Consumergruppe hinzufügen.

* Erstellen und konfigurieren Sie einen Stream Analytics-Auftrag zum Übertragen von Daten von Ihrem IoT Hub zu Ihrem Power BI-Konto, und führen Sie ihn aus.

* Erstellen und veröffentlichen Sie einen Power BI-Bericht zum Visualisieren der Daten.

## <a name="what-you-need"></a>Voraussetzungen

* Sie müssen das Tutorial [Verbinden des Raspberry Pi-Onlinesimulators mit Azure IoT Hub (Node.js)](iot-hub-raspberry-pi-web-simulator-get-started.md) oder eines der gerätespezifischen Tutorials wie [Verbinden von Raspberry Pi mit Azure IoT Hub (Node.js)](iot-hub-raspberry-pi-kit-node-get-started.md) abgeschlossen haben. In diesen Artikeln werden folgende Anforderungen beschrieben:
  
  * Ein aktives Azure-Abonnement.
  * Ein Azure IoT Hub in Ihrem Abonnement.
  * Eine Clientanwendung, die Nachrichten an Ihren Azure IoT Hub sendet.

* Ein Power BI-Konto. ([Power BI kostenlos testen](https://powerbi.microsoft.com/))

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Erstellen, Konfigurieren und Ausführen eines Stream Analytics-Auftrags

Als Erstes erstellen wir einen Stream Analytics-Auftrag. Nachdem Sie den Auftrag erstellt haben, definieren Sie die Eingaben, Ausgaben und die Abfrage zum Abrufen der Daten.

### <a name="create-a-stream-analytics-job"></a>Erstellen eines Stream Analytics-Auftrags

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Ressource erstellen** > **Internet der Dinge** > **Stream Analytics-Auftrag**.

2. Geben Sie die folgenden Informationen für den Auftrag ein.

   **Auftragsname**: Der Name des Auftrags. Der Name muss global eindeutig sein.

   **Ressourcengruppe**: Verwenden Sie dieselbe Ressourcengruppe wie für Ihren IoT Hub.

   **Standort**: Verwenden Sie denselben Speicherort wie für Ihre Ressourcengruppe.

   ![Erstellen eines Stream Analytics-Auftrags in Azure](./media/iot-hub-live-data-visualization-in-power-bi/create-stream-analytics-job.png)

3. Klicken Sie auf **Erstellen**.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Hinzufügen einer Eingabe zum Stream Analytics-Auftrag

1. Öffnen Sie den Stream Analytics-Auftrag.

2. Wählen Sie unter **Auftragstopologie** die Option **Eingaben** aus.

3. Wählen Sie im Bereich **Eingaben** die Option **Datenstromeingabe hinzufügen** und dann **IoT Hub** aus der Dropdownliste aus. Geben Sie im neuen Eingabebereich die folgenden Informationen ein:

   **Eingabealias**: Geben Sie einen eindeutigen Alias für die Eingabe ein.

   **IoT Hub aus Ihrem Abonnement auswählen**: Wählen Sie dieses Optionsfeld aus.

   **Abonnement**: Wählen Sie das Azure-Abonnement aus, das Sie für dieses Tutorial verwenden.

   **IoT Hub**: Wählen Sie den IoT Hub aus, den Sie für dieses Tutorial verwenden.

   **Endpunkt**: Wählen Sie **Messaging** aus.

   **Name der SAS-Richtlinie**: Wählen Sie den Namen der SAS-Richtlinie aus, die vom Stream Analytics-Auftrag für Ihren IoT Hub verwendet werden soll. Für dieses Tutorial können Sie *service* auswählen. Die Richtlinie *service* wird standardmäßig auf neuen IoT Hubs erstellt und erteilt die Berechtigung zum Senden und Empfangen an cloudseitigen Endpunkten, die vom IoT Hub verfügbar gemacht werden. Weitere Informationen finden Sie unter [Access Control und Berechtigungen](iot-hub-devguide-security.md#access-control-and-permissions).

   **Schlüssel für SAS-Richtlinie**: Dieses Feld wird auf Grundlage des von Ihnen ausgewählten Namens der SAS-Richtlinie automatisch ausgefüllt.

   **Consumergruppe**: Wählen Sie die Consumergruppe aus, die Sie zuvor erstellt haben.

   Behalten Sie bei allen anderen Feldern die Standardwerte bei.

   ![Hinzufügen einer Eingabe zum Stream Analytics-Auftrag in Azure](./media/iot-hub-live-data-visualization-in-power-bi/add-input-to-stream-analytics-job.png)

4. Wählen Sie **Speichern** aus.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Hinzufügen einer Ausgabe zum Stream Analytics-Auftrag

1. Wählen Sie unter **Auftragstopologie** die Option **Ausgaben** aus.

2. Wählen Sie im Bereich **Ausgaben** die Option **Hinzufügen** und dann **Power BI** aus.

3. Wählen Sie im Bereich **Power BI – Neue Ausgabe** die Option **Autorisieren** aus, und folgen Sie den Anweisungen zum Anmelden bei Ihrem Power BI-Konto.

4. Nachdem Sie sich bei Power BI angemeldet haben, geben Sie die folgenden Informationen ein:

   **Ausgabealias**: Ein eindeutiger Alias für die Ausgabe.

   **Gruppenarbeitsbereich**: Wählen Sie den Arbeitsbereich der Zielgruppe aus.

   **Datasetname**: Geben Sie einen Datasetnamen ein.

   **Tabellenname**: Geben Sie einen Tabellennamen ein.

   **Authentifizierungsmodus**: Übernehmen Sie den Standardwert.

   ![Hinzufügen einer Ausgabe zum Stream Analytics-Auftrag in Azure](./media/iot-hub-live-data-visualization-in-power-bi/add-output-to-stream-analytics-job.png)

5. Wählen Sie **Speichern** aus.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>Konfigurieren der Abfrage des Stream Analytics-Auftrags

1. Wählen Sie unter **Auftragstopologie** die Option **Abfrage** aus.

2. Ersetzen Sie `[YourInputAlias]` durch den Eingabealias des Auftrags.

3. Ersetzen Sie `[YourOutputAlias]` durch den Ausgabealias des Auftrags.

   ![Hinzufügen einer Abfrage zum Stream Analytics-Auftrag in Azure](./media/iot-hub-live-data-visualization-in-power-bi/add-query-to-stream-analytics-job.png)

4. Wählen Sie **Abfrage speichern** aus.

### <a name="run-the-stream-analytics-job"></a>Ausführen des Stream Analytics-Auftrags

Wählen Sie im Stream Analytics-Auftrag die Option **Übersicht** und dann **Starten** > **Jetzt** > **Starten** aus. Sobald der Auftrag erfolgreich gestartet wurde, ändert sich der Status des Auftrags von **Beendet** in **Wird ausgeführt**.

![Ausführen eines Stream Analytics-Auftrags in Azure](./media/iot-hub-live-data-visualization-in-power-bi/run-stream-analytics-job.png)

## <a name="create-and-publish-a-power-bi-report-to-visualize-the-data"></a>Erstellen und Veröffentlichen eines Power BI-Berichts zum Visualisieren der Daten

In den folgenden Schritten erfahren Sie, wie Sie einen Bericht mithilfe des Power BI-Diensts erstellen und veröffentlichen. Wenn Sie das „neue Design“ in Power BI verwenden möchten, können Sie diese Schritte mit einigen Änderungen ausführen. Informationen zu den Unterschieden und zum Navigieren im „neuen Design“ finden Sie unter [The „new look“ of the Power BI service](/power-bi/consumer/service-new-look) (Das neue Design des Power BI-Diensts).

1. Stellen Sie sicher, dass die Beispielanwendung auf Ihrem Gerät ausgeführt wird. Ist dies nicht der Fall, finden Sie in den Tutorials unter [Einrichten Ihres Geräts](./iot-hub-raspberry-pi-kit-node-get-started.md) weitere Informationen.

2. Melden Sie sich bei Ihrem [Power BI](https://powerbi.microsoft.com/en-us/)-Konto an.

3. Wählen Sie den von Ihnen verwendeten Arbeitsbereich, **Mein Arbeitsbereich**, aus.

4. Wählen Sie die Option **Datasets**.

   Das Dataset, das Sie beim Erstellen der Ausgabe für den Stream Analytics-Auftrag angegeben haben, sollte angezeigt werden.

5. Wählen Sie für das Dataset, das Sie erstellt haben, **Bericht hinzufügen** aus (das erste Symbol rechts neben dem Namen des Datasets).

   ![Erstellen eines Microsoft Power BI-Berichts](./media/iot-hub-live-data-visualization-in-power-bi/power-bi-create-report.png)

6. Erstellen Sie ein Liniendiagramm, um die Temperatur in Echtzeit im Zeitverlauf anzuzeigen.

   1. Klicken Sie auf der Seite zur Berichterstellung im Bereich **Visualisierungen** auf das Symbol für Liniendiagramme, um ein Liniendiagramm hinzuzufügen.

   2. Erweitern Sie im Bereich **Felder** die Tabelle, die Sie beim Erstellen der Ausgabe für den Stream Analytics-Auftrag angegeben haben.

   3. Ziehen Sie **EventEnqueuedUtcTime** im Bereich **Visualisierungen** auf **Achse**.

   4. Ziehen Sie **Temperatur** auf **Werte**.

      Ein Liniendiagramm wird erstellt. Die x-Achse zeigt Datum und Uhrzeit in der Zeitzone UTC. Die y-Achse zeigt vom Sensor empfangene Temperatur an.

      ![Hinzufügen eines Liniendiagramms für Temperatur zu einem Microsoft Power BI-Bericht](./media/iot-hub-live-data-visualization-in-power-bi/power-bi-add-temperature.png)

7. Erstellen Sie ein weiteres Liniendiagramm, um die Luftfeuchtigkeit in Echtzeit im Zeitverlauf anzuzeigen. Klicken Sie dazu auf einen leeren Bereich der Zeichenfläche, und führen Sie die oben beschriebenen Schritte aus, um **EventEnqueuedUtcTime** auf der x-Achse und **humidity** auf der y-Achse zu platzieren.

   ![Hinzufügen eines Liniendiagramms für Luftfeuchtigkeit zu einem Microsoft Power BI-Bericht](./media/iot-hub-live-data-visualization-in-power-bi/power-bi-add-humidity.png)

8. Wählen Sie **Speichern**, um den Bericht zu speichern.

9. Klicken Sie im linken Bereich auf **Berichte**, und wählen Sie dann den soeben erstellten Bericht aus.

10. Klicken Sie auf **Datei** > **Im Web veröffentlichen**.

    ![Wählen Sie für den Microsoft Power BI-Bericht „Im Web veröffentlichen“aus.](./media/iot-hub-live-data-visualization-in-power-bi/power-bi-select-publish-to-web.png)

    > [!NOTE]
    > Wenn eine Benachrichtigung angezeigt wird, dass Sie sich an Ihren Administrator wenden müssen, um die Erstellung von Einbindungscode zu aktivieren, dann müssen Sie das möglicherweise wirklich tun. Die Erstellung von Einbindungscode muss aktiviert werden, damit Sie diesen Schritt ausführen können.
    >
    > ![Benachrichtigung „Wenden Sie sich an Ihren Administrator“](./media/iot-hub-live-data-visualization-in-power-bi/contact-admin.png)

11. Wählen Sie **Einbindungscode erstellen** und dann **Veröffentlichen** aus.

Sie erhalten einen Berichtslink, den Sie für den Zugriff auf den Bericht freigeben können, und einen Codeausschnitt, mit dem Sie den Bericht in Ihren Blog oder Ihre Website integrieren können.

![Veröffentlichen eines Microsoft Power BI-Berichts](./media/iot-hub-live-data-visualization-in-power-bi/power-bi-web-output.png)

Microsoft bietet auch die [mobilen Power BI-Apps](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) für die Anzeige von und Interaktion mit Ihren Power BI-Dashboards und -Berichten auf Ihrem mobilen Gerät.

## <a name="next-steps"></a>Nächste Schritte

Sie haben Power BI erfolgreich eingesetzt, um Sensordaten in Ihrem Azure IoT Hub in Echtzeit zu visualisieren.

Eine andere Möglichkeit zur Visualisierung von Daten aus Azure IoT Hub finden Sie unter [Visualisieren von Sensordaten aus Azure IoT Hub in Echtzeit mit einer Web-App](iot-hub-live-data-visualization-in-web-apps.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
