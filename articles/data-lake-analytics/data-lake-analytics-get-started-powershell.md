---
title: Erstellen und Abfragen in Azure Data Lake Analytics – PowerShell
description: Verwenden Sie Azure PowerShell zum Erstellen eines Azure Data Lake Analytics-Kontos und Übermitteln eines U-SQL-Auftrags.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 05/04/2017
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 93a05231bc971737a08d74ad04150e5449dfc792
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92220940"
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Erste Schritte mit Azure Data Lake Analytics mithilfe von Azure PowerShell

[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Es wird beschrieben, wie Sie Azure PowerShell zum Erstellen von Azure Data Lake Analytics-Konten verwenden und anschließend U-SQL-Aufträge senden und ausführen. Weitere Informationen zu Data Lake Analytics finden Sie unter [Übersicht über Azure Data Lake Analytics](data-lake-analytics-overview.md).

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

* **Ein Azure Data Lake Analytics-Konto**. Weitere Informationen finden Sie unter [Erste Schritte mit Data Lake Analytics](./data-lake-analytics-get-started-portal.md).
* **Eine Arbeitsstation mit Azure PowerShell**. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/).

## <a name="log-in-to-azure"></a>Anmelden an Azure

In diesem Tutorial wird davon ausgegangen, dass Sie bereits mit der Verwendung von Azure PowerShell vertraut sind. Insbesondere müssen Sie wissen, wie Sie sich bei Azure anmelden. Wenn Sie Hilfe benötigen, finden Sie weitere Informationen unter [Erste Schritte mit Azure PowerShell](/powershell/azure/get-started-azureps).

So melden Sie sich mit einem Abonnementnamen an:

```powershell
Connect-AzAccount -SubscriptionName "ContosoSubscription"
```

Anstelle des Abonnementnamens können Sie für die Anmeldung auch eine Abonnement-ID verwenden:

```powershell
Connect-AzAccount -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

Bei erfolgreicher Durchführung ähnelt die Ausgabe dieses Befehls dem folgenden Text:

```text
Environment           : AzureCloud
Account               : joe@contoso.com
TenantId              : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionId        : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
SubscriptionName      : ContosoSubscription
CurrentStorageAccount :
```

## <a name="preparing-for-the-tutorial"></a>Vorbereiten des Tutorials

In den PowerShell-Codeausschnitten dieses Tutorials werden die folgenden Variablen zum Speichern dieser Informationen verwendet:

```powershell
$rg = "<ResourceGroupName>"
$adls = "<DataLakeStoreAccountName>"
$adla = "<DataLakeAnalyticsAccountName>"
$location = "East US 2"
```

## <a name="get-information-about-a-data-lake-analytics-account"></a>Abrufen von Informationen zu einem Data Lake Analytics-Konto

```powershell
Get-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla  
```

## <a name="submit-a-u-sql-job"></a>Senden eines U-SQL-Auftrags

Erstellen Sie eine PowerShell-Variable, die das U-SQL-Skript enthalten soll.

```powershell
$script = @"
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();

"@
```

Übermitteln Sie den Skripttext mithilfe des Cmdlets `Submit-AdlJob` und des Parameters `-Script`.

```powershell
$job = Submit-AdlJob -Account $adla -Name "My Job" -Script $script
```

Alternativ können Sie mithilfe des Parameters `-ScriptPath` eine Skriptdatei übermitteln:

```powershell
$filename = "d:\test.usql"
$script | out-File $filename
$job = Submit-AdlJob -Account $adla -Name "My Job" -ScriptPath $filename
```

Rufen Sie mithilfe von `Get-AdlJob` den Status eines Auftrags ab. 

```powershell
$job = Get-AdlJob -Account $adla -JobId $job.JobId
```

Verwenden Sie das Cmdlet `Wait-AdlJob`, anstatt immer wieder „Get-AdlJob“ aufzurufen, bis ein Auftrag abgeschlossen ist.

```powershell
Wait-AdlJob -Account $adla -JobId $job.JobId
```

Laden Sie mithilfe von `Export-AdlStoreItem` die Ausgabedatei herunter.

```powershell
Export-AdlStoreItem -Account $adls -Path "/data.csv" -Destination "C:\data.csv"
```

## <a name="see-also"></a>Siehe auch

* Wenn Sie dasselbe Tutorial mit anderen Tools verwenden möchten, klicken Sie oben auf der Seite auf die Registerkartenauswahl.
* Informationen zum Erlernen von U-SQL finden Sie unter [Erste Schritte mit der Sprache U-SQL für Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
* Informationen zu Verwaltungsaufgaben finden Sie unter [Verwalten von Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-manage-use-portal.md).