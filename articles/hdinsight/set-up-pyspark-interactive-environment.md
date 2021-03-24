---
title: Interaktive PySpark-Umgebung mit Azure HDInsight-Tools
description: Hier erfahren Sie, wie Sie mithilfe von Azure HDInsight Tools for Visual Studio Code Abfragen und Skripts erstellen und übermitteln.
keywords: VScode, Azure HDInsight Tools, Hive, Python, PySpark, Spark, HDInsight, Hadoop, LLAP, Interactive Hive, Interactive Query
ms.service: hdinsight
ms.topic: how-to
ms.custom: seoapr2020, devx-track-python
ms.date: 04/23/2020
ms.openlocfilehash: a6247116cdf579691e48883231f57712da36c4ad
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2021
ms.locfileid: "104864566"
---
# <a name="set-up-the-pyspark-interactive-environment-for-visual-studio-code"></a>Einrichten einer interaktiven PySpark-Umgebung für Visual Studio Code

Die folgenden Schritte zeigen das Einrichten der interaktiven PySpark-Umgebung in VS Code. Dieser Schritt betrifft nur nicht-Windows-Benutzer.

Mit dem Befehl **python/pip** wird die virtuelle Umgebung in Ihrem Home-Pfad erstellt. Wenn Sie eine andere Version verwenden möchten, müssen Sie die Standardversion des Befehls **python/pip** manuell ändern. Weitere Informationen finden Sie unter [update-alternatives](https://linux.die.net/man/8/update-alternatives).

1. Installieren Sie [Python](https://www.python.org/downloads/) und [pip](https://pip.pypa.io/en/stable/installing/).

   * Installieren Sie Python über [https://www.python.org/downloads/](https://www.python.org/downloads/). 
   * Installieren Sie pip über [https://pip.pypa.io/en/stable/installing](https://pip.pypa.io/en/stable/installing/) (wenn nicht aus der Python-Installation installiert).
   * Vergewissern Sie sich optional mithilfe der Befehle `python --version` und `pip --version`, dass Python und pip erfolgreich installiert wurden. 

     > [!NOTE]
     > Sie sollten Python manuell installieren, anstatt die Standardversion für macOS zu verwenden.

2. Installieren Sie **virtualenv** mit dem folgenden Befehl.

   ```bash
   pip install virtualenv
   ```

## <a name="other-packages"></a>Andere Pakete

Wenn unter Linux die Fehlermeldung unten ausgegeben wird, installieren Sie die erforderlichen Pakete, indem Sie die folgenden zwei Befehle ausführen:

   :::image type="content" source="./media/set-up-pyspark-interactive-environment/install-libkrb5-package.png" alt-text="Installieren des libkrb5-Pakets für Python" border="true":::

```bash
sudo apt-get install libkrb5-dev
```

```bash
sudo apt-get install python-dev
```

Starten Sie VSCode erneut, wechseln Sie dann zurück zum VSCode-Editor, und führen Sie den Befehl **Spark: PySPark Interactive** aus.

## <a name="next-steps"></a>Nächste Schritte

### <a name="demo"></a>Demo

* HDInsight for VS Code: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen

* [Verwenden von Azure HDInsight-Tools für Visual Studio Code](hdinsight-for-vscode.md)
* [Verwenden des Azure-Toolkits für IntelliJ zum Erstellen und Übermitteln von Apache Spark Scala-Anwendungen](spark/apache-spark-intellij-tool-plugin.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](spark/apache-spark-jupyter-notebook-install-locally.md)
