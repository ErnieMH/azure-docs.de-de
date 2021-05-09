---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 04/21/2021
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: b9b09012237d8f519322c927f8f2bbca1d3edef2
ms.sourcegitcommit: 19dcad80aa7df4d288d40dc28cb0a5157b401ac4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107925383"
---
Inkrementelle Momentaufnahmen sind Point-in-Time-Sicherungen für verwaltete Datenträger, die bei der Erfassung nur aus den Änderungen seit der letzten Momentaufnahme bestehen. Beim Wiederherstellen eines Datenträgers aus einer inkrementellen Momentaufnahme rekonstruiert das System den vollständigen Datenträger, der die Point-in-Time-Sicherung des Datenträgers zu dem Zeitpunkt darstellt, zu dem die inkrementelle Momentaufnahme erstellt wurde. Durch diese Funktion für Momentaufnahmen verwalteter Datenträger werden diese potenziell kostengünstiger, da Sie nicht mehr mit jeder einzelnen Momentaufnahme den gesamten Datenträger speichern müssen, es sei denn, Sie entscheiden sich dazu. Wie vollständige Momentaufnahmen können auch inkrementelle Momentaufnahmen verwendet werden, um entweder einen vollständigen verwalteten Datenträger oder eine vollständige Momentaufnahme zu erstellen. Sowohl vollständige als auch inkrementelle Momentaufnahmen können sofort nach dem Erstellen verwendet werden. Anders ausgedrückt: Sobald Sie eine Momentaufnahme erstellen, können Sie die zugrunde liegende VHD sofort lesen und zum Wiederherstellen von Datenträgern verwenden.

Es gibt einige Unterschiede zwischen einer inkrementellen und einer vollständigen Momentaufnahme. Inkrementelle Momentaufnahmen verwenden immer Festplattenlaufwerk-Standardspeicher, und zwar unabhängig vom Speichertyp des Datenträgers, wohingegen vollständige Momentaufnahmen Premium-SSD-Datenträger verwenden können. Wenn Sie vollständige Momentaufnahmen auf Storage Premium zum Hochskalieren von VM-Bereitstellungen verwenden, empfiehlt es sich, benutzerdefinierte Images im Standardspeicher in [Shared Image Gallery](../articles/virtual-machines/shared-image-galleries.md) zu verwenden. Dies unterstützt Sie dabei, eine umfangreiche Skalierung mit geringeren Kosten zu erzielen. Außerdem bieten inkrementelle Momentaufnahmen möglicherweise bessere Zuverlässigkeit durch [zonenredundanten Speicher](../articles/storage/common/storage-redundancy.md) (ZRS). Wenn ZRS in der ausgewählten Region verfügbar ist, verwendet eine inkrementelle Momentaufnahme automatisch ZRS. Wenn ZRS nicht in der Region verfügbar ist, wird für die Momentaufnahme standardmäßig [lokal redundanter Speicher](../articles/storage/common/storage-redundancy.md) (LRS) verwendet. Sie können dieses Verhalten außer Kraft setzen und manuell ein anderes Verhalten auswählen. Dies wird jedoch nicht empfohlen.

Inkrementelle Momentaufnahmen bieten außerdem eine differenzielle Funktion, die nur für verwaltete Datenträger verfügbar ist. Sie ermöglichen es Ihnen, die Änderungen zwischen zwei inkrementellen Momentaufnahmen der gleichen verwalteten Datenträger bis hinunter zur Blockebene abzurufen. Sie können diese Funktion verwenden, um den Datenbedarf beim regionsübergreifenden Kopieren von Momentaufnahmen zu verringern.  Beispielsweise können Sie die erste inkrementelle Momentaufnahme als Basisblob in einer anderen Region herunterladen. Für die nachfolgenden inkrementellen Momentaufnahmen können Sie nur die Änderungen seit der letzten Momentaufnahme in das Basisblob kopieren. Nachdem Sie die Änderungen kopiert haben, können Sie Momentaufnahmen für das Basisblob erstellen, die die Point-in-Time-Sicherung des Datenträgers in einer anderen Region darstellen. Sie können den Datenträger aus dem Basisblob oder aus einer Momentaufnahme für das Basisblob in einer anderen Region wiederherstellen.

:::image type="content" source="media/virtual-machines-disks-incremental-snapshots-description/incremental-snapshot-diagram.png" alt-text="Diagramm von in mehreren Regionen kopierte inkrementellen Momentaufnahmen. Momentaufnahmen führen verschiedene API-Aufrufe durch, bis sie schließlich bei jeder Momentaufnahme Seitenblobs bilden.":::

Für inkrementelle Momentaufnahmen wird nur die verwendete Größe abgerechnet. Sie finden die verwendete Größe Ihrer Momentaufnahmen im [Azure-Nutzungsbericht](../articles/cost-management-billing/understand/review-individual-bill.md). Beispiel: Beträgt die verwendete Datengröße einer Momentaufnahme 10 GiB, wird im **täglichen** Nutzungsbericht als verbrauchte Menge Folgendes angezeigt: 10 GiB/(31 Tage) = 0,3226.