---
title: Erstellen einer Skalierungsgruppe, die Azure Spot-VMS nutzt
description: Erfahren Sie, wie Sie Azure-VM-Skalierungsgruppen erstellen, die Spot-VMs nutzen, um Kosten zu sparen.
author: cynthn
ms.author: cynthn
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: spot
ms.date: 03/25/2020
ms.reviewer: jagaveer
ms.custom: jagaveer, devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 4c5386e2fad0ebdd30ca8f9a8f4933e8adaf5d6b
ms.sourcegitcommit: 638f326d02d108cf7e62e996adef32f2b2896fd5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729014"
---
# <a name="azure-spot-vms-for-virtual-machine-scale-sets"></a>Azure-Spot-VMs für VM-Skalierungsgruppen 

Wenn Sie Azure Spot-VMs für Skalierungsgruppen verwenden, profitieren Sie von unserer ungenutzten Kapazität und erzielen wesentliche Kosteneinsparungen. Wenn die Kapazität von Azure wieder benötigt wird, werden die Spot-Instanzen durch die Azure-Infrastruktur entfernt. Aus diesem Grund eignen sich Spot-Instanzen hervorragend für Workloads, die Unterbrechungen tolerieren, z. B. Batchverarbeitungsaufträge, Dev/Test-Umgebungen, umfangreiche Computeworkloads und mehr.

Die verfügbare Kapazität kann abhängig von der Größe, Region, Tageszeit usw. variieren. Beim Bereitstellen von Spot-Instanzen für Skalierungsgruppen wird die Instanz von Azure nur zugeordnet, wenn Kapazität verfügbar ist, für diese Instanzen aber keine SLA besteht. Eine Spot-Skalierungsgruppe wird in einer einzelnen Fehlerdomäne bereitgestellt und gewährleistet keine Hochverfügbarkeit.


## <a name="pricing"></a>Preise

Die Preise für Spot-Instanzen variieren je nach Region und SKU. Weitere Informationen finden Sie unter den Preisen für [Linux](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) und [Windows](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/windows/). 


Bei der variablen Preisgestaltung können Sie einen maximalen Preis in US-Dollar (USD) mit bis zu 5 Dezimalstellen festlegen. Der Wert `0.98765` würde beispielsweise einem maximalen Preis von 0,98765 US-Dollar pro Stunde entsprechen. Wenn Sie den maximalen Preis auf `-1` festlegen, wird die Instanz nicht basierend auf dem Preis entfernt. Der Preis für die Instanz entspricht dem aktuellen Preis für Spot-Instanzen oder dem Preis für eine Standardinstanz, je nachdem, welcher Preis niedriger ist, solange Kapazität und Kontingente verfügbar sind.

## <a name="eviction-policy"></a>Entfernungsrichtlinie

Wenn Sie Spot-Skalierungsgruppen erstellen, können Sie die Entfernungsrichtlinie auf *Zuordnung aufheben* (Standardeinstellung) oder *Löschen* festlegen. 

Durch die Richtlinie *Zuordnung aufheben* werden die entfernten Instanzen in den Zustand „Beendet/Zuordnung aufgehoben“ versetzt, sodass Sie entfernte Instanzen erneut bereitstellen können. Es gibt jedoch keine Garantie dafür, dass die Zuordnung erfolgreich ist. Die virtuellen Computer, deren Zuordnung aufgehoben wurde, werden auf Ihr Kontingent an Skalierungsgruppeninstanzen angerechnet, und die zugrunde liegenden Datenträger werden Ihnen in Rechnung gestellt. 

Wenn die Instanzen in Ihrer Spot-Skalierungsgruppe beim Entfernen gelöscht werden sollen, können Sie die Entfernungsrichtlinie auf *Löschen* festlegen. Wenn die Entfernungsrichtlinie zum Löschen festgelegt ist, können Sie neue VMs durch Heraufsetzen der Skalierungsgruppeninstanzenanzahl-Eigenschaft erstellen. Die entfernten VMs werden zusammen mit ihren zugrunde liegenden Datenträgern gelöscht, und darum fallen keine Kosten für ihre Speicherung an. Sie können auch die Funktion zur automatischen Skalierung von Skalierungsgruppen verwenden, um zu versuchen, entfernte VMs automatisch zu kompensieren, es gibt jedoch keine Garantie, dass die Zuordnung erfolgreich ist. Die Funktion für die automatische Skalierung sollte nur für Spot-Skalierungsgruppen verwendet werden, wenn Sie die Entfernungsrichtlinie auf „Löschen“ festlegen. So vermeiden Sie Kosten für Datenträger und das Überschreiten von Kontingentgrenzen. 

