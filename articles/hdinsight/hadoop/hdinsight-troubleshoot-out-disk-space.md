---
title: Clusterknoten geht der Speicherplatz auf dem Datenträger in Azure HDInsight aus
description: Problembehandlung bei Problemen mit dem Speicherplatz auf dem Apache Hadoop-Clusterknoten in Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 04/30/2020
ms.openlocfilehash: 042a894d9f6f2a8fb7737d0a20abeb2293f756aa
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2020
ms.locfileid: "92540600"
---
# <a name="scenario-cluster-node-runs-out-of-disk-space-in-azure-hdinsight"></a>Szenario: Clusterknoten geht der Speicherplatz auf dem Datenträger in Azure HDInsight aus

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Ein Auftrag schlägt möglicherweise mit einer Fehlermeldung ähnlich der folgenden fehl: `/usr/hdp/2.6.3.2-14/hadoop/libexec/hadoop-config.sh: fork: No space left on device.`

Oder Sie erhalten eine ähnliche Apache Ambari-Warnung wie diese: `local-dirs usable space is below configured utilization percentage`.

## <a name="cause"></a>Ursache

Der Apache Yarn-Anwendungscache hat möglicherweise den gesamten verfügbaren Speicherplatz verbraucht. Ihre Spark-Anwendung wird wahrscheinlich ineffizient ausgeführt.

## <a name="resolution"></a>Lösung

1. Verwenden Sie die Ambari-Benutzeroberfläche, um zu ermitteln, auf welchem Knoten der Speicherplatz knapp wird.

1. Bestimmen Sie, welcher Ordner im problematischen Knoten zum größten Teil des Speicherplatzes auf dem Datenträger beiträgt. Verwenden Sie zunächst SSH für den Knoten, und führen Sie dann `df` aus, um die Datenträgerverwendung für alle Bereitstellungen aufzulisten. Normalerweise handelt es sich um `/mnt`, einen temporären Datenträger, der von OSS verwendet wird. Sie können in einen Ordner navigieren und dann `sudo du -hs` eingeben, um zusammengefasste Dateigrößen unter einem Ordner anzuzeigen. Wenn Sie einen ähnlichen Ordner wie `/mnt/resource/hadoop/yarn/local/usercache/livy/appcache/application_1537280705629_0007` sehen, bedeutet dies, dass die Anwendung weiterhin ausgeführt wird. Mögliche Ursachen hierfür sind RDD-Persistenz oder Shuffle-Zwischendateien.

1. Um das Problem zu beheben, beenden Sie die Anwendung, wodurch der von dieser Anwendung verwendete Speicherplatz freigegeben wird.

1. Wenn das Problem auf den Workerknoten häufig auftritt, können Sie die Einstellungen für den lokalen YARN-Cache auf dem Cluster optimieren.

    Öffnen Sie die Ambari-Benutzeroberfläche, navigieren Sie zu „YARN“--> „Configs“--> „Advanced“.  
    Fügen Sie dem benutzerdefinierten Abschnitt „yarn-site.xml“ die zwei folgenden Eigenschaften hinzu, und speichern Sie dann Folgendes:

    ```
    yarn.nodemanager.localizer.cache.target-size-mb=2048
    yarn.nodemanager.localizer.cache.cleanup.interval-ms=300000
    ```

1. Wenn das Problem durch die vorstehenden Schritte nicht dauerhaft behoben wurde, optimieren Sie Ihre Anwendung.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support** . Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](../../azure-portal/supportability/how-to-create-azure-support-request.md). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.