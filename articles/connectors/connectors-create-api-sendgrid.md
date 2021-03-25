---
title: Herstellen einer Verbindung mit SendGrid über Azure Logic Apps
description: Automatisieren von Aufgaben und Workflows, die E-Mails senden und Adressenlisten in SendGrid mithilfe von Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/24/2018
tags: connectors
ms.openlocfilehash: a140ae0f27c61959d8ebc6854c5bcb2a916a0fc6
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "87290434"
---
# <a name="send-emails-and-manage-mailing-lists-in-sendgrid-by-using-azure-logic-apps"></a>Senden von E-Mails und Verwalten von Adressenlisten in SendGrid mithilfe von Azure Logic Apps

Mit Azure Logic Apps und dem SendGrid-Connector können Sie automatisierte Aufgaben und Workflows erstellen, die E-Mails senden und Ihre Empfängerlisten verwalten, z.B.:

* Senden von E-Mails
* Hinzufügen von Empfängern zu Listen
* Abrufen, Hinzufügen und Verwalten von globaler Unterdrückung

Sie können SendGrid-Aktionen in Ihren Logik-Apps verwenden, um diese Aufgaben auszuführen. Sie können die Ausgaben von SendGrid-Aktionen auch in anderen Aktionen verwenden. 

Dieser Connector ermöglicht nur Aktionen, daher müssen Sie zum Starten Ihrer Logik-App einen separaten Trigger wie einen **Wiederholungstrigger** verwenden. Beispiel: Wenn Sie Ihren Listen regelmäßig Empfänger hinzufügen, können Sie E-Mails zu Empfängern und Listen mithilfe des Office 365 Outlook- oder Outlook.com-Connectors senden.
Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/). 

* Ein [SendGrid-Konto](https://www.sendgrid.com/) und ein [SendGrid-API-Schlüssel](https://sendgrid.com/docs/ui/account-and-settings/api-keys/)

   Ihr API-Schlüssel autorisiert Ihre Logik-App zur Erstellung einer Verbindung mit Ihrem SendGrid-Konto sowie zum Zugriff darauf.

* Grundlegende Kenntnisse über die [Erstellung von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Die Logik-App, in der Sie auf Ihr SendGrid-Konto zugreifen möchten. Um eine SendGrid-Aktion zu verwenden, starten Sie Ihre Logik-App mit einem anderen Trigger, z.B. dem **Wiederholungstrigger**.

## <a name="connect-to-sendgrid"></a>Herstellen einer Verbindung mit SendGrid

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und öffnen Sie Ihre Logik-App im Logik-App-Designer, sofern sie nicht bereits geöffnet ist.

1. Auswählen eines Pfads: 

   * Wählen Sie im letzten Schritt zum Hinzufügen einer Aktion **Neuer Schritt** aus. 

     Oder

   * Wenn Sie zwischen Schritten eine Aktion einfügen möchten, bewegen Sie den Mauszeiger über den Pfeil zwischen den Schritten. 
   Wählen Sie das daraufhin angezeigte Pluszeichen ( **+** ) und dann **Aktion hinzufügen** aus.

1. Geben Sie im Suchfeld den Begriff „sendgrid“ als Filter ein. Wählen Sie in der Liste mit den Aktionen die gewünschte Aktion aus.

1. Geben Sie einen Namen für die Verbindung an. 

1. Geben Sie Ihren SendGrid-API-Schlüssel ein, und wählen Sie dann **Erstellen**.

1. Geben Sie die erforderlichen Informationen für die ausgewählte Aktion ein, und fahren Sie mit dem Erstellen Ihres Logik-App-Workflows fort.

## <a name="connector-reference"></a>Connector-Referenz

Technische Details zu Triggern, Aktionen und Beschränkungen aus der OpenAPI-Beschreibung (ehemals Swagger) des Connectors finden Sie auf der [Referenzseite](/connectors/sendgrid/) des Connectors.

## <a name="get-support"></a>Support

* Weitere Informationen finden Sie auf der [Frageseite von Microsoft Q&A für Azure Logic Apps](/answers/topics/azure-logic-apps.html).
* Wenn Sie Features vorschlagen oder für Vorschläge abstimmen möchten, besuchen Sie die [Website für Logic Apps-Benutzerfeedback](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)
