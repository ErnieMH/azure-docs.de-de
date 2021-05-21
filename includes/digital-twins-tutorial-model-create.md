---
author: baanders
description: 'Tutorial: Includedatei für Azure Digital Twins – Modellanweisungen für die Befehlszeile'
ms.topic: include
ms.date: 3/5/2021
ms.author: baanders
ms.openlocfilehash: 784dde0790633adbbae0ae315ce0fd5c271bdb4c
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108766754"
---
1. **Aktualisieren Sie die Versionsnummer**, um anzugeben, dass Sie eine aktualisierte Version dieses Modells bereitstellen. Ändern Sie hierzu *1* am Ende des Werts `@id` in *2*. Jede Zahl, die größer als die aktuelle Versionsnummer ist, funktioniert ebenfalls.
1. **Bearbeiten Sie eine Eigenschaft.** Ändern Sie den Namen der `Humidity`-Eigenschaft in *HumidityLevel* (oder einen anderen Wert). Wenn Sie einen anderen Wert als *HumidityLevel* verwenden, merken Sie sich diesen, und nutzen Sie ihn anstelle von *HumidityLevel* in diesem Tutorial).
1. **Fügen Sie eine Eigenschaft hinzu.** Fügen Sie unterhalb der Eigenschaft `HumidityLevel`, die in Zeile 15 endet, den folgenden Code ein, um dem Raum eine Eigenschaft vom Typ `RoomName` hinzuzufügen:

    :::code language="json" source="~/digital-twins-docs-samples/models/Room.json" range="16-20":::

1. **Fügen Sie eine Beziehung hinzu.** Fügen Sie unterhalb der soeben hinzugefügten Eigenschaft `RoomName` den folgenden Code ein, um dieser Art von Zwilling die Möglichkeit zu geben, Beziehungen vom Typ *contains* mit anderen Zwillingen zu erstellen:

    :::code language="json" source="~/digital-twins-docs-samples/models/Room.json" range="21-24":::

Wenn Sie fertig sind, sollte das aktualisierte Modell wie folgt aussehen:

:::code language="json" source="~/digital-twins-docs-samples/models/Room.json":::

Achten Sie darauf, die Datei vor dem Fortfahren zu speichern.