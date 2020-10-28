---
title: Einführung in Azure Cosmos DB
description: Hier finden Sie Informationen zu Azure Cosmos DB. Diese global verteilte Datenbank mit mehreren Modellen ist für geringe Wartezeiten, flexible Skalierbarkeit und Hochverfügbarkeit konzipiert und bietet native Unterstützung für NoSQL-Daten.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: overview
ms.date: 10/23/2019
ms.openlocfilehash: cd6bbe963f0cee89c188053d0770816fdabc1b93
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2020
ms.locfileid: "92490389"
---
# <a name="welcome-to-azure-cosmos-db"></a>Willkommen bei Azure Cosmos DB

Moderne Anwendungen erfordern eine hohe Reaktionsschnelligkeit und müssen immer online sein. Um eine geringe Latenz und Hochverfügbarkeit zu erreichen, müssen Instanzen dieser Anwendungen in Rechenzentren eingesetzt werden, die sich in der Nähe ihrer Benutzer befinden. Anwendungen müssen in Echtzeit auf große Nutzungsänderungen zu Spitzenzeiten reagieren, immer größere Datenmengen speichern und diese den Benutzern innerhalb von Millisekunden zur Verfügung stellen.

Azure Cosmos DB ist der global verteilte Datenbankdienst von Microsoft mit mehreren Modellen. Mit Cosmos DB können Sie Durchsatz und Speicher mit nur einem Klick elastisch und unabhängig voneinander über eine beliebige Anzahl von weltweiten Azure-Regionen hinweg skalieren. Sie können den Durchsatz und Speicherplatz elastisch skalieren und von einem schnellen Datenzugriff im einstelligen Millisekundenbereich über Ihre bevorzugte API profitieren, z. B.: SQL, MongoDB, Cassandra, Tables oder Gremlin. Cosmos DB bietet umfassende [Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_3/) (Service Level Agreements, SLAs) mit Durchsatz-, Latenz-, Verfügbarkeits- und Konsistenzgarantien – dies kann kein anderer Datenbankdienst bereitstellen.

