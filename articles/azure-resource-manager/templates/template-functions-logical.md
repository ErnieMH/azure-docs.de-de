---
title: Vorlagenfunktionen – logisch
description: Hier werden die Funktionen beschrieben, die in einer Azure Resource Manager-Vorlage zum Bestimmen von logischen Werten verwendet werden können.
ms.topic: conceptual
ms.date: 10/12/2020
ms.openlocfilehash: ede41bd6c03eb7a01ae63526810d0310f31e4014
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91978508"
---
# <a name="logical-functions-for-arm-templates"></a>Logikfunktionen für ARM-Vorlagen

Resource Manager stellt mehrere Funktionen zum Durchführen von Vergleichen in Ihren Azure Resource Manager-Vorlagen (ARM) bereit.

* [and](#and)
* [bool](#bool)
* [false](#false)
* [if](#if)
* [not](#not)
* [or](#or)
* [true](#true)

## <a name="and"></a>und

`and(arg1, arg2, ...)`

Überprüft, ob alle Parameterwerte zutreffen.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | type | BESCHREIBUNG |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |boolean |Der erste Wert, für den überprüft wird, ob er zutrifft. |
| arg2 |Ja |boolean |Der zweite Wert, für den überprüft wird, ob er zutrifft. |
| zusätzliche Argumente |Nein |boolean |Weitere Argumente, für die überprüft wird, ob sie zutreffen. |

### <a name="return-value"></a>Rückgabewert

Gibt **True** zurück, wenn alle Werte zutreffen. Andernfalls wird **False** zurückgegeben.

### <a name="examples"></a>Beispiele

Die folgende [Beispielvorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) zeigt die Verwendung logischer Funktionen.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

Die Ausgabe aus dem vorherigen Beispiel lautet wie folgt:

| Name | type | Wert |
| ---- | ---- | ----- |
| andExampleOutput | Bool | False |
| orExampleOutput | Bool | True |
| notExampleOutput | Bool | False |

## <a name="bool"></a>bool

`bool(arg1)`

Konvertiert den Parameter in einen booleschen Wert.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | type | BESCHREIBUNG |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |Zeichenfolge oder ganze Zahl |Der Wert, der in einen booleschen Wert konvertiert werden soll. |

### <a name="return-value"></a>Rückgabewert

Ein boolescher Wert des konvertierten Werts.

### <a name="remarks"></a>Bemerkungen

Sie können auch [true()](#true) und [false()](#false) verwenden, um boolesche Werte zu erhalten.

### <a name="examples"></a>Beispiele

Die folgende [Beispielvorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/bool.json) zeigt, wie „bool“ mit einer Zeichenfolge oder ganzen Zahl verwendet wird.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "trueString": {
            "value": "[bool('true')]",
            "type" : "bool"
        },
        "falseString": {
            "value": "[bool('false')]",
            "type" : "bool"
        },
        "trueInt": {
            "value": "[bool(1)]",
            "type" : "bool"
        },
        "falseInt": {
            "value": "[bool(0)]",
            "type" : "bool"
        }
    }
}
```

Die Ausgabe aus dem vorherigen Beispiel mit den Standardwerten lautet:

| Name | type | Wert |
| ---- | ---- | ----- |
| trueString | Bool | True |
| falseString | Bool | False |
| trueInt | Bool | True |
| falseInt | Bool | False |

## <a name="false"></a>false

`false()`

Gibt false zurück.

### <a name="parameters"></a>Parameter

Die Funktion „false“ akzeptiert keine Parameter.

### <a name="return-value"></a>Rückgabewert

Ein boolescher Wert, der immer „false“ lautet.

### <a name="example"></a>Beispiel

Im folgenden Beispiel wird ein Ausgabewert „false“ zurückgegeben.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "falseOutput": {
            "value": "[false()]",
            "type" : "bool"
        }
    }
}
```

Die Ausgabe aus dem vorherigen Beispiel lautet wie folgt:

| Name | type | Wert |
| ---- | ---- | ----- |
| falseOutput | Bool | False |

## <a name="if"></a>if

`if(condition, trueValue, falseValue)`

Gibt einen Wert abhängig davon zurück, ob eine Bedingung zutrifft oder nicht.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | type | BESCHREIBUNG |
|:--- |:--- |:--- |:--- |
| condition |Ja |boolean |Der Wert, für den überprüft wird, ob er zutrifft oder nicht. |
| trueValue |Ja | Zeichenfolge, Integer, Objekt oder Array |Der zurückzugebende Wert, wenn die Bedingung zutrifft. |
| falseValue |Ja | Zeichenfolge, Integer, Objekt oder Array |Der zurückzugebende Wert, wenn die Bedingung nicht zutrifft. |

### <a name="return-value"></a>Rückgabewert

Gibt den zweiten Parameter zurück, wenn der erste Parameter **True** ist, andernfalls wird der dritte Parameter zurückgegeben.

### <a name="remarks"></a>Bemerkungen

Wenn die Bedingung zutrifft (**True**), wird nur der Wert „True“ ausgewertet. Wenn die Bedingung nicht zutrifft (**False**), wird nur der Wert „False“ ausgewertet. Mit der **if**-Funktion („Wenn“) können Sie Ausdrücke einschließen, die nur bedingt gültig sind. Beispielsweise können Sie auf eine Ressource verweisen, die unter der einen Bedingung, nicht aber unter der anderen Bedingung vorhanden ist. Ein Beispiel für die bedingte Auswertung von Ausdrücken wird im nächsten Abschnitt gezeigt.

### <a name="examples"></a>Beispiele

Die folgende [Beispielvorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/if.json) zeigt die Verwendung der `if`-Funktion.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "yesOutput": {
            "type": "string",
            "value": "[if(equals('a', 'a'), 'yes', 'no')]"
        },
        "noOutput": {
            "type": "string",
            "value": "[if(equals('a', 'b'), 'yes', 'no')]"
        },
        "objectOutput": {
            "type": "object",
            "value": "[if(equals('a', 'a'), json('{\"test\": \"value1\"}'), json('null'))]"
        }
    }
}
```

Die Ausgabe aus dem vorherigen Beispiel lautet wie folgt:

| Name | type | Wert |
| ---- | ---- | ----- |
| yesOutput | String | ja |
| noOutput | Zeichenfolge | nein |
| objectOutput | Object | { "test": "value1" } |

In der folgenden [Beispielvorlage](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/conditionWithReference.json) wird gezeigt, wie Sie diese Funktion mit Ausdrücken verwenden, die nur bedingt gültig sind.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "logAnalytics": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
        {
            "condition": "[not(empty(parameters('logAnalytics')))]",
            "name": "[concat(parameters('vmName'),'/omsOnboarding')]",
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "location": "[parameters('location')]",
            "apiVersion": "2017-03-30",
            "properties": {
                "publisher": "Microsoft.EnterpriseCloud.Monitoring",
                "type": "MicrosoftMonitoringAgent",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "workspaceId": "[if(not(empty(parameters('logAnalytics'))), reference(parameters('logAnalytics'), '2015-11-01-preview').customerId, json('null'))]"
                },
                "protectedSettings": {
                    "workspaceKey": "[if(not(empty(parameters('logAnalytics'))), listKeys(parameters('logAnalytics'), '2015-11-01-preview').primarySharedKey, json('null'))]"
                }
            }
        }
    ],
    "outputs": {
        "mgmtStatus": {
            "type": "string",
            "value": "[if(not(empty(parameters('logAnalytics'))), 'Enabled monitoring for VM!', 'Nothing to enable')]"
        }
    }
}
```

## <a name="not"></a>not

`not(arg1)`

Konvertiert den booleschen Wert in seinen gegenteiligen Wert.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | type | BESCHREIBUNG |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |boolean |Der zu konvertierende Wert. |

### <a name="return-value"></a>Rückgabewert

Gibt **True** zurück, wenn der Parameter **False** ist. Gibt **False** zurück, wenn der Parameter **True** ist.

### <a name="examples"></a>Beispiele

Die folgende [Beispielvorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) zeigt die Verwendung logischer Funktionen.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

Die Ausgabe aus dem vorherigen Beispiel lautet wie folgt:

| Name | type | Wert |
| ---- | ---- | ----- |
| andExampleOutput | Bool | False |
| orExampleOutput | Bool | True |
| notExampleOutput | Bool | False |

In der folgenden [Beispielvorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/not-equals.json) wird **not** mit „[equals](template-functions-comparison.md#equals)“ verwendet.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "checkNotEquals": {
            "type": "bool",
            "value": "[not(equals(1, 2))]"
        }
    }
}
```

Die Ausgabe aus dem vorherigen Beispiel lautet wie folgt:

| Name | type | Wert |
| ---- | ---- | ----- |
| checkNotEquals | Bool | True |

## <a name="or"></a>oder

`or(arg1, arg2, ...)`

Überprüft, ob einer der Parameterwerte zutrifft.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | type | BESCHREIBUNG |
|:--- |:--- |:--- |:--- |
| arg1 |Ja |boolean |Der erste Wert, für den überprüft wird, ob er zutrifft. |
| arg2 |Ja |boolean |Der zweite Wert, für den überprüft wird, ob er zutrifft. |
| zusätzliche Argumente |Nein |boolean |Weitere Argumente, für die überprüft wird, ob sie zutreffen. |

### <a name="return-value"></a>Rückgabewert

Gibt **True** zurück, wenn einer der Werte zutrifft. Andernfalls wird **False** zurückgegeben.

### <a name="examples"></a>Beispiele

Die folgende [Beispielvorlage](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/functions/andornot.json) zeigt die Verwendung logischer Funktionen.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

Die Ausgabe aus dem vorherigen Beispiel lautet wie folgt:

| Name | type | Wert |
| ---- | ---- | ----- |
| andExampleOutput | Bool | False |
| orExampleOutput | Bool | True |
| notExampleOutput | Bool | False |

## <a name="true"></a>true

`true()`

Gibt true zurück.

### <a name="parameters"></a>Parameter

Die Funktion „true“ akzeptiert keine Parameter.

### <a name="return-value"></a>Rückgabewert

Ein boolescher Wert, der immer „true“ lautet.

### <a name="example"></a>Beispiel

Im folgenden Beispiel wird ein Ausgabewert „true“ zurückgegeben.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "trueOutput": {
            "value": "[true()]",
            "type" : "bool"
        }
    }
}
```

Die Ausgabe aus dem vorherigen Beispiel lautet wie folgt:

| Name | type | Wert |
| ---- | ---- | ----- |
| trueOutput | Bool | True |

## <a name="next-steps"></a>Nächste Schritte

* Eine Beschreibung der Abschnitte in einer Azure Resource Manager-Vorlage finden Sie unter [Grundlegendes zur Struktur und Syntax von ARM-Vorlagen](template-syntax.md).

