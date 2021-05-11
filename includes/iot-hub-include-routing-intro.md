---
title: include file
description: include file
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/05/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: f2d91e9405c17afe44a05deedc8bb0f0c40377d7
ms.sourcegitcommit: 43be2ce9bf6d1186795609c99b6b8f6bb4676f47
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2021
ms.locfileid: "108278284"
---
Das [Nachrichtenrouting](../articles/iot-hub/iot-hub-devguide-messages-d2c.md) ermöglicht das Senden von Telemetriedaten von Ihren IoT-Geräten an integrierte, Event Hub-kompatible Endpunkte oder benutzerdefinierte Endpunkte wie Blob Storage-Instanzen, Service Bus-Warteschlangen, Service Bus-Themen und Event Hubs. Zum Konfigurieren des benutzerdefinierten Nachrichtenroutings erstellen Sie [Routingabfragen](../articles/iot-hub/iot-hub-devguide-routing-query-syntax.md), um die Route anzupassen, die eine bestimmte Bedingung erfüllt. Nach der Einrichtung werden die eingehenden Daten von IoT Hub automatisch an die Endpunkte weitergeleitet. Wenn eine Nachricht nicht mit einer der definierten Routingabfragen übereinstimmt, wird sie an den Standardendpunkt geleitet.

In diesem zweiteiligen Tutorial lernen Sie, wie Sie diese benutzerdefinierten Routingabfragen mit IoT Hub einrichten und verwenden. Sie leiten Nachrichten von einem IoT-Gerät an einen von mehreren Endpunkten weiter, z. B. Blob Storage oder eine Service Bus-Warteschlange. Nachrichten an die Service Bus-Warteschlange werden über eine Logik-App per E-Mail gesendet. Nachrichten, für die kein benutzerdefiniertes Nachrichtenrouting definiert ist, werden an den Standardendpunkt gesendet und dann von Azure Stream Analytics ausgewählt und in einer Power BI-Visualisierung angezeigt.

Sie führen die folgenden Aufgaben aus, um Teil 1 und 2 dieses Tutorials durchzuarbeiten:

**Teil 1: Erstellen von Ressourcen, Einrichten des Nachrichtenroutings**
> [!div class="checklist"]
> * Sie erstellen die Ressourcen: einen IoT-Hub, ein Speicherkonto, eine Service Bus-Warteschlange und ein simuliertes Gerät. Hierfür können Sie das Portal, eine Azure Resource Manager-Vorlage, die Azure CLI oder Azure PowerShell verwenden.
> * Sie konfigurieren die Endpunkte und Nachrichtenrouten in IoT Hub für das Speicherkonto und die Service Bus-Warteschlange.

**Teil 2: Senden von Nachrichten an den Hub, Anzeigen von Routingergebnissen**
> [!div class="checklist"]
> * Sie erstellen eine Logik-App, die ausgelöst wird und eine E-Mail sendet, wenn der Service Bus-Warteschlange eine Nachricht hinzugefügt wird.
> * Sie laden eine App herunter und führen sie aus, die ein IoT-Gerät simuliert, das für die verschiedenen Weiterleitungsoptionen Nachrichten an den Hub sendet.
> * Sie erstellen eine Power BI-Visualisierung für die Daten, die an den Standardendpunkt gesendet werden.
> * Sie zeigen die Ergebnisse an:
> * – in der Service Bus-Warteschlange und in E-Mails
> * – im Speicherkonto
> * – in der Power BI-Visualisierung

## <a name="prerequisites"></a>Voraussetzungen

* Für Teil 1 dieses Tutorials:
  - Sie benötigen ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

* Für Teil 2 dieses Tutorials:
  - Sie müssen Teil 1 dieses Tutorials abgeschlossen haben, und die Ressourcen müssen noch verfügbar sein.
  - Installieren Sie [Visual Studio](https://www.visualstudio.com/).
  - Zugriff auf ein Power BI-Konto zum Analysieren der Stream Analytics-Daten des Standardendpunkts. ([Power BI kostenlos testen](https://app.powerbi.com/signupredirect?pbi_source=web))
  - Sie müssen über ein Geschäfts-, Schul-oder Unikonto zum Senden von Benachrichtigungs-E-Mails verfügen.
  - Stellen Sie sicher, dass Port 8883 in Ihrer Firewall geöffnet ist. Für das Beispiel in diesem Tutorial wird das MQTT-Protokoll verwendet, das über Port 8883 kommuniziert. In einigen Netzwerkumgebungen von Unternehmen oder Bildungseinrichtungen ist dieser Port unter Umständen blockiert. Weitere Informationen und Problemumgehungen finden Sie unter [Herstellen einer Verbindung mit IoT Hub (MQTT)](../articles/iot-hub/iot-hub-mqtt-support.md#connecting-to-iot-hub).

[!INCLUDE [cloud-shell-try-it.md](cloud-shell-try-it.md)]
