---
title: Erste Schritte mit Azure AD in Visual Studio-WebApi-Projekten
description: Erfahren Sie etwas über die ersten Schritte mit Azure Active Directory in WebApi-Projekten nach dem Herstellen einer Verbindung oder dem Erstellen eines Azure AD mithilfe von verbundenen Visual Studio-Diensten.
author: ghogen
manager: jillfra
ms.prod: visual-studio-windows
ms.technology: vs-azure
ms.workload: azure-vs
ms.topic: how-to
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: devx-track-csharp, aaddev, vs-azure
ms.openlocfilehash: fb8c1f25c8a22c96679a0310353f474f6b9d5ba5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88165429"
---
# <a name="get-started-with-azure-active-directory-webapi-projects"></a>Erste Schritte mit Azure Active Directory (WebApi-Projekte)

> [!div class="op_single_selector"]
> - [Erste Schritte](vs-active-directory-webapi-getting-started.md)
> - [Was ist passiert?](vs-active-directory-webapi-what-happened.md)

Dieser Artikel enthält weiterführende Informationen, nachdem Sie Active Directory in Visual Studio über **Projekt > Verbundene Dienste** einem ASP.NET-WebAPI-Projekt hinzugefügt haben. Sollten Sie den Dienst Ihrem Projekt noch nicht hinzugefügt haben, können Sie dies jederzeit nachholen.

Informationen zu den Änderungen, die an Ihrem Projekt vorgenommen werden, wenn der verbundene Dienst hinzugefügt wird, finden Sie unter [Was ist mit meinem WebAPI-Projekt passiert?](vs-active-directory-webapi-what-happened.md).

## <a name="requiring-authentication-to-access-controllers"></a>Erfordern von Authentifizierung für den Zugriff auf Controller

Alle Controller in Ihrem Projekt wurden mit dem Attribut `[Authorize]` versehen. Dieses Attribut erfordert, dass der Benutzer vor dem Zugriff auf die durch diese Controller definierten APIs authentifiziert werden muss. Wenn Sie anonymen Zugriff auf diesen Controller erlauben möchten, entfernen Sie dieses Attribut vom Controller. Wenn Sie die Berechtigungen präziser festlegen möchten, wenden Sie das Attribut auf jede Methode an, die Autorisierung erfordert, anstatt es auf die Controllerklasse anzuwenden.

## <a name="next-steps"></a>Nächste Schritte

- [Authentifizierungsszenarien für Azure Active Directory](./authentication-vs-authorization.md)
- [Hinzufügen von „Mit Microsoft anmelden“ zu einer ASP.NET-Web-App](quickstart-v2-aspnet-webapp.md)