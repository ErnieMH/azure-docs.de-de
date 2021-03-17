---
title: 'Tutorial: Stream Analytics im Edgebereich mit Azure IoT Edge'
description: In diesem Tutorial stellen Sie Azure Stream Analytics als Modul auf einem IoT Edge-Gerät bereit.
author: kgremban
ms.author: kgremban
ms.date: 07/29/2020
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 323973b7646acee07a0c4dbc59834e0aceca75ee
ms.sourcegitcommit: afb9e9d0b0c7e37166b9d1de6b71cd0e2fb9abf5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/14/2021
ms.locfileid: "103462047"
---
# <a name="tutorial-deploy-azure-stream-analytics-as-an-iot-edge-module"></a>Tutorial: Bereitstellen von Azure Stream Analytics als IoT Edge-Modul

[!INCLUDE [iot-edge-version-all-supported](../../includes/iot-edge-version-all-supported.md)]

Viele IoT-Lösungen verwenden Analysedienste, um Erkenntnisse zu Daten zu gewinnen, die von IoT-Geräten an die Cloud übermittelt werden. Mit Azure IoT Edge können Sie die Logik von [Azure Stream Analytics](../stream-analytics/index.yml) direkt auf dem Gerät bereitstellen. Die Verarbeitung von Telemetriedatenströmen im Edgebereich verringert die Menge an Uploaddaten sowie die Zeit, die benötigt wird, um auf verwertbare Erkenntnisse zu reagieren.

Azure IoT Edge und Azure Stream Analytics sind integriert, um Ihre Workloadentwicklung zu vereinfachen. Sie können im Azure-Portal einen Azure Stream Analytics-Auftrag erstellen und ihn anschließend ohne zusätzlichen Programmieraufwand als IoT Edge-Modul bereitstellen.  

Azure Stream Analytics bietet eine umfassend strukturierte Abfragesyntax für die Datenanalyse in der Cloud und auf IoT Edge-Geräten. Weitere Informationen finden Sie in der [Azure Stream Analytics-Dokumentation](../stream-analytics/stream-analytics-edge.md).

Das Stream Analytics-Modul in diesem Tutorial berechnet die Durchschnittstemperatur der letzten 30 Sekunden. Bei Erreichen einer Durchschnittstemperatur von 70 Grad sendet das Modul eine Warnung, damit das Gerät eine entsprechende Aktion ausführen kann. In diesem Fall wird der simulierte Temperatursensor zurückgesetzt. In einer Produktionsumgebung kann diese Funktion dazu verwendet werden, einen Computer herunterzufahren oder vorbeugende Maßnahmen zu ergreifen, wenn die Temperatur eine gefährliche Höhe erreicht.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
>
> * Erstellen eines Azure Stream Analytics-Auftrags zum Verarbeiten von Daten auf dem Edge-Gerät.
> * Verbinden des neuen Azure Stream Analytics-Auftrags mit anderen IoT Edge-Modulen.
> * Bereitstellen des Azure Stream Analytics-Auftrags auf einem IoT Edge-Gerät über das Azure-Portal.

<center>

![Architekturdiagramm des Tutorials: Staging und Bereitstellung des ASA-Auftrags](./media/tutorial-deploy-stream-analytics/asa-architecture.png)
</center>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Ein Azure IoT Edge-Gerät:

* Sie können einen virtuellen Azure-Computer als IoT Edge-Gerät verwenden, indem Sie die Schritte der Schnellstartanleitung für [Linux-](quickstart-linux.md) oder [Windows-Geräte](quickstart.md) ausführen.

Cloudressourcen:

