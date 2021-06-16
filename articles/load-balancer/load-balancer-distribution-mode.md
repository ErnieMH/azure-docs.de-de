---
title: Konfigurieren des Azure Load Balancer-Verteilungsmodus
titleSuffix: Azure Load Balancer
description: In diesem Artikel werden die ersten Schritte zum Konfigurieren des Verteilungsmodus für Azure Load Balancer zur Unterstützung von Quell-IP-Affinität beschrieben.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: how-to
ms.custom: seodec18, devx-track-azurecli, devx-track-azurepowershell
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2021
ms.author: allensu
ms.openlocfilehash: 3cc3470b31c6626a0412c1f5d2e3e899808c316a
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110700279"
---
# <a name="configure-the-distribution-mode-for-azure-load-balancer"></a>Konfigurieren des Verteilungsmodus für Azure Load Balancer

Azure Load Balancer unterstützt zwei Verteilungsmodi für die Verteilung des Datenverkehrs auf Ihre Anwendungen:

* Hashbasiert
* Quell-IP-Affinität

In diesem Artikel erfahren Sie, wie Sie den Verteilungsmodus für Ihre Azure Load Balancer-Instanz konfigurieren.


## <a name="configure-distribution-mode"></a>Konfigurieren des Verteilungsmodus

---

# <a name="azure-portal"></a>[**Azure-Portal**](#tab/azure-portal)

Sie können die Konfiguration des Verteilungsmodus ändern, indem Sie die Lastenausgleichsregel im Portal ändern.

1. Melden Sie sich beim Azure-Portal an, und suchen Sie die Ressourcengruppe, die die Load Balancer-Instanz enthält, die Sie ändern möchten, indem Sie auf **Ressourcengruppen** klicken.
2. Klicken Sie auf dem Bildschirm mit der Übersicht über die Load Balancer-Instanz unter **Einstellungen** auf **Lastenausgleichsregeln**.
3. Klicken Sie auf dem Bildschirm mit den Load Balancer-Regeln auf die Load Balancer-Regel, für die Sie den Verteilungsmodus ändern möchten.
4. Der Verteilungsmodus wird unter der Regel geändert, indem Sie das Dropdownfeld **Sitzungspersistenz** geändert wird. 

Die folgenden Optionen sind verfügbar: 

* **None (hash-based)** (Keine (Hash-basiert)): Gibt an, dass aufeinander folgende Anforderungen vom selben Client von jedem virtuellen Computer verarbeitet werden können.
* **Client IP (source IP affinity two-tuple)** (Client-IP (Quell-IP-Affinität 2-Tupel)): gibt an, dass aufeinanderfolgende Anforderungen von derselben Client-IP-Adresse vom selben virtuellen Computer verarbeitet werden
* **Client IP and protocol (source IP affinity three-tuple)** (Client-IP und Protokoll (Quell-IP-Affinität 3-Tupel)): gibt an, dass aufeinanderfolgende Anforderungen von derselben Kombination aus Client-IP-Adresse und Protokoll vom selben virtuellen Computer verarbeitet werden

5. Wählen Sie den Verteilungsmodus aus, und klicken Sie auf **Speichern**.

:::image type="content" source="./media/load-balancer-distribution-mode/session-persistence.png" alt-text="Ändern der Sitzungspersistenz für die Load Balancer-Regel" border="true":::


# <a name="powershell"></a>[**PowerShell**](#tab/azure-powershell)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Verwenden Sie PowerShell, um die Load Balancer-Verteilungseinstellungen für eine vorhandene Load Balancer-Regel zu ändern. Der folgende Befehl aktualisiert den Verteilungsmodus: 

```azurepowershell-interactive
$lb = Get-AzLoadBalancer -Name MyLoadBalancer -ResourceGroupName MyResourceGroupLB
$lb.LoadBalancingRules[0].LoadDistribution = 'default'
Set-AzLoadBalancer -LoadBalancer $lb
```

Legen Sie den Wert des `LoadDistribution`-Elements auf den Typ des erforderlichen Lastenausgleichs fest. 

* Legen Sie **SourceIP** auf den Zwei-Tupel-Lastenausgleich (Quell- und Ziel-IP) fest. 

* Legen Sie **SourceIPProtocol** auf den Drei-Tupel-Lastenausgleich (Quell-IP, Ziel-IP und Protokolltyp) fest. 

* Geben Sie **Default** an, um das Standardverhalten, also den Fünf-Tupel-Lastenausgleich, zu verwenden.

# <a name="cli"></a>[**BEFEHLSZEILENSCHNITTSTELLE (CLI)**](#tab/azure-cli)

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

Verwenden Sie die Azure CLI, um die Load Balancer-Verteilungseinstellungen für eine vorhandene Load Balancer-Regel zu ändern.  Der folgende Befehl aktualisiert den Verteilungsmodus:

```azurecli-interactive
az network lb rule update \
    --lb-name myLoadBalancer \
    --load-distribution Default \
    --name myHTTPRule \
    --resource-group myResourceGroupLB 
```
Legen Sie den Wert von `--load-distribution` auf den Typ des erforderlichen Lastenausgleichs fest.

* Legen Sie **SourceIP** auf den Zwei-Tupel-Lastenausgleich (Quell- und Ziel-IP) fest. 

* Legen Sie **SourceIPProtocol** auf den Drei-Tupel-Lastenausgleich (Quell-IP, Ziel-IP und Protokolltyp) fest. 

* Geben Sie **Default** an, um das Standardverhalten, also den Fünf-Tupel-Lastenausgleich, zu verwenden.

Weitere Informationen zu dem in diesem Artikel verwendeten Befehl finden Sie unter [az network lb rule update](/cli/azure/network/lb/rule#az_network_lb_rule_update).

---

## <a name="next-steps"></a>Nächste Schritte

* [Übersicht über Azure Load Balancer](load-balancer-overview.md)
* [Erste Schritte zum Konfigurieren eines Lastenausgleichs mit Internetzugriff](quickstart-load-balancer-standard-public-powershell.md)
* [Konfigurieren von TCP-Leerlauftimeout-Einstellungen für den Lastenausgleich](load-balancer-tcp-idle-timeout.md)
