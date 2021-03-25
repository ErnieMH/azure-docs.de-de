---
title: Anzeigen der Modelldetails – Custom Translator
titleSuffix: Azure Cognitive Services
description: Die Registerkarte „Modelle“ zeigt unter jedem Projekt Details zu jedem Modell an, z.B. Modellname, Modellstatus, BLEU-Score, Training, Optimierung, Anzahl von Testsets.
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 08/17/2020
ms.author: lajanuar
ms.topic: conceptual
ms.openlocfilehash: 11f39aa480e3ba6508bf730c61891e80ea689e8b
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98895950"
---
# <a name="view-model-details"></a>Anzeigen der Modelldetails

Die Registerkarte „Modelle“ unter „Projekt“ zeigt alle Modelle in diesem Projekt an. Alle für dieses Projekt trainierten Modelle sind in dieser Registerkarte aufgelistet.

Für jedes Modell im Projekt werden folgende Details angezeigt.

1. Model Name (Modellname): Der Modellname eines bestimmten Modells.

2. Status: Der Status eines bestimmten Modells. Ihr neues Training hat den Status „Übermittelt“, bis die Daten angenommen werden. Der Status ändert sich in „Datenverarbeitung“, wenn der Dienst den Inhalt Ihrer Dokumente auswertet. Wenn die Auswertung Ihrer Dokumente abgeschlossen ist, wechselt der Status zu „Wird ausgeführt“. Nun können Sie die Anzahl von Sätzen in Ihrem Training sehen, einschließlich der Optimierungs- und Testsets, die automatisch für Sie erstellt werden. Die folgende Liste beschreibt die unterschiedlichen Modellstatus.

    - Submitted (Übermittelt): Das Back-End verarbeitet die Dokumente für das Modell.

    - TrainingQueued: Das Training für dieses Modell wurde der Warteschlange des MT-Systems hinzugefügt.

    - Running (Wird ausgeführt): Das Training wird im MT-System für dieses Modell ausgeführt.

    - Succeeded (Erfolgreich): Das Training war im MT-System erfolgreich, und ein Modell ist verfügbar. Bei diesem Status wird ein BLEU-Score (Bilingual Evaluation Understudy) für dieses Modell angezeigt.

    - Deployed (Bereitgestellt): Das erfolgreich trainierte Modell wird an das MT-System zum Bereitstellen übermittelt.

    - Undeploying (Bereitstellung wird aufgehoben): Die Bereitstellung des Modells wird aufgehoben.

    - Undeployed (Bereitstellung wurde aufgehoben): Die Bereitstellung eines Modells wurde erfolgreich aufgehoben.

    - Training Failed (Trainingsfehler): Beim Training ist ein Fehler aufgetreten. Wenn ein Trainingsfehler auftritt, versuchen Sie den Trainingsauftrag erneut. Wenden Sie sich an uns, wenn der Fehler weiterhin auftritt. Löschen Sie nicht das fehlerhafte Modell.

    - DataProcessingFailed: Bei der Datenverarbeitung ist für mindestens ein Dokument des Modells ein Fehler aufgetreten.

    - DeploymentFailed: Fehler bei der Modellbereitstellung.

    - MigratedDraft: Das Modell befindet sich nach der Migration von Hub zu Custom Translator im Entwurfsstatus.

3. BLEU Score: BLEU-Score des Modells, der die Qualität Ihres Übersetzungssystems anzeigt. Er gibt Auskunft darüber, wie exakt die vom Übersetzungssystem anhand dieses Trainings angefertigten Übersetzungen mit den Referenzsätzen im Testdataset übereinstimmen. Der BLEU-Score wird angezeigt, wenn das Training erfolgreich abgeschlossen wurde. Wenn das Training nicht abgeschlossen wurde bzw. ein Fehler aufgetreten ist, wird kein BLEU-Score angezeigt.

4. Training Sentence count (Anzahl Trainingssätze): Die Gesamtanzahl von Sätzen, die als Trainingssatz verwendet werden.

5. Tuning Sentence count (Anzahl Tuningsätze): Die Gesamtanzahl von Sätzen, die als Tuningset verwendet werden.

6.  Training Sentence count (Anzahl Trainingssätze): Die Gesamtanzahl von Sätzen, die als Testset verwendet werden.

7.  Mono Sentence count (Anzahl Monosätze): Die Gesamtanzahl von Sätzen, die als Monoset verwendet werden.

8.  Interaktive Schaltfläche „Bereitstellen“: Für erfolgreich trainierte Modelle, die noch nicht bereitgestellt wurden, wird die Schaltfläche „Bereitstellen“ angezeigt. Wenn ein Modell bereitgestellt wurde, wird die Schaltfläche „Bereitstellung zurücknehmen“ angezeigt.

9. Delete (Löschen): Mit dieser Schaltfläche können Sie das Modell löschen. Wenn Sie ein Modell löschen, werden nicht die Dokumente entfernt, die zum Erstellen dieses Modells verwendet wurden.

    ![Anzeigen der Modelldetails](media/how-to/how-to-view-model-details.png)

>[!Note]
>Zum Vergleichen aufeinanderfolgender Trainings für dieselben Systeme müssen das Optimierungs- und Testset konstant sein.

## <a name="view-model-training-details"></a>Anzeigen von Modelltrainingsdetails

Wenn Ihr Training abgeschlossen ist, werden auf der Detailseite die genauen Trainingsinformationen angezeigt. Wählen Sie ein Projekt aus, gehen Sie zur Registerkarte „Modelle“, und wählen Sie diese sowie ein Modell aus.

Die Modellseite verfügt über zwei Registerkarten: Trainingsdetails und Testen.

1.  **Training Details (Trainingsdetails):** Hier wird die Liste der Dokumente angezeigt, die für das Training verwendet wurden:

    -  Documents Name (Dokumentname): Dieses Feld zeigt den Namen des Dokuments an.

    -  Document Type (Dokumenttyp): Dieses Feld zeigt an, ob dieses Dokument parallel/mono ist.

    -  Sentence count in source language (Satzanzahl in Ausgangssprache): Dieses Feld gibt die Anzahl von Sätzen in der Ausgangssprache an.

    -  Sentence count in target language (Satzanzahl in Zielsprache): Dieses Feld gibt die Anzahl von Sätzen in der Zielsprache an.

    -  Aligned Sentences (Zugeordnete Sätze): Dieses Feld zeigt an, wie viele Sätze bei der Zuordnung durch Custom Translator zugeordnet wurden.

    -  Used Sentences (Verwendete Sätze): Dieses Feld zeigt an, wie viele Sätze beim Trainieren von Custom Translator verwendet wurden.

    ![Modelltrainingsdetails](media/how-to/how-to-model-training-details.png)

2.  **Test (Testen):** Diese Registerkarte zeigt die Testdetails für ein erfolgreiches Training an.

## <a name="next-steps"></a>Nächste Schritte

- Überprüfen und analysieren Sie die [Testergebnisse](how-to-view-system-test-results.md).
