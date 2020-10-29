---
title: 'Nicht in der Apache Hive-Ansicht angezeigte Fehlermeldung: Azure HDInsight'
description: Bei der Abfrage tritt in der Apache Hive-Ansicht ein Fehler auf. Es werden keine Informationen zum Azure HDInsight-Cluster angezeigt.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: 7945410c803964e664af20fb9bc91ab82e8ddc6e
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2020
ms.locfileid: "92533035"
---
# <a name="scenario-query-error-message-not-displayed-in-apache-hive-view-in-azure-hdinsight"></a>Szenario: Abfragefehlermeldung wird in Apache Hive-Ansicht in Azure HDInsight nicht angezeigt

Dieser Artikel beschreibt Schritte zur Problembehandlung sowie mögliche Lösungen für Probleme bei der Verwendung von Interactive Query-Komponenten in Azure HDInsight-Clustern.

## <a name="issue"></a>Problem

Die Abfragefehlermeldung in der Apache Hive-Ansicht sieht in etwa wie folgt aus und enthält keine weiteren Informationen:

```
"Failed to execute query. <a href="#/messages/1">(details)</a>"
```

## <a name="cause"></a>Ursache

Manchmal ist die Fehlermeldung eines Abfragefehlers ggf. zu groß, um auf der Hauptseite der Hive-Ansicht angezeigt zu werden.

## <a name="resolution"></a>Lösung

Überprüfen Sie die Registerkarte „Benachrichtigungen“ (rechts oben in der Hive-Ansicht), um sich die vollständige Stapelüberwachung und die gesamte Fehlermeldung anzusehen.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support** . Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](../../azure-portal/supportability/how-to-create-azure-support-request.md). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.