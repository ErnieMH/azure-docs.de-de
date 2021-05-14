---
author: v-tcassi
ms.service: iot-edge
ms.topic: include
ms.date: 08/26/2020
ms.author: v-tcassi
ms.openlocfilehash: b5450e4846c3c49c89830ae65c50a95ee0c8d6eb
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104803386"
---
## <a name="verify-iot-edge-cicd-with-the-build-and-release-pipelines"></a>Überprüfen von CI/CD in IoT Edge mit den Build- und Releasepipelines

Um einen Buildauftrag auszulösen, können Sie einen Commit in das Quellcoderepository pushen oder manuell auslösen. In diesem Abschnitt lösen Sie manuell die CI/CD-Pipeline aus, um die Funktionsweise zu testen. Anschließend überprüfen Sie, ob die Bereitstellung erfolgreich ausgeführt wird.

1. Wählen Sie im Menü auf der linken Seite **Pipelines** aus, und öffnen Sie die Buildpipeline, die Sie am Anfang dieses Artikels erstellt haben.

2. Sie können einen Buildauftrag in ihrer Buildpipeline auslösen, indem Sie oben rechts die Schaltfläche **Pipeline ausführen** auswählen.

    ![Manuelles Auslösen Ihrer Buildpipeline über die Schaltfläche „Pipeline ausführen“](./media/iot-edge-verify-iot-edge-continuous-integration-continuous-deployment/manual-trigger.png)

3. Überprüfen Sie die Einstellungen für **Pipeline ausführen**. Wählen Sie dann **Ausführen** aus.

    ![Optionen für „Pipeline ausführen“ angeben und „Ausführen“ auswählen](./media/iot-edge-verify-iot-edge-continuous-integration-continuous-deployment/run-pipeline-settings.png)

4. Wählen Sie **Agentauftrag 1** aus, um den Fortschritt der Ausführung zu überwachen. Durch Auswählen des Auftrags können Sie die Protokolle der Auftragsausgabe überprüfen. 

    ![Protokollausgabe des Auftrags überprüfen](./media/iot-edge-verify-iot-edge-continuous-integration-continuous-deployment/view-job-run.png)

5. Wenn die Buildpipeline erfolgreich abgeschlossen ist, löst sie eine Releasepipeline zur **dev**-Stage aus. Das erfolgreiche **dev**-Release erstellt eine IoT Edge-Bereitstellung für IoT Edge-Geräte.

    ![Release für dev](./media/iot-edge-verify-iot-edge-continuous-integration-continuous-deployment/pending-approval.png)

6. Klicken Sie auf die Stage **dev**, um die Releaseprotokolle anzuzeigen.

    ![Releaseprotokolle](./media/iot-edge-verify-iot-edge-continuous-integration-continuous-deployment/release-logs.png)

7. Wenn bei Ihrer Pipeline ein Fehler aufgetreten ist, sehen Sie sich zuerst die Protokolle an. Sie können Protokolle anzeigen, indem Sie zur Zusammenfassung der Pipelineausführung navigieren und dann den Auftrag und die Aufgabe auswählen. Wenn eine bestimmte Aufgabe fehlschlägt, überprüfen Sie die Protokolle dafür. Ausführliche Anleitungen zum Konfigurieren und Verwenden von Protokollen finden Sie unter [Überprüfen von Protokollen zum Diagnostizieren von Pipelineproblemen](/azure/devops/pipelines/troubleshooting/review-logs).
