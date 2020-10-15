---
title: 'Speech-Dienst: Erste Schritte mit Custom Speech'
titleSuffix: Azure Cognitive Services
description: Bei Custom Speech handelt es sich um eine Reihe von Onlinetools, mit denen Sie die Genauigkeit unserer Spracherkennung für Ihre Anwendungen, Tools und Produkte bewerten und verbessern können. Für den Einstieg benötigen Sie lediglich eine Handvoll Audiotestdateien. Nutzen Sie die unten angegebenen Links, um mit der Erstellung einer benutzerdefinierten Spracherkennung zu beginnen.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: trbye
ms.openlocfilehash: b9ac473c3864b4be06c978e7d6a3555bb4550d2a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88056838"
---
# <a name="what-is-custom-speech"></a>Was ist Custom Speech?

Bei [Custom Speech](https://aka.ms/customspeech) handelt es sich um eine Reihe von Onlinetools, mit denen Sie die Genauigkeit der Spracherkennung von Microsoft für Ihre Anwendungen, Tools und Produkte bewerten und verbessern können. Für den Einstieg benötigen Sie lediglich eine Handvoll Audiotestdateien. Nutzen Sie die unten angegebenen Links, um mit der Erstellung einer benutzerdefinierten Spracherkennung zu beginnen.

## <a name="whats-in-custom-speech"></a>Was ist in Custom Speech enthalten?

Bevor Sie Custom Speech verwenden können, benötigen Sie ein Azure-Konto und ein Abonnement für Speech-Dienste. Sobald Sie ein Konto haben, können Sie Daten vorbereiten, Modelle trainieren und testen, die Erkennungsqualität überprüfen, die Genauigkeit bewerten und das benutzerdefinierte Spracherkennungsmodell schließlich bereitstellen und verwenden.

Das folgende Diagramm zeigt die Teile, aus denen sich das [Custom Speech-Portal](https://aka.ms/customspeech) zusammensetzt. Verwenden Sie die unten angegebenen Links, um mehr über die einzelnen Schritte zu erfahren.

![Zeigt die verschiedenen Komponenten, aus denen sich das Custom Speech-Portal zusammensetzt.](./media/custom-speech/custom-speech-overview.png)

1. [Abonnieren und Erstellen eines Projekts:](#set-up-your-azure-account) Erstellen Sie ein Azure-Konto, und abonnieren Sie die Speech-Dienste. Durch dieses einheitliche Abonnement erhalten Sie Zugriff auf die Spracherkennung, Sprachsynthese, Sprachübersetzung und das [Custom Speech-Portal](https://speech.microsoft.com/customspeech). Erstellen Sie dann mit Ihrem Abonnement für Speech-Dienste Ihr erstes Custom Speech-Projekt.

2. [Hochladen von Testdaten](how-to-custom-speech-test-data.md): Laden Sie Testdaten (Audiodateien) hoch, um das Spracherkennungsangebot von Microsoft für Ihre Anwendungen, Tools und Produkte zu bewerten.

3. [Überprüfen der Erkennungsqualität](how-to-custom-speech-inspect-data.md): Verwenden Sie das [Custom Speech-Portal](https://speech.microsoft.com/customspeech) zur Wiedergabe hochgeladener Audiodateien, und überprüfen Sie die Qualität der Spracherkennung Ihrer Testdaten. Informationen zu quantitativen Messungen finden Sie unter [Überprüfen von Daten](how-to-custom-speech-inspect-data.md).

4. [Bewerten der Genauigkeit](how-to-custom-speech-evaluate-data.md): Bewerten Sie die Genauigkeit des Spracherkennungsmodells. Das [Custom Speech-Portal](https://speech.microsoft.com/customspeech) stellt eine *Wort-Fehler-Rate* bereit, mit der bestimmt werden kann, ob zusätzliches Training erforderlich ist. Wenn Sie mit der Genauigkeit zufrieden sind, können Sie die Speech-Dienst-APIs direkt verwenden. Wenn Sie die Genauigkeit um durchschnittlich 5 – 20 % verbessern möchten, verwenden Sie die Registerkarte **Training** im Portal, um zusätzliche Trainingsdaten hochzuladen, z.B. menschenmarkierte Transkripte und zugehörigen Text.

5. [Verbessern der Genauigkeit:](how-to-custom-speech-improve-accuracy.md) Wählen Sie strategisch zusätzliche Trainingsdaten, um die Qualität des Spracherkennungsmodells basierend auf Ihrem Szenario zu verbessern.

6. [Trainieren des Modells](how-to-custom-speech-train-model.md): Verbessern Sie die Genauigkeit Ihres Spracherkennungsmodells, indem Sie schriftliche Transkripte (10 – 1.000 Stunden) und zugehörigen Text (< 200 MB) zusammen mit Ihren Audiotestdaten bereitstellen. Mithilfe dieser Daten kann das Spracherkennungsmodell trainiert werden. Testen Sie das Modell nach dem Training erneut. Wenn Sie mit dem Ergebnis zufrieden sind, können Sie das Modell bereitstellen.

7. [Bereitstellen des Modells](how-to-custom-speech-deploy-model.md): Erstellen Sie einen benutzerdefinierten Endpunkt für Ihr Spracherkennungsmodell, und verwenden Sie es in Ihren Anwendungen, Tools oder Produkten.

## <a name="set-up-your-azure-account"></a>Einrichten Ihres Azure-Kontos

Sie benötigen ein Abonnement für die Speech-Dienste, um das [Custom Speech-Portal](https://speech.microsoft.com/customspeech) zum Erstellen eines benutzerdefinierten Modells verwenden zu können. Zum Erstellen eines Standardabonnements für Speech-Dienste gehen Sie wie folgt vor: [Erstellen Sie ein Speech-Abonnement](get-started.md#new-resource).

> [!NOTE]
> Achten Sie darauf, dass Sie Standardabonnements (S0) erstellen. Kostenlose Abonnements (F0) werden nicht unterstützt.

Nachdem Sie ein Azure-Konto und ein Abonnement für Speech-Dienste erstellt haben, müssen Sie sich beim [Custom Speech-Portal](https://speech.microsoft.com/customspeech) anmelden und Ihr Abonnement verbinden.

1. Rufen Sie über das Azure-Portal Ihren Abonnementschlüssel für die Speech-Dienste ab.
2. Melden Sie sich beim [Custom Speech-Portal](https://aka.ms/custom-speech) an.
3. Wählen Sie das Abonnement aus, das Sie bearbeiten möchten, und erstellen Sie ein Speech-Projekt.
4. Wenn Sie Ihr Abonnement ändern möchten, verwenden Sie das **Zahnrad**-Symbol in der oberen Navigationsleiste.

## <a name="how-to-create-a-project"></a>So erstellen Sie ein Projekt

Inhalte wie Daten, Modelle, Tests und Endpunkte sind im [Custom Speech-Portal](https://speech.microsoft.com/customspeech) in **Projekten** organisiert. Jedes Projekt ist für eine Domäne und das Land/die Sprache spezifisch. Sie können beispielsweise ein Projekt für Callcenter erstellen, die Englisch in den USA verwenden.

Wählen Sie zum Erstellen Ihres ersten Projekts die **Spracherkennung/Custom Speech** aus, und klicken Sie dann auf **Neues Projekt**. Folgen Sie den Anweisungen des Assistenten, um Ihr Projekt zu erstellen. Nachdem Sie ein Projekt erstellt haben, sollten vier Registerkarten angezeigt werden: **Daten**, **Test**, **Training** und **Bereitstellung**. Verwenden Sie die Links unter [Nächste Schritte](#next-steps), um mehr über die Verwendung der einzelnen Registerkarten zu erfahren.

> [!IMPORTANT]
> Das [Custom Speech-Portal](https://aka.ms/custom-speech) wurde kürzlich aktualisiert! Wenn Sie vorherige Daten, Modelle, Tests und veröffentlichte Endpunkte im CRIS.ai-Portal oder mit APIs erstellt haben, müssen Sie im neuen Portal ein neues Projekt erstellen, um eine Verbindung mit diesen alten Entitäten herzustellen.

## <a name="next-steps"></a>Nächste Schritte

* [Vorbereiten und Testen Ihrer Daten](how-to-custom-speech-test-data.md)
* [Überprüfen Ihrer Daten](how-to-custom-speech-inspect-data.md)
* [Bewerten Ihrer Daten](how-to-custom-speech-evaluate-data.md)
* [Trainieren Ihres Modells](how-to-custom-speech-train-model.md)
* [Bereitstellen Ihres Modells](how-to-custom-speech-deploy-model.md)
