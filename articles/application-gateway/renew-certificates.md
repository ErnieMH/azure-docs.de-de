---
title: Verlängern des Azure Application Gateway-Zertifikats
description: Es wird beschrieben, wie Sie ein Zertifikat verlängern, das einem Anwendungsgateway-Listener zugeordnet ist.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: how-to
ms.date: 8/15/2018
ms.author: victorh
ms.openlocfilehash: de57a58f7c891009d2e0cc43b351c2cad42a2766
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84807874"
---
# <a name="renew-application-gateway-certificates"></a>Verlängern von Application Gateway-Zertifikaten

Sie müssen Zertifikate nach einiger Zeit verlängern, falls Sie für Ihr Anwendungsgateway die TLS/SSL-Verschlüsselung konfiguriert haben.

Sie können ein Zertifikat, das einem Listener zugeordnet ist, verlängern, indem Sie entweder das Azure-Portal, Azure PowerShell oder die Azure-Befehlszeilenschnittstelle verwenden:

## <a name="azure-portal"></a>Azure-Portal

Navigieren Sie zu Ihren Anwendungsgateway-Listenern, um ein Listenerzertifikat über das Portal zu verlängern. Klicken Sie auf den Listener, der über ein zu verlängerndes Zertifikat verfügt, und klicken Sie anschließend auf **Ausgewähltes Zertifikat erneuern oder bearbeiten**.

![Verlängern des Zertifikats](media/renew-certificate/ssl-cert.png)

Laden Sie Ihr neues PFX-Zertifikat hoch, geben Sie ihm einen Namen, geben Sie das Kennwort ein, und klicken Sie dann auf **Speichern**.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Verwenden Sie das folgende Skript, um Ihr Zertifikat mit Azure PowerShell zu verlängern:

```azurepowershell-interactive
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName <ResourceGroup> `
  -Name <AppGatewayName>

$password = ConvertTo-SecureString `
  -String "<password>" `
  -Force `
  -AsPlainText

set-AzApplicationGatewaySSLCertificate -Name <oldcertname> `
-ApplicationGateway $appgw -CertificateFile <newcertPath> -Password $password

Set-AzApplicationGateway -ApplicationGateway $appgw
```
## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

```azurecli-interactive
az network application-gateway ssl-cert update \
  -n "<CertName>" \
  --gateway-name "<AppGatewayName>" \
  -g "ResourceGroupName>" \
  --cert-file <PathToCerFile> \
  --cert-password "<password>"
```

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren der TLS-Abladung mit Azure Application Gateway finden Sie unter [Konfigurieren der TLS-Abladung](application-gateway-ssl-portal.md).
