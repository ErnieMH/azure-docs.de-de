---
title: Löschen von Ressourcengruppen und Ressourcen
description: Beschreibt, wie Ressourcengruppen und Ressourcen gelöscht werden. Es wird beschrieben, in welcher Reihenfolge Azure Resource Manager das Löschen von Ressourcen beim Löschen einer Ressourcengruppe ausführt. Sie erhalten Informationen zu den Antwortcodes und dazu, wie Resource Manager anhand der Codes bestimmt, ob die Löschung erfolgreich war.
ms.topic: conceptual
ms.date: 09/03/2019
ms.custom: seodec18
ms.openlocfilehash: d6c38169916cc6c47d6f05c88645fa435bbdb146
ms.sourcegitcommit: 06ba80dae4f4be9fdf86eb02b7bc71927d5671d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2020
ms.locfileid: "91614409"
---
# <a name="azure-resource-manager-resource-group-and-resource-deletion"></a>Löschen von Ressourcengruppen und Ressourcen mit Azure Resource Manager

Dieser Artikel zeigt, wie Ressourcengruppen und Ressourcen gelöscht werden. Er beschreibt, in welcher Reihenfolge Azure Resource Manager das Löschen von Ressourcen ausführt, wenn Sie eine Ressourcengruppe löschen.

## <a name="how-order-of-deletion-is-determined"></a>Vorgehensweise zur Bestimmung der Reihenfolge des Löschvorgangs

Wenn Sie eine Ressourcengruppe löschen, bestimmt Resource Manager die Reihenfolge, in der Ressourcen gelöscht werden. Die folgende Reihenfolge wird verwendet:

1. Alle untergeordneten (geschachtelten) Ressourcen werden gelöscht.

2. Ressourcen, die andere Ressourcen verwalten, werden im nächsten Schritt gelöscht. Bei einer Ressource kann die Eigenschaft `managedBy` festgelegt sein, um anzuzeigen, dass eine andere Ressource sie verwaltet. Wenn diese Eigenschaft festgelegt ist, wird die Ressource, die die andere Ressource verwaltet, vor den anderen Ressourcen gelöscht.

3. Die verbleibenden Ressourcen werden im Anschluss an die oben genannten beiden Kategorien gelöscht.

Nachdem die Reihenfolge ermittelt wurde, gibt Resource Manager einen DELETE-Vorgang für jede Ressource aus. Es wird gewartet, bis alle Abhängigkeiten beendet wurden, bevor der Vorgang fortgesetzt wird.

Für synchrone Vorgänge lauten die erwarteten Antwortcodes bei Erfolg folgendermaßen:

* 200
* 204
* 404

Für asynchrone Vorgänge lautet die erwartete Antwort bei Erfolg 202. Resource Manager verfolgt den Location-Header oder den azure-async-Vorgangsheader nach, um den Status des asynchronen Löschvorgangs zu ermitteln.
  
### <a name="deletion-errors"></a>Fehler beim Löschen

Wenn ein Löschvorgang einen Fehler zurückgibt, wiederholt Resource Manager den DELETE-Aufruf. Wiederholungsversuche finden für die Statuscodes 5xx, 429 und 408 statt. Standardmäßig beträgt der Zeitraum für Wiederholungsversuche 15 Minuten.

## <a name="after-deletion"></a>Nach dem Löschen

Resource Manager gibt einen GET-Aufruf für jede Ressource aus, für die ein Löschversuch stattgefunden hat. Als Antwort auf diesen GET-Aufruf wird 404 erwartet. Wenn Resource Manager eine 404-Antwort erhält, wird davon ausgegangen, dass der Löschvorgang erfolgreich war. Resource Manager entfernt die Ressource aus dem Cache.

Wenn der GET-Aufruf für die Ressource jedoch 200 oder 201 zurückgibt, erstellt Resource Manager die Ressource erneut.

Wenn der GET-Vorgang einen Fehler zurückgibt, wiederholt Resource Manager die GET-Anforderung für die folgenden Fehlercodes:

* Kleiner als 100
* 408
* 429
* Größer als 500

Bei anderen Fehlercodes kann Resource Manager das Löschen der Ressource nicht durchführen.

> [!IMPORTANT]
> Das Löschen einer Ressourcengruppe kann nicht rückgängig gemacht werden.

## <a name="delete-resource-group"></a>Ressourcengruppe löschen

Verwenden Sie eine der folgenden Methoden, um die Ressourcengruppe zu löschen.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Remove-AzResourceGroup -Name ExampleResourceGroup
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az group delete --name ExampleResourceGroup
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Wählen Sie im [Portal](https://portal.azure.com) die Ressourcengruppe aus, die Sie löschen möchten.

1. Wählen Sie die Option **Ressourcengruppe löschen**.

   ![Ressourcengruppe löschen](./media/delete-resource-group/delete-group.png)

1. Geben Sie den Namen der Ressourcengruppe ein, um den Löschvorgang zu bestätigen.

---

## <a name="delete-resource"></a>Ressource löschen

Verwenden Sie eine der folgenden Methoden, um eine Ressource zu löschen.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Remove-AzResource `
  -ResourceGroupName ExampleResourceGroup `
  -ResourceName ExampleVM `
  -ResourceType Microsoft.Compute/virtualMachines
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az resource delete \
  --resource-group ExampleResourceGroup \
  --name ExampleVM \
  --resource-type "Microsoft.Compute/virtualMachines"
```

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Wählen Sie im [Portal](https://portal.azure.com) die Ressource aus, die Sie löschen möchten.

1. Klicken Sie auf **Löschen**. Der folgende Screenshot zeigt die Verwaltungsoptionen für einen virtuellen Computer.

   ![Ressource löschen](./media/delete-resource-group/delete-resource.png)

1. Bestätigen Sie den Löschvorgang, wenn Sie dazu aufgefordert werden.

---


## <a name="next-steps"></a>Nächste Schritte

* Um die Konzepte von Resource Manager zu verstehen, lesen Sie [Azure Resource Manager: Übersicht](overview.md).
* Informationen zu Löschbefehlen finden Sie unter [PowerShell](/powershell/module/az.resources/Remove-AzResourceGroup), [Azure CLI](/cli/azure/group#az-group-delete) und [REST-API](/rest/api/resources/resourcegroups/delete).
