---
title: Verwenden von Blob Storage für IIS und Table Storage für Ereignisse in Azure Monitor | Microsoft-Dokumentation
description: Azure Monitor kann die Protokolle für Azure-Dienste, die Diagnosedaten in Table Storage schreiben, sowie die IIS-Protokolle, die in Blob Storage geschrieben werden, lesen.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/14/2020
ms.openlocfilehash: 703a5f145aee93fe7ec4ad2f8ec102f98bdd4174
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100601492"
---
# <a name="collect-data-from-azure-diagnostics-extension-to-azure-monitor-logs"></a>Sammeln von Daten aus der Azure-Diagnoseerweiterung in Azure Monitor-Protokollen
Die Azure-Diagnoseerweiterung ist ein [Agent in Azure Monitor](../agents/agents-overview.md), der Überwachungsdaten aus dem Gastbetriebssystem von Azure-Computeressourcen einschließlich VMs sammelt. In diesem Artikel wird beschrieben, wie von der Diagnoseerweiterung gesammelte Daten aus Azure Storage in Azure Monitor-Protokollen erfasst werden.

> [!NOTE]
> Der Log Analytics-Agent in Azure Monitor ist normalerweise die bevorzugte Methode zum Sammeln von Daten aus dem Gastbetriebssystem in Azure Monitor-Protokollen. Einen ausführlichen Vergleich der Agents finden Sie unter [Übersicht über die Azure Monitor-Agents](../agents/agents-overview.md).

## <a name="supported-data-types"></a>Unterstützte Datentypen
Die Azure-Diagnoseerweiterung speichert Daten in einem Azure Storage-Konto. Damit diese Daten in Azure Monitor-Protokollen gesammelt werden können, müssen Sie sich an folgenden Speicherorten befinden:

| Protokolltyp | Ressourcentyp | Position |
| --- | --- | --- |
| IIS-Protokolle |Virtual Machines <br> Webrollen <br> Workerrollen |wad-iis-logfiles (Blob Storage) |
| syslog |Virtual Machines |LinuxsyslogVer2v0 (Table Storage) |
| Service Fabric – operative Ereignisse |Service Fabric-Knoten |WADServiceFabricSystemEventTable |
| Service Fabric Reliable Actor-Ereignisse |Service Fabric-Knoten |WADServiceFabricReliableActorEventTable |
| Service Fabric Reliable Service-Ereignisse |Service Fabric-Knoten |WADServiceFabricReliableServiceEventTable |
| Windows-Ereignisprotokolle |Service Fabric-Knoten <br> Virtual Machines <br> Webrollen <br> Workerrollen |WADWindowsEventLogsTable (Table Storage) |
| Windows-ETW-Protokolle |Service Fabric-Knoten <br> Virtual Machines <br> Webrollen <br> Workerrollen |WADETWEventTable (Table Storage) |

## <a name="data-types-not-supported"></a>Nicht unterstützte Datentypen

- Leistungsdaten aus dem Gastbetriebssystem
- IIS-Protokolle von Azure-Websites


## <a name="enable-azure-diagnostics-extension"></a>Aktivieren der Azure-Diagnoseerweiterung
Ausführliche Informationen zum Installieren und Konfigurieren der Diagnoseerweiterung finden Sie unter [Installieren und Konfigurieren der Microsoft Azure-Diagnoseerweiterung (WAD)](../agents/diagnostics-extension-windows-install.md) und [Verwenden der Linux-Diagnoseerweiterung zum Überwachen von Metriken und Protokollen](../../virtual-machines/extensions/diagnostics-linux.md). Hierbei können Sie das Speicherkonto angeben und die Erfassung der Daten konfigurieren, die Sie an Azure Monitor-Protokolle weiterleiten möchten.


## <a name="collect-logs-from-azure-storage"></a>Erfassen von Protokollen aus Azure Storage
Verwenden Sie das folgende Verfahren, um die Erfassung von Daten der Diagnoseerweiterung aus einem Azure Storage-Konto zu ermöglichen:

1. Navigieren Sie im Azure-Portal zu **Log Analytics-Arbeitsbereiche**, und wählen Sie Ihren Arbeitsbereich aus.
1. Klicken Sie im Abschnitt **Arbeitsbereichsdatenquellen** des Menüs auf die Option **Speicherkontoprotokolle**.
2. Klicken Sie auf **Hinzufügen**.
3. Wählen Sie das **Speicherkonto** aus, das die zu erfassenden Daten enthält.
4. Wählen Sie den **Datentyp** aus, den Sie sammeln möchten.
5. Der Wert für die Quelle wird automatisch basierend auf dem Datentyp aufgefüllt.
6. Klicken Sie auf **OK**, um die Konfiguration zu speichern.
7. Wiederholen Sie diesen Vorgang für weitere Datentypen.

Nach etwa 30 Minuten können Sie Daten aus dem Speicherkonto im Log Analytics-Arbeitsbereich sehen. Es werden nur Daten angezeigt, die nach dem Anwenden der Konfiguration in den Speicher geschrieben wurden. Der Arbeitsbereich liest keine bereits vorher im Speicherkonto vorhandenen Daten.

> [!NOTE]
> Das Portal überprüft nicht, ob die Quelle im Speicherkonto vorhanden ist oder ob neue Daten geschrieben werden.



## <a name="next-steps"></a>Nächste Schritte

* [Sammeln von Protokollen und Metriken für Azure-Dienste](../platform/resource-logs.md#send-to-log-analytics-workspace) für unterstützte Azure-Dienste.
* [Aktivieren Sie Lösungen](../insights/solutions.md) , um Einblick in die Daten bereitzustellen.
* [Erstellen Sie Suchabfragen](../log-query/log-query-overview.md) , um die Daten zu analysieren.

