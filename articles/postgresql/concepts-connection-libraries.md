---
title: Datenverbindungsbibliotheken – Azure Database for PostgreSQL – Einzelserver
description: In diesem Artikel werden verschiedene Bibliotheken und Treiber beschrieben, die Sie beim Schreiben von Anwendungen zum Erstellen von Verbindungen mit und Abfragen von „Azure Database for PostgreSQL – Einzelserver“ verwenden können.
author: sunilagarwal
ms.author: sunila
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: b8a1526605195b5eb24d8044f42b70ca5336bf7c
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107878306"
---
# <a name="connection-libraries-for-azure-database-for-postgresql---single-server"></a>Datenverbindungsbibliotheken für Azure Database for PostgreSQL – Einzelserver
In diesem Artikel werden Bibliotheken und Treiber aufgeführt, die Entwickler beim Entwickeln von Anwendungen zum Herstellen von Verbindungen mit und zum Abfragen von Azure Database for PostgreSQL verwenden können.

## <a name="client-interfaces"></a>Clientschnittstellen
Die meisten Clientsprachbibliotheken, die eine Verbindung mit dem Server für PostgreSQL herstellen, sind externe Projekte und werden unabhängig verteilt. Die aufgeführten Bibliotheken werden auf den Plattformen Windows, Linux und Mac unterstützt und für Verbindungen mit Azure Database for PostgreSQL verwendet. Im Abschnitt „Nächste Schritte“ werden mehrere Schnellstartbeispiele aufgelistet.

| **Sprache** | **Clientschnittstelle** | **Zusätzliche Informationen** | **Download** |
|--------------|----------------------------------------------------------------|-------------------------------------|--------------------------------------------------------------------|
| Python | [psycopg](http://initd.org/psycopg/) | DB API 2.0-konform | [Download](http://initd.org/psycopg/download/) |
| PHP | [php-pgsql](https://secure.php.net/manual/en/book.pgsql.php) | Datenbankerweiterung | [Installieren](https://secure.php.net/manual/en/pgsql.installation.php) |
| Node.js | [Pg npm package](https://www.npmjs.com/package/pg) | Reiner nicht blockierender JavaScript-Client | [Installieren](https://www.npmjs.com/package/pg) |
| Java | [JDBC](https://jdbc.postgresql.org/) | JDBC-Treiber vom Typ 4 | [Download](https://jdbc.postgresql.org/download.html)  |
| Ruby | [Pg gem](https://deveiate.org/code/pg/) | Ruby-Schnittstelle | [Download](https://rubygems.org/downloads/pg-0.20.0.gem) |
| Go | [Package pq](https://godoc.org/github.com/lib/pq) | Reiner Go-Postgres-Treiber | [Installieren](https://github.com/lib/pq/blob/master/README.md) |
| C\#/ .NET | [Npgsql](https://www.npgsql.org/) | ADO.NET-Datenanbieter | [Download](https://dotnet.microsoft.com/download) |
| ODBC | [psqlODBC](https://odbc.postgresql.org/) | ODBC-Treiber | [Download](https://www.postgresql.org/ftp/odbc/versions/) |
| C | [libpq](https://www.postgresql.org/docs/9.6/static/libpq.html) | Primäre Schnittstelle der C-Sprache | Enthalten |
| C++ | [libpqxx](http://pqxx.org/) | Schnittstelle C++ mit neuer Formatvorlage | [Download](http://pqxx.org/download/software/) |

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie diese Schnellstarts, in denen beschrieben wird, wie Azure Database for PostgreSQL mit der gewünschten Sprache verbunden und abgefragt wird:

[Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md) | [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [Go](./connect-go.md)
