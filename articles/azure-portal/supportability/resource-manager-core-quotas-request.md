---
title: Anforderungen zur Erhöhung von Azure Resource Manager-vCPU-Kontingenten
description: Anforderungen zur Erhöhung von Azure Resource Manager-vCPU-Kontingenten
author: sowmyavenkat86
ms.author: svenkat
ms.date: 01/27/2020
ms.topic: how-to
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 7fd04428edc28315e714b9e81af0bea0e9344e52
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84763804"
---
# <a name="quota-increase-requests"></a>Anfordern einer Kontingenterhöhung

Resource Manager-vCPU-Kontingente für virtuelle Computer und VM-Skalierungsgruppen werden in jeder Region für jedes Abonnement auf zwei Ebenen erzwungen.

Die erste Ebene ist der Grenzwert regionaler vCPUs gesamt (für alle VM-Serien). Die zweite Ebene ist der vCPU-Grenzwert pro VM-Serie (z. B. vCPUs der D-Serie). Bei jeder Bereitstellung eines neuen virtuellen Computers darf die Summe aus neuer und bereits vorhandener vCPU-Nutzung für diese VM-Serie das für diese spezielle VM-Serie genehmigte vCPU-Kontingent nicht überschreiten. Ferner darf die Gesamtzahl der neuen und vorhandenen vCPUs, die insgesamt für alle VM-Serien bereitgestellt werden, das für das Abonnement genehmigte Kontingent „Regionale vCPUs gesamt“ nicht überschreiten. Wird eines dieser Kontingente überschritten, wird die VM-Bereitstellung nicht zugelassen.
Über das Azure-Portal können Sie eine Erhöhung des vCPU-Kontingentlimits für die VM-Serie anfordern. Bei einer Erhöhung des Kontingents für die VM-Serie erhöht sich automatisch auch der Grenzwert „Regionale vCPUs gesamt“ um den gleichen Wert.

Wenn ein neues Abonnement erstellt wird, entspricht der Standardwert für „Regionale vCPUs gesamt“ möglicherweise nicht der Summe der vCPU-Standardkontingente für alle einzelnen VM-Serien. Diese Tatsache kann zu einem Abonnement mit ausreichendem Kontingent für jede einzelne VM-Serie führen, die Sie bereitstellen möchten. Dadurch reicht das Kontingent für „Regionale vCPUs gesamt“ möglicherweise nicht für alle Bereitstellungen aus. In diesem Fall müssen Sie explizit eine Erhöhung des Grenzwerts „Regionale vCPUs gesamt“ anfordern. Der Grenzwert „Regionale vCPUs gesamt“ darf die Summe des genehmigten Kontingents für alle VM-Serien der Region nicht überschreiten.

Weitere Informationen zu Kontingenten finden Sie unter [vCPU-Kontingente für virtuelle Computer](../../virtual-machines/windows/quotas.md) und [Grenzwerte, Kontingente und Einschränkungen für Azure-Abonnements und Dienste](../../azure-resource-manager/management/azure-subscription-service-limits.md).

