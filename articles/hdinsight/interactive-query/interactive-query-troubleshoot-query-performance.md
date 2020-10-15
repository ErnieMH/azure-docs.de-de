---
title: Schlechte Leistung bei Apache Hive LLAP-Abfragen in Azure HDInsight
description: Abfragen in Apache Hive LLAP werden in Azure HDInsight langsamer als erwartet ausgeführt.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: 8bd20849b15f8c8d5a14653f702f78c6404d82e5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "75895123"
---
# <a name="scenario-poor-performance-in-apache-hive-llap-queries-in-azure-hdinsight"></a>Szenario: Schlechte Leistung bei Apache Hive LLAP-Abfragen in Azure HDInsight

Dieser Artikel beschreibt Schritte zur Problembehandlung sowie mögliche Lösungen für Probleme bei der Verwendung von Interactive Query-Komponenten in Azure HDInsight-Clustern.

## <a name="issue"></a>Problem

Die Standardclusterkonfigurationen sind für Ihre Workload nicht ausreichend optimiert. Abfragen in Hive LLAP werden langsamer als erwartet ausgeführt.

## <a name="cause"></a>Ursache

Dies kann aufgrund einer Vielzahl von Gründen geschehen.

## <a name="resolution"></a>Lösung

LLAP ist für Abfragen optimiert, die Joins und Aggregate einschließen. Abfragen wie die folgende funktionieren in einem interaktiven Hive-Cluster nicht besonders gut:

```
select * from table where column = "columnvalue"
```

Legen Sie die folgenden Konfigurationen fest, um die Leistung von Punktabfragen in Hive LLAP zu verbessern:

```
hive.llap.io.enabled=false; (disable LLAP IO)
hive.optimize.index.filter=false; (disable ORC row index)
hive.exec.orc.split.strategy=BI; (to avoid recombining splits)
```

Sie können mit der folgenden Konfigurationsänderung auch die Nutzung des LLAP-Caches erhöhen, um die Leistung zu verbessern:

```
hive.fetch.task.conversion=none
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.
