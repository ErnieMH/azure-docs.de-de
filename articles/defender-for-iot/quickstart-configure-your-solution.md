---
title: 'Schnellstart: Konfigurieren Ihrer Lösung'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie Ihre End-to-End-IoT-Lösung mit Azure Defender für IoT konfigurieren.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/06/2020
ms.author: mlottner
ms.openlocfilehash: f949d52d4ba461da94153cbbd17ca5fb3eb214d7
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92092085"
---
# <a name="quickstart-configure-your-azure-defender-for-iot-solution"></a>Schnellstart: Konfigurieren Ihrer Lösung mit Azure Defender für IoT

In diesem Artikel wird die Erstkonfiguration Ihrer IoT-Sicherheitslösung mit Defender für IoT erläutert.

## <a name="what-is-defender-for-iot"></a>Was ist Defender für IoT?

Defender für IoT bietet umfassende End-to-End-Sicherheit für Azure-basierte IoT-Lösungen.

Mit Defender für IoT können Sie Ihre gesamte IoT-Lösung über ein zentrales Dashboard überwachen, auf dem alle Ihre IoT-Geräte, IoT-Plattformen und Back-End-Ressourcen in Azure angezeigt werden.

Nach der Aktivierung in Ihrer IoT Hub-Instanz erkennt Defender für IoT automatisch andere Azure-Dienste, die ebenfalls mit Ihrer IoT Hub-Instanz verbunden sind und mit Ihrer IoT-Lösung zusammenhängen.

Zusätzlich zur automatischen Beziehungserkennung können Sie auch weitere Azure-Ressourcengruppen als Teil Ihrer IoT-Lösung kennzeichnen.

Dadurch können Sie vollständige Abonnements, Ressourcengruppen oder auch einzelne Ressourcen hinzufügen.

Nachdem Sie alle Ressourcenbeziehungen definiert haben, werden von Defender für IoT unter Verwendung von Defender Sicherheitsempfehlungen und Warnungen für diese Ressourcen bereitgestellt.

## <a name="add-azure-resources-to-your-iot-solution"></a>Hinzufügen von Azure-Ressourcen zu Ihrer IoT-Lösung

Gehen Sie wie folgt vor, um Ihrer IoT-Lösung neue Ressourcen hinzuzufügen:

1. Öffnen Sie im Azure-Portal Ihre Instanz von **IoT Hub**.
1. Wählen Sie unter **Sicherheit** die Option **Übersicht**, anschließend **Einstellungen** und dann **Überwachte Ressourcen** aus.
1. Wählen Sie **Bearbeiten** und dann die überwachten Ressourcen aus, die zu ihrer IoT-Lösung gehören.
1. Klicken Sie auf **Hinzufügen**.

Glückwunsch! Sie haben Ihrer IoT-Lösung eine neue Ressourcengruppe hinzugefügt.

Defender für IoT überwacht nun die neu hinzugefügten Ressourcengruppen und zeigt relevante Sicherheitsempfehlungen und Warnungen im Rahmen Ihrer IoT-Lösung an.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel erfahren Sie, wie Sie Sicherheitsmodule erstellen:

> [!div class="nextstepaction"]
> [Quickstart: Create an azureiotsecurity module twin](quickstart-create-security-twin.md) (Schnellstart: Erstellen eines azureiotsecurity-Modulzwillings)
