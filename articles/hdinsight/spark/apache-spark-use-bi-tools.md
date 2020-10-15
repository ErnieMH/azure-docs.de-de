---
title: 'Tutorial: Analysieren von Apache Spark-Daten mit Power BI in Azure HDInsight'
description: 'Tutorial: Verwenden von Microsoft Power BI zum Visualisieren von Apache Spark-Daten, die in HDInsight-Clustern gespeichert sind'
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.custom: hdinsightactive,mvc,seoapr2020
ms.date: 04/21/2020
ms.openlocfilehash: 4eb10298ac683c991835f86582d82fb952b314b2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "82195110"
---
# <a name="tutorial-analyze-apache-spark-data-using-power-bi-in-hdinsight"></a>Tutorial: Analysieren von Apache Spark-Daten mithilfe von Power BI in HDInsight

In diesem Tutorial erfahren Sie, wie Sie mit Microsoft Power BI Daten in einem Apache Spark-Cluster in Azure HDInsight visualisieren.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Visualisieren von Spark-Daten mithilfe von Power BI

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

* Lesen Sie den Artikel [Tutorial: Laden von Daten und Ausführen von Abfragen auf einem Apache Spark-Cluster in Azure HDInsight](./apache-spark-load-data-run-query.md).

* [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).

* Optional: [Power BI-Testabonnement](https://app.powerbi.com/signupredirect?pbi_source=web).

## <a name="verify-the-data"></a>Überprüfen der Daten

Das im [vorherigen Tutorial](apache-spark-load-data-run-query.md) erstellte [Jupyter Notebook](https://jupyter.org/) enthält Code zum Erstellen einer `hvac`-Tabelle. Diese Tabelle basiert auf der CSV-Datei, die in allen HDInsight Spark-Clustern unter `\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv` verfügbar ist. Gehen Sie folgendermaßen vor, um die Daten zu überprüfen.

1. Fügen Sie im Jupyter Notebook den folgenden Code ein, und drücken Sie **UMSCHALT+EINGABETASTE**. Der Code überprüft das Vorhandensein der Tabellen.

    ```PySpark
    %%sql
    SHOW TABLES
    ```

    Die Ausgabe sieht wie folgt aus:

    ![Anzeigen von Tabellen in Spark](./media/apache-spark-use-bi-tools/apache-spark-show-tables.png)

    Wenn Sie das Notebook vor dem Starten dieses Tutorials geschlossen haben, wird `hvactemptable` bereinigt und ist daher nicht in der Ausgabe enthalten.  Nur Hive-Tabellen, die im Metastore gespeichert werden (angegeben durch **False** in der Spalte **isTemporary**), sind für die BI-Tools zugänglich. In diesem Tutorial stellen Sie eine Verbindung mit der erstellten Tabelle **hvac** her.

2. Fügen Sie den folgenden Code in eine leere Zelle ein, und drücken Sie **UMSCHALT+EINGABETASTE**. Der Code überprüft die Daten in der Tabelle.

    ```PySpark
    %%sql
    SELECT * FROM hvac LIMIT 10
    ```

    Die Ausgabe sieht wie folgt aus:

    ![Anzeigen von Zeilen aus der hvac-Tabelle in Spark](./media/apache-spark-use-bi-tools/apache-spark-select-limit.png)

3. Wählen Sie im Menü **Datei** des Notebooks die Option **Schließen und Anhalten** aus. Fahren Sie das Notebook herunter, um die Ressourcen freizugeben.

## <a name="visualize-the-data"></a>Visualisieren der Daten

In diesem Abschnitt verwenden Sie Power BI, um Visualisierungen, Berichte und Dashboards aus den Daten im Spark-Cluster zu erstellen.

### <a name="create-a-report-in-power-bi-desktop"></a>Erstellen eines Berichts in Power BI Desktop

Die ersten Schritte bei der Verwendung von Spark bestehen darin, eine Verbindung mit dem Cluster in Power BI Desktop herzustellen, Daten aus dem Cluster zu laden und eine grundlegende Visualisierung auf der Grundlage dieser Daten zu erstellen.

1. Öffnen Sie Power BI Desktop. Schließen Sie den Begrüßungsbildschirm, wenn er geöffnet ist.

2. Navigieren Sie auf der Registerkarte **Home** zu **Daten abrufen** > **Mehr..** .

    ![Abrufen von Daten aus HDInsight Apache Spark in Power BI Desktop](./media/apache-spark-use-bi-tools/hdinsight-spark-power-bi-desktop-get-data.png "Abrufen von Daten aus Apache Spark BI in Power BI")

3. Geben Sie `Spark` in das Suchfeld ein, wählen Sie **Azure HDInsight Spark** aus, und wählen Sie dann **Verbinden** aus.

    ![Abrufen von Daten aus Apache Spark BI in Power BI](./media/apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "Abrufen von Daten aus Apache Spark BI in Power BI")

4. Geben Sie Ihre Cluster-URL (in der Form `mysparkcluster.azurehdinsight.net`) in das Textfeld **Server** ein.

5. Wählen Sie unter **Datenverbindungsmodus:** die Option **DirectQuery** aus. Klicken Sie anschließend auf **OK**.

    Sie können mit Spark beide Datenkonnektivitätsmodi verwenden. Wenn Sie „DirectQuery“ verwenden, werden Änderungen in Berichten ohne Aktualisierung des gesamten Datasets wiedergegeben. Wenn Sie Daten importieren, müssen Sie das Dataset aktualisieren, um die Änderungen zu sehen. Weitere Informationen dazu, wie und wann Sie „DirectQuery“ verwenden, finden Sie unter [Verwenden von DirectQuery mit Power BI](https://powerbi.microsoft.com/documentation/powerbi-desktop-directquery-about/).

6. Geben Sie die Kontoinformationen für die HDInsight-Anmeldung ein, und wählen Sie dann **Verbinden** aus. Der Standardkontoname lautet *admin*.

7. Wählen Sie die Tabelle `hvac` aus, warten Sie, um eine Vorschau der Daten anzuzeigen, und wählen Sie dann **Laden** aus.

    ![Benutzername und Kennwort des Spark-Clusters](./media/apache-spark-use-bi-tools/apache-spark-bi-select-table.png "Benutzername und Kennwort des Spark-Clusters")

    Power BI Desktop verfügt nun über alle Informationen, die zum Herstellen einer Verbindung mit dem Spark-Cluster und zum Laden von Daten aus der Tabelle `hvac` erforderlich sind. Die Tabelle und ihre Spalten werden im Bereich **Felder** angezeigt.

8. Visualisieren Sie die Abweichung zwischen Zieltemperatur und Ist-Temperatur für jedes Gebäude:

    1. Wählen Sie im Bereich **VISUALISIERUNGEN** die Option **Flächendiagramm** aus.

    2. Ziehen Sie das Feld **BuildingID** unter **Achse** und die Felder **ActualTemp** und **TargetTemp** unter **Wert**.

        ![Hinzufügen von Spaltenwerten](./media/apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "Hinzufügen von Spaltenwerten")

        Das Diagramm sieht wie folgt aus:

        ![Summe des Bereichsdiagramms](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph-sum.png "Summe des Bereichsdiagramms")

        Standardmäßig werden in der Visualisierung die Summen für **ActualTemp** und **TargetTemp** angezeigt. Wählen Sie den Pfeil nach unten neben **ActualTemp** und **TargetTemp** im Bereich „Visualisierungen“ aus. Sie sehen, dass **Summe** ausgewählt ist.

    3. Wählen Sie den Pfeil nach unten neben **ActualTemp** und **TargetTemp** im Bereich „Visualisierungen“ aus, wählen Sie **Durchschnitt** aus, um den Durchschnittswert zwischen tatsächlicher und Zieltemperatur für jedes Gebäude zu erhalten.

        ![Durchschnitt der Werte](./media/apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "Durchschnitt der Werte")

        Ihre Datenvisualisierung sollte ähnlich dem Screenshot aussehen. Bewegen Sie den Cursor über die Visualisierung, um QuickInfos mit relevanten Daten abzurufen.

        ![Bereichsdiagramm](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "Bereichsdiagramm")

9. Navigieren Sie zu **Datei** > **Speichern**, geben Sie den Namen `BuildingTemperature` für die Datei ein, und wählen Sie dann **Speichern** aus.

### <a name="publish-the-report-to-the-power-bi-service-optional"></a>Veröffentlichen des Berichts im Power BI-Dienst (optional)

Mit dem Power BI-Dienst können Sie Berichte und Dashboards in Ihrer Organisation freigeben. In diesem Abschnitt veröffentlichen Sie zunächst das DataSet und den Bericht. Dann heften Sie den Bericht an ein Dashboard an. Dashboards werden üblicherweise dazu verwendet, sich auf eine Teilmenge der Daten in einem Bericht zu konzentrieren. Ihr Bericht enthält zwar nur eine Visualisierung, es ist aber dennoch hilfreich, die Schritte durchzugehen.

1. Öffnen Sie Power BI Desktop.

1. Klicken Sie auf der Registerkarte **Start** auf **Veröffentlichen**.

    ![Veröffentlichen aus Power BI Desktop](./media/apache-spark-use-bi-tools/apache-spark-bi-publish.png "Veröffentlichen aus Power BI Desktop")

1. Wählen Sie einen Arbeitsbereich für die Veröffentlichung des Datasets und des Berichts aus, und wählen Sie dann **Auswählen**. In der folgenden Abbildung wird die Standardoption **My Workspace** (Mein Arbeitsbereich) ausgewählt.

    ![Auswählen eines Arbeitsbereichs zum Veröffentlichen eines Datasets und eines Berichts](./media/apache-spark-use-bi-tools/apache-spark-bi-select-workspace.png "Auswählen eines Arbeitsbereichs zum Veröffentlichen eines Datasets und eines Berichts")

1. Nachdem die Veröffentlichung abgeschlossen ist, wählen Sie **Open „BuildingTemperature.pbix“ in Power BI** („BuildingTemperature.pbix“ in Power BI öffnen).

    ![Erfolgreiche Veröffentlichung, Klicken zum Eingeben der Anmeldeinformationen](./media/apache-spark-use-bi-tools/apache-spark-bi-publish-success.png "Erfolgreiche Veröffentlichung, Klicken zum Eingeben der Anmeldeinformationen")

1. Wählen Sie im Power BI-Dienst **Anmeldeinformationen eingeben**.

    ![Eingeben von Anmeldeinformationen im Power BI-Dienst](./media/apache-spark-use-bi-tools/apache-spark-bi-enter-credentials.png "Eingeben von Anmeldeinformationen im Power BI-Dienst")

1. Wählen Sie **Anmeldeinformationen bearbeiten**.

    ![Bearbeiten von Anmeldeinformationen im Power BI-Dienst](./media/apache-spark-use-bi-tools/apache-spark-bi-edit-credentials.png "Bearbeiten von Anmeldeinformationen im Power BI-Dienst")

1. Geben Sie die Kontoinformationen für die HDInsight-Anmeldung ein, und wählen Sie **Anmelden**. Der Standardkontoname lautet *admin*.

    ![Anmelden beim Spark-Cluster](./media/apache-spark-use-bi-tools/apache-spark-bi-sign-in.png "Anmelden beim Spark-Cluster")

1. Navigieren Sie im linken Bereich zu **Arbeitsbereiche** > **Mein Arbeitsbereich** > **BERICHTE**, und wählen Sie **BuildingTemperature**.

    ![Bericht aufgeführt in den Berichten im linken Bereich](./media/apache-spark-use-bi-tools/apache-spark-bi-service-left-pane.png "Bericht aufgeführt in den Berichten im linken Bereich")

    Im linken Bereich sollte unter **DATASETS** außerdem **BuildingTemperature** aufgeführt sein.

    Das in Power BI Desktop erstellte visuelle Element ist nun im Power BI-Dienst verfügbar.

1. Zeigen Sie mit der Maus auf die Visualisierung, und wählen Sie dann das Symbol zum Anheften in der rechten oberen Ecke.

    ![Bericht im Power BI-Dienst](./media/apache-spark-use-bi-tools/apache-spark-bi-service-report.png "Bericht im Power BI-Dienst")

1. Wählen Sie „Neues Dashboard“, geben Sie den Namen `Building temperature` ein, und wählen Sie dann **Anheften**.

    ![Anheften an neues Dashboard](./media/apache-spark-use-bi-tools/apache-spark-bi-pin-dashboard.png "Anheften an neues Dashboard")

1. Wählen Sie im Bericht **Zu Dashboard wechseln**.

Ihr visuelles Element wird an das Dashboard angeheftet. Sie können weitere visuelle Elemente zum Bericht hinzufügen und sie ans gleiche Dashboard anheften. Weitere Informationen zu Berichten und Dashboards finden Sie unter [Berichte in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-reports/) und [Einführung in Dashboards für Power BI-Designer](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Nach Abschluss des Tutorials kann es ratsam sein, den Cluster zu löschen. Mit HDInsight werden Ihre Daten in Azure Storage gespeichert, sodass Sie einen Cluster problemlos löschen können, wenn er nicht verwendet wird. Für einen HDInsight-Cluster fallen auch dann Gebühren an, wenn er nicht verwendet wird. Da die Gebühren für den Cluster erheblich höher sind als die Kosten für den Speicher, ist es sinnvoll, nicht verwendete Cluster zu löschen.

Informationen zum Löschen eines Clusters finden Sie unter [Löschen eines HDInsight-Clusters mit Ihrem Browser, PowerShell oder der Azure CLI](../hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie mit Microsoft Power BI Daten in einem Apache Spark-Cluster in Azure HDInsight visualisieren. Fahren Sie mit dem nächsten Artikel fort, um zu erfahren, wie Sie eine Anwendung für maschinelles Lernen erstellen.

> [!div class="nextstepaction"]
> [Erstellen einer Machine Learning-Anwendung](./apache-spark-ipython-notebook-machine-learning.md)