---
title: 'PowerShell-Skript: Überwachen der Nutzung von Azure Data Share | Microsoft-Dokumentation'
description: Dieses PowerShell-Skript ruft Nutzungsmetriken einer gesendeten Datenfreigabe ab.
services: data-share
author: joannapea
ms.service: data-share
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: 0a4084d309dd0160970f1c03540705b310eb8e75
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "70307205"
---
# <a name="use-powershell-to-monitor-the-usage-of-a-sent-data-share"></a>Verwenden von PowerShell zum Überwachen der Verwendung einer gesendeten Datenfreigabe

Dieses PowerShell-Skript überwacht die Datennutzung durch Auflisten der Synchronisierungen einer gesendeten Datenfreigabe und Abrufen der Details zu einer bestimmten Synchronisierung.

## <a name="sample-script"></a>Beispielskript


```powershell
# Set variables with your own values
$resourceGroupName = "<Resource group name>"
$dataShareAccountName = "<Data share account name>"
$dataShareName = "<Data share name>"
$synchronizationId = "<Azure synchronization id>"

# List synchronizations on a share
Get-AzDataShareSynchronization -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -ShareName $dataShareName

#Get details of a specific synchronization
Get-AzDataShareSynchronizationDetails -ResourceGroupName $resourceGroupName -AccountName $dataShareAccountName -ShareName $dataShareName -SynchronizationId $synchronizationId
```


## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle: 

| Get-Help | Notizen |
|---|---|
| [Get-AzDataShareSynchronization](/powershell/module/az.datashare/get-azdatasharesynchronization?view=azps-2.6.0) | Auflisten der Synchronisierung in einer Freigabe. |
| [Get-AzDataShareSynchronizationDetails](/powershell/module/az.datashare/get-azdatasharesynchronizationdetail?view=azps-2.6.0) | Ruft die Synchronisierungsdetails einer Freigabesynchronisierung ab |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/).

Weitere PowerShell-Skriptbeispiele für Azure Data Share finden Sie unter [Azure PowerShell-Beispiele für Azure Data Share](../../samples-powershell.md).
