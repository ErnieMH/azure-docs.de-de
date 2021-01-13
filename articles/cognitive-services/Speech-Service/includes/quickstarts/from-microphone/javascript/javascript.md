---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: trbye
ms.openlocfilehash: a110ff1db9b75287eb1b75d80003796354aba35b
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86035867"
---
## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen:

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Erstellen einer Azure Speech-Ressource<span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [Einrichten Ihrer Entwicklungsumgebung und Erstellen eines leeren Projekts](../../../../quickstarts/setup-platform.md)
> * Stellen Sie sicher, dass Sie Zugriff auf ein Mikrofon für die Audioaufnahme haben.

## <a name="create-a-new-website-folder"></a>Erstellen eines neuen Websiteordners

Erstellen Sie einen neuen leeren Ordner. Wenn Sie das Beispiel auf einem Webserver hosten möchten, stellen Sie sicher, dass der Webserver auf den Ordner zugreifen kann.

## <a name="unpack-the-speech-sdk-for-javascript-into-that-folder"></a>Entpacken des Speech SDK für JavaScript in diesen Ordner

Laden Sie das Speech SDK als [ZIP-Paket](https://aka.ms/csspeech/jsbrowserpackage) herunter, und entpacken Sie es in den neu erstellten Ordner. Dabei werden zwei Dateien entpackt: `microsoft.cognitiveservices.speech.sdk.bundle.js` und `microsoft.cognitiveservices.speech.sdk.bundle.js.map`.
Die zweite Datei ist optional und hilfreich beim Debuggen im SDK-Code.

## <a name="create-an-indexhtml-page"></a>Erstellen einer Seite „index.html“

Erstellen Sie eine neue Datei im Ordner `index.html`, und öffnen Sie diese Datei mit einem Text-Editor.

1. Erstellen Sie das folgende HTML-Gerüst:

 [!code-html [SampleCode](~/samples-cognitive-services-speech-sdk/quickstart/javascript/browser/index-from-mic.html)]

## <a name="create-the-token-source-optional"></a>Erstellen der Tokenquelle (optional)

Wenn Sie die Webseite auf einem Webserver hosten möchten, können Sie optional eine Tokenquelle für die Demoanwendung bereitstellen.
Auf diese Weise verlässt Ihr Abonnementschlüssel nie den Server, während Benutzer gleichzeitig die Sprachfunktionen verwenden können, ohne selbst Autorisierungscode eingeben zu müssen.

Erstellen Sie eine neue Datei mit dem Namen `token.php`. In diesem Beispiel wird davon ausgegangen, dass der Webserver die PHP-Skriptsprache mit aktiviertem cURL unterstützt. Geben Sie den folgenden Code ein:

```php
<?php
header('Access-Control-Allow-Origin: ' . $_SERVER['SERVER_NAME']);

// Replace with your own subscription key and service region (e.g., "westus").
$subscriptionKey = 'YourSubscriptionKey';
$region = 'YourServiceRegion';

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, 'https://' . $region . '.api.cognitive.microsoft.com/sts/v1.0/issueToken');
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, '{}');
curl_setopt($ch, CURLOPT_HTTPHEADER, array('Content-Type: application/json', 'Ocp-Apim-Subscription-Key: ' . $subscriptionKey));
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
echo curl_exec($ch);
?>
```

> [!NOTE]
> Autorisierungstoken haben nur eine begrenzte Lebensdauer.
> Dieses vereinfachte Beispiel zeigt nicht, wie Autorisierungstoken automatisch aktualisiert werden. Als Benutzer können Sie die Seite manuell laden oder F5 drücken, um sie zu aktualisieren.

## <a name="build-and-run-the-sample-locally"></a>Erstellen und lokales Ausführen des Beispiels

Um die App zu starten, doppelklicken Sie auf die Datei „index.html“, oder öffnen Sie die Datei „index.html“ mit Ihren bevorzugten Browser. Es wird eine einfache Oberfläche angezeigt, auf der Sie Ihren Abonnementschlüssel und die [Region](../../../../regions.md) eingeben und eine Erkennung über das Mikrofon auslösen können.

> [!NOTE]
> Diese Methode kann für den Safari-Browser nicht verwendet werden.
> Für Safari muss die Beispielwebseite auf einem Webserver gehostet werden. In Safari können Websites, die aus einer lokalen Datei geladen werden, das Mikrofon nicht verwenden.

## <a name="build-and-run-the-sample-via-a-web-server"></a>Erstellen und Ausführen des Beispiels über einen Webserver

Starten Sie Ihre App, öffnen Sie Ihren bevorzugten Browser, und verweisen Sie in diesem auf die öffentliche URL, unter der Sie den Ordner hosten. Geben Sie Ihre [Region](../../../../regions.md) ein, und lösen Sie über das Mikrofon eine Erkennung aus. Wenn dies konfiguriert wurde, wird ein Token aus der Tokenquelle abgerufen.


## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]

