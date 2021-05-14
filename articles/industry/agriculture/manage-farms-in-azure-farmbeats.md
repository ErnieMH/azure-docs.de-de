---
title: Verwalten landwirtschaftlicher Betriebe
description: Hier erfahren Sie, wie Sie landwirtschaftliche Betriebe verwalten.
author: RiyazPishori
ms.topic: article
ms.date: 11/04/2019
ms.author: riyazp
ms.openlocfilehash: 70d7ae985a56842f2b02c5af5a7c7c1e589b2b56
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108759813"
---
# <a name="manage-farms"></a>Verwalten landwirtschaftlicher Betriebe

Sie können ihre landwirtschaftlichen Betriebe in Azure FarmBeats verwalten. In diesem Artikel erfahren Sie, wie Sie landwirtschaftliche Betriebe erstellen, Geräte und Sensoren installieren sowie Drohnen nutzen, um Sie bei der Verwaltung Ihrer landwirtschaftlichen Betriebe zu unterstützen.

## <a name="create-farms"></a>Erstellen landwirtschaftlicher Betriebe

Führen Sie die folgenden Schritte durch:

1. Melden Sie sich bei Farm Accelerator an. Daraufhin wird die Seite **Farms** (Landwirtschaftliche Betriebe) angezeigt.
    Die Seite **Farms** (Landwirtschaftliche Betriebe) enthält eine Liste der landwirtschaftlichen Betriebe, die ggf. bereits unter Ihrem Abonnement erstellt wurden.

    Das sieht in etwa wie folgt aus:

    ![Screenshot der Seite „Farms“](./media/create-farms-in-azure-farmbeats/create-farm-main-page-1.png)


2. Wählen Sie **Create Farm** (Landwirtschaftlichen Betrieb erstellen) aus, und machen Sie unter **Name**, **Crops** (Anbauprodukt) und **Adresse** entsprechende Angaben.
3. Wählen Sie im Pflichtfeld **Define Farm Boundary** (Grenze des landwirtschaftlichen Betriebs definieren) entweder **Mark on Map** (Auf Karte markieren) oder **Paste GeoJSON code** (GeoJSON-Code einfügen) aus.

Die Grenze eines landwirtschaftlichen Betriebs kann auf zwei Arten definiert werden:

1. **Mark on Map** (Auf Karte markieren): Sie können das Kartensteuerelement verwenden, um die Grenze des landwirtschaftlichen Betriebs einzuzeichnen. Klicken Sie zum Einzeichnen der Grenzen auf ![Screenshot des Stiftsymbols zum Einzeichnen der Grenzen auf der Karte](./media/create-farms-in-azure-farmbeats/pencil-icon-1.png), und zeichnen Sie die Grenzen exakt ein.

    ![Screenshot der eingezeichneten Grenzen auf einer Karte](./media/create-farms-in-azure-farmbeats/create-farm-mark-on-map-1.png)

2. **Paste GeoJSON Code** (GeoJSON-Code einfügen): GeoJSON ist ein Format zum Codieren geografischer Datenstrukturen mittels JavaScript Object Notation (JSON). Bei Verwendung dieser Option wird ein Textfeld angezeigt, in das eine GeoJSON-Zeichenfolge eingegeben werden kann, um die Grenzen des landwirtschaftlichen Betriebs zu markieren. GeoJSON-Code kann auch mittels GeoJSON.io erstellt werden.
Nutzen Sie die QuickInfos, wenn Sie Hilfe beim Ausfüllen benötigen.

    ![Screenshot des Bildschirms „Create Farm“ (Landwirtschaftlichen Betrieb erstellen), auf dem die Option „Paste GeoJson Code“ (GeoJSON-Code einfügen) hervorgehoben ist](./media/create-farms-in-azure-farmbeats/create-new-farm-1.png)

