---
title: Anfordern einer Kontingenterhöhung
description: Auf dieser Seite wird beschrieben, wie Sie eine Supportanfrage zum Erhöhen der Kontingente für Azure SQL-Datenbank und Azure SQL Managed Instance erstellen.
services: sql-database
ms.service: sql-db-mi
ms.subservice: service
ms.topic: how-to
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: sstein
ms.date: 06/04/2020
ms.openlocfilehash: 44a37a912c5c7a882d21631b8ce2da2c7ba9c05e
ms.sourcegitcommit: 9514d24118135b6f753d8fc312f4b702a2957780
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/07/2021
ms.locfileid: "97967700"
---
# <a name="request-quota-increases-for-azure-sql-database-and-sql-managed-instance"></a>Anfordern von Kontingenterhöhungen für Azure SQL-Datenbank und SQL Managed Instance
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

In diesem Artikel wird erläutert, wie Sie eine Erhöhung des Kontingents für Azure SQL-Datenbank und Azure SQL Managed Instance anfordern. Außerdem wird erläutert, wie Sie den Abonnementzugriff auf eine Region aktivieren.

## <a name="create-a-new-support-request"></a><a id="newquota"></a> Erstellen einer neuen Supportanfrage

Führen Sie die folgenden Schritte aus, um im Azure-Portal eine neue Supportanfrage für SQL-Datenbank zu erstellen.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im Menü die Option **Hilfe und Support** aus.

   ![Link „Hilfe und Support“](./media/quota-increase-request/help-plus-support.png)

1. Wählen Sie unter **Hilfe und Support** die Option **Neue Supportanfrage** aus.

    ![Erstellen einer neuen Supportanfrage](./media/quota-increase-request/new-support-request.png)

1. Wählen Sie unter **Problemtyp** den Eintrag **Grenzwerte für Dienste und Abonnements (Kontingente)** aus.

   ![Auswählen eines Problemtyps](./media/quota-increase-request/select-quota-issue-type.png)

1. Wählen Sie unter **Abonnement** das Abonnement aus, dessen Kontingent Sie erhöhen möchten.

   ![Auswählen eines Abonnements für eine Kontingenterhöhung](./media/quota-increase-request/select-subscription-support-request.png)

1. Wählen Sie unter **Kontingenttyp** einen der folgenden Kontingenttypen aus:

   - **SQL-Datenbank-** für Kontingente für Singletons und Pools für elastische Datenbanken
   - **Verwaltete SQL-Datenbank-Instanz** für verwaltete Instanzen

   Wählen Sie anschließend **Next: Lösungen >>** .

   ![Auswählen eines Kontingenttyps](./media/quota-increase-request/select-quota-type.png)

1. Wählen Sie im Fenster **Details** die Option **Details eingeben** aus, um zusätzliche Informationen einzugeben.

   ![Link zum Eingeben von Details](./media/quota-increase-request/provide-details-link.png)

Wenn Sie auf **Details eingeben** klicken, wird das Fenster **Kontingentdetails** angezeigt, in dem Sie zusätzliche Informationen hinzufügen können. In den folgenden Abschnitten werden die verschiedenen Optionen für die Kontingenttypen von **SQL-Datenbank** und **Verwaltete SQL-Datenbank-Instanz** beschrieben.

## <a name="sql-database-quota-types"></a><a id="sqldbquota"></a> Kontingenttypen von SQL-Datenbank

In den folgenden Abschnitten werden die Optionen zum Erhöhen der Kontingente für die Kontingenttypen von **SQL-Datenbank** beschrieben:

- Datenbanktransaktionseinheiten (DTUs) pro Server
- Server pro Abonnement
- Regionszugriff für die M-Serie
- Regionszugriff

### <a name="database-transaction-units-dtus-per-server"></a>Datenbanktransaktionseinheiten (DTUs) pro Server

Führen Sie die folgenden Schritte aus, um eine Erhöhung der DTUs pro Server anzufordern.

1. Wählen Sie den Kontingenttyp **Datenbanktransaktionseinheiten (DTUs) pro Server** aus.

1. Wählen Sie in der Liste **Ressourcen** die Zielressource aus.

1. Geben Sie im Feld **Neues Kontingent** den neuen DTU-Grenzwert ein, den Sie anfordern möchten.

   ![DTU-Kontingentdetails](./media/quota-increase-request/quota-details-dtus.png)

