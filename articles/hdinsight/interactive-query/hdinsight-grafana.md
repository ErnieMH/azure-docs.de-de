---
title: Verwenden von Grafana in Azure HDInsight
description: Erfahren Sie, wie Sie mit Apache Hadoop-Clustern in Azure HDInsight auf das Grafana-Dashboard zugreifen.
ms.service: hdinsight
ms.topic: how-to
ms.date: 12/27/2019
ms.openlocfilehash: 81fd3b368f9405192c164ed7a0638caad0cd75fc
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104869751"
---
# <a name="access-grafana-in-azure-hdinsight"></a>Zugreifen auf Grafana in Azure HDInsight

[Grafana](https://grafana.com/) ist ein verbreiteter Open-Source-Generator für Diagramme und Dashboards. Mit dem umfangreichen Funktionssatz von Grafana können Benutzer nicht nur anpassbare und gemeinsam nutzbare Dashboards erstellen, sondern u.a. auch von Dashboards mit Vorlagen bzw. Skripts, LDAP-Integration und verschiedenen Datenquellen profitieren.

Derzeit wird Grafana in Azure HDInsight mit den Clustertypen Spark, HBase, Kafka und Interactive Query unterstützt. Cluster mit aktiviertem Enterprise-Sicherheitspaket werden nicht unterstützt.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-an-apache-hadoop-cluster"></a>Erstellen eines Apache Hadoop-Clusters

Siehe [Erstellen von Apache Hadoop-Clustern im Azure-Portal](../hdinsight-hadoop-create-linux-clusters-portal.md). Wählen Sie für **Clustertyp** die Option **Spark**, **Kafka**, **HBase** oder **Interactive Query** aus.

## <a name="access-the-grafana-dashboard"></a>Zugreifen auf das Grafana-Dashboard

1. Navigieren Sie in einem Webbrowser zu `https://CLUSTERNAME.azurehdinsight.net/grafana/`, wobei CLUSTERNAME der Name Ihres Clusters ist.

1. Geben Sie die Benutzeranmeldeinformationen für den Hadoop-Cluster ein.

1. Das Grafana-Dashboard wird angezeigt und sieht wie in diesem Beispiel aus:

    :::image type="content" source="./media/hdinsight-grafana/hdinsight-grafana-dashboard.png " alt-text="HDInsight Grafana-Webdashboard" border="true":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese Anwendung nicht mehr benötigen, gehen Sie wie folgt vor, um den erstellten Cluster zu löschen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Geben Sie oben im **Suchfeld** den Suchbegriff **HDInsight** ein.

1. Wählen Sie unter **Dienste** die Option **HDInsight-Cluster** aus.

1. Klicken Sie in der daraufhin angezeigten Liste mit den HDInsight-Clustern neben dem von Ihnen erstellten Cluster auf die Auslassungspunkte (**...**).

1. Klicken Sie auf **Löschen**. Wählen Sie **Ja** aus.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Datenanalyse mit HDInsight finden Sie in den folgenden Artikeln:

* [Verwenden von Apache Hive mit HDInsight](../hadoop/hdinsight-use-hive.md)

* [Verwenden von MapReduce mit HDInsight](../hadoop/hdinsight-use-mapreduce.md)

* [Erste Schritte bei der Verwendung von Hadoop-Tools für Visual Studio für HDInsight](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)
