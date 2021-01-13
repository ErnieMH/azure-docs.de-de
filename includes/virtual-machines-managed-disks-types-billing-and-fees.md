---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/22/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: c0627dd0833e3b20468eb5f50fbeb9fd9d9ae2b3
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88864728"
---
**Ausgehende Datenübertragungen**: [Ausgehende Datenübertragungen](https://azure.microsoft.com/pricing/details/bandwidth/) (Daten, die von den Azure-Datencentern ausgehen) verursachen Kosten bei der Bandbreitenverwendung.

**Transaktionen:** Ihnen wird die Anzahl von Transaktionen berechnet, die Sie für einen verwalteten Standarddatenträger durchführen. Für SSD Standard gilt jeder E/A-Vorgang kleiner oder gleich 256 KiB Durchsatz als ein einzelner E/A-Vorgang. E/A-Vorgänge mit einem Durchsatz größer als 256 KiB gelten als mehrere Ein-bzw. Ausgabevorgänge der Größe 256 KiB. Für HDD Standard wird jeder E/A-Vorgang als einzelne Transaktion betrachtet, und zwar unabhängig von der E/A-Größe.

Ausführliche Informationen zu den Preisen für Managed Disks, einschließlich der Transaktionskosten, finden Sie unter [Verwaltete Datenträger – Preise](https://azure.microsoft.com/pricing/details/managed-disks).

### <a name="ultra-disk-vm-reservation-fee"></a>Reservierungsgebühr für virtuellen Computer auf Ultra-Datenträger

Virtuelle Azure-Computer können angeben, ob sie mit Ultra-Datenträgern kompatibel sind. Ein mit Ultra-Datenträgern kompatibler virtueller Computer weist dedizierte Bandbreitenkapazität zwischen der Compute-VM-Instanz und der Blockspeicher-Skalierungseinheit zu, um die Leistung zu optimieren und die Wartezeiten zu verringern. Das Hinzufügen dieser Funktion auf dem virtuellen Computer führt dazu, dass die Reservierungsgebühr nur erhoben wird, wenn Sie die Ultra-Datenträgerfunktion auf dem virtuellen Computer aktiviert haben, ohne ihr einen Ultra-Datenträger anzufügen. Wenn ein Ultra-Datenträger an den mit Ultra-Datenträgern kompatiblen virtuellen Computer angefügt ist, fällt diese Gebühr nicht an. Diese Gebühr fällt pro auf der VM bereitgestellter vCPU an. 

> [!Note]
> Für [VM-Größen mit eingeschränkter Anzahl der Kerne](../articles/virtual-machines/constrained-vcpu.md) ergibt sich die Reservierungsgebühr anhand der tatsächlichen Anzahl der vCPUs und nicht anhand der eingeschränkten Kerne. Für Standard_E32-8s_v3 basiert die Reservierungsgebühr auf 32 Kernen. 

Preisdetails für Ultra-Datenträger finden Sie auf der [Seite mit der Preisübersicht für Azure-Datenträger](https://azure.microsoft.com/pricing/details/managed-disks/).

### <a name="azure-disk-reservation"></a>Azure-Datenträgerreservierung

Die Datenträgerreservierung bietet die Möglichkeit, Datenträgerspeicher ein Jahr im Voraus zu Rabattkonditionen zu erwerben und so die Gesamtkosten zu reduzieren. Beim Erwerb einer Datenträgerreservierung wählen Sie eine bestimmte Datenträger-SKU in einer Zielregion aus, z. B. 10 P30 (1 TiB) Premium-SSDs in der Region USA, Osten 2 für eine Laufzeit von einem Jahr. Die Reservierung folgt einem Modell, das dem für reservierte VM-Instanzen ähnelt. Sie können VM- und Datenträgerreservierungen bündeln, um die Einsparungen zu maximieren. Derzeit bietet die Azure-Datenträgerreservierung einen Ein-Jahres-Verpflichtungsplan für Premium-SSD-SKUs von P30 (1 TiB) bis P80 (32 TiB) in allen Produktregionen. Weitere Informationen zu den Preisen für reservierte Datenträger finden Sie auf der [Seite mit der Preisübersicht für Azure-Datenträger](https://azure.microsoft.com/pricing/details/managed-disks/).