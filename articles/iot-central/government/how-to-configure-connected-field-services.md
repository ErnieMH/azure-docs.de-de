---
title: Verbinden Ihrer Azure IoT Central-Anwendung mit Dynamics 365 for Field Service | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie mit Azure IoT Central und Dynamics 365 for Field Service eine End-to-End-Lösung erstellen.
author: miriambrus
ms.author: miriamb
ms.date: 12/11/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 5a62e87e97a719991169112d7280593f6ab4b340
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108772349"
---
# <a name="build-end-to-end-solution-with-azure-iot-central-and-dynamics-365-field-service"></a>Erstellen einer End-to-End-Lösung mit Azure IoT Central und Dynamics 365 for Field Service 

In diesem Artikel wird beschrieben, wie Sie die Integration Ihrer Azure IoT Central-Anwendung in andere Geschäftssysteme aktivieren. 

Bei einer Lösung für vernetzte Abfallwirtschaft können Sie beispielsweise die Aussendung von Müllfahrzeugen optimieren. Diese Optimierung kann basierend auf IoT-Sensordaten von vernetzten Mülleimern erfolgen. In Ihrer [IoT Central-Anwendung für die vernetzte Abfallwirtschaft](./tutorial-connected-waste-management.md) können Sie Regeln und Aktionen konfigurieren und so festlegen, dass in Dynamics 365 for Field Service die Erstellung von Warnungen ausgelöst wird. Dies wird mit Power Automate erreicht. Diese Anwendung wird direkt in IoT Central konfiguriert, um Workflows für Anwendungen und Dienste zu automatisieren. Darüber hinaus können Informationen basierend auf Dienstaktivitäten in Field Service zurück an Azure IoT Central gesendet werden. 

## <a name="how-to-connect-your-azure-iot-central-application-with-dynamics-365-field-services"></a>Verbinden Ihrer Azure IoT Central-Anwendung mit Dynamics 365 for Field Service 

Die folgenden Integrationsprozesse können über eine reine Konfigurationsumgebung leicht implementiert werden:
* Azure IoT Central kann Informationen zu Geräteanomalien zur Diagnose an Connected Field Service senden (als IoT-Warnung).
* Mit Connected Field Service können Vorfälle oder Arbeitsaufträge erstellt werden, die über Geräteanomalien ausgelöst werden.
* Connected Field Service kann auch genutzt werden, um Techniker für die Inspektion einzuplanen, damit Ausfallzeiten verhindert werden.
* Das Gerätedashboard von Azure IoT Central kann mit relevanten Dienst- und Zeitplaninformationen aktualisiert werden.


## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich über [IoT Central-Vorlagen für Behörden](./overview-iot-central-government.md).
* Informieren Sie sich über [IoT Central](../core/overview-iot-central.md).
* Informieren Sie sich über [Dynamics 365 for Field Service](/dynamics365/field-service/cfs-iot-overview).
