---
title: Verwenden von VNET-Regeln – Azure-Befehlszeilenschnittstelle – Azure Database for PostgreSQL (Einzelserver)
description: In diesem Artikel wird beschrieben, wie Sie mithilfe der Azure CLI-Befehlszeile VNET-Dienstendpunkte und -Regeln für Azure Database for PostgreSQL erstellen und verwalten.
author: niklarin
ms.author: nlarin
ms.service: postgresql
ms.devlang: azurecli
ms.topic: how-to
ms.date: 5/6/2019
ms.custom: devx-track-azurecli
ms.openlocfilehash: 4bb65732a1f2667ded017077921f151e740e2c1a
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2020
ms.locfileid: "92489879"
---
# <a name="create-and-manage-vnet-service-endpoints-for-azure-database-for-postgresql---single-server-using-azure-cli"></a>Erstellen und Verwalten von VNET-Dienstendpunkten für Azure Database for PostgreSQL (Einzelserver) mithilfe der Azure-Befehlszeilenschnittstelle
VNet-Dienstendpunkte und -regeln (Virtual Network) erweitern den privaten Adressraum eines virtuellen Netzwerks auf Ihren Azure Database for PostgreSQL-Server. Mit den praktischen Befehlen der Azure-Befehlszeilenschnittstelle (Azure CLI) können Sie VNET-Dienstpunkte und -Regeln erstellen, aktualisieren, löschen, auflisten und anzeigen, um Ihren Server zu verwalten. Eine Übersicht über die VNet-Dienstendpunkte für Azure Database for PostgreSQL, einschließlich Einschränkungen, finden Sie unter [Azure Database for PostgreSQL Server VNet service endpoints (VNet-Dienstendpunkte für Azure Database for PostgreSQL Server)](concepts-data-access-and-security-vnet.md). VNET-Dienstendpunkte sind in allen unterstützten Regionen für Azure Database for PostgreSQL verfügbar.

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- Installieren Sie die [Azure CLI](/cli/azure/install-azure-cli) oder verwenden Sie Azure Cloud Shell im Browser.
- Einen [Server und eine Datenbank für Azure Database for PostgreSQL](quickstart-create-server-database-azure-cli.md).

> [!NOTE]
> VNET-Dienstendpunkte werden nur für Server vom Typ „Universell“ und „Arbeitsspeicheroptimiert“ unterstützt.
> Wenn beim VNET-Peering der Datenverkehr über eine gemeinsame VNet Gateway-Instanz mit Dienstendpunkten fließt und an den Peer fließen soll, erstellen Sie eine ACL/VNET-Regel, damit Azure Virtual Machines im Gateway-VNET auf den Azure Database for PostgreSQL-Server zugreifen kann.


## <a name="configure-vnet-service-endpoints-for-azure-database-for-postgresql"></a>Konfigurieren von Vnet-Dienstendpunkten für Azure Database for PostgreSQL
Die [az network vnet](/cli/azure/network/vnet)-Befehle werden zum Konfigurieren von virtuellen Netzwerken verwendet.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Artikel die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie den Befehl `az --version` aus, um die installierte Version anzuzeigen. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli). 

Wenn Sie die CLI lokal ausführen, müssen Sie sich mit dem Befehl [az login](/cli/azure/authenticate-azure-cli) an Ihrem Konto anmelden. Beachten Sie die Eigenschaft **id** aus der Befehlsausgabe für den entsprechenden Abonnementnamen.
```azurecli-interactive
az login
```

