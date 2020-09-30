---
title: Kontingente und Einschränkungen von Live Video Analytics in IoT Edge | Azure
description: In diesem Artikel werden die Kontingente und Einschränkungen von Live Video Analytics in IoT Edge beschrieben.
ms.topic: conceptual
ms.date: 05/22/2020
ms.openlocfilehash: df1978de4ee1bbbe15d0df3b02a70fb51491e9d2
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2020
ms.locfileid: "90529229"
---
# <a name="quotas-and-limitations"></a>Kontingente und Einschränkungen

In diesem Artikel werden die Kontingente und Einschränkungen im Modul Live Video Analytics in IoT Edge aufgelistet.

## <a name="maximum-period-of-disconnected-use"></a>Maximaler Verwendungszeitraum bei getrennter Verbindung

Das Edgemodul kann bei einem zeitlich begrenzten Verlust der Netzwerkverbindung fortgesetzt werden. Wenn das Modul länger als 36 Stunden getrennt bleibt, deaktiviert es alle ausgeführten Diagramminstanzen und blockiert alle weiteren direkten Methodenaufrufe.

Wenn Sie das Edgemodul wieder in den Betriebsstatus versetzen möchten, müssen Sie die Netzwerkkonnektivität wiederherstellen. Außerdem muss das Modul erfolgreich mit dem Azure Media Services-Konto kommunizieren können.

## <a name="maximum-number-of-graph-instances"></a>Maximale Anzahl von Diagramminstanzen

Sie können pro Modul über maximal 1.000 Diagramminstanzen verfügen (erstellt mit GraphInstanceSet).

## <a name="maximum-number-of-graph-topologies"></a>Maximale Anzahl von Diagrammtopologien

Sie können pro Modul über maximal 50 Diagrammtopologien verfügen (erstellt mit GraphTopologySet).

## <a name="limitations-on-graph-topologies-at-preview"></a>Einschränkungen bei Diagrammtopologien in der Vorschauversion

Für die Vorschauversion gelten Einschränkungen für die unterschiedlichen Knoten, die in einer Mediendiagrammtopologie miteinander verbunden werden können.

* RTSP-Quelle
   * Pro Diagrammtopologie ist nur eine RTSP-Quelle zulässig.
* Verarbeitungsknoten für Frameratenfilter
   * Muss direkt hinter dem RTSP-Quellknoten oder dem Verarbeitungsknoten für die Bewegungserkennung angeordnet sein.
   * Darf nicht hinter einem HTTP- oder gRPC-Erweiterungsprozessor angeordnet sein.
   * Darf nicht vor einem Verarbeitungsknoten für die Bewegungserkennung angeordnet sein.
* Verarbeitungsknoten für die HTTP-Erweiterung
   * Pro Diagrammtopologie ist maximal ein solcher Verarbeitungsknoten zulässig.
* gRPC-Erweiterungsprozessor
   * Pro Diagrammtopologie ist maximal ein solcher Verarbeitungsknoten zulässig.
* Verarbeitungsknoten für die Bewegungserkennung
   * Muss direkt hinter dem RTSP-Quellknoten angeordnet sein.
   * Pro Diagrammtopologie ist maximal ein solcher Verarbeitungsknoten zulässig.
   * Darf nicht hinter einem HTTP- oder gRPC-Erweiterungsprozessor angeordnet sein.
* Signalgateprozessor
   * Muss direkt hinter dem RTSP-Quellknoten angeordnet sein.
* Medienobjektsenke 
   * Muss direkt hinter dem RTSP-Quellknoten oder Signalgateprozessor angeordnet sein.
* Dateisenke
   * Muss direkt hinter dem Signalgateprozessor angeordnet sein.
   * Darf nicht direkt hinter einem HTTP- oder gRPC-Erweiterungs- oder Bewegungserkennungsprozessor angeordnet sein.
* IoT Hub-Senke
   * Darf nicht direkt hinter einer IoT Hub-Quelle angeordnet sein.

Wenn Verarbeitungsknoten sowohl für die Bewegungserkennung als auch die Frameratenfilterung verwendet werden, sollten diese sich in derselben Knotenkette befinden, die zum RTSP-Quellknoten führt.

## <a name="limitations-on-media-service-operations-at-preview"></a>Einschränkungen bei Media Service-Vorgängen in der Vorschauversion

Während der Vorschauphase unterstützt Live Video Analytics in IoT Edge Folgendes nicht:

* Die Möglichkeit, das Media Services-Konto ohne Unterbrechung von einem Abonnement zu einem anderen zu migrieren
* Die Möglichkeit, mehrere Speicherkonten mit dem Media Services-Konto zu verwenden
* Die Möglichkeit, die Informationen zum Dienstprinzipal in den gewünschten Eigenschaften des Moduls dynamisch zu ändern, ohne dass ein Neustart erforderlich ist

Es können nur IP-Kameras verwendet werden, die das RTSP-Protokoll unterstützen. IP-Kameras, die RTSP unterstützen, finden Sie auf der [Seite mit den ONVIF-konformen Produkten](https://www.onvif.org/conformant-products). Suchen Sie nach Geräten, die mit den Profilen G, S oder T konform sind.

Außerdem sollten diese Kameras für die Verwendung von H.264-Video und AAC-Audio konfiguriert werden. Andere Codecs werden zurzeit nicht unterstützt. 

## <a name="next-steps"></a>Nächste Schritte

[Übersicht](overview.md)
