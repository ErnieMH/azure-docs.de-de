---
title: Azure PowerShell-Beispiele für virtuelle Netzwerke
description: Hier erfahren Sie mehr über Azure PowerShell-Beispiele zum Verwalten virtueller Netzwerke. Dazu gehört u. a. ein Beispiel zum Erstellen eines virtuellen Netzwerks für Anwendungen mit mehreren Ebenen.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: mtillman
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 07/15/2019
ms.author: kumud
ms.openlocfilehash: b3107d521fb79e3ea6cfe190abd2ec3fb7859cb3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87288232"
---
# <a name="azure-powershell-samples-for-virtual-network"></a>Azure PowerShell-Beispiele für virtuelle Netzwerke

Die folgende Tabelle enthält Links zu Azure PowerShell-Skripts:

| Skript | BESCHREIBUNG |
|----|----|
| [Erstellen eines virtuellen Netzwerks für Anwendungen mit mehreren Ebenen](./scripts/virtual-network-powershell-sample-multi-tier-application.md) | Erstellt ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetzen. Der Datenverkehr am Front-End-Subnetz ist auf HTTP beschränkt, während der Datenverkehr zum Back-End-Subnetz auf SQL und Port 1433 beschränkt ist. |
| [Herstellen einer Peerverbindung zwischen zwei virtuellen Netzwerken](./scripts/virtual-network-powershell-sample-peer-two-virtual-networks.md) | Erstellt zwei virtuelle Netzwerke in derselben Region und stellt eine Verbindung zwischen diesen her. |
| [Weiterleiten von Datenverkehr über ein virtuelles Netzwerkgerät](./scripts/virtual-network-powershell-sample-route-traffic-through-nva.md) | Erstellt ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetzen und einer VM, die Datenverkehr zwischen den zwei Subnetzen weiterleiten kann. |
| [Filtern von ein- und ausgehenden VM-Netzwerkdatenverkehr](./scripts/virtual-network-powershell-sample-filter-network-traffic.md) | Erstellt ein virtuelles Netzwerk mit Front-End- und Back-End-Subnetzen. Der im Front-End-Subnetz eingehende Netzwerkdatenverkehr ist auf HTTP und HTTPS beschränkt. Aus dem Back-End-Subnetz an das Internet ausgehender Datenverkehr ist nicht zulässig. |
|[Konfigurieren eines virtuellen Dual-Stack-Netzwerks (IPv4 und IPv6)mit Load Balancer im Tarif „Basic“](./scripts/virtual-network-powershell-sample-ipv6-dual-stack.md)|Stellt ein virtuelles Dual-Stack-Netzwerk (IPv4 + IPv6) mit zwei virtuellen Computern und einem Azure Load Balancer Basic mit öffentlichen IPv4- und IPv6-Adressen bereit. |
|[Konfigurieren eines virtuellen Dual-Stack-Netzwerks (IPv4 und IPv6) mit Load Balancer Standard](./scripts/virtual-network-powershell-sample-ipv6-dual-stack-standard-load-balancer.md)|Stellt ein virtuelles Dual-Stack-Netzwerk (IPv4 und IPv6) mit zwei virtuellen Computern und einer Azure Load Balancer Standard-Instanz mit öffentlichen IPv4- und IPv6-Adressen bereit. |
