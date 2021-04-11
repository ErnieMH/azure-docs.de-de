---
title: 'Muster: Gruppieren von Richtliniendefinitionen mit Initiativen'
description: Dieses Azure Policy-Muster enthält ein Beispiel für das Gruppieren von Richtliniendefinitionen in einer Initiative.
ms.date: 03/31/2021
ms.topic: sample
ms.openlocfilehash: 7bbb2efdd27ead942fa0ef48f7785eec8bce9378
ms.sourcegitcommit: 99fc6ced979d780f773d73ec01bf651d18e89b93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106092856"
---
# <a name="azure-policy-pattern-group-policy-definitions"></a>Azure Policy-Muster: Gruppieren von Richtliniendefinitionen

Bei einer Initiative handelt es sich um eine Gruppe von Richtliniendefinitionen. Durch die Gruppierung verwandter Richtliniendefinitionen in einem einzelnen Objekt müssen Sie anstelle mehrerer Zuweisungen nur noch eine einzelne Zuweisung erstellen.

## <a name="sample-initiative-definition"></a>Beispiel für eine Initiativendefinition

Mit der folgenden Initiative werden zwei Richtliniendefinitionen bereitgestellt, die jeweils die Parameter **tagName** und **tagValue** akzeptieren. Die Initiative selbst verfügt über zwei Parameter: **costCenterValue** und **productNameValue**.
Diese Initiativenparameter werden jeweils für die einzelnen gruppierten Richtliniendefinitionen bereitgestellt. Dadurch wird die Wiederverwendung vorhandener Richtliniendefinitionen maximiert und gleichzeitig die Anzahl von Zuweisungen beschränkt, die zur bedarfsgerechten Implementierung der Definitionen erstellt werden.

:::code language="json" source="~/policy-templates/patterns/pattern-group-with-initiative.json":::

### <a name="explanation"></a>Erklärung

#### <a name="initiative-parameters"></a>Initiativparameter

Eine Initiative kann eigene Parameter definieren, die dann an die gruppierten Richtliniendefinitionen übergeben werden.
In diesem Beispiel wird sowohl **costCenterValue** als auch **productNameValue** als Initiativenparameter definiert. Die Werte werden bereitgestellt, wenn die Initiative zugewiesen wird.

:::code language="json" source="~/policy-templates/patterns/pattern-group-with-initiative.json" range="5-18":::

#### <a name="includes-policy-definitions"></a>Enthaltene Richtliniendefinitionen

Jede enthaltene Richtliniendefinition muss die Richtliniendefinitions-ID (**policyDefinitionId**) und ein Parameterarray (**parameters**) bereitstellen, falls die Richtliniendefinition Parameter akzeptiert. Im folgenden Codeausschnitt akzeptiert die enthaltene Richtliniendefinition zwei Parameter: **tagName** und **tagValue**. **tagName** ist mit einem Literal definiert. Für **tagValue** wird dagegen der durch die Initiative definierte Parameter **costCenterValue** verwendet. Diese Durchleitung von Werten trägt zur Verbesserung der Wiederverwendung bei.

:::code language="json" source="~/policy-templates/patterns/pattern-group-with-initiative.json" range="30-40":::

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich andere [Muster und integrierte Definitionen](./index.md) an.
- Lesen Sie die Informationen unter [Struktur von Azure Policy-Definitionen](../concepts/definition-structure.md).
- Lesen Sie [Grundlegendes zu Richtlinienauswirkungen](../concepts/effects.md).