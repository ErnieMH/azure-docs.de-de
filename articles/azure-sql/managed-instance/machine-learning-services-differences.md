---
title: Wichtige Unterschiede bei Machine Learning Services (Vorschauversion)
description: In diesem Artikel werden die wichtigsten Unterschiede zwischen Machine Learning Services in SQL Managed Instance und SQL Server Machine Learning Services beschrieben.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.reviewer: sstein, davidph
manager: cgronlun
ms.date: 05/27/2020
ms.openlocfilehash: 9ff2de18042c466bdd8fa6c71194fff4286c820d
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91325095"
---
# <a name="key-differences-between-machine-learning-services-in-azure-sql-managed-instance-and-sql-server"></a>Wichtige Unterschiede zwischen Machine Learning Services in Azure SQL Managed Instance und SQL Server

Die Funktionalität von [Machine Learning Services in SQL Managed Instance (Vorschauversion)](machine-learning-services-overview.md) ist nahezu identisch mit [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning). Nachfolgend finden Sie einige wesentliche Unterschiede.

> [!IMPORTANT]
> Machine Learning Services in SQL Managed Instance befindet sich in der öffentlichen Vorschauphase. Informationen zur Registrierung finden Sie unter [Registrieren für die Vorschauversion](machine-learning-services-overview.md#signup).

## <a name="preview-limitations"></a>Einschränkungen der Vorschau

Während der Vorschauphase gelten die folgenden Einschränkungen für den Dienst:

- Loopbackverbindungen funktionieren nicht (siehe [Loopbackverbindung zu SQL Server über ein Python- oder R-Skript](/sql/machine-learning/connect/loopback-connection)).
- Externe Ressourcenpools werden nicht unterstützt.
- Nur Python und R werden unterstützt. Externe Sprachen wie Java können nicht hinzugefügt werden.
- Szenarios, in denen [Message Passing Interface](https://docs.microsoft.com/message-passing-interface/microsoft-mpi) (MPI) verwendet wird, werden nicht unterstützt.

Aktualisieren Sie das Servicelevelziel im Falle einer Änderung an diesem, und öffnen Sie ein Supportticket, um die dedizierten Ressourceneinschränkungen für R/Python wieder zu aktivieren.

## <a name="language-support"></a>Sprachunterstützung

Das [Erweiterbarkeitsframework](https://docs.microsoft.com/sql/advanced-analytics/concepts/extensibility-framework) von Python und R wird in Machine Learning Services in SQL Managed Instance und SQL Server unterstützt. Folgende wichtige Unterschiede bestehen:

- Die ersten Versionen von Python und R unterscheiden sich zwischen Machine Learning Services in SQL Managed Instance und SQL Server:

  | System               | Python | R     |
  |----------------------|--------|-------|
  | Verwaltete SQL-Instanz | 3.7.1  | 3.5.2 |
  | SQL Server           | 3.5.2  | 3.3.3 |

- Es ist nicht erforderlich, `external scripts enabled` über `sp_configure` zu konfigurieren. Nachdem Sie für die Vorschauversion [registriert](machine-learning-services-overview.md#signup) sind, wird für Ihre Azure SQL Managed Instance maschinelles Lernen aktiviert.

## <a name="packages"></a>Pakete

Die Paketverwaltung von Python und R unterscheidet sich zwischen SQL Managed Instance und SQL Server. Die folgenden Unterschiede bestehen:

- Pakete können keine ausgehenden Netzwerkaufrufe ausführen. Diese Einschränkung ähnelt den [Standardfirewallregeln für Machine Learning Services](https://docs.microsoft.com//sql/advanced-analytics/security/firewall-configuration) in SQL Server, kann jedoch in SQL Managed Instance nicht geändert werden.
- Es gibt keine Unterstützung für Pakete, die von externen Runtimes (z.B. Java) abhängig sind oder Zugriff auf Betriebssystem-APIs für die Installation oder Verwendung benötigen.

Weitere Informationen zum Verwalten von Python- und R-Paketen finden Sie unter:

- [Abrufen von Paketinformationen für Python](https://docs.microsoft.com/sql/machine-learning/package-management/python-package-information?context=azure/sql-database/context/ml-context&view=sql-server-ver15)
- [Abrufen von R-Paketinformationen](https://docs.microsoft.com/sql/machine-learning/package-management/r-package-information?context=azure/sql-database/context/ml-context&view=sql-server-ver15)

## <a name="resource-governance"></a>Ressourcengovernance

Es ist nicht möglich, R-Ressourcen durch [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) und externe Ressourcenpools zu beschränken.

In der öffentlichen Vorschauversion sind R-Ressourcen auf ein Maximum von 20 % der SQL Managed Instance-Ressourcen festgelegt. Das richtet sich nach der ausgewählten Dienstebene. Weitere Informationen finden Sie unter [Kaufmodelle für Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers).

### <a name="insufficient-memory-error"></a>Fehler bei nicht ausreichendem Arbeitsspeicher

Wenn für R nicht genügend Arbeitsspeicher verfügbar ist, erhalten Sie eine Fehlermeldung. Häufige Fehlermeldungen:

- Fehler beim Kommunizieren mit der Runtime für das Skript "R" für die Anforderungs-ID: "*******". Überprüfen Sie die Anforderungen der Runtime
- Unerwarteter "R"-Skriptfehler beim Ausführen von "sp_execute_external_script" mit HRESULT 0x80004004. Externer Skriptfehler: "In C-Funktion 'R_AllocStringBuffer' konnte kein Speicher (0 MB) zugewiesen werden"
- Externer Skriptfehler: Fehler: Vektor dieser Größe kann nicht zugewiesen werden.

Die Arbeitsspeichernutzung hängt von der in R-Skripts verwendeten Anzahl und von der Anzahl parallel ausgeführter Abfragen ab. Wenn die obigen Fehlermeldungen angezeigt werden, können Sie Ihre Datenbank zur Lösung dieses Problems auf eine höhere Dienstebene hochstufen.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht finden Sie unter [Machine Learning Services in Azure SQL Managed Instance](machine-learning-services-overview.md).
- Weitere Informationen zur Verwendung von Python in Machine Learning Services finden Sie unter [Ausführen von Python-Skripts](https://docs.microsoft.com/sql/machine-learning/tutorials/quickstart-python-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=sql-server-ver15).
- Informationen zur Verwendung von R in Machine Learning Services finden Sie unter [Schnellstart: Ausführen einfacher R-Skripts mit SQL Machine Learning](https://docs.microsoft.com/sql/machine-learning/tutorials/quickstart-r-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=sql-server-ver15).
