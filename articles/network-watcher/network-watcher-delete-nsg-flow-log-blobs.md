---
title: Löschen von Speicherblobs für Flowprotokolle für Netzwerksicherheitsgruppen in Azure Network Watcher | Microsoft-Dokumentation
description: In diesem Artikel wird erläutert, wie Sie die Speicherblobs für Flowprotokolle für Netzwerksicherheitsgruppen, die sich nicht mehr innerhalb Aufbewahrungsrichtlinie befinden, in Azure Network Watcher löschen.
services: network-watcher
documentationcenter: na
author: damendo
manager: ''
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/07/2021
ms.author: damendo
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 9ef7160c667ed9fa5f932b49c295a1247ff94c30
ms.sourcegitcommit: df574710c692ba21b0467e3efeff9415d336a7e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110665347"
---
# <a name="delete-network-security-group-flow-log-storage-blobs-in-network-watcher"></a>Löschen von Speicherblobs für Flowprotokolle für Netzwerksicherheitsgruppen in Network Watcher

Wenn Sie Datenflussprotokolle manuell aus Ihrem Speicherkonto löschen müssen, können Sie das folgende PowerShell-Skript verwenden.
Mit diesem Skript werden nur Speicherblobs gelöscht, die älter sind als die vorhandene Aufbewahrungsrichtlinie, die vom Benutzer angegeben wurde.

## <a name="run-powershell-script-to-delete-nsg-flow-logs"></a>Ausführen des PowerShell-Skripts zum Löschen von NSG-Flowprotokollen
 
Kopieren und speichern Sie das folgende Skript an einem Speicherort, z. B. in Ihrem aktuellen Arbeitsverzeichnis. 

```powershell
# This powershell script deletes all NSG flow log blobs that should not be retained anymore as per configured retention policy.
# While configuring NSG flow logs on Azure portal, the user configures the retention period of NSG flow log blobs in
# their storage account (in days).
# This script reads all blobs and deletes blobs that are not to be retained (outside retention window)
# if the retention days are zero; all blobs are retained forever and hence no blobs are deleted.
#
#

param (
        [string] [Parameter(Mandatory=$true)]  $SubscriptionId,
        [string] [Parameter(Mandatory=$true)]  $Location,
        [switch] [Parameter(Mandatory=$false)] $Confirm
    )

Login-AzAccount

$SubId = Get-AzSubscription| Where-Object {$_.Id.contains($SubscriptionId.ToLower())}

if ($SubId.Count -eq 0)
{
    Write-Error 'The SubscriptionId does not exist' -ErrorAction Stop
}

Set-AzContext -SubscriptionId $SubscriptionId

$NsgList = Get-AzNetworkSecurityGroup | Where-Object {$_.Location -eq $Location}
$NW = Get-AzNetworkWatcher | Where-Object {$_.Location -eq $Location}

$FlowLogsList = @()
foreach ($Nsg in $NsgList)
{
    # Query Flow Log Status which are enabled
    $NsgFlowLog = Get-AzNetworkWatcherFlowLogStatus -NetworkWatcher $NW -TargetResourceId $Nsg.Id | Where-Object {$_.Enabled -eq "True"}
    if ($NsgFlowLog.Count -gt 0)
    {
        $FlowLogsList +=  $NsgFlowLog
        Write-Output ('Enabled NSG found: ' +  $NsgFlowLog.TargetResourceId)
    }
}

foreach ($Psflowlog in $FlowLogsList)
{
    $RetentionDays = $Psflowlog.RetentionPolicy.Days
    if ($RetentionDays -le 0)
    {
        continue
    }

    $Strings = $Psflowlog.StorageId -split '/'
    $RGName = $Strings[4]
    $StorageAccountName = $Strings[-1]

    $Key = (Get-AzStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName).Value[1]
    $StorageAccount = New-AzStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $Key

    $ContainerName = 'insights-logs-networksecuritygroupflowevent'  
    $BLobsList = Get-AzStorageBlob -Container $ContainerName -Context $StorageAccount.Context

    $TargetBLobsList = $BLobsList | Where-Object {$_.Name.contains($Psflowlog.TargetResourceId.ToUpper())}

    $RetentionDate = Get-Date
    $RetentionDate = $RetentionDate.AddDays(-1*$RetentionDays)
    $RetentionDateInUTC = $RetentionDate.ToUniversalTime()

    foreach ($Blob in $TargetBLobsList)
    {
        $BlobLastModifietedDTinUTC = [datetime]$Blob.LastModified.UtcDateTime

        if ($BlobLastModifietedDTinUTC -ge  $RetentionDateInUTC)
        {
            Write-Output ($Blob.Name + '===>' + $BlobLastModifietedDTinUTC  + ' ===> RETAINED')
            continue
        }

        if ($Confirm)
        {
            Write-Output (Blob to be deleted: $Blob.Name)
            $Confirmation = Read-Host "Are you sure you want to remove this blob (Y/N)?"
        }

        if ((-not $Confirm) -or ($Confirmation -eq 'Y'))
        {
            Write-Output ($Blob.Name + '===>' + $BlobLastModifietedDTinUTC  + ' ===> DELETED')
            Remove-AzStorageBlob -Container $ContainerName -Context $StorageAccount.Context -Blob $Blob.Name
        }
        else
        {
            Write-Output ($Blob.Name + '===>' + $BlobLastModifietedDTinUTC  + ' ===> RETAINED')
        }
    }
}

Write-Output ('Retention policy for all NSGs evaluated and completed successfully')
```

1. Geben Sie bei Bedarf die folgenden Parameter im Skript ein:
   - **SubscriptionId** [obligatorisch]: Die Abonnement-ID, über die die Blobs für die NSG-Flowprotokolle gelöscht werden sollen.
   - **Location** [obligatorisch]: Die _Standortzeichenfolge_ der Region der Netzwerksicherheitsgruppen, für die die Blobs der NSG-Flowprotokolle gelöscht werden sollen. Diese Informationen können Sie im Azure-Portal oder über [GitHub](https://github.com/Azure/azure-extensions-cli/blob/beb3d3fe984cfa9c7798cb11a274c5337968cbc5/regions.go#L23) anzeigen.
   - **Confirm** [optional]: Übergeben Sie das confirm-Flag, wenn Sie den Löschvorgang der einzelnen Speicherblobs manuell überprüfen und bestätigen möchten.

1. Führen Sie das gespeicherte Skript wie im folgenden Beispiel gezeigt aus. In diesem Beispiel wurde die Skriptdatei unter dem Namen **Delete-NsgFlowLogsBlobs.ps1** gespeichert:
   ```
   .\Delete-NsgFlowLogsBlobs.ps1 -SubscriptionId <subscriptionId> -Location  <location> -Confirm
   ```
    
## <a name="next-steps"></a>Nächste Schritte
- Kunden können die Ausführung des Skripts mit [Azure Logic Apps](../logic-apps/logic-apps-overview.md) oder [Azure Automation](https://azure.microsoft.com/services/automation/) automatisieren.
- Unter [Azure Monitor-Protokolle für Netzwerksicherheitsgruppen (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) finden Sie weitere Informationen über die NSG-Protokollierung.

