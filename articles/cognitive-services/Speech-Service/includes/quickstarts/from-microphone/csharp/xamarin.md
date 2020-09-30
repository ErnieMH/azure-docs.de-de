---
title: 'Schnellstart: Erkennen von Spracheingaben per Mikrofon, C# (Xamarin) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erstellen Sie mithilfe des Cognitive Services Speech SDK eine plattformübergreifende C#-Xamarin-Anwendung für die universelle Windows-Plattform (UWP), Android und iOS. Sie transkribieren über das Mikrofon des Geräts oder Simulators Sprache in Echtzeit in Text. Die Anwendung basiert auf dem NuGet-Paket für das Speech SDK und auf Microsoft Visual Studio 2019.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/02/2020
ms.author: erhopf
ms.custom: devx-track-csharp
ms.openlocfilehash: f89bdbfd144fb327c1daae020a1e371c00045859
ms.sourcegitcommit: d95cab0514dd0956c13b9d64d98fdae2bc3569a0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91376461"
---
## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../overview.md#try-the-speech-service-for-free)
> * [Einrichten Ihrer Entwicklungsumgebung und Erstellen eines leeren Projekts](../../../../quickstarts/setup-platform.md?tabs=xamarin&pivots=programming-language-csharp)
> * Stellen Sie sicher, dass Sie Zugriff auf ein Mikrofon für die Audioaufnahme haben.

Wenn Sie dies bereits getan haben, umso besser. Wir fahren fort.

## <a name="add-sample-code-for-the-common-helloworld-project"></a>Hinzufügen von Beispielcode für das allgemeine helloworld-Projekt

Das allgemeine helloworld-Projekt enthält plattformunabhängige Implementierungen für Ihre plattformübergreifende Anwendung. Fügen Sie nun den XAML-Code, der die Benutzeroberfläche der Anwendung definiert, sowie den C#-Code hinter der Implementierung hinzu.

1. Öffnen Sie im **Projektmappen-Explorer** unter dem allgemeinen helloworld-Projekt die Datei `MainPage.xaml`.

1. Fügen Sie in der Designer-XAML-Ansicht den folgenden XAML-Ausschnitt in das **Grid**-Tag zwischen `<StackLayout>` und `</StackLayout>` ein:

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld/MainPage.xaml)]

1. Öffnen Sie im **Projektmappen-Explorer** die CodeBehind-Quelldatei `MainPage.xaml.cs`. Sie befindet sich unter `MainPage.xaml`.

1. Ersetzen Sie den gesamten darin enthaltenen Code durch den folgenden Codeausschnitt:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld/MainPage.xaml.cs)]

1. Suchen Sie im Handler `OnRecognitionButtonClicked` der Quelldatei nach der Zeichenfolge `YourSubscriptionKey`, und ersetzen Sie sie durch Ihren Abonnementschlüssel.


1. Suchen Sie im Handler `OnRecognitionButtonClicked` nach der Zeichenfolge `YourServiceregion`, und ersetzen Sie sie durch den **Regionsbezeichner** der [Region](https://aka.ms/speech/sdkregion), die mit Ihrem Abonnement verknüpft ist. 

1. Als Nächstes müssen Sie einen [Xamarin-Dienst](https://docs.microsoft.com/xamarin/android/app-fundamentals/services/creating-a-service/) erstellen. Dieser Dienst wird zum Abfragen von Mikrofonberechtigungen von Projekten verschiedener Plattformen wie UWP, Android und iOS verwendet. Fügen Sie hierzu unter dem helloworld-Projekt einen neuen Ordner mit dem Namen *Services* hinzu, und erstellen Sie darin eine neue C#-Quelldatei. Klicken Sie mit der rechten Maustaste auf den Ordner *Services*, und wählen Sie **Hinzufügen** > **Neues Element** > **Codedatei** aus. Benennen Sie die Datei in `IMicrophoneService.cs` um, und fügen Sie den gesamten Code aus dem folgenden Codeausschnitt in die Datei ein:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld/Services/IMicrophoneService.cs)]

#### <a name="android"></a>[Android](#tab/x-android)
## <a name="add-sample-code-for-the-helloworldandroid-project"></a>Hinzufügen von Beispielcode für das Projekt `helloworld.Android`

Fügen Sie nun den C#-Code hinzu, der den Android-spezifischen Teil der Anwendung definiert.

1. Öffnen Sie im **Projektmappen-Explorer** unter dem helloworld.Android-Projekt die Datei `MainActivity.cs`.

1. Ersetzen Sie den gesamten darin enthaltenen Code durch den folgenden Codeausschnitt:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld.Android/MainActivity.cs)]

