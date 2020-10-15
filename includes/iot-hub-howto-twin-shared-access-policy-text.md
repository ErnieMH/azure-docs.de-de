---
title: include file
description: include file
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 08/07/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: e81ce2743d2509ce52f3190218decfa4e518bdad
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "70050456"
---
<!-- This contains intro text for the "Get an IoT hub connection string" section in the iot-hub-lang-lang-twin-getstarted.md files-->

In diesem Artikel erstellen Sie einen Back-End-Dienst, der einem Gerätezwilling die gewünschten Eigenschaften hinzufügt und anschließend die Identitätsregistrierung abfragt, um alle Geräte mit gemeldeten Eigenschaften zu ermitteln, die entsprechend aktualisiert wurden. Ihr Dienst benötigt die Berechtigung **Dienstverbindung**, um die gewünschten Eigenschaften eines Gerätezwillings ändern zu können, sowie die Berechtigung **Lesevorgänge in Registrierung**, um die Identitätsregistrierung abfragen zu können. Da keine SAS-Standardrichtlinie mit nur diesen beiden Berechtigungen zur Verfügung steht, müssen Sie eine erstellen.
