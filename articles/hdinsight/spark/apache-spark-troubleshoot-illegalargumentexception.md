---
title: Fehler „IllegalArgumentException“ für Apache Spark – Azure HDInsight
description: IllegalArgumentException für Apache Spark-Aktivität in Azure HDInsight für Azure Data Factory
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: 334e8d129a7b5bb79c9e01d5ba7f347682e5bd79
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2020
ms.locfileid: "92545615"
---
# <a name="scenario-illegalargumentexception-for-apache-spark-activity-in-azure-hdinsight"></a>Szenario: IllegalArgumentException für Apache Spark-Aktivität in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Verwendung von Apache Spark-Komponenten in Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Sie erhalten die folgende Ausnahme, wenn Sie versuchen, eine Spark-Aktivität in einer Azure Data Factory-Pipeline auszuführen:

```error
Exception in thread "main" java.lang.IllegalArgumentException:
Wrong FS: wasbs://additional@xxx.blob.core.windows.net/spark-examples_2.11-2.1.0.jar, expected: wasbs://wasbsrepro-2017-11-07t00-59-42-722z@xxx.blob.core.windows.net
```

## <a name="cause"></a>Ursache

Ein Spark-Auftrag kann nicht ausgeführt werden, wenn sich die JAR-Datei der Anwendung nicht im standardmäßigen oder primären Speicher des Spark-Clusters befindet.

Dies ist ein bekanntes Problem beim Open-Source-Framework von Spark, das in diesem Fehler nachverfolgt wird: [Fehler beim Spark-Auftrag, wenn fs.defaultFS und die JAR-Datei der Anwendung unterschiedliche URLs aufweisen](https://issues.apache.org/jira/browse/SPARK-22587).

Das Problem wurde in Spark 2.3.0 gelöst.

## <a name="resolution"></a>Lösung

Stellen Sie sicher, dass die JAR-Datei der Anwendung im standardmäßigen/primären Speicher für den HDInsight-Cluster gespeichert wird. Stellen Sie bei Azure Data Factory sicher, dass der über ADF verknüpfte Dienst auf den HDInsight-Standardcontainer verweist, nicht auf einen sekundären Container.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support** . Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](../../azure-portal/supportability/how-to-create-azure-support-request.md). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.