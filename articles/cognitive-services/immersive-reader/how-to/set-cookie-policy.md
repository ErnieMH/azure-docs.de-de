---
title: Festlegen der Cookierichtlinie für den plastischen Reader
titleSuffix: Azure Cognitive Services
description: In diesem Artikel wird gezeigt, wie Sie die Cookierichtlinie für den plastischen Reader festlegen.
services: cognitive-services
author: nitinme
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/06/2020
ms.author: nitinme
ms.custom: devx-track-js
ms.openlocfilehash: 02901e6b4a75257b2cda8dee84dbe3438dc15c18
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91332371"
---
# <a name="how-to-set-the-cookie-policy-for-the-immersive-reader"></a>Vorgehensweise: Festlegen der Cookierichtlinie für den plastischen Reader

Der plastische Reader deaktiviert standardmäßig die Verwendung von Cookies. Wenn Sie die Cookieverwendung aktivieren, kann der plastische Reader Cookies verwenden, um Benutzereinstellungen beizubehalten und die Verwendung von Features zu verfolgen. Wenn Sie die Cookieverwendung im plastischen Reader aktivieren, sollten Sie die Anforderungen der Cookiekonformitätsrichtlinie der EU beachten. Es liegt in der Verantwortung der Hostanwendung, die erforderliche Zustimmung des Benutzers in Übereinstimmung mit der Cookiekonformitätsrichtlinie der EU einzuholen.

Die Cookierichtlinie kann über die [Optionen](../reference.md#options) für den plastischen Reader festgelegt werden.

## <a name="enable-cookie-usage"></a>Aktivieren der Cookienutzung

```javascript
var options = {
    'cookiePolicy': ImmersiveReader.CookiePolicy.Enable
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

## <a name="disable-cookie-usage"></a>Deaktivieren der Cookienutzung

```javascript
var options = {
    'cookiePolicy': ImmersiveReader.CookiePolicy.Disable
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

## <a name="next-steps"></a>Nächste Schritte

* Sehen Sie sich die [Node.js-Schnellstartanleitung](../quickstarts/client-libraries.md?pivots=programming-language-nodejs) an, um zu erfahren, welche weiteren Möglichkeiten das SDK für den plastischen Reader in Verbindung mit Node.js bietet.
* Sehen Sie sich das [Android-Tutorial](../tutorial-android.md) an, um zu erfahren, welche weiteren Möglichkeiten das SDK für den plastischen Reader in Verbindung mit Java oder Kotlin für Android bietet.
* Sehen Sie sich das [iOS-Tutorial](../tutorial-ios.md) an, um zu erfahren, welche weiteren Möglichkeiten das SDK für den plastischen Reader in Verbindung mit Swift für iOS bietet.
* Sehen Sie sich das [Python-Tutorial](../tutorial-python.md) an, um zu erfahren, welche weiteren Möglichkeiten das SDK für den plastischen Reader in Verbindung mit Python bietet.
* Machen Sie sich mit dem [SDK für Plastischer Reader](https://github.com/microsoft/immersive-reader-sdk) und der [zugehörigen Referenz](../reference.md) vertraut.