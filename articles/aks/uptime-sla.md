---
title: Azure Kubernetes Service (AKS) mit Betriebszeit-SLA
description: Erfahren Sie mehr über das optionale Betriebszeit-SLA-Angebot für den Azure Kubernetes Service (AKS)-API-Server.
services: container-service
ms.topic: conceptual
ms.date: 01/08/2021
ms.custom: references_regions, devx-track-azurecli
ms.openlocfilehash: 846446b4c19c066afe789bf636d68ad37b20709e
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107779559"
---
# <a name="azure-kubernetes-service-aks-uptime-sla"></a>Azure Kubernetes Service (AKS): Betriebszeit-SLA

Betriebszeit-SLA ist ein optionales Feature, das eine finanziell abgesicherte, höhere SLA für einen Cluster ermöglicht. Betriebszeit-SLA garantiert eine Verfügbarkeit von 99,95 % des Kubernetes-API-Serverendpunkts für Cluster, die [Verfügbarkeitszonen][availability-zones] verwenden, und eine Verfügbarkeit von 99,9 % für Cluster, die keine Verfügbarkeitszonen verwenden. AKS verwendet Masterknotenreplikate über Update- und Fehlerdomänen hinweg, um sicherzustellen, dass die SLA-Anforderungen erfüllt werden.

Kunden, die eine SLA benötigen, um Complianceanforderungen zu erfüllen, oder die eine Erweiterung einer SLA auf ihre Endbenutzer erfordern, sollten dieses Feature aktivieren. Kunden mit kritischen Workloads, die von einer höheren Betriebszeit-SLA profitieren, haben möglicherweise ebenfalls Vorteile. Die Verwendung der Betriebszeit-SLA mit Verfügbarkeitszonen ermöglicht eine höhere Verfügbarkeit für die Betriebszeit des Kubernetes-API-Servers.  

Kunden können nach wie vor unbegrenzt kostenlose Cluster mit einem Servicelevelziel (SLO) von 99,5 % erstellen und sich je nach Bedarf für die bevorzugte SLO- oder SLA-Betriebszeit entscheiden.

> [!Important]
> Für Cluster mit ausgehender Sperrung finden Sie unter [Einschränken des ausgehenden Datenverkehrs](limit-egress-traffic.md) Informationen zum Öffnen entsprechender Ports.

## <a name="region-availability"></a>Regionale Verfügbarkeit

* Die Uptime-SLA ist in öffentlichen Regionen und Azure Government-Regionen mit [AKS-Unterstützung](https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service) verfügbar.
* Die Uptime-SLA ist für [private AKS-Cluster][private-clusters] in allen öffentlichen Regionen verfügbar, in denen AKS unterstützt wird.

## <a name="sla-terms-and-conditions"></a>SLA-Geschäftsbedingungen

