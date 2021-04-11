---
title: Verwenden von .NET mit Hadoop MapReduce in Linux-basierten HDInsight-Clustern – Azure
description: Erfahren Sie, wie Sie .NET-Anwendungen für das Streaming von MapReduce auf Linux-basierten HDInsight-Clustern verwenden.
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: how-to
ms.date: 02/27/2018
ms.openlocfilehash: 9e626d08e7fd315ca70765477c5dae56030521c6
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104866402"
---
# <a name="migrate-net-solutions-for-windows-based-hdinsight-to-linux-based-hdinsight"></a>Migrieren von .NET-Lösungen für Windows-basierte HDInsight-Cluster zu Linux-basierten HDInsight-Clustern

Linux-basierte HDInsight-Cluster nutzen [Mono (https://mono-project.com)](https://mono-project.com) für die Ausführung von .NET-Anwendungen. Mit Mono können Sie .NET-Komponenten wie MapReduce-Anwendungen mit Linux-basierten HDInsight-Clustern verwenden. In diesem Artikel erfahren Sie, wie Sie für Windows-basierte HDInsight-Cluster erstellte .NET-Lösungen migrieren, um die Verwendung mit Mono auf Linux-basierten HDInsight-Clustern zu ermöglichen.

## <a name="mono-compatibility-with-net"></a>Kompatibilität von Mono mit .NET

Mono-Version 4.2.1 ist in HDInsight Version 3.6 enthalten. Weitere Informationen zu der mit HDInsight bereitgestellten Version von Mono finden Sie unter [HDInsight-Komponentenversionen](hdinsight-component-versioning.md).

Weitere Informationen zur Kompatibilität von Mono und .NET finden Sie im Dokument [Mono compatibility (https://www.mono-project.com/docs/about-mono/compatibility/)](https://www.mono-project.com/docs/about-mono/compatibility/)).

> [!IMPORTANT]  
> Das SCP.NET-Framework ist kompatibel mit Mono. Weitere Informationen zur Verwendung von SCP.NET mit Mono finden Sie unter [Verwenden von Visual Studio zum Entwickeln von C#-Topologien für Apache Storm in HDInsight](storm/apache-storm-develop-csharp-visual-studio-topology.md).

## <a name="automated-portability-analysis"></a>Automatisierte Portabilitätsanalyse

Mit [.NET Portability Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) können Sie einen Bericht über Inkompatibilitäten zwischen Ihrer Anwendung und Mono generieren. Führen Sie die folgenden Schritte aus, um den Analyzer zum Überprüfen Ihrer Anwendung auf Mono-Portabilität zu konfigurieren:

1. Installieren von [.NET Portability Analyzer](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer). Während der Installation wählen Sie die zu verwendende Version von Visual Studio aus.

2. Wählen Sie in Visual Studio 2015 __Analysieren__ > __Portability Analyzer-Einstellungen__ aus, und stellen Sie sicher, dass __4.5__ im Abschnitt __Mono__ aktiviert ist.

    :::image type="content" source="./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-settings.png" alt-text="Im Abschnitt „Mono“ für die Analyzer-Einstellungen ist „4.5“ aktiviert":::

    Klicken Sie auf __OK__, um die Konfiguration zu speichern.

3. Wählen Sie __Analysieren__ > __Portabilität der Assembly analysieren__ aus. Wählen Sie die Assembly aus, die Ihre Lösung enthält, und klicken Sie dann auf __Öffnen__, um die Analyse zu starten.

4. Nachdem die Analyse abgeschlossen ist, wählen Sie __Analysieren__ > __Analyseberichte anzeigen__ aus. Wählen Sie unter __Ergebnisse der Portabilitätsanalyse__ die Option __Bericht öffnen__ aus, um den Bericht zu öffnen.

    :::image type="content" source="./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-results.png" alt-text="Dialogfeld „Ergebnisse der Portabilitätsanalyse“":::

> [!IMPORTANT]  
> Der Analyzer kann nicht jedes Problem mit Ihrer Lösung abfangen. Der Dateipfad `c:\temp\file.txt` wird beispielsweise als gültig angesehen, wenn Mono unter Windows ausgeführt wird. Der gleiche Pfad ist auf einer Linux-Plattform ungültig.

## <a name="manual-portability-analysis"></a>Manuelle Portabilitätsanalyse

Führen Sie eine manuelle Prüfung des Codes anhand der Informationen im Artikel [Application Portability (https://www.mono-project.com/docs/getting-started/application-portability/)](https://www.mono-project.com/docs/getting-started/application-portability/)) durch.

## <a name="modify-and-build"></a>Ändern und Erstellen

Sie können Visual Studio weiterhin zum Erstellen Ihrer .NET-Lösungen für HDInsight verwenden. Allerdings müssen Sie sicherstellen, dass das Projekt für die Verwendung von .NET Framework 4.5 konfiguriert ist.

## <a name="deploy-and-test"></a>Bereitstellen und Testen

Nachdem Sie Ihre Lösung mithilfe der Empfehlungen von .NET Portability Analyzer oder einer manuellen Analyse geändert haben, müssen Sie sie mit HDInsight testen. Das Testen der Lösung auf einem Linux-basierten HDInsight-Cluster legt möglicherweise geringfügige Probleme offen, die korrigiert werden müssen. Es empfiehlt sich, während des Testens in der Anwendung zusätzliche Protokollierung zu aktivieren.

Weitere Informationen zum Zugriff auf Protokolle finden Sie in den folgenden Artikeln:

* [Zugreifen auf Apache Hadoop YARN-Anwendungsprotokolle unter Linux-basiertem HDInsight](hdinsight-hadoop-access-yarn-app-logs-linux.md)

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von C# mit MapReduce in HDInsight](hadoop/apache-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [Verwenden benutzerdefinierter C#-Funktionen mit Apache Hive und Apache Pig](hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)

* [Entwickeln von C#-Topologien für Apache Storm in HDInsight](storm/apache-storm-develop-csharp-visual-studio-topology.md)
