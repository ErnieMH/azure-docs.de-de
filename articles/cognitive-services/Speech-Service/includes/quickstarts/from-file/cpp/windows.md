---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/04/2020
ms.author: trbye
ms.openlocfilehash: e3eac0311b8b24c8c6e1fbf4d8cae55c360a7352
ms.sourcegitcommit: d95cab0514dd0956c13b9d64d98fdae2bc3569a0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91377469"
---
## <a name="prerequisites"></a>Voraussetzungen

Führen Sie die folgenden Schritte aus, bevor Sie beginnen:

> [!div class="checklist"]
> * [Erstellen einer Azure Speech-Ressource](../../../../overview.md#try-the-speech-service-for-free)
> * [Einrichten Ihrer Entwicklungsumgebung und Erstellen eines leeren Projekts](../../../../quickstarts/setup-platform.md?tabs=windows&pivots=programming-language-cpp)

[!INCLUDE [Audio input format](~/articles/cognitive-services/speech-service/includes/audio-input-format-chart.md)]

## <a name="add-sample-code"></a>Hinzufügen von Beispielcode

1. Öffnen Sie die Quelldatei **helloworld.cpp**.

1. Ersetzen Sie den gesamten Code durch den folgenden Codeausschnitt:
   
   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-file/helloworld/helloworld.cpp#code)]

1. Ersetzen Sie in der gleichen Datei die Zeichenfolge `YourSubscriptionKey` durch Ihren Abonnementschlüssel.

1. Ersetzen Sie die Zeichenfolge `YourServiceRegion` durch den **Regionsbezeichner** der [Region](https://aka.ms/speech/sdkregion), die mit Ihrem Abonnement verknüpft ist.

1. Ersetzen Sie die Zeichenfolge `whatstheweatherlike.wav` durch Ihren eigenen Dateinamen.

1. Wählen Sie auf der Menüleiste **Datei** > **Alle speichern** aus.

> [!NOTE]
> Das Speech SDK verwendet für die Erkennung standardmäßig amerikanisches Englisch (en-us). Informationen zum Auswählen der Ausgangssprache finden Sie unter [Angeben der Ausgangssprache für die Spracherkennung](../../../../how-to-specify-source-language.md).

## <a name="build-and-run-the-application"></a>Erstellen und Ausführen der Anwendung

1. Wählen Sie auf der Menüleiste **Erstellen** > **Projektmappe erstellen** aus, um die Anwendung zu erstellen. Der Code sollte nun ohne Fehler kompiliert werden.

1. Wählen Sie **Debuggen** > **Debuggen starten** aus (oder drücken Sie**F5**), um die Anwendung **helloworld** zu starten.

1. Ihre Audiodatei wird an den Speech-Dienst übermittelt, und die erste Äußerung in der Datei wird in Text transkribiert, der in demselben Fenster angezeigt wird.

   ```text
   Recognizing first result...
   We recognized: What's the weather like?
   ```

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]