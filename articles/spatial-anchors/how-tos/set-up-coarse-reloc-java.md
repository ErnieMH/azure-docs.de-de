---
title: Ungefähre Standortbestimmung in Java
description: Hier erfahren Sie ausführlich, wie Sie mithilfe der ungefähren Standortbestimmung in Java Anker erstellen und nach ihnen suchen.
author: bucurb
manager: dacoghl
services: azure-spatial-anchors
ms.custom: devx-track-java
ms.author: bobuc
ms.date: 09/19/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 01a58207d893b6e4dcadf0b8b295d4bf7ed2ec31
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87319030"
---
# <a name="how-to-create-and-locate-anchors-using-coarse-relocalization-in-java"></a>Erstellen von und Suchen nach Ankern mithilfe der ungefähren Standortbestimmung in Java

> [!div  class="op_single_selector"]
> * [Unity](set-up-coarse-reloc-unity.md)
> * [Objective-C](set-up-coarse-reloc-objc.md)
> * [Swift](set-up-coarse-reloc-swift.md)
> * [Android Java](set-up-coarse-reloc-java.md)
> * [C++/NDK](set-up-coarse-reloc-cpp-ndk.md)
> * [C++/WinRT](set-up-coarse-reloc-cpp-winrt.md)

Azure Spatial Anchors kann Daten eines geräteeigenen Positionierungssensors mit den von Ihnen erstellten Ankern verknüpfen. Diese Daten können auch verwendet werden, um schnell festzustellen, ob in der Nähe Ihres Geräts irgendwelche Anker vorhanden sind. Weitere Informationen finden Sie unter [Ungefähre Standortbestimmung](../concepts/coarse-reloc.md).

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie dieses Anleitungen ausführen können, stellen Sie sicher, dass die folgenden Bedingungen erfüllt sind:

- Java-Grundkenntnisse.
- Sie haben die Informationen unter [Azure Spatial Anchors-Übersicht](../overview.md) gelesen.
- Sie haben eine der [fünfminütigen Schnellstartanleitungen](../index.yml) absolviert.
- Sie haben die Informationen unter [Erstellen von und Suchen nach Ankern mit Azure Spatial Anchors](../create-locate-anchors-overview.md) gelesen.

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-configure-provider.md)]

```java
// Create the sensor fingerprint provider
PlatformLocationProvider sensorProvider = new PlatformLocationProvider();

// Allow GPS
SensorCapabilities sensors = sensorProvider.getSensors();
sensors.setGeoLocationEnabled(true);

// Allow WiFi scanning
sensors.setWifiEnabled(true);

// Populate the set of known BLE beacons' UUIDs
String uuids[] = new String[2];
uuids[0] = "22e38f1a-c1b3-452b-b5ce-fdb0f39535c1";
uuids[1] = "a63819b9-8b7b-436d-88ec-ea5d8db2acb0";

// Allow the set of known BLE beacons
sensors.setBluetoothEnabled(true);
sensors.setKnownBeaconProximityUuids(uuids);
```

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-configure-session.md)]

```java
// Set the session's sensor fingerprint provider
cloudSpatialAnchorSession.setLocationProvider(sensorProvider);

// Configure the near-device criteria
NearDeviceCriteria nearDeviceCriteria = new NearDeviceCriteria();
nearDeviceCriteria.setDistanceInMeters(5.0f);
nearDeviceCriteria.setMaxResultCount(25);

// Set the session's locate criteria
AnchorLocateCriteria anchorLocateCriteria = new AnchorLocateCriteria();
anchorLocateCriteria.setNearDevice(nearDeviceCriteria);
cloudSpatialAnchorSession.createWatcher(anchorLocateCriteria);
```

[!INCLUDE [Locate](../../../includes/spatial-anchors-create-locate-anchors-locating-events.md)]

[!INCLUDE [Configure Provider](../../../includes/spatial-anchors-set-up-coarse-reloc-next-steps.md)]