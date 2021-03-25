---
title: Ändern des Tarifs bei Notification Hubs-Namespace | Microsoft-Dokumentation
description: Informationen zum Ändern des Tarifs eines Azure Notification Hubs-Namespace.
services: notification-hubs
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 08/03/2020
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 01/28/2019
ms.openlocfilehash: 1455259bc42aea9d506a9a2a19d725cac3d643f8
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "87562768"
---
# <a name="change-pricing-tier-of-an-azure-notification-hubs-namespace"></a>Ändern des Tarifs bei Azure Notification Hubs-Namespace

Notification Hubs wird in drei Tarifen angeboten: **Free**, **Basic** und **Standard**. In diesem Artikel wird das Ändern des Tarifs für einen Azure Notification Hubs-Namespace veranschaulicht.

## <a name="overview"></a>Übersicht

In Azure Notification Hubs ist ein *Benachrichtigungshub* die kleinste Ressource/Entität. Er ist in der Regel einer einzigen Anwendung zugeordnet und kann für jedes Plattformbenachrichtigungssystem (PNS), das für die App unterstützt wird, ein Zertifikat aufnehmen. Die Anwendung kann hybrid oder nativ und plattformübergreifend sein.

Ein *Namespace* ist eine Sammlung von Benachrichtigungshubs. Jeder Namespace besteht in der Regel aus Hubs, die im Zusammenhang stehen und für einen bestimmten Zweck verwendet werden. Sie können z. B. drei verschiedene Namespaces für Entwicklungs-, Test- und Produktionszwecke verwenden.

Sie können einen Namespace mit den **Free**-, **Basic**- oder **Standard**-Tarifen verknüpfen. Sie können für jeden Namespace den Tarif verwenden, der Ihren Anforderungen am besten entspricht. Die folgenden Abschnitte zeigen Ihnen, wie Sie den Tarif eines Notification Hubs-Namespace ändern.

## <a name="use-azure-portal"></a>Verwenden des Azure-Portals

Wenn Sie das Azure-Portal verwenden, können Sie den Tarif für einen Namespace auf der Namespaceseite oder auf einer Hubseite ändern. Wenn Sie ihn auf einer Hubseite ändern, ändern Sie ihn tatsächlich auf Namespaceebene. Der Tarif wird für den Namespace und alle Hubs im Namespace geändert.

### <a name="change-tier-on-the-namespace-page"></a>Ändern des Tarifs auf der Namespaceseite

Mit den Schritten des folgenden Verfahrens ändern Sie den Tarif für einen Namespace auf der Namespaceseite. Wenn Sie den Tarif für einen Namespace ändern, gilt dies für alle Hubs im Namespace.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Menü links **Alle Dienste** aus.
3. Wählen Sie **Notification Hub-Namespaces** im Abschnitt **Internet der Dinge** aus. Bei Auswahl des Sternchens (`*`) neben dem Text wird der Eintrag der linken Navigationsleiste unter **Favoriten** hinzugefügt. Dies hilft Ihnen, beim nächsten Mal schneller auf die Namespaceseite zuzugreifen. Wählen Sie **Notification Hub-Namespaces** aus, nachdem Sie den Eintrag den **FAVORITEN** hinzugefügt haben.

    ![Alle Dienste -> Notification Hub-Namespaces](./media/change-pricing-tier/all-services-nhub.png)

4. Wählen Sie auf der Seite **Notification Hub-Namespaces** den Namespace aus, für den Sie den Tarif ändern möchten.
5. Auf der Seite **Notification Hub-Namespace** für Ihren Namespace finden Sie im Abschnitt **Essentials** den aktuellen Tarif für den Namespace. In der folgenden Abbildung sehen Sie, dass dem Namespace der Tarif **Free** zugeordnet ist.

    ![Ändern des aktuellen Tarifs auf der Namespaceseite](./media/change-pricing-tier/pricing-tier-before.png)

6. Wählen Sie auf der Seite **Notification Hub-Namespace** für Ihren Namespace **Tarif** im Abschnitt **Verwalten** aus.

    ![Auswählen des Tarifs auf der Namespaceseite](./media/change-pricing-tier/namespace-select-pricing-menu.png)

7. Ändern Sie den Tarif, und klicken Sie dann auf die Schaltfläche **Auswählen**.
8. Sie können den Status der Tarifänderung unter **Warnungen** sehen.
9. Wechseln Sie zur Seite **Übersicht**. Vergewissern Sie sich, dass der neue Tarif für das **Tarif**-Feld im Abschnitt **Essentials** angezeigt wird.
10. Dieser Schritt ist optional. Wählen Sie einen beliebigen Hub im Namespace aus. Überprüfen Sie, ob derselbe Tarif im Abschnitt **Essentials** angezeigt wird. Derselbe Tarif sollte für alle Hubs im Namespace angezeigt werden.

### <a name="change-tier-on-the-hub-page"></a>Ändern des Tarifs auf der Hubseite

Befolgen Sie diese Schritte, um den Tarif für einen Namespace auf der Hubseite zu ändern. Auch wenn Sie die folgenden Schritte auf der Hubseite beginnen, ändern Sie den Tarif für den Namespace und alle Hubs tatsächlich im Namespace:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Menü links **Alle Dienste** aus.
3. Wählen Sie **Notification Hubs** im Abschnitt **Internet der Dinge** aus.
4. Wählen Sie Ihre Notification **Hub**-Instanz aus.
5. Wählen Sie **Tarif** im linken Menü aus.
6. Ändern Sie den Tarif, und klicken Sie auf die Schaltfläche **Auswählen**. Diese Aktion ändert die Einstellung des Tarifs für den Namespace, der den Hub enthält. Darum sehen Sie den neuen Tarif auf der Namespaceseite und allen Hubseiten.

> [!NOTE]
> Alle Tarifänderungen werden sofort wirksam.

## <a name="use-rest-api"></a>REST-API

Sie können mit den folgenden Ressourcenanbieter-REST-APIs den aktuellen Tarif abrufen und ihn aktualisieren.

### <a name="get-current-pricing-tier-for-a-namespace"></a>Abrufen des aktuellen Tarif für einen Namespace

Zum Abrufen des aktuellen Namespacetarifs senden Sie einen GET-Befehl, wie im folgenden Beispiel gezeigt:

```REST
GET: https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/notificationhubplan
```

### <a name="update-pricing-tier-for-a-namespace"></a>Aktualisieren des Tarifs für einen Namespace

Zum Aktualisieren des Namespacetarifs senden Sie einen PUT-Befehl, wie im folgenden Beispiel gezeigt:

```REST
PUT: https://management.core.windows.net/{subscription ID}/services/ServiceBus/Namespaces/{namespace name}/notificationhubplan
Body: <NotificationHubPlan xmlns:i="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"><SKU>Standard</SKU></NotificationHubPlan>
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu diesen Tarifen und Preisen finden Sie unter [Notification Hubs – Preise](https://azure.microsoft.com/pricing/details/notification-hubs/).
