---
title: 'Azure HDInsight-Thema: Was sind Apache Hadoop und MapReduce?'
description: Eine Einführung in HDInsight, den Apache Hadoop-Technologiestapel und Apache Hadoop-Komponenten
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: overview
ms.custom: hdinsightactive,hdiseo17may2017,mvc,seodec18
ms.date: 02/27/2020
ms.openlocfilehash: 5e5f02b1684e56496778ab677aa9dc46e7dcd9aa
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "87086522"
---
# <a name="what-is-apache-hadoop-in-azure-hdinsight"></a>Worum handelt es sich bei Apache Hadoop in Azure HDInsight?

[Apache Hadoop](https://hadoop.apache.org/) war ursprünglich ein Open Source-Framework für die verteilte Verarbeitung und Analyse umfangreicher Datasets in Clustern. Das Hadoop-Ökosystem umfasst verwandte Software und Hilfsprogramme, einschließlich Apache Hive, Apache HBase, Spark, Kafka und viele andere.

Azure HDInsight ist ein umfassender, vollständig verwalteter Open-Source-Analysedienst in der Cloud für Unternehmen. Der Apache Hadoop-Clustertyp in Azure HDInsight ermöglicht Ihnen die Verwendung von Hadoop Distributed File System, der YARN-Ressourcenverwaltung und eines einfachen MapReduce-Programmiermodells zum parallelen Verarbeiten und Analysieren von Batchdaten.

Informationen zu verfügbare Komponenten des Hadoop-Technologiestapels für HDInsight finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](../hdinsight-component-versioning.md). Weitere Informationen zu Hadoop in HDInsight finden Sie auf der Seite mit [Azure-Features für HDInsight](https://azure.microsoft.com/services/hdinsight/).

## <a name="what-is-mapreduce"></a>Was ist MapReduce?

Apache Hadoop MapReduce ist ein Softwareframework zum Schreiben von Aufträgen, die sehr große Datenmengen verarbeiten. Eingabedaten werden in unabhängigen Blöcke aufgeteilt. Jeder Block wird in den Knoten im Cluster parallel verarbeitet. Ein MapReduce-Auftrag besteht aus zwei Funktionen:

* **Mapper**: Verarbeitet Eingabedaten, analysiert sie (in der Regel mit Filter- und Sortiervorgängen) und gibt Tupel (Schlüssel-Wert-Paare) aus.

* **Reducer**: Verarbeitet Tupel, die vom Mapper ausgegeben wurden, und führt einen Zusammenfassungsvorgang aus, der kleinere, kombinierte Ergebnisse aus den Mapper-Daten erstellt.

Ein Beispiel für einen grundlegenden MapReduce-Auftrag zum Zählen von Wörtern wird im folgenden Diagramm veranschaulicht:

 ![HDI.Wortzähldiagramm](./media/apache-hadoop-introduction/hdi-word-count-diagram.gif)

Dieses Auftrag gibt die Anzahl der Vorkommen jedes Worts im Text aus.

* Der Mapper verwendet die einzelnen Zeilen des Eingabetexts als Eingabe und unterteilt diese in Wörter. Er gibt bei jedem Vorkommen eines Worts ein Schlüssel-Wert-Paar des Worts gefolgt von 1 aus. Die Ausgabe wird sortiert, bevor sie an den Reducer gesendet wird.
* Der Reducer addiert die einzelnen Zählwerte jedes Worts und gibt ein einziges Schlüssel-Wert-Paar aus, das aus dem Wort gefolgt von der Summe seiner Vorkommen besteht.

MapReduce kann in verschiedenen Sprachen implementiert werden. Java ist die am häufigsten verwendete Implementierung und wird in diesem Dokument zu Demonstrationszwecken verwendet.

## <a name="development-languages"></a>Entwicklungssprachen

Sprachen oder Frameworks auf der Grundlage von Java und der Java Virtual Machine können direkt als MapReduce-Auftrag ausgeführt werden. Das in diesem Artikel verwendete Beispiel ist eine Java-MapReduce-Anwendung. Nicht Java-basierte Sprachen (etwa C# oder Python) bzw. eigenständige ausführbare Dateien müssen **Hadoop-Datenströme** verwenden.

Hadoop-Datenströme kommunizieren über STDIN und STDOUT mit Mapper und Reducer. Mapper und Reducer lesen zeilenweise Daten aus STDIN, und schreiben die Ausgabe in STDOUT. Jede vom Mapper und vom Reducer gelesene oder ausgegebene Zeile muss als Schlüssel-Wert-Paar mit Tabstopptrennzeichen formatiert sein:

`[key]\t[value]`

Weitere Informationen finden Sie unter [Hadoop Streaming](https://hadoop.apache.org/docs/current/hadoop-streaming/HadoopStreaming.html)(in englischer Sprache).

Beispiele für die Verwendung von Hadoop-Datenströmen mit HDInsight finden Sie in dem folgenden Artikel:

* [Entwickeln von C#-MapReduce-Aufträgen](apache-hadoop-dotnet-csharp-mapreduce-streaming.md)

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines Apache Hadoop-Clusters in HDInsight](apache-hadoop-linux-create-cluster-get-started-portal.md)
