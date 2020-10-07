---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.custom: devx-track-js
ms.openlocfilehash: f8c7b9ee90e947534d6f938b1eb22f58b57270e6
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91377514"
---
:::row:::
    :::column span="3":::
        Das Speech-SDK für JavaScript ist als npm-Paket verfügbar. Weitere Informationen finden Sie unter <a href="https://www.npmjs.com/package/microsoft-cognitiveservices-speech-sdk" target="_blank">microsoft-cognitiveservices-speech-sdk <span class="docon docon-navigate-external x-hidden-focus"></span></a> und im zugehörigen GitHub-Repository <a href="https://github.com/Microsoft/cognitive-services-speech-sdk-js" target="_blank">cognitive-services-speech-sdk-js <span class="docon docon-navigate-external x-hidden-focus"></span></a>.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="JavaScript" src="https://docs.microsoft.com/media/logos/logo_js.svg"  width="60px">
        </div>
    :::column-end:::
:::row-end:::

> [!TIP]
> Obwohl das Speech SDK for JavaScript als npm-Paket verfügbar ist und daher sowohl von Clientwebbrowsern als auch von Node.js genutzt werden kann, beachten Sie die verschiedenen Auswirkungen der einzelnen Umgebungen auf die Architektur. Beispielsweise ist das <a href="https://en.wikipedia.org/wiki/Document_Object_Model" target="_blank">Dokumentobjektmodell (DOM) <span class="docon docon-navigate-external x-hidden-focus"></span></a> nicht für serverseitige Anwendungen verfügbar, ebenso wie das <a href="https://nodejs.org/api/fs.html" target="_blank">Dateisystem <span class="docon docon-navigate-external x-hidden-focus"></span></a> nicht für clientseitige Anwendungen verfügbar ist.

### <a name="nodejs-package-manager-npm"></a>Node.js-Paket-Manager (NPM)

Führen Sie den folgenden Befehl `npm install` aus, um das Speech SDK für JavaScript zu installieren.

```nodejs
npm install microsoft-cognitiveservices-speech-sdk
```

### <a name="html-script-tag"></a>HTML-Skripttag

Alternativ dazu können Sie ein `<script>`-Tag direkt im `<head>`-Element des HTML-Codes einfügen, das das <a href="https://www.jsdelivr.com/package/npm/microsoft-cognitiveservices-speech-sdk" target="_blank">**JSDelivr**-NPM-Syndikat <span class="docon docon-navigate-external x-hidden-focus"></span></a> nutzt.

```html
<script src="https://cdn.jsdelivr.net/npm/microsoft-cognitiveservices-speech-sdk@latest/distrib/browser/microsoft.cognitiveservices.speech.sdk.bundle-min.js">
</script>
```

Weitere Informationen finden Sie unter <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/browser" target="_blank">Quickstart: Recognize speech in JavaScript on a Web Browser <span class="docon docon-navigate-external x-hidden-focus"></span></a> (Schnellstart: Spracherkennung in JavaScript in einem Webbrowser).
