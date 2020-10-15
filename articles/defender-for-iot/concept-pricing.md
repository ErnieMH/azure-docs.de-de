---
title: Preise und anfallende Kosten
description: Informationen zu den für Defender für IoT anfallenden Kosten und dazu, wie sie gesteuert werden.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/04/2020
ms.author: mlottner
ms.openlocfilehash: 24ae6c4014948639aa737a0d2d88ec15f98a7cb4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90931461"
---
# <a name="pricing-and-associated-costs"></a>Preise und anfallende Kosten

In diesem Artikel wird das Preismodell für Defender für IoT erläutert. Außerdem werden alle anfallenden Kosten zusammenfasst, und es wird erläutert, wie sie verwaltet werden.

## <a name="pricing"></a>Preise

Das Preismodell von Defender für IoT besteht aus zwei Teilen. Die Abrechnung erfolgt, sobald ein IoT Hub in Defender für IoT [aktiviert](quickstart-onboard-iot-hub.md) wird:

- Kosten nach Gerät: integrierte Sicherheitsfunktionen basierend auf der Analyse von IoT Hub-Protokollen

- Kosten nach Meldung: erweiterte Sicherheitsfunktionen basierend auf Sicherheitsmeldungen von IoT Edge oder Blattgeräten

Weitere Informationen finden Sie unter [Security Center – Preise](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="associated-costs"></a>Anfallende Kosten

Für Defender für IoT fallen Kosten an, die nicht Teil der direkten Preise sind:

- Kosten für Log Analytics-Speicher

Sie können die anfallenden Kosten reduzieren, indem Sie bestimmte Funktionen der Lösung deaktivieren. Sie bewirken das, indem Sie Ihre Einstellungen ändern.

So ändern Sie Ihre Einstellungen

1. Öffnen Sie IoT Hub.

1. Klicken Sie unter **Sicherheit** auf **Einstellungen**.

1. Klicken Sie auf **Datensammlung**.

Die folgende Tabelle enthält eine Zusammenfassung der anfallenden Kosten und Auswirkungen der einzelnen Optionen.

| Option | Verwendung | Comment |
| --- | --- | --- |
| **Log Analytics-Speicher** |  |
| Geräteempfehlungen und -warnungen| Vom Dienst generierte Sicherheitsempfehlungen und -warnungen | Nicht optional |
| Sicherheitsrohdaten| Sicherheitsrohdaten von IoT-Geräten, erfasst von Sicherheits-Agents | Deaktivieren von _Store raw device security events_ (Sicherheitsrohereignisse für Geräte speichern) |
|

>[!Important]
> Die Deaktivierung hat schwerwiegende Auswirkungen auf die Verfügbarkeit von Defender für IoT-Sicherheitsfunktionen.

| Deaktivierung | Auswirkungen |
| --- | --- |
| _Twin metadata collection (Sammlung von Gerätezwillingsmetadaten)_ | [Benutzerdefinierte Benachrichtigungen](quickstart-create-custom-alerts.md) sind deaktiviert |
| | Empfehlungen zum IoT Edge-Manifest sind deaktiviert |
| | Empfehlungen und Warnungen basierend auf Geräteidentität sind deaktiviert |
| _Store raw device security events (Sicherheitsrohereignisse für Geräte speichern)_ | Details zu grundlegenden Empfehlungen zum Gerätebetriebssystem sind nicht verfügbar |
| | Details zu Untersuchungen von [Warnungen](concept-security-alerts.md) und [Empfehlungen](concept-recommendations.md) sind nicht verfügbar |
|

## <a name="see-also"></a>Weitere Informationen

- [Zugreifen auf Sicherheitsdaten](how-to-security-data-access.md)
- [Untersuchen eines Geräts](how-to-investigate-device.md)
- Verstehen und Erkunden von [Sicherheitsempfehlungen](concept-recommendations.md)
- Verstehen und Erkunden von [Sicherheitswarnungen](concept-security-alerts.md)
