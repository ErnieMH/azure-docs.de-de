---
title: Erstellen von Richtlinien für Arrayeigenschaften für Ressourcen
description: Erfahren Sie, wie Sie mit Arrayparametern und Arrayausdrücken arbeiten, den [*]-Alias auswerten und Elemente mit Azure Policy-Definitionsregeln anfügen.
ms.date: 09/30/2020
ms.topic: how-to
ms.openlocfilehash: c67982197c0161d99f29747d6fd11166cba86079
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91576896"
---
# <a name="author-policies-for-array-properties-on-azure-resources"></a>Erstellen von Richtlinien für Arrayeigenschaften für Azure-Ressourcen

Azure Resource Manager-Eigenschaften werden häufig als Zeichenfolgen und boolesche Werte definiert. Im Fall einer 1: n-Beziehung werden komplexe Eigenschaften stattdessen als Arrays definiert. In Azure Policy werden Arrays auf verschiedene Weise verwendet:

- Als Typ eines [Definitionsparameters](../concepts/definition-structure.md#parameters), um mehrere Optionen bereitzustellen
- Als Teil einer [Richtlinienregel](../concepts/definition-structure.md#policy-rule) mit den Bedingungen **in** oder **notIn**
- Als Teil einer Richtlinienregel, die den [\[\*\]-Alias](../concepts/definition-structure.md#understanding-the--alias) auswertet für:
  - Szenarien wie **Keine**, **Beliebig** oder **Alle**
  - Komplexe Szenarien mit **count**
- In der [Auswirkung „append“](../concepts/effects.md#append), um ein vorhandenes Array zu ersetzen oder ihm Elemente hinzuzufügen

Dieser Artikel befasst sich mit jedem dieser Verwendungsfälle in Azure Policy und enthält mehrere Beispieldefinitionen.

## <a name="parameter-arrays"></a>Parameterarrays

### <a name="define-a-parameter-array"></a>Definieren eines Parameterarrays

Durch das Definieren eines Parameters als Array kann die Richtlinie flexibel gestaltet werden, wenn mehrere Werte erforderlich sind.
Diese Richtliniendefinition lässt einen beliebigen einzelnen Ort für den Parameter **AllowedLocations** zu (der Standardwert ist _eastus2_):

```json
"parameters": {
    "allowedLocations": {
        "type": "string",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        },
        "defaultValue": "eastus2"
    }
}
```

Da **type** auf _string_ festgelegt ist, kann beim Zuweisen der Richtlinie nur ein Wert festgelegt werden. Wenn diese Richtlinie zugewiesen ist, sind Ressourcen innerhalb des Bereichs nur in einer einzigen Azure-Region zulässig. Die meisten Richtliniendefinitionen müssen eine Liste genehmigter Optionen zulassen, z. B. _eastus2_, _eastus_ und _westus2_.

Verwenden Sie zum Erstellen einer Richtliniendefinition, die mehrere Optionen zulässt, den **Typ** _array_. Diese Richtlinie kann wie folgt umgeschrieben werden:

```json
"parameters": {
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        },
        "defaultValue": "eastus2",
        "allowedValues": [
            "eastus2",
            "eastus",
            "westus2"
        ]

    }
}
```

> [!NOTE]
> Nachdem eine Richtliniendefinition gespeichert wurde, kann die **type**-Eigenschaft eines Parameters nicht mehr geändert werden.

Diese neue Parameterdefinition akzeptiert während der Richtlinienzuweisung mehrere Werte. Wenn die Arrayeigenschaft **allowedValues** definiert ist, werden die bei der Zuweisung verfügbaren Werte zusätzlich auf die vordefinierte Liste von Optionen beschränkt. Die Verwendung von **allowedValues** ist optional.

### <a name="pass-values-to-a-parameter-array-during-assignment"></a>Übergeben von Werten an ein Parameterarray während der Zuweisung

Wenn Sie die Richtlinie über das Azure-Portal zuweisen, wird ein Parameter vom **Typ** _array_ als ein einzelnes Textfeld angezeigt. Der Hinweis lautet „Trennen Sie die Werte durch Semikola (;). (Beispiel: London;New York)“. Verwenden Sie die folgende Zeichenfolge, um die zulässigen Werte _eastus2_, _eastus_ und _westus2_ für den Ort an den Parameter zu übergeben:

`eastus2;eastus;westus2`

Das Format für den Parameterwert unterscheidet sich bei der Verwendung der Azure-Befehlszeilenschnittstelle (Azure CLI), von Azure PowerShell oder der REST-API. Die Werte werden in Form einer JSON-Zeichenfolge übergeben, die auch den Namen des Parameters enthält.

```json
{
    "allowedLocations": {
        "value": [
            "eastus2",
            "eastus",
            "westus2"
        ]
    }
}
```

Die Befehle zur Verwendung dieser Zeichenfolge mit den einzelnen SDKs lauten wie folgt:

- Azure CLI: Befehl [az policy assignment create](/cli/azure/policy/assignment#az-policy-assignment-create) mit dem Parameter **params**
- Azure PowerShell: Cmdlet [New-AzPolicyAssignment](/powershell/module/az.resources/New-Azpolicyassignment) mit dem Parameter **PolicyParameter**
- REST-API: Im _PUT_-Vorgang [create](/rest/api/resources/policyassignments/create) als Teil des Anforderungstextes als Wert der **properties.parameters**-Eigenschaft

## <a name="policy-rules-and-arrays"></a>Richtlinienregeln und Arrays

### <a name="array-conditions"></a>Arraybedingungen

Die [Bedingungen](../concepts/definition-structure.md#conditions) für Richtlinienregeln, mit denen ein Parameter vom 
**Typ**_array_ verwendet werden kann, sind auf `in` und `notIn` beschränkt. Die folgende Richtliniendefinition mit der Bedingung `equals` kann hierfür als Beispiel dienen:

```json
{
  "policyRule": {
    "if": {
      "not": {
        "field": "location",
        "equals": "[parameters('allowedLocations')]"
      }
    },
    "then": {
      "effect": "audit"
    }
  },
  "parameters": {
    "allowedLocations": {
      "type": "Array",
      "metadata": {
        "description": "The list of allowed locations for resources.",
        "displayName": "Allowed locations",
        "strongType": "location"
      }
    }
  }
}
```

Wenn Sie versuchen, diese Richtliniendefinition über das Azure-Portal zu erstellen, führt dies zu einer Fehlermeldung wie der folgenden:

- „Die Richtlinie ‚{0}‘ konnte aufgrund von Überprüfungsfehlern nicht parametrisiert werden. Überprüfen Sie, ob die Richtlinienparameter richtig definiert sind. Innere Ausnahme: Das Auswertungsergebnis des Sprachausdrucks ‚[parameters('allowedLocations')]‘ ist vom Typ ‚Array‘. Erwartet wird Typ ‚String‘.“

Der erwartete **Typ** der Bedingung `equals` ist _string_. Da **allowedLocations** als **Typ** _array_ definiert ist, wertet die Richtlinien-Engine den Sprachausdruck aus und löst den Fehler aus. Bei den Bedingungen `in` und `notIn` erwartet die Richtlinien-Engine den **Typ** _array_ im Sprachausdruck. Um dieses Problem zu beheben, ändern Sie `equals` in `in` oder `notIn`.

### <a name="evaluating-the--alias"></a>Auswerten des [*]-Alias

Wenn an den Namen von Aliasen **\[\*\]** angefügt ist, bedeutet dies, dass es sich um den **Typ**_array_ handelt. **\[\*\]** ermöglicht das Auswerten der einzelnen Elemente im Array, anstatt jedes Element des Arrays einzeln mit einer Verknüpfung über einen logischen AND-Operator auszuwerten. Die Auswertung jedes einzelnen Elements ist in drei Standardszenarien hilfreich: Elemente vom Typ _None_, _Any_ und _All_ stimmen überein. Verwenden Sie für komplexe Szenarien [count](../concepts/definition-structure.md#count).

Das Richtlinienmodul löst die Auswirkung (**effect**) in **then** nur aus, wenn die **if**-Regel als „true“ ausgewertet wird.
Dies ist wichtig für das Verständnis der Art und Weise, wie **\[\*\]** jedes einzelne Element des Arrays auswertet.

Die Beispielrichtlinienregel für die unten stehende Szenariotabelle sieht wie folgt aus:

```json
"policyRule": {
    "if": {
        "allOf": [
            {
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
                "exists": "true"
            },
            <-- Condition (see table below) -->
        ]
    },
    "then": {
        "effect": "[parameters('effectType')]"
    }
}
```

Das **ipRules**-Array für die unten stehende Szenariotabelle sieht wie folgt aus:

```json
"ipRules": [
    {
        "value": "127.0.0.1",
        "action": "Allow"
    },
    {
        "value": "192.168.1.1",
        "action": "Allow"
    }
]
```

Ersetzen Sie für jede der unten stehenden Beispielbedingungen `<field>` durch `"field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value"`.

Die Kombination der Bedingung, der Beispielrichtlinienregel und des Arrays der obigen vorhandenen Werte führt zu den folgenden Ergebnissen:

|Bedingung |Ergebnis | Szenario |Erklärung |
|-|-|-|-|
|`{<field>,"notEquals":"127.0.0.1"}` |Nichts |Keine Übereinstimmung |Ein Arrayelement wird als „false“ ausgewertet (127.0.0.1 != 127.0.0.1) und ein Element als „true“ (127.0.0.1 != 192.168.1.1). Die **notEquals**-Bedingung ist daher _false_, und die Auswirkung wird nicht ausgelöst. |
|`{<field>,"notEquals":"10.0.4.1"}` |Richtlinienauswirkung |Keine Übereinstimmung |Beide Arrayelemente werden als „true“ ausgewertet (10.0.4.1 != 127.0.0.1 und 10.0.4.1 != 192.168.1.1). Die **notEquals**-Bedingung ist daher _true_, und die Auswirkung wird ausgelöst. |
|`"not":{<field>,"notEquals":"127.0.0.1" }` |Richtlinienauswirkung |Eine oder mehrere Übereinstimmungen |Ein Arrayelement wird als „false“ ausgewertet (127.0.0.1 != 127.0.0.1) und ein Element als „true“ (127.0.0.1 != 192.168.1.1). Die **notEquals**-Bedingung ist daher _false_. Der logische Operator wird als TRUE ausgewertet (**nicht** _FALSE_), sodass die Auswirkung ausgelöst wird. |
|`"not":{<field>,"notEquals":"10.0.4.1"}` |Nichts |Eine oder mehrere Übereinstimmungen |Beide Arrayelemente werden als „true“ ausgewertet (10.0.4.1 != 127.0.0.1 und 10.0.4.1 != 192.168.1.1). Die **notEquals**-Bedingung ist daher _true_. Der logische Operator wird als FALSE ausgewertet (**nicht** _TRUE_), sodass die Auswirkung nicht ausgelöst wird. |
|`"not":{<field>,"Equals":"127.0.0.1"}` |Richtlinienauswirkung |Nicht alle stimmen überein |Ein Arrayelement wird als „true“ ausgewertet (127.0.0.1 == 127.0.0.1) und ein Element als „false“ (127.0.0.1 == 192.168.1.1). Die **Equals**-Bedingung ist daher _false_. Der logische Operator wird als TRUE ausgewertet (**nicht** _FALSE_), sodass die Auswirkung ausgelöst wird. |
|`"not":{<field>,"Equals":"10.0.4.1"}` |Richtlinienauswirkung |Nicht alle stimmen überein |Beide Arrayelemente werden als „false“ ausgewertet (10.0.4.1 == 127.0.0.1 und 10.0.4.1 == 192.168.1.1). Die **Equals**-Bedingung ist daher _false_. Der logische Operator wird als TRUE ausgewertet (**nicht** _FALSE_), sodass die Auswirkung ausgelöst wird. |
|`{<field>,"Equals":"127.0.0.1"}` |Nichts |Alle stimmen überein |Ein Arrayelement wird als „true“ ausgewertet (127.0.0.1 == 127.0.0.1) und ein Element als „false“ (127.0.0.1 == 192.168.1.1). Die **Equals**-Bedingung ist daher _false_, und die Auswirkung wird nicht ausgelöst. |
|`{<field>,"Equals":"10.0.4.1"}` |Nichts |Alle stimmen überein |Beide Arrayelemente werden als „false“ ausgewertet (10.0.4.1 == 127.0.0.1 und 10.0.4.1 == 192.168.1.1). Die **Equals**-Bedingung ist daher _false_, und die Auswirkung wird nicht ausgelöst. |

## <a name="modifying-arrays"></a>Ändern von Arrays

[append](../concepts/effects.md#append) und [modify](../concepts/effects.md#modify) ändern Eigenschaften von Ressourcen bei deren Erstellung oder einem Update. Beim Arbeiten mit Arrayeigenschaften hängt das Verhalten bei diesen Auswirkungen davon ab, ob der Vorgang versucht, den **\[\*\]** -Alias zu ändern:

> [!NOTE]
> Die Verwendung des `modify`-Effekts mit Aliasen befindet sich derzeit in der **Vorschau**.

|Alias |Wirkung | Ergebnis |
|-|-|-|
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules` | `append` | Azure Policy fügt das gesamte in den Auswirkungsdetails angegebene Array an, wenn nicht vorhanden. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules` | `modify` mit `add`-Vorgang | Azure Policy fügt das gesamte in den Auswirkungsdetails angegebene Array an, wenn nicht vorhanden. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules` | `modify` mit `addOrReplace`-Vorgang | Azure Policy fügt das gesamte in den Auswirkungsdetails angegebene Array an, wenn nicht vorhanden, oder ersetzt das vorhandene Array. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]` | `append` | Azure Policy fügt den in den Auswirkungsdetails angegebenen Arraymember an, wenn nicht vorhanden. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]` | `modify` mit `add`-Vorgang | Azure Policy fügt den in den Auswirkungsdetails angegebenen Arraymember an, wenn nicht vorhanden. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]` | `modify` mit `addOrReplace`-Vorgang | Azure Policy entfernt alle vorhandenen Arraymember und fügt den in den Auswirkungsdetails angegebenen Arraymember an. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].action` | `append` | Azure Policy fügt einen Wert an die `action`-Eigenschaft der einzelnen Arraymember an. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].action` | `modify` mit `add`-Vorgang | Azure Policy fügt einen Wert an die `action`-Eigenschaft der einzelnen Arraymember an. |
| `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].action` | `modify` mit `addOrReplace`-Vorgang | Azure Policy fügt bei den einzelnen Arraymembern die `action`-Eigenschaft an oder ersetzt sie, wenn vorhanden. |

Weitere Informationen finden Sie unter [Beispiele für „append“](../concepts/effects.md#append-examples).

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die Beispiele unter [Azure Policy-Beispiele](../samples/index.md) an.
- Lesen Sie die Informationen unter [Struktur von Azure Policy-Definitionen](../concepts/definition-structure.md).
- Lesen Sie [Grundlegendes zu Richtlinienauswirkungen](../concepts/effects.md).
- Informieren Sie sich über das [programmgesteuerte Erstellen von Richtlinien](programmatically-create.md).
- Erfahren Sie, wie Sie [nicht konforme Ressourcen korrigieren](remediate-resources.md) können.
- Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../../management-groups/overview.md).
