---
title: 'Generieren und Exportieren von Zertifikaten für P2S: PowerShell'
titleSuffix: Azure VPN Gateway
description: Erfahren Sie, wie Sie ein selbstsigniertes Stammzertifikat erstellen, einen öffentlichen Schlüssel exportieren und Clientzertifikate für VPN Gateway Point-to-Site-Verbindungen generieren.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 04/28/2021
ms.author: cherylmc
ms.openlocfilehash: 3a0d8303f74da60f5acaa674947129aade508ffd
ms.sourcegitcommit: a5dd9799fa93c175b4644c9fe1509e9f97506cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108202459"
---
# <a name="generate-and-export-certificates-for-point-to-site-using-powershell"></a>Generieren und Exportieren von Zertifikaten für Punkt-zu-Standort-Verbindungen mithilfe von PowerShell

Punkt-zu-Standort-Verbindungen verwenden Zertifikate zur Authentifizierung. In diesem Artikel wird beschrieben, wie Sie mithilfe von PowerShell unter Windows 10 oder Windows Server 2016 ein selbstsigniertes Stammzertifikat erstellen und Clientzertifikate generieren. Anleitungen für andere Zertifikate finden Sie unter [Zertifikate – Linux](vpn-gateway-certificates-point-to-site-linux.md) oder [Zertifikate – MakeCert](vpn-gateway-certificates-point-to-site-makecert.md).

Sie müssen die in diesem Artikel beschriebenen Schritte auf einem Computer unter Windows 10 oder Windows Server 2016 ausführen. Die zum Generieren von Zertifikaten verwendeten PowerShell-Cmdlets sind Teil des Betriebssystems und können in anderen Versionen von Windows nicht ausgeführt werden. Der Windows 10- oder Windows Server 2016-Computer wird lediglich zum Generieren der Zertifikate benötigt. Nachdem die Zertifikate generiert wurden, können sie hochgeladen oder unter jedem unterstützten Clientbetriebssystem installiert werden.

Wenn Sie keinen Zugriff auf einen Windows 10- oder Windows Server 2016-Computer haben, können Sie Zertifikate mit [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) generieren. Unabhängig davon, mit welcher Methode sie generiert wurden, können die Zertifikate unter jedem [unterstützten](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) Clientbetriebssystem installiert werden.

[!INCLUDE [generate and export certificates](../../includes/vpn-gateway-generate-export-certificates-include.md)]

## <a name="install-an-exported-client-certificate"></a><a name="install"></a>Installieren eines exportierten Clientzertifikats

Jeder Client, der über eine P2S-Verbindung eine Verbindung mit dem VNet herstellt, erfordert, dass ein Clientzertifikat lokal installiert ist.

Informationen zum Installieren eines Clientzertifikats finden Sie unter [Installieren eines Clientzertifikats für Point-to-Site-Verbindungen mit Azure-Zertifikatauthentifizierung](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="next-steps"></a>Nächste Schritte

Setzen Sie die Punkt-zu-Standort-Konfiguration fort.

* Schritte für das **Resource Manager**-Bereitstellungsmodell finden Sie unter [Konfigurieren einer Point-to-Site-Verbindung mit einem VNET unter Verwendung der Zertifikatauthentifizierung: Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md).
* Schritte für das **klassische** Bereitstellungsmodell finden Sie unter [Konfigurieren einer Point-to-Site-Verbindung mit einem VNet über das Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md) (klassisch).