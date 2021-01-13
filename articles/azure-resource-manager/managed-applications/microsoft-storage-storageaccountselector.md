---
title: Benutzeroberflächenelement „StorageAccountSelector“
description: Hier wird das Benutzeroberflächenelement Microsoft.Storage.StorageAccountSelector für das Azure-Portal beschrieben.
author: tfitzmac
ms.topic: conceptual
ms.date: 06/28/2018
ms.author: tomfitz
ms.openlocfilehash: fe70c42387e9dedea8943b5dcce04a1f9ded5190
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87033286"
---
# <a name="microsoftstoragestorageaccountselector-ui-element"></a>Benutzeroberflächenelement „Microsoft.Storage.StorageAccountSelector“

Ein Steuerelement zum Auswählen eines neuen oder vorhandenen Speicherkontos.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

Das Steuerelement zeigt den Standardwert an.

![Microsoft.Storage.StorageAccountSelector](./media/managed-application-elements/microsoft-storage-storageaccountselector.png)

Das Steuerelement ermöglicht es dem Benutzer, ein neues Speicherkonto zu erstellen oder ein bereits vorhandenes Speicherkonto auszuwählen.

![Microsoft.Storage.StorageAccountSelector neu](./media/managed-application-elements/microsoft-storage-storageaccountselector-new.png)

## <a name="schema"></a>Schema

```json
{
  "name": "element1",
  "type": "Microsoft.Storage.StorageAccountSelector",
  "label": "Storage account",
  "toolTip": "",
  "defaultValue": {
    "name": "storageaccount01",
    "type": "Premium_LRS"
  },
  "constraints": {
    "allowedTypes": [],
    "excludedTypes": []
  },
  "options": {
    "hideExisting": false
  },
  "visible": true
}
```

## <a name="sample-output"></a>Beispielausgabe

```json
{
  "name": "storageaccount01",
  "resourceGroup": "rg01",
  "type": "Premium_LRS",
  "newOrExisting": "new"
}
```

## <a name="remarks"></a>Bemerkungen

- Wird `defaultValue.name` angegeben, wird das Element automatisch auf Eindeutigkeit überprüft. Wenn der Name des Speicherkontos nicht eindeutig ist, muss der Benutzer einen anderen Namen angeben oder ein vorhandenes Speicherkonto auswählen.
- Der Standardwert für `defaultValue.type` lautet **Premium_LRS**.
- Alle nicht in `constraints.allowedTypes` angegebenen Typen werden ausgeblendet, und nicht in `constraints.excludedTypes` angegebene Typen werden angezeigt. `constraints.allowedTypes` und `constraints.excludedTypes` sind optional, können jedoch nicht gleichzeitig angegeben werden.
- Wenn **true** für `options.hideExisting` angegeben ist, kann der Benutzer kein vorhandenes Speicherkonto auswählen. Der Standardwert ist **false**.

## <a name="next-steps"></a>Nächste Schritte
* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
