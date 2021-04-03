---
title: Problembehandlung von Fehlern beim Verschieben
description: Problembehandlung beim Verschieben von Azure-Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement.
ms.topic: conceptual
ms.date: 08/27/2019
ms.openlocfilehash: 41b1e2435caf9874f3582a3394664c7b7f5a8d29
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "90054161"
---
# <a name="troubleshoot-moving-azure-resources-to-new-resource-group-or-subscription"></a>Beheben von Problemen beim Verschieben von Azure-Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement

In diesem Artikel finden Sie Vorschläge zum Beheben von Problemen beim Verschieben von Ressourcen.

## <a name="upgrade-a-subscription"></a>Aktualisieren eines Abonnements

Wenn Sie ein Upgrade für Ihr Azure-Abonnement durchführen möchten (z.B. Wechsel vom kostenlosen Abonnement zur nutzungsbasierten Bezahlung), müssen Sie Ihr Abonnement konvertieren.

* Falls Sie für eine kostenlose Testversion ein Upgrade durchführen möchten, helfen Ihnen die Informationen unter [Aktualisieren Ihrer kostenlosen Testversion oder Ihres Microsoft Imagine Azure-Abonnements auf nutzungsbasierte Bezahlung](../../cost-management-billing/manage/upgrade-azure-subscription.md) weiter.
* Informationen zum Ändern eines Kontos mit nutzungsbasierter Bezahlung finden Sie unter [Ändern Ihres Azure-Abonnements mit nutzungsbasierter Bezahlung in ein anderes Angebot](../../cost-management-billing/manage/switch-azure-offer.md).

Wenn Sie das Abonnement nicht konvertieren können, können Sie [eine Azure-Supportanfrage erstellen](../../azure-portal/supportability/how-to-create-azure-support-request.md). Wählen Sie **Abonnementverwaltung** als Problemtyp aus.

## <a name="service-limitations"></a>Diensteinschränkungen

Einige Dienste erfordern zusätzliche Überlegungen beim Verschieben von Ressourcen. Wenn Sie die folgenden Dienste verschieben, sollten Sie unbedingt die Anleitungen und Einschränkungen überprüfen.

* [App Services](./move-limitations/app-service-move-limitations.md)
* [Azure DevOps Services](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json)
* [Klassisches Bereitstellungsmodell](./move-limitations/classic-model-move-limitations.md)
* [Netzwerk](./move-limitations/networking-move-limitations.md)
* [Recovery Services](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)
* [Virtuelle Computer](./move-limitations/virtual-machines-move-limitations.md)

## <a name="large-requests"></a>Umfangreiche Anforderungen

Unterteilen Sie große Verschiebevorgänge nach Möglichkeit in separate Verschiebevorgänge. Resource Manager gibt sofort einen Fehler aus, wenn ein einziger Vorgang mehr als 800 Ressourcen umfasst. Beim Verschieben von weniger als 800 Ressourcen kann jedoch ebenfalls ein Fehler durch ein Timeout auftreten.

## <a name="resource-not-in-succeeded-state"></a>Ressource nicht in erfolgreichem Zustand

Wenn Sie eine Fehlermeldung erhalten, die besagt, dass eine Ressource nicht verschoben werden kann, da sie sich nicht in einem erfolgreichen Zustand befindet, blockiert möglicherweise eine abhängige Ressource die Verschiebung. In der Regel lautet der Fehlercode **MoveCannotProceedWithResourcesNotInSucceededState**.

Wenn die Quell- oder Zielgruppenressource ein virtuelles Netzwerk enthält, werden während des Verschiebens die Zustände aller abhängigen Ressourcen des virtuellen Netzwerks überprüft. Die Überprüfung umfasst direkt und indirekt vom virtuellen Netzwerk abhängige Ressourcen. Wenn eine dieser Ressourcen den Zustand „Ausgefallen“ aufweist, wird die Verschiebung blockiert. Wenn beispielsweise ein virtueller Computer, der das virtuelle Netzwerk nutzt, ausgefallen ist, wird das Verschieben blockiert. Das Verschieben wird selbst dann blockiert, wenn der virtuelle Computer nicht zu den zu verschiebenden Ressourcen gehört und sich nicht in einer der Ressourcengruppen des Verschiebevorgangs befindet.

Wenn Sie diesen Fehler erhalten, haben Sie zwei Möglichkeiten. Verschieben Sie entweder Ihre Ressourcen in eine Ressourcengruppe ohne virtuelles Netzwerk, oder [kontaktieren Sie den Support](../../azure-portal/supportability/how-to-create-azure-support-request.md).

## <a name="next-steps"></a>Nächste Schritte

Befehle zum Verschieben von Ressourcen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](move-resource-group-and-subscription.md).
