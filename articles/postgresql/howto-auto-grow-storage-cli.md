---
title: Automatisches Vergrößern des Speichers – Azure-Befehlszeilenschnittstelle – Azure Database for PostgreSQL (Einzelserver)
description: In diesem Artikel wird beschrieben, wie Sie das automatische Vergrößern von Speicher über die Azure-Befehlszeilenschnittstelle in Azure Database for PostgreSQL (Einzelserver) aktivieren.
author: sunilagarwal
ms.author: sunila
ms.service: postgresql
ms.topic: how-to
ms.date: 8/7/2019
ms.custom: devx-track-azurecli
ms.openlocfilehash: d16fe5ef6654ee29c3e345ff0532ed91206d86d3
ms.sourcegitcommit: dddd1596fa368f68861856849fbbbb9ea55cb4c7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107366163"
---
# <a name="auto-grow-azure-database-for-postgresql-storage---single-server-using-the-azure-cli"></a>Automatisches Vergrößern des Azure Database for PostgreSQL-Speichers: Einzelserver über die Azure CLI
In diesem Artikel wird beschrieben, wie Sie die Speichervergrößerung für einen Azure Database for PostgreSQL-Server konfigurieren können, ohne die Workload zu beeinträchtigen.

Der Server, für den die [Speicherbegrenzung erreicht wird](./concepts-pricing-tiers.md#reaching-the-storage-limit), ist auf „Schreibgeschützt“ festgelegt. Wenn die automatische Speichervergrößerung dann für Server mit weniger als 100 GB bereitgestelltem Speicher aktiviert wird, wird die bereitgestellte Speichergröße um 5 GB erhöht, sobald der freie Speicher unter 1 GB oder zehn Prozent des bereitgestellten Speichers sinkt. Bei Servern mit mehr als 100 GB bereitgestelltem Speicher wird die bereitgestellte Speichergröße um 5 % erhöht, sobald der freie Speicherplatz unter 10 GB der bereitgestellten Speichergröße sinkt. Dabei gelten die [hier](./concepts-pricing-tiers.md#storage) beschriebenen Grenzwerte für maximalen Speicher.

## <a name="prerequisites"></a>Voraussetzungen

- Sie benötigen einen [Azure Database for PostgreSQL-Server](quickstart-create-server-database-azure-cli.md).

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

- Für diesen Artikel ist mindestens Version 2.0 der Azure CLI erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert.

## <a name="enable-postgresql-server-storage-auto-grow"></a>Aktivieren der automatischen Speichervergrößerung für den PostgreSQL-Server

Aktivieren Sie die automatische Speichervergrößerung auf einem vorhandenen Server mit dem folgenden Befehl:

```azurecli-interactive
az postgres server update --name mydemoserver --resource-group myresourcegroup --auto-grow Enabled
```

Aktivieren Sie die automatische Speichervergrößerung beim Erstellen eines neuen Servers mit dem folgenden Befehl:

```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mydemoserver  --auto-grow Enabled --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 --version 9.6
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Warnungen zu Metriken erstellen](howto-alert-on-metric.md).
