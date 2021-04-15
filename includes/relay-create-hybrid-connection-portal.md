---
author: clemensv
ms.service: service-bus-relay
ms.topic: include
ms.date: 11/09/2018
ms.author: clemensv
ms.openlocfilehash: ceb2626a43ed44338bb0faad475ae2333af2de9e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "67178323"
---
Vergewissern Sie sich, dass Sie bereits [einen Relay-Namespace erstellt haben][namespace-how-to].

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im linken Menü die Option **Alle Ressourcen** aus.
3. Wählen Sie den Namespace aus, in dem Sie die Hybridverbindung erstellen möchten. In diesem Fall: **mynewns**.  
4. Klicken Sie unter **Relay namespace** (Relaynamespace) auf **Hybrid Connections**.

    ![Hybridverbindung erstellen](./media/relay-create-hybrid-connection-portal/create-hc-1.png)

5. Klicken Sie im Fenster mit der Namespaceübersicht auf **+ Hybridverbindung**.
   
    ![Auswählen der Hybridverbindung](./media/relay-create-hybrid-connection-portal/create-hc-2.png)
6. Geben Sie unter **Hybridverbindung erstellen** einen Wert für den Namen der Hybridverbindung ein. Lassen Sie die anderen Standardwerte unverändert.
   
    ![„Neu“ wählen](./media/relay-create-hybrid-connection-portal/create-hc-3.png)
7. Klicken Sie auf **Erstellen**.

[namespace-how-to]: ../articles/service-bus-relay/relay-create-namespace-portal.md 