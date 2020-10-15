---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: 9ac7966538102273b91d6b7f15b90e18ceedd421
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "83779931"
---
Sie können auch die Metriken zur Überwachung der Leistung des Geräts und in einigen Fällen zur Problembehandlung bei Geräteproblemen anzeigen.

Führen Sie die folgenden Schritte im Azure-Portal aus, um ein Diagramm für ausgewählte Gerätemetriken zu erstellen.

1. Wechseln Sie für Ihre Ressource im Azure-Portal zu **Überwachung > Metriken**, und wählen Sie **Metrik hinzufügen** aus.

    ![Hinzufügen von Metriken](media/data-box-edge-gateway-view-metrics/view-metrics-1.png)

2. Die Ressource wird automatisch aufgefüllt.  

    ![Aktuelle Ressource](media/data-box-edge-gateway-view-metrics/view-metrics-2.png)

    Um eine andere Ressource anzugeben, wählen Sie die Ressource aus. Wählen Sie auf dem Blatt **Ressource auswählen** das Abonnement, die Ressourcengruppe, den Ressourcentyp und die bestimmte Ressource aus, für die Sie die Metriken anzeigen möchten, und wählen Sie dann **Anwenden** aus.

    ![Auswählen einer anderen Ressource](media/data-box-edge-gateway-view-metrics/view-metrics-3.png)

3. Wählen Sie aus der Dropdownliste eine Metrik aus, um Ihr Gerät zu überwachen. Eine vollständige Liste dieser Metriken finden Sie unter [Metriken auf Ihrem Gerät](#metrics-on-your-device).

4. Wenn eine Metrik aus der Dropdownliste ausgewählt wird, kann auch eine Aggregation definiert werden. „Aggregation“ bezeichnet den tatsächlichen Wert, der über einen bestimmten Zeitraum aggregiert wird. Die aggregierten Werte können durchschnittlich, minimal oder maximal sein. Wählen Sie die Aggregation aus „Avg“, „Max“ oder „Min“ aus.

    ![Diagramm anzeigen](media/data-box-edge-gateway-view-metrics/view-metrics-4.png)

5. Wenn die von Ihnen ausgewählte Metrik mehrere Instanzen aufweist, haben Sie die Möglichkeit zur Aufteilung. Wählen Sie **Teilen anwenden** und dann den Wert aus, der für die Aufschlüsselung verwendet werden soll.

    ![Anwenden des Teilens](media/data-box-edge-gateway-view-metrics/view-metrics-5.png)

6. Wenn Sie jetzt die Aufschlüsselung nur noch für einige wenige Instanzen anzeigen möchten, können Sie die Daten filtern. Wenn Sie in diesem Fall z. B. den Netzwerkdurchsatz nur für die beiden angeschlossenen Netzwerkschnittstellen auf Ihrem Gerät anzeigen möchten, können Sie diese Schnittstellen filtern. Wählen Sie **Filter hinzufügen** aus, und geben Sie den Namen der Netzwerkschnittstelle für die Filterung an.

    ![Filter hinzufügen](media/data-box-edge-gateway-view-metrics/view-metrics-6.png)

7. Sie können das Diagramm auch für den einfachen Zugriff an das Dashboard anheften.

    ![An Dashboard anheften](media/data-box-edge-gateway-view-metrics/view-metrics-7.png)

8. Um Diagrammdaten in eine Excel-Tabelle zu exportieren oder einen Link zum Diagramm abzurufen, den Sie freigeben können, wählen Sie die Freigabeoption auf der Befehlsleiste aus.

    ![Exportieren von Daten](media/data-box-edge-gateway-view-metrics/view-metrics-8.png)
