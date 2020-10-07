---
author: craigktreasure
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 12/13/2018
ms.author: crtreasu
ms.openlocfilehash: b2b3ca886359a0b4c906b89ed76f57486fc2c368
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "83638993"
---
## <a name="create-a-spatial-anchors-resource"></a>Erstellen einer Spatial Anchors-Ressource

Öffnen Sie das <a href="https://portal.azure.com" target="_blank">Azure-Portal</a>.

Wählen Sie im linken Navigationsbereich des Azure-Portals die Option **Ressource erstellen** aus.

Suchen Sie über das Suchfeld nach **Spatial Anchors**.

   ![Suchen nach Spatial Anchors](./media/spatial-anchors-get-started-create-resource/portal-search.png)

Wählen Sie **Spatial Anchors** aus. Wählen Sie im Dialogfeld die Option **Erstellen** aus.

Gehen Sie im Dialogfeld **Spatial Anchors-Konto** wie folgt vor:

- Geben Sie einen eindeutigen Ressourcennamen in regulären alphanumerischen Zeichen ein.
- Wählen Sie das Abonnement aus, an das die Ressource angefügt werden soll.
- Erstellen Sie eine Ressourcengruppe durch Auswählen von **Neu erstellen**. Nennen Sie sie **myResourceGroup**, und wählen Sie **OK** aus.
      [!INCLUDE [resource group intro text](resource-group.md)]
- Wählen Sie einen Standort (Region) für die Ressource aus.
- Wählen Sie **Neu** aus, um mit der Ressourcenerstellung zu beginnen.

   ![Erstellen einer Ressource](./media/spatial-anchors-get-started-create-resource/create-resource-form.png)

Nachdem die Ressource erstellt wurde, zeigt das Azure-Portal an, dass die Bereitstellung abgeschlossen ist. Klicken Sie auf **Zu Ressource wechseln**.

![Bereitstellung abgeschlossen](./media/spatial-anchors-get-started-create-resource/deployment-complete.png)

Anschließend können Sie die Ressourceneigenschaften anzeigen. Kopieren Sie den Wert für **Konto-ID** der Ressource in einen Text-Editor, da Sie ihn später benötigen.

   ![Ressourceneigenschaften](./media/spatial-anchors-get-started-create-resource/view-resource-properties.png)

Kopieren Sie auch den Wert für **Kontodomäne** der Ressource in einen Text-Editor, da Sie ihn später benötigen.

   ![Kontodomäne](./media/spatial-anchors-get-started-create-resource/view-resource-domain.png)

Wählen Sie unter **Einstellungen** die Option **Schlüssel** aus. Kopieren Sie den Wert für **Primärschlüssel** in einen Text-Editor. Dieser Wert ist der `Account Key`. Sie benötigen die Information später.

   ![Kontoschlüssel](./media/spatial-anchors-get-started-create-resource/view-account-key.png)
