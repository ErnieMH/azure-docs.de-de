---
title: Migrieren Sie SQL Server-Windows-Benutzern und -Gruppen in eine verwaltete SQL-Instanz mit T-SQL
description: Hier erfahren Sie, wie Sie Windows-Benutzer und -Gruppen in einer SQL Server-Instanz zu einer verwalteten Azure SQL-Instanz migrieren.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: security
ms.custom: seo-lt-2019, sqldbrb=1
ms.topic: tutorial
author: GitHubMirek
ms.author: mireks
ms.reviewer: vanto
ms.date: 10/30/2019
ms.openlocfilehash: f2dd34ab7c6ee5be26836e4abb86960605ee44ee
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84708660"
---
# <a name="tutorial-migrate-windows-users-and-groups-in-a-sql-server-instance-to-azure-sql-managed-instance-using-t-sql-ddl-syntax"></a>Tutorial: Migrieren von Windows-Benutzern und -Gruppen in einer SQL Server-Instanz zu einer verwalteten Azure SQL-Instanz unter Verwendung der T-SQL-DDL-Syntax
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

> [!NOTE]
> Die Syntax, die in diesem Artikel zum Migrieren von Benutzern und Gruppen zu einer verwalteten SQL-Instanz verwendet wird, befindet sich in der **Public Preview-Phase**.

In diesem Artikel erfahren Sie, wie Sie Ihre lokalen Windows-Benutzer und -Gruppen in Ihrer SQL Server-Instanz unter Verwendung der T-SQL-Syntax zu einer verwalteten Azure SQL-Instanz migrieren.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> - Erstellen von Anmeldungen für SQL Server
> - Erstellen einer Testdatenbank für die Migration
> - Erstellen von Anmeldungen, Benutzern und Rollen
> - Sichern und Wiederherstellen Ihrer Datenbank in einer verwalteten SQL-Instanz (MI)
> - Manuelles Migrieren von Benutzern zur MI mithilfe der ALTER USER-Syntax
> - Testen der Authentifizierung mit den neuen zugeordneten Benutzern

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorials ist Folgendes erforderlich:

- Die Windows-Domäne muss sich in einem Verbund mit Azure Active Directory (Azure AD) befinden.
- Zugriff auf Active Directory für die Erstellung von Benutzern/Gruppen
- Eine SQL Server-Instanz in Ihrer lokalen Umgebung
- Eine vorhandene verwaltete SQL-Instanz. Weitere Informationen finden Sie unter [Schnellstart: Erstellen einer verwalteten SQL-Instanz](instance-create-quickstart.md).
  - Ein `sysadmin` in der verwalteten SQL-Instanz muss verwendet werden, um Azure AD-Anmeldungen zu erstellen.
