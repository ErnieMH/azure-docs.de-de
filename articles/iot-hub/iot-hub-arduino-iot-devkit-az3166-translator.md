---
title: Erstellen eines Übersetzers mit Azure Functions, Cognitive Services, IoT DevKit
description: In diesem Artikel erfahren Sie, wie Sie das Mikrofon mit einem IoT DevKit verwenden, um eine Sprachnachricht zu empfangen, und die Nachricht anschließend mit Azure Cognitive Services in die englische Sprache übersetzen.
author: liydu
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 05/07/2021
ms.author: liydu
ms.custom: devx-track-csharp
ms.openlocfilehash: b9150a669c3f684c0cf5e0fbb5bd28c32594762a
ms.sourcegitcommit: eda26a142f1d3b5a9253176e16b5cbaefe3e31b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/11/2021
ms.locfileid: "109734247"
---
# <a name="use-iot-devkit-az3166-with-azure-functions-and-cognitive-services-to-make-a-language-translator"></a>Verwenden von IoT DevKit AZ3166 mit Azure Functions und Cognitive Services zum Erstellen eines Sprachübersetzers

In diesem Artikel wird erläutert, wie Sie IoT DevKit als Sprachübersetzer mit [Azure Cognitive Services](https://azure.microsoft.com/services/cognitive-services/) verwenden. Dabei wird Ihre Stimme aufgezeichnet und in englischen Text übersetzt, der auf dem DevKit-Bildschirm angezeigt wird.

Das [MXChip IoT DevKit](https://aka.ms/iot-devkit) ist ein mit Arduino kompatibles All-in-One-Board mit umfangreichen Peripheriegeräten und Sensoren. Mit den Erweiterungspaketen [Azure IoT Device Workbench](https://aka.ms/iot-workbench) und [Azure IoT Tools](https://aka.ms/azure-iot-tools) können Sie in Visual Studio Code dafür entwickeln. Der [Projektkatalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) enthält Beispielanwendungen, die Sie beim Erstellen von Prototypen für IoT-Lösungen unterstützen.

## <a name="before-you-begin"></a>Voraussetzungen

Um die Schritte in diesem Tutorial auszuführen, erledigen Sie zuerst die folgenden Aufgaben:

* Bereiten Sie Ihr DevKit vor, indem Sie die in [Verbinden von IoT DevKit AZ3166 mit Azure IoT Hub in der Cloud](./iot-hub-arduino-iot-devkit-az3166-get-started.md) beschriebenen Schritte ausführen.

## <a name="create-azure-cognitive-service"></a>Erstellen von Azure Cognitive Services

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**, und suchen Sie nach **Speech**. Füllen Sie das Formular zum Erstellen des Sprachdiensts aus.
  ![Speech-Dienst](media/iot-hub-arduino-iot-devkit-az3166-translator/speech-service.png)

1. Navigieren Sie zum eben erstellten Speech-Dienst, klicken Sie auf **Keys and Endpoint** (Schlüssel und Endpunkt), und kopieren Sie **Key 1**. Das Development Kit verwendet diesen Schlüssel für den Zugriff auf den Dienst.
  ![Kopieren der Schlüssel](media/iot-hub-arduino-iot-devkit-az3166-translator/copy-keys.png)

## <a name="open-sample-project"></a>Öffnen eines Beispielprojekts

1. Stellen Sie sicher, dass Ihr IoT-DevKit **nicht** mit Ihrem Computer verbunden ist. Starten Sie zuerst Visual Studio Code, und stellen Sie dann eine Verbindung von DevKit mit Ihrem Computer her.

1. Drücken Sie `F1`, um die Befehlspalette zu öffnen, und geben Sie **Azure IoT Device Workbench: Open Examples...** (Azure IoT Device Workbench: Beispiele öffnen) ein. Wählen Sie dann **MXChip IoT DevKit** als Board aus.

1. Suchen Sie auf der Seite mit IoT Workbench-Beispielen nach **DevKit Translator**, und klicken Sie auf **Beispiel öffnen**. Wählen Sie dann den Standardpfad zum Herunterladen des Beispielcodes aus.
  ![Beispiel öffnen](media/iot-hub-arduino-iot-devkit-az3166-translator/open-sample.png)

## <a name="use-speech-service-with-azure-functions"></a>Verwenden von Speech mit Azure Functions

1. Klicken Sie im VS Code auf `F1`, nehmen Sie Ihre Eingabe vor und wählen Sie **Azure IoT Device Workbench: Provision Azure Services...** (Bereitstellen von Azure-Diensten) aus. ![Bereitstellen von Azure-Diensten](media/iot-hub-arduino-iot-devkit-az3166-translator/provision.png)

1. Führen Sie die angezeigten Schritte aus, um Azure IoT Hub und Azure Functions bereitzustellen. Es sind drei Konfigurationen erforderlich.

   | Konfiguration    |  BESCHREIBUNG   |
   | --- | --- |    
   | **IoT Hub** | Wählen Sie eine vorhandene IoT Hub-Instanz aus, oder erstellen Sie eine neue. |
   | **IoT Hub-Gerät** | Wählen Sie ein vorhandenes IoT-Gerät aus, das bei Ihrem Hub registriert ist, oder erstellen Sie eine neue Geräteregistrierung für Ihren Hub. |
   | **Azure Functions-App** | Erstellen Sie eine neue Azure Functions-App für das Beispiel. |

   ![Bereitstellungsschritte](media/iot-hub-arduino-iot-devkit-az3166-translator/provision-steps.png)

   Notieren Sie sich den Namen des Azure IoT Hub-Geräts, das Sie erstellt haben.

1. Öffnen Sie `Functions\DevKitTranslatorFunction.cs`, und aktualisieren Sie die folgende Codezeile mit dem Gerätenamen, den Sie bei Ihrem Hub registriert haben, und dem kopierten Speech-Dienst **Key 1**. Fügen Sie außerdem die Region hinzu, in der Sie den Speech-Dienst erstellt haben.
   ```csharp
   // Subscription Key of Speech Service
   const string speechSubscriptionKey = "";

   // Region of the speech service, see https://docs.microsoft.com/azure/cognitive-services/speech-service/regions for more details.
   const string speechServiceRegion = "";

   // Device ID
   const string deviceName = "";
   ```

1. Klicken Sie auf `F1`, nehmen Sie Ihre Eingabe vor und wählen Sie **Azure IoT Device Workbench: Bereitstellen in Azure...** Wenn VS Code zur Bestätigung der erneuten Bereitstellung auffordert, klicken Sie auf **Ja**.
   ![Bereitstellungswarnung](media/iot-hub-arduino-iot-devkit-az3166-translator/deploy-warning.png)

1. Vergewissern Sie sich, dass die Bereitstellung erfolgreich war.
   ![Erfolgreiche Bereitstellung](media/iot-hub-arduino-iot-devkit-az3166-translator/deploy-success.png)

1. Klicken Sie im Azure-Portal auf Ihre Funktions-App. Klicken Sie dann im Menü auf **Funktionen** und dann auf die Funktion **devkit_translator**.
   ![Auswählen der Übersetzerfunktion](media/iot-hub-arduino-iot-devkit-az3166-translator/select-translator-function.png)

1. Klicken Sie auf **Funktions-URL abrufen**, um die URL für die Übersetzerfunktion zu kopieren.
   ![Kopieren der Funktions-URL](media/iot-hub-arduino-iot-devkit-az3166-translator/get-function-url.png)

1. Fügen Sie die URL in die Datei `azure_config.h` ein.
   ![Azure-Konfigurationsdatei](media/iot-hub-arduino-iot-devkit-az3166-translator/azure-config.png)

   > [!NOTE]
   > Wenn die Funktions-App nicht ordnungsgemäß funktioniert, finden Sie in diesem [FAQ](https://microsoft.github.io/azure-iot-developer-kit/docs/faq#compilation-error-for-azure-function)-Abschnitt Hinweise zur Fehlerbehebung.

## <a name="build-and-upload-device-code"></a>Erstellen und Hochladen von Gerätecode

1. Versetzen Sie das DevKit in den **Konfigurationsmodus**, indem Sie folgende Schritte durchführen:
   * Halten Sie die Taste **A** gedrückt.
   * Drücken Sie die Taste **Zurücksetzen**, und lassen Sie sie wieder los.
   * Lassen Sie die Taste **A** los.

   Auf dem Bildschirm werden die DevKit-ID und **Configuration** angezeigt.

   ![DevKit-Konfigurationsmodus](media/iot-hub-arduino-iot-devkit-az3166-translator/devkit-configuration-mode.png)

1. Drücken Sie `F1`, nehmen Sie Ihre Eingabe vor und wählen Sie **Azure IoT Device Workbench: Configure Device Settings... > Config Device Connection String** (Geräteeinstellungen konfigurieren &gt; Verbindungszeichenfolge für Gerät konfigurieren) aus. Klicken Sie auf **Select IoT Hub Device Connection String** (IoT Hub-Geräteverbindungszeichenfolge auswählen), um diese für das Development Kit zu konfigurieren.
   ![Konfigurieren der Verbindungszeichenfolge](media/iot-hub-arduino-iot-devkit-az3166-translator/configure-connection-string.png)

1. Sobald dieser Vorgang abgeschlossen ist, wird eine Benachrichtigung angezeigt.
   ![Erfolgreiche Konfiguration der Verbindungszeichenfolge](media/iot-hub-arduino-iot-devkit-az3166-translator/configure-connection-string-success.png)

1. Klicken Sie erneut auf `F1`, geben Sie **Azure IoT Device Workbench: Upload Device Code** (Azure IoT Device Workbench: Gerätecode hochladen) ein, und wählen Sie den angezeigten Befehl aus. Daraufhin wird der Code kompiliert und auf das DevKit hochgeladen.
   ![Hochladen des Codes](media/iot-hub-arduino-iot-devkit-az3166-translator/device-upload.png)

## <a name="test-the-project"></a>Testen des Projekts

Befolgen Sie nach der App-Initialisierung die Anweisungen auf dem DevKit-Bildschirm. Die Standardausgangssprache ist Chinesisch.

So wählen Sie eine neue zu übersetzende Sprache aus

1. Drücken Sie die Taste A, um in den Setupmodus zu gelangen.

2. Drücken Sie Taste B, um durch alle unterstützten Ausgangssprachen zu scrollen.

3. Drücken Sie Taste A, um Ihre Auswahl der Quellsprache zu bestätigen.

4. Drücken und halten Sie Taste B, während Sie sprechen, und geben Sie dann Taste B frei, um die Übersetzung einzuleiten.

5. Der ins Englische übersetzte Text wird auf dem Bildschirm angezeigt.

![Scrollen zum Auswählen der Sprache](media/iot-hub-arduino-iot-devkit-az3166-translator/select-language.jpg)

![Übersetzungsergebnis](media/iot-hub-arduino-iot-devkit-az3166-translator/translation-result.jpg)

Auf dem Bildschirm mit dem Übersetzungsergebnis können Sie folgende Aktionen ausführen:

- Drücken Sie die Tasten A und B, um zur Ausgangssprache zu scrollen und sie auszuwählen.

- Drücken Sie Taste B, um zu sprechen. Um das Gesprochene zu senden und den Übersetzungstext zu erhalten, geben Sie Taste B frei.

## <a name="how-it-works"></a>Funktionsweise

![mini-solution-voice-to-tweet-diagram](media/iot-hub-arduino-iot-devkit-az3166-translator/diagram.png)

Das IoT-DevKit zeichnet Ihre Stimme auf und sendet dann eine HTTP-Anforderung zum Auslösen von Azure Functions. Azure Functions ruft die Sprachübersetzer-API von Cognitive Services für die Übersetzung auf. Nachdem Azure Functions den Übersetzungstext abgerufen hat, wird eine C2D-Nachricht an das Gerät gesendet. Anschließend wird die Übersetzung auf dem Bildschirm angezeigt.

## <a name="problems-and-feedback"></a>Probleme und Feedback

Wenn Probleme auftreten, lesen Sie die [häufig gestellten Fragen zum IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/), oder wenden Sie sich über folgende Kanäle an uns:

* [Gitter.im](https://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Nächste Schritte

Sie haben erfahren, wie Sie das IoT DevKit mithilfe von Azure Functions und Cognitive Services als Übersetzer verwenden können. In diesen Anweisungen wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Verwenden einer Visual Studio Code-Aufgabe zum Automatisieren von Cloudbereitstellungen
> * Konfigurieren einer Verbindungszeichenfolge für Azure IoT-Geräte
> * Bereitstellen der Azure-Funktions-App
> * Testen der Übersetzung der Sprachnachricht

Wechseln Sie zu den anderen Tutorials, um Folgendes zu lernen:

> [!div class="nextstepaction"]
> [Verbinden von IoT DevKit AZ3166 mit dem Solution Accelerator für die Azure IoT-Remoteüberwachung](./iot-hub-arduino-iot-devkit-az3166-devkit-remote-monitoring.md)