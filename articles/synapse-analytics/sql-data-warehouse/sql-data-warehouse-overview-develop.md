---
title: Ressourcen für die Entwicklung eines dedizierten SQL-Pools in Azure Synapse Analytics
description: Entwicklungskonzepte, Entwurfsentscheidungen, Empfehlungen und Programmiertechniken für einen dedizierten SQL-Pool in Azure Synapse Analytics
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 08/29/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: f158655cd7aeba647480325966c7f0cfcf65b13c
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93322140"
---
# <a name="design-decisions-and-coding-techniques-for-a-dedicated-sql-pool-in-azure-synapse-analytics"></a>Entwurfsentscheidungen und Programmiertechniken für einen dedizierten SQL-Pool in Azure Synapse Analytics 

 In diesem Artikel finden Sie weitere Ressourcen, die Ihnen helfen, wichtige Entwurfsentscheidungen, Empfehlungen und Programmiertechniken für einen dedizierten SQL-Pool in Azure Synapse besser zu verstehen.

## <a name="key-design-decisions"></a>Wesentliche Entwurfsentscheidungen

In den folgenden Artikeln werden Konzepte und Entwurfsentscheidungen für die Entwicklung eines verteilten Data Warehouse mithilfe der Funktionen für dedizierte SQL-Pools in Azure Synapse vorgestellt:

* [connections](../sql/connect-overview.md)
* [concurrency](resource-classes-for-workload-management.md)
* [Transaktionen](sql-data-warehouse-develop-transactions.md)
* [Benutzerdefinierte Schemas](sql-data-warehouse-develop-user-defined-schemas.md)
* [Tabellenverteilung](sql-data-warehouse-tables-distribute.md)
* [Tabellenindizes](sql-data-warehouse-tables-index.md)
* [Tabellenpartitionen](sql-data-warehouse-tables-partition.md)
* [CTAS](sql-data-warehouse-develop-ctas.md)
* [Statistiken](sql-data-warehouse-tables-statistics.md)

## <a name="development-recommendations-and-coding-techniques"></a>Entwicklungsempfehlungen und Programmiertechniken

In den folgenden Artikeln werden bestimmte Programmiertechniken, Tipps und Empfehlungen für die Entwicklung eines dedizierten SQL-Pools behandelt:

* [Gespeicherte Prozeduren](sql-data-warehouse-develop-stored-procedures.md)
* [Bezeichnungen](sql-data-warehouse-develop-label.md)
* [Ansichten](performance-tuning-materialized-views.md)
* [Temporäre Tabellen](sql-data-warehouse-tables-temporary.md)
* [Dynamisches SQL](sql-data-warehouse-develop-dynamic-sql.md)
* [Schleifen](sql-data-warehouse-develop-loops.md)
* [Gruppierungsoptionen](sql-data-warehouse-develop-group-by-options.md)
* [Variablenzuweisung](sql-data-warehouse-develop-variable-assignment.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Referenzinformationen finden Sie unter [T-SQL-Anweisungen](sql-data-warehouse-reference-tsql-statements.md).
