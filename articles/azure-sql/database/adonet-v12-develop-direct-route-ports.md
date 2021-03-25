---
title: Andere Ports als 1433
description: Clientverbindungen von ADO.NET mit Azure SQL-Datenbank können den Proxy umgehen und über andere Ports als Port 1433 direkt mit der Datenbank interagieren.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: sqldbrb=1, devx-track-dotnet
ms.devlang: ''
ms.topic: reference
author: stevestein
ms.author: sstein
ms.reviewer: genemi
ms.date: 06/11/2020
ms.openlocfilehash: 0d009522ea0d0986233983f8725549b618ffb537
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91444870"
---
# <a name="ports-beyond-1433-for-adonet-45"></a>Andere Ports als 1433 für ADO.NET 4.5
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Dieses Thema beschreibt das Verbindungsverhalten von Azure SQL-Datenbank für Clients mit ADO.NET 4.5 oder einer höheren Version.

> [!IMPORTANT]
> Informationen zur Verbindungsarchitektur finden Sie unter [Verbindungsarchitektur der Azure SQL-Datenbank](connectivity-architecture.md).
>

## <a name="outside-vs-inside"></a>Außerhalb im Vergleich zu innerhalb

Bei Verbindungen mit Azure SQL-Datenbank lautet die erste Frage, ob das Clientprogramm *außerhalb* oder *innerhalb* der Azure-Cloudgrenzen ausgeführt wird. In den Unterabschnitten werden zwei häufige Szenarien erläutert.

### <a name="outside-client-runs-on-your-desktop-computer"></a>*Außerhalb:* Client wird auf dem Desktopcomputer ausgeführt.

Port 1433 ist der einzige Port, der auf dem Desktopcomputer geöffnet sein muss, auf dem die Clientanwendung der SQL-Datenbank gehostet ist.

### <a name="inside-client-runs-on-azure"></a>*Innerhalb:* Client wird in Azure ausgeführt.

Wenn der Client innerhalb der Azure-Cloudgrenzen ausgeführt wird, verwendet er die sogenannte *direkte Route* für die Interaktion mit Azure SQL-Datenbank. Nachdem eine Verbindung hergestellt wurde, ist bei weiteren Interaktionen zwischen dem Client und der Datenbank kein Azure SQL-Datenbank-Gateway beteiligt.

Der Ablauf ist wie folgt:

1. ADO.NET 4.5 (oder höher) initiiert eine kurze Interaktion mit der Azure-Cloud und empfängt eine dynamisch ermittelte Portnummer.

   * Die dynamisch ermittelte Portnummer liegt im Bereich von 11000 bis 11999.
2. ADO.NET stellt dann eine direkte Verbindung mit SQL-Datenbank ohne Verwendung von Middleware her.
3. Abfragen werden direkt an die Datenbank gesendet, und Ergebnisse werden direkt an den Client zurückgegeben.

Stellen Sie sicher, dass der Portbereich von 11000 bis 11999 auf Ihrem Azure-Clientcomputer für ADO.NET 4.5-Clientinteraktionen mit SQL-Datenbank verfügbar ist.

* Insbesondere dürfen Ports in diesem Bereich keine anderen ausgehenden Blockierungen aufweisen.
* Auf Ihrer Azure-VM steuert die **Windows-Firewall mit erweiterter Sicherheit** die Porteinstellungen.
  
  * Sie können die [Benutzeroberfläche der Firewall](/sql/sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access) verwenden, um eine Regel hinzuzufügen, für die Sie das **TCP**-Protokoll sowie einen Portbereich mit einer Syntax wie **11000–11999** angeben.

## <a name="version-clarifications"></a>Erläuterungen zu Versionen

In diesem Abschnitt werden die Moniker erläutert, die auf Produktversionen verweisen. Außerdem sind einige Kombinationen von Versionen zwischen Produkten aufgeführt.

### <a name="adonet"></a>ADO.NET

* ADO.NET 4.0 unterstützt das Protokoll TDS 7.3, aber nicht 7.4.
* ADO.NET 4.5 und höher unterstützt das Protokoll TDS 7.4.

### <a name="odbc"></a>ODBC

* Microsoft SQL Server ODBC 11 oder höher

### <a name="jdbc"></a>JDBC

* Microsoft SQL Server JDBC 4.2 oder höher (JDBC 4.0 unterstützt zwar TDS 7.4, implementiert jedoch keine „Umleitung“)

## <a name="related-links"></a>Verwandte Links

* ADO.NET 4.6 wurde am 20. Juli 2015 veröffentlicht. Eine Ankündigung im Blog des .NET-Teams finden Sie [hier](https://devblogs.microsoft.com/dotnet/announcing-net-framework-4-6/).
* ADO.NET 4.5 wurde am 15. August 2012 veröffentlicht. Eine Ankündigung im Blog des .NET-Teams finden Sie [hier](https://devblogs.microsoft.com/dotnet/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code/).
  * Ein Blogbeitrag zu ADO.NET 4.5.1 steht [hier](https://devblogs.microsoft.com/dotnet/announcing-the-net-framework-4-5-1-preview/)bereit.

* Microsoft ODBC Driver 17 for SQL Server https://aka.ms/downloadmsodbcsql

* Verbinden mit Azure SQL-Datenbank V12 über Umleitung https://techcommunity.microsoft.com/t5/DataCAT/Connect-to-Azure-SQL-Database-V12-via-Redirection/ba-p/305362

* [Liste der TDS-Protokollversionen](https://www.freetds.org/)
* [Übersicht über die Entwicklung von SQL-Datenbanken](develop-overview.md)
* [Firewall für die Azure SQL-Datenbank](firewall-configure.md)