1. Fügen Sie als Nächstes eine Android-spezifische Implementierung für `MicrophoneService` hinzu, indem Sie unter dem helloworld.Android-Projekt den neuen Ordner *Services* erstellen. Erstellen Sie anschließend darin eine neue C# Quelldatei. Benennen Sie die Datei in `MicrophoneService.cs` um. Kopieren Sie den folgenden Codeausschnitt, und fügen Sie ihn in die Datei ein:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld.Android/Services/MicrophoneService.cs)]

1. Öffnen Sie anschließend `AndroidManifest.xml` unter dem Ordner *Eigenschaften*. Fügen Sie die folgende Einstellung für „uses-permission“ für das Mikrofon zwischen `<manifest>` und `</manifest>` ein:

   ```xml
   <uses-permission android:name="android.permission.RECORD_AUDIO" />
   ```
   
#### <a name="ios"></a>[iOS](#tab/ios)
## <a name="add-sample-code-for-the-helloworldios-project"></a>Hinzufügen von Beispielcode für das Projekt `helloworld.iOS`

Fügen Sie nun den C#-Code hinzu, der den iOS-spezifischen Teil der Anwendung definiert. Erstellen Sie außerdem für Apple-Geräte spezifische Konfigurationen für das helloworld.iOS-Projekt.

1. Öffnen Sie im **Projektmappen-Explorer** unter dem helloworld.iOS-Projekt die Datei `AppDelegate.cs`.

1. Ersetzen Sie den gesamten darin enthaltenen Code durch den folgenden Codeausschnitt:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld.iOS/AppDelegate.cs)]

1. Fügen Sie als Nächstes eine iOS-spezifische Implementierung für `MicrophoneService` hinzu, indem Sie unter dem helloworld.iOS-Projekt den neuen Ordner *Services* erstellen. Erstellen Sie anschließend darin eine neue C# Quelldatei. Benennen Sie die Datei in `MicrophoneService.cs` um. Kopieren Sie den folgenden Codeausschnitt, und fügen Sie ihn in die Datei ein:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld.iOS/Services/MicrophoneService.cs)]

1. Öffnen Sie `Info.plist` unter dem helloworld.iOS-Projekt im Text-Editor. Fügen Sie im Abschnitt „dict“ das folgende Schlüssel-Wert-Paar hinzu:

   <key>NSMicrophoneUsageDescription</key>
   <string>Diese Beispiel-App benötigt Mikrofonzugriff.</string>

   > [!NOTE]
   > Wenn Sie für ein iPhone-Gerät entwickeln, stellen Sie sicher, dass `Bundle Identifier` mit der App-ID des Bereitstellungsprofils Ihres Geräts übereinstimmt. Andernfalls tritt bei der Erstellung ein Fehler auf. Bei der Verwendung von iPhoneSimulator können Sie den Wert beibehalten.

1. Bei der Entwicklung auf einem Windows-PC müssen Sie über **Tools** > **iOS** > **Mit Mac koppeln** eine Verbindung mit dem Mac-Gerät herstellen. Führen Sie die Schritte des von Visual Studio bereitgestellten Assistenten aus, um eine Verbindung mit dem Mac-Gerät zu ermöglichen.

#### <a name="uwp"></a>[UWP](#tab/helloworlduwp)
## <a name="add-sample-code-for-the-helloworlduwp-project"></a>Hinzufügen von Beispielcode für das Projekt `helloworld.UWP`

