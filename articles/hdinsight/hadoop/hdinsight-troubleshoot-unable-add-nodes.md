---
title: Dem Azure HDInsight-Cluster können keine Knoten hinzugefügt werden.
description: Problembehandlung, wenn das Hinzufügen von Knoten zum Apache Hadoop-Cluster in Azure HDInsight nicht möglich ist
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.openlocfilehash: b8e8bf0a8a0555b23799600c3958fbbfd5a510dd
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2020
ms.locfileid: "92540566"
---
# <a name="scenario-unable-to-add-nodes-to-azure-hdinsight-cluster"></a>Szenario: Dem Azure HDInsight-Cluster können keine Knoten hinzugefügt werden.

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Dem Azure HDInsight-Cluster können keine Knoten hinzugefügt werden.

## <a name="cause"></a>Ursache

Die Gründe können variieren.

## <a name="resolution"></a>Lösung

Berechnen Sie mithilfe des Features [Clustergröße](../hdinsight-scaling-best-practices.md) die Anzahl von für den Cluster erforderlichen zusätzlichen Kernen. Diese Anzahl basiert auf der Gesamtanzahl von Kernen in den neuen Workerknoten. Probieren Sie dann die folgenden Schritte aus:

* Überprüfen Sie, ob am Speicherort des Clusters Kerne verfügbar sind.

* Sehen Sie sich die Anzahl verfügbarer Kernen an anderen Speicherorten an. Ziehen Sie die Neuerstellung des Clusters an einem anderen Speicherort mit ausreichend verfügbaren Kernen in Betracht.

* Wenn Sie das Kernkontingent für einen bestimmten Speicherort erhöhen möchten, legen Sie ein Supportticket für eine Erhöhung des HDInsight-Kernkontingents an.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support** . Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](../../azure-portal/supportability/how-to-create-azure-support-request.md). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.