---
title: 'Ändern der Einstellungen für die Gateway-IP-Adresse: Azure CLI'
titleSuffix: Azure VPN Gateway
description: Hier erfahren Sie, wie Sie IP-Adresspräfixe für Ihr lokales Netzwerkgateway mithilfe der Azure CLI ändern können.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/02/2020
ms.author: cherylmc
ms.openlocfilehash: 5e03da37a727c87995496878b191cbf48cfe0347
ms.sourcegitcommit: 49bd8e68bd1aff789766c24b91f957f6b4bf5a9b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2021
ms.locfileid: "108229610"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-cli"></a>Ändern der Einstellungen des lokalen Netzwerkgateways mithilfe der Azure CLI

Manchmal ändern sich die Einstellungen für das Adresspräfix oder die Gateway-IP-Adresse Ihres lokalen Netzwerkgateways. In diesem Artikel wird gezeigt, wie Sie die Einstellungen Ihres lokalen Netzwerkgateways ändern. Sie können diese Einstellungen auch über eine andere Option aus der folgenden Liste ändern:

> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Azure-Befehlszeilenschnittstelle](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <a name="before-you-begin"></a><a name="before"></a>Voraussetzungen

Installieren Sie die aktuelle Version der CLI-Befehle (2.0 oder höher). Informationen zur Installation der CLI-Befehle finden Sie unter [Installieren von Azure CLI](/cli/azure/install-azure-cli).

[!INCLUDE [CLI-login](../../includes/vpn-gateway-cli-login-include.md)]

## <a name="modify-ip-address-prefixes"></a><a name="ipaddprefix"></a>Ändern von IP-Adresspräfixen

[!INCLUDE [modify-prefix](../../includes/vpn-gateway-modify-ip-prefix-cli-include.md)]

## <a name="modify-the-gateway-ip-address"></a><a name="gwip"></a>Ändern der Gateway-IP-Adresse

[!INCLUDE [modify-gateway-IP](../../includes/vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie können die Gatewayverbindung überprüfen. Informationen finden Sie unter [Überprüfen einer Gatewayverbindung](vpn-gateway-verify-connection-resource-manager.md).