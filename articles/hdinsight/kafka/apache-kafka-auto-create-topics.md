---
title: Aktivieren der automatischen Themenerstellung in Apache Kafka – Azure HDInsight
description: Erfahren Sie, wie Sie Apache Kafka in HDInsight für die automatische Erstellung von Themen konfigurieren. Sie können Kafka konfigurieren, indem Sie `auto.create.topics.enable` über Ambari auf TRUE festlegen. Dies können Sie auch während der Clustererstellung über PowerShell- oder Resource Manager-Vorlagen festlegen.
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive,seoapr2020
ms.date: 04/28/2020
ms.openlocfilehash: b416623d6637cfe8e2c1cd795dd62553f8c0aed4
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98933208"
---
# <a name="how-to-configure-apache-kafka-on-hdinsight-to-automatically-create-topics"></a>Konfigurieren von Apache Kafka in HDInsight für die automatische Erstellung von Themen

Standardmäßig aktiviert Apache Kafka in HDInsight die automatische Themenerstellung nicht. Sie können die automatische Themenerstellung für vorhandene Cluster mithilfe von Apache Ambari aktivieren. Sie können die automatische Themenerstellung auch aktivieren, wenn Sie mithilfe einer Azure Resource Manager-Vorlage einen neuen Kafka-Cluster erstellen.

## <a name="apache-ambari-web-ui"></a>Apache Ambari-Webbenutzeroberfläche

Um die automatische Themenerstellung in einem vorhandenen Cluster über die Ambari-Web-UI zu aktivieren, führen Sie folgende Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) Ihren Kafka-Cluster aus.

1. Wählen Sie aus **Clusterdashboards** die Option **Ambari-Homepage** aus.

    ![Abbildung: Portal mit ausgewähltem Clusterdashboard](./media/apache-kafka-auto-create-topics/azure-portal-cluster-dashboard-ambari.png)

    Bei Aufforderung authentifizieren Sie sich mit den Anmeldeinformationen (Admin) für den Cluster. Stattdessen können Sie sich auch direkt von `https://CLUSTERNAME.azurehdinsight.net/` aus mit Amabri verbinden, wobei `CLUSTERNAME` der Name Ihres Kafka-Clusters ist.

1. Wählen Sie auf der linken Seite aus der Liste den Kafka-Dienst aus.

    ![Apache Ambari – Registerkarte mit Dienstliste](./media/apache-kafka-auto-create-topics/hdinsight-service-list.png)

1. Klicken Sie in der Mitte der Seite auf „Konfigurationen“.

    ![Apache Ambari – Registerkarte mit Dienstkonfigurationen](./media/apache-kafka-auto-create-topics/hdinsight-service-config.png)

1. Geben Sie in das Feld „Filter“ den Wert `auto.create` ein.

    ![Apache Ambari – Suchfilterfeld](./media/apache-kafka-auto-create-topics/hdinsight-filter-field.png)

    Diese Einstellung filtert die Liste der Eigenschaften und zeigt die Einstellung `auto.create.topics.enable` an.

1. Ändern Sie den Wert von `auto.create.topics.enable` zu `true`, und klicken Sie dann auf **Speichern**. Fügen Sie einen Hinweis hinzu, und wählen Sie dann erneut **Speichern**.

    ![Abbildung: Eintrag „auto.create.topics.enable“](./media/apache-kafka-auto-create-topics/auto-create-topics-enable.png)

1. Wählen Sie nacheinander den Kafka-Dienst, die Option __Neu starten__ und die Option __Neustart aller betroffenen__ aus. Klicken Sie bei entsprechender Aufforderung auf __Neustart aller Dienste bestätigen__.

    ![Neustarten aller betroffenen in Apache Ambari](./media/apache-kafka-auto-create-topics/restart-all-affected.png)

> [!NOTE]  
> Sie können Ambari-Werte auch über die Ambari-REST-API festlegen. Dies ist im Allgemeinen schwieriger, da Sie mehrere REST-Aufrufe ausführen müssen, um die aktuelle Konfiguration abzurufen, sie bearbeiten usw. Weitere Informationen finden Sie im Dokument [Verwalten von HDInsight-Clustern mithilfe der Apache Ambari-REST-API](../hdinsight-hadoop-manage-ambari-rest-api.md).

## <a name="resource-manager-templates"></a>Resource Manager-Vorlagen

Wenn Sie einen Kafka-Cluster mithilfe einer Azure Resource Manager-Vorlage erstellen, können Sie `auto.create.topics.enable` durch Hinzufügen in einem `kafka-broker` direkt festlegen. Der folgende JSON-Codeausschnitt zeigt, wie dieser Wert auf `true` festgelegt wird:

```json
"clusterDefinition": {
    "kind": "kafka",
    "configurations": {
    "gateway": {
        "restAuthCredential.isEnabled": true,
        "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
        "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
    },
    "kafka-broker": {
        "auto.create.topics.enable": "true"
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie die automatische Themenerstellung für Apache Kafka in HDInsight aktivieren. Weitere Informationen zur Arbeit mit Kafka finden Sie unter den folgenden Links:

* [Analysieren von Apache Kafka-Protokollen](apache-kafka-log-analytics-operations-management.md)
* [Replizieren von Daten zwischen Apache Kafka-Clustern](apache-kafka-mirroring.md)
