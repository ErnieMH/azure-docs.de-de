---
title: Verwalten des Zustands in Azure Service Fabric-Diensten
description: Hier erfahren Sie mehr über den Zustand in Azure Service Fabric, einschließlich der Vorgehensweise zum Definieren und Verwalten des Dienstzustands in Service Fabric-Diensten.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 9acd3031d1071d1822791b333976aaf76161600f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "75614569"
---
# <a name="service-state"></a>Dienstzustand
**Dienstzustand** bezieht sich auf die Daten (im Arbeitsspeicher oder auf einem Datenträger), die der Dienst für seine Ausführung benötigt. Er umfasst unter anderem die Datenstrukturen und Membervariablen, die der Dienst im Rahmen seiner Aufgaben liest und schreibt. Abhängig von der Architektur des Diensts können dazu auch Dateien oder andere Ressourcen gehören, die auf einem Datenträger gespeichert sind. Ein Beispiel wären etwa Dateien, die eine Datenbank zum Speichern von Daten und Transaktionsprotokollen verwendet.

Als Beispieldienst soll ein Rechner dienen. Ein einfacher Rechnerdienst berechnet die Summe zweier Zahlen und gibt sie zurück. Diese Berechnung umfasst keine Membervariablen oder anderen Informationen.

Nehmen wir nun an, der gleiche Rechner verfügt zusätzlich über eine Methode zum Speichern und Zurückgeben der zuletzt berechneten Summe. Dieser Dienst ist nun zustandsbehaftet. Zustandsbehaftet bedeutet, dass er über einen Zustand verfügt, in den er beim Berechnen einer neuen Summe schreibt und aus dem er beim Zurückgeben der zuletzt berechneten Summe liest.

In Azure Service Fabric wird der erste Dienst als „zustandsloser Dienst“ bezeichnet. Der zweite Dienst wird „zustandsbehafteter Dienst“ genannt.

## <a name="storing-service-state"></a>Speichern des Dienstzustands
Der Zustand kann ausgelagert oder mit dem Code, der den Zustand bearbeitet, gespeichert werden. Die Auslagerung des Zustands erfolgt in der Regel mithilfe einer externen Datenbank oder einem anderen Datenspeicher, der über das Netzwerk auf verschiedenen Computern oder außerhalb des Prozesses auf dem gleichen Computer ausgeführt wird. In unserem Rechnerbeispiel kann es sich bei dem Datenspeicher etwa um eine SQL-Datenbank oder um eine Azure-Tabellenspeicherinstanz handeln. Diese Daten werden bei jeder Summenberechnungsanforderung aktualisiert, und wenn vom Dienst die Rückgabe des Werts angefordert wird, wird der aktuelle Wert aus dem Speicher abgerufen. 

Der Zustand kann auch gemeinsam mit dem Code gespeichert werden, der den Zustand ändert. Zustandsbehaftete Dienste in Service Fabric basieren für gewöhnlich auf diesem Modell. Die von Service Fabric bereitgestellte Infrastruktur stellt sicher, dass dieser Zustand hoch verfügbar, konsistent und beständig ist und dass die so erstellten Dienste problemlos skalierbar sind.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den Service Fabric-Konzepten finden Sie in den folgenden Artikeln:

* [Verfügbarkeit der Service Fabric-Dienste](service-fabric-availability-services.md)
* [Scaling Service Fabric Applications (in englischer Sprache)](service-fabric-concepts-scalability.md)
* [Partitionieren von Service Fabric-Diensten](service-fabric-concepts-partitioning.md)
* [Service Fabric Reliable Services](service-fabric-reliable-services-introduction.md)
