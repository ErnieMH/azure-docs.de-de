---
title: Verbinden von Forcepoint DLP mit Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Forcepoint DLP mit Azure Sentinel verbinden.
services: sentinel
author: yelevin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/20/2020
ms.author: yelevin
ms.openlocfilehash: 3bdb9d2b23ce05929ba5612e0c6a03fe1aab05de
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "77588245"
---
# <a name="connect-your-forcepoint-dlp-to-azure-sentinel"></a>Verbinden von Forcepoint DLP mit Azure Sentinel

> [!IMPORTANT]
> Der Datenconnector für Forcepoint DLP (Data Loss Prevention) in Azure Sentinel befindet sich derzeit in der Public Preview. Dieses Feature wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



Mit dem Forcepoint DLP-Connector können Sie DLP-Incidentdaten automatisch in Azure Sentinel exportieren. Sie können ihn verwenden, um Einblicke in Benutzeraktivitäten und Datenverlustincidents zu erhalten. Er ermöglicht außerdem eine Korrelation mit Daten aus Azure-Workloads und anderen Feeds und verbessert mit Workbooks in Azure Sentinel die Überwachungsfunktionalität.

> [!NOTE]
> Daten werden am geografischen Standort des Arbeitsbereichs gespeichert, in dem Sie Azure Sentinel ausführen.

## <a name="configure-and-connect-forcepoint-dlp"></a>Konfigurieren und Verbinden von Forcepoint DLP

Konfigurieren Sie Forcepoint DLP so, dass Incidentdaten im JSON-Format über die REST-API an Ihren Azure-Arbeitsbereich weitergeleitet werden, wie im [Forcepoint DLP-Integrationshandbuch](https://frcpnt.com/dlp-sentinel) erläutert.


## <a name="find-your-data"></a>Suchen nach den Daten

Nachdem der Forcepoint DLP-Connector eingerichtet wurde, werden die Daten in Log Analytics unter „CustomLogs **ForcepointDLPEvents_CL**“ angezeigt.


Um das relevante Schema in Log Analytics für Forcepoint DLP zu verwenden, suchen Sie nach **ForcepointDLPEvents_CL**.


## <a name="validate-connectivity"></a>Überprüfen der Konnektivität

Es kann bis zu 20 Minuten dauern, bis Ihre Protokolle in Log Analytics angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie gelernt, wie Sie Forcepoint DLP mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Verwenden Sie Arbeitsmappen](tutorial-monitor-your-data.md), um Ihre Daten zu überwachen.