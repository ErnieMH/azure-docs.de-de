---
title: InvalidNetworkSecurityGroupSecurityRules-Fehler – Azure HDInsight
description: 'Fehler bei der Clustererstellung. Fehlercode: InvalidNetworkSecurityGroupSecurityRules'
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.openlocfilehash: 1662be3bedb9ec0f2dc21e98ffbd57d8f8ed5777
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2020
ms.locfileid: "92540617"
---
# <a name="scenario-invalidnetworksecuritygroupsecurityrules---cluster-creation-fails-in-azure-hdinsight"></a>Szenario: InvalidNetworkSecurityGroupSecurityRules: Fehler bei der Clustererstellung in Azure HDInsight

In diesem Artikel werden Schritte zur Problembehandlung und mögliche Lösungen für Probleme bei der Interaktion mit Azure HDInsight-Clustern beschrieben.

## <a name="issue"></a>Problem

Sie erhalten den Fehlercode `InvalidNetworkSecurityGroupSecurityRules` mit einer Beschreibung wie: „Die Sicherheitsregeln in der Netzwerksicherheitsgruppe, die für das Subnetz konfiguriert ist, lässt die erforderliche ein- bzw. ausgehende Konnektivität nicht zu“.

## <a name="cause"></a>Ursache

Es liegt vermutlich ein Problem mit den Regeln für [Netzwerksicherheitsgruppen](../../virtual-network/virtual-network-vnet-plan-design-arm.md) für eingehenden Datenverkehr vor, die für Ihren Cluster konfiguriert wurden.

## <a name="resolution"></a>Lösung

Navigieren Sie zum Azure-Portal, und ermitteln Sie die NSG des Subnetzes, in dem der Cluster bereitgestellt wird. Stellen Sie im Abschnitt **Eingangssicherheitsregeln** sicher, dass die Regeln den Zugriff in eingehender Richtung auf Port 443 für die [hier](../control-network-traffic.md) genannten IP-Adressen zulassen.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Problem nicht aufgeführt ist oder Sie es nicht lösen können, besuchen Sie einen der folgenden Kanäle, um weitere Unterstützung zu erhalten:

* Nutzen Sie den [Azure-Communitysupport](https://azure.microsoft.com/support/community/), um Antworten von Azure-Experten zu erhalten.

* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.

* Sollten Sie weitere Unterstützung benötigen, senden Sie eine Supportanfrage über das [Azure-Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Wählen Sie dazu auf der Menüleiste die Option **Support** aus, oder öffnen Sie den Hub **Hilfe und Support** . Ausführlichere Informationen hierzu finden Sie unter [Erstellen einer Azure-Supportanfrage](../../azure-portal/supportability/how-to-create-azure-support-request.md). Zugang zu Abonnementverwaltung und Abrechnungssupport ist in Ihrem Microsoft Azure-Abonnement enthalten. Technischer Support wird über einen [Azure-Supportplan](https://azure.microsoft.com/support/plans/) bereitgestellt.