---
title: 'Azure VPN Gateway: Installieren eines Point-to-Site-Clientzertifikats'
description: Installieren eines Clientzertifikats für die P2S-Zertifikatauthentifizierung – Windows, Mac, Linux.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/02/2020
ms.author: cherylmc
ms.openlocfilehash: 8c4d37f67d89ad1b7973a85fb6c2a9f7e818a028
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "89421661"
---
# <a name="install-client-certificates-for-p2s-certificate-authentication-connections"></a>Installieren eines Clientzertifikats für die Zertifikatauthentifizierung bei P2S-Verbindungen

Alle Clients, die mithilfe der P2S-Azure-Zertifikatauthentifizierung eine Verbindung mit virtuellen Netzwerken herstellen, benötigen ein Clientzertifikat. Dieser Artikel unterstützt Sie beim Installieren eines Clientzertifikats, das beim Herstellen einer Verbindung mit einem VNET über P2S für die Authentifizierung verwendet wird.

## <a name="acquire-a-client-certificate"></a><a name="generate"></a>Abrufen eines Clientzertifikats

Unabhängig davon, über welches Clientbetriebssystem Sie eine Verbindung herstellen möchten, benötigen Sie immer ein Clientzertifikat. Sie können ein Clientzertifikat mithilfe eines Stammzertifikats, das mit einer Lösung einer Unternehmenszertifizierungsstelle generiert wurde, oder mithilfe eines selbstsignierten Zertifikats erstellen. Die Schritte zum Generieren eines Clientzertifikats finden Sie in den Anweisungen für [PowerShell](vpn-gateway-certificates-point-to-site.md), [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) und [Linux](vpn-gateway-certificates-point-to-site-linux.md). 

## <a name="windows"></a><a name="installwin"></a>Windows

[!INCLUDE [Install on Windows](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="mac"></a><a name="installmac"></a>Mac

>[!NOTE]
>Mac-VPN-Clients werden nur für das Resource Manager-Bereitstellungsmodell unterstützt. Für das klassische Bereitstellungsmodell werden sie nicht unterstützt.
>
>

[!INCLUDE [Install on Mac](../../includes/vpn-gateway-certificates-install-mac-client-cert-include.md)]

## <a name="linux"></a><a name="installlinux"></a>Linux

Das Linux-Clientzertifikat wird auf dem Client als Teil der Clientkonfiguration installiert. Anweisungen finden Sie unter [Clientkonfiguration – Linux](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli).

## <a name="next-steps"></a>Nächste Schritte

Fahren Sie mit den Point-to-Site-Konfigurationsschritten zum [Erstellen und Installieren von VPN-Clientkonfigurationsdateien](point-to-site-vpn-client-configuration-azure-cert.md) fort.
