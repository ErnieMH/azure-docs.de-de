---
title: Bewerten von Machine Learning-Modellen mit PREDICT
description: Erfahren Sie, wie Sie Machine Learning-Modelle mithilfe der T-SQL-Funktion PREDICT in Synapse SQL bewerten.
services: synapse-analytics
author: anumjs
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: machine-learning
ms.date: 07/21/2020
ms.author: anjangsh
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: a8caf6cd5072b4c098adff57194784491c92bb0a
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93325374"
---
# <a name="score-machine-learning-models-with-predict"></a>Bewerten von Machine Learning-Modellen mit PREDICT

Synapse SQL bietet Ihnen die Möglichkeit, Machine Learning-Modelle mit der vertrauten Sprache T-SQL zu bewerten. Mit der T-SQL-Funktion [PREDICT](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=azure-sqldw-latest) können Sie Ihre vorhandenen Machine Learning-Modelle, die mit Verlaufsdaten trainiert wurden, innerhalb der sicheren Grenzen Ihrer Data Warehouse bewerten. Die PREDICT-Funktion nimmt ein [ONNX-Modell (Open Neural Network Exchange)](https://onnx.ai/) und die Daten als Eingaben an. Dieses Feature beseitigt den Schritt des Verschiebens von wertvollen Daten für die Bewertung an einen Speicherort außerhalb des Data Warehouse. Es soll Datenspezialisten die Möglichkeit geben, Machine Learning-Modelle ganz einfach mit der vertrauten T-SQL-Schnittstelle bereitzustellen und nahtlos mit Data Scientists zusammenzuarbeiten, die mit dem richtigen Framework für ihre Aufgabe arbeiten.

> [!NOTE]
> Diese Funktionalität wird im serverlosen SQL-Pool zurzeit nicht unterstützt.

Die Funktionalität erfordert, dass das Modell außerhalb von Synapse SQL trainiert wird. Nachdem Sie das Modell erstellt haben, laden Sie es in das Data Warehouse und bewerten es mit der T-SQL-Syntax von PREDICT, um Erkenntnisse aus den Daten zu gewinnen.

![Übersicht über PREDICT](./media/sql-data-warehouse-predict/datawarehouse-overview.png)

## <a name="training-the-model"></a>Trainieren des Modells

Synapse SQL erwartet ein vorab trainiertes Modell. Beachten Sie beim Trainieren eines Machine Learning-Modells, das zum Ausführen von Vorhersagen in Synapse SQL verwendet werden soll, die folgenden Punkte.

- Synapse SQL unterstützt nur Modelle im ONNX-Format. ONNX ist ein Open-Source-Modellformat, mit dem Sie Modelle zwischen verschiedenen Frameworks austauschen können, um Interoperabilität zu ermöglichen. Sie können Ihre vorhandenen Modelle in das ONNX-Format konvertieren. Dazu verwenden Sie Frameworks, die es entweder nativ unterstützen oder über Konvertierungspakete verfügen. Beispielsweise konvertiert das [sklearn-onnx](https://github.com/onnx/sklearn-onnx)-Paket scikit-learn-Modelle in ONNX. Das [ONNX-GitHub-Repository](https://github.com/onnx/tutorials#converting-to-onnx-format) enthält eine Liste der unterstützten Frameworks sowie Beispiele.

   Wenn Sie [automatisiertes ML](https://docs.microsoft.com/azure/machine-learning/concept-automated-ml) für das Training verwenden, stellen Sie sicher, dass der Parameter *enable_onnx_compatible_models* auf TRUE festgelegt ist, damit ein Modell im ONNX-Format Modell erstellt wird. Das [Notebook für automatisiertes maschinelles Lernen](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb) zeigt ein Beispiel für die Verwendung von AutoML zum Erstellen eines Machine Learning-Modells mit dem ONNX-Format.

- Die folgenden Datentypen werden als Eingabedaten unterstützt:
    - int, bigint, real, float
    - char, varchar, nvarchar

- Die Bewertungsdaten müssen dasselbe Format wie die Trainingsdaten aufweisen. Komplexe Datentypen wie mehrdimensionale Arrays werden von PREDICT nicht unterstützt. Stellen Sie daher für das Training sicher, dass jede Eingabe des Modells einer einzelnen Spalte der Bewertungstabelle entspricht, anstatt ein einzelnes Array zu übergeben, das alle Eingaben enthält.

- Stellen Sie sicher, dass die Namen und Datentypen der Modelleingaben mit den Spaltennamen und Datentypen der neuen Vorhersagedaten identisch sind. Die Visualisierung eines ONNX-Modells mithilfe verschiedener Open-Source-Tools, die online verfügbar sind, kann das Debuggen weiter vereinfachen.

## <a name="loading-the-model"></a>Laden des Modells

Das Modell wird in einer Synapse SQL-Benutzertabelle als hexadezimale Zeichenfolge gespeichert. Weitere Spalten, z. B. die ID und eine Beschreibung, können zur Identifikation des Modells in der Modelltabelle hinzugefügt werden. Verwenden Sie „varbinary(max)“ als Datentyp der Modellspalte. Im Folgenden finden Sie ein Codebeispiel für eine Tabelle, die zum Speichern von Modellen verwendet werden kann:

```sql
-- Sample table schema for storing a model and related data
CREATE TABLE [dbo].[Models]
(
    [Id] [int] IDENTITY(1,1) NOT NULL,
    [Model] [varbinary](max) NULL,
    [Description] [varchar](200) NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN,
    HEAP
)
GO

```

Nachdem das Modell in eine hexadezimale Zeichenfolge konvertiert und die Tabellendefinition angegeben wurde, verwenden Sie den Befehl [COPY](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) oder PolyBase, um das Modell in die Synapse SQL-Tabelle zu laden. Im folgenden Codebeispiel wird der Befehl „Copy“ verwendet, um das Modell zu laden.

```sql
-- Copy command to load hexadecimal string of the model from Azure Data Lake storage location
COPY INTO [Models] (Model)
FROM '<enter your storage location>'
WITH (
    FILE_TYPE = 'CSV',
    CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='<enter your storage key here>')
)
```

## <a name="scoring-the-model"></a>Bewerten des Modells

Nachdem das Modell und die Daten in das Data Warehouse geladen wurden, verwenden Sie die T-SQL-Funktion **PREDICT** , um das Modell zu bewerten. Stellen Sie sicher, dass die neuen Eingabedaten dasselbe Format aufweisen wie die Trainingsdaten, die zum Erstellen des Modells verwendet wurden. Die T-SQL-Funktion PREDICT nimmt zwei Eingaben an: das Modell und neue Eingabedaten für die Bewertung. Mit diesen generiert sie neue Spalten für die Ausgabe. Das Modell kann als Variable, Literal oder skalare Unterabfrage angegeben werden. Verwenden Sie [WITH common_table_expression](https://docs.microsoft.com/sql/t-sql/queries/with-common-table-expression-transact-sql?view=sql-server-ver15), um ein benanntes Resultset für den DATA-Parameter anzugeben.

Das folgende Beispiel zeigt eine Beispielabfrage mit der Funktion PREDICT. Es wird eine zusätzliche Spalte mit dem Namen *Score* und dem Datentyp *float* erstellt, die die Vorhersageergebnisse enthält. Alle Eingabedatenspalten und die Ausgabevorhersagespalten können mit der SELECT-Anweisung angezeigt werden. Weitere Details finden Sie unter [PREDICT (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=azure-sqldw-latest).

```sql
-- Query for ML predictions
SELECT d.*, p.Score
FROM PREDICT(MODEL = (SELECT Model FROM Models WHERE Id = 1),
DATA = dbo.mytable AS d) WITH (Score float) AS p;
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Funktion PREDICT finden Sie unter [PREDICT (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=azure-sqldw-latest).
