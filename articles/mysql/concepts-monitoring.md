---
title: Überwachung – Azure Database for MySQL
description: In diesem Artikel werden die Überwachungs- und Warnmetriken (CPU, Speicher, Verbindungsstatistiken und Ähnliches) für Azure Database for MySQL beschrieben.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.custom: references_regions
ms.date: 8/13/2020
ms.openlocfilehash: 9e1bd3f555873503aa1f6ed9c804aced3620fb9e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91627515"
---
# <a name="monitoring-in-azure-database-for-mysql"></a>Überwachen in Azure Database for MySQL
Die Überwachung der Daten zu Ihren Servern unterstützt Sie bei der Problembehandlung und der Optimierung Ihrer Workloads. Azure Database for MySQL bietet verschiedene Metriken, die Einblicke in das Verhalten Ihres Servers ermöglichen.

## <a name="metrics"></a>Metriken
Alle Azure-Metriken werden im Minutentakt erfasst, und für jede Metrik steht ein Verlauf von 30 Tagen zur Verfügung. Sie können Warnungen für die Metriken konfigurieren. Eine Schritt-für-Schritt-Anleitung finden Sie unter [Use the Azure portal to set up alerts on metrics for Azure Database for PostgreSQL](howto-alert-on-metric.md) (Verwenden des Azure-Portals zum Einrichten von Warnungen zu Metriken für Azure Database for PostgreSQL). Darüber hinaus können weitere Aufgaben wie das Einrichten automatisierter Aktionen, das Durchführen erweiterter Analysen und das Archivieren des Verlaufs ausgeführt werden. Weitere Informationen finden Sie unter [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Liste der Metriken
Für Azure Database for MySQL sind folgende Metriken verfügbar:

|Metrik|Metrikanzeigename|Einheit|BESCHREIBUNG|
|---|---|---|---|
|cpu_percent|CPU in Prozent|Percent|Die CPU-Auslastung in Prozent|
|memory_percent|Arbeitsspeicher in Prozent|Percent|Die Arbeitsspeicherauslastung in Prozent|
|io_consumption_percent|E/A in Prozent|Percent|Die E/A-Auslastung in Prozent (Gilt nicht für Server im Tarif „Basic“.)|
|storage_percent|Speicher in Prozent|Percent|Der verwendete Speicher relativ zum Maximalwert des Servers (in Prozent)|
|storage_used|Verwendeter Speicher|Byte|Die Menge des verwendeten Speichers. Der vom Dienst verwendete Speicher kann die Datenbankdateien, Transaktionsprotokolle und Serverprotokolle umfassen.|
|serverlog_storage_percent|Serverprotokollspeicher in Prozent|Percent|Der Prozentsatz des Serverprotokollspeichers, der aus dem maximalen Serverprotokollspeicher des Servers verwendet wird.|
|serverlog_storage_usage|Verwendeter Serverprotokollspeicher|Byte|Die Menge des verwendeten Serverprotokollspeichers.|
|serverlog_storage_limit|Begrenzung des Serverprotokollspeichers|Byte|Der maximale Serverprotokollspeicher für diesen Server.|
|storage_limit|Speicherbegrenzung|Byte|Der maximale Speicher für diesen Server|
|active_connections|Die aktiven Verbindungen.|Anzahl|Die Anzahl aktiver Verbindungen mit dem Server|
|connections_failed|Verbindungsfehler|Anzahl|Die Anzahl von Verbindungsfehlern für den Server|
|seconds_behind_master|Replikationsverzögerung in Sekunden|Anzahl|Die Anzahl von Sekunden, die die Verzögerung des Replikatservers im Vergleich zum Quellserver angeben. (Gilt nicht für Server im Tarif „Basic“.)|
|network_bytes_egress|Netzwerk ausgehend|Byte|Ausgehender Netzwerkdatenverkehr über aktive Verbindungen.|
|network_bytes_ingress|Netzwerk eingehend|Byte|Eingehender Netzwerkdatenverkehr über aktive Verbindungen.|
|backup_storage_used|Verwendeter Sicherungsspeicher|Byte|Die Menge des verwendeten Sicherungsspeichers. Diese Metrik stellt den gesamten Speicherplatz dar, der von allen vollständigen Datenbanksicherungen, differenziellen Sicherungen und Protokollsicherungen beansprucht wurde, die auf der Grundlage der für den Server festgelegten Beibehaltungsdauer für Sicherungen aufbewahrt wurden. Die Häufigkeit der Sicherungen wird durch den Dienst verwaltet und im Artikel zu [Konzepten](concepts-backup.md) erläutert. Bei georedundantem Speicher wird doppelt so viel Sicherungsspeicher genutzt wie bei lokal redundantem Speicher.|

## <a name="server-logs"></a>Serverprotokolle
Sie können die Protokollierung von langsamen Abfragen und die Überwachungsprotokollierung auf Ihrem Server aktivieren. Diese Protokolle sind ebenfalls durch Azure-Diagnoseprotokolle in Azure Monitor-Protokolle, Event Hubs und im Speicherkonto verfügbar. Weitere Informationen zur Protokollierung finden Sie in den Artikeln über  [Überwachungsprotokolle](concepts-audit-logs.md) und [Protokolle für langsame Abfragen](concepts-server-logs.md).

## <a name="query-store"></a>Abfragespeicher
[Abfragespeicher](concepts-query-store.md) ist ein Feature, das die Abfrageleistung im Zeitablauf verfolgt, einschließlich Abfrageausführungszeitstatistiken und Warteereignissen. Das Feature speichert Informationen zur Laufzeitleistung der Abfrage im **mysql**-Schema. Sie können die Sammlung und Speicherung von Daten über verschiedene Konfigurationsoptionen steuern.

## <a name="query-performance-insight"></a>Query Performance Insight
[Query Performance Insight](concepts-query-performance-insight.md) arbeitet mit dem Abfragespeicher zusammen, um Visualisierungen bereitzustellen, auf die über das Azure-Portal zugegriffen werden kann. Diese Diagramme ermöglichen es Ihnen, wichtige Abfragen zu identifizieren, die sich auf die Leistung auswirken. Query Performance Insight ist im Abschnitt **Intelligente Leistung** auf der Portalseite Ihres Azure Database for MySQL-Servers verfügbar.

## <a name="performance-recommendations"></a>Leistungsempfehlungen
Das Feature [Leistungsempfehlungen](concepts-performance-recommendations.md) identifiziert Möglichkeiten zur Verbesserung der Workloadleistung. Unter „Leistungsempfehlungen“ erhalten Sie Empfehlungen zum Erstellen neuer Indizes, mit denen sich die Leistung Ihrer Workloads u. U. verbessern lässt. Um Indexempfehlungen zu generieren, berücksichtigt das Feature verschiedene Datenbankmerkmale einschließlich des Schemas und der Workload laut Abfragespeicher. Nach der Implementierung von Leistungsempfehlungen sollten Kunden die Leistung testen, um die Auswirkungen dieser Änderungen auszuwerten.

## <a name="planned-maintenance-notification"></a>Benachrichtigungen zu geplanten Wartungen

**Benachrichtigungen zu geplanten Wartungen** ermöglichen Ihnen das Empfangen von Warnungen für anstehende geplante Wartungsarbeiten an Azure Database for MySQL. Diese Benachrichtigungen sind in die geplante Wartung von [Service Health](../service-health/overview.md) integriert, sodass Sie alle geplanten Wartungsarbeiten für Ihre Abonnements an zentraler Stelle anzeigen können. Außerdem ist es hilfreich, die Benachrichtigungen an die richtigen Zielgruppen für verschiedene Ressourcengruppen zu richten, da möglicherweise unterschiedliche Ansprechpartner für verschiedene Ressourcen zuständig sind. Sie erhalten die Benachrichtigung über die anstehende Wartung 72 Stunden vor dem Ereignis.

Während der geplanten Wartung ist davon auszugehen, dass der Server neu gestartet wird und [vorübergehende Fehler](concepts-connectivity.md#transient-errors) auftreten. Meist werden diese Ereignisse in weniger als 60 Sekunden automatisch vom System behoben.

> [!IMPORTANT]
> Benachrichtigungen für geplante Wartungen sind derzeit in allen Regionen **mit Ausnahme von** „USA, Westen-Mitte“ als öffentliche Vorschau verfügbar.

### <a name="to-receive-planned-maintenance-notification"></a>Empfangen von Benachrichtigungen zu geplanten Wartungen

1. Wählen Sie im [Portal](https://portal.azure.com) die Option **Dienstintegrität** aus.
2. Wählen Sie im Abschnitt **Warnungen** die Option **Integritätswarnungen** aus.
3. Wählen Sie **+ Service Health-Warnung hinzufügen** aus, und füllen Sie die Felder aus.
4. Füllen Sie die erforderlichen Felder aus. 
5. Wählen Sie den **Ereignistyp** aus. Wählen Sie **Geplante Wartung** oder **Alle auswählen**.
6. Legen Sie unter **Aktionsgruppen** fest, wie Sie die Warnung erhalten möchten (Empfangen einer E-Mail, Auslösen einer Logik-App usw.)  
7. Stellen Sie sicher, dass „Regel beim Erstellen aktivieren“ auf „Ja“ festgelegt ist.
8. Wählen Sie **Warnungsregel erstellen** aus, um die Warnung fertig zu stellen.

Eine ausführliche Beschreibung der Schritte zum Erstellen von **Service Health-Warnungen** finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](../service-health/alerts-activity-log-service-notifications.md).

> [!Note]
> Es wird jeder Versuch unternommen, die **Benachrichtigung zur geplanten Wartung** für alle Ereignisse 72 Stunden im Voraus bereitzustellen. Im Fall von kritischen oder Sicherheitspatches können Benachrichtigungen jedoch zeitlich näher am Ereignis gesendet werden oder ganz entfallen.

## <a name="next-steps"></a>Nächste Schritte
- Anleitungen zum Erstellen einer Warnung zu einer Metrik finden Sie unter [Einrichten von Warnungen](howto-alert-on-metric.md).
- Weitere Informationen dazu, wie Sie mit dem Azure-Portal, der REST-API oder der CLI auf Metriken zugreifen bzw. diese exportieren, finden Sie unter [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
- Lesen Sie unseren Blog zu [Best Practices für die Überwachung Ihres Servers](https://azure.microsoft.com/blog/best-practices-for-alerting-on-metrics-with-azure-database-for-mysql-monitoring/) (in englischer Sprache).
