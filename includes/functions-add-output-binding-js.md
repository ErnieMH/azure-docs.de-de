---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/23/2019
ms.author: glenga
ms.openlocfilehash: 098aaba0a357c13a059d3c4042a19715c6b5ad42
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "78191034"
---
Fügen Sie Code hinzu, der das Ausgabebindungsobjekt `msg` für `context.bindings` verwendet, um eine Warteschlangennachricht zu erstellen. Fügen Sie diesen Code vor der `context.res`-Anweisung hinzu.

:::code language="javascript" range="7" source="~/functions-docs-javascript/functions-add-output-binding-storage-queue-cli/HttpExample/index.js":::

Die Funktion sollte nun wie folgt aussehen:

:::code language="javascript" source="~/functions-docs-javascript/functions-add-output-binding-storage-queue-cli/HttpExample/index.js":::