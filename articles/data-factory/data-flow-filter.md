---
title: Filter-Transformation in einem Zuordnungsdatenfluss
description: Mithilfe der Filter-Transformation in einem Azure Data Factory-Zuordnungsdatenfluss können Sie Zeilen herausfiltern.
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 05/26/2020
ms.openlocfilehash: 8189228d6707812fb943e9925dc2bbf1b6da4972
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "84112813"
---
# <a name="filter-transformation-in-mapping-data-flow"></a>Filter-Transformation in einem Zuordnungsdatenfluss

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Filter-Transformationen ermöglicht das Filtern von Zeilen auf der Grundlage einer Bedingung. Der Ausgabestream enthält alle Zeilen, die der Filterbedingung entsprechen. Die Filter-Transformation ähnelt einer WHERE-Klausel in SQL.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4xnxN]

## <a name="configuration"></a>Konfiguration

Verwenden Sie den Ausdrucks-Generator für Datenflüsse, um einen Ausdruck für die Filterbedingung einzugeben. Klicken Sie auf das blaue Feld, um den Ausdrucks-Generator zu öffnen. Bei der Filterbedingung muss es sich um eine boolesche Bedingung handeln. Weitere Informationen zur Ausdruckserstellung finden Sie in der [Dokumentation für den Ausdrucks-Generator](concepts-data-flow-expression-builder.md).

![Filter-Transformation](media/data-flow/filter1.png "Filter-Transformation")

## <a name="data-flow-script"></a>Datenflussskript

### <a name="syntax"></a>Syntax

```
<incomingStream>
    filter(
        <conditionalExpression>
    ) ~> <filterTransformationName>
```

### <a name="example"></a>Beispiel

Das folgende Beispiel ist eine Filtertransformation mit dem Namen `FilterBefore1960`, bei der der eingehende Stream `CleanData` verwendet wird. Die Filterbedingung ist der Ausdruck `year <= 1960`.

Auf der Data Factory-Benutzeroberfläche sieht diese Transformation wie folgt aus:

![Filter-Transformation](media/data-flow/filter1.png "Filter-Transformation")

Das Datenflussskript für diese Transformation befindet sich im folgenden Codeausschnitt:

```
CleanData
    filter(
        year <= 1960
    ) ~> FilterBefore1960

```

## <a name="next-steps"></a>Nächste Schritte

Verwenden Sie die [Select-Transformation](data-flow-select.md), um Spalten herauszufiltern.
