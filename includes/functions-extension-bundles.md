---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/27/2019
ms.author: glenga
ms.openlocfilehash: d697334fe56fb9133a06cee79067c60bc3a37281
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "68639144"
---
Der einfachste Weg, Bindungserweiterungen zu installieren, ist die Aktivierung von [Erweiterungspaketen](../articles/azure-functions/functions-bindings-register.md#extension-bundles). Wenn Sie Pakete aktivieren, wird automatisch ein vordefinierter Satz von Erweiterungspaketen installiert.

Um Erweiterungspakete zu aktivieren, öffnen Sie die Datei „host.json“, und aktualisieren Sie ihren Inhalt entsprechend dem folgenden Code:

```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```