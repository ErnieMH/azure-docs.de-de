---
title: 'Ereignisquellen zur Streamingerfassung: Azure Time Series Insights Gen2 | Microsoft-Dokumentation'
description: Hier erfahren Sie mehr über das Streamen von Daten in Azure Time Series Insights Gen2.
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 03/18/2021
ms.openlocfilehash: 4021a705668db82e47a23808ef0f6546f86866be
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108138263"
---
# <a name="azure-time-series-insights-gen2-event-sources"></a>Azure Time Series Insights Gen2-Ereignisquellen

Ihre Azure Time Series Insights Gen2-Umgebung kann bis zu zwei Streamingereignisquellen enthalten. Zwei Arten von Azure-Ressourcen werden als Eingaben unterstützt:

- [Azure IoT Hub](../iot-hub/about-iot-hub.md)
- [Azure Event Hubs](../event-hubs/event-hubs-about.md)

Ereignisse müssen als UTF-8-codierte JSON-Dateien gesendet werden.

## <a name="create-or-edit-event-sources"></a>Erstellen oder Bearbeiten von Ereignisquellen

Eine Ereignisquelle ist die Verknüpfung zwischen Ihrem Hub und der Azure Time Series Insights Gen2-Umgebung. In Ihrer Ressourcengruppe wird eine separate Ressource vom Typ `Time Series Insights event source` erstellt. Die IoT Hub- oder Event Hub-Ressourcen können sich im selben Azure-Abonnement wie Ihre Azure Time Series Insights Gen2-Umgebung oder in einem anderen Abonnement befinden. Es hat sich jedoch bewährt, die Azure Time Series Insights-Umgebung und die IoT Hub- oder Event Hub-Ressource in derselben Azure-Region auszuführen.

