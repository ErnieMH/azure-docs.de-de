---
title: include file
description: include file
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/05/2019
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 609b62312329b3a8f9f16d15458a0a47f5eb377f
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "85115205"
---
Sie können zum Abrufen und Filtern Ihrer Daten Abfragen im Daten-Explorer verwenden.

1. Sehen Sie sich im Daten-Explorer oben auf der Registerkarte **Elemente** die Standardabfrage `SELECT * FROM c` an. Diese Abfrage ruft alle Dokumente aus dem Container ab und zeigt sie nach ID sortiert an. 
   
   :::image type="content" source="./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-query.png" alt-text="Die Standardabfrage im Daten-Explorer ist „SELECT * FROM c“.":::
   
1. Wählen Sie zum Ändern der Abfrage **Filter bearbeiten** aus, ersetzen Sie die Standardabfrage durch`ORDER BY c._ts DESC`, und wählen Sie dann **Filter anwenden** aus.
   
   :::image type="content" source="./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edit-query.png" alt-text="Die Standardabfrage im Daten-Explorer ist „SELECT * FROM c“.":::

   Mithilfe der geänderten Abfrage werden die Dokumente auf Basis ihrer Zeitstempel in absteigender Reihenfolge angezeigt, sodass Ihr zweites Dokument nun zuerst aufgeführt wird. 
   
   :::image type="content" source="./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edited-query.png" alt-text="Die Standardabfrage im Daten-Explorer ist „SELECT * FROM c“.":::

Wenn Sie mit der SQL-Syntax vertraut sind, können Sie eine der unterstützten [SQL-Abfragen](../articles/cosmos-db/sql-api-sql-query.md) in das Feld für das Abfrageprädikat eingeben. Der Daten-Explorer kann auch zum Erstellen von gespeicherten Prozeduren, UDFs und Triggern für serverseitige Geschäftslogik verwendet werden. 

Der Daten-Explorer ermöglicht über das Azure-Portal einfachen Zugriff auf die gesamten integrierten programmgesteuerten Datenzugriffsfunktionen in den APIs. Sie verwenden das Portal außerdem zum Skalieren des Durchsatzes, zum Abrufen von Schlüsseln und Verbindungszeichenfolgen sowie zum Überprüfen von Metriken und SLAs für Ihr Azure Cosmos DB-Konto. 