## <a name="add-sample-code-for-the-helloworlduwp-project"></a>Hinzufügen von Beispielcode für das helloworld.UWP-Projekt

Fügen Sie nun den C#-Code hinzu, der den UWP-spezifischen Teil der Anwendung definiert.

1. Öffnen Sie im **Projektmappen-Explorer** unter dem helloworld.UWP-Projekt die Datei `MainPage.xaml.cs`.

1. Ersetzen Sie den gesamten darin enthaltenen Code durch den folgenden Codeausschnitt:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld.UWP/MainPage.xaml.cs)]

1. Fügen Sie als Nächstes eine UWP-spezifische Implementierung für `MicrophoneService` hinzu, indem Sie unter dem helloworld.UWP-Projekt den neuen Ordner *Services* erstellen. Erstellen Sie anschließend darin eine neue C# Quelldatei. Benennen Sie die Datei in `MicrophoneService.cs` um. Kopieren Sie den folgenden Codeausschnitt, und fügen Sie ihn in die Datei ein:

   [!code-csharp[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/xamarin/helloworld/helloworld.UWP/Services/MicrophoneService.cs)]

1. Doppelklicken Sie dann auf die Datei `Package.appxmanifest` unter dem helloworld.UWP-Projekt in Visual Studio. Stellen Sie sicher, dass unter **Funktionen** die Option **Mikrofon** ausgewählt ist, und speichern Sie die Datei.

1. Doppelklicken Sie dann in Visual Studio unter dem Projekt `helloworld.UWP` auf die Datei `Package.appxmanifest`, und überprüfen Sie, ob **Mikrofon** unter **Funktionen** aktiviert ist. Speichern Sie anschließend die Datei.
   > Hinweis: Falls die Warnung „Die Zertifikatdatei ist nicht vorhanden: helloworld.UWP_TemporaryKey.pfx“ angezeigt wird, finden Sie im Beispiel für die [Spracherkennung](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=uwp) weitere Informationen.

1. Wählen Sie auf der Menüleiste **Datei** > **Alle speichern** aus, um Ihre Änderungen zu speichern.

## <a name="build-and-run-the-uwp-application"></a>Erstellen und Ausführen der UWP-Anwendung

1. Legen Sie helloworld.UWP als Startprojekt fest. Klicken Sie mit der rechten Maustaste auf das helloworld.UWP-Projekt, und wählen Sie **Erstellen** aus, um die Anwendung zu erstellen.

1. Wählen Sie **Debuggen** > **Debuggen starten** aus (oder wählen Sie**F5** aus), um die Anwendung zu starten. Das Fenster **helloworld** wird angezeigt.

   ![UWP-Beispielanwendung für die Spracherkennung in C#: Schnellstart](../../../../media/sdk/qs-csharp-xamarin-helloworld-uwp-window.png)

1. Wählen Sie **Mikrofon aktivieren** aus. Wenn Zugriffsberechtigungen angefordert werden, wählen Sie **Ja** aus.

   ![Berechtigungsanforderung für den Mikrofonzugriff](../../../../media/sdk/qs-csharp-xamarin-uwp-access-prompt.png)

1. Wählen Sie **Spracherkennung starten** aus, und sprechen Sie einen englischen Ausdruck oder Satz in das Mikrofon Ihres Geräts. Ihre Spracheingabe wird an den Spracherkennungsdienst übermittelt und in Text transkribiert, der im Fenster angezeigt wird.

   ![Spracherkennungsbenutzeroberfläche](../../../../media/sdk/qs-csharp-xamarin-uwp-ui-result.png)
* * *

## <a name="build-and-run-the-android-and-ios-applications"></a>Erstellen und Ausführen der Android- und iOS-Anwendungen

Android- und iOS-Anwendungen werden auf dem Gerät oder im Simulator auf ähnliche Weise wie UWP-Anwendungen erstellt und ausgeführt. Stellen Sie sicher, dass alle im Abschnitt „Voraussetzungen“ dieses Dokuments aufgeführten SDKs ordnungsgemäß installiert sind.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]