- [Erstellen eines Azure AD-Administrators für die verwaltete SQL-Instanz](../database/authentication-aad-configure.md#provision-azure-ad-admin-sql-managed-instance).
- Erfolgreiche Verbindungsherstellung mit der verwalteten SQL-Instanz in Ihrem Netzwerk. Weitere Informationen finden Sie in den folgenden Artikeln:
  - [Herstellen einer Verbindung zwischen einer Anwendung und einer verwalteten Azure SQL-Instanz](connect-application-instance.md)
  - [Schnellstart: Konfigurieren einer Point-to-Site-Verbindung von einem lokalen Computer mit einer verwalteten Azure SQL-Instanz](point-to-site-p2s-configure.md)
  - [Konfigurieren des öffentlichen Endpunkts in der verwalteten Azure SQL-Instanz](public-endpoint-configure.md)

## <a name="t-sql-ddl-syntax"></a>T-SQL-DDL-Syntax

In diesem Abschnitt finden Sie die T-SQL-DDL-Syntax für die Migration von Windows-Benutzern und -Gruppen aus einer SQL Server-Instanz zu einer verwalteten SQL-Instanz mit Azure AD-Authentifizierung.

```sql
-- For individual Windows users with logins
ALTER USER [domainName\userName] WITH LOGIN = [loginName@domainName.com];

--For individual groups with logins
ALTER USER [domainName\groupName] WITH LOGIN=[groupName]
```

## <a name="arguments"></a>Argumente

_domainName_</br>
Gibt den Domänennamen des Benutzers an.

_userName_</br>
Gibt den Namen des in der Datenbank identifizierten Benutzers an.

_= loginName\@domainName.com_</br>
Ordnet einen Benutzer der Azure AD-Anmeldung zu.

_groupName_</br>
Gibt den Namen der in der Datenbank identifizierten Gruppe an.

## <a name="part-1-create-logins-in-sql-server-for-windows-users-and-groups"></a>Teil 1: Erstellen von Anmeldungen in SQL Server für Windows-Benutzer und -Gruppen

> [!IMPORTANT]
> Die folgende Syntax erstellt einen Benutzer und eine Gruppenanmeldung in Ihrer SQL Server-Instanz. Der Benutzer und die Gruppe müssen vor dem Ausführen der folgenden Syntax bereits in Active Directory (AD) vorhanden sein. </br> </br>
> Benutzer: „testUser1“, „testGroupUser“ </br>
> Gruppe: „migration“; „testGroupUser“ muss der Migrationsgruppe in AD angehören.

Im folgenden Beispiel in SQL Server eine Anmeldung für ein Konto namens _testUser1_ unter der Domäne _aadsqlmi_ erstellt.

```sql
-- Sign into SQL Server as a sysadmin or a user that can create logins and databases

use master;  
go

-- Create Windows login
create login [aadsqlmi\testUser1] from windows;
go;

/** Create a Windows group login which contains one user [aadsqlmi\testGroupUser].
testGroupUser will need to be added to the migration group in Active Directory
**/
create login [aadsqlmi\migration] from windows;
go;


-- Check logins were created
select * from sys.server_principals;
go;
```

Erstellen Sie eine Datenbank für diesen Test:

```sql
-- Create a database called [migration]
create database migration
go
```

## <a name="part-2-create-windows-users-and-groups-then-add-roles-and-permissions"></a>Teil 2: Erstellen von Windows-Benutzern und -Gruppen und Hinzufügen von Rollen und Berechtigungen

Verwenden Sie die folgende Syntax, um den Testbenutzer zu erstellen:

```sql
use migration;  
go

-- Create Windows user [aadsqlmi\testUser1] with login
create user [aadsqlmi\testUser1] from login [aadsqlmi\testUser1];
go
```

Überprüfen Sie die Benutzerberechtigungen:

```sql
-- Check the user in the Metadata
select * from sys.database_principals;
go

-- Display the permissions – should only have CONNECT permissions
select user_name(grantee_principal_id), * from sys.database_permissions;
go
```

Erstellen Sie eine Rolle, und weisen Sie ihr Ihren Testbenutzer zu:

```sql
-- Create a role with some permissions and assign the user to the role
create role UserMigrationRole;
go

grant CONNECT, SELECT, View DATABASE STATE, VIEW DEFINITION to UserMigrationRole;
go

alter role UserMigrationRole add member [aadsqlmi\testUser1];
go
```

Verwenden Sie die folgende Abfrage, um Benutzernamen anzuzeigen, die einer bestimmten Rolle zugewiesen sind:

```sql
-- Display user name assigned to a specific role
SELECT DP1.name AS DatabaseRoleName,
   isnull (DP2.name, 'No members') AS DatabaseUserName
 FROM sys.database_role_members AS DRM
 RIGHT OUTER JOIN sys.database_principals AS DP1
   ON DRM.role_principal_id = DP1.principal_id
 LEFT OUTER JOIN sys.database_principals AS DP2
   ON DRM.member_principal_id = DP2.principal_id
WHERE DP1.type = 'R'
ORDER BY DP1.name;
```

Verwenden Sie die folgende Syntax, um eine Gruppe zu erstellen. Fügen Sie die Gruppe anschließend der Rolle `db_owner` hinzu:

```sql
-- Create Windows group
create user [aadsqlmi\migration] from login [aadsqlmi\migration];
go

-- ADD 'db_owner' role to this group
sp_addrolemember 'db_owner', 'aadsqlmi\migration';
go

--Check the db_owner role for 'aadsqlmi\migration' group
select is_rolemember('db_owner', 'aadsqlmi\migration')
go
-- Output  ( 1 means YES)
```

Erstellen Sie eine Testtabelle, und fügen Sie mithilfe der folgenden Syntax einige Daten hinzu:

```sql
-- Create a table and add data
create table test ( a int, b int);
go

insert into test values (1,10)
go

-- Check the table values
select * from test;
go
```

## <a name="part-3-backup-and-restore-the-individual-user-database-to-sql-managed-instance"></a>Teil 3: Sichern und Wiederherstellen der individuellen Benutzerdatenbank in einer verwalteten SQL-Instanz

Erstellen wie im Artikel [Kopieren von Datenbanken durch Sichern und Wiederherstellen](/sql/relational-databases/databases/copy-databases-with-backup-and-restore) beschrieben eine Sicherung der Migrationsdatenbank, oder verwenden Sie die folgende Syntax:

```sql
use master;
go
backup database migration to disk = 'C:\Migration\migration.bak';
go
```

Gehen Sie wie unter [Schnellstart: Wiederherstellen einer Datenbank in einer verwalteten SQL-Instanz](restore-sample-database-quickstart.md).

## <a name="part-4-migrate-users-to-sql-managed-instance"></a>Teil 4: Migrieren von Benutzern zu einer verwalteten SQL-Instanz

Führen Sie den Befehl „ALTER USER“ aus, um die Migration zu einer verwalteten SQL-Instanz abzuschließen.

1. Melden Sie sich bei Ihrer verwalteten SQL-Instanz mit Ihrem Azure AD-Administratorkonto für die verwaltete SQL-Instanz an. Erstellen Sie anschließend mithilfe der folgenden Syntax Ihre Azure AD-Anmeldung in der verwalteten SQL-Instanz. Weitere Informationen finden Sie im [Tutorial: Sicherheit für verwaltete SQL-Instanzen in Azure SQL-Datenbank durch Azure AD-Serverprinzipale (Anmeldungen)](aad-security-configure-tutorial.md).

    ```sql
    use master
    go

    -- Create login for AAD user [testUser1@aadsqlmi.net]
    create login [testUser1@aadsqlmi.net] from external provider
    go

    -- Create login for the Azure AD group [migration]. This group contains one user [testGroupUser@aadsqlmi.net]
    create login [migration] from external provider
    go

    --Check the two new logins
    select * from sys.server_principals
    go
    ```

1. Überprüfen Sie, ob Ihre Migration über die korrekte Datenbank und Tabelle sowie über die korrekten Prinzipale verfügt:

    ```sql
    -- Switch to the database migration that is already restored for MI
    use migration;
    go

    --Check if the restored table test exist and contain a row
    select * from test;
    go

    -- Check that the SQL on-premises Windows user/group exists  
    select * from sys.database_principals;
    go
    -- the old user aadsqlmi\testUser1 should be there
    -- the old group aadsqlmi\migration should be there
    ```

1. Ordnen Sie den lokalen Benutzer mithilfe der ALTER USER-Syntax der Azure AD-Anmeldung zu:

    ```sql
    /** Execute the ALTER USER command to alter the Windows user [aadsqlmi\testUser1]
    to map to the Azure AD user testUser1@aadsqlmi.net
    **/
    alter user [aadsqlmi\testUser1] with login = [testUser1@aadsqlmi.net];
    go

    -- Check the principal
    select * from sys.database_principals;
    go
    -- New user testUser1@aadsqlmi.net should be there instead
    --Check new user permissions  - should only have CONNECT permissions
    select user_name(grantee_principal_id), * from sys.database_permissions;
    go

    -- Check a specific role
    -- Display Db user name assigned to a specific role
    SELECT DP1.name AS DatabaseRoleName,
    isnull (DP2.name, 'No members') AS DatabaseUserName
    FROM sys.database_role_members AS DRM
    RIGHT OUTER JOIN sys.database_principals AS DP1
    ON DRM.role_principal_id = DP1.principal_id
    LEFT OUTER JOIN sys.database_principals AS DP2
    ON DRM.member_principal_id = DP2.principal_id
    WHERE DP1.type = 'R'
    ORDER BY DP1.name;
    ```

1. Ordnen Sie die lokale Gruppe mithilfe der ALTER USER-Syntax der Azure AD Anmeldung zu:

    ```sql
    /** Execute ALTER USER command to alter the Windows group [aadsqlmi\migration]
    to the Azure AD group login [migration]
    **/
    alter user [aadsqlmi\migration] with login = [migration];
    -- old group migration is changed to Azure AD migration group
    go

    -- Check the principal
    select * from sys.database_principals;
    go

    --Check the group permission - should only have CONNECT permissions
    select user_name(grantee_principal_id), * from sys.database_permissions;
    go

    --Check the db_owner role for 'aadsqlmi\migration' user
    select is_rolemember('db_owner', 'migration')
    go
    -- Output 1 means 'YES'
    ```

## <a name="part-5-testing-azure-ad-user-or-group-authentication"></a>Teil 5: Testen der Authentifizierung für Azure AD-Benutzer oder -Gruppen

Testen Sie die Authentifizierung bei der verwalteten SQL-Instanz mithilfe des Benutzers, den Sie zuvor unter Verwendung der ALTER USER-Syntax der Azure AD-Anmeldung zugeordnet haben.

1. Melden Sie sich mit Ihrem Abonnement für die verwalteten Azure SQL-Instanz als `aadsqlmi\testUser1` bei dem virtuellen Verbundcomputer an.
1. Melden Sie sich unter Verwendung von SQL Server Management Studio (SSMS) bei Ihrer verwalteten SQL-Instanz an. Verwenden Sie dabei die integrierte **Active Directory-Authentifizierung**, und stellen Sie eine Verbindung mit der Datenbank `migration` her.
    1. Sie können sich auch mit den Anmeldeinformationen testUser1@aadsqlmi.net und der SSMS-Option **Active Directory: universell mit MFA-Unterstützung** anmelden. In diesem Fall steht allerdings das einmalige Anmelden nicht zur Verfügung, und Sie müssen ein Kennwort eingeben. Für die Anmeldung bei Ihrer verwalteten SQL-Instanz muss kein virtueller Verbundcomputer verwendet werden.
1. Im Rahmen des Rollenmitglieds **SELECT** können Sie Inhalte aus der Tabelle `test` auswählen:

    ```sql
    Select * from test  --  and see one row (1,10)
    ```

Testen Sie die Authentifizierung bei einer verwalteten SQL-Instanz mithilfe eines Mitglieds einer Windows-Gruppe (`migration`). Der Benutzer `aadsqlmi\testGroupUser` muss der Gruppe `migration` vor der Migration hinzugefügt worden sein.

1. Melden Sie sich mit Ihrem Abonnement für die verwalteten Azure SQL-Instanz als `aadsqlmi\testGroupUser` bei dem virtuellen Verbundcomputer an.
1. Stellen Sie unter Verwendung von SSMS mit **integrierter Active Directory-Authentifizierung** eine Verbindung mit dem Server der verwalteten Azure SQL-Instanz und der Datenbank `migration` her.
    1. Sie können sich auch mit den Anmeldeinformationen testGroupUser@aadsqlmi.net und der SSMS-Option **Active Directory: universell mit MFA-Unterstützung** anmelden. In diesem Fall steht allerdings das einmalige Anmelden nicht zur Verfügung, und Sie müssen ein Kennwort eingeben. Für die Anmeldung bei Ihrer verwalteten SQL-Instanz muss kein virtueller Verbundcomputer verwendet werden.
1. Im Rahmen der Rolle `db_owner` können Sie eine neue Tabelle erstellen.

    ```sql
    -- Create table named 'new' with a default schema
    Create table dbo.new ( a int, b int)
    ```

> [!NOTE]
> Aufgrund eines bekannten entwurfsbedingten Problems im Zusammenhang mit Azure SQL-Datenbank tritt der folgende Fehler auf, wenn eine Tabellenerstellungsanweisung als Mitglied einer Gruppe ausgeführt wird: </br> </br>
> `Msg 2760, Level 16, State 1, Line 4
The specified schema name "testGroupUser@aadsqlmi.net" either does not exist or you do not have permission to use it.` </br> </br>
> Erstellen Sie zur Umgehung dieses Problems eine Tabelle mit einem vorhandenen Schema (im obigen Fall: <dbo.new>).

## <a name="next-steps"></a>Nächste Schritte

[Tutorial: Offlinemigration von SQL Server zu einer verwalteten Azure SQL-Instanz unter Verwendung von DMS](../../dms/tutorial-sql-server-to-managed-instance.md?toc=/azure/sql-database/toc.json)
