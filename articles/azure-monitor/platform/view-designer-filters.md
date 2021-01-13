---
title: Filter in Azure Monitor-Ansichten | Microsoft-Dokumentation
description: Mit Filtern in einer Azure Monitor-Ansicht können Benutzer die Daten in der Ansicht anhand des Werts einer bestimmten Eigenschaft filtern, ohne die Ansicht selbst zu ändern.  Dieser Artikel beschreibt, wie Sie Filter verwenden und einer benutzerdefinierten Ansicht hinzufügen.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/22/2018
ms.openlocfilehash: d428382493e15d2e0571f4cb4b6f090cf9056fe4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91449310"
---
# <a name="filters-in-azure-monitor-views"></a>Filter in Azure Monitor-Ansichten
Mit **Filtern** in einer [Azure Monitor-Ansicht](view-designer.md) können Benutzer die Daten in der Ansicht anhand des Werts einer bestimmten Eigenschaft filtern, ohne die Ansicht selbst zu ändern.  Beispielsweise können Sie Benutzern Ihrer Ansicht ermöglichen, die Ansicht nach Daten nur von einem bestimmten Computer oder eine Gruppe von Computern zu filtern.  Sie können mehrere Filter für eine einzelne Ansicht erstellen, um Benutzern das Filtern nach mehreren Eigenschaften zu ermöglichen.  Dieser Artikel beschreibt, wie Sie Filter verwenden und einer benutzerdefinierten Ansicht hinzufügen.

## <a name="using-a-filter"></a>Verwenden eines Filters
Klicken Sie oben in der Ansicht auf den Datums-/Uhrzeitbereich, um die Dropdownliste zu öffnen, in der Sie den Datums-/Uhrzeitbereich für die Ansicht ändern können.

![Screenshot des Dropdownmenüs „Zeitbereich“ für eine Ansicht in Azure Monitor mit ausgewähltem Optionsfeld „Letzte 7 Tage“](media/view-designer-filters/filters-example-time.png)

Klicken Sie auf **+** , um einen Filter mithilfe benutzerdefinierter Filter hinzuzufügen, die für die Ansicht definiert sind. Wählen Sie entweder einen Wert für den Filter aus der Dropdownliste, oder geben Sie einen Wert ein. Klicken Sie auf **+** , um weitere Filter hinzuzufügen. 


![Screenshot des Dialogfelds zum Hinzufügen eines benutzerdefinierten Filters in Azure Monitor. Im Dropdownmenü „Eigenschaft auswählen“ wird die Eigenschaft „Computer“ ausgewählt.](media/view-designer-filters/filters-example-custom.png)

Wenn Sie alle Werte für einen Filter entfernen, wird dieser Filter nicht mehr angewendet.


## <a name="creating-a-filter"></a>Erstellen eines Filters

Erstellen Sie einen Filter über die Registerkarte **Filter**, wenn Sie [eine Ansicht bearbeiten](view-designer.md).  Der Filter gilt global für die Ansicht und wird auf alle Teile der Ansicht angewendet.  

![Filtereinstellungen](media/view-designer-filters/filters-settings.png)

In der folgenden Tabelle werden die Einstellungen für Filter beschrieben.

| Einstellung | Beschreibung |
|:---|:---|
| Feldname | Der Name des zum Filtern verwendeten Felds.  Dieses Feld muss dem Zusammenfassungsfeld unter **Werte abfragen** entsprechen. |
| Werte abfragen | Die Abfrage, die zum Auffüllen der Filterdropdownliste für den Benutzer ausgeführt werden soll.  Diese Abfrage muss [Zusammenfassen](/azure/kusto/query/summarizeoperator) oder [Verschieden](/azure/kusto/query/distinctoperator) verwenden, um eindeutige Werte für ein bestimmtes Feld bereitzustellen, und sie muss dem **Feldnamen** entsprechen.  Sie können [Sortieren](/azure/kusto/query/sortoperator) verwenden, um die dem Benutzer angezeigten Werte zu sortieren. |
| Tag | Name für das Feld, der in Abfragen mit dem Filter verwendet und dem Benutzer angezeigt wird. |

### <a name="examples"></a>Beispiele

Die folgende Tabelle enthält einige Beispiele für häufig verwendete Filter.  

| Feldname | Werte abfragen | Tag |
|:--|:--|:--|
| Computer   | Heartbeat &#124; distinct Computer &#124; sort by Computer asc | Computers |
| EventLevelName | Event &#124; distinct EventLevelName | Schweregrad |
| SeverityLevel | Syslog &#124; distinct SeverityLevel | Schweregrad |
| SvcChangeType | ConfigurationChange &#124; distinct svcChangeType | ChangeType |


## <a name="modify-view-queries"></a>Ändern von Ansichtsabfragen

Damit ein Filter wirkt, müssen Sie Abfragen in der Ansicht so ändern, dass über die ausgewählten Werte gefiltert wird.  Wenn Sie keine Abfragen in der Ansicht ändern, haben die vom Benutzer ausgewählten Werte keine Auswirkungen.

Die Syntax für die Verwendung eines Filterwerts in einer Abfrage lautet: 

`where ${filter name}`  

Wenn Ihre Ansicht beispielsweise eine Abfrage aufweist, die Ereignisse zurückgibt und einen Filter namens _Computer_ verwendet, können Sie die folgende Abfrage verwenden.

```kusto
Event | where ${Computers} | summarize count() by EventLevelName
```

Wenn Sie einen weiteren Filter mit dem Namen „Schweregrad“ hinzugefügt haben, können Sie mit der folgenden Abfrage beide Filter verwenden.

```kusto
Event | where ${Computers} | where ${Severity} | summarize count() by EventLevelName
```

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über die [Visualisierungsteile](view-designer-parts.md), die Sie der benutzerdefinierten Ansicht hinzufügen können.
