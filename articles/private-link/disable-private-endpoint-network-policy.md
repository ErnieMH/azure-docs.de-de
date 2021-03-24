---
title: Deaktivieren von Netzwerkrichtlinien für private Endpunkte in Azure
description: Erfahren Sie, wie Sie Netzwerkrichtlinien für private Endpunkte deaktivieren.
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: how-to
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: a4c04033f3fb5ff523f0d80bb6c978955f4c9ec2
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "84737511"
---
# <a name="disable-network-policies-for-private-endpoints"></a>Deaktivieren von Netzwerkrichtlinien für private Endpunkte

Netzwerkrichtlinien wie Netzwerksicherheitsgruppen (NSG) werden für private Endpunkte nicht unterstützt. Zum Bereitstellen eines privaten Endpunkts in einem bestimmten Subnetz ist eine explizite Einstellung zum Deaktivieren in diesem Subnetz erforderlich. Diese Einstellung gilt nur für den privaten Endpunkt. Für andere Ressourcen im Subnetz wird der Zugriff basierend auf der Definition von Sicherheitsregeln der Netzwerksicherheitsgruppen (NSG) gesteuert. 
 
Wenn Sie das Portal zum Erstellen eines privaten Endpunkts verwenden, wird diese Einstellung im Rahmen des Erstellungsprozesses automatisch deaktiviert. Für die Bereitstellung mit anderen Clients ist ein zusätzlicher Schritt erforderlich, um diese Einstellung zu ändern. Sie können die Einstellung mithilfe von Cloud Shell aus dem Azure-Portal oder lokalen Installationen von Azure PowerShell, der Azure CLI oder von Azure Resource Manager Vorlagen deaktivieren.  
 
In den folgenden Beispielen wird beschrieben, wie `PrivateEndpointNetworkPolicies` für ein virtuelles Netzwerk namens *myVirtualNetwork* mit einem *standardmäßigen* Subnetz in einer Ressourcengruppe namens *myResourceGroup* deaktiviert wird.

## <a name="using-azure-powershell"></a>Verwenden von Azure PowerShell
In diesem Abschnitt wird beschrieben, wie Sie Subnetzrichtlinien für private Endpunkte mit Azure PowerShell deaktivieren können.

```azurepowershell
$virtualNetwork= Get-AzVirtualNetwork `
  -Name "myVirtualNetwork" ` 
  -ResourceGroupName "myResourceGroup"  
   
($virtualNetwork | Select -ExpandProperty subnets | Where-Object  {$_.Name -eq 'default'} ).PrivateEndpointNetworkPolicies = "Disabled" 
 
$virtualNetwork | Set-AzVirtualNetwork 
```
## <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle
In diesem Abschnitt wird beschrieben, wie Sie Subnetzrichtlinien für private Endpunkte mit der Azure CLI deaktivieren können.
```azurecli
az network vnet subnet update \ 
  --name default \ 
  --resource-group myResourceGroup \ 
  --vnet-name myVirtualNetwork \ 
  --disable-private-endpoint-network-policies true
```
## <a name="using-a-template"></a>Verwenden einer Vorlage
In diesem Abschnitt wird beschrieben, wie Sie Subnetzrichtlinien für private Endpunkte mit einer Azure Resource Manager-Vorlage deaktivieren können.
```json
{ 
          "name": "myVirtualNetwork", 
          "type": "Microsoft.Network/virtualNetworks", 
          "apiVersion": "2019-04-01", 
          "location": "WestUS", 
          "properties": { 
                "addressSpace": { 
                     "addressPrefixes": [ 
                          "10.0.0.0/16" 
                        ] 
                  }, 
                  "subnets": [ 
                         { 
                                "name": "default", 
                                "properties": { 
                                    "addressPrefix": "10.0.0.0/24", 
                                    "privateEndpointNetworkPolicies": "Disabled" 
                                 } 
                         } 
                  ] 
          } 
} 
```
## <a name="next-steps"></a>Nächste Schritte
- Weitere Informationen zu [privaten Azure-Endpunkten](private-endpoint-overview.md)
 