Die Betriebszeit-SLA ist ein kostenpflichtiges Feature und wird pro Cluster aktiviert. Die Preise für die Betriebszeit-SLA werden durch die Anzahl der separaten Cluster und nicht durch die Größe der einzelnen Cluster bestimmt. Weitere Informationen finden Sie unter [Preisdetails zur Betriebszeit-SLA](https://azure.microsoft.com/pricing/details/kubernetes-service/).

## <a name="before-you-begin"></a>Voraussetzungen

* Installieren Sie mindestens die Version 2.8.0 der [Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli).

## <a name="creating-a-new-cluster-with-uptime-sla"></a>Erstellen eines neuen Clusters mit Betriebszeit-SLA

Um einen neuen Cluster mit Betriebszeit-SLA zu erstellen, verwenden Sie die Azure CLI.

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus* erstellt:

```azurecli-interactive
# Create a resource group
az group create --name myResourceGroup --location eastus
```
Erstellen Sie mithilfe des Befehls [`az aks create`][az-aks-create] einen AKS-Cluster. Im folgenden Beispiel wird ein Cluster mit dem Namen *myAKSCluster* mit einem Knoten erstellt. Dieser Vorgang dauert einige Minuten:

```azurecli-interactive
# Create an AKS cluster with uptime SLA
az aks create --resource-group myResourceGroup --name myAKSCluster --uptime-sla --node-count 1
```
Nach wenigen Minuten ist die Ausführung des Befehls abgeschlossen, und es werden Informationen zum Cluster im JSON-Format zurückgegeben. Der folgende JSON-Codeausschnitt enthält den kostenpflichtigen Tarif für die SKU, was darauf hindeutet, dass die Betriebszeit-SLA für Ihren Cluster aktiviert ist:

```output
  },
  "sku": {
    "name": "Basic",
    "tier": "Paid"
  },
```

## <a name="modify-an-existing-cluster-to-use-uptime-sla"></a>Ändern eines vorhandenen Clusters für die Verwendung der Betriebszeit-SLA

Bereits vorhandene Cluster können optional aktualisiert werden, um die Betriebszeit-SLA zu verwenden.

Falls Sie mit den oben genannten Schritten einen AKS-Cluster erstellt haben, löschen Sie die Ressourcengruppe:

```azurecli-interactive
# Delete the existing cluster by deleting the resource group 
az group delete --name myResourceGroup --yes --no-wait
```

Erstellen Sie eine neue Ressourcengruppe:

```azurecli-interactive
# Create a resource group
az group create --name myResourceGroup --location eastus
```

Erstellen Sie einen neuen Cluster ohne Betriebszeit-SLA:

```azurecli-interactive
# Create a new cluster without uptime SLA
az aks create --resource-group myResourceGroup --name myAKSCluster--node-count 1
```

Verwenden Sie den Befehl [`az aks update`][az-aks-update], um den vorhandenen Cluster zu aktualisieren:

```azurecli-interactive
# Update an existing cluster to use Uptime SLA
 az aks update --resource-group myResourceGroup --name myAKSCluster --uptime-sla
 ```

 Der folgende JSON-Codeausschnitt enthält den kostenpflichtigen Tarif für die SKU, was darauf hindeutet, dass die Betriebszeit-SLA für Ihren Cluster aktiviert ist:

 ```output
  },
  "sku": {
    "name": "Basic",
    "tier": "Paid"
  },
  ```

## <a name="opt-out-of-uptime-sla"></a>Abmelden von der Uptime-SLA

Sie können Ihren Cluster aktualisieren, um zum kostenlosen Tarif zu wechseln und sich von der Uptime-SLA abzumelden.

```azurecli-interactive
# Update an existing cluster to opt out of Uptime SLA
 az aks update --resource-group myResourceGroup --name myAKSCluster --no-uptime-sla
 ```

## <a name="clean-up"></a>Bereinigung

Bereinigen Sie alle erstellten Ressourcen, um Kosten zu vermeiden. Wenn Sie den Cluster löschen möchten, löschen Sie die AKS-Ressourcengruppe mithilfe des Befehls [`az group delete`][az-group-delete]:

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```


## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie [Verfügbarkeitszonen][availability-zones], um die Hochverfügbarkeit Ihrer AKS-Clusterworkloads zu erhöhen.

Konfigurieren Sie den Cluster so, dass der [ausgehende Datenverkehr eingeschränkt](limit-egress-traffic.md) wird.

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
[region-availability]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service

<!-- LINKS - Internal -->
[vm-skus]: ../virtual-machines/sizes.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[faq]: ./faq.md
[availability-zones]: ./availability-zones.md
[az-aks-create]: /cli/azure/aks?#az_aks_create
[limit-egress-traffic]: ./limit-egress-traffic.md
[az-extension-add]: /cli/azure/extension#az_extension_add
[az-extension-update]: /cli/azure/extension#az_extension_update
[az-aks-update]: /cli/azure/aks#az_aks_update
[az-group-delete]: /cli/azure/group#az_group_delete
[private-clusters]: private-clusters.md
