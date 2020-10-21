---
title: Konsistenzebenen und Azure Cosmos DB-APIs
description: Grundlegendes zur Zuordnung der Konsistenzebene zwischen verschiedenen APIs in Azure Cosmos DB und Apache Cassandra, MongoDB
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/6/2020
ms.reviewer: sngun
ms.openlocfilehash: bb8a413f2e2a3aa4a8facd533d822312bb61fa0e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91613559"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Konsistenzebenen und Azure Cosmos DB-APIs

Azure Cosmos DB bietet für gängige Datenbanken native Unterstützung für Wire Protocol-kompatible APIs. Dazu zählen MongoDB, Apache Cassandra, Gremlin und Azure Table Storage. Diese Datenbanken bieten keine genau definierten Konsistenzmodelle und keine von der SLA unterstützten Garantien für die Konsistenzebenen. Sie umfassen in der Regel nur eine Teilmenge der fünf in Azure Cosmos DB bereitgestellten Konsistenzmodelle.

Für die SQL-API, die Gremlin-API und die Tabellen-API wird die im Azure Cosmos-Konto konfigurierte Standardkonsistenzebene verwendet. 

Bei Verwenden der Cassandra-API oder der Azure Cosmos DB-API für MongoDB erhalten Anwendungen einen vollständigen Satz von Konsistenzebenen, die von Apache Cassandra bzw. MongoDB geboten werden, mit noch stärkeren Garantien für Konsistenz und Dauerhaftigkeit. In diesem Dokument sind die entsprechenden Konsistenzebenen von Azure Cosmos DB für die Konsistenzebenen von Apache Cassandra und MongoDB aufgeführt.

> [!NOTE]
> Das Standardkonsistenzmodell für Azure Cosmos DB lautet „Sitzung“. „Sitzung“ ist ein clientorientiertes Konsistenzmodell, das nicht nativ von Cassandra oder MongoDB unterstützt wird. Weitere Informationen dazu, welches Konsistenzmodell gewählt werden sollte, finden Sie unter [Konsistenzebenen in Azure Cosmos DB](consistency-levels.md).

## <a name="mapping-between-apache-cassandra-and-azure-cosmos-db-consistency-levels"></a><a id="cassandra-mapping"></a>Zuordnung zwischen Apache Cassandra- und Azure Cosmos DB-Konsistenzebenen

Im Gegensatz zu Azure Cosmos DB bietet Apache Cassandra nativ keine genau definierten Konsistenzgarantien.  Stattdessen bietet Apache Cassandra eine Schreib- und Lesekonsistenzebene, um in den Genuss von Vorteilen in Bezug auf Hochverfügbarkeit, Konsistenz und Latenz zu kommen. Bei Verwenden der Cassandra-API von Azure Cosmos DB gilt Folgendes: 

* Die Schreibkonsistenzebene von Apache Cassandra wird der Standardkonsistenzebene zugeordnet, die in Ihrem Azure Cosmos-Konto konfiguriert ist. Die Konsistenz für einen Schreibvorgang (CL) kann nicht anforderungsweise geändert werden.

* Azure Cosmos DB ordnet die vom Cassandra-Clienttreiber angegebene Lesekonsistenzebene dynamisch einer der Azure Cosmos DB-Konsistenzebenen zu, die bei einer Leseanforderung dynamisch konfiguriert wird. 

Die folgende Tabelle veranschaulicht, wie die nativen Cassandra-Konsistenzebenen den Konsistenzebenen von Azure Cosmos DB zugeordnet werden, wenn die Cassandra-API verwendet wird:  

:::image type="content" source="./media/consistency-levels-across-apis/consistency-model-mapping-cassandra.png" alt-text="Zuordnung zum Cassandra-Konsistenzmodell" lightbox="./media/consistency-levels-across-apis/consistency-model-mapping-cassandra.png" :::

## <a name="mapping-between-mongodb-and-azure-cosmos-db-consistency-levels"></a><a id="mongo-mapping"></a>Zuordnung zwischen Konsistenzebenen von MongoDB und Azure Cosmos DB

Im Gegensatz zu Azure Cosmos DB bietet MongoDB nativ keine genau definierten Konsistenzgarantien. Stattdessen ermöglicht MongoDB Benutzern nativ die Konfiguration folgender Konsistenzgarantien: eine Schreibbestätigung, eine Lesebestätigung und die „isMaster“-Anweisung, um die Lesevorgänge entweder zu primären oder sekundären Replikaten zu leiten, damit die gewünschte Konsistenzebene erreicht wird.

Wenn Sie die Azure Cosmos DB-API für MongoDB verwenden, behandelt der MongoDB-Treiber Ihren Schreibbereich als primäres Replikat und alle anderen Bereiche als Lesereplikat. Sie können wählen, welche mit Ihrem Azure Cosmos-Konto verknüpfte Region das primäre Replikat ist. 

Bei Verwenden der Azure Cosmos DB-API für MongoDB gilt Folgendes:

* Die Schreibbestätigung wird der Standardkonsistenzebene zugeordnet, die in Ihrem Azure Cosmos-Konto konfiguriert ist.

* Azure Cosmos DB ordnet die vom MongoDB-Clienttreiber angegebene Lesebestätigung dynamisch einer der Azure Cosmos DB-Konsistenzebenen zu, die bei einer Leseanforderung dynamisch konfiguriert wird.  

* Sie können eine bestimmte mit Ihrem Azure Cosmos-Konto verknüpfte Region als „Primär“ kennzeichnen, indem Sie die Region als erste beschreibbare Region festlegen. 

Die folgende Tabelle veranschaulicht, wie die nativen Schreib- und Lesebestätigungen von MongoDB den Konsistenzebenen von Azure Cosmos DB zugeordnet werden, wenn die Azure Cosmos DB-API für MongoDB verwendet wird:

:::image type="content" source="./media/consistency-levels-across-apis/consistency-model-mapping-mongodb.png" alt-text="Zuordnung zum Cassandra-Konsistenzmodell" lightbox= "./media/consistency-levels-across-apis/consistency-model-mapping-mongodb.png":::

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Konsistenzebenen und die Kompatibilität zwischen Azure Cosmos DB-APIs und Open-Source-APIs. Weitere Informationen finden Sie in folgenden Artikeln:

* [Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen](consistency-levels-tradeoffs.md)
* [MongoDB-Features, die von der API für MongoDB von Azure Cosmos DB unterstützt werden](mongodb-feature-support.md)
* [Apache Cassandra-Features, die von der Cassandra-API für Azure Cosmos DB unterstützt werden](cassandra-support.md)
