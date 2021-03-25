---
title: Installieren des Microsoft Azure StorSimple 8100-Geräts
description: Beschreibt, wie Sie das StorSimple 8100-Gerät auspacken, in ein Rack einbauen und verkabeln., bevor Sie die Software bereitstellen und konfigurieren.
author: alkohli
ms.assetid: 6098a01e-c031-488a-a8d7-0b607ce665e1
ms.service: storsimple
ms.topic: conceptual
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: 113b72ddf7e5d508c8a0b577d4004d4fbd83e8e5
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "84699106"
---
# <a name="unpack-rack-mount-and-cable-your-storsimple-8100-device"></a>Auspacken, Einbauen und Verkabeln des StorSimple 8100-Geräts

[!INCLUDE [storsimple-8000-eol-banner](../../includes/storsimple-8000-eol-banner.md)]

## <a name="overview"></a>Übersicht
Microsoft Azure StorSimple 8100 ist ein Gerät mit einem Gehäuse, das in ein Rack eingebaut wird. In diesem Tutorial wird erläutert, wie die StorSimple 8100-Gerätehardware vor dem Konfigurieren und Bereitstellen des StorSimple-Geräts ausgepackt, in ein Rack eingebaut und verkabelt wird.

## <a name="unpack-your-storsimple-8100-device"></a>Auspacken des StorSimple 8100-Geräts
Die folgenden Schritte bieten klare und ausführliche Anweisungen zum Auspacken des StorSimple 8100-Speichergeräts. Dieses Gerät wird in einem einzelnen Karton ausgeliefert.

### <a name="prepare-to-unpack-your-device"></a>Vorbereitungen zum Auspacken des Geräts
Lesen Sie die folgenden Informationen, bevor Sie das Gerät auspacken.

