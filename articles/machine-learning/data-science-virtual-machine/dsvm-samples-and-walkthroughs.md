---
title: Beispielprogramme und exemplarische Vorgehensweisen für ML
titleSuffix: Azure Data Science Virtual Machine
description: Anhand dieser Beispiele und exemplarischen Vorgehensweisen erfahren Sie, wie Sie allgemeine Aufgaben und Szenarios mit der Data Science Virtual Machine bewältigen können.
keywords: Data Science-Tools, virtuelle Computer für Data Science, Tools für Data Science, Linux Data Science
services: machine-learning
ms.service: data-science-vm
author: timoklimmer
ms.author: tklimmer
ms.topic: conceptual
ms.date: 05/12/2021
ms.openlocfilehash: 5ce6b2d80341a9c6ebb8afcbbe8f7072b54ca93c
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110087902"
---
# <a name="samples-on-azure-data-science-virtual-machines"></a>Beispiele in Azure Data Science Virtual Machine-Instanzen

Azure Data Science Virtual Machines (DSVMs) umfassen eine umfangreiche Sammlung von Beispielcode. Diese Beispiele beinhalten Jupyter-Notebooks und -Skripts in Sprachen wie Python und R.
> [!NOTE]
> Weitere Informationen zum Ausführen von Jupyter-Notebooks auf Ihren Data Science Virtual Machines finden Sie im Abschnitt [Zugreifen auf Jupyter](#access-jupyter).

## <a name="prerequisites"></a>Voraussetzungen

Um diese Beispiele ausführen zu können, müssen Sie eine [Ubuntu-Data Science Virtual Machine-Instanz](./dsvm-ubuntu-intro.md) bereitgestellt haben.

## <a name="available-samples"></a>Verfügbare Beispiele
| Beispielkategorie | BESCHREIBUNG | Standorte |
| ------------- | ------------- | ------------- |
| R-Sprache  | In Beispielen werden Szenarien veranschaulicht, etwa wie Verbindungen mit Azure-basierten Clouddatenspeichern hergestellt und wie Open Source-R und Microsoft Machine Learning Server verglichen werden. Anhand dieser Beisiel wird auch erläutert, wie Modelle in Microsoft Machine Learning Server und SQL Server operationalisiert werden. <br/> [R (Programmiersprache)](#r-language) | <br/>`~notebooks` <br/> <br/> `~samples/MicrosoftR` <br/> <br/> `~samples/RSqlDemo` <br/> <br/> `~samples/SQLRServices`<br/> <br/>|
| Python (Programmiersprache)  | In Beispielen werden Szenarien erläutert, etwa wie Verbindungen mit Azure-basierten Clouddatenspeichern hergestellt werden und wie mit Azure Machine Learning gearbeitet wird.  <br/> [Python (Programmiersprache)](#python-language) | <br/>`~notebooks` <br/><br/>|
| Julia (Programmiersprache)  | Bietet eine ausführliche Beschreibung zu Plotten und Deep Learning in Julia. Außerdem wird erläutert, wie C und Python aus Julia aufgerufen werden. <br/> [Julia (Programmiersprache)](#julia-language) |<br/> Windows:<br/> `~notebooks/Julia_notebooks`<br/><br/> Linux:<br/> `~notebooks/julia`<br/><br/> |
| Azure Machine Learning  | Veranschaulicht, wie Machine Learning-Modelle und Deep Learning-Modelle mit Machine Learning erstellt werden. Stellen Sie die Modelle an beliebigen Speicherorten bereit. Verwenden Sie automatisiertes maschinelles Lernen und intelligente Optimierung von Hyperparametern. Nutzen Sie darüber hinaus Modellverwaltung und verteiltes Training. <br/> [Machine Learning](#azure-machine-learning) | <br/>`~notebooks/AzureML`<br/> <br/>|
| PyTorch-Notebooks  | Deep Learning-Beispiele, die neuronale Netze auf der Grundlage von PyTorch verwenden. Mit Notebooks von Einsteiger- bis zu fortgeschrittenen Szenarien.  <br/> [PyTorch-Notebooks](#pytorch) | <br/>`~notebooks/Deep_learning_frameworks/pytorch`<br/> <br/>|
| TensorFlow  |  Eine Vielzahl von Beispielen und Techniken für neuronale Netzwerke, die mithilfe des TensorFlow-Frameworks implementiert werden. <br/> [TensorFlow](#tensorflow) | <br/>`~notebooks/Deep_learning_frameworks/tensorflow`<br/><br/> |
| H2O   | Python-basierte Beispiele, die H2O für praxisorientierte Aufgabenstellungen verwenden. <br/> [H2O](#h2o) | <br/>`~notebooks/h2o`<br/><br/> |
| SparkML (Programmiersprache)  | Beispiele, in denen Features des Apache Spark MLLib-Toolkits über pySpark und MMLSpark verwendet werden: Microsoft Machine Learning for Apache Spark on Apache Spark 2.x.  <br/> [SparkML (Programmiersprache)](#sparkml) | <br/>`~notebooks/SparkML/pySpark`<br/>`~notebooks/MMLSpark`<br/><br/>  |
| XGBoost | Standardmäßige Machine Learning-Beispiele in XGBoost für Szenarien wie Klassifizierung und Regression. <br/> [XGBoost](#xgboost) | <br/>Windows:<br/>`\dsvm\samples\xgboost\demo`<br/><br/> |

<br/>

## <a name="access-jupyter"></a>Zugreifen auf Jupyter 

Um auf Jupyter zuzugreifen, wählen Sie das **Jupyter**-Symbol auf dem Desktop oder im Anwendungsmenü aus. Sie können auch auf Jupyter in einer Linux-Edition einer DSVM zugreifen. Um unter Ubuntu remote aus einem Webbrowser zuzugreifen, wechseln Sie zu `https://<Full Domain Name or IP Address of the DSVM>:8000`.

Wenn Sie Ausnahmen hinzufügen und Jupyter-Zugriff über einen Browser verfügbar machen möchten, gehen Sie wie folgt vor:


![Aktivieren von Jupyter-Ausnahmen](./media/ubuntu-jupyter-exception.png)


Melden Sie sich mit dem Kennwort an, das Sie für ein Anmelden bei der Data Science Virtual Machine verwenden.
<br/>

**Jupyter-Startseite**
<br/>![Jupyter-Startseite](./media/jupyter-home.png)<br/>

## <a name="r-language"></a>R-Sprache 
<br/>![Beispiele für R](./media/r-language-samples.png)<br/>

## <a name="python-language"></a>Python (Programmiersprache)
<br/>![Python-Beispiele](./media/python-language-samples.png)<br/>

## <a name="julia-language"></a>Julia (Programmiersprache) 
<br/>![Beispiele für Julia](./media/julia-samples.png)<br/>

## <a name="azure-machine-learning"></a>Azure Machine Learning 
<br/>![Azure Machine Learning-Beispiele](./media/azureml-samples.png)<br/>

## <a name="pytorch"></a>PyTorch
<br/>![Beispiele für PyTorch](./media/pytorch-samples.png)<br/>

## <a name="tensorflow"></a>TensorFlow 
<br/>![Beispiele für TensorFlow](./media/tensorflow-samples.png)<br/>

## <a name="h2o"></a>H2O 
<br/>![Beispiele für H2O](./media/h2o-samples.png)<br/>

## <a name="sparkml"></a>SparkML 
<br/>![Beispiele für SparkML](./media/sparkml-samples.png)<br/>

## <a name="xgboost"></a>XGBoost 
<br/>![Beispiele für XGBoost](./media/xgboost-samples.png)<br/>