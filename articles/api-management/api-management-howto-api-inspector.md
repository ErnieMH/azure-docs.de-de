---
title: Debuggen von APIs mithilfe der Anforderungsablaufverfolgung in Azure API Management | Microsoft-Dokumentation
description: In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie die Anforderungsverarbeitungsschritte in Azure API Management überprüfen.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 06/15/2018
ms.author: apimpm
ms.openlocfilehash: fc5e8c7a7aa0d4693d96c3405ec0e180a6d13f8e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "75768525"
---
# <a name="debug-your-apis-using-request-tracing"></a>Debuggen von APIs mit der Anforderungsablaufverfolgung

In diesem Tutorial wird beschrieben, wie Sie die Anforderungsverarbeitung für das Debugging und die Problembehandlung für Ihre API überprüfen. 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Verfolgen eines Aufrufs

![API-Inspektor](media/api-management-howto-api-inspector/api-inspector001.PNG)

## <a name="prerequisites"></a>Voraussetzungen

+ Machen Sie sich mit der [Azure API Management-Terminologie](api-management-terminology.md) vertraut.
+ Absolvieren Sie den folgenden Schnellstart: [Erstellen einer Azure API Management-Instanz](get-started-create-service-instance.md).
+ Schließen Sie darüber hinaus das folgende Tutorial ab: [Importieren und Veröffentlichen Ihrer ersten API](import-and-publish.md).

## <a name="trace-a-call"></a>Verfolgen eines Aufrufs

![API-Ablaufverfolgung](media/api-management-howto-api-inspector/06-DebugYourAPIs-01-TraceCall.png)

1. Klicken Sie auf **APIs**.
2. Klicken Sie in der API-Liste auf **Demo Conference API**.
3. Wechseln Sie zur Registerkarte **Test**.
4. Wählen Sie den Vorgang **GetSpeakers** aus.
5. Stellen Sie sicher, dass ein HTTP-Header namens **Ocp-Apim-Trace** enthalten und der Wert auf **true** festgelegt ist.

   > [!NOTE]
   > * Wenn „Ocp-Apim-Subscription-Key“ nicht automatisch ausgefüllt wird, können Sie den Wert abrufen, indem Sie die Schlüssel im Entwicklerportal auf der Profilseite verfügbar machen.
   > * Wenn Sie bei Verwendung des HTTP-Headers „Ocp-Apim-Trace“ eine Ablaufverfolgung abrufen möchten, muss die Einstellung **Ablaufverfolgung zulassen** für den Abonnementschlüssel aktiviert sein. Wenn Sie die Einstellung **Ablaufverfolgung zulassen** konfigurieren möchten, wählen Sie im linken Menü unter **API Management** die Option **Abonnements** aus.
   >   ![„Ablaufverfolgung zulassen“ im Bereich „Abonnements“ unter „API Management“](media/api-management-howto-api-inspector/allowtracing.png)

6. Klicken Sie auf **Senden**, um einen API-Aufruf durchzuführen. 
7. Warten Sie, bis der Aufruf abgeschlossen ist. 
8. Wechseln Sie in der **API-Konsole** zur Registerkarte **Ablaufverfolgung**. Sie können auf einen der folgenden Links klicken, um ausführliche Informationen zur Ablaufverfolgung anzuzeigen: **Eingehend**, **Back-End**, **Ausgehend**.

    Im Abschnitt **Eingehend** werden die ursprüngliche Anforderung, die API Management vom Aufrufer erhalten hat, sowie alle auf die Anforderung angewendeten Richtlinien angezeigt, einschließlich der Richtlinien „rate-limit“ und „set-header“, die wir in Schritt 2 hinzugefügt haben.

    Im Abschnitt **Back-End** werden die Anforderungen, die API Management an das API-Back-End gesendet hat, sowie die erhaltene Antwort angezeigt.

    Im Abschnitt **Ausgehend** werden alle Richtlinien angezeigt, die auf die Antwort angewendet werden, bevor sie zurück an den Aufrufer gesendet wird.

    > [!TIP]
    > Jeder Schritt zeigt zudem die Zeit an, die verstrichen ist, seit die Anforderung von API Management empfangen wurde.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Verfolgen eines Aufrufs

Fahren Sie mit dem nächsten Tutorial fort:

> [!div class="nextstepaction"]
> [Gefahrloses Vornehmen geringfügiger Änderungen mithilfe von Revisionen](api-management-get-started-revise-api.md)