Benutzer können sich für den Empfang von Benachrichtigungen in der VM über [Azure Scheduled Events](../virtual-machines/linux/scheduled-events.md) anmelden. Dadurch werden Sie benachrichtigt, wenn Ihre virtuellen Computer entfernt werden, und Sie haben vor dem Entfernen 30 Sekunden Zeit, Aufträge abzuschließen und die VMs herunterzufahren. 

## <a name="placement-groups"></a>Platzierungsgruppen
Eine Platzierungsgruppe ist ein ähnliches Konstrukt wie eine Azure-Verfügbarkeitsgruppe und verfügt über eigene Fehler- und Upgradedomänen. Standardmäßig besteht eine Skalierungsgruppe aus einer einzelnen Platzierungsgruppe mit einer maximalen Größe von 100 virtuellen Computern. Wenn die Skalierungsgruppeneigenschaft `singlePlacementGroup` auf *false* festgelegt ist, kann die Skalierungsgruppe mehrere Platzierungsgruppen und bis zu 1.000 virtuelle Computer umfassen. 

> [!IMPORTANT]
> Sofern Sie nicht InfiniBand mit HPC verwenden, wird dringend empfohlen, die Skalierungsgruppeneigenschaft `singlePlacementGroup` auf *false* festzulegen, um mehrere Platzierungsgruppen für eine bessere Skalierung in der Region oder Zone zu ermöglichen. 

## <a name="deploying-spot-vms-in-scale-sets"></a>Bereitstellen von Spot-VMs in Skalierungsgruppen

