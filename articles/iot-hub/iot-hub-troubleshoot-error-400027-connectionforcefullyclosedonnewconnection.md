---
title: Problembehandlung – Azure IoT Hub-Fehler „400027 ConnectionForcefullyClosedOnNewConnection“
description: Grundlegendes zum Beheben des Fehlers „400027 ConnectionForcefullyClosedOnNewConnection“
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: f4949816f516c6a6b60cfda0602f458256370d40
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "76960211"
---
# <a name="400027-connectionforcefullyclosedonnewconnection"></a>400027 ConnectionForcefullyClosedOnNewConnection

In diesem Artikel werden die Ursachen des Fehlers **400027 ConnectionForcefullyClosedOnNewConnection** und die Lösungen dafür beschrieben.

## <a name="symptoms"></a>Symptome

Ihr Gerät-zu-Cloud-Zwillingsvorgang (z. B. gemeldete Eigenschaften lesen oder patchen) oder der direkte Methodenaufruf schlägt mit dem Fehlercode **400027** fehl.

## <a name="cause"></a>Ursache

Weil von einem anderen Client eine neue Verbindung mit IoT Hub unter Verwendung derselben Anmeldeinformationen erstellt wurde, hat IoT Hub die vorherige Verbindung geschlossen. IoT Hub lässt nicht zu, dass mehr als ein Client eine Verbindung unter Verwendung desselben Satzes von Anmeldeinformationen herstellt.

## <a name="solution"></a>Lösung

Stellen Sie sicher, dass jeder Client eine Verbindung mit IoT Hub unter Verwendung seiner eigenen Identität herstellt.