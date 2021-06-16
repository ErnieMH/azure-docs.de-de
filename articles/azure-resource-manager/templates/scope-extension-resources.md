---
title: Bereich für Erweiterungsressourcentypen
description: Hier wird die Verwendung der Bereichseigenschaft beim Bereitstellen von Erweiterungsressourcentypen erläutert.
ms.topic: conceptual
ms.date: 01/13/2021
ms.custom: devx-track-azurepowershell, devx-track-azurecli
ms.openlocfilehash: 534011658502829df8fa23e44264c8ff2992d3cf
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111960013"
---
# <a name="setting-scope-for-extension-resources-in-arm-templates"></a>Festlegen des Bereichs für Erweiterungsressourcen in Resource Manager-Vorlagen

Eine Erweiterungsressource ist eine Ressource, die eine andere Ressource ändert. Sie können beispielsweise einer Ressource eine Rolle zuweisen. Die Rollenzuweisung ist ein Erweiterungsressourcentyp.

Eine vollständige Liste der Erweiterungsressourcentypen finden Sie unter [Ressourcentypen, die Funktionen anderer Ressourcen erweitern](../management/extension-resource-types.md).

In diesem Artikel wird gezeigt, wie Sie den Bereich für einen Erweiterungsressourcentyp festlegen, wenn dieser mit einer Resource Manager-Vorlage (ARM-Vorlage) bereitgestellt wird. Außerdem wird die Bereichseigenschaft beschrieben, die für Erweiterungsressourcen beim Anwenden auf eine Ressource verfügbar ist.

> [!NOTE]
> Die Bereichseigenschaft ist nur für Erweiterungsressourcentypen verfügbar. Wenn Sie einen anderen Bereich für einen Ressourcentyp angeben möchten, der kein Erweiterungstyp ist, verwenden Sie eine geschachtelte oder verknüpfte Bereitstellung. Weitere Informationen finden Sie unter [Bereitstellungen von Ressourcengruppen](deploy-to-resource-group.md), [Bereitstellungen von Abonnements](deploy-to-subscription.md), [Bereitstellungen von Verwaltungsgruppen](deploy-to-management-group.md) und [Bereitstellungen von Mandanten](deploy-to-tenant.md).

## <a name="apply-at-deployment-scope"></a>Anwenden im Bereitstellungsumfang

Fügen Sie die Ressource wie bei jedem anderen Ressourcentyp zu Ihrer Vorlage hinzu, um einen Erweiterungsressourcentyp im Zielbereitstellungsumfang anzuwenden. Die verfügbaren Bereiche sind [Ressourcengruppe](deploy-to-resource-group.md), [Abonnement](deploy-to-subscription.md), [Verwaltungsgruppe](deploy-to-management-group.md) und [Mandant](deploy-to-tenant.md). Der Bereitstellungsbereich muss den Ressourcentyp unterstützen.

Mit der folgenden Vorlage wird eine Sperre bereitgestellt.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/locktargetscope.json":::

Bei der Bereitstellung in einer Ressourcengruppe wird die Ressourcengruppe gesperrt.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az deployment group create \
  --resource-group ExampleGroup \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/scope/locktargetscope.json"
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
 New-AzResourceGroupDeployment `
  -ResourceGroupName ExampleGroup `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/scope/locktargetscope.json"
```

---

Im nächsten Beispiel wird eine Rolle zugewiesen.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/roletargetscope.json":::

Bei der Bereitstellung für ein Abonnement wird dem Abonnement die Rolle zugewiesen.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az deployment sub create \
  --name demoSubDeployment \
  --location centralus \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/scope/roletargetscope.json"
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
New-AzSubscriptionDeployment `
  -Name demoSubDeployment `
  -Location centralus `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/scope/roletargetscope.json"
```

---

## <a name="apply-to-resource"></a>Anwenden auf die Ressource

Verwenden Sie die `scope`-Eigenschaft, um eine Erweiterungsressource auf eine Ressource anzuwenden. Legen Sie die Bereichseigenschaft auf den Namen der Ressource fest, der Sie die Erweiterung hinzufügen möchten. Die Bereichseigenschaft ist eine Stammeigenschaft für den Erweiterungsressourcentyp.

Im folgenden Beispiel wird ein Speicherkonto erstellt und eine Rolle darauf angewendet.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/storageandrole.json" highlight="56":::

## <a name="next-steps"></a>Nächste Schritte

* Um zu verstehen, wie Parameter in der Vorlage definiert werden, lesen Sie [Verstehen der Struktur und Syntax von ARM-Vorlagen](./syntax.md).
* Tipps zum Beheben gängiger Azure-Bereitstellungsfehler finden Sie unter [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](common-deployment-errors.md).
* Informationen zum Bereitstellen einer Vorlage, die ein SAS-Token erfordert, finden Sie unter [Bereitstellen einer privaten ARM-Vorlage mit SAS-Token](secure-template-with-sas-token.md).