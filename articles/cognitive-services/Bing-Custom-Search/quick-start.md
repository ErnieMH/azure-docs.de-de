---
title: 'Schnellstart: Erstellen der ersten Instanz der benutzerdefinierten Bing-Suche'
titleSuffix: Azure Cognitive Services
description: Verwenden Sie diese Schnellstartanleitung zum Erstellen einer Instanz der benutzerdefinierten Bing-Suche, mit der von Ihnen definierte Domänen und Webseiten durchsucht werden können.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 03/24/2020
ms.author: aahi
ms.openlocfilehash: b8287250df4e278d4904e31121ed7d2df208e1c9
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "80238853"
---
# <a name="quickstart-create-your-first-bing-custom-search-instance"></a>Schnellstart: Erstellen Ihrer ersten Instanz der benutzerdefinierten Bing-Suche

Zum Verwenden einer benutzerdefinierten Bing-Suche müssen Sie eine Instanz der benutzerdefinierten Suche erstellen, mit der Ihre Ansicht bzw. das Segment des Webs definiert wird. Diese Instanz enthält die öffentlichen Domänen, Websites und Webseiten, die Sie durchsuchen möchten, sowie alle Rangfolgenanpassungen, die Sie ggf. durchführen. 

Verwenden Sie zum Erstellen der Instanz das [Portal für die benutzerdefinierte Bing-Suche](https://customsearch.ai). 

![Abbildung: Portal für die benutzerdefinierte Bing-Suche](media/blockedCustomSrch.png)

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## <a name="create-a-custom-search-instance"></a>Erstellen einer Instanz für die benutzerdefinierte Suche

Erstellen Sie wie folgt eine Instanz für die benutzerdefinierte Bing-Suche:

1. Klicken Sie auf der Webseite mit dem [Portal für die benutzerdefinierte Bing-Suche](https://customsearch.ai) auf **Get Started** (Jetzt einsteigen), und melden Sie sich an Ihrem Microsoft-Konto an.

2. Klicken Sie auf **Neue Instanz**, und geben Sie einen aussagekräftigen Namen ein. Sie können den Namen Ihrer Instanz jederzeit ändern.
 
3. Geben Sie auf der **Suchoberfläche** auf der Registerkarte **Aktiv** die URL für mindestens eine Website ein, die Sie in Ihre Suche einbeziehen möchten. 

    > [!NOTE]
    > Instanzen der benutzerdefinierten Bing-Suche geben nur Ergebnisse für Domänen sowie für Webseiten zurück, die öffentlich sind und von Bing indiziert wurden.

4. Sie können im Portal für die benutzerdefinierte Bing-Suche auf der rechten Seite eine Abfrage eingeben und die Suchergebnisse untersuchen, die von Ihrer Suchinstanz zurückgegeben werden. Wenn keine Ergebnisse zurückgegeben werden, können Sie versuchen, eine andere URL einzugeben.  

5. Klicken Sie auf **Veröffentlichen**, um Ihre Änderungen an der Produktionsumgebung zu veröffentlichen, und aktualisieren Sie die Endpunkte der Instanz.

6.  Klicken Sie auf die Registerkarte **Produktion** unter **Endpunkte**, und kopieren Sie Ihre **Benutzerdefinierte Konfigurations-ID**. Sie benötigen diese ID zum Aufrufen der API für die benutzerdefinierte Bing-Suche, indem Sie sie in Ihren Aufrufen an den Abfrageparameter `customconfig=` anfügen.


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Schnellstart: Aufrufen eines Endpunkts für die benutzerdefinierte Bing-Suche](./call-endpoint-csharp.md)
