---
title: Debuggen einer Java-App in einem lokalen Service Fabric-Cluster
description: In diesem Tutorial erfahren Sie, wie Sie eine in einem lokalen Cluster ausgeführte Service Fabric-Java-Anwendung debuggen und Protokolle daraus abrufen.
ms.topic: tutorial
ms.date: 02/26/2018
ms.custom: mvc, devx-track-java
ms.openlocfilehash: 801702d43bae6d925fa4f7fbc37cf44bf585fa6d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91531989"
---
# <a name="tutorial-debug-a-java-application-deployed-on-a-local-service-fabric-cluster"></a>Tutorial: Debuggen einer in einem lokalen Service Fabric-Cluster bereitgestellten Java-Anwendung

Dieses Tutorial ist der zweite Teil einer Reihe. Hier erfahren Sie, wie Sie mithilfe von Eclipse einen Remotedebugger für die Service Fabric-Anwendung anfügen. Außerdem erfahren Sie, wie Sie Protokolle von den ausgeführten Anwendungen an einen geeigneten Speicherort für den Entwickler umleiten.

In dieser Tutorialserie lernen Sie Folgendes:
> [!div class="checklist"]
> * [Erstellen einer Service Fabric Reliable Services-Java-Anwendung](service-fabric-tutorial-create-java-app.md)
> * Bereitstellen und Debuggen der Anwendung in einem lokalen Cluster
> * [Bereitstellen der Anwendung in einem Azure-Cluster](service-fabric-tutorial-java-deploy-azure.md)
> * [Einrichten der Überwachung und Diagnose für die Anwendung](service-fabric-tutorial-java-elk.md)
> * [Richten Sie CI/CD ein.](service-fabric-tutorial-java-jenkins.md)


Im zweiten Teil der Reihe lernen Sie Folgendes:
> [!div class="checklist"]
> * Debuggen einer Java-Anwendung mithilfe von Eclipse
> * Umleiten von Protokollen an einen konfigurierbaren Speicherort


## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:

* Richten Sie Ihre Entwicklungsumgebung für [Mac](service-fabric-get-started-mac.md) oder [Linux](service-fabric-get-started-linux.md) ein. Führen Sie die erforderlichen Schritte aus, um das Eclipse-Plug-In, Gradle, das Service Fabric SDK und die Service Fabric-Befehlszeilenschnittstelle (sfctl) zu installieren.

## <a name="download-the-voting-sample-application"></a>Laden Sie die Beispielanwendung „Voting“ herunter.

Falls Sie die Beispielanwendung „Voting“ aus [Teil 1 dieser Tutorialreihe](service-fabric-tutorial-create-java-app.md) nicht erstellt haben, können Sie sie herunterladen. Führen Sie in einem Befehlsfenster den folgenden Befehl aus, um das Beispiel-App-Repository auf Ihren lokalen Computer zu klonen.

```bash
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart
```

