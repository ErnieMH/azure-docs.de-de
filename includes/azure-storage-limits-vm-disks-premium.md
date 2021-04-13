---
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/09/2018
ms.author: rogarana
ms.openlocfilehash: ef18feb10dabc6a77e6512c6a32ad44b32c6e832
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "80334972"
---
**Nicht verwaltete Premium-VM-Datenträger: Grenzwerte pro Konto**

| Resource | Begrenzung |
| --- | --- |
| Datenträgerkapazität insgesamt pro Konto |35 TB |
| Kapazität für Momentaufnahmen insgesamt pro Konto |10 TB |
| Max. Bandbreite pro Konto (Ein- und Ausgang)<sup>1</sup> |<= 50 GBit/s |

<sup>1</sup>*Eingang* bezieht sich auf alle Daten aus Anforderungen, die an ein Speicherkonto gesendet werden. *Ausgang* bezieht sich auf alle Daten aus Anforderungen, die von einem Speicherkonto empfangen werden.

**Nicht verwaltete Premium-VM-Datenträger: Grenzwerte pro Datenträger**

| Storage Premium-Datenträgertyp | P10 | P20 | P30 | P40 | P50 |
| --- | --- | --- | --- | --- | --- |
| Datenträgergröße |128 GB |512 GB |1.024GiB (1TB) |2.048GiB (2TB)|4.095GiB (4TB)|
| Maximale Anzahl IOPS pro Datenträger |500 |2\.300 |5\.000 |7\.500 |7\.500 |
| Maximaler Durchsatz pro Datenträger |100 MB/s | 150 MB/s |200 MB/s |250MB/s |250 MB/s |
| Maximale Anzahl von Datenträgern pro Speicherkonto |280 |70 |35 | 17 | 8 |

**Nicht verwaltete Premium-VM-Datenträger: Grenzwerte pro VM**

| Resource | Begrenzung |
| --- | --- |
| Maximale Anzahl IOPS pro VM |80.000 IOPS mit GS5-VM |
| Maximaler Durchsatz pro VM |2.000MB/s mit GS5-VM |

