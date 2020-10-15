---
title: Ereignisdomänen in Azure Event Grid
description: Dieser Artikel beschreibt, wie Sie Ereignisdomänen verwenden, um den Fluss benutzerdefinierter Ereignisse für Ihre verschiedenen Geschäftsorganisationen, Kunden oder Anwendungen zu verwalten.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 02529ba770e636021cf9cec4ed555247e1c63d8c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86114362"
---
# <a name="understand-event-domains-for-managing-event-grid-topics"></a>Grundlegendes zu Ereignisdomänen für die Verwaltung von Event Grid-Themen

Dieser Artikel beschreibt, wie Sie Ereignisdomänen verwenden, um den Fluss benutzerdefinierter Ereignisse für Ihre verschiedenen Geschäftsorganisationen, Kunden oder Anwendungen zu verwalten. Verwenden Sie Ereignisdomänen für die folgenden Aufgaben:

* Verwalten mehrinstanzenfähiger Ereignisarchitekturen nach Maß.
* Verwalten der Autorisierung und Authentifizierung.
* Partitionieren von Themen, ohne diese einzeln zu verwalten.
* Vermeiden der individuellen Veröffentlichung für jeden einzelnen Themaendpunkt.

## <a name="event-domain-overview"></a>Übersicht über Ereignisdomänen

Eine Ereignisdomäne ist ein Verwaltungstool für eine große Anzahl von Event Grid-Themen, die sich auf dieselbe Anwendung beziehen. Sie können sie sich als ein Metathema vorstellen, das Tausende von Einzelthemen aufweisen kann.

Ereignisdomänen stellen Ihnen die Architektur, die Azure-Dienste (wie Storage und IoT Hub) zum Veröffentlichen ihrer Ereignisse verwenden, zur Verfügung. Sie ermöglichen es Ihnen, Ereignisse für Tausende von Themen zu veröffentlichen. Domänen geben Ihnen auch die Autorisierungs- und Authentifizierungskontrolle über jedes Thema, damit Sie Ihre Mandanten partitionieren können.

### <a name="example-use-case"></a>Beispiel eines Anwendungsfalls

Ereignisdomänen lassen sich am einfachsten an einem Beispiel veranschaulichen. Angenommen, Sie betreiben Contoso Construction Machinery, ein Unternehmen, das Traktoren, Bagger und anderes schweres Gerät herstellt. Als Teil des Geschäftsbetriebs übermitteln Sie Echtzeitinformationen zur Ausrüstungswartung, zur Systemintegrität und zu Vertragsaktualisierungen an Kunden. Alle diese Informationen werden an verschiedene Endpunkte weitergeleitet (z.B. an Ihre App, an Kundenendpunkte und andere Infrastrukturen, die Kunden eingerichtet haben).

Ereignisdomänen ermöglichen es Ihnen, Contoso Construction Machinery als eine einzige Ereigniseinheit zu modellieren. Jeder Ihrer Kunden wird als ein Thema in der Domäne dargestellt. Authentifizierung und Autorisierung werden mit Azure Active Directory durchgeführt. Jeder Ihrer Kunden kann sein Thema abonnieren und sich seine Ereignisse zustellen lassen. Verwalteter Zugriff über die Ereignisdomäne stellt sicher, dass die Kunden nur auf ihr jeweiliges Thema zugreifen können.

Außerdem erhalten Sie einen einzigen Endpunkt, in dem Sie alle Ihre Kundenereignisse veröffentlichen können. Event Grid sorgt dafür, dass jedes Thema nur die Ereignisse kennt, die sich auf den Mandanten beziehen.

![Contoso Construction-Beispiel](./media/event-domains/contoso-construction-example.png)

## <a name="access-management"></a>Zugriffsverwaltung

Mit einer Domäne erhalten Sie über die rollenbasierte Zugriffssteuerung (RBAC) in Azure differenzierte Autorisierungs- und Authentifizierungskontrolle über jedes Thema. Mit diesen Rollen können Sie jeden Mandanten in Ihrer Anwendung auf die Themen beschränken, auf die Sie ihm Zugriff gewähren möchten.

RBAC in Ereignisdomänen funktioniert auf dieselbe Weise wie die [verwaltete Zugriffssteuerung](security-authorization.md) im Rest von Event Grid und Azure. Verwenden Sie RBAC, um benutzerdefinierte Rollendefinitionen in Ereignisdomänen zu erstellen und zu erzwingen.

### <a name="built-in-roles"></a>Integrierte Rollen

Event Grid verfügt über zwei integrierte Rollendefinitionen, um die Nutzung von RBAC in Ereignisdomänen zu vereinfachen. Diese Rollen sind **EventGrid EventSubscription-Mitwirkender (Vorschau)** und **EventGrid EventSubscription-Leser (Vorschau)** . Sie können diese Rollen zu Benutzern zuweisen, die Themen in der Ereignisdomäne abonnieren müssen. Sie können die Rollenzuweisung auf das Thema beschränken, das Benutzer abonnieren müssen.

