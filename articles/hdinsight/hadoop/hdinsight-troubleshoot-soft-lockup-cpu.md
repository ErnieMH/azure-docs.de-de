---
title: Fehler „Watchdog BUG soft lockup - CPU“ in einem Azure HDInsight-Cluster
description: Der Fehler „Watchdog BUG soft lockup CPU“ wird in Kernel-Syslog-Protokollen des Azure HDInsight-Clusters angezeigt
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/05/2019
ms.openlocfilehash: 6c5b2dd35fe6a61d34f24c10145d4d572c97f595
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2020
ms.locfileid: "92540583"
---
# <a name="scenario-watchdog-bug-soft-lockup---cpu-error-from-an-azure-hdinsight-cluster"></a>Szenario: Fehler „watchdog: BUG: soft lockup - CPU“ in einem Azure HDInsight-Cluster

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Die Kernel-Syslog-Protokolle enthalten die folgende Fehlermeldung: `watchdog: BUG: soft lockup - CPU`.

## <a name="cause"></a>Ursache

Ein [Fehler](https://bugzilla.kernel.org/show_bug.cgi?id=199437) im Linux-Kernel verursacht eine vorübergehende CPU-Blockierung.

## <a name="resolution"></a>Lösung

Wenden Sie einen Kernelpatch an. Das folgende Skript aktualisiert den Linux-Kernel und startet die Computer im Laufe von 24 Stunden zu verschiedenen Zeitpunkten neu. Führen Sie die Skriptaktion in zwei Batches aus. Der erste Batch wird für alle Knoten mit Ausnahme des Hauptknotens ausgeführt. Der zweite Batch wird für den Hauptknoten ausgeführt. Führen Sie die Batches nicht gleichzeitig für den Hauptknoten und die anderen Knoten aus.

1. Navigieren Sie im Azure-Portal zu Ihrem HDInsight-Cluster.

1. Rufen Sie die Skriptaktionen auf.

1. Wählen Sie **Neue übermitteln** aus, und geben Sie Folgendes ein:

    | Eigenschaft | Wert |
    | --- | --- |
    | Skripttyp | -Custom |
    | Name |Fix for kernel soft lock issue |
    | Bash-Skript-URI |`https://raw.githubusercontent.com/hdinsight/hdinsight.github.io/master/ClusterCRUD/KernelSoftLockFix/scripts/KernelSoftLockIssue_FixAndReboot.sh` |
    | Knotentyp(en) |Worker, Zookeeper |
    | Parameter |– |

    Wählen Sie **Speichern Sie diese Skriptaktion...** aus, wenn Sie das Skript beim Hinzufügen neuer Knoten ausführen möchten.

1. Klicken Sie auf **Erstellen** .

1. Warten Sie, bis die Ausführung erfolgreich abgeschlossen wurde.

1. Führen Sie die Skriptaktion für den Hauptknoten aus, indem Sie Schritt 3 erneut ausführen. Geben Sie dieses Mal jedoch den Knotentyp „Hauptknoten“ an.

1. Warten Sie, bis die Ausführung erfolgreich abgeschlossen wurde.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support** . Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](../../azure-portal/supportability/how-to-create-azure-support-request.md). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.