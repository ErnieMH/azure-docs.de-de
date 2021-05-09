---
title: Zugreifen auf Azure AD-App-Proxy-Apps in Teams | Microsoft-Dokumentation
description: Verwenden Sie den Azure AD-Anwendungsproxy, um über Microsoft Teams auf Ihre lokale Anwendung zuzugreifen.
services: active-directory
documentationcenter: ''
author: kenwith
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 09/05/2017
ms.author: kenwith
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: b2834dff789aac95e19f7d1229fc3b3c9449688b
ms.sourcegitcommit: a5dd9799fa93c175b4644c9fe1509e9f97506cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2021
ms.locfileid: "108224481"
---
# <a name="access-your-on-premises-applications-through-microsoft-teams"></a>Zugreifen auf lokale Anwendungen über Microsoft Teams

Der Azure Active Directory-Anwendungsproxy ermöglicht ortsunabhängiges einmaliges Anmelden bei lokalen Anwendungen. Microsoft Teams optimiert die Zusammenarbeit an einem zentralen Ort. Durch die Integration beider Lösungen können Benutzer in jeder Situation produktiv mit ihren Teamkollegen zusammenarbeiten.

Benutzer können ihren Teams-Kanälen [mithilfe von Registerkarten](https://support.office.com/article/Video-Using-Tabs-7350a03e-017a-4a00-a6ae-1c9fe8c497b3?ui=en-US&rs=en-US&ad=US) Cloud-Apps hinzufügen. Aber was, wenn die von allen verwendete SharePoint-Websites oder das von allen verwendete Planungstool lokal gehostet wird? Dieses Problem wird durch den Anwendungsproxy gelöst. Benutzer können Ihren Kanälen Apps hinzufügen, die über den Anwendungsproxy veröffentlicht wurden. Dabei können sie die gleichen externen URLs verwenden wie beim Remotezugriff auf ihre Apps. Und da die Authentifizierung des Anwendungsproxys über Azure Active Directory erfolgt, können die Benutzer das einmalige Anmelden verwenden.

## <a name="install-the-application-proxy-connector-and-publish-your-app"></a>Installieren des Anwendungsproxyconnectors und Veröffentlichen der App

[Konfigurieren Sie den Anwendungsproxy für Ihren Mandanten, und installieren Sie den Connector](../app-proxy/application-proxy-add-on-premises-application.md) (sofern noch nicht geschehen). Veröffentlichen Sie Ihre lokale Anwendung, um Remotezugriff zu ermöglichen. Wenn Sie die App veröffentlichen, notieren Sie sich die externe URL, da damit die App Teams hinzugefügt wird.

Falls Sie Ihre Apps bereits veröffentlicht haben und sich nicht mehr an die externen URLs erinnern, können Sie sie im [Azure-Portal](https://portal.azure.com) nachschauen. Melden Sie sich an, navigieren Sie zu **Azure Active Directory** > **Unternehmensanwendungen** > **Alle Anwendungen**, wählen Sie Ihre App aus, und wählen Sie schließlich **Anwendungsproxy** aus.

## <a name="add-your-app-to-teams"></a>Hinzufügen Ihrer App zu Teams

Nachdem Sie die App über den Anwendungsproxy veröffentlicht haben, teilen Sie Ihren Benutzern mit, dass sie sie ihren Teams-Kanälen direkt als Registerkarte hinzufügen können und dass die App dann von allen im Team verwendet werden kann. Dazu müssen die Benutzer die folgenden drei Schritte ausführen:

1. Navigieren Sie zu dem Teams-Kanal, unter dem Sie die App hinzufügen möchten, und wählen Sie **+** aus, um eine Registerkarte hinzuzufügen.

   ![Auswählen des Pluszeichens (+) zum Hinzufügen einer Registerkarte in Teams](./media/application-proxy-integrate-with-teams/add-tab.png)

1. Wählen Sie für die Registerkarte die Option **Website** aus.

   ![Auswählen einer Website auf dem Bildschirm „Registerkarte hinzufügen“](./media/application-proxy-integrate-with-teams/website.png)

1. Benennen Sie die Registerkarte, und legen Sie die URL auf die externe Anwendungsproxy-URL fest.

   ![Benennen der Registerkarte und Hinzufügen der externen URL](./media/application-proxy-integrate-with-teams/tab-name-url.png)

Nachdem ein Mitglied eines Teams die Registerkarte hinzugefügt hat, wird sie für alle Benutzer des Kanals angezeigt. Alle Benutzer mit Zugriff auf die App können beim Zugreifen auf die App einmaliges Anmelden mit den Anmeldeinformationen nutzen, die sie auch für Microsoft Teams verwenden. Benutzern, die keinen Zugriff auf die App haben, wird die Registerkarte zwar in Teams angezeigt, sie können aber nur darauf zugreifen, wenn Sie ihnen Berechtigungen für die lokale App sowie für die über das Azure-Portal veröffentlichte Version der App erteilen.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über das [Veröffentlichen lokaler SharePoint-Websites](application-proxy-integrate-with-sharepoint-server.md) mit dem Anwendungsproxy.
- Konfigurieren Sie Ihre Apps für die Verwendung von [benutzerdefinierten Domänen](application-proxy-configure-custom-domain.md) für die externe URL.