Weitere Informationen zu diesen Rollen finden Sie unter [Integrierte Rollen für Event Grid](security-authorization.md#built-in-roles).

## <a name="subscribing-to-topics"></a>Abonnieren von Themen

Das Abonnieren von Ereignissen zu einem Thema innerhalb einer Ereignisdomäne ist identisch mit dem [Erstellen eines Ereignisabonnements für ein benutzerdefiniertes Thema](./custom-event-quickstart.md) oder dem Abonnieren eines Ereignisses von einem Azure-Dienst.

### <a name="domain-scope-subscriptions"></a>Domänenbereichabonnements

Ereignisdomänen ermöglichen außerdem Domänenbereichabonnements. Ein Ereignisabonnement für eine Ereignisdomäne empfängt alle an die Domäne gesendeten Ereignisse, unabhängig davon, an welches Thema die Ereignisse gesendet werden. Domänenbereichabonnements können für Verwaltungs- und Überwachungszwecke nützlich sein.

## <a name="publishing-to-an-event-domain"></a>Veröffentlichen für eine Ereignisdomäne

Wenn Sie eine Ereignisdomäne erstellen, erhalten Sie einen Veröffentlichungsendpunkt, ähnlich wie beim Erstellen eines Themas in Event Grid. 

Um Ereignisse für ein beliebiges Thema in einer Ereignisdomäne zu veröffentlichen, übertragen Sie die Ereignisse [auf die gleiche Weise wie bei einem benutzerdefinierten Thema](./post-to-custom-topic.md) mithilfe von Push an den Endpunkt der Domäne. Der einzige Unterschied besteht darin, dass Sie das Thema angeben müssen, an das das Ereignis übermittelt werden soll.

Beispielsweise würde die Veröffentlichung des folgenden Arrays von Ereignissen das Ereignis mit `"id": "1111"` an das Thema `foo` senden, während das Ereignis mit `"id": "2222"` an das Thema `bar` gesendet würde:

```json
[{
  "topic": "foo",
  "id": "1111",
  "eventType": "maintenanceRequested",
  "subject": "myapp/vehicles/diggers",
  "eventTime": "2018-10-30T21:03:07+00:00",
  "data": {
    "make": "Contoso",
    "model": "Small Digger"
  },
  "dataVersion": "1.0"
},
{
  "topic": "bar",
  "id": "2222",
  "eventType": "maintenanceCompleted",
  "subject": "myapp/vehicles/tractors",
  "eventTime": "2018-10-30T21:04:12+00:00",
  "data": {
    "make": "Contoso",
    "model": "Big Tractor"
  },
  "dataVersion": "1.0"
}]
```

Ereignisdomänen übernehmen die Veröffentlichung von Themen für Sie. Anstatt Ereignisse für jedes Thema, das Sie verwalten, einzeln zu veröffentlichen, können Sie alle Ihre Ereignisse im Endpunkt der Domäne veröffentlichen. Event Grid stellt sicher, dass jedes Ereignis an das richtige Thema gesendet wird.

## <a name="limits-and-quotas"></a>Grenzen und Kontingente
Im Anschluss sind die Grenzwerte und Kontingente für Ereignisdomänen angegeben:

- 100.000 Themen pro Ereignisdomäne 
- 100 Ereignisdomänen pro Azure-Abonnement 
- 500 Ereignisabonnements pro Thema innerhalb einer Ereignisdomäne
- 50 Domänenbereichabonnements 
- 5\.000 Ereignisse pro Sekunde (Rate für die Erfassung in einer Domäne)

Sollten diese Grenzwerte für Sie nicht geeignet sein, wenden Sie sich an das Produktteam, indem Sie entweder ein Supportticket erstellen oder eine E-Mail an [askgrid@microsoft.com](mailto:askgrid@microsoft.com) senden. 

## <a name="pricing"></a>Preise
Für Ereignisdomänen gelten die gleichen [Betriebspreise](https://azure.microsoft.com/pricing/details/event-grid/) wie für alle anderen Features in Event Grid.

Vorgänge funktionieren in Ereignisdomänen wie in benutzerdefinierten Themen. Jede Erfassung eines Ereignisses in einer Ereignisdomäne ist ein Vorgang, und jeder Zustellungsversuch für ein Ereignis ist ein Vorgang.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zum Einrichten von Ereignisdomänen, zum Erstellen von Themen, zum Erstellen von Ereignisabonnements und zum Veröffentlichen von Ereignissen finden Sie unter [Verwalten von Ereignisdomänen](./how-to-event-domains.md).
