---
title: Problembehandlung für das Speech-Geräte-SDK – Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Dieser Artikel enthält Informationen zum Beheben von Problemen, die bei Verwendung des Speech Devices SDK auftreten können.
services: cognitive-services
author: mswellsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: wellsi
ms.openlocfilehash: c590e0972de289a36890a75b220eddbded701ea8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "74815563"
---
# <a name="troubleshoot-the-speech-devices-sdk"></a>Problembehandlung für das Speech Devices SDK

Dieser Artikel enthält Informationen zum Beheben von Problemen, die bei Verwendung des Speech Devices SDK auftreten können.

## <a name="certificate-failures"></a>Zertifikatfehler

Wenn bei der Verwendung des Speech-Diensts Zertifikatfehler auftreten, stellen Sie sicher, dass Datum und Uhrzeit des Geräts richtig sind:

1. Wechseln Sie zu **Einstellungen**. Wählen Sie unter **System** die Option **Datum und Uhrzeit** aus.

    ![Wählen Sie unter „Einstellungen“ die Option „Datum und Uhrzeit“ aus.](media/speech-devices-sdk/qsg-12.png)

1. Belassen Sie die Option **Automatische Datums- und Uhrzeiteinstellung** aktiviert. Wählen Sie unter **Zeitzone auswählen** die aktuelle Zeitzone aus.

    ![Auswählen der Optionen für Datum und Zeitzone](media/speech-devices-sdk/qsg-13.png)

    Wenn Sie sehen, dass die Zeit des Development Kits mit der Zeit Ihres PC übereinstimmt, ist das Development Kit mit dem Internet verbunden.

## <a name="next-steps"></a>Nächste Schritte

* [Lesen Sie die Anmerkungen zu dieser Version](devices-sdk-release-notes.md)
