---
title: 'Schnellstart: Herstellen einer Verbindung unter Verwendung von PHP: Azure Database for MySQL'
description: Dieser Schnellstart enthält mehrere PHP-Codebeispiele, die Sie nutzen können, um zu den Daten von Azure-Datenbank für MySQL eine Verbindung herzustellen und Abfragen dafür durchzuführen.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.custom: mvc
ms.topic: quickstart
ms.date: 5/26/2020
ms.openlocfilehash: ec406208f862eac2450cc6352f13f3596a7c9775
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93337386"
---
# <a name="quickstart-use-php-to-connect-and-query-data-in-azure-database-for-mysql"></a>Schnellstart: Verwenden von PHP zum Herstellen einer Verbindung und zum Abfragen von Daten in Azure Database for MySQL
Dieser Schnellstart zeigt, wie Sie mit einer [PHP](https://secure.php.net/manual/intro-whatis.php)-Anwendung eine Verbindung mit einer Azure-Datenbank für MySQL herstellen. Es wird veranschaulicht, wie Sie SQL-Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank verwenden. Bei den Schritten in diesem Artikel wird davon ausgegangen, dass Sie mit der PHP-Entwicklung vertraut sind und noch keine Erfahrung mit Azure Database for MySQL haben.

## <a name="prerequisites"></a>Voraussetzungen
In diesem Schnellstart werden die Ressourcen, die in den folgenden Anleitungen erstellt wurden, als Startpunkt verwendet:
- [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe des Azure-Portals](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Erstellen eines Servers für Azure-Datenbank für MySQL mithilfe der Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

> [!IMPORTANT] 
> Sicherstellen, dass der IP-Adresse, über die Sie eine Verbindung herstellen, die Firewallregeln des Servers über das [Azure-Portal](./howto-manage-firewall-using-portal.md) oder die [Azure CLI](./howto-manage-firewall-using-cli.md) hinzugefügt wurden

## <a name="install-php"></a>Installieren von PHP
Installieren Sie PHP auf Ihrem eigenen Server, oder erstellen Sie eine Azure-[Web-App](../app-service/overview.md), die PHP enthält.

### <a name="macos"></a>macOS
- Laden Sie die [PHP-Version 7.1.4](https://secure.php.net/downloads.php) herunter.
- Installieren Sie PHP, und informieren Sie sich im [PHP-Handbuch](https://secure.php.net/manual/install.macosx.php) über die weitere Konfiguration.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
- Laden Sie die [PHP-Version 7.1.4 non-thread safe (x64)](https://secure.php.net/downloads.php) herunter.
- Installieren Sie PHP, und informieren Sie sich im [PHP-Handbuch](https://secure.php.net/manual/install.unix.php) über die weitere Konfiguration.

### <a name="windows"></a>Windows
- Laden Sie die [PHP-Version 7.1.4 non-thread safe (x64)](https://windows.php.net/download#php-7.1) herunter.
- Installieren Sie PHP, und informieren Sie sich im [PHP-Handbuch](https://secure.php.net/manual/install.windows.php) über die weitere Konfiguration.

## <a name="get-connection-information"></a>Abrufen von Verbindungsinformationen
Rufen Sie die Verbindungsinformationen ab, die zum Herstellen einer Verbindung mit der Azure SQL-Datenbank für MySQL erforderlich sind. Sie benötigen den vollqualifizierten Servernamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Azure-Portal im linken Menü auf **Alle Ressourcen** , und suchen Sie dann nach dem soeben erstellten Server, z.B. **mydemoserver**.
3. Klicken Sie auf den Servernamen.
4. Notieren Sie sich im Bereich **Übersicht** des Servers den **Servernamen** und den **Anmeldenamen des Serveradministrators**. Wenn Sie Ihr Kennwort vergessen haben, können Sie es in diesem Bereich auch zurücksetzen.
 :::image type="content" source="./media/connect-php/1_server-overview-name-login.png" alt-text="Servername für Azure-Datenbank für MySQL":::

## <a name="connect-and-create-a-table"></a>Herstellen einer Verbindung und Erstellen einer Tabelle
Verwenden Sie den folgenden Code, um mit der SQL-Anweisung des Typs **CREATE TABLE** eine Verbindung herzustellen und eine Tabelle zu erstellen. 

Im Code wird die Klasse mit der **verbesserten MySQL-Erweiterung** (mysqli) verwendet, die in PHP enthalten ist. Im Code werden die Methoden [mysqli_init](https://secure.php.net/manual/mysqli.init.php) und [mysqli_real_connect](https://secure.php.net/manual/mysqli.real-connect.php) aufgerufen, um eine Verbindung mit MySQL herzustellen. Anschließend wird die [mysqli_query](https://secure.php.net/manual/mysqli.query.php)-Methode aufgerufen, um die Abfrage auszuführen. Als Nächstes wird die [mysqli_close](https://secure.php.net/manual/mysqli.close.php)-Methode aufgerufen, um die Verbindung zu schließen.

Ersetzen Sie die Parameter „host“, „username“, „password“ und „db_name“ durch Ihre eigenen Werte. 

```php
<?php
$host = 'mydemoserver.mysql.database.azure.com';
$username = 'myadmin@mydemoserver';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

// Run the create table query
if (mysqli_query($conn, '
CREATE TABLE Products (
`Id` INT NOT NULL AUTO_INCREMENT ,
`ProductName` VARCHAR(200) NOT NULL ,
`Color` VARCHAR(50) NOT NULL ,
`Price` DOUBLE NOT NULL ,
PRIMARY KEY (`Id`)
);
')) {
printf("Table created\n");
}

//Close the connection
mysqli_close($conn);
?>
```

## <a name="insert-data"></a>Einfügen von Daten
Verwenden Sie den folgenden Code, um eine Verbindung herzustellen und mit einer SQL-Anweisung des Typs **INSERT** Daten einzufügen.

Im Code wird die Klasse mit der **verbesserten MySQL-Erweiterung** (mysqli) verwendet, die in PHP enthalten ist. Im Code wird die [mysqli_prepare](https://secure.php.net/manual/mysqli.prepare.php)-Methode verwendet, um eine vorbereitete INSERT-Anweisung zu erstellen, und anschließend werden die Parameter für jeden eingefügten Spaltenwert mit der [mysqli_stmt_bind_param](https://secure.php.net/manual/mysqli-stmt.bind-param.php)-Methode gebunden. Im Code wird die Anweisung mit der [mysqli_stmt_execute](https://secure.php.net/manual/mysqli-stmt.execute.php)-Methode ausgeführt, und anschließend wird die Anweisung mit der [mysqli_stmt_close](https://secure.php.net/manual/mysqli-stmt.close.php)-Methode geschlossen.

Ersetzen Sie die Parameter „host“, „username“, „password“ und „db_name“ durch Ihre eigenen Werte. 

```php
<?php
$host = 'mydemoserver.mysql.database.azure.com';
$username = 'myadmin@mydemoserver';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Create an Insert prepared statement and run it
$product_name = 'BrandNewProduct';
$product_color = 'Blue';
$product_price = 15.5;
if ($stmt = mysqli_prepare($conn, "INSERT INTO Products (ProductName, Color, Price) VALUES (?, ?, ?)")) {
mysqli_stmt_bind_param($stmt, 'ssd', $product_name, $product_color, $product_price);
mysqli_stmt_execute($stmt);
printf("Insert: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));
mysqli_stmt_close($stmt);
}

// Close the connection
mysqli_close($conn);
?>
```

## <a name="read-data"></a>Lesen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer SQL-Anweisung des Typs **SELECT** zu verbinden und zu lesen.  Im Code wird die Klasse mit der **verbesserten MySQL-Erweiterung** (mysqli) verwendet, die in PHP enthalten ist. Im Code wird die [mysqli_query](https://secure.php.net/manual/mysqli.query.php)-Methode zum Durchführen der SQL-Abfrage genutzt, und mit der [mysqli_fetch_assoc](https://secure.php.net/manual/mysqli-result.fetch-assoc.php)-Methode werden die sich ergebenden Zeilen abgerufen.

Ersetzen Sie die Parameter „host“, „username“, „password“ und „db_name“ durch Ihre eigenen Werte. 

```php
<?php
$host = 'mydemoserver.mysql.database.azure.com';
$username = 'myadmin@mydemoserver';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Select query
printf("Reading data from table: \n");
$res = mysqli_query($conn, 'SELECT * FROM Products');
while ($row = mysqli_fetch_assoc($res)) {
var_dump($row);
}

//Close the connection
mysqli_close($conn);
?>
```

## <a name="update-data"></a>Aktualisieren von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer SQL-Anweisung des Typs **UPDATE** zu verbinden und zu aktualisieren.

Im Code wird die Klasse mit der **verbesserten MySQL-Erweiterung** (mysqli) verwendet, die in PHP enthalten ist. Im Code wird die [mysqli_prepare](https://secure.php.net/manual/mysqli.prepare.php)-Methode verwendet, um eine vorbereitete UPDATE-Anweisung zu erstellen, und anschließend werden die Parameter für jeden aktualisierten Spaltenwert mit der [mysqli_stmt_bind_param](https://secure.php.net/manual/mysqli-stmt.bind-param.php)-Methode gebunden. Im Code wird die Anweisung mit der [mysqli_stmt_execute](https://secure.php.net/manual/mysqli-stmt.execute.php)-Methode ausgeführt, und anschließend wird die Anweisung mit der [mysqli_stmt_close](https://secure.php.net/manual/mysqli-stmt.close.php)-Methode geschlossen.

Ersetzen Sie die Parameter „host“, „username“, „password“ und „db_name“ durch Ihre eigenen Werte. 

```php
<?php
$host = 'mydemoserver.mysql.database.azure.com';
$username = 'myadmin@mydemoserver';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Update statement
$product_name = 'BrandNewProduct';
$new_product_price = 15.1;
if ($stmt = mysqli_prepare($conn, "UPDATE Products SET Price = ? WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 'ds', $new_product_price, $product_name);
mysqli_stmt_execute($stmt);
printf("Update: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));

//Close the connection
mysqli_stmt_close($stmt);
}

mysqli_close($conn);
?>
```


## <a name="delete-data"></a>Löschen von Daten
Verwenden Sie den folgenden Code, um die Daten mit einer SQL-Anweisung des Typs **DELETE** zu verbinden und zu lesen. 

Im Code wird die Klasse mit der **verbesserten MySQL-Erweiterung** (mysqli) verwendet, die in PHP enthalten ist. Im Code wird die [mysqli_prepare](https://secure.php.net/manual/mysqli.prepare.php)-Methode verwendet, um eine vorbereitete DELETE-Anweisung zu erstellen, und anschließend werden die Parameter für die WHERE-Klausel in der Anweisung mit der [mysqli_stmt_bind_param](https://secure.php.net/manual/mysqli-stmt.bind-param.php)-Methode gebunden. Im Code wird die Anweisung mit der [mysqli_stmt_execute](https://secure.php.net/manual/mysqli-stmt.execute.php)-Methode ausgeführt, und anschließend wird die Anweisung mit der [mysqli_stmt_close](https://secure.php.net/manual/mysqli-stmt.close.php)-Methode geschlossen.

Ersetzen Sie die Parameter „host“, „username“, „password“ und „db_name“ durch Ihre eigenen Werte. 

```php
<?php
$host = 'mydemoserver.mysql.database.azure.com';
$username = 'myadmin@mydemoserver';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Delete statement
$product_name = 'BrandNewProduct';
if ($stmt = mysqli_prepare($conn, "DELETE FROM Products WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 's', $product_name);
mysqli_stmt_execute($stmt);
printf("Delete: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));
mysqli_stmt_close($stmt);
}

//Close the connection
mysqli_close($conn);
?>
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe mit dem folgenden Befehl, um alle in dieser Schnellstartanleitung verwendeten Ressourcen zu bereinigen:

```azurecli
az group delete \
    --name $AZ_RESOURCE_GROUP \
    --yes
```

## <a name="next-steps"></a>Nächste Schritte
> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit Azure-Datenbank für MySQL über SSL](howto-configure-ssl.md)