Wenn Sie über mehrere Abonnements verfügen, wählen Sie das entsprechende Abonnement aus, in dem die Ressource fakturiert sein sollte. Wählen Sie mithilfe des Befehls [az account set](/cli/azure/account#az-account-set) die Abonnement-ID unter Ihrem Konto aus. Ersetzen Sie den Platzhalter für die Abonnement-ID durch die Eigenschaft **id** aus der Ausgabe von **az login** für Ihr Abonnement.

- Das Konto muss über die erforderlichen Berechtigungen zum Erstellen eines virtuellen Netzwerks und eines Dienstendpunkts verfügen.

Dienstendpunkte können unabhängig für virtuelle Netzwerke konfiguriert werden, der Benutzer benötigt hierzu Schreibzugriff auf das virtuelle Netzwerk.

Um Azure-Dienstressourcen in einem VNET zu sichern, muss der Benutzer für die hinzuzufügenden Subnetze über die Berechtigung für „Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/“ verfügen. Diese Berechtigung ist standardmäßig in die integrierten Dienstadministratorrollen integriert und kann durch die Erstellung von benutzerdefinierten Rollen geändert werden.

Erfahren Sie mehr über [integrierte Rollen](../role-based-access-control/built-in-roles.md) und das Zuweisen bestimmter Berechtigungen zu [benutzerdefinierten Rollen](../role-based-access-control/custom-roles.md).

VNETs und Ressourcen von Azure-Diensten können sich in demselben oder in unterschiedlichen Abonnements befinden. Wenn das VNET und die Ressourcen von Azure-Diensten in unterschiedlichen Abonnements enthalten sind, sollten sich die Ressourcen unter demselben Active Directory-Mandanten (AD) befinden. Stellen Sie sicher, dass für beide Abonnements der Ressourcenanbieter **Microsoft.Sql** registriert ist. Weitere Informationen finden Sie unter [Azure-Ressourcenanbieter und -typen][resource-manager-portal].

> [!IMPORTANT]
> Es wird dringend empfohlen, diesen Artikel zu Dienstendpunktkonfigurationen und -überlegungen zu lesen, bevor Sie das unten aufgeführte Beispielskript ausführen oder Dienstendpunkte konfigurieren. **Virtual Network-Dienstendpunkt:** Ein [VNET-Dienstendpunkt](../virtual-network/virtual-network-service-endpoints-overview.md) ist ein Subnetz, dessen Eigenschaftswerte mindestens einen formalen Azure-Diensttypnamen enthalten. VNET-Dienstendpunkte verwenden den Diensttypnamen **Microsoft.Sql** , der auf den Azure-Dienst „SQL-Datenbank“ verweist. Dieses Diensttag gilt auch für die Dienste Azure SQL-Datenbank, Azure Database for PostgreSQL und Azure Database for MySQL. Beim Anwenden des Diensttags **Microsoft.Sql** auf einen VNET-Dienstendpunkt muss beachtet werden, dass auf diese Weise der Dienstendpunkt-Datenverkehr für alle Azure-Datenbankdienste konfiguriert wird. Dies schließt Azure SQL-Datenbank-, Azure Database for PostgreSQL- und Azure Database for MySQL-Server im Subnetz ein. 
> 

### <a name="sample-script-to-create-an-azure-database-for-postgresql-database-create-a-vnet-vnet-service-endpoint-and-secure-the-server-to-the-subnet-with-a-vnet-rule"></a>Beispielskript für die Erstellung einer Azure Database for PostgreSQL-Datenbank, zur Erstellung eines VNets, eines VNet-Dienstendpunkts und für die Sicherung des Servers mit einer VNet-Regel im Subnetz
Ändern Sie in diesem Beispielskript die hervorgehobenen Zeilen, um den Benutzernamen und das Kennwort des Administrators anzupassen. Ersetzen Sie die SubscriptionID im Befehl `az account set --subscription` durch Ihre eigene Abonnement-ID.
[!code-azurecli-interactive[main](../../cli_scripts/postgresql/create-postgresql-server-vnet/create-postgresql-server.sh?highlight=5,20 "Create an Azure Database for PostgreSQL, VNet, VNet service endpoint, and VNet rule.")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung
Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.
[!code-azurecli-interactive[main](../../cli_scripts/postgresql/create-postgresql-server-vnet/delete-postgresql.sh "Delete the resource group.")]

<!-- Link references, to text, Within this same GitHub repo. --> 
[resource-manager-portal]: ../azure-resource-manager/management/resource-providers-and-types.md