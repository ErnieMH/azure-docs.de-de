---
title: Azure Cosmos DB-Ausgabe von Azure Stream Analytics
description: In diesem Artikel wird beschrieben, wie Daten aus Azure Stream Analytics in Azure Cosmos DB ausgegeben werden.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 08/25/2020
ms.openlocfilehash: e322135cfdb7aaff331367e84c603e8344436528
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90906250"
---
# <a name="azure-cosmos-db-output-from-azure-stream-analytics"></a>Azure Cosmos DB-Ausgabe von Azure Stream Analytics

[Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) ist ein global verteilter Datenbankdienst, der eine grenzenlose elastische Skalierung rund um den Globus, umfangreiche Abfragen und automatische Indizierung über schemaunabhängige Datenmodelle bietet. Weitere Informationen zu den Azure Cosmos DB-Containeroptionen für Stream Analytics finden Sie im Artikel [Azure Stream Analytics mit Azure Cosmos DB als Ausgabe](stream-analytics-documentdb-output.md).

Die Azure Cosmos DB-Ausgabe aus Stream Analytics ist zurzeit nicht in den Regionen Azure China 21Vianet und Azure Deutschland (T-Systems International) verfügbar.

> [!Note]
> Azure Stream Analytics unterstützt nur die Verbindung mit Azure Cosmos DB über die SQL-API.
> Andere Azure Cosmos DB-APIs werden noch nicht unterstützt. Wenn Sie Azure Stream Analytics auf die mit anderen APIs erstellten Azure Cosmos DB-Konten verweisen, werden die Daten unter Umständen nicht richtig gespeichert.

In der folgenden Tabelle werden die Eigenschaften zum Erstellen einer Azure Cosmos DB-Ausgabe beschrieben.

| Eigenschaftenname | BESCHREIBUNG |
| --- | --- |
| Ausgabealias | Ein Alias zum Verweisen auf diese Ausgabe in Ihrer Stream Analytics-Abfrage. |
| Senke | Azure Cosmos DB. |
| Importoption | Wählen Sie entweder **Cosmos DB über das eigene Abonnement auswählen** oder **Cosmos DB-Einstellungen manuell festlegen** aus.
| Konto-ID | Der Name oder Endpunkt-URI des Azure Cosmos DB-Kontos. |
| Kontoschlüssel | Der Schlüssel für den gemeinsamen Zugriff für das Azure Cosmos DB-Konto. |
| Datenbank | Der Name der Azure Cosmos DB-Datenbank. |
| Containername | Der zu verwendende Containername, der in Cosmos DB vorhanden sein muss. Beispiel:  <br /><ul><li> _MyContainer_: Ein Container namens „MyContainer“ muss vorhanden sein.</li>|
| Dokument-ID |Optional. Der Name des Felds in Ausgabeereignissen, das zur Angabe des Primärschlüssels verwendet wird, auf dem Einfüge- und Aktualisierungsvorgänge basieren.

## <a name="partitioning"></a>Partitionierung

Der Partitionsschlüssel basiert auf der PARTITION BY-Klausel in der Abfrage. Die Anzahl der Ausgabeschreiber folgt der Eingabepartitionierung für [vollständig parallelisierte Abfragen](stream-analytics-scale-jobs.md). Stream Analytics konvertiert den Cosmos DB-Ausgabepartitionsschlüssel in eine Zeichenfolge. Wenn Sie z. B. einen Partitionsschlüssel mit einem Wert von 1 vom Typ „bigint“ verwenden, wird er in „1“ vom Typ „string“ konvertiert.

## <a name="output-batch-size"></a>Ausgabebatchgröße

Die maximale Nachrichtengröße finden Sie unter [Einschränkungen für Azure Cosmos DB](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-cosmos-db-limits). Die Batchgröße und Schreibfrequenz werden basierend auf den Azure Cosmos DB-Antworten dynamisch angepasst. Es gelten keine vordefinierten Stream Analytics-Einschränkungen.

## <a name="next-steps"></a>Nächste Schritte

* [Schnellstart: Erstellen eines Stream Analytics-Auftrags mithilfe des Azure-Portals](stream-analytics-quick-create-portal.md)
* [Schnellstart: Erstellen eines Azure Stream Analytics-Auftrags mit der Azure CLI](quick-create-azure-cli.md)
* [Schnellstart: Erstellen eines Azure Stream Analytics-Auftrags mithilfe einer ARM-Vorlage](quick-create-azure-resource-manager.md)
* [Schnellstart: Erstellen eines Stream Analytics-Auftrags mit Azure PowerShell](stream-analytics-quick-create-powershell.md)
* [Schnellstart: Erstellen eines Azure Stream Analytics-Auftrags mithilfe von Visual Studio](stream-analytics-quick-create-vs.md)
* [Schnellstart: Erstellen eines Azure Stream Analytics-Auftrags in Visual Studio Code](quick-create-visual-studio-code.md)
