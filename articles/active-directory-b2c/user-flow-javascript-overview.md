---
title: JavaScript und Seitenlayoutversionen
titleSuffix: Azure AD B2C
description: Erfahren Sie, wie Sie in Azure Active Directory B2C JavaScript aktivieren und Seitenlayoutversionen verwenden.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 07/30/2020
ms.custom: project-no-code, devx-track-js
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 50644ad3be226648accba6a2f43d4ea068ff977c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91258845"
---
# <a name="javascript-and-page-layout-versions-in-azure-active-directory-b2c"></a>JavaScript und Seitenlayoutversionen in Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure AD B2C stellt eine Reihe von gepackten Inhalten bereit, die HTML, CSS und JavaScript für die Benutzeroberflächenelemente in Ihren Benutzerflows und benutzerdefinierten Richtlinien enthalten.

So aktivieren Sie JavaScript für Ihre Anwendungen

* Aktivieren Sie es im Benutzerflow über das Azure-Portal.
* Wählen Sie ein [Seitenlayout](page-layout.md) aus.
* Verwenden Sie [b2clogin.com](b2clogin.md) in Ihren Anforderungen.

Wenn Sie beabsichtigen, clientseitigen [JavaScript](javascript-samples.md)-Code zu aktivieren, müssen die Elemente, auf denen der JavaScript-Code basiert, unveränderlich sein. Wenn sie nicht unveränderlich sind, könnten Änderungen zu einem unerwarteten Verhalten auf Ihren Benutzerseiten führen. Um diese Probleme zu vermeiden, erzwingen Sie die Verwendung eines Seitenlayouts, und geben Sie eine Seitenlayoutversion an, um sicherzustellen, dass die Inhaltsdefinitionen, auf denen Ihr JavaScript basiert, unveränderlich sind. Auch wenn Sie nicht beabsichtigen, JavaScript zu aktivieren, können Sie eine Seitenlayoutversion für Ihre Seiten angeben.

## <a name="enable-javascript"></a>Aktivieren von JavaScript

Im Benutzerflow **Eigenschaften** können Sie JavaScript aktivieren. Das Aktivieren von JavaScript erzwingt auch die Verwendung eines Seitenlayouts. Anschließend können Sie die Seitenlayoutversion für den Benutzerflow festlegen, wie im nächsten Abschnitt beschrieben.

![Benutzerablaufeigenschaften-Seite mit hervorgehobener Einstellung „JavaScript aktivieren“](media/user-flow-javascript-overview/javascript-settings.png)

## <a name="select-a-page-layout-version"></a>Auswählen einer Seitenlayoutversion

Sie können eine Seitenlayoutversion für Ihre Benutzerflowseiten unabhängig davon angeben, ob Sie JavaScript in den Benutzerfloweigenschaften aktivieren oder nicht. Öffnen Sie den Benutzerflow, und wählen Sie **Seitenlayouts** aus. Wählen Sie unter **Layoutname** eine Benutzerflowseite und dann die Option **Seitenlayoutversion (Vorschau)** aus.

Informationen zu den verschiedenen Seitenlayoutversionen finden Sie im [Versionsänderungsprotokoll für das Seitenlayout](page-layout.md).

![Seitenlayouteinstellungen im Portal mit der Dropdownliste für Seitenlayouts](media/user-flow-javascript-overview/page-layout-version.png)

[!INCLUDE [active-directory-b2c-javascript-guidelines](../../includes/active-directory-b2c-javascript-guidelines.md)]

## <a name="next-steps"></a>Nächste Schritte

Beispiele für die JavaScript-Verwendung finden Sie in [JavaScript-Beispiele für die Verwendung in Azure Active Directory B2C](javascript-samples.md).