* Eine [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-Instanz in Azure im Tarif „Free“ oder „Standard“.

## <a name="create-an-azure-stream-analytics-job"></a>Erstellen eines Azure Stream Analytics-Auftrags

In diesem Abschnitt erstellen Sie einen Azure Stream Analytics-Auftrag, der folgende Schritte ausführt:

* Empfangen von Daten von Ihrem IoT Edge-Gerät
* Abfragen der Telemetriedaten nach Werten außerhalb eines festgelegten Bereichs
* Ergreifen von Maßnahmen für das IoT Edge-Gerät auf der Grundlage der Abfrageergebnisse

### <a name="create-a-storage-account"></a>Speicherkonto erstellen

Wenn Sie einen Azure Stream Analytics-Auftrag zur Ausführung auf einem IoT Edge-Gerät erstellen, muss er so gespeichert werden, dass er vom Gerät aufgerufen werden kann. Sie können ein bereits vorhandenes Azure Storage-Konto verwenden oder jetzt ein neues erstellen.

1. Navigieren Sie im Azure-Portal zu **Ressource erstellen** > **Storage** > **Speicherkonto**.

1. Geben Sie die folgenden Werte an, um Ihr Speicherkonto zu erstellen:

   | Feld | Wert |
   | ----- | ----- |
   | Subscription | Wählen Sie das gleiche Abonnement wie für Ihren IoT Hub. |
   | Resource group | Es empfiehlt sich, im Rahmen der IoT Edge-Schnellstartanleitungen und -Tutorials die gleiche Ressourcengruppe für alle Ihre Testressourcen zu verwenden. Beispielsweise **IoTEdgeResources**. |
   | Name | Geben Sie einen eindeutigen Namen für Ihr Speicherkonto an. |
   | Standort | Wählen Sie einen Standort in Ihrer Nähe aus. |

1. Behalten Sie in den restlichen Feldern die Standardwerte bei, und wählen Sie **Überprüfen und erstellen** aus.

1. Überprüfen Sie Ihre Einstellungen, und wählen Sie anschließend **Erstellen** aus.

### <a name="create-a-new-job"></a>Erstellen eines neuen Auftrags

1. Navigieren Sie im Azure-Portal zu **Ressource erstellen** > **Internet der Dinge** > **Stream Analytics-Auftrag**.

1. Geben Sie die folgenden Werte an, um Ihren Auftrag zu erstellen:

   | Feld | Wert |
   | ----- | ----- |
   | Auftragsname | Geben Sie einen Namen für Ihren Auftrag an. Beispielsweise **IoTEdgeJob** |
   | Subscription | Wählen Sie das gleiche Abonnement wie für Ihren IoT Hub. |
   | Resource group | Es wird empfohlen, die gleiche Ressourcengruppe für alle Testressourcen zu verwenden, die Sie während der IoT Edge-Schnellstarts und -Tutorials erstellen. Beispielsweise **IoTEdgeResources**. |
   | Standort | Wählen Sie einen Standort in Ihrer Nähe aus. |
   | Hosting-Umgebung | Wählen Sie **Edge** aus. |

1. Klicken Sie auf **Erstellen**.

### <a name="configure-your-job"></a>Konfigurieren des Auftrags

Nachdem Ihr Stream Analytics-Auftrag im Azure-Portal erstellt wurde, können Sie ihn mit einer Eingabe, einer Ausgabe und einer Abfrage konfigurieren, die für die Daten ausgeführt wird, die er durchläuft.

Mithilfe der drei Elemente – Eingabe, Ausgabe und Abfrage – wird in diesem Abschnitt ein Auftrag erstellt, der Temperaturdaten vom IoT Edge-Gerät empfängt. Die Daten werden in einem rollierenden Fenster von 30 Sekunden analysiert. Wenn die Durchschnittstemperatur in diesem Fenster auf mehr als 70 Grad ansteigt, wird eine Warnung an das IoT Edge-Gerät gesendet. Im nächsten Abschnitt legen Sie beim Bereitstellen des Auftrags genau fest, woher die Daten stammen und wohin sie gesendet werden.  

1. Navigieren Sie im Azure-Portal zu Ihrem Stream Analytics-Auftrag.

1. Wählen Sie unter **Auftragstopologie** die Option **Eingaben** und dann **Datenstromeingabe hinzufügen**.

   ![Azure Stream Analytics: Hinzufügen einer Eingabe](./media/tutorial-deploy-stream-analytics/asa-input.png)

1. Wählen Sie in der Dropdownliste die Option **Edge-Hub** aus.

1. Geben Sie im Bereich **Neue Eingabe** die **Temperatur** als Eingabealias ein.

1. Behalten Sie in den restlichen Feldern die Standardwerte bei, und wählen Sie **Speichern**.

1. Öffnen Sie unter **Auftragstopologie** die Option **Ausgaben**, und wählen Sie dann **Hinzufügen**.

   ![Azure Stream Analytics: Hinzufügen einer Ausgabe](./media/tutorial-deploy-stream-analytics/asa-output.png)

1. Wählen Sie in der Dropdownliste die Option **Edge-Hub** aus.

1. Geben Sie im Bereich **Neue Ausgabe** die **Warnung** als Ausgabealias ein.

1. Behalten Sie in den restlichen Feldern die Standardwerte bei, und wählen Sie **Speichern**.

1. Wählen Sie unter **Auftragstopologie** die Option **Abfrage** aus.

1. Ersetzen Sie den Standardtext mit der folgenden Abfrage. Der SQL-Code sendet einen Zurücksetzungsbefehl an die Warnungsausgabe, wenn die durchschnittliche Maschinentemperatur in einem 30-sekündigen Fenster 70 Grad erreicht. Der Zurücksetzungsbefehl wurde im Sensor als ausführbare Aktion vorprogrammiert.

    ```sql
    SELECT  
        'reset' AS command
    INTO
       alert
    FROM
       temperature TIMESTAMP BY timeCreated
    GROUP BY TumblingWindow(second,30)
    HAVING Avg(machine.temperature) > 70
    ```

1. Wählen Sie **Abfrage speichern** aus.

### <a name="configure-iot-edge-settings"></a>Konfigurieren von IoT Edge-Einstellungen

Um Ihren Stream Analytics-Auftrag auf die Bereitstellung als IoT Edge-Gerät vorzubereiten, müssen Sie den Auftrag einem Container in einem Speicherkonto zuordnen. Wenn Sie Ihren Auftrag bereitstellen, wird die Auftragsdefinition in den Speichercontainer extrahiert.

1. Wählen Sie unter **Konfigurieren** die Option **Speicherkontoeinstellungen** und anschließend **Speicherkonto hinzufügen** aus.

   ![Azure Stream Analytics: Hinzufügen eines Speicherkontos](./media/tutorial-deploy-stream-analytics/add-storage-account.png)

1. Wählen Sie über das Dropdownmenü das **Speicherkonto** aus, das Sie zu Beginn dieses Tutorials erstellt haben.

1. Klicken Sie für das Feld **Container** auf **Neu erstellen**, und geben Sie einen Namen für den Speichercontainer an.

1. Wählen Sie **Speichern** aus.

## <a name="deploy-the-job"></a>Bereitstellen des Auftrags

Sie können den Azure Stream Analytics-Auftrag jetzt auf Ihrem IoT Edge-Gerät bereitstellen.

In diesem Abschnitt verwenden Sie den Assistenten zum **Festlegen von Modulen** im Azure-Portal zum Erstellen eines *Bereitstellungsmanifests*. Ein Bereitstellungsmanifest ist eine JSON-Datei mit Beschreibungen für: alle Module, die auf einem Gerät bereitgestellt werden, die Containerregistrierungen, welche die Modulimages speichern, die Verwaltung der Module und die Kommunikationsmöglichkeiten für die Module untereinander. Ihr IoT Edge-Gerät ruft das Bereitstellungsmanifest aus IoT Hub ab und verwendet die Informationen darin zur Bereitstellung und Konfiguration aller seiner zugewiesenen Module.

In diesem Tutorial stellen Sie zwei Module bereit. Das erste ist das Modul **SimulatedTemperatureSensor**, das einen Sensor für Temperatur und Luftfeuchtigkeit simuliert. Das zweite ist Ihr Stream Analytics-Auftrag. Das Sensormodul stellt den Datenstrom bereit, der von Ihrer Auftragsabfrage analysiert wird.

1. Navigieren Sie im Azure-Portal zu Ihrem IoT Hub.

1. Navigieren Sie zu **IoT Edge**, und öffnen Sie die Detailseite für Ihr IoT Edge-Gerät.

1. Wählen Sie **Module festlegen** aus.  

1. Wenn Sie zuvor das SimulatedTemperatureSensor-Modul auf diesem Gerät bereitgestellt haben, wird es unter Umständen automatisch aufgefüllt. Führen Sie andernfalls die folgenden Schritte aus, um das Modul hinzuzufügen:

   1. Klicken Sie auf **Hinzufügen** und anschließend auf **IoT Edge-Modul**.
   1. Geben Sie **SimulatedTemperatureSensor** als Name ein.
   1. Geben Sie als Bild-URI **mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0** ein.
   1. Behalten Sie die anderen Einstellungen unverändert bei, und wählen Sie **Hinzufügen** aus.

1. Fügen Sie Ihren Azure Stream Analytics Edge-Auftrag wie folgt hinzu:

   1. Klicken Sie auf **Hinzufügen** und anschließend auf **Azure Stream Analytics-Modul**.
   1. Wählen Sie das Abonnement und den erstellten Azure Stream Analytics Edge-Auftrag aus.
   1. Wählen Sie **Speichern** aus.

   Nach dem Speichern Ihrer Änderungen werden die Details Ihres Stream Analytics-Auftrags in dem von Ihnen erstellten Speichercontainer veröffentlicht.

1. Wenn das Stream Analytics-Modul zur Liste der Module hinzugefügt wird, wählen Sie zum Anzeigen der Struktur seinen Namen aus, und aktualisieren Sie die Einstellungen auf der Seite **IoT Edge-Modul aktualisieren**.

   Auf der Registerkarte **Moduleinstellungen** ist der **Image-URI** angegeben, der auf ein Azure Stream Analytics-Standardimage verweist. Dieses einzelne Image wird für jedes Stream Analytics-Modul verwendet, das für ein IoT Edge-Gerät bereitgestellt wird.

   Auf der Registerkarte **Einstellungen für Modulzwilling** ist der JSON-Code angegeben, der die ASA-Eigenschaft (Azure Stream Analytics) **ASAJobInfo** definiert. Der Wert dieser Eigenschaft verweist auf die Auftragsdefinition in Ihrem Speichercontainer. Mithilfe dieser Eigenschaft wird das Stream Analytics-Image mit Ihren individuellen Auftragsdetails konfiguriert.

   Das Stream Analytics-Modul hat standardmäßig den gleichen Namen wie der Auftrag, auf dem es basiert. Der Modulname kann bei Bedarf auf dieser Seite geändert werden, dies ist jedoch nicht erforderlich.

1. Wählen Sie **Aktualisieren** oder **Abbrechen** aus.

1. Notieren Sie sich den Namen Ihres Stream Analytics-Moduls, da Sie ihn im nächsten Schritt benötigen. Wählen Sie anschließend **Weiter: Routen** aus, um den Vorgang fortzusetzen.

1. Auf der Registerkarte **Routen** definieren Sie, wie Nachrichten zwischen Modulen und dem IoT Hub übergeben werden. Nachrichten werden mit Name-Wert-Paaren erstellt. Ersetzen Sie die Standardnamen und -werte für `route` und `upstream` durch die Name-Wert-Paare in der folgenden Tabelle. Ersetzen Sie dabei Instanzen von _{moduleName}_ durch den Namen Ihres Azure Stream Analytics-Moduls.

    | Name | Wert |
    | --- | --- |
    | `telemetryToCloud` | `FROM /messages/modules/SimulatedTemperatureSensor/* INTO $upstream` |
    | `alertsToCloud` | `FROM /messages/modules/{moduleName}/* INTO $upstream` |
    | `alertsToReset` | `FROM /messages/modules/{moduleName}/* INTO BrokeredEndpoint("/modules/SimulatedTemperatureSensor/inputs/control")` |
    | `telemetryToAsa` | `FROM /messages/modules/SimulatedTemperatureSensor/* INTO BrokeredEndpoint("/modules/{moduleName}/inputs/temperature")`|

    Die Routen, die Sie hier deklarieren, definieren den Fluss der Daten durch das IoT Edge-Gerät. Die Telemetriedaten von „SimulatedTemperatureSensor“ werden an IoT Hub und an die Eingabe **temperature** (Temperatur) gesendet, die im Stream Analytics-Auftrag konfiguriert wurde. Die Meldungen der Ausgabe **alert** (Warnung) werden an IoT Hub und an das SimulatedTemperatureSensor-Modul gesendet, um den Zurücksetzungsbefehl auszulösen.

1. Klicken Sie auf **Weiter: Überprüfen + erstellen**.

1. Auf der Registerkarte **Überprüfen + erstellen** sehen Sie, wie die im Assistenten angegebenen Informationen in ein JSON-Bereitstellungsmanifest konvertiert werden. Wählen Sie nach Abschluss der Manifestüberprüfung die Option **Erstellen** aus.

1. Sie werden zurück auf die Seite mit den Gerätedetails geleitet. Klicken Sie auf **Aktualisieren**.  

    Das neue Stream Analytics-Modul sollte nun ausgeführt werden – zusammen mit den Modulen für IoT Edge-Agent und IoT Edge-Hub. Unter Umständen dauert es ein paar Minuten, bis die Informationen Ihr IoT Edge-Gerät erreichen und die Module gestartet werden. Sollten die Module nicht gleich als ausgeführt angezeigt werden, aktualisieren in regelmäßigen Abständen die Seite.

    ![Gerätemeldung für SimulatedTemperatureSensor- und ASA-Modul](./media/tutorial-deploy-stream-analytics/module-output2.png)

## <a name="view-data"></a>Anzeigen von Daten

Wechseln Sie nun zu Ihrem IoT Edge-Gerät, um sich die Interaktion zwischen dem Azure Stream Analytics-Modul und dem SimulatedTemperatureSensor-Modul anzusehen.

1. Überprüfen Sie, ob alle Module in Docker ausgeführt werden:

   ```cmd/sh
   iotedge list  
   ```

1. Zeigen Sie alle Systemprotokolle und Metrikdaten an. Verwenden Sie den Namen des Stream Analytics-Moduls:

   ```cmd/sh
   iotedge logs -f {moduleName}  
   ```

1. Sehen Sie sich anhand der Sensorprotokolle an, wie sich der Zurücksetzungsbefehl auf das SimulatedTemperatureSensor-Modul auswirkt:

   ```cmd/sh
   iotedge logs SimulatedTemperatureSensor
   ```

   Sie können beobachten, wie die Temperatur des Computers allmählich steigt, bis sie 30 Sekunden lang 70 Grad beträgt. Dann löst das Stream Analytics-Modul eine Zurücksetzung aus, und die Computertemperatur fällt zurück auf 21.

   ![Zurücksetzen der Befehlsausgabe in den Modulprotokollen](./media/tutorial-deploy-stream-analytics/docker_log.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie mit dem nächsten empfohlenen Artikel fortfahren möchten, können Sie die erstellten Ressourcen und Konfigurationen beibehalten und wiederverwenden. Sie können auch dasselbe IoT Edge-Gerät als Testgerät weiter nutzen.

Andernfalls können Sie die in diesem Artikel verwendeten lokalen Konfigurationen und die Azure-Ressourcen löschen, um Kosten zu vermeiden.

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Azure Streaming Analytics-Auftrag zum Analysieren von Daten konfiguriert, die von ihrem IoT Edge-Gerät stammen. Anschließend haben Sie das Azure Stream Analytics-Modul auf Ihrem IoT Edge-Gerät geladen, um die Daten lokal zu verarbeiten, lokal auf einen Temperaturanstieg zu reagieren und den aggregierten Datenstrom an die Cloud zu senden. Sie können nun die anderen Tutorials bearbeiten, um noch mehr darüber zu erfahren, wie sich mit Azure IoT Edge weitere Lösungen für Ihr Unternehmen erstellen lassen.

> [!div class="nextstepaction"]
> [Bereitstellen eines Azure Machine Learning-Modells als Modul](tutorial-deploy-machine-learning.md)