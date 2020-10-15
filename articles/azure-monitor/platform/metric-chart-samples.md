---
title: Beispiel für Azure Monitor-Metrikdiagramme
description: Erfahren Sie mehr über die Visualisierung Ihrer Azure Monitor-Daten.
author: vgorbenko
services: azure-monitor
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: 9b2ab664f319de07fd70bd1a22b1ba6d64ac208f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87320254"
---
# <a name="metric-chart-examples"></a>Beispiele für Metrikdiagramme 

Die Azure-Plattform stellt [mehr als tausend Metriken](./metrics-supported.md) bereit, von denen viele Dimensionen umfassen. Durch die Verwendung von [Dimensionsfiltern](./metrics-charts.md), das Anwenden einer [Aufteilung](./metrics-charts.md), die Steuerung des Diagrammtyps und eine Anpassung der Diagrammeinstellungen können Sie leistungsstarke Diagnoseansichten und Dashboards erstellen, die Einblick in den Zustand Ihrer Infrastruktur und Anwendungen bieten. Dieser Artikel zeigt einige Beispiele für die Diagramme, die Sie mit dem [Metrik-Explorer](./metrics-charts.md) erstellen können, und erklärt die notwendigen Schritte zur Konfiguration jedes dieser Diagramme.

Möchten Sie Ihre Diagrammbeispiele für andere Benutzer zur Verfügung stellen? Tragen Sie zu dieser Seite auf GitHub bei, und teilen Sie hier Ihre eigenen Diagrammbeispiele!

## <a name="website-cpu-utilization-by-server-instances"></a>CPU-Auslastung einer Website nach Serverinstanzen

Dieses Diagramm zeigt, ob die CPU-Auslastung für eine App Service-Ressource innerhalb des akzeptablen Bereichs liegt. Durch eine Aufschlüsselung nach Serverinstanz kann bestimmt werden, ob die Last in geeigneter Weise verteilt wurde. Aus dem Diagramm wird ersichtlich, dass die App vor 6 Uhr morgens auf einer einzelnen Serverinstanz ausgeführt und dann durch Hinzufügen einer weiteren Instanz zentral hochskaliert wurde.

![Liniendiagramm der durchschnittlichen CPU-Nutzung in Prozent nach Serverinstanz](./media/metric-chart-samples/cpu-by-instance.png)

### <a name="how-to-configure-this-chart"></a>Wie wird dieses Diagramm konfiguriert?

Wählen Sie Ihre App Service-Ressource aus, und suchen Sie nach der Metrik **CPU-Prozentsatz**. Klicken Sie dann auf **Aufteilung anwenden**, und wählen Sie die Dimension **Instanz** aus.

## <a name="application-availability-by-region"></a>Anwendungsverfügbarkeit nach Region

Zeigen Sie die Verfügbarkeit Ihrer Anwendung nach Regionen an, um festzustellen, an welchen geografischen Standorten Probleme vorliegen. Dieses Diagramm zeigt die Application Insights-Metrik zur Verfügbarkeit. Sie können sehen, dass für die überwachte Anwendung im Rechenzentrum „USA, Osten“ keine Probleme vorliegen, dass es aber in den Regionen „USA, Westen“ und „Asien, Osten“ teilweise zu Verfügbarkeitsproblemen kommt.

![Diagramm der durchschnittlichen Verfügbarkeit nach Standort](./media/metric-chart-samples/availability-run-location.png)

### <a name="how-to-configure-this-chart"></a>Wie wird dieses Diagramm konfiguriert?

Sie müssen zunächst die [Application Insights-Verfügbarkeitsüberwachung](../app/monitor-web-app-availability.md) für Ihre Website aktivieren. Anschließend wählen Sie die Application Insights-Ressource aus, und geben die Metrik zur Verfügbarkeit an. Wenden Sie auf die Dimension **Ausführungsort** eine Aufteilung an.

## <a name="volume-of-storage-account-transactions-by-api-name"></a>Anzahl von Speicherkontotransaktionen nach API-Name

Für Ihre Speicherkontoressource wird eine übermäßige Anzahl von Transaktionen verzeichnet. Anhand der Transaktionsmetrik können Sie feststellen, welche API für die Überlastung verantwortlich ist. Beachten Sie, dass das folgende Diagramm bei Filterung und Aufteilung mit der gleichen Dimension (API-Name) konfiguriert ist, um die Ansicht nur auf die relevanten API-Aufrufe zu beschränken:

![Balkendiagramm der API-Transaktionen](./media/metric-chart-samples/transactions-by-api.png)

### <a name="how-to-configure-this-chart"></a>Wie wird dieses Diagramm konfiguriert?

Bei der Metrikauswahl geben Sie Ihr Speicherkonto und die Metrik **Transaktionen** an. Ändern Sie den Diagrammtyp in **Balkendiagramm**. Klicken Sie auf **Aufteilung anwenden**, und wählen Sie die Dimension **API-Name** aus. Klicken Sie anschließend auf **Filter hinzufügen**, und wählen Sie erneut die Dimension **API-Name** aus. Wählen Sie im Filterdialogfeld die APIs aus, die im Diagramm angezeigt werden sollen.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu Azure Monitor-[Arbeitsmappen](./workbooks-overview.md)
* Weitere Informationen zum [Metrik-Explorer](metrics-charts.md)

