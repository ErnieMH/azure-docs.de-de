---
title: Update der klassischen Warnungen und Überwachung in Azure Monitor
description: Beschreibung der Einstellung der klassischen Überwachungsdienste und -funktionen, die bisher im Azure-Portal unter Benachrichtigungen (klassisch) angezeigt wurden.
author: yanivlavi
services: azure-monitor
ms.topic: conceptual
ms.date: 2/7/2019
ms.author: yalavi
ms.subservice: alerts
ms.openlocfilehash: 5fe33fc85768bec53bfb2c998c2e0518f79b2236
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100600912"
---
# <a name="unified-alerting--monitoring-in-azure-monitor-replaces-classic-alerting--monitoring"></a>Einheitliche Benachrichtigung und Überwachung in Azure Monitor tritt an die Stelle von klassischer Benachrichtigung und Überwachung

Azure Monitor hat sich zu einem vereinheitlichen, voll ausgestatteten Überwachungsdienst entwickelt, der nun ressourcenübergreifend einzelne Metriken und Benachrichtigungen unterstützt. Weitere Informationen finden Sie in unserem [Blogbeitrag zu Azure Monitor](https://azure.microsoft.com/blog/new-full-stack-monitoring-capabilities-in-azure-monitor/). Die neuen Überwachungs- und Benachrichtigungsplattformen von Azure sind schneller, intelligenter und erweiterbar. Dabei halten sie mit der zunehmenden Verbreitung von Cloud Computing Schritt und folgen der Philosophie der Microsoft Intelligent Cloud.

Mit der neuen Azure-Plattform für Überwachung und Warnungen werden klassische Warnungen in Azure Monitor für Benutzer der öffentlichen Cloud eingestellt. Für Ressourcen, die die neuen Warnungen noch nicht unterstützen, sind sie jedoch noch beschränkt im Einsatz. Das Deaktivierungsdatum für diese Warnungen wurde auf einen späteren Termin verschoben. In Kürze wird ein neues Datum für die Migration verbleibender Warnungen, [Azure Government-Cloud](../../azure-government/documentation-government-welcome.md) und [Azure China 21Vianet](https://docs.azure.cn/) bekannt gegeben.

 ![Klassische Warnung im Azure-Portal](media/monitoring-classic-retirement/monitor-alert-screen2.png) 

Wir empfehlen Ihnen, sich mit der neuen Plattform vertraut zu machen und damit zu beginnen, Warnungen auf dieser neu zu erstellen.

> [!IMPORTANT]
> Im Aktivitätsprotokoll erstellte klassische Warnungsregeln werden nicht als veraltet eingestuft und nicht migriert. Alle im Aktivitätsprotokoll erstellten klassischen Warnungsregeln können nun über „Azure Monitor – Warnungen“ unverändert aufgerufen und verwendet werden. Weitere Informationen finden Sie unter [Erstellen, Anzeigen und Verwalten von Aktivitätsprotokollwarnungen mit Azure Monitor](./alerts-activity-log.md). Warnungen zur Dienstintegrität können im neuen Abschnitt „Dienstintegrität“ unverändert aufgerufen und verwendet werden. Weitere Informationen finden Sie unter [Warnungen zu Dienstintegritätsbenachrichtigungen](../../service-health/alerts-activity-log-service-notifications-portal.md).

## <a name="unified-metrics-and-alerts-in-application-insights"></a>Einheitliche Metriken und Benachrichtigungen in Application Insights

Die neuere Metrikplattform von Azure Monitor unterstützt jetzt auch Überwachung aus Application Insights. Diese Umstellung bedeutet, dass Application Insights sich jetzt in Aktionsgruppen einhängt und damit weit mehr Optionen als die bisherigen E-Mail- und Webhook-Aufrufe bietet. Warnungen können jetzt Sprachanrufe, Azure Functions, Logik-Apps, SMS und ITSM-Tools wie ServiceNow und Automation-Runbooks auslösen. Mit der fast in Echtzeit erfolgenden Überwachung und Benachrichtigung können Benutzer von Application Insights dank der neuen Plattform die gleiche Technologie nutzen, die auch übergreifend die Überwachung in anderen Azure-Ressourcen unterstützt und den Unterbau bei der Überwachung von Microsoft-Produkten bildet.

Die neue einheitliche Überwachung und Benachrichtigung für Application Insights wird diese Punkte umfassen:

- **Application Insights-Plattformmetriken**: Beliebte vorkonfigurierte Metriken aus dem Application Insights-Produkt. Weitere Informationen finden Sie in diesem Artikel zur Verwendung von [Plattformmetriken für Application Insights im neuen Azure Monitor](../app/pre-aggregated-metrics-log-metrics.md#pre-aggregated-metrics).
- **Application Insights-Verfügbarkeits- und Web-Test**: Bietet die Möglichkeit, die Reaktionsfähigkeit und Verfügbarkeit Ihrer Web-App oder Ihres Servers zu beurteilen. Weitere Informationen finden Sie in diesem Artikel zum Verwenden von [Availability Tests and Alerts for Application Insights on new Azure Monitor](../app/monitor-web-app-availability.md) (Verfügbarkeitstests und Benachrichtigungen für Application Insights im neuen Azure Monitor).
- **Application Insights benutzerdefinierte Metriken**: Sie können eigene Metriken für Überwachung und Benachrichtigung definieren und ausgeben. Weitere Informationen finden Sie in diesem Artikel über [Custom Metric for Application Insights on new Azure Monitor](../app/pre-aggregated-metrics-log-metrics.md#custom-metrics-dimensions-and-pre-aggregation) (Benutzerdefinierte Metriken für Application Insights im neuen Azure Monitor).
- **Application Insights Fehleranomalien (Teil der intelligenten Erkennung)** : Benachrichtigt Sie automatisch und nahezu in Echtzeit, wenn es bei Ihrer Web-App zu einer ungewöhnlichen Zunahme von Fehlern bei HTTP-Anforderungen oder Abhängigkeitsaufrufen kommt. Weitere Informationen finden Sie in diesem Artikel zur Verwendung von [Intelligente Erkennung – Fehlerabweichungen](../app/proactive-failure-diagnostics.md).

## <a name="unified-metrics-and-alerts-for-other-azure-resources"></a>Einheitliche Metriken und Benachrichtigungen für andere Azure-Ressourcen

Die nächste Generation von Benachrichtigung und mehrdimensionaler Überwachung für Azure-Ressourcen ist seit März 2018 verfügbar. Die neuere Metrikplattform und die Benachrichtigungsfunktionalität ist jetzt schneller und bietet nahezu Echtzeitfähigkeit. Noch wichtiger ist, dass die Benachrichtigungen der neueren Metrikplattform mehr Granularität bieten, da die neue Plattform optional Dimensionen aufweist, die es Ihnen erlauben, mit Datenschnitten und Filtern bestimmte Wertkombinationen, Bedingungen oder Betriebsumstände zu suchen. Wie alle Benachrichtigungen im neuen Azure Monitor lassen sich die neuen Metrikwarnungen jetzt mithilfe von Aktionsgruppen besser erweitern: Benachrichtigungen können jenseits von E-Mail oder Webhook auch SMS, Sprache, Azure Functions, Automation-Runbooks und mehr verwenden. Weitere Informationen finden Sie unter [Erstellen, Anzeigen und Verwalten von Metrikwarnungen mit Azure Monitor](./alerts-metric.md).
Die neueren Metriken für Azure-Ressourcen sind in diesen Formen verfügbar:

- **Azure Monitor-Standard-Plattformmetriken**: beliebte, vorkonfigurierte Metriken aus verschiedenen Azure-Diensten und Produkten. Weitere Informationen finden Sie in diesen Artikeln über [Unterstützte Metriken in Azure Monitor](./alerts-metric-near-real-time.md#metrics-and-dimensions-supported) und [Unterstützte Metrikwarnungen in Azure Monitor](./alerts-metric-overview.md#supported-resource-types-for-metric-alerts).
- **Azure Monitor benutzerdefinierte Metriken**: Metriken aus benutzerdefinierten Quellen, einschließlich des Azure-Diagnose-Agents. Weitere Informationen finden Sie in diesem Artikel über [Benutzerdefinierte Metriken in Azure Monitor](../essentials/metrics-custom-overview.md). Mithilfe benutzerdefinierter Metriken können Sie auch vom [Microsoft Azure-Diagnose-Agent](../essentials/collect-custom-metrics-guestos-resource-manager-vm.md) und vom [InfluxData Telegraf-Agent](../essentials/collect-custom-metrics-linux-telegraf.md) erfasste Metriken veröffentlichen.

## <a name="retirement-of-classic-monitoring-and-alerting-platform"></a>Einstellung der klassischen Überwachungs- und Benachrichtigungsplattform

Wie bereits erwähnt, werden klassische Überwachungs- und Warnungsfeatures für Benutzer der öffentlichen Cloud eingestellt. Das schließt zugehörige APIs, die Schnittstelle zum Azure-Portal und die enthaltenen Dienste ein. Für Ressourcen, die die neuen Warnungen noch nicht unterstützen, werden sie jedoch noch eingeschränkt verwendet. Insbesondere werden diese Funktionen als veraltet markiert:

- Alte (klassische) Metriken und Warnungen für Azure-Ressourcen, die aktuell im Abschnitt [Warnungen (klassisch)](./alerts-classic.overview.md) im Azure-Portal zur Verfügung stehen, auf die als Ressource unter [microsoft.insights/alertrules](/rest/api/monitor/alertrules) zugegriffen wird
- Ältere (klassische) Plattform- und benutzerdefinierte Metriken für Application Insights sowie Benachrichtigungen für sie, die aktuell im Bereich [Warnungen (klassisch)](./alerts-classic.overview.md) im Azure-Portal verfügbar sind, auf die als Ressource unter [microsoft.insights/alertrules](/rest/api/monitor/alertrules) zugegriffen wird
- Ältere (klassische) Fehleranomaliebenachrichtigungen, die im Azure-Portal als [Intelligente Erkennung in Application Insights](../app/proactive-diagnostics.md) verfügbar sind; die dafür konfigurierten Warnungen werden im Bereich [Warnungen (klassisch)](./alerts-classic.overview.md) im Azure-Portal angezeigt

Das bedeutet:

- Klassische Überwachungs- und Warnungsdienste werden außer Betrieb genommen und sind für die Erstellung neuer Warnungsregeln nicht mehr verfügbar.
- Alle Warnungsregeln, die in „Warnungen (klassisch)“ noch vorhanden sind, werden auch weiterhin Benachrichtigungen ausführen und auslösen.
- Warnungsregeln der klassischen Überwachung und Warnung, die migriert werden können, werden von Microsoft automatisch in den äquivalenten Dienst auf der neuen Azure-Überwachungsplattform verschoben. Dies erfolgt in Phasen, die sich über mehrere Wochen erstrecken. Dabei handelt es sich um einen nahtlosen Prozess ohne Ausfallzeiten, bei dem für Kunden keine Überwachungslücken auftreten.
- Warnungsregeln, die auf die neue Warnungsplattform migriert werden, bieten die Überwachungsabdeckung wie bisher, lösen aber die Benachrichtigung mit neuen Nutzlasten aus. Alle E-Mail-Adressen, Webhookendpunkte oder Logik-App-Links, die mit einer klassischen Warnungsregel verbunden sind, werden bei der Migration übernommen, verhalten sich aber möglicherweise nicht richtig, da die Warnungsnutzlast auf der neuen Plattform eine andere ist.
- Einige [klassische Warnungsregeln, die nicht automatisch migriert werden können](../alerts/alerts-understand-migration.md#manually-migrating-classic-alerts-to-newer-alerts) und manuelle Aktionen von Benutzern erfordern, werden weiterhin ausgeführt.

> [!IMPORTANT]
> Microsoft Azure Monitor hat in Phasen ein [Tool zum freiwilligen Migrieren](../alerts/alerts-using-migration-tool.md) herausgebracht, mit dem Kunden ihre klassischen Warnungsregeln bald zur neuen Plattform migrieren können. Die Ausführung des Tools wird für alle noch vorhandenen klassischen Warnungsregeln erzwungen, die migriert werden können. Kunden müssen sicherstellen, dass die Automatisierung der Nutzung der Nutzlast klassischer Warnungsregeln für die Verarbeitung der neuen Nutzlast aus [Einheitlichen Metriken und Warnungen in Application Insights](#unified-metrics-and-alerts-in-application-insights) oder [Einheitlichen Metriken und Warnungen für andere Azure-Ressourcen](#unified-metrics-and-alerts-for-other-azure-resources) nach der Migration der klassischen Warnungsregeln angepasst wird. Weitere Informationen finden Sie unter [Vorbereiten Ihrer Logik-Apps und Runbooks für die Migration klassischer Warnungsregeln](../alerts/alerts-prepare-migration.md).

Dieser Artikel wird fortlaufend mit Links und Details zur neuen Überwachungs- und Benachrichtigungsfunktionalität von Azure sowie zur Verfügbarkeit von Tools aktualisiert, die Benutzer bei der Umstellung auf die neue Azure Monitor-Plattform unterstützen.

## <a name="pricing-for-migrated-alert-rules"></a>Preise für migrierte Warnungsregeln

Wir führen ein Migrationstool ein, das Sie beim Migrieren der [klassischen Warnungen](./alerts-classic.overview.md) von Azure Monitor zur neuen Benutzeroberfläche für Warnungen unterstützt. Die migrierten Warnungsregeln und die zugehörigen migrierten Aktionsgruppen (E-Mail, Webhook oder LogicApp) bleiben gebührenfrei. Die Funktionen, die Sie bei klassischen Warnungen nutzen konnten – Bearbeiten von Schwellenwerten, Aggregationstyp und Detailgenauigkeit einer Aggregation – bleiben auch bei der migrierten Warnungsregel weiterhin kostenlos verfügbar. Wenn Sie jedoch eine migrierte Warnungsregel bearbeiten, sodass diese ein Feature der neuen Warnungsplattform, eine Benachrichtigung oder einen Aktionstyp verwendet, wird die entsprechende Gebühr berechnet. Weitere Informationen zu den Preisen für Warnungsregeln und Benachrichtigungen finden Sie unter [Preise für Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).

Im Folgenden finden Sie einige Beispiele für Fälle, in denen für Ihre Warnungsregel eine Gebühr berechnet wird:

- Alle neuen (nicht migrierten) Warnungsregeln, die über die kostenlosen Einheiten auf der neuen Azure Monitor-Plattform erstellt werden
- Alle über die in Azure Monitor inbegriffenen kostenlosen Einheiten hinaus erfassten und aufbewahrten Daten
- Alle mehrfachen Web-Tests, die von Application Insights ausgeführt werden
- Alle benutzerdefinierten Metriken, die über die inbegriffenen kostenlosen Einheiten hinaus in Azure Monitor gespeichert werden
- Alle migrierten Warnungsregeln, die bearbeitet wurden und neuere Metrikwarnungsfeatures nutzen – z. B. Häufigkeit, mehrere Ressourcen/Dimensionen, [dynamischer Schwellwert](../alerts/alerts-dynamic-thresholds.md), Ändern von Ressourcen/Signalen usw.
- Alle migrierten Aktionsgruppen, die bearbeitet wurden und neuere Benachrichtigungen oder Aktionstypen wie SMS, Sprachanruf und/oder ITSM-Integration verwenden

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum [neuen einheitlichen Azure Monitor](../overview.md).
* Weitere Informationen über die neuen [Azure-Warnungen](../platform/alerts-overview.md).

