---
title: include file
description: include file
services: storage
author: MarkMcGeeAtAquent
ms.service: storage
ms.topic: include
ms.date: 04/06/2018
ms.author: mimig
ms.custom: include file
ms.openlocfilehash: 6cf6cc781a9fd5353f458c2317b51a47df779e4d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85942352"
---
Wenn Sie derzeit Azure Table Storage verwenden, bietet Ihnen der Wechsel zur Azure Cosmos DB-Tabellen-API folgende Vorteile:

|Funktion | Azure-Tabellenspeicher | Azure Cosmos DB-Tabellen-API |
| --- | --- | --- |
| Latency | Schnell, aber keine Obergrenzen für die Wartezeit. | Wartezeit im einstelligen Millisekundenbereich für Lese- und Schreibvorgänge, unterstützt durch weniger als 10 ms Wartezeit bei Lese- und weniger als 15 ms Wartezeit bei Schreibvorgängen im 99. Perzentil, bei beliebiger Skalierung weltweit. |
| Throughput | Variables Durchsatzmodell. Tabellen verfügen über eine maximale Skalierbarkeit von 20.000 Vorgängen/s. | Hochgradig skalierbar mit einem [dedizierten reservierten Durchsatz pro Tabelle](../articles/cosmos-db/request-units.md), der durch SLAs abgedeckt ist. Konten haben keine Obergrenze für den Durchsatz und unterstützen pro Tabelle mehr als 10 Millionen Vorgänge/s. |
| Globale Verteilung | Einzelne Region mit einer optionalen sekundären Leseregion für Hochverfügbarkeit. Es kann kein Failover initiiert werden. | [Globale, sofort einsatzbereite Verteilung](../articles/cosmos-db/distribute-data-globally.md) für 1 bis mehr als 30 Regionen. Unterstützung von [automatischen und manuellen Failovern](../articles/cosmos-db/high-availability.md) zu jeder Zeit und an jedem Ort der Welt. |
| Indizierung | Nur primärer Index für PartitionKey und RowKey. Keine sekundären Indizes. | Automatische und vollständige Indizierung für alle Eigenschaften, keine Indexverwaltung. |
| Abfrage | Abfrageausführung verwendet Index für Primärschlüssel, andernfalls die Suche. | Abfragen können die automatische Indizierung für Eigenschaften für schnelle Abfragezeiten nutzen. |
| Konsistenz | „Stark“ in der primären Region. „Möglich“ in der sekundären Region. | [Fünf klar definierte Konsistenzebenen](../articles/cosmos-db/consistency-levels.md) zur Abstimmung von Verfügbarkeit, Latenz, Durchsatz und Konsistenz basierend auf Ihren Anwendungsanforderungen. |
| Preise | Speicheroptimiert | Durchsatzoptimiert |
| SLAs | Verfügbarkeit von 99,99%. | Verfügbarkeit von 99,99% (SLA) für alle Konten für eine einzelne Region und alle Konten für mehrere Regionen mit gelockerter Konsistenz sowie Leseverfügbarkeit von 99,999% für alle Datenbankkonten für mehrere Regionen ([branchenführende umfassende SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/)) mit allgemeiner Verfügbarkeit. |
