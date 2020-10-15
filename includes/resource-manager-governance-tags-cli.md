---
title: include file
description: include file
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/20/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: c1259584e91461865b0c7e7bbbd6aced1781827b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "67708439"
---
Verwenden Sie zum Hinzufügen von zwei Tags zu einer Ressourcengruppe den Befehl [az group update](/cli/azure/group):

```azurecli-interactive
az group update -n myResourceGroup --set tags.Environment=Test tags.Dept=IT
```

Angenommen, Sie möchten ein drittes Tag hinzufügen. Führen Sie den Befehl erneut mit dem neuen Tag aus. Es wird an die vorhandenen Tags angefügt.

```azurecli-interactive
az group update -n myResourceGroup --set tags.Project=Documentation
```

Ressourcen erben keine Tags aus der Ressourcengruppe. Derzeit enthält Ihre Ressourcengruppe drei Tags, die Ressourcen besitzen jedoch keine Tags. Verwenden Sie das folgende Skript, um alle Tags einer Ressourcengruppe auf die darin enthaltenen Ressourcen anzuwenden und vorhandene Tags für Ressourcen beizubehalten:

```azurecli-interactive
# Get the tags for the resource group
jsontag=$(az group show -n myResourceGroup --query tags)

# Reformat from JSON to space-delimited and equals sign
t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')

# Get the resource IDs for all resources in the resource group
r=$(az resource list -g myResourceGroup --query [].id --output tsv)

# Loop through each resource ID
for resid in $r
do
  # Get the tags for this resource
  jsonrtag=$(az resource show --id $resid --query tags)
  
  # Reformat from JSON to space-delimited and equals sign
  rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
  
  # Reapply the updated tags to this resource
  az resource tag --tags $t$rt --id $resid
done
```

Alternativ können Sie Tags aus der Ressourcengruppe auf die Ressourcen anwenden, ohne die vorhandenen Tags beizubehalten:

```azurecli-interactive
# Get the tags for the resource group
jsontag=$(az group show -n myResourceGroup --query tags)

# Reformat from JSON to space-delimited and equals sign
t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')

# Get the resource IDs for all resources in the resource group
r=$(az resource list -g myResourceGroup --query [].id --output tsv)

# Loop through each resource ID
for resid in $r
do
  # Apply tags from resource group to this resource
  az resource tag --tags $t --id $resid
done
```

Kombinieren Sie mehrere Werte in einem einzelnen Tag mithilfe einer JSON-Zeichenfolge.

```azurecli-interactive
az group update -n myResourceGroup --set tags.CostCenter='{"Dept":"IT","Environment":"Test"}'
```

Entfernen Sie alle Tags für eine Ressourcengruppe wie folgt:

```azurecli-interactive
az group update -n myResourceGroup --remove tags
```