![Symbol „Warnung“](./media/storsimple-safety/IC740879.png)![Symbol „Schwergewicht“](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **WARNUNG!**

1. Stellen Sie aufgrund des Gewichts des Gehäuses sicher, dass zwei Personen verfügbar sind, wenn Sie den Vorgang manuell durchführen. Ein vollständig konfiguriertes Gehäuse kann bis zu 32 kg wiegen.
2. Legen Sie den Karton auf einen flachen, ebenen Untergrund.

Führen Sie dann die folgenden Schritte aus, um das Gerät auszupacken.

#### <a name="to-unpack-your-device"></a>So packen Sie das Gerät aus
1. Überprüfen Sie den Karton und das Verpackungsmaterial auf Risse, Schnitte, Wasserschäden oder anderweitige offensichtliche Beschädigungen. Wenn der Karton oder die Verpackung stark beschädigt sind, öffnen Sie den Karton nicht. [Wenden Sie sich an den Microsoft Support](storsimple-8000-contact-microsoft-support.md) , um zu ermitteln, ob sich das Gerät in funktionsfähigem Zustand befindet.
2. Packen Sie den Karton aus. Die folgende Abbildung zeigt das ausgepackte StorSimple-Gerät.
   
     ![Auspacken des Speichergeräts](./media/storsimple-8100-hardware-installation/HCSUnpackyour2Udevice.png)
   
    **Das ausgepackte Speichergerät**
   
   | Bezeichnung | BESCHREIBUNG |
   | --- | --- |
   |   1 |Karton |
   |   2 |Untere Styroporeinlage |
   |   3 |Sicherungsmedium |
   |   4 |Obere Styroporeinlage |
   |   5 |Zubehörkarton |
3. Stellen Sie nach dem Auspacken des Kartons sicher, dass Folgendes vorhanden ist:
   
   * Ein Gerät mit einem Gehäuse
   * Zwei Netzkabel
   * Ein Crossover-Ethernet-Kabel
   * Zwei serielle Konsolenkabel
   * Ein Seriell-USB-Konverter für seriellen Zugriff
   * Ein T10-Sicherheitsschraubendreher
   * Vier QSFP-zu-SFP+-Adapter für die Verwendung mit 10-GbE-Netzwerkschnittstellen
   * Ein Rackmontagekit (zwei Seitenschienen mit Befestigungsteilen)
   * Dokumentation "Erste Schritte"
     
     Wenn Sie eines der oben aufgeführten Teile nicht erhalten haben, [wenden Sie sich an den Microsoft Support](storsimple-8000-contact-microsoft-support.md).

Im nächsten Schritt bauen Sie das Gerät in ein Rack ein.

## <a name="rack-mount-your-storsimple-8100-device"></a>Einbauen des StorSimple 8100-Geräts in ein Rack
Führen Sie die folgenden Schritte aus, um das StorSimple 8100-Speichergerät in ein 19-Zoll-Standardrack mit Pfosten auf Vorder- und Rückseite einzubauen. Das StorSimple 8100-Gerät verfügt über ein einziges, primäres Gehäuse.

Die Installation umfasst mehrere Schritte, die im Folgenden detailliert erläutert werden.

> [!IMPORTANT]
> StorSimple-Geräte müssen für den ordnungsgemäßen Betrieb in ein Rack eingebaut werden.
> 
> 

### <a name="prepare-the-site"></a>Vorbereiten des Standorts
Das Gerät wird in ein 19-Zoll-Standardrack mit Pfosten an Vorder- und Rückseite eingebaut. Gehen Sie folgendermaßen vor, um die Rackmontage vorzubereiten.

#### <a name="to-prepare-the-site-for-rack-installation"></a>So bereiten Sie den Standort für die Rackmontage vor
1. Vergewissern Sie sich, dass das Gerät sicher auf einer flachen, stabilen und ebenen Arbeitsfläche (oder ähnlich) liegt.
2. Stellen Sie sicher, dass am vorgesehenen Standort eine Standardstromversorgung von einer unabhängigen Quelle oder eine Rack-PDU (Power Distribution Unit) mit unterbrechungsfreier Stromversorgung (USV) vorhanden ist.
3. Vergewissern Sie sich, dass das Rack, in das Sie das Gerät einbauen möchten, Platz für einen Einschub mit 2 HE bietet.

![Symbol „Warnung“](./media/storsimple-safety/IC740879.png)![Symbol „Schwergewicht“](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **WARNUNG!**

Stellen Sie aufgrund des Gewichts des Geräts sicher, dass zwei Personen verfügbar sind, wenn Sie die Einrichtung des Geräts manuell durchführen. Ein vollständig konfiguriertes Gehäuse kann bis zu 32 kg wiegen.

### <a name="rack-prerequisites"></a>Voraussetzungen
Das Gehäuse des 8100-Geräts ist für den Einbau in einen 19-Zoll-Standardrackschrank vorgesehen, wobei Folgendes gilt:

* Mindesttiefe des Racks von 707,14 mm (27,84 Zoll) von Pfosten zu Pfosten
* Maximales Gewicht des Geräts von 32 kg
* Maximaler Gegendruck von 5 Pascal (0,5 mm Wassersäule)

### <a name="rack-mounting-rail-kit"></a>Schienenkit für die Rackmontage
Im Lieferumfang ist ein Satz Montageschienen für die Verwendung mit einem 19-Zoll-Rackschrank enthalten. Die Schienen wurden für das maximale Gehäusegewicht getestet. Mit den Schienen können auch mehrere Gehäuse in das Rack eingebaut werden, ohne dass dabei Platz verloren geht.

#### <a name="to-install-the-device-on-the-rails"></a>So befestigen Sie das Gerät an den Schienen
1. Führen Sie diesen Schritt nur aus, wenn in Ihrem Gerät keine inneren Schienen installiert sind. In der Regel sind die inneren Schienen bereits werkseitig installiert. Ist dies nicht der Fall, bringen Sie die linke und die rechte Gleitschiene an den Seiten des Gehäuses an. Sie werden auf jeder Seite mit sechs metrischen Schrauben befestigt. Als Hilfe bei der Ausrichtung sind die Gleitschienen mit **LH – Front** (Vorne links) und **RH – Front** (Vorne rechts) gekennzeichnet. Darüber hinaus weisen die Schienen jeweils an dem Ende, das in Richtung der Rückseite des Gehäuses befestigt wird, eine Verjüngung auf.<br/>
   
    ![Befestigen von Gleitschienen am Gehäuse](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)

    **Befestigen der inneren Gleitschienen an den Seiten des Gehäuses**
   
    Bezeichnung | BESCHREIBUNG
    ----- | -----------
    1     | Rundkopfschrauben M3 x 4
    2     | Gehäusegleitschiene

2. Befestigen Sie die linke und die rechte äußere Schienenbaugruppe an den vertikalen Blechwinkeln des Rackschranks. Die Halterungen sind mit **LH** (Links), **RH** (Rechts) und **This side up** (Diese Seite nach oben) gekennzeichnet, um Sie bei der richtigen Ausrichtung zu unterstützen.
3. Suchen Sie die Fixierungsstifte, die sich vorne und hinten an der Schienenbaugruppe befinden. Verlängern Sie die Schiene, sodass sie zwischen die Rackpfosten passt, und führen Sie die Stifte in die Bohrungen der vertikalen Blechwinkel am vorderen und hinteren Pfosten ein. Achten Sie darauf, dass die Schienenbaugruppe waagerecht ausgerichtet ist.
4. Sichern Sie die Schienenbaugruppe mit zwei der mitgelieferten metrischen Schrauben an den vertikalen Blechwinkeln des Racks. Verwenden Sie eine Schraube an der Vorderseite und eine Schraube an der Rückseite.
5. Wiederholen Sie diese Schritte für die andere Schienenbaugruppe.<br/>
   
     ![Befestigen von Gleitschienen am Rackschrank](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)
   
    **Befestigen der äußeren Schienenbaugruppen am Rack**
   
   | Bezeichnung | BESCHREIBUNG |
   | --- | --- |
   |   1 |Klemmschraube |
   |   2 |Vierkantloch-Schraube für vorderen Rackpfosten |
   |   3 |Linke Schiene vorn, Fixierungsstifte |
   |   4 |Klemmschraube |
   |   5 |Linke Schiene hinten, Fixierungsstifte |

### <a name="mounting-the-device-in-the-rack"></a>Einbauen des Geräts in das Rack
Führen Sie die folgenden Schritte aus, um das Gerät unter Verwendung der soeben installierten Schienen in das Rack einzubauen.

#### <a name="to-mount-the-device"></a>So bauen Sie das Gerät ein
1. Heben Sie das Gehäuse zusammen mit einem Helfer an, und richten sie es an den Rackschienen aus.
2. Führen Sie das Gerät sorgfältig in die Schienen ein, und schieben Sie es dann vollständig in den Rackschrank hinein.<br/>
   
    ![Einführen des Geräts in das Rack](./media/storsimple-8100-hardware-installation/HCSInsertingDeviceintheRack.png)
   
    **Einbauen des Geräts in das Rack**
3. Entfernen Sie links und rechts die vorderen Flanschkappen, indem Sie sie abziehen. Die Flanschkappen sind einfach an den Flanschen eingerastet.
4. Sichern Sie das Gehäuse im Rack am linken und am rechten Flansch mit je einer der mitgelieferten Kreuzschlitzschrauben.
5. Bringen Sie die Flanschkappen an, indem Sie diese an der vorgesehenen Position andrücken und einrasten lassen.<br/>
   
     ![Anbringen der Flanschkappen](./media/storsimple-8100-hardware-installation/HCSInstallingFlangeCaps.png)
   
    **Anbringen der Flanschkappen**
   
   | Bezeichnung | BESCHREIBUNG |
   | --- | --- |
   |   1 |Befestigungsschraube für Gehäuse |

Im nächsten Schritt verkabeln Sie das Gerät für die Stromversorgung, die Netzwerkverbindung und den seriellen Zugriff.

## <a name="cable-your-storsimple-8100-device"></a>Verkabeln des StorSimple 8100-Geräts
Im Folgenden wird erläutert, wie Sie das StorSimple 8100-Gerät für die Stromversorgung, die Netzwerkverbindung und serielle Verbindungen verkabeln.

### <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit dem Verkabeln des Geräts beginnen können, benötigen Sie Folgendes:

* Speichergerät, vollständig ausgepackt und im Rack eingebaut
* Zwei Netzkabel, die zum Lieferumfang des Geräts gehören
* Zugang zu zwei PDUs (Power Distribution Units) (empfohlen)
* Netzwerkkabel
* Mitgelieferte serielle Kabel
* Seriell-USB-Konverter, für den der entsprechende Treiber auf dem PC installiert ist (sofern erforderlich)
* Vier beigefügte QSFP-zu-SFP+-Adapter für die Verwendung mit 10-GbE-Netzwerkschnittstellen
* [Unterstützte Hardware für 10-GbE-Netzwerkschnittstellen auf Ihrem StorSimple-Gerät](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="power-cabling"></a>Stromverkabelung
Das Gerät enthält redundante Stromversorgungs- und Kühleinheiten (Power and Cooling Modules, PCMs). Beide PCMs müssen installiert und mit unterschiedlichen Stromquellen verbunden sein, um Hochverfügbarkeit sicherzustellen.

Führen Sie die folgenden Schritte aus, um das Gerät für die Stromversorgung zu verkabeln.

[!INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

### <a name="network-cabling"></a>Netzwerkverkabelung
Das Gerät besitzt eine Konfiguration mit aktivem Standbymodus: Zu jedem Zeitpunkt ist ein Controllermodul aktiv und verarbeitet alle Datenträger- und Netzwerkvorgänge, während sich das andere Controllermodul im Standbymodus befindet. Wenn ein Controller ausfällt, wird der Standbycontroller sofort aktiviert und setzt alle Datenträger- und Netzwerkvorgänge fort.

Verkabeln Sie Ihr Gerätenetzwerk wie in den folgenden Schritten beschrieben, um dieses redundante Controllerfailover zu unterstützen.

#### <a name="to-cable-for-network-connection"></a>So verkabeln Sie das Gerät für die Netzwerkverbindung
1. Das Gerät verfügt über sechs Netzwerkschnittstellen an jedem Controller: vier Ethernet-Anschlüsse mit 1 GBit/s und zwei Ethernet-Anschlüsse mit 10 GBit/s. Identifizieren Sie die verschiedenen Datenanschlüsse an der Rückwand des Geräts.
   
    ![Rückwand des 8100-Geräts](./media/storsimple-8100-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)
   
    **Rückseite des Geräts mit Datenanschlüssen**
   
   | Bezeichnung | BESCHREIBUNG |
   | --- | --- |
   |   0,1,4,5 |1-GbE-Netzwerkschnittstellen |
   |   2,3 |10-GbE-Netzwerkschnittstellen |
   |   6 |Serielle Anschlüsse |
2. Die Netzwerkverkabelung ist im folgenden Diagramm dargestellt. (Die Mindestkonfiguration des Netzwerks ist durch durchgängige blaue Linien gekennzeichnet. Die für Hochverfügbarkeit und Leistung zusätzlich erforderliche Konfiguration wird durch die gepunkteten Linien dargestellt.)

    ![Netzwerkverkabelung des 2 HE-Geräts](./media/storsimple-8100-hardware-installation/HCSCableYour2UDeviceforNetwork.png)

    **Netzwerkverkabelung des Geräts**

   |Bezeichnung | BESCHREIBUNG |
   |----- | ----------- |
   | Ein    | LAN mit Internetzugriff |
   | B    | Controller 0 |
   | C    | PCM 0 |
   | D    | Controller 1 |
   | E    | PCM 1 |
   | F, G | Hosts |
   | 0-5  | Netzwerkschnittstellen |



Beim Verkabeln des Geräts ist die folgende Mindestkonfiguration erforderlich:

* Mindestens zwei Netzwerkschnittstellen pro Controller – eine für den Cloudzugriff und eine für iSCSI. Der DATA 0-Anschluss wird automatisch über die serielle Konsole des Geräts aktiviert und konfiguriert. Zusätzlich zu DATA 0 muss ein weiterer Datenanschluss über das klassische Azure-Portal konfiguriert werden. Verbinden Sie den DATA 0-Anschluss in diesem Fall mit dem primären LAN (Netzwerk mit Internetzugriff). Die anderen Datenanschlüsse können in Abhängigkeit von der vorgesehenen Rolle mit dem SAN/iSCSI-LAN (VLAN)-Segment des Netzwerks verbunden werden.
* Verbinden Sie identische Schnittstellen an jedem Controller mit demselben Netzwerk, um die Verfügbarkeit bei einem Controllerfailover sicherzustellen. Wenn Sie z. B. DATA 0 und DATA 3 bei einem der Controller verbinden, müssen Sie DATA 0 und DATA 3 auch am anderen Controller verbinden.

Beachten Sie zur Sicherstellung der Hochverfügbarkeit und Leistung Folgendes:

* Konfigurieren Sie auf jedem Controller nach Möglichkeit ein Netzwerkschnittstellenpaar für den Cloudzugriff (1 GbE) und ein weiteres Paar für iSCSI (10 GbE empfohlen).
* Verbinden Sie die Netzwerkschnittstellen jedes Controllers nach Möglichkeit mit zwei unterschiedlichen Switches, um sicherzustellen, dass die Verfügbarkeit auch bei einem Switchausfall gewährleistet ist. Die Abbildung zeigt die beiden 10 GbE-Netzwerkschnittstellen, DATA 2 und DATA 3, jedes Controllers, die mit zwei unterschiedlichen Switches verbunden sind.

Weitere Informationen finden Sie im Abschnitt **Netzwerkschnittstellen** unter [Anforderungen an die Hochverfügbarkeit für Ihr StorSimple-Gerät](storsimple-8000-system-requirements.md#high-availability-requirements-for-storsimple).

> [!NOTE]
> Verwenden Sie die beigefügten QSFP-SFP+-Adapter, wenn Sie SFP+-Transceiver mit Ihren 10-GbE-Netzwerkschnittstellen nutzen. Weitere Informationen finden Sie unter [Unterstützte Hardware für 10-GbE-Netzwerkschnittstellen auf Ihrem StorSimple-Gerät](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).
> 
> 

### <a name="serial-port-cabling"></a>Verkabelung des seriellen Anschlusses
Führen Sie die folgenden Schritte aus, um den seriellen Anschluss zu verkabeln.

#### <a name="to-cable-for-serial-connection"></a>So verkabeln Sie das Gerät für serielle Verbindungen
1. Das Gerät verfügt über einen seriellen Anschluss an jedem Controller, der durch ein Schraubenschlüsselsymbol gekennzeichnet ist. Sehen Sie sich die Abbildung im Abschnitt [Netzwerkverkabelung](#network-cabling) an, um die seriellen Anschlüsse an der Rückwand des Geräts zu identifizieren.
2. Identifizieren Sie den aktiven Controller anhand der Rückwand des Geräts. Eine blinkende blaue LED zeigt an, dass der Controller aktiv ist.
3. Verwenden Sie die mitgelieferten seriellen Kabel (bei Bedarf den USB-Seriell-Konverter für Ihren Laptop), und verbinden Sie die Konsole oder den Computer (mit Terminalemulation zum Gerät) mit dem seriellen Anschluss des aktiven Controllers.
4. Installieren Sie die Seriell-USB-Treiber (im Lieferumfang des Geräts enthalten) auf Ihrem Computer.
5. Richten Sie die serielle Verbindung wie folgt ein: 115.200 Baud, 8 Datenbits, 1 Stoppbit, keine Parität und keine Flusssteuerung.
6. Stellen Sie sicher, dass die Verbindung funktioniert, indem Sie auf der Konsole die EINGABETASTE drücken. Ein Menü der seriellen Konsole sollte angezeigt werden.

> [!NOTE]
> **Lights-Out-Management:** Wenn das Gerät in einem Remoterechenzentrum oder in einem Computerraum mit beschränktem Zugriff installiert ist, stellen Sie sicher, dass die seriellen Verbindungen zu beiden Controllern immer mit einem Switch einer seriellen Konsole oder einem ähnlichen Gerät verbunden sind. Dies ermöglicht bei Netzwerkunterbrechungen oder unerwarteten Fehlern Out-of-Band-Remotesteuerungs- und -Supportvorgänge.
> 
> 

Das Gerät ist jetzt für Stromversorgung, Netzwerkzugriff und serielle Konnektivität verkabelt. Im nächsten Schritt konfigurieren Sie die Software und stellen das Gerät bereit.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie Sie Ihr [lokales StorSimple-Gerät bereitstellen und konfigurieren](storsimple-8000-deployment-walkthrough-u2.md).

