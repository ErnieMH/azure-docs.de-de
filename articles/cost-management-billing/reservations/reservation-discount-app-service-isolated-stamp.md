---
title: Reservierungsrabatte für Azure App Service
description: Hier erfahren Sie, wie Reservierungsrabatte auf Azure App Service-Stempelgebühren (gesondert) angewendet werden. Rabatte werden auf die Stempelgebühr in einer Region automatisch angewendet.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: banders
ms.openlocfilehash: f7d3134652d75cb4ce40b3e9366d3083f5d760df
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88682619"
---
# <a name="how-reservation-discounts-apply-to-azure-app-service-isolated-stamps"></a>Anwendung von Reservierungsrabatten auf Azure App Service-Stempelgebühren (gesondert)

Nachdem Sie reservierte Kapazität für die App Service-Stempelgebühr (gesondert) erworben haben, wird der Reservierungsrabatt automatisch auf die Stempelgebühr in einer Region angewendet. Der Reservierungsrabatt gilt für die über die Verbrauchseinheit für die Stempelgebühr (gesondert) ausgegebene Nutzung. Worker, zusätzliche Front-Ends und alle anderen Ressourcen, die dem Stempel zugeordnet sind, werden weiterhin zum regulären Satz abgerechnet.

## <a name="reservation-discount-application"></a>Anwendung des Reservierungsrabatts

Der Rabatt für die App Service-Stempelgebühr (gesondert) wird auf ausgeführte gesonderte Stempel auf Stundenbasis angewendet. Wenn Sie für eine Stunde keinen Stempel bereitgestellt haben, verfällt die reservierte Kapazität für diese Stunde. Sie wird nicht übertragen.

Nach dem Kauf wird die erworbene Reservierung einem gesonderten Stempel zugeordnet, der in einer bestimmten Region ausgeführt wird. Wenn Sie diesen Stempel herunterfahren, werden Reservierungsrabatte automatisch auf andere Stempel angewendet, die in der Region ausgeführt werden. Sind keine Stempel vorhanden, wird die Reservierung auf den nächsten Stempel angewendet, der in der Region erstellt wird.

Wenn Stempel nicht für eine ganze Stunde ausgeführt werden, gilt die Reservierung automatisch für andere entsprechende Stempel in derselben Region während dieser Stunde.

## <a name="choose-a-stamp-type---windows-or-linux"></a>Auswählen eines Stempeltyps: Windows oder Linux

Ein leerer gesonderter Stempel gibt standardmäßig die Windows-Verbrauchseinheit aus. Dies ist beispielsweise der Fall, wenn keine Worker bereitgestellt sind. Die Verbrauchseinheit wird weiterhin ausgegeben, wenn Windows-Worker bereitgestellt werden. Wenn Sie einen Linux-Worker bereitstellen, ändert sich die Verbrauchseinheit in die Verbrauchseinheit für Linux-Stempel. Werden sowohl Linux- als auch Windows-Worker bereitgestellt, gibt der Stempel die Windows-Verbrauchseinheit aus.

Daher kann sich die Verbrauchseinheit im Laufe der Lebensdauer des Stempels zwischen Windows und Linux ändern. Zwischenzeitlich sind Reservierungen betriebssystemspezifisch. Sie müssen eine Reservierung kaufen, die die Worker unterstützt, die Sie auf dem Stempel bereitstellen möchten. Für reine Windows-Stempel und gemischte Stempel wird die Windows-Reservierung verwendet. Bei Stempeln, auf denen ausschließlich Linux-Worker bereitgestellt werden, wird die Linux-Reservierung verwendet.

Eine Linux-Reservierung sollte nur erworben werden, wenn Sie _ausschließlich_ Linux-Worker für den Stempel verwenden möchten.

## <a name="discount-examples"></a>Beispiele für die Anwendung des Rabatts

Die folgenden Beispiele zeigen, wie der Rabatt für die reservierte Instanz der Stempelgebühr (gesondert) abhängig von den jeweiligen Bereitstellungen angewandt wird.

- **Beispiel 1:** Sie erwerben eine Instanz der reservierten Kapazität für gesonderte Stempel in einer Region ohne gesonderte App Service-Stempel. Sie stellen einen neuen Stempel für die Region bereit und zahlen die reservierten Raten für diesen Stempel.
- **Beispiel 2:** Sie erwerben eine Instanz der reservierten Kapazität für gesonderte Stempel in einer Region, in der bereits ein gesonderter App Service-Stempel bereitsteht. Sie erhalten die reservierte Rate für den bereitgestellten Stempel.
- **Beispiel 3:** Sie erwerben eine Instanz der reservierten Kapazität für gesonderte Stempel in einer Region, in der bereits ein gesonderter App Service-Stempel bereitsteht. Sie erhalten die reservierte Rate für den bereitgestellten Stempel. Später löschen Sie den Stempel und stellen einen neuen bereit. Sie erhalten die reservierte Rate für den neuen Stempel. Rabatte werden für Zeiträume ohne bereitgestellte Stempel nicht übertragen.
- **Beispiel 4:** Sie erwerben eine Instanz der Kapazität für gesonderte Linux-reservierte Stempel in einer Region und stellen dann einen neuen Stempel für die Region bereit. Wird der Stempel anfänglich ohne Worker bereitgestellt, wird die Windows-Verbrauchseinheit ausgegeben. Es wird kein Rabatt angewendet. Sobald der erste Linux-Worker für den Stempel bereitstellt wird, wird die Linux-Verbrauchseinheit ausgegeben und der Reservierungsrabatt angewendet. Wird später ein Windows-Worker für den Stempel bereitgestellt, wird die Verbrauchseinheit auf Windows zurückgesetzt. Sie erhalten keinen Rabatt mehr auf die Reservierung für gesonderte Linux-reservierte Stempel.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Verwalten einer Reservierung finden Sie unter [Verwalten von Azure-Reservierungen](manage-reserved-vm-instance.md).
- Weitere Informationen zum Voraberwerb reservierter Kapazität für gesonderte App Service-Stempel zur Einsparung von Kosten finden Sie unter [Vorauszahlen der Azure App Service-Stempelgebühr (gesondert) mit reservierter Kapazität](prepay-app-service-isolated-stamp.md).
- Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:
  - [Was sind Azure-Reservierungen?](save-compute-costs-reservations.md)
  - [Verwalten von Reservierungen für Ressourcen in Azure](manage-reserved-vm-instance.md)
  - [Grundlegendes zur Reservierungsnutzung bei einem Abonnement mit Sätzen für nutzungsbasierte Bezahlung](understand-reserved-instance-usage.md)
  - [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](understand-reserved-instance-usage-ea.md)
