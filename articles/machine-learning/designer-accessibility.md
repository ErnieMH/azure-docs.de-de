---
title: Verwenden von Barrierefreiheitsfunktionen im Designer
titleSuffix: Azure Machine Learning
description: Erfahren Sie mehr über die im Designer verfügbaren Tastenkombinationen und Barrierefreiheitsfunktionen für die Sprachausgabe.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.author: peterlu
author: peterclu
ms.date: 01/09/2020
ms.custom: designer
ms.openlocfilehash: 86cb5260a59f864658fbb7ac1c1da2d943c6253e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90893428"
---
# <a name="use-a-keyboard-to-use-azure-machine-learning-designer"></a>Verwenden einer Tastatur zur Verwendung von Azure Machine Learning-Designer

Erfahren Sie, wie Sie den Azure Machine Learning-Designer mithilfe einer Tastatur und der Sprachausgabe verwenden können. Eine Liste der Tastenkombinationen, die überall im Azure-Portal funktionieren, finden Sie unter [Tastenkombinationen im Azure-Portal](../azure-portal/azure-portal-keyboard-shortcuts.md)

Dieser Workflow wurde mit [Sprachausgabe](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator) und [JAWS](https://www.freedomscientific.com/products/software/jaws/) getestet, sollte aber auch mit anderen standardmäßigen Sprachausgaben funktionieren.

## <a name="navigate-the-pipeline-graph"></a>Navigieren im Pipelinegraph

Der Pipelinegraph ist als geschachtelte Liste organisiert. Die äußere Liste ist eine Modulliste, die alle Module im Pipelinegraph beschreibt. Die innere Liste ist eine Verbindungsliste, die alle Verbindungen eines bestimmten Moduls beschreibt.  

1. In der Modulliste können Sie mit der Pfeiltaste zwischen den Modulen wechseln.
1. Verwenden Sie die Registerkarte, um die Verbindungsliste für das Zielmodul zu öffnen.
1. Verwenden Sie die Pfeiltaste, um zwischen den Verbindungsports für das Modul zu wechseln.
1. Verwenden Sie „G“, um zum Zielmodul zu wechseln.

## <a name="edit-the-pipeline-graph"></a>Bearbeiten des Pipelinegraphs

### <a name="add-a-module-to-the-graph"></a>Hinzufügen eines Moduls zum Graph

1. Verwenden Sie STRG+F6, um den Fokus von der Canvas zur Modulstruktur zu verschieben.
1. Suchen Sie das gewünschte Modul in der Modulstruktur mithilfe des standardmäßigen TreeView-Steuerelements.

### <a name="edit-a-module"></a>Bearbeiten eines Moduls

So verbinden Sie ein Modul mit einem anderen Modul

1. Verwenden Sie STRG + UMSCHALT + H, wenn Sie auf ein Modul in der Modulliste abzielen, um den Verbindungshilfsprogramm zu öffnen.
1. Bearbeiten Sie die Verbindungsports für das Modul.

So passen Sie die Moduleigenschaften an

1. Verwenden Sie STRG + UMSCHALT + E, wenn Sie auf ein Modul abzielen, um die Moduleigenschaften zu öffnen.
1. Bearbeiten Sie die Moduleigenschaften.

## <a name="navigation-shortcuts"></a>Tastenkombinationen zur Navigation

| Tastaturanschläge | BESCHREIBUNG |
|-|-|
| STRG + F6 | Fokus zwischen Canvas und Modulstruktur umschalten |
| STRG + F1   | Informationskarte beim Fokussieren auf einen Knoten in der Modulstruktur öffnen |
| STRG + UMSCHALT + H | Verbindungshilfsprogramm öffnen, wenn der Fokus auf einem Knoten liegt |
| STRG + UMSCHALT + E | Moduleigenschaften öffnen, wenn der Fokus auf einem Knoten liegt |
| STRG + G | Fokus auf ersten fehlerhaften Knoten verschieben, wenn bei der Pipelineausführung ein Fehler aufgetreten ist |

## <a name="action-shortcuts"></a>Tastenkombinationen für Aktionen

Verwenden Sie die folgenden Tastenkombinationen mit dem Zugriffsschlüssel. Weitere Informationen zu Zugriffsschlüsseln finden Sie unter https://en.wikipedia.org/wiki/Access_key.

| Tastaturanschläge | Action |
|-|-|
| Zugriffsschlüssel + R | Ausführen |
| Zugriffsschlüssel + P | Veröffentlichen |
| Zugriffsschlüssel + C | Klon |
| Zugriffsschlüssel + D | Bereitstellen |
| Zugriffsschlüssel + I | Rückschlusspipeline erstellen/aktualisieren |
| Zugriffsschlüssel + B | Batchrückschlusspipeline erstellen/aktualisieren |
| Zugriffsschlüssel + K | Dropdownliste „Rückschlusspipeline erstellen“ öffnen |
| Zugriffsschlüssel + U | Dropdownliste „Rückschlusspipeline aktualisieren“ öffnen |
| Zugriffsschlüssel + M | Dropdownliste „Mehr(...)“ öffnen |

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren des hohen Kontrasts oder Ändern des Designs](../azure-portal/set-preferences.md#choose-a-theme-or-enable-high-contrast)
- [Tools für die Barrierefreiheit bei Microsoft](https://www.microsoft.com/accessibility)
