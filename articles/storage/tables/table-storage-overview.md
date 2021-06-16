---
title: 'Einführung in Table Storage: Objektspeicher in Azure | Microsoft-Dokumentation'
description: Speichern Sie strukturierte Daten mit Azure Table Storage, einem NoSQL-Datenspeicher, in der Cloud.
services: storage
ms.service: storage
author: tamram
ms.author: tamram
ms.devlang: dotnet
ms.topic: overview
ms.date: 05/27/2021
ms.subservice: tables
ms.openlocfilehash: 690afd54cef64f893bf5e68d537de4a47f1a93db
ms.sourcegitcommit: 1b698fb8ceb46e75c2ef9ef8fece697852c0356c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110654950"
---
# <a name="what-is-azure-table-storage-"></a>Was ist Azure Table Storage? 

[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-tip-include.md)]

Beim Dienst Azure Table Storage werden nicht relationale strukturierte Daten (auch als strukturierte NoSQL-Daten bezeichnet) in der Cloud gespeichert, und es ist ein Schlüssel-/Attributspeicher mit einem schemalosen Design vorhanden. Aufgrund der Schemalosigkeit von Table Storage ist es einfach, Ihre Daten an die Entwicklung Ihrer Anwendungen anzupassen. Viele Arten von Anwendungen können schnell und kostengünstig auf Table Storage-Daten zugreifen, und die Kosten liegen in der Regel unter den Kosten herkömmlicher SQL-Lösungen für vergleichbare Datenmengen.

Mit Table Storage können Sie flexible Datasets wie Benutzerdaten für Webanwendungen, Adressbücher, Geräteinformationen und andere Arten von Metadaten speichern, die Ihr Dienst benötigt. Sie können eine beliebige Anzahl von Entitäten in einer Tabelle speichern, und ein Speicherkonto kann eine beliebige Anzahl von Tabellen enthalten (bis zur Speicherkapazitätsgrenze eines Speicherkontos).

[!INCLUDE [storage-table-concepts-include](../../../includes/storage-table-concepts-include.md)]

## <a name="next-steps"></a>Nächste Schritte

* Beim [Microsoft Azure Storage-Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md) handelt es sich um eine kostenlose eigenständige App von Microsoft, über die Sie ganz einfach visuell mit Azure Storage-Daten arbeiten können – unter Windows, MacOS und Linux.

* [Getting Started with Azure Table Storage in .NET](../../cosmos-db/tutorial-develop-table-dotnet.md) (Erste Schritte mit Azure Table Storage in .NET)

* In der Referenzdokumentation für den Tabellenspeicherdienst finden Sie alle Details zu verfügbaren APIs:

    * [Referenz zur Storage-Clientbibliothek für .NET](/dotnet/api/overview/azure/storage)

    * [REST-API-Referenz](/rest/api/storageservices/)