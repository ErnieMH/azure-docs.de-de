---
title: 'Schnellstart: Übersetzen von Sprache in Text, C# (UWP) – Speech-Dienst'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: lisaweixu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 04/04/2020
ms.author: jhakulin
ms.topic: include
ms.openlocfilehash: cdc1bfcc7c2ea0cc51fe830c5218cf10cae7d990
ms.sourcegitcommit: 152c522bb5ad64e5c020b466b239cdac040b9377
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2020
ms.locfileid: "88226424"
---
## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden Schritte aus, bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../get-started.md)
> * [Einrichten Ihrer Entwicklungsumgebung und Erstellen eines leeren Projekts](../../../../quickstarts/setup-platform.md?tabs=uwp&pivots=programming-language-csharp)

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

Fügen Sie nun den XAML-Code, der die Benutzeroberfläche der Anwendung definiert, sowie die C#-CodeBehind-Implementierung hinzu.

1. Öffnen Sie `MainPage.xaml` im **Projektmappen-Explorer**.

1. Fügen Sie in der XAML-Ansicht des Designers den folgenden XAML-Ausschnitt in das **Grid**-Tag (zwischen `<Grid>` und `</Grid>`) ein.

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/translate-speech-to-text/helloworld/MainPage.xaml#StackPanel)]

1. Öffnen Sie im **Projektmappen-Explorer** die CodeBehind-Quelldatei `MainPage.xaml.cs`. (Sie befindet sich unter `MainPage.xaml`.)

1. Ersetzen Sie den gesamten darin enthaltenen Code durch den folgenden Codeausschnitt:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/translate-speech-to-text/helloworld/MainPage.xaml.cs#code)]

1. Suchen Sie in dieser Datei im Handler `SpeechTranslationFromMicrophone_ButtonClicked` nach der Zeichenfolge `YourSubscriptionKey`, und ersetzen Sie sie durch Ihren Abonnementschlüssel.

1. Suchen Sie im Handler `SpeechTranslationFromMicrophone_ButtonClicked` nach der Zeichenfolge `YourServiceRegion`, und ersetzen Sie sie durch die [Region](~/articles/cognitive-services/Speech-Service/regions.md), die mit Ihrem Abonnement verknüpft ist.

1. Wählen Sie auf der Menüleiste **Datei** > **Alle speichern** aus, um Ihre Änderungen zu speichern.

## <a name="build-and-run-the-application"></a>Erstellen und Ausführen der Anwendung

Nun können Sie Ihre Anwendung erstellen und testen.

1. Wählen Sie auf der Menüleiste **Erstellen** > **Projektmappe erstellen** aus, um die Anwendung zu erstellen. Der Code sollte nun ohne Fehler kompiliert werden.

1. Wählen Sie **Debuggen** > **Debuggen starten** aus (oder drücken Sie**F5**), um die Anwendung zu starten. Das Fenster **helloworld** wird angezeigt.

   ![UWP-Beispielanwendung für die Übersetzung in C#: Schnellstart](~/articles/cognitive-services/Speech-Service/media/sdk/qs-translate-speech-uwp-helloworld-window.png)

1. Wählen Sie **Mikrofon aktivieren** und dann in der Zugriffsberechtigungsanforderung **Ja** aus.

   ![Berechtigungsanforderung für den Mikrofonzugriff](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-10-access-prompt.png)

1. Wählen Sie **Translate speech from the microphone input** (Sprachübersetzung aus Mikrofoneingabe) aus, und sprechen Sie einen englischen Ausdruck oder Satz in das Mikrofon Ihres Geräts. Die Anwendung überträgt ihn an die Speech-Dienste, die ihn in Text in einer anderen Sprache (in diesem Fall: Deutsch) übersetzen. Der Speech-Dienst sendet den übersetzten Text zurück an die Anwendung, die die Übersetzung im Fenster anzeigt.

   ![Benutzeroberfläche der Sprachübersetzung](~/articles/cognitive-services/Speech-Service/media/sdk/qs-translate-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [footer](./footer.md)]
