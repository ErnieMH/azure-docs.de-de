---
title: Verwalten des Zugriffs auf Arbeitsbereiche, Daten und Pipelines
description: Hier erfahren Sie, wie Sie die Zugriffssteuerung für Arbeitsbereiche, Daten und Pipelines in Azure Synapse Analytics verwalten.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 04/15/2020
ms.author: stefanazaric
ms.reviewer: jrasnick
ms.openlocfilehash: c4304aeadf2950c1a91ee50ba9ecd895b2561b41
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2020
ms.locfileid: "96461347"
---
# <a name="manage-access-to-workspaces-data-and-pipelines"></a>Verwalten des Zugriffs auf Arbeitsbereiche, Daten und Pipelines

Hier erfahren Sie, wie Sie die Zugriffssteuerung für Arbeitsbereiche, Daten und Pipelines in Azure Synapse Analytics verwalten.

> [!NOTE]
> In der allgemein verfügbaren Version werden zur Weiterentwicklung der rollenbasierten Zugriffssteuerung (Role-Based Access Control, Azure RBAC) Synapse-spezifische Azure-Rollen eingeführt.

## <a name="access-control-for-workspace"></a>Access Control für den Arbeitsbereich

Bei einer Produktionsbereitstellung in einem Azure Synapse-Arbeitsbereich empfiehlt es sich, die Umgebung zu strukturieren, um die Bereitstellung von Benutzern und Administratoren zu erleichtern.

> [!NOTE]
> Der hier verfolgte Ansatz besteht darin, mehrere Sicherheitsgruppen zu erstellen und anschließend den Arbeitsbereich für eine konsistente Verwendung zu konfigurieren. Nach Einrichtung der Gruppen muss ein Administrator nur noch die Mitgliedschaft innerhalb der Sicherheitsgruppen verwalten.

### <a name="step-1-set-up-security-groups-with-names-following-this-pattern"></a>Schritt 1: Einrichten von Sicherheitsgruppen mit Namen nach dem folgenden Muster

1. Erstellen Sie eine Sicherheitsgruppe namens `Synapse_WORKSPACENAME_Users`.
2. Erstellen Sie eine Sicherheitsgruppe namens `Synapse_WORKSPACENAME_Admins`.
3. `Synapse_WORKSPACENAME_Admins` zu `Synapse_WORKSPACENAME_Users` hinzugefügt

> [!NOTE]
> Informationen zum Erstellen einer Sicherheitsgruppe finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal).
>
> Informationen zum Hinzufügen einer Sicherheitsgruppe über eine andere Sicherheitsgruppe finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-membership-azure-portal).
>
> WORKSPACENAME – Sie sollten diesen Teil durch Ihren tatsächlichen Arbeitsbereichsnamen ersetzen.

### <a name="step-2-prepare-the-default-adls-gen2-account"></a>Schritt 2: Vorbereiten des ADLS Gen2-Standardkontos

Beim Bereitstellen Ihres Arbeitsbereichs mussten Sie ein [Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction)-Konto und einen Container für das Dateisystem auswählen, damit der Arbeitsbereich verwendet werden konnte.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Navigieren Sie zum Azure Data Lake Storage Gen2-Konto.
3. Navigieren Sie zu dem Container (Dateisystem), den Sie für den Azure Synapse-Arbeitsbereich ausgewählt haben.
4. Wählen Sie die Option **Zugriffssteuerung (IAM)** aus.
5. Weisen Sie die folgenden Rollen zu:
   1. **Leser** für: `Synapse_WORKSPACENAME_Users`
   2. **Besitzer von Speicherblobdaten** für: `Synapse_WORKSPACENAME_Admins`
   3. **Mitwirkender an Speicherblobdaten** für: `Synapse_WORKSPACENAME_Users`
   4. **Besitzer von Speicherblobdaten** für: `WORKSPACENAME`

> [!NOTE]
> WORKSPACENAME – Sie sollten diesen Teil durch Ihren tatsächlichen Arbeitsbereichsnamen ersetzen.

### <a name="step-3-configure-the-workspace-admin-list"></a>Schritt 3: Konfigurieren der Arbeitsbereichsadministratorliste