Zum Bereitstellen von Spot-VMs in Skalierungsgruppen können Sie das neue *Priority*-Flag auf *Spot* festlegen. Für alle VMs in Ihrer Skalierungsgruppe wird „Spot“ festgelegt. Verwenden Sie zum Erstellen einer Skalierungsgruppe mit Spot-VMs eine der folgenden Methoden:
- [Azure portal](#portal)
- [Azure-Befehlszeilenschnittstelle](#azure-cli)
- [Azure PowerShell](#powershell)
- [Azure-Ressourcen-Manager-Vorlagen](#resource-manager-templates)

## <a name="portal"></a>Portal

Das Erstellen einer Skalierungsgruppe, die Spot-VMs nutzt, entspricht den Ausführungen im [Artikel „Erste Schritte“](quick-create-portal.md). Wenn Sie eine Skalierungsgruppe bereitstellen, können Sie das Spot-Flag und die Entfernungsrichtlinie festlegen: ![Erstellen einer Skalierungsgruppe mit Spot-VMs](media/virtual-machine-scale-sets-use-spot/vmss-spot-portal-max-price.png)


## <a name="azure-cli"></a>Azure CLI

Das Erstellen einer Skalierungsgruppe mit Spot-VMs entspricht den Ausführungen im [Artikel „Erste Schritte“](quick-create-cli.md). Fügen Sie einfach „--Priority Spot“ und `--max-price` hinzu. In diesem Beispiel verwenden wir `-1` für `--max-price`, sodass die Instanz nicht basierend auf dem Preis entfernt wird.

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --single-placement-group false \
    --admin-username azureuser \
    --generate-ssh-keys \
    --priority Spot \
    --max-price -1 
```

## <a name="powershell"></a>PowerShell

Das Erstellen einer Skalierungsgruppe mit Spot-VMs entspricht den Ausführungen im [Artikel „Erste Schritte“](quick-create-powershell.md).
Fügen Sie einfach „-Priority Spot“ hinzu, und geben Sie `-max-price` für [New-AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig) an.

```powershell
$vmssConfig = New-AzVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Priority "Spot" `
    --max-price -1
```

## <a name="resource-manager-templates"></a>Resource Manager-Vorlagen

Das Erstellen einer Skalierungsgruppe, die Spot-VMs nutzt, entspricht den Ausführungen im Artikel „Erste Schritte“ für [Linux](quick-create-template-linux.md) oder [Windows](quick-create-template-windows.md). 

Verwenden Sie `"apiVersion": "2019-03-01"` oder höher für Spot-Vorlagenbereitstellungen. 

Fügen Sie dem Abschnitt `"virtualMachineProfile":` in Ihrer Vorlage die Eigenschaften `priority`, `evictionPolicy` und `billingProfile` und dem Abschnitt `"Microsoft.Compute/virtualMachineScaleSets"` die Eigenschaft `"singlePlacementGroup": false,` hinzu:

```json

{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  },
  "properties": {
    "singlePlacementGroup": false,
    }

        "virtualMachineProfile": {
              "priority": "Spot",
                "evictionPolicy": "Deallocate",
                "billingProfile": {
                    "maxPrice": -1
                }
            },
```

Um die Instanz nach dem Entfernen zu löschen, ändern Sie den `evictionPolicy`-Parameter in `Delete`.

## <a name="faq"></a>Häufig gestellte Fragen

**F:** Ist eine Spot-Instanz nach der Erstellung identisch mit einer Standardinstanz?

**A:** Ja, mit der Ausnahme, dass für Spot-VMs keine SLA besteht und dass sie jederzeit entfernt werden können.


**F:** Was kann ich tun, wenn ich nach dem Entfernen immer noch Kapazität benötige?

**A:** Es wird empfohlen, anstelle von Spot-VMs Standard-VMs zu verwenden, wenn Sie sofort Kapazität benötigen.


**F:** Wie werden Kontingente für Spot-Instanzen verwaltet?

**A:** Für Spot-Instanzen und Standardinstanzen werden getrennte Kontingentpools verwendet. Das Spotkontingent wird von virtuellen Computern und Skalierungsgruppeninstanzen gemeinsam genutzt. Weitere Informationen finden Sie unter [Grenzwerte für Azure-Abonnements, -Dienste und -Kontingente sowie allgemeine Beschränkungen](../azure-resource-manager/management/azure-subscription-service-limits.md).


**F:** Kann ich ein zusätzliches Kontingent für Spot anfordern?

**A:** Ja, Sie können eine Anforderung zur Erhöhung Ihres Kontingents für Spot-VMs über den [Standard-Kontingentanforderungsprozess](../azure-portal/supportability/per-vm-quota-requests.md) übermitteln.


**F:** Kann ich vorhandene Skalierungsgruppen in Spot-Skalierungsgruppen konvertieren?

**A:** Nein. Das `Spot`-Flag kann nur zum Zeitpunkt der Erstellung festgelegt werden.


**F:** Wenn ich für Skalierungsgruppen mit niedriger Priorität `low` verwende, muss ich ab jetzt stattdessen `Spot` verwenden?

**A:** Zum jetzigen Zeitpunkt funktionieren sowohl `low` als auch `Spot`, Sie sollten aber mit der Umstellung auf `Spot` beginnen.


**F:** Kann ich eine Skalierungsgruppe sowohl mit regulären VMs als auch mit Spot-VMs erstellen?

**A:** Nein. Eine Skalierungsgruppe kann nicht mehr als einen Prioritätstyp unterstützen.


**F:**  Kann ich die automatische Skalierung für Spot-Skalierungsgruppen verwenden?

**A:** Ja. Sie können Regeln für die automatische Skalierung für Ihre Spot-Skalierungsgruppe festlegen. Wenn Ihre VMs entfernt werden, kann von der Funktion für die automatische Skalierung versucht werden, neue Spot-VMs zu erstellen. Denken Sie aber daran: Dieser Kapazität ist nicht garantiert. 


**F:**  Funktioniert die automatische Skalierung mit beiden Entfernungsrichtlinien (Zuordnung aufheben und löschen)?

**A:** Ja, es empfiehlt sich jedoch, die Entfernungsrichtlinie auf „Löschen“ festzulegen, wenn Sie die Autoskalierung verwenden. Der Grund hierfür ist, dass Instanzen, deren Zuordnung aufgehoben wurde, auf die Kapazität in Ihrer Skalierungsgruppe angerechnet werden. Bei Verwendung der automatischen Skalierung erreichen Sie aufgrund der entfernten Instanzen, deren Zuordnung aufgehoben wurde, die angestrebte Anzahl von Instanzen wahrscheinlich recht schnell. Außerdem kann die Spot-Entfernung Auswirkungen auf Ihre Skalierungsvorgänge haben. Beispielsweise kann es vorkommen, dass VMSS-Instanzen bei Skalierungsvorgängen aufgrund verschiedener Spot-Entfernungen unter die festgelegte Mindestanzahl fallen. 

**F:** Welche Kanäle unterstützen Spot-VMs?

**A:** In der folgenden Tabelle finden Sie Informationen zur Verfügbarkeit von Spot-VMs.

<a name="channel"></a>

| Azure-Kanäle               | Azure-Spot-VM-Verfügbarkeit       |
|------------------------------|-----------------------------------|
| Enterprise Agreement         | Ja                               |
| Nutzungsbasierte Bezahlung                | Ja                               |
| Clouddienstanbieter | [Kontaktieren Sie Ihren Partner.](/partner-center/azure-plan-get-started) |
| Vorteile                     | Nicht verfügbar                     |
| Sponsoren                    | Ja                               |
| Kostenlose Testversion                   | Nicht verfügbar                     |


**F:** Wo kann ich Fragen stellen?

**A:** Sie können Ihre Frage in [Q&A](/answers/topics/azure-spot.html) veröffentlichen und mit `azure-spot` markieren. 

## <a name="next-steps"></a>Nächste Schritte

Detaillierte Preisinformationen finden Sie auf der Seite [Linux VM-Skalierungsgruppen – Preise ](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/).
