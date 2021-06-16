---
title: Delegieren einer Unterdomäne – Azure PowerShell – Azure DNS
description: Mit diesem Lernpfad können Sie mit dem Delegieren einer Azure DNS-Unterdomäne mithilfe der Azure PowerShell beginnen.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: how-to
ms.date: 05/03/2021
ms.author: rohink
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 0030eccff0885c2cf2e1e0eef386b8907aa0df29
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110689367"
---
# <a name="delegate-an-azure-dns-subdomain-using-azure-powershell"></a>Delegieren einer Azure DNS-Unterdomäne mithilfe von Azure PowerShell

Sie können Azure PowerShell zum Delegieren einer DNS-Unterdomäne verwenden. Wenn Sie beispielsweise die Domäne contoso.com besitzen, können Sie eine Unterdomäne namens *engineering* an eine andere separate Zone delegieren, die Sie separat von der contoso.com Zone verwalten können.

Falls Sie dies vorziehen, können Sie eine Unterdomäne auch mithilfe des [Azure-Portals](delegate-subdomain.md) delegieren.

> [!NOTE]
> In diesem Artikel wird „contoso.com“ als Beispiel verwendet. Ersetzen Sie Ihren eigenen Domänennamen durch „contoso.com“.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Voraussetzungen

Zum Delegieren einer Azure DNS-Unterdomäne müssen Sie zunächst Ihre öffentliche Domäne an Azure DNS delegieren. Anweisungen zum Konfigurieren Ihrer Namenserver für die Delegierung finden Sie unter [Delegieren einer Domäne an Azure DNS](./dns-delegate-domain-azure-dns.md). Sobald Ihre Domäne an Ihre Azure DNS-Zone delegiert ist, können Sie Ihre Unterdomäne delegieren.

## <a name="create-a-zone-for-your-subdomain"></a>Erstellen einer Zone für die Unterdomäne

Erstellen Sie zuerst die Zone für die Unterdomäne **engineering**.

```azurepowershell-interactive
New-AzDnsZone -ResourceGroupName <resource group name> -Name engineering.contoso.com
```

## <a name="note-the-name-servers"></a>Notieren der Namenserver

Notieren Sie sich als Nächstes die vier Namenserver für die engineering-Unterdomäne.

```azurepowershell-interactive
Get-AzDnsRecordSet -ZoneName engineering.contoso.com -ResourceGroupName <resource group name> -RecordType NS
```

## <a name="create-a-test-record"></a>Erstellen eines Testeintrags

Erstellen Sie einen **A**-Eintrag in der engineering-Zone, den Sie zum Testen verwenden.

```azurepowershell-interactive
New-AzDnsRecordSet -ZoneName engineering.contoso.com -ResourceGroupName <resource group name> -Name www -RecordType A -ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address 10.10.10.10)
```

## <a name="create-an-ns-record"></a>Erstellen eines NS-Eintrags

Als Nächstes erstellen Sie einen Eintrag für den Namenserver (NS) für die **engineering**-Zone in der contoso.com-Zone.

```azurepowershell-interactive
$Records = @()
$Records += New-AzDnsRecordConfig -Nsdname <name server 1 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 2 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 3 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 4 noted previously>
$RecordSet = New-AzDnsRecordSet -Name engineering -RecordType NS -ResourceGroupName <resource group name> -TTL 3600 -ZoneName contoso.com -DnsRecords $Records
```

## <a name="test-the-delegation"></a>Testen der Delegierung

Verwenden Sie „nslookup“ zum Testen der Delegierung.

1. Öffnen Sie ein PowerShell-Fenster.

1. Geben Sie an einer Eingabeaufforderung Folgendes ein: `nslookup www.engineering.contoso.com.`.

1. Sie sollten eine nicht autoritative Antwort mit der Adresse **10.10.10.10** erhalten.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Reverse-DNS-Lookups für in Azure gehostete Dienste konfigurieren](dns-reverse-dns-for-azure-services.md).
