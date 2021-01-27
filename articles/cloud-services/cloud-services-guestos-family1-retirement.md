---
title: Deaktivierungsinformationen für die Gastbetriebssystemfamilie 1 | Microsoft Docs
description: Bietet Informationen zum Zeitpunkt der Deaktivierung der Azure-Gastbetriebssystemfamilie 1 und dazu, wie Sie ermitteln, ob Sie betroffen sind
services: cloud-services
documentationcenter: na
author: raiye
manager: timlt
ms.service: cloud-services
ms.topic: article
ms.date: 5/21/2017
ms.author: raiye
ms.openlocfilehash: 7f6d3feee95d4cce654b2cc1547b8bd7f4eb45d2
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98743014"
---
# <a name="guest-os-family-1-retirement-notice"></a>Deaktivierungsinformationen für die Azure-Gastbetriebssystemfamilie 1

Die Deaktivierung der Betriebssystemfamilie 1 wurde erstmals am 1. Juni 2013 angekündigt.

**2. September 2014** Die Azure-Gastbetriebssystemfamilie 1.x, die auf dem Betriebssystem Windows Server 2008 basiert, wurde offiziell außer Betrieb gestellt. Alle Versuche der Bereitstellung neuer Dienste oder der Aktualisierung vorhandener Dienste mithilfe von Familie 1 führen zu einer Fehlermeldung, die Sie darüber informiert, dass die Gastbetriebssystemfamilie 1 eingestellt wurde.

**3. November 2014** Die erweiterte Unterstützung für die Gastbetriebssystemfamilie 1 wurde beendet, und die Familie wurde vollständig außer Betrieb genommen. Alle noch in Familie 1 enthaltenen Dienste sind davon betroffen. Wir können diese Dienste jederzeit beenden. Es gibt keine Garantie dafür, dass Ihre Dienste weiterhin ausgeführt werden können, sofern Sie diese nicht selbst manuell aktualisieren.

Wenn Sie weitere Fragen haben, besuchen Sie die [Frageseite von Microsoft Q&A (Fragen und Antworten) für Cloud Services](/answers/topics/azure-cloud-services.html), oder [wenden Sie sich an den Azure-Support](https://azure.microsoft.com/support/options/).

## <a name="are-you-affected"></a>Sind Sie betroffen?
Ihre Clouddienste sind betroffen, wenn eine der folgenden Bedingungen zutrifft:

1. In der Datei "ServiceConfiguration.cscfg" Ihres Clouddiensts ist explizit der Wert "osFamily = "1" angegeben.
2. In der Datei "ServiceConfiguration.cscfg" Ihres Clouddiensts ist kein expliziter Wert für "osFamily" angegeben. In diesem Fall verwendet das System aktuell den Standardwert "1".
3. Im Azure-Portal ist der Wert Ihrer Gastbetriebssystemfamilie als „Windows Server 2008“ angegeben.

Um herauszufinden, welcher Ihrer Clouddienste unter welcher Betriebssystemfamilie ausgeführt wird, können Sie das folgende Skript in Azure PowerShell ausführen. Sie müssen jedoch zuerst [Azure PowerShell einrichten](/powershell/azure/). Weitere Informationen zum Skript finden Sie unter [Azure Guest OS Family 1 End of Life: June 2014](/archive/blogs/ryberry/azure-guest-os-family-1-end-of-life-june-2014) (Einstellung der Azure-Gastbetriebssystemfamilie 1: Juni 2014).

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

Ihre Clouddienste sind von der Deaktivierung der Betriebssystemfamilie 1 betroffen, wenn die Spalte "osFamily" in der Ausgabe des Skripts leer ist oder den Wert "1" enthält.

## <a name="recommendations-if-you-are-affected"></a>Empfehlungen, wenn Sie betroffen sind
Wir empfehlen Ihnen, Ihre Clouddienstrollen zu einer der unterstützten Gastbetriebssystemfamilien zu migrieren:

**Gastbetriebssystemfamilie 4.x** - Windows Server 2012 R2 *(empfohlen)*

1. Stellen Sie sicher, dass Ihre Anwendung SDK 2.1 oder höher mit .NET-Framework 4.0, 4.5 oder 4.5.1 verwendet.
2. Legen Sie in der Datei "ServiceConfiguration.cscfg" das osFamily-Attribut auf den Wert "4" fest, und stellen Sie Ihren Clouddienst erneut bereit.

**Gastbetriebssystemfamilie 3.x** - Windows Server 2012

1. Stellen Sie sicher, dass Ihre Anwendung SDK 1.8 oder höher mit .NET-Framework 4.0 oder 4.5 verwendet.
2. Legen Sie in der Datei "ServiceConfiguration.cscfg" das osFamily-Attribut auf den Wert "3" fest, und stellen Sie Ihren Clouddienst erneut bereit.

**Gastbetriebssystemfamilie 2.x** - Windows Server 2008 R2

1. Stellen Sie sicher, dass Ihre Anwendung SDK 1.3 oder höher mit .NET-Framework 3.5 oder 4.0 verwendet.
2. Legen Sie in der Datei "ServiceConfiguration.cscfg" das osFamily-Attribut auf den Wert "2" fest, und stellen Sie Ihren Clouddienst erneut bereit.

## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a>Erweiterte Unterstützung für Gastbetriebssystemfamilie 1 wurde am 3. November 2014 beendet
Clouddienste unter Gastbetriebssystemfamilie 1 werden nicht mehr unterstützt. Migrieren Sie schnellstmöglich von Familie 1 zu einer neueren Familie, um Dienstunterbrechungen zu vermeiden.  

## <a name="next-steps"></a>Nächste Schritte
Überprüfen Sie die neuesten [Gastbetriebssystemreleases](cloud-services-guestos-update-matrix.md).