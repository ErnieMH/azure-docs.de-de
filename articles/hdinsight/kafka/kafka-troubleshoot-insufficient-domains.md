---
title: Fehler aufgrund unzureichender Fehlerdomänen in einer Region in Azure HDInsight
description: Fehler bei der Clustererstellung, da in einer Region in Azure HDInsight nicht genügend Fehlerdomänen vorhanden sind
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 08/09/2019
ms.openlocfilehash: ed9e98750a469f78855096c3149254cf92c2788f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "102501344"
---
# <a name="scenario-cluster-creation-failed-due-to-not-sufficient-fault-domains-in-region-in-azure-hdinsight"></a>Szenario: Fehler `not sufficient fault domains in region` bei der Clustererstellung in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Beim Versuch, einen Apache Kafka-Cluster zu erstellen, erhalten Sie ungefähr folgende Fehlermeldung: `not sufficient fault domains in region`.

## <a name="cause"></a>Ursache

Eine Fehlerdomäne ist eine logische Gruppierung von zugrundeliegender Hardware in einem Azure-Rechenzentrum. Jede Fehlerdomäne verwendet eine Stromquelle und einen Netzwerkswitch gemeinsam. Die virtuellen Computer und verwalteten Datenträger, die die Knoten innerhalb eines HDInsight-Clusters implementieren, werden auf diese Fehlerdomänen verteilt. Diese Architektur schränkt die potenziellen Auswirkungen physischer Hardwarefehler ein.

Jede Azure-Region weist eine bestimmte Anzahl von Fehlerdomänen auf. Eine Liste der Domänen und die Anzahl der Fehlerdomänen, die sie enthalten, finden Sie in der Dokumentation zu [Verfügbarkeitsgruppen](../../virtual-machines/availability.md).

In HDInsight müssen Kafka-Cluster in einer Region mit mindestens drei Fehlerdomänen bereitgestellt werden.

## <a name="resolution"></a>Lösung

Enthält die Region, in der Sie den Cluster erstellen möchten, nicht genügend Fehlerdomänen, wenden Sie sich an das Produktteam, um die Bereitstellung des Clusters zu ermöglichen, auch wenn keine drei Fehlerdomänen vorhanden sind.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](../../azure-portal/supportability/how-to-create-azure-support-request.md). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.