Weitere Informationen finden Sie unter [Ressourcenlimits für Singletons, die das DTU-Kaufmodell verwenden](resource-limits-dtu-single-databases.md) und [Ressourcenlimits für Pools für elastische Datenbanken, die das DTU-Kaufmodell verwenden](resource-limits-dtu-elastic-pools.md).

### <a name="servers-per-subscription"></a>Server pro Abonnement

Führen Sie die folgenden Schritte aus, um eine Erhöhung der Anzahl der Server pro Abonnement anzufordern.

1. Wählen Sie den Kontingenttyp **Server pro Abonnement** aus.

1. Wählen Sie in der Liste **Standort** die zu verwendende Azure-Region aus. Das Kontingent gilt pro Abonnement in jeder Region.

1. Geben Sie im Feld **Neues Kontingent** Ihre Anforderung für die maximale Anzahl von Servern in dieser Region ein.

   ![Serverkontingentdetails](./media/quota-increase-request/quota-details-servers.png)

Weitere Informationen finden Sie unter [SQL-Datenbank-Ressourcenlimits und -Ressourcenkontrolle](resource-limits-logical-server.md).

### <a name="enable-subscription-access-to-a-region"></a><a id="region"></a> Aktivieren des Abonnementzugriffs auf eine Region

Einige Angebotstypen sind nicht in allen Regionen verfügbar. Unter Umständen wird beispielsweise folgender Fehler angezeigt:

`Your subscription does not have access to create a server in the selected region. For the latest information about region availability for your subscription, go to aka.ms/sqlcapacity. Please try another region or create a support ticket to request access.`

Wenn Ihr Abonnement Zugriff auf eine bestimmte Region benötigt, wählen Sie die Option **Regionszugriff** aus. Geben Sie in Ihrer Anforderung die Details zu dem Angebot und der SKU an, die Sie für die Region aktivieren möchten. Informationen zu den Angebots- und SKU-Optionen finden Sie unter [Azure SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/single/).

1. Wählen Sie den Kontingenttyp **Regionszugriff** aus.

1. Wählen Sie in der Liste **Standort auswählen** die zu verwendende Azure-Region aus. Das Kontingent gilt pro Abonnement in jeder Region.

1. Geben Sie das **Kaufmodell** und die Details für **Erwartete Nutzung** ein.

   ![Anfordern des Zugriffs auf eine Region](./media/quota-increase-request/quota-request.png)

<!--
### <a id="mseries"></a> Enable M-series access to a region

To enable M-series hardware for a subscription and region, a support request must be opened.

1. Select the **M-series region access** quota type.

1. In the **Select a location** list, select the Azure region to use. The quota is per subscription in each region.


   ![Request M-series region access](./media/quota-increase-request/quota-m-series.png)
-->

## <a name="sql-managed-instance-quota-type"></a><a id="sqlmiquota"></a> Kontingenttyp für SQL Managed Instance

Führen Sie für den Kontingenttyp **SQL Managed Instance** die folgenden Schritte aus:

1. Wählen Sie in der Liste **Region** die Azure-Zielregion aus.

1. Geben Sie die neuen Grenzwerte ein, die Sie für **Subnetz** und **Virtuelle Kerne** anfordern möchten.

   ![Kontingentdetails für SQL Managed Instance](./media/quota-increase-request/quota-details-managed-instance.png)

Weitere Informationen finden Sie unter [Übersicht über Ressourceneinschränkungen für Azure SQL Managed Instance](../managed-instance/resource-limits.md).

## <a name="submit-your-request"></a>Senden der Anforderung

Im letzten Schritt füllen Sie die restlichen Details Ihrer Kontingentanfrage für SQL-Datenbank aus. Wählen Sie anschließend **Next: Bewerten + erstellen>>** aus, und klicken Sie nach dem Überprüfen der Anforderungsdetails auf **Erstellen**, um die Anforderung zu übermitteln.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre Anfrage übermittelt haben, wird sie überprüft. Sie erhalten eine Antwort entsprechend den Informationen, die Sie im Formular angegeben haben.

Weitere Informationen zu anderen Azure-Grenzwerten finden Sie unter [Grenzwerte für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../../azure-resource-manager/management/azure-subscription-service-limits.md).
