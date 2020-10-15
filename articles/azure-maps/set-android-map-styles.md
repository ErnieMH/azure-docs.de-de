---
title: Festlegen des Kartenstils mithilfe des Android SDK von Azure Maps | Microsoft Azure Maps
description: Erlernen Sie zwei Möglichkeiten, den Stil einer Karte festzulegen. Erfahren Sie, wie Sie den Stil mit dem Azure Maps Android SDK in der Layoutdatei oder der „Activity“-Klasse anpassen.
author: anastasia-ms
ms.author: v-stharr
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 4e37ae82b46ce06162d0a67d74af54cddaf3f6eb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88030944"
---
# <a name="set-map-style-using-azure-maps-android-sdk"></a>Festlegen des Kartenstils mithilfe des Android SDK von Azure Maps

In diesem Artikel werden Ihnen zwei Möglichkeiten gezeigt, Kartenstile mit dem Android SDK von Azure Maps festzulegen. In Azure Maps stehen sechs Kartenstile zur Auswahl. Weitere Informationen zu unterstützten Kartenstilen finden Sie unter [Unterstützte Kartenstile in Azure Maps](./supported-map-styles.md).


## <a name="prerequisites"></a>Voraussetzungen

Um den Vorgang in diesem Artikel abzuschließen, müssen Sie das [Android SDK für Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) installieren, um eine Karte zu laden.


## <a name="set-map-style-in-the-layout"></a>Festlegen des Kartenstils im Layout

In der Layoutdatei für Ihre „Activity“-Klasse können Sie einen Kartenstil festlegen. Bearbeiten Sie **res > layout > activity_main.xml** so, dass die Datei wie folgt aussieht:

```XML
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >

    <com.microsoft.azure.maps.mapcontrol.MapControl
        android:id="@+id/mapcontrol"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:mapcontrol_centerLat="47.602806"
        app:mapcontrol_centerLng="-122.329330"
        app:mapcontrol_zoom="12"
        app:mapcontrol_style="grayscale_dark"
        />

</FrameLayout>
```

Das obige `mapcontrol_style`-Attribut legt den Kartenstil auf **grayscale_dark** fest. 

<center>

![style-grayscale_dark](./media/set-android-map-styles/grayscale-dark.png)</center>

## <a name="set-map-style-in-the-activity-class"></a>Festlegen des Kartenstils in der „Activity“-Klasse

Der Kartenstil kann in der „Activity“-Klasse festgelegt werden. Kopieren Sie den folgenden Codeausschnitt in die **onCreate()** -Methode Ihrer `MainActivity.java`-Klasse. Dieser Code legt den Kartenstil auf **satellite_road_labels** fest.

```Java
mapControl.onReady(map -> {
    //Set the camera of the map.
    map.setCamera(center(47.64, -122.33), zoom(14));

    //Set the style of the map.
    map.setStyle(style(MapStyle.SATELLITE));
});
```

<center>

![style-satellite-road-labels](./media/set-android-map-styles/satellite-road-labels.png)</center>