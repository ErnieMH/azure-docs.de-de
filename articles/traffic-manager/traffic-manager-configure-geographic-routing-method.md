---
title: 'Tutorial: Konfigurieren von geografischem Routing für Datenverkehr mit Azure Traffic Manager'
description: In diesem Tutorial wird die Konfiguration der geografischen Routingmethode für Datenverkehr mit Azure Traffic Manager beschrieben.
services: traffic-manager
author: duongau
manager: kumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: duau
ms.openlocfilehash: 53773d7c616edec067e1ed1778b7ce6b500ee936
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89462614"
---
# <a name="tutorial-configure-the-geographic-traffic-routing-method-using-traffic-manager"></a>Tutorial: Konfigurieren der geografischen Routingmethode für Datenverkehr mithilfe von Traffic Manager

Die geografische Routingmethode für Datenverkehr ermöglicht das Leiten von Datenverkehr an bestimmte Endpunkte auf der Grundlage des geografischen Ursprungsorts von Anforderungen. Dieses Tutorial zeigt, wie ein Traffic Manager-Profil mit dieser Routingmethode erstellt wird und die Endpunkte für den Empfang von Datenverkehr von angegebenen Geografien konfiguriert werden.

## <a name="create-a-traffic-manager-profile"></a>Erstellen eines Traffic Manager-Profils

1. Melden Sie sich im Browser beim [Azure-Portal](https://portal.azure.com) an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose einmonatige Testversion](https://azure.microsoft.com/free/) registrieren.
2. Klicken Sie auf **Ressource erstellen** > **Netzwerk** > **Traffic Manager-Profil** > **Erstellen**.
4. Führen Sie unter **Traffic Manager-Profil erstellen** folgende Aktionen aus:
    1. Geben Sie einen Namen für Ihr Profil an. Dieser Name muss innerhalb der Zone trafficmanager.net eindeutig sein. Um auf das Traffic Manager-Profil zuzugreifen, verwenden Sie den DNS-Namen `<profilename>.trafficmanager.net`.
    2. Wählen Sie die Routingmethode **Geographisch** aus.
    3. Wählen Sie das Abonnement aus, unter dem Sie das Profil erstellen möchten.
    4. Verwenden Sie eine vorhandene Ressourcengruppe, oder erstellen Sie eine neue Ressourcengruppe, unter der Sie das Profil platzieren möchten. Wenn Sie sich für die Erstellung einer neuen Ressourcengruppe entscheiden, verwenden Sie die Dropdownliste **Ressourcengruppenstandort**, um den Speicherort der Ressourcengruppe anzugeben. Diese Einstellung bezieht sich auf den Speicherort der Ressourcengruppe und hat keine Auswirkungen auf das global bereitgestellte Traffic Manager-Profil.
    5. Nachdem Sie auf **Erstellen** klicken, wird Ihr Traffic Manager-Profil erstellt und global bereitgestellt.

![Erstellen eines Traffic Manager-Profils](./media/traffic-manager-geographic-routing-method/create-traffic-manager-profile.png)

## <a name="add-endpoints"></a>Hinzufügen von Endpunkten

1. Suchen Sie auf der Suchleiste des Portals nach dem Namen des erstellten Traffic Manager-Profils, und klicken Sie auf das Ergebnis, wenn es angezeigt wird.
2. Navigieren Sie in Traffic Manager zu **Einstellungen** -> **Endpunkte**.
3. Klicken Sie auf **Hinzufügen**, und füllen Sie im angezeigten Bereich **Endpunkt hinzufügen** die Werte wie folgt aus:
4. Wählen Sie je nach dem Typ des hinzugefügten Endpunkts **Typ** aus. Für geografische Routingprofile, die in Produktionsumgebungen verwendet werden, empfehlen wir dringend die Verwendung von geschachtelten Endpunkttypen, die ein untergeordnetes Profil mit mehr als einem Endpunkt enthalten. Weitere Details finden Sie unter [Häufig gestellte Fragen zu geografischen Routingmethoden für Datenverkehr](traffic-manager-FAQs.md).
5. Geben Sie einen **Namen** an, den Sie zur Bezeichnung des Endpunkts verwenden möchten.
6. Bestimmte Felder auf dieser Seite sind vom Typ des hinzugefügten Endpunkts abhängig:
    1. Wenn Sie einen Azure-Endpunkt hinzufügen, wählen Sie basierend auf der Ressource, zu der Sie Datenverkehr leiten möchten, den **Zielressourcentyp** und das **Ziel** aus
    2. Wenn Sie einen **externen** Endpunkt hinzufügen, geben Sie den **vollqualifizierten Domänennamen (FQDN)** für Ihren Endpunkt an.
    3. Wenn Sie einen **geschachtelten Endpunkt** hinzufügen, wählen Sie die **Zielressource** aus, die dem untergeordneten Profil entspricht, das Sie verwenden möchten, und legen Sie die **Mindestanzahl der untergeordneten Endpunkte** fest.
7. Verwenden Sie im Abschnitt für die geografische Zuordnung das Dropdownfeld, um die Regionen hinzuzufügen, aus denen Datenverkehr an den betreffenden Endpunkt gesendet werden soll. Sie müssen mindestens eine Region hinzufügen, und es können mehrere Regionen zugeordnet werden.
8. Wiederholen Sie dies für alle Endpunkte, die unter dem betreffenden Profil hinzugefügt werden sollen

![Hinzufügen eines Traffic Manager-Endpunkts](./media/traffic-manager-geographic-routing-method/add-traffic-manager-endpoint.png)

## <a name="use-the-traffic-manager-profile"></a>Verwenden des Traffic Manager-Profils
1.  Suchen Sie in der Suchleiste des Portals nach dem Namen des **Traffic Manager-Profils**, das Sie im vorhergehenden Abschnitt erstellt haben, und klicken Sie in den angezeigten Ergebnissen auf das Traffic Manager-Profil.
2. Klicken Sie auf **Overview**.
3. Unter **Traffic Manager-Profil** wird der DNS-Name Ihres neu erstellten Traffic Manager-Profils angezeigt. Dieser kann von beliebigen Clients (etwa durch Navigation in einem Webbrowser) für das Routing zum richtigen Endpunkt nach Maßgabe des Routingtyps verwendet werden.  Im Fall von geografischem Routing untersucht Traffic Manager die IP-Quelladresse der eingehenden Anforderung und bestimmt die Region, aus der sie stammt. Wenn diese Region einem Endpunkt zugeordnet ist, wird der Datenverkehr dorthin geroutet. Wenn diese Region keinem Endpunkt zugeordnet ist, gibt Traffic Manager die Abfrageantwort NODATA zurück.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Geografische Methode für das Datenverkehrsrouting](traffic-manager-routing-methods.md#geographic).
- Informationen zum [Testen von Traffic Manager-Einstellungen](traffic-manager-testing-settings.md).
