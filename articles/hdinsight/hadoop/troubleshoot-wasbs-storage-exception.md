---
title: Das Konto, auf das zugegriffen wird, unterstützt keine HTTP-Fehler in Azure HDInsight.
description: In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 02/06/2020
ms.openlocfilehash: 46063d5f2d9ff4b85914ad7c4cd74a2400298db0
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98943073"
---
# <a name="the-account-being-accessed-does-not-support-http-error-in-azure-hdinsight"></a>Das Konto, auf das zugegriffen wird, unterstützt keine HTTP-Fehler in Azure HDInsight.

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Die folgende Fehlermeldung wird empfangen:

```
com.microsoft.azure.storage.StorageException: The account being accessed does not support http.
```

## <a name="cause"></a>Ursache

Es gibt mehrere Gründe für diese Fehlermeldung:

* Für das Speicherkonto ist die [sichere Übertragung](../../storage/common/storage-require-secure-transfer.md) aktiviert, und es wird das falsche [URI-Schema](../hdinsight-hadoop-linux-information.md#URI-and-scheme) verwendet.

* Ein Cluster wurde mit einem Speicherkonto erstellt, für das die sichere Übertragung *deaktiviert* war. Anschließend wurde die sichere Übertragung für das Speicherkonto aktiviert.

## <a name="resolution"></a>Auflösung

Wenn die sichere Übertragung für Azure Storage oder Data Lake Storage Gen2 aktiviert ist, lautet der URI `wasbs://` bzw. `abfss://`.  Siehe auch [Vorschreiben einer sicheren Übertragung in Azure Storage](../../storage/common/storage-require-secure-transfer.md).

Verwenden Sie für neue Cluster ein Speicherkonto, das bereits die gewünschte Einstellung für die sichere Übertragung aufweist. Bei einem Speicherkonto, das von einem vorhandenen Cluster verwendet wird, sollten Sie die Einstellung für die sichere Übertragung nicht ändern.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Herstellen einer Verbindung mit [@AzureSupport](https://twitter.com/azuresupport), dem offiziellen Microsoft Azure-Konto zum Verbessern der Kundenfreundlichkeit. Verbinden der Azure-Community mit den richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support**. Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](../../azure-portal/supportability/how-to-create-azure-support-request.md). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.