---
title: Skalieren des Durchsatzes in Azure Cosmos DB
description: In diesem Artikel wird beschrieben, wie Azure Cosmos DB den Durchsatz in verschiedenen Regionen skaliert, in denen das Azure Cosmos-Konto bereitgestellt wird.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: sngun
ms.reviewer: sngun
ms.openlocfilehash: 94b5b3d2ab1f576f87ead23b389252ec96a20f11
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91397350"
---
# <a name="how-does-azure-cosmos-db-globally-scale-the-provisioned-throughput"></a>Wie skaliert Azure Cosmos DB den bereitgestellten Durchsatz global?

In Azure Cosmos DB wird der bereitgestellte Durchsatz als Anforderungseinheit/Sekunde (RU/s oder die Pluralform RUs) dargestellt. Anforderungseinheiten messen die Kosten für Lese- und Schreibvorgänge für Ihre Cosmos-Container wie in der folgenden Abbildung dargestellt:

:::image type="content" source="./media/scaling-throughput/request-unit-charge-of-read-and-write-operations.png" alt-text="Anforderungseinheiten" border="false":::

Sie können Anforderungseinheiten für einen Cosmos-Container oder eine Cosmos-Datenbanken bereitstellen. In einem Container bereitgestellte Anforderungseinheiten stehen ausschließlich für die Vorgänge zur Verfügung, die in diesem Container ausgeführt werden. In einer Datenbank bereitgestellte Anforderungseinheiten werden für alle Container in dieser Datenbank (ausgenommen Container mit exklusiv zugewiesenen RUs) freigegeben.

Sie können jederzeit die bereitgestellte RU/s erhöhen oder verringern, um den bereitgestellten Durchsatz elastisch zu skalieren. Weitere Informationen finden Sie im Artikel zum [Bereitstellen von Durchsatz](set-throughput.md) und zur elastischen Skalierung von Cosmos-Containern und -Datenbanken. Um den Durchsatz global zu skalieren, können Sie Ihrem Cosmos-Konto jederzeit Regionen hinzufügen oder daraus entfernen. Weitere Informationen finden Sie unter [Verwalten von Datenbankkonten in Azure Cosmos DB](how-to-manage-database-account.md#addremove-regions-from-your-database-account). Die Zuordnung mehrerer Regionen zu einem Cosmos-Konto ist in vielen Szenarien wichtig, um eine geringe Latenz und [Hochverfügbarkeit](high-availability.md) auf der ganzen Welt zu erreichen.

## <a name="how-provisioned-throughput-is-distributed-across-regions"></a>Verteilen des bereitgestellten Durchsatzes über mehrere Regionen

Wenn Sie *R* RUs in einem Cosmos-Container (oder einer Datenbank) bereitstellen, stellt Cosmos DB sicher, dass in *jeder* Region, die mit Ihrem Cosmos-Konto verknüpft ist, *R* RUs verfügbar sind. Jedes Mal, wenn Sie Ihrem Konto eine neue Region hinzufügen, stellt Cosmos DB automatisch *R* RUs in der neu hinzugefügten Region bereit. Die Vorgänge, die für Ihren Cosmos-Container ausgeführt werden, erhalten garantiert *R* RUs in jeder Region. Sie können RUs nicht selektiv einer bestimmten Region zuweisen. Die in einem Cosmos-Container (oder einer Datenbank) bereitgestellten RUs werden in allen mit Ihrem Cosmos-Konto verbundenen Regionen bereitgestellt.

Angenommen, ein Cosmos-Container ist mit *R* RUs konfiguriert und es gibt *N* Regionen, die dem Cosmos-Konto zugeordnet sind:

- Wenn das Cosmos-Konto mit einer einzigen Schreibregion konfiguriert ist, beträgt die Gesamtzahl von global im Container verfügbaren RUs: *R* x *N*.

- Wenn das Cosmos-Konto mit mehreren Schreibregionen konfiguriert ist, beträgt die Gesamtzahl von global im Container verfügbaren RUs: *R* x (*N*+1). Die zusätzlichen *R* RUs werden automatisch bereitgestellt, um Updatekonflikte und Anti-Entropie-Datenverkehr in den Regionen zu verarbeiten.

Die Wahl des [Konsistenzmodells](consistency-levels.md) wirkt sich auch auf den Durchsatz aus. Für die eher gelockerten Konsistenzebenen (z.B. *Sitzung*, *Präfixkonsistenz* und *letztliche Konsistenz*) können Sie den etwa 2-fachen Lesedurchsatz im Vergleich zu höheren Konsistenzebenen (z.B. *begrenzte Veraltung* oder *starke Konsistenz*) erzielen.

## <a name="next-steps"></a>Nächste Schritte

Als Nächstes erfahren Sie, wie Sie den Durchsatz für einen Container oder eine Datenbank konfigurieren:

* [Festlegen und Abrufen des Durchsatzes für Container und Datenbanken](set-throughput.md) 

