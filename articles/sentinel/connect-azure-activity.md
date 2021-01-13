---
title: Verknüpfen von Azure-Aktivitätsdaten mit Azure Sentinel | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Azure-Aktivitätsprotokolle mit nur einem Klick an Azure Sentinel streamen. Im Aktivitätsprotokoll werden Ereignisse auf Abonnementebene für die gesamte Azure-Umgebung erfasst und angezeigt.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 8c25baa8-b93b-41da-9e6c-15bb7b5c5511
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/22/2020
ms.author: yelevin
ms.openlocfilehash: 88f363406ec40dfa439a52ad351501d1c64dce95
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85564495"
---
# <a name="connect-data-from-azure-activity-log"></a>Verknüpfen von Daten aus dem Azure-Aktivitätsprotokoll

Sie können Protokolle aus dem [Azure-Aktivitätsprotokoll](../azure-monitor/platform/platform-logs-overview.md) mit einem einzigen Klick an Azure Sentinel streamen. Das Aktivitätsprotokoll ist ein Abonnementprotokoll, das Ereignisse auf Abonnentenebene in ganz Azure aufzeichnet und anzeigt, von operativen Daten von Azure Resource Manager bis hin zu Aktualisierungen von Service Health-Ereignissen. Mit dem Aktivitätsprotokoll können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für jeden Schreibvorgang (PUT, POST, DELETE) ermitteln, der für die Ressourcen Ihres Abonnements ausgeführt wurde. Sie können auch den Status des Vorgangs und andere relevante Eigenschaften erfahren. Das Aktivitätsprotokoll umfasst keine Lesevorgänge (GET) oder Vorgänge für Ressourcen, die das klassische Modell/RDFE-Modell verwenden. 

## <a name="prerequisites"></a>Voraussetzungen

- Ihr Benutzer muss über Berechtigungen vom Typ „Mitwirkender“ für den Log Analytics-Arbeitsbereich verfügen.
- Ihr Benutzer muss über Leseberechtigungen für jedes Abonnement verfügen, dessen Protokolle Sie in Azure Sentinel streamen möchten.

## <a name="set-up-the-azure-activity-connector"></a>Einrichten des Azure-Aktivitätsconnectors

1. Klicken Sie im Azure Sentinel-Navigationsmenü auf **Datenconnectors**. Klicken Sie in der Liste mit Connectors auf **Azure-Aktivität** und dann auf die Schaltfläche **Connectorseite öffnen** unten rechts.

2. Klicken Sie unter der Registerkarte **Anweisungen** auf den Link **Azure-Aktivitätsprotokolle konfigurieren >** .

3. Wählen Sie im Bereich für das **Azure-Aktivitätsprotokoll** die Abonnements aus, deren Protokolle Sie an Azure Sentinel streamen möchten. 

4. Klicken Sie in dem sich rechts öffnenden Abonnementfenster auf **Verbinden**.

5. Geben Sie `AzureActivity` in das Abfragefenster ein, um das entsprechende Schema in Log Analytics für Azure-Aktivitätswarnungen zu verwenden.

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie das Azure-Aktivitätsprotokoll mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit dem Erkennen von Bedrohungen mit Azure Sentinel mithilfe von [integrierten](tutorial-detect-threats-built-in.md) oder [benutzerdefinierten](tutorial-detect-threats-custom.md) Regeln.
