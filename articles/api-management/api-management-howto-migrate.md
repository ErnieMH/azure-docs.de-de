---
title: Regionsübergreifendes Migrieren von Azure API Management
description: Erfahren Sie, wie Sie eine API Management-Instanz aus einer Region zu einer anderen migrieren.
services: api-management
documentationcenter: ''
author: miaojiang
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/26/2019
ms.author: apimpm
ms.openlocfilehash: 0eed2328aca78402c5f4691bb9b3d07d4f36472e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86250225"
---
# <a name="how-to-migrate-azure-api-management-across-regions"></a>Regionsübergreifendes Migrieren von Azure API Management
Wenn Sie API Management-Instanzen von einer Azure-Region zu einer anderen migrieren möchten, können Sie die Funktion [Sichern und Wiederherstellen](api-management-howto-disaster-recovery-backup-restore.md) verwenden. In den Quell- und Zielregionen sollten Sie denselben API Management-Tarif auswählen. 

> [!NOTE]
> Sichern und Wiederherstellen funktioniert nicht bei der Migration zwischen verschiedenen Cloudtypen. Dazu müssen Sie die Ressource [als Vorlage](../azure-resource-manager/management/manage-resource-groups-portal.md#export-resource-groups-to-templates) exportieren. Passen Sie dann die exportierte Vorlage für die Azure-Zielregion an, und erstellen Sie die Ressource neu. 

## <a name="option-1-use-a-different-api-management-instance-name"></a>Option 1: Verwenden eines anderen API Management-Instanznamens

1. Erstellen Sie in der Zielregion eine neue API Management-Instanz mit demselben Tarif wie die API Management-Quellinstanz. Die neue Instanz sollte einen anderen Namen aufweisen. 
1. Sichern Sie die vorhandene API Management-Instanz in einem Speicherkonto.
1. Stellen Sie die in Schritt 2 erstellte Sicherung in der neuen API Management-Instanz wieder her, die Sie in Schritt 1 in der neuen Region erstellt haben.
1. Wenn Sie über eine benutzerdefinierte Domäne verfügen, die auf die API Management-Instanz in der Quellregion verweist, aktualisieren Sie den CNAME-Eintrag der benutzerdefinierten Domäne so, dass er auf die neue API Management-Instanz verweist. 


## <a name="option-2-use-the-same-api-management-instance-name"></a>Option 2: Verwenden desselben API Management-Instanznamens

> [!NOTE]
> Diese Option führt zu einer Ausfallzeit während der Migration.

1. Sichern Sie die API Management-Instanz in der Quellregion in einem Speicherkonto.
1. Löschen Sie die API Management-Instanz in der Quellregion. 
1. Erstellen Sie eine neue API Management-Instanz in der Zielregion mit demselben Tarif wie in der Quellregion.
1. Stellen Sie die in Schritt 1 erstellte Sicherung in der neuen API Management-Instanz in der Zielregion wieder her.  


## <a name="next-steps"></a><a name="next-steps"> </a>Nächste Schritte
* Weitere Informationen zur Funktion „Sichern und Wiederherstellen“ finden Sie unter [So implementieren Sie die Notfallwiederherstellung](api-management-howto-disaster-recovery-backup-restore.md).
* Informationen zur Migration von Azure-Ressourcen finden Sie unter [Anleitung zur regionsübergreifenden Azure-Migration](https://github.com/Azure/Azure-Migration-Guidance).
* [Ermitteln und Analysieren von Kosten mit der Kostenanalyse](../cost-management-billing/costs/quick-acm-cost-analysis.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)