Sie können [Azure Cosmos DB kostenlos ohne Azure-Abonnement und ohne Verpflichtungen testen](https://azure.microsoft.com/try/cosmosdb/) oder ein [Azure Cosmos DB-Konto im Tarif „Free“](optimize-dev-test.md#azure-cosmos-db-free-tier) nutzen, bei dem die ersten 400 RU/s und 5 GB Speicher kostenlos sind.

> [!div class="nextstepaction"]
> [Azure Cosmos DB kostenlos testen](https://azure.microsoft.com/try/cosmosdb/)

:::image type="content" source="./media/introduction/azure-cosmos-db.png" alt-text="Azure Cosmos DB ist ein weltweit verteilter Datenbankdienst von Microsoft mit elastischer Aufskalierung, garantierter geringer Wartezeit, fünf Konsistenzmodellen und umfassenden SLA-Garantien." border="false":::

## <a name="key-benefits"></a>Hauptvorteile

### <a name="turnkey-global-distribution"></a>Globale, sofort einsatzbereite Verteilung

Cosmos DB ermöglicht es Ihnen, hochgradig reaktive und hochverfügbare Anwendungen weltweit zu erstellen. Cosmos DB repliziert Ihre Daten transparent, wo auch immer sich Ihre Benutzer befinden, sodass Ihre Benutzer mit dem Replikat der Daten interagieren können, das sich am nächsten befindet.

Cosmos DB ermöglicht es Ihnen, jederzeit mit einem Klick auf eine Schaltfläche Ihrem Cosmos-Konto eine der Azure-Regionen hinzuzufügen oder diese aus ihm zu entfernen. Cosmos DB repliziert Ihre Daten nahtlos in alle Regionen, die Ihrem Cosmos-Konto zugeordnet sind, während Ihre Anwendung dank der *Multihostingfunktionen* des Diensts weiterhin hochverfügbar ist. Weitere Informationen finden Sie im Artikel zur [globalen Verteilung](distribute-data-globally.md).

### <a name="always-on"></a>Always On

Aufgrund der engen Integration in die Azure-Infrastruktur und der [transparenten Replikation von Schreibvorgängen in mehreren Regionen](global-dist-under-the-hood.md) bietet Cosmos DB [eine Hochverfügbarkeit von 99,999 %](high-availability.md) sowohl für Lese- als auch für Schreibvorgänge. Cosmos DB bietet Ihnen auch die Möglichkeit, programmgesteuert (oder über das Portal) ein regionales Failover Ihres Cosmos-Kontos aufzurufen. Diese Funktion sorgt dafür, dass bei einem regionalen Notfall ein Failover Ihrer Anwendung durchgeführt werden kann.

### <a name="elastic-scalability-of-throughput-and-storage-worldwide"></a>Elastische Skalierbarkeit für Durchsatz und Speicher weltweit

Cosmos DB wurde mit transparenter horizontaler Partitionierung und Replikation von Schreibvorgängen in mehreren Regionen konzipiert und bietet eine nie dagewesene elastische Skalierbarkeit für alle Ihre Schreib- und Lesevorgänge rund um den Globus. Sie können mit einem einzigen API-Aufruf von Tausenden auf Hunderte von Millionen Anforderungen/Sek. rund um den Globus hochskalieren, wobei Sie nur für den von Ihnen benötigten Durchsatz (und Speicher) zahlen. Diese Funktionalität hilft Ihnen, unerwartete Belastungsspitzen in Ihren Workloads zu bewältigen, ohne für Spitzen übermäßig Kapazität vorhalten zu müssen. Weitere Informationen finden Sie unter [Partitionierung in Cosmos DB](partitioning-overview.md), [Bereitstellen des Durchsatzes für Container und Datenbanken](set-throughput.md) und [Globales Skalieren von bereitgestelltem Durchsatz](./request-units.md).

### <a name="guaranteed-low-latency-at-99th-percentile-worldwide"></a>Weltweit garantiert niedrige Latenz im 99. Quantil

Mit Cosmos DB können Sie global nutzbare Anwendungen mit hoher Reaktionsfähigkeit erstellen. Dank des neuen Protokolls für die Replikation von Schreibvorgängen in mehreren Regionen und der [schreiboptimierten Datenbank-Engine](index-policy.md) ohne Latches garantiert Cosmos DB Latenzen von unter 10 ms sowohl für (indizierte) Lesevorgänge als auch für Schreibvorgänge auf der ganzen Welt im 99. Quantil. Diese Funktion ermöglicht eine nachhaltige Erfassung von Daten und blitzschnelle Abfragen für hochgradig reaktive Anwendungen.

### <a name="precisely-defined-multiple-consistency-choices"></a>Genau definiert, mehrere Konsistenzoptionen

Beim Erstellen von global verteilten Anwendungen in Cosmos DB müssen Sie keine extremen [Kompromisse zwischen Konsistenz, Verfügbarkeit, Latenz und Durchsatz](./consistency-levels.md) mehr eingehen. Das Cosmos DB-Protokoll für die Replikation von Schreibvorgängen in mehreren Regionen bietet [fünf klar definierte Konsistenzoptionen](consistency-levels.md) ( *starke Veraltung* , *begrenzte Veraltung* , *Sitzungskonsistenz* , *Präfixkonsistenz* und *letztliche Konsistenz* ) für ein intuitives Programmiermodell mit geringen Wartezeiten und Hochverfügbarkeit für Ihre global verteilte Anwendung.

### <a name="no-schema-or-index-management"></a>Keine Schema- oder Indexverwaltung

Die Synchronisierung von Datenbankschemas und -indizes mit dem Schema einer Anwendung ist bei global verteilten Anwendungen besonders problematisch. Dank Cosmos DB müssen Sie sich nicht länger mit der Schema- oder Indexverwaltung befassen. Die Datenbank-Engine ist vollkommen unabhängig vom Schema.  Da keine Schema- und Indexverwaltung erforderlich ist, müssen Sie sich auch bei der Migration von Schemas keine Gedanken um Anwendungsausfallzeiten machen. Cosmos DB [indiziert automatisch alle Daten](index-policy.md) und ermöglicht blitzschnelle Abfragen.

### <a name="battle-tested-database-service"></a>Kampferprobter Datenbankdienst

Cosmos DB ist ein Basisdienst in Azure. Seit fast einem Jahrzehnt wird Cosmos DB von vielen Microsoft-Produkten für unternehmenskritische Anwendungen auf globaler Ebene eingesetzt, darunter Skype, Xbox, Microsoft 365, Azure und viele weitere. Heute ist Cosmos DB einer der am schnellsten wachsenden Dienste in Azure und wird von vielen externen Kunden und unternehmenskritischen Anwendungen genutzt, die eine elastische Skalierung, eine schlüsselfertige globale Verteilung sowie eine Replikation von Schreibvorgängen in mehreren Regionen für geringe Latenzen und hohe Verfügbarkeit für Lese- und Schreibvorgänge erfordern.

### <a name="ubiquitous-regional-presence"></a>Ubiquitäre regionale Präsenz

Cosmos DB ist in allen Azure-Regionen weltweit verfügbar. Hierzu zählen mehr als 54 Regionen in der öffentlichen Cloud, [Azure China 21Vianet](https://www.azure.cn/en-us/), Azure Deutschland, Azure Government und Azure Government for Department of Defense (DoD). Siehe [Hochverfügbarkeit mit Azure Cosmos DB](high-availability.md).

### <a name="secure-by-default-and-enterprise-ready"></a>Standardmäßig sicher und einsatzbereit.

Cosmos DB ist für eine [Vielzahl von Konformitätsstandards](compliance.md) zertifiziert. Zusätzlich werden alle Daten in Cosmos DB im Ruhezustand und während der Übertragung verschlüsselt. Cosmos DB bietet Autorisierung auf Zeilenebene und erfüllt strenge Sicherheitsstandards.

### <a name="significant-tco-savings"></a>Erhebliche Einsparungen bei den Gesamtbetriebskosten

Da Cosmos DB ein vollständig verwalteter Dienst ist, müssen Sie keine komplexen Bereitstellungen in mehreren Rechenzentren und Upgrades Ihrer Datenbanksoftware mehr verwalten und betreiben. Außerdem müssen Sie nicht mehr für Support, Lizenzierung oder Betrieb bezahlen oder Ihre Datenbank für Workloadspitzen bereitstellen. Weitere Informationen finden Sie unter [Optimieren der Kosten mit Cosmos DB](total-cost-ownership.md).

### <a name="industry-leading-comprehensive-slas"></a>Branchenweit führende, umfassende SLAs

Cosmos DB ist der erste und einzige Dienst, der [branchenführende umfassende SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/) bietet, die 99,999 % Hochverfügbarkeits-, Lese- und Schreiblatenzzeiten mit dem 99. Quantil, garantierten Durchsatz und Konsistenz umfassen.

### <a name="globally-distributed-operational-analytics-and-ai-with-natively-built-in-apache-spark"></a>Global verteilte operative Analysen und KI mit nativ integriertem Apache Spark

Sie können [Spark](spark-connector.md) direkt für in Cosmos DB gespeichert Daten ausführen. Diese Funktion ermöglicht es Ihnen, operative Analysen mit niedriger Latenz auf globaler Ebene durchzuführen, ohne die transaktionalen Workloads zu beeinträchtigen, die direkt mit Cosmos DB ausgeführt werden. Weitere Informationen finden Sie unter [Global verteilte operative Analysen](./synapse-link.md).

### <a name="develop-applications-on-cosmos-db-using-popular-open-source-software-oss-apis"></a>Entwickeln von Anwendungen in Cosmos DB mit gängigen OSS-APIs (Open Source Software)

Cosmos DB bietet verschiedene APIs zum Arbeiten mit den in Ihrer Cosmos-Datenbank gespeicherten Daten. Standardmäßig können Sie zum Abfragen Ihrer Cosmos-Datenbank [SQL verwenden](./sql-query-getting-started.md) (eine grundlegende API). Cosmos DB implementiert auch APIs für [Cassandra](cassandra-introduction.md), [MongoDB](mongodb-introduction.md), [Gremlin](graph-introduction.md) und [Azure Table Storage](table-introduction.md). Sie können Clienttreiber (und Tools) für die häufig verwendeten NoSQL-APIs (z.B. MongoDB, Cassandra, Gremlin) direkt auf Ihre Cosmos-Datenbank verweisen. Durch Unterstützung der Wire Protocols häufig verwendeter NoSQL-APIs ermöglicht Cosmos DB Folgendes:

* Einfaches Migrieren Ihrer Anwendung zu Cosmos DB unter Beibehaltung wesentlicher Teile Ihrer Anwendungslogik.
* Aufrechterhalten der Portierbarkeit und Unabhängigkeit von Cloudanbietern Ihrer Anwendung.
* Vollständig verwalteter Clouddienst mit branchenführenden, finanziell abgesicherten SLAs für die gängigen NoSQL-APIs. 
* Elastischen Skalieren des bereitgestellten Durchsatzes und Speichers für Ihre Datenbanken basierend auf Ihrem Bedarf und Bezahlung nur für den Durchsatz und den Speicher, den Sie benötigen. Dies führt zu erheblichen Kosteneinsparungen.

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Lösungen, die von Azure Cosmos DB profitieren

Von der [garantierten Hochverfügbarkeit](https://azure.microsoft.com/support/legal/sla/cosmos-db/), dem hohen Durchsatz, der geringen Latenz und der abstimmbaren Konsistenz von Cosmos DB profitieren alle [Web-, Mobil-, Gaming- und IoT-Anwendungen](use-cases.md), die auf [globaler Ebene](distribute-data-globally.md) große Datenmengen und zahlreiche Lese- und Schreibvorgänge mit Antwortzeiten nahezu in Echtzeit für verschiedenste Daten bewältigen müssen. Erfahren Sie mehr darüber, wie Azure Cosmos DB für die Erstellung von [IoT und Telematik](use-cases.md#iot-and-telematics), [Einzelhandel und Marketing](use-cases.md#retail-and-marketing), [Gaming](use-cases.md#gaming) sowie für [mobile Anwendungen und Webanwendungen](use-cases.md#web-and-mobile-applications) verwendet werden kann.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die grundlegenden Konzepte von Cosmos DB: [schlüsselfertige globale Verteilung](distribute-data-globally.md), [Partitionierung](partitioning-overview.md) und [bereitgestellter Durchsatz](request-units.md).

Erste Schritte mit Azure Cosmos DB mit einem unserer Schnellstarts:

* [Erste Schritte mit der SQL-API von Azure Cosmos DB](create-sql-api-dotnet.md)
* [Erste Schritte mit der Azure Cosmos DB-API für MongoDB](create-mongodb-nodejs.md)
* [Erste Schritte mit der Cassandra-API von Azure Cosmos DB](create-cassandra-dotnet.md)
* [Erste Schritte mit der Gremlin-API von Azure Cosmos DB](create-graph-dotnet.md)
* [Erste Schritte mit der Tabellen-API von Azure Cosmos DB](create-table-dotnet.md)

> [!div class="nextstepaction"]
> [Azure Cosmos DB kostenlos testen](https://azure.microsoft.com/try/cosmosdb/)