[Erstellen Sie die Anwendung, und stellen Sie sie in einem lokalen Entwicklungscluster bereit.](service-fabric-tutorial-create-java-app.md#deploy-application-to-local-cluster)

## <a name="debug-java-application-using-eclipse"></a>Debuggen einer Java-Anwendung mithilfe von Eclipse

1. Öffnen Sie auf Ihrem Computer die Eclipse-IDE, und klicken Sie auf **File > Import...** (Datei > Importieren...).

2. Klicken Sie im Popupfenster auf **General > Existing Projects into Workspace** (Allgemein > Vorhandene Projekte in Arbeitsbereich), und klicken Sie anschließend auf „Next“ (Weiter).

3. Klicken Sie im Fenster „Import Projects“ (Projekte importieren) auf **Select root directory** (Stammverzeichnis auswählen), und wählen Sie das Verzeichnis **Voting** aus. Wenn Sie den ersten Teil der Tutorialreihe absolviert haben, befindet sich das Verzeichnis **Voting** im Verzeichnis **Eclipse-workspace**.

4. Aktualisieren Sie „entryPoint.sh“ für den Dienst, den Sie debuggen möchten, sodass der Java-Prozess mit Remotedebugparametern gestartet wird. Für dieses Tutorial wird das zustandslose Front-End verwendet: *Voting/VotingApplication/VotingWebPkg/Code/entryPoint.sh*. In diesem Beispiel wurde für das Debuggen Port 8001 festgelegt.

    ```bash
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n -Djava.library.path=$LD_LIBRARY_PATH -jar VotingWeb.jar
    ```

5. Aktualisieren Sie das Anwendungsmanifest, indem die Instanz- oder Replikatanzahl für den zu debuggenden Dienst auf „1“ festlegen. Diese Einstellung vermiedet Konflikte an dem Port, der für das Debuggen verwendet wird. Legen Sie für zustandslose Dienste z.B. ``InstanceCount="1"`` und für zustandsbehaftete Dienste die Ziel- und Mindestgröße für Replikatgruppen wie folgt auf 1 fest: ``TargetReplicaSetSize="1" MinReplicaSetSize="1"``.

6. Klicken Sie in der Eclipse-IDE auf **Run > Debug Configurations > Remote Java Application** (Ausführen > Debugkonfigurationen > Java-Remoteanwendung), klicken Sie auf die Schaltfläche **New** (Neu), legen Sie die Eigenschaften wie folgt fest, und klicken Sie anschließend auf **Apply** (Anwenden):

    ```
    Name: Voting
    Project: Voting
    Connection Type: Standard
    Host: localhost
    Port: 8001
    ```

7. Fügen Sie in Zeile 109 der Datei *Voting/VotingWeb/src/statelessservice/HttpCommunicationListener.java* einen Breakpoint ein.

8. Klicken Sie im Paket-Explorer mit der rechten Maustaste auf das Projekt **Voting**, und klicken Sie dann auf **Service Fabric > Publish Application...** (Service Fabric > Anwendung veröffentlichen...).

9. Wählen Sie im Fenster **Publish Application** (Anwendung veröffentlichen) in der Dropdownliste die Option **Local.json** aus, und klicken Sie auf **Publish** (Veröffentlichen).

10. Klicken Sie in der Eclipse-IDE auf **Run > Debug Configurations > Remote Java Application** (Ausführen > Debugkonfigurationen > Java-Remoteanwendung), klicken Sie auf die erstellte Konfiguration **Voting**, und klicken Sie anschließend auf **Debug** (Debuggen).

11. Navigieren Sie in Ihrem Webbrowser zu **localhost:8080**. Dadurch wird automatisch der Breakpoint erreicht, und Eclipse wechselt in die **Debugging-Perspektive**.

Sie können nun die gleichen Schritte ausführen, um eine beliebige Service Fabric-Anwendung in Eclipse zu debuggen.

## <a name="redirect-application-logs-to-custom-location"></a>Umleiten von Anwendungsprotokollen an einen benutzerdefinierten Speicherort

Die folgenden Schritte zeigen, wie Sie die Anwendungsprotokolle aus dem Standardspeicherort */var/log/syslog* an einen benutzerdefinierten Speicherort umleiten.

1. Derzeit unterstützen Anwendungen, die in Service Fabric-Linux-Clustern ausgeführt werden, nur die Erfassung einer einzelnen Protokolldatei. Wenn Sie eine Anwendung so konfigurieren möchten, dass die Protokolle immer unter */tmp/mysfapp0.0.log* gespeichert werden, erstellen Sie unter *Voting/VotingApplication/VotingWebPkg/Code/logging.properties* eine Datei namens „logging.properties“, und fügen Sie ihr folgenden Inhalt hinzu:

    ```
    handlers = java.util.logging.FileHandler

    java.util.logging.FileHandler.level = ALL
    java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter

    # This value specifies your custom location.
    # You will have to ensure this path has read and write access by the process running the SF Application
    java.util.logging.FileHandler.pattern = /tmp/mysfapp0.0.log
    ```

2. Fügen Sie in der Datei *Voting/VotingApplication/VotingWebPkg/Code/entryPoint.sh* den folgenden Parameter für den Java-Ausführungsbefehl hinzu:

    ```bash
    -Djava.util.logging.config.file=logging.properties
    ```

    Das folgende Beispiel zeigt eine Beispielausführung mit angefügtem Debugger. Sie ähnelt der Ausführung im vorherigen Abschnitt.

    ```bash
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=logging.properties -jar VotingWeb.jar
    ```

Sie wissen nun, wie Sie das Debugging durchführen und bei der Entwicklung von Service Fabric-Java-Anwendungen auf Ihre Anwendungsprotokolle zugreifen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Teil des Tutorials haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Debuggen einer Java-Anwendung mithilfe von Eclipse
> * Umleiten von Protokollen an einen konfigurierbaren Speicherort

Fahren Sie mit dem nächsten Tutorial fort:
> [!div class="nextstepaction"]
> [Bereitstellen einer Anwendung in Azure](service-fabric-tutorial-java-deploy-azure.md)