Zum Erstellen, Bearbeiten oder Entfernen von Ereignisquellen in Ihrer Umgebung können Sie das [Azure-Portal](./tutorial-set-up-environment.md#create-an-azure-time-series-insights-gen2-environment), die [Azure CLI](/cli/azure/tsi/event-source), [Azure Resource Manager-Vorlagen](time-series-insights-manage-resources-using-azure-resource-manager-template.md) sowie die [REST-API](/rest/api/time-series-insights/management(gen1/gen2)/eventsources) nutzen.

> [!WARNING]
> Schränken Sie den öffentlichen Internetzugriff nicht auf einen Hub oder eine Ereignisquelle ein, der bzw. die von Time Series Insights verwendet wird. Andernfalls wird die erforderliche Verbindung unterbrochen.

## <a name="start-options"></a>Startoptionen

Beim Erstellen einer Ereignisquelle können Sie angeben, welche bereits vorhandenen Daten gesammelt werden sollen. Diese Einstellung ist optional. Die folgenden Optionen sind verfügbar:

| Name   |  BESCHREIBUNG  |  Beispiel einer Azure Resource Manager-Vorlage |
|----------|-------------|------|
| EarliestAvailable | Es werden alle bereits in der IoT Hub- oder Event Hub-Ressource vorhandenen Daten erfasst. | `"ingressStartAt": {"type": "EarliestAvailable"}` |
| EventSourceCreationTime |  Es werden alle Daten erfasst, die nach der Erstellung der Ereignisquelle eintreffen. Alle bereits vorhandenen Daten, die vor der Erstellung der Ereignisquelle gestreamt wurden, werden ignoriert. Dies ist die Standardeinstellung im Azure-Portal.   |   `"ingressStartAt": {"type": "EventSourceCreationTime"}` |
| CustomEnqueuedTime | In Ihrer Umgebung werden die Daten erfasst, die nach der benutzerdefinierten Zeit (UTC) in die Warteschlange eingereiht wurden. Alle Ereignisse, die zur benutzerdefinierten Zeit oder danach in die Warteschlange Ihrer IoT Hub- oder Event Hub-Ressource eingereiht wurden, werden erfasst und gespeichert. Alle Ereignisse, die vor der benutzerdefinierten Zeit in die Warteschlange eingereiht wurden, werden ignoriert. Hinweis: „Benutzerdefinierte Zeit“ bezieht sich auf den Zeitpunkt (in UTC), an dem das Ereignis in Ihrer IoT Hub- oder Event Hub-Ressource eingetroffen ist. Dieser Zeitpunkt ist nicht identisch mit der benutzerdefinierten [Zeitstempeleigenschaft](./concepts-streaming-ingestion-event-sources.md#event-source-timestamp) im Ereignistext. |     `"ingressStartAt": {"type": "CustomEnqueuedTime", "time": "2021-03-01T17:00:00.20Z"}` |

> [!IMPORTANT]
>
> - Wenn Sie „EarliestAvailable“ auswählen und über eine große Menge bereits vorhandener Daten verfügen, kann es anfänglich zu einer hohen Latenz kommen, da die Azure Time Series Insights Gen2-Umgebung alle Daten verarbeitet.
> - Die Latenz sollte mit zunehmender Indizierung der Daten abnehmen. Senden Sie ein Supportticket über das Azure-Portal, falls es bei Ihnen zu einer dauerhaft hohen Latenz kommt.

- EarliestAvailable

![EarliestAvailable-Diagramm](media/concepts-streaming-event-sources/event-source-earliest-available.png)

- EventSourceCreationTime

![EventSourceCreationTime-Diagramm](media/concepts-streaming-event-sources/event-source-creation-time.png)

- CustomEnqueuedTime

![CustomEnqueuedTime-Diagramm](media/concepts-streaming-event-sources/event-source-custom-enqueued-time.png)

## <a name="streaming-ingestion-best-practices"></a>Bewährte Methoden für die Streamingerfassung

- Erstellen Sie immer eine eindeutige Consumergruppe für Ihre Azure Time Series Insights Gen2-Umgebung, um Daten aus Ihrer Ereignisquelle zu nutzen. Die erneute Verwendung von Consumergruppen kann zu zufälligen Verbindungsunterbrechungen und Datenverlusten führen.

- Konfigurieren Sie Ihre Azure Time Series Insights Gen2-Umgebung und IoT Hub und/oder Event Hubs in derselben Azure-Region. Obwohl eine Ereignisquelle in einer anderen Region konfiguriert werden kann, wird dieses Szenario nicht unterstützt, und es kann keine Hochverfügbarkeit garantiert werden.

- Überschreiten Sie nicht den [Grenzwert Ihrer Umgebung für die Durchsatzrate](./concepts-streaming-ingress-throughput-limits.md) oder den Durchsatz pro Partition.

- Konfigurieren Sie eine [Verzögerungswarnung](./time-series-insights-environment-mitigate-latency.md#monitor-latency-and-throttling-with-alerts), damit Sie benachrichtigt werden, wenn in Ihrer Umgebung Probleme beim Verarbeiten von Daten auftreten. Unter [Produktionsworkloads](./concepts-streaming-ingestion-event-sources.md#production-workloads) unten finden Sie Vorschläge für Warnungsbedingungen.

- Verwenden Sie die Streamingerfassung nur für Daten in Quasi-Echtzeit und für aktuelle Daten. Das Streamen historischer Daten wird nicht unterstützt.

- Informieren Sie sich, wie Eigenschaften mit Escapezeichen versehen werden und wie JSON-[Daten vereinfacht und gespeichert](./concepts-json-flattening-escaping-rules.md) werden.

- Befolgen Sie das Prinzip der geringsten Rechte, wenn Sie Verbindungszeichenfolgen für Ereignisquellen angeben. Konfigurieren Sie für Event Hubs eine SAS-Richtlinie, die ausschließlich den *send*-Anspruch umfasst, und verwenden Sie für IoT Hub nur die *service connect*-Berechtigung.

> [!CAUTION]
> Wenn Sie die vorhandene IoT Hub- oder Event Hub-Ressource löschen und eine neue Ressource mit demselben Namen erstellen, müssen Sie eine neue Ereignisquelle erstellen, der Sie die neue IoT Hub- oder Event Hub-Ressource anfügen. Erst wenn Sie diesen Schritt ausgeführt haben, werden die Daten erfasst.

## <a name="production-workloads"></a>Produktionsworkloads

Zusätzlich zu den oben beschriebenen bewährten Methoden sollten Sie die folgenden geschäftskritischen Workloads implementieren.

- Erhöhen Sie Ihre IoT Hub- oder Event Hub-Datenaufbewahrungsdauer auf maximal sieben Tage.

- Erstellen von Umgebungswarnungen im Azure-Portal Mithilfe von Warnungen, die auf [Plattformmetriken basieren](./how-to-monitor-tsi-reference.md#metrics), können Sie das Verhalten von End-to-End-Pipelines überprüfen. Die Anweisungen zum Erstellen und Verwalten von Warnungen finden Sie [hier](./time-series-insights-environment-mitigate-latency.md#monitor-latency-and-throttling-with-alerts). Vorgeschlagene Warnungsbedingungen:

  - IngressReceivedMessagesTimeLag überschreitet 5 Minuten.
  - IngressReceivedBytes ist 0.
- Sorgen Sie dafür, dass die Erfassungslast zwischen Ihren IoT Hub- oder Event Hub-Partitionen ausgeglichen ist.

### <a name="historical-data-ingestion"></a>Erfassung historischer Daten

Die Streamingpipeline kann in Azure Time Series Insights Gen2 aktuell nicht zum Importieren historischer Daten verwendet werden. Wenn Sie ältere Daten in Ihre Umgebung importieren möchten, beachten Sie die folgenden Richtlinien:

- Streamen Sie Livedaten und historische Daten nicht parallel. Die Erfassung unsortierter Daten wirkt sich nachteilig auf die Abfrageleistung aus.
- Erfassen Sie historische Daten in zeitlicher Reihenfolge, um die bestmögliche Leistung zu erzielen.
- Bleiben Sie innerhalb der unten angegebenen Ratengrenzwerte für den Erfassungsdurchsatz.
- Deaktivieren Sie den warmen Speicher, wenn das Alter der Daten den Aufbewahrungszeitraum Ihres warmen Speichers übersteigt.

## <a name="event-source-timestamp"></a>Zeitstempel der Ereignisquelle

Beim Konfigurieren einer Ereignisquelle werden Sie dazu aufgefordert, eine Zeitstempel-ID-Eigenschaft anzugeben. Mit der Zeitstempeleigenschaft werden Ereignisse im Zeitverlauf nachverfolgt. Diese Zeit wird als Zeitstempel `$ts` in den [Abfrage-APIs](/rest/api/time-series-insights/dataaccessgen2/query/execute) und zum Zeichnen von Reihen im Azure Time Series Insights-Explorer verwendet. Wenn zum Zeitpunkt der Erstellung keine Eigenschaft bereitgestellt wird oder die Zeitstempeleigenschaft aus einem Ereignis fehlt, wird der Zeitpunkt der Einreihung in die IoT Hub- oder Event Hubs-Warteschlange als Standardwert verwendet. Werte der Zeitstempeleigenschaft werden in UTC gespeichert.

Im Allgemeinen entscheiden sich Benutzer dafür, die Zeitstempeleigenschaft anzupassen und den Zeitpunkt zu verwenden, zu dem der Sensor oder das Tag den Lesevorgang generiert hat, anstatt den Standardwert des Zeitpunkts der Einreihung in die Hub-Warteschlange zu verwenden. Dies ist besonders dann notwendig, wenn Geräte vorübergehenden Konnektivitätsverlust aufweisen und ein Batch verzögerter Nachrichten an Azure Time Series Insights Gen2 weitergeleitet wird.

Wenn sich der benutzerdefinierte Zeitstempel innerhalb eines geschachtelten JSON-Objekts oder eines Arrays befindet, müssen Sie den richtigen Eigenschaftennamen unter Berücksichtigung der [Namenskonventionen beim Vereinfachen und Versehen mit Escapezeichen](concepts-json-flattening-escaping-rules.md) bereitstellen. Der [hier](concepts-json-flattening-escaping-rules.md#example-a) angezeigte Zeitstempel der Ereignisquelle für die JSON-Nutzlast sollte als `"values.time"` eingegeben werden.

### <a name="time-zone-offsets"></a>Zeitzonenoffset

Zeitstempel müssen im ISO 8601-Format gesendet werden und werden im UTC-Format gespeichert. Wenn ein Zeitzonenoffset angegeben wird, wird der Offset angewendet und anschließend die Zeit im UTC-Format gespeichert und zurückgegeben. Wenn der Offset nicht ordnungsgemäß formatiert ist, wird er ignoriert. In Situationen, in denen Ihre Lösung möglicherweise keinen Kontext des ursprünglichen Offsets aufweist, können Sie die Offsetdaten in einer zusätzlichen separaten Ereigniseigenschaft senden, um sicherzustellen, dass sie beibehalten werden und Ihre Anwendung in einer Abfrageantwort verweisen kann.

Der Zeitzonenoffset sollte eines der folgenden Formate aufweisen:

±HHMMZ<br />
±HH:MM<br />
±HH:MMZ

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie die [JSON-Vereinfachungs- und -Escaperegeln](./concepts-json-flattening-escaping-rules.md), um zu verstehen, wie Ereignisse gespeichert werden.

- Informieren Sie sich über die [Durchsatzbeschränkungen](./concepts-streaming-ingress-throughput-limits.md) Ihrer Umgebung