1. Navigieren Sie zur [**Webbenutzeroberfläche von Azure Synapse**](https://web.azuresynapse.net).
2. Navigieren Sie zu **Verwalten**  > **Sicherheit** > **Zugriffssteuerung**.
3. Wählen Sie **Administrator hinzufügen** und dann `Synapse_WORKSPACENAME_Admins` aus.

### <a name="step-4-configure-sql-admin-access-for-the-workspace"></a>Schritt 4: Konfigurieren des SQL-Administratorzugriffs für den Arbeitsbereich

1. [Navigieren Sie zum Azure-Portal](https://portal.azure.com).
2. Navigieren Sie zu Ihrem Arbeitsbereich.
3. Navigieren Sie zu **Einstellungen** > **Active Directory-Administrator**.
4. Wählen Sie **Administrator festlegen** aus.
5. `Synapse_WORKSPACENAME_Admins` auswählen
6. Wählen Sie **Auswählen** aus.
7. Wählen Sie **Speichern** aus.

> [!NOTE]
> WORKSPACENAME – Sie sollten diesen Teil durch Ihren tatsächlichen Arbeitsbereichsnamen ersetzen.

### <a name="step-5-add-and-remove-users-and-admins-to-security-groups"></a>Schritt 5: Hinzufügen und Entfernen von Benutzern und Administratoren zu bzw. aus Sicherheitsgruppen

1. Fügen Sie Benutzer, die Administratorzugriff benötigen, `Synapse_WORKSPACENAME_Admins` hinzu.
2. Fügen Sie alle anderen Benutzer `Synapse_WORKSPACENAME_Users` hinzu.

> [!NOTE]
> Informationen zum Hinzufügen eines Benutzers als Mitglied zu einer Sicherheitsgruppe finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-members-azure-portal)
> 
> WORKSPACENAME – Sie sollten diesen Teil durch Ihren tatsächlichen Arbeitsbereichsnamen ersetzen.

## <a name="access-control-to-data"></a>Zugriffssteuerung für Daten

Die Zugriffssteuerung für die zugrunde liegenden Daten ist in drei Bereiche unterteilt:

- Datenebenenzugriff auf das Speicherkonto (wurde bereits oben in Schritt 2 konfiguriert)
- Datenebenenzugriff auf die SQL-Datenbanken (sowohl für dedizierte SQL-Pools als auch für den serverlosen SQL-Pool)
- Erstellen von Anmeldeinformationen für Datenbanken im serverlosen SQL-Pool über das Speicherkonto

## <a name="access-control-to-sql-databases"></a>Zugriffssteuerung für SQL-Datenbanken

> [!TIP]
> Die folgenden Schritte müssen für **jede** SQL-Datenbank ausgeführt werden, um dem Benutzer Zugriff auf alle SQL-Datenbanken zu gewähren, außer in Abschnitt [Berechtigung auf Serverebene](#server-level-permission), wo Sie dem Benutzer eine Systemadministratorrolle zuweisen können.

### <a name="serverless-sql-pool"></a>Serverloser SQL-Pool

In diesem Abschnitt finden Sie Beispiele dafür, wie Sie Benutzern eine Berechtigung für eine bestimmte Datenbank oder uneingeschränkte Berechtigungen für einen Server erteilen können.

#### <a name="database-level-permission"></a>Berechtigung auf Datenbankebene

Führen Sie die Schritte des folgenden Beispiels aus, um einem Benutzer Zugriff auf eine **einzelne** Datenbank im serverlosen SQL-Pool zu gewähren:

1. Erstellen Sie eine Anmeldung:

    ```sql
    use master
    go
    CREATE LOGIN [alias@domain.com] FROM EXTERNAL PROVIDER;
    go
    ```

2. Erstellen Sie einen Benutzer:

    ```sql
    use yourdb -- Use your DB name
    go
    CREATE USER alias FROM LOGIN [alias@domain.com];
    ```

3. Fügen Sie den Benutzer den Mitgliedern der angegebenen Rolle hinzu:

    ```sql
    use yourdb -- Use your DB name
    go
    alter role db_owner Add member alias -- Type USER name from step 2
    ```

> [!NOTE]
> Ersetzen Sie den Alias durch den Alias des Benutzers, dem Sie Zugriff gewähren möchten, und die Domäne durch die zu verwendende Unternehmensdomäne.

#### <a name="server-level-permission"></a>Berechtigung auf Serverebene

Führen Sie den Schritt des folgenden Beispiels aus, um einem Benutzer Vollzugriff auf **alle** Datenbanken im serverlosen SQL-Pool zu gewähren:

```sql
CREATE LOGIN [alias@domain.com] FROM EXTERNAL PROVIDER;
ALTER SERVER ROLE  sysadmin  ADD MEMBER [alias@domain.com];
```

### <a name="dedicated-sql-pool"></a>Dedizierter SQL-Pool

Führen Sie die folgenden Schritte aus, um einem Benutzer Zugriff auf eine **einzelne** SQL-Datenbank zu gewähren:

1. Erstellen Sie den Benutzer in der Datenbank, indem Sie den folgenden Befehl ausführen, und verwenden Sie dabei die gewünschte Datenbank als Ziel in der Kontextauswahl (Dropdownliste zum Auswählen von Datenbanken):

    ```sql
    --Create user in SQL DB
    CREATE USER [<alias@domain.com>] FROM EXTERNAL PROVIDER;
    ```

2. Weisen Sie dem Benutzer eine Rolle für den Zugriff auf die Datenbank zu:

    ```sql
    --Create user in SQL DB
    EXEC sp_addrolemember 'db_owner', '<alias@domain.com>';
    ```

> [!IMPORTANT]
> Wenn Sie die Berechtigung *db_owner* nicht zuweisen möchten, können Sie *db_datareader* und *db_datawriter* für Lese-/Schreibberechtigungen verwenden.
> Damit ein Spark-Benutzer Daten direkt über Spark aus einem dedizierten SQL-Pool lesen bzw. in einen dedizierten SQL-Pool schreiben kann, benötigt er die Berechtigung *db_owner*.

Vergewissern Sie sich im Anschluss an die Benutzererstellung, dass das Speicherkonto von einem serverlosen SQL-Pool abgefragt werden kann.

## <a name="access-control-to-workspace-pipeline-runs"></a>Zugriffssteuerung für Pipelineausführungen im Arbeitsbereich

### <a name="workspace-managed-identity"></a>Vom Arbeitsbereich verwaltete Identität

> [!IMPORTANT]
> Zur erfolgreichen Ausführung von Pipelines mit Datasets oder Aktivitäten, die auf einen dedizierten SQL-Pool verweisen, muss der Arbeitsbereichsidentität direkter Zugriff auf den SQL-Pool gewährt werden.

Führen Sie die folgenden Befehle für jeden dedizierten SQL-Pool aus, um der vom Arbeitsbereich verwalteten Identität das Ausführen von Pipelines für die SQL-Pooldatenbank zu ermöglichen:

```sql
--Create user in DB
CREATE USER [<workspacename>] FROM EXTERNAL PROVIDER;

--Granting permission to the identity
GRANT CONTROL ON DATABASE::<SQLpoolname> TO <workspacename>;
```

Wenn Sie diese Berechtigung wieder entfernen möchten, führen Sie das folgende Skript für den gleichen SQL-Pool aus:

```sql
--Revoking permission to the identity
REVOKE CONTROL ON DATABASE::<SQLpoolname> TO <workspacename>;

--Deleting the user in the DB
DROP USER [<workspacename>];
```

## <a name="next-steps"></a>Nächste Schritte

Eine Übersicht über die vom Synapse-Arbeitsplatz verwaltete Identität finden Sie unter [Azure Synapse-Arbeitsbereich – verwaltete Identität](../security/synapse-workspace-managed-identity.md). Weitere Informationen zu Datenbankprinzipalen finden Sie unter [Prinzipale](https://msdn.microsoft.com/library/ms181127.aspx). Weitere Informationen zu Datenbankrollen finden Sie im Artikel zu [Datenbankrollen](https://msdn.microsoft.com/library/ms189121.aspx).
