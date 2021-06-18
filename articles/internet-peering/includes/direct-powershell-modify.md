---
title: Datei einfügen
titleSuffix: Azure
description: include file
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: ecc8396204138df05aa779e8bfa09bbfc6cd184d
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110721595"
---
In diesem Abschnitt wird beschrieben, wie Sie die folgenden Änderungsvorgänge für Direct Peering ausführen:

* Hinzufügen von Direct-Peeringverbindungen
* Entfernen von Direct-Peeringverbindungen
* Upgrade oder Downgrade der Bandbreite in aktiven Verbindungen
* Hinzufügen von IPv4-/IPv6-Sitzungen in aktiven Verbindungen
* Entfernen von IPv4-/IPv6-Sitzungen aus aktiven Verbindungen

### <a name="add-direct-peering-connections"></a>Hinzufügen von Direct Peering-Verbindungen

Das folgende Beispiel beschreibt, wie Sie einem vorhandenen Direct-Peering Verbindungen hinzufügen.

```powershell

$directPeering = Get-AzPeering -Name "SeattleDirectPeering" -ResourceGroupName "PeeringResourceGroup"

$connection = New-AzPeeringDirectConnection `
    -PeeringDBFacilityId $peeringLocation.PeeringDBFacilityId `
    -SessionPrefixV4 "10.22.31.0/31" `
    -SessionPrefixV6 "fe02::3e:0/127" `
    -MaxPrefixesAdvertisedIPv4 1000 `
    -MaxPrefixesAdvertisedIPv6 100 `
    -BandwidthInMbps 10000

$directPeering.Connections.Add($connection)

$directPeering | Update-AzPeering
```

### <a name="remove-direct-peering-connections"></a>Entfernen von Direct Peering-Verbindungen

Das Entfernen einer Verbindung mit PowerShell wird derzeit nicht unterstützt. Wenden Sie sich für weitere Informationen an das [Microsoft-Peering-Team](mailto:peeringexperience@microsoft.com).

<!--
```powershell
$directPeering.Connections.Remove($directPeering.Connections[0])

$directPeering | Update-AzPeering
```
-->

### <a name="upgrade-or-downgrade-bandwidth-on-active-connections"></a>Upgrade oder Downgrade der Bandbreite in aktiven Verbindungen

Dieses Beispiel beschreibt, wie Sie einer Direct-Verbindung 10 GBit/s hinzufügen.

```powershell

$directPeering = Get-AzPeering -Name "SeattleDirectPeering" -ResourceGroupName "PeeringResourceGroup"
$directPeering.Connections[0].BandwidthInMbps  = 20000
$directPeering | Update-AzPeering

```

### <a name="add-ipv4-or-ipv6-sessions-on-active-connections"></a>Hinzufügen von IPv4-/IPv6-Sitzungen in aktiven Verbindungen

In diesem Beispiel wird beschrieben, wie Sie einer vorhandenen Direct-Verbindung, die nur über eine IPv4-Sitzung verfügt, eine IPv6-Sitzung hinzufügen. 

```powershell

$directPeering = Get-AzPeering -Name "SeattleDirectPeering" -ResourceGroupName "PeeringResourceGroup"
$directPeering.Connections[0].BGPSession.SessionPrefixv6 = "fe01::3e:0/127"
$directPeering | Update-AzPeering

```

### <a name="remove-ipv4-or-ipv6-sessions-on-active-connections"></a>Entfernen von IPv4-/IPv6-Sitzungen aus aktiven Verbindungen

Das Entfernen einer IPv4-/IPv6-Sitzung aus einer vorhandenen Verbindung wird mit PowerShell derzeit nicht unterstützt. Wenden Sie sich für weitere Informationen an das [Microsoft-Peering-Team](mailto:peeringexperience@microsoft.com).
