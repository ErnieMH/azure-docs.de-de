---
title: include file
description: include file
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 09/10/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 8700bbfe697a6b5fb81380831950d704fcb1f5ff
ms.sourcegitcommit: 5d7f8c57eaae91f7d9cf1f4da059006521ed4f9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "90012853"
---
In der folgenden Tabelle finden Sie die Kontingente und Grenzwerte, die für [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) gelten. Informationen zu den Preisen von Event Hubs finden Sie unter [Event Hubs – Preise](https://azure.microsoft.com/pricing/details/event-hubs/).

Die folgenden Limits sind gängige Werte für die Tarife „Basic“ und „Standard“. 

| Begrenzung | `Scope` | Notizen | Wert |
| --- | --- | --- | --- |
| Anzahl von Event Hubs-Namespaces pro Abonnement |Subscription |- |100 |
| Anzahl von Event Hubs pro Namespace |Namespace |Nachfolgende Anforderungen für die Erstellung eines neuen Event Hub werden zurückgewiesen. |10 |
| Anzahl von Partitionen pro Event Hub |Entität |- |32 |
| Maximale Größe eines Event Hub-Namens |Entität |- | 256 Zeichen |
| Maximale Größe eines Consumergruppennamens |Entität |- | 256 Zeichen |
| Anzahl nicht epochenbezogener Empfänger pro Consumergruppe |Entität |- |5 |
| Maximale Durchsatzeinheiten |Namespace |Bei einer Überschreitung des Grenzwerts für Durchsatzeinheiten werden Ihre Daten gedrosselt, und es wird eine [ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) ausgelöst. Um eine höhere Anzahl von Durchsatzeinheiten für den Tarif „Standard“ anzufordern, erstellen Sie eine [Supportanfrage](/azure/azure-portal/supportability/how-to-create-azure-support-request). [Zusätzliche Durchsatzeinheiten](../articles/event-hubs/event-hubs-auto-inflate.md) sind für einen festgelegten Kaufpreis in 20er-Blöcken verfügbar. |20 |
| Anzahl von Autorisierungsregeln pro Namespace |Namespace|Nachfolgende Anforderungen zur Erstellung von Autorisierungsregeln werden abgelehnt.|12 |
| Die Anzahl der Aufrufe der GetRuntimeInformation-Methode | Entität | - | 50 pro Sekunde | 
| Anzahl von Regeln für virtuelle Netzwerke (VNET) und IP-Konfigurationen | Entität | - | 128 | 

### <a name="event-hubs-basic-and-standard---quotas-and-limits"></a>Event Hubs der Tarife „Basic“ und „Standard“: Kontingente und Grenzwerte
| Begrenzung | `Scope` | Notizen | Basic | Standard |
| --- | --- | --- | -- | --- |
| Maximale Größe des Event Hubs-Ereignisses|Entität | &nbsp; | 256 KB | 1 MB |
| Anzahl von Consumergruppen pro Event Hub |Entität | &nbsp; |1 |20 |
| Anzahl von AMQP-Verbindungen pro Namespace |Namespace |Nachfolgende Anforderungen für zusätzliche Verbindungen werden abgelehnt, und der aufrufende Code empfängt eine Ausnahme. |100 |5\.000|
| Maximale Aufbewahrungsdauer von Ereignisdaten |Entität | &nbsp; |1 Tag |1–7 Tage |
|Apache Kafka-fähiger Namespace|Namespace |Der Event Hubs-Namespace streamt Anwendungen unter Verwendung des Kafka-Protokolls. Weitere Informationen finden Sie unter [Verwenden von Azure Event Hubs aus Apache Kafka-Anwendungen](../articles/event-hubs/event-hubs-for-kafka-ecosystem-overview.md). |Nein | Ja |
|Erfassung |Entität | Ist diese Option aktiviert, werden Mikrobatches für den gleichen Stream verwendet. Weitere Informationen finden Sie unter [Erfassen von Ereignissen über Azure Event Hubs in Azure Blob Storage oder Azure Data Lake Storage](../articles/event-hubs/event-hubs-capture-overview.md). |Nein |Ja |


### <a name="event-hubs-dedicated---quotas-and-limits"></a>Event Hubs Dedicated – Kontingente und Limits
Das Event Hubs Dedicated-Angebot wird zu einem festen Monatspreis mit einem Minimum von vier Stunden Nutzung in Rechnung gestellt. Der Dedicated-Tarif umfasst die Funktionen des Standard-Tarifs, jedoch mit Kapazitäten und Limits auf Unternehmensniveau für Kunden mit anspruchsvollen Workloads. 

| Funktion | Einschränkungen |
| --- | ---|
| Bandbreite |  20 CUs |
| Namespaces | 50 pro CU |
| Event Hubs |  1000 pro Namespace |
| Eingangsereignisse | Enthalten |
| Nachrichtengröße | 1 MB |
| Partitionen | 2000 pro CU |
| Verbrauchergruppen | Kein Limit pro CU, 1000 pro Event Hub |
| Brokerverbindungen | 100.000 enthalten |
| Nachrichtenaufbewahrung | 90 Tage, 10 TB enthalten pro CU |
| Erfassung | Enthalten |
