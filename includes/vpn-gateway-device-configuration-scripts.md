---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/07/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e84a77629026bb8885a48b8ed928699825f07115
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "77111232"
---
| **Hersteller** | **Gerätefamilie** | **Firmwareversion** |
| --- | --- | --- |
|Cisco | ISR| IOS 15.1 (Vorschau)|
|Cisco | ASA | ASA ( * ) RouteBased (IKEv2 – ohne BGP) für ASA unter 9.8 |
|Cisco | ASA | ASA RouteBased (IKEv2 – ohne BGP) für ASA 9.8 und höher |
|Juniper | SRX_GA | 12.x|
|Juniper | SSG_GA | ScreenOS 6.2.x|
|Juniper | JSeries_GA | JunOS 12.x|
|Juniper | SRX | JunOS 12.x RouteBased BGP |
|Ubiquiti| EdgeRouter| EdgeOS v1.10x RouteBased VTI|
|Ubiquiti| EdgeRouter| EdgeOS v1.10x RouteBased BGP|

> [!NOTE]
> (*) Erforderlich: NarrowAzureTrafficSelectors (Option „UsePolicyBasedTrafficSelectors“ aktivieren) und CustomAzurePolicies (IKE/IPsec)
>
