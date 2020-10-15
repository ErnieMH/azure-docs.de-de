---
title: Wiederherstellen von Azure Cosmos DB-Daten aus einer Sicherung
description: In diesem Artikel wird beschrieben, wie Sie Azure Cosmos DB-Daten aus einer Sicherung wiederherstellen, wie Sie zum Wiederherstellen von Daten den Azure-Support kontaktieren und welche Schritte Sie ausführen, nachdem die Daten wiederhergestellt wurden.
author: kanshiG
ms.service: cosmos-db
ms.topic: how-to
ms.date: 08/24/2020
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 9956ca0ca9c0957557e7ee74883a75c074ff22f8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88797971"
---
# <a name="restore-data-from-a-backup-in-azure-cosmos-db"></a>Wiederherstellen von Daten aus einer Sicherung in Azure Cosmos DB

Falls Sie Ihre Datenbank oder einen Container versehentlich löschen, können Sie ein [Supportticket erstellen](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) oder [sich an den Azure Support wenden](https://azure.microsoft.com/support/options/), um die Daten aus automatischen Onlinesicherungen wiederherstellen zu lassen. Der Azure-Support steht nur in ausgewählten Tarifen wie **Standard** und **Developer** sowie in höheren Tarifen zur Verfügung. Für den **Basic**-Tarif ist kein Azure-Support verfügbar. Weitere Informationen zu anderen Supportplänen finden Sie auf der Seite [Azure-Supportpläne](https://azure.microsoft.com/support/plans/).

Für die Wiederherstellung einer bestimmten Momentaufnahme der Sicherung setzt Azure Cosmos DB voraus, dass die Daten für die Dauer des Sicherungszyklus dieser Momentaufnahme verfügbar sind.

## <a name="request-a-restore"></a>Anfordern einer Wiederherstellung

Sie benötigen für das Anfordern einer Wiederherstellung die folgenden Informationen:

* Halten Sie Ihre Abonnement-ID bereit.

* Basierend darauf, wie Ihre Daten versehentlich gelöscht oder geändert wurden, sollten Sie ggf. weitere Informationen bereithalten. Es wird empfohlen, die Informationen vorab zusammenzustellen, um die Kommunikation zu minimieren, die sich bei zeitkritischen Fällen nachteilig auswirken kann.

* Wenn das gesamte Azure Cosmos DB-Konto gelöscht wurde, müssen Sie den Namen des gelöschten Kontos angeben. Wenn Sie ein anderes Konto mit dem gleichen Namen wie das gelöschte Konto erstellen, geben Sie dies dem Supportteam an, da es dabei hilft, das richtige Konto auszuwählen. Es wird empfohlen, für jedes gelöschte Konto unterschiedliche Supporttickets zu erstellen, weil dadurch Irritationen im Hinblick auf den Status der Wiederherstellung minimiert werden.

* Wenn Datenbanken gelöscht wurden, sollten Sie das Azure Cosmos-Konto sowie die Namen der Azure Cosmos-Datenbanken angeben und ggf. darüber informieren, wenn eine neue Datenbank mit demselben Namen vorhanden ist.

* Wenn Container gelöscht wurden, sollten Sie den Namen des Azure Cosmos-Kontos, die Datenbanknamen und die Containernamen bereitstellen. Geben Sie auch an, ob ein Container mit demselben Namen vorhanden ist.

* Wenn Sie Ihre Daten versehentlich gelöscht oder beschädigt haben, sollten Sie sich innerhalb von 8 Stunden an den [Azure-Support](https://azure.microsoft.com/support/options/) wenden, damit das Azure Cosmos DB-Team Sie beim Wiederherstellen der Daten aus den Sicherungen unterstützen kann. **Bevor Sie eine Supportanfrage zum Wiederherstellen der Daten erstellen, müssen Sie [die Sicherungsaufbewahrung](online-backup-and-restore.md) für Ihr Konto auf mindestens sieben Tage erhöhen. Am besten ist es, den Aufbewahrungszeitraum innerhalb von 8 Stunden nach diesem Ereignis zu erhöhen.** Auf diese Weise hat das Supportteam von Azure Cosmos DB genug Zeit zum Wiederherstellen Ihres Kontos.

Zusätzlich zum Namen des Azure Cosmos-Kontos, den Datenbanknamen und den Containernamen sollten Sie den Zeitpunkt angeben, zu dem die Daten wiederhergestellt werden können. Es ist wichtig, dabei so präzise wie möglich zu sein, damit wir die besten verfügbaren Sicherungen für diesen Zeitpunkt bestimmen können. **Es ist auch wichtig, die Uhrzeit in UTC anzugeben.**

Der folgende Screenshot veranschaulicht das Erstellen einer Supportanfrage für einen Container (Sammlung/Graph/Tabelle) zum Wiederherstellen von Daten mithilfe des Azure-Portals. Geben Sie zusätzliche Details wie z.B. den Datentyp, den Zweck der Wiederherstellung und den Zeitpunkt der Datenlöschung an, damit wir die Anforderung priorisieren können.

:::image type="content" source="./media/how-to-backup-and-restore/backup-support-request-portal.png" alt-text="Erstellen einer Supportanforderung mithilfe des Azure-Portals":::

## <a name="post-restore-actions"></a>Aktionen nach der Wiederherstellung

Nachdem die Daten wiederhergestellt wurden, erhalten Sie eine Benachrichtigung über den Namen des neuen Kontos (in der Regel im Format `<original-name>-restored1`) und der Uhrzeit, zu der das Konto wiederhergestellt wurde. Das wiederhergestellte Konto verfügt den gleichen bereitgestellten Durchsatz und dieselben Richtlinien für die Indizierung, und es befindet sich in derselben Region wie das ursprüngliche Konto. Ein Benutzer, der Abonnementadministrator oder ein Co-Administrator ist, kann das wiederhergestellte Konto anzeigen.

Nach dem Wiederherstellen der Daten sollten Sie diese im wiederhergestellten Konto untersuchen und überprüfen und sicherstellen, dass die gewünschte Version enthalten ist. Wenn alles gut aussieht, sollten Sie die Daten mithilfe des [Azure Cosmos DB-Änderungsfeeds](change-feed.md) oder von [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) zum ursprünglichen Konto migrieren.

Es wird empfohlen, den Container oder die Datenbank sofort nach der Migration zu löschen. Wenn Sie die wiederhergestellten Datenbanken oder Container nicht löschen, entstehen Kosten für Anforderungseinheiten, Speicherung und Erfassung.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich als Nächstes in den folgenden Artikeln über das Migrieren der Daten zum ursprünglichen Konto:

* Wenn Sie eine Wiederherstellungsanforderung übermitteln möchten, kontaktieren Sie den Azure-Support, und [erstellen Sie im Azure-Portal ein Ticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
* [Verwenden des Änderungsfeeds von Cosmos DB](change-feed.md) zum Verschieben von Daten in Azure Cosmos DB

* [Verwenden von Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) zum Verschieben von Daten in Azure Cosmos DB