3.  Wählen Sie **Übermitteln** aus, um einen landwirtschaftlichen Betrieb zu erstellen. Ein neuer landwirtschaftlicher Betrieb wird erstellt und auf der Seite **Farms** (Landwirtschaftliche Betriebe) angezeigt.

## <a name="view-farm"></a>Anzeigen des landwirtschaftlichen Betriebs

Auf der Seite mit der Liste landwirtschaftlicher Betriebe wird eine Liste mit den erstellten landwirtschaftlichen Betrieben angezeigt. Wählen Sie einen landwirtschaftlichen Betrieb aus, um eine Liste mit folgenden Informationen anzuzeigen:

 - **Geräteanzahl**: Anzahl und Status der Geräte, die innerhalb des landwirtschaftlichen Betriebs bereitgestellt sind.
 - **Karte**: Karte des landwirtschaftlichen Betriebs mit den bereitgestellten Geräten.
 - **Telemetrie**: Die Telemetriedaten der innerhalb des landwirtschaftlichen Betriebs bereitgestellten Sensoren.
 - **Latest Precision Maps** (Neueste Präzisionskarten): Die neueste Satellitenindizes-Karte (EVI, NDWI), die neueste Heatmap für die Bodenfeuchtigkeit sowie die neueste Karte für Sensorplatzierungen.

## <a name="edit-farm"></a>Bearbeiten des landwirtschaftlichen Betriebs

Auf der Seite **Farms** (Landwirtschaftliche Betriebe) wird eine Liste mit den erstellten landwirtschaftlichen Betrieben angezeigt.

1.  Wählen Sie einen landwirtschaftlichen Betrieb aus, um ihn anzuzeigen und zu bearbeiten.
2.  Wählen Sie **Edit Farm** (Landwirtschaftlichen Betrieb bearbeiten) aus, um die Informationen zu bearbeiten. Im Fenster **Farm Details** (Details zum landwirtschaftlichen Betrieb) können Sie die Felder **Name**, **Crops** (Anbauprodukte) und **Adresse** bearbeiten sowie **Farm Boundary** (Grenze des landwirtschaftlichen Betriebs) definieren.

    ![FarmBeats-Projekt](./media/create-farms-in-azure-farmbeats/edit-farm-1.png)

3. Wählen Sie **Übermitteln** aus, um die bearbeiteten Details zu speichern.

## <a name="delete-farm"></a>Löschen eines landwirtschaftlichen Betriebs

Auf der Seite **Farms** (Landwirtschaftliche Betriebe) wird eine Liste mit den erstellten landwirtschaftlichen Betrieben angezeigt. Führen Sie die folgenden Schritte aus, um einen landwirtschaftlichen Betrieb löschen:

1.  Wählen Sie in der Liste einen landwirtschaftlichen Betrieb aus, dessen Details Sie löschen möchten.
2.  Wählen Sie **Delete Farm** (Landwirtschaftlichen Betrieb löschen) aus, um den landwirtschaftlichen Betrieb zu löschen.

    ![Screenshot des Bildschirms „Delete Farm“ (Landwirtschaftlichen Betrieb löschen), auf dem die Schaltfläche „Delete“ (Löschen) hervorgehoben ist](./media/create-farms-in-azure-farmbeats/delete-farm-1.png)

    > [!NOTE]
    > Wenn Sie einen landwirtschaftlichen Betrieb löschen, bleiben die zugeordneten Geräte und Karten erhalten. Informationen des landwirtschaftlichen Betriebs, die mit den Geräten und Karten verknüpft sind, sind nicht mehr relevant. Geräte, Telemetriedaten und Karten können weiterhin über den FarmBeats-Dienst angezeigt werden.


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun Ihren landwirtschaftlichen Betrieb erstellt haben, informieren Sie sich als Nächstes darüber, wie Sie in Ihrem landwirtschaftlichen Betrieb eingehende [Sensordaten abrufen](get-sensor-data-from-sensor-partner.md).
