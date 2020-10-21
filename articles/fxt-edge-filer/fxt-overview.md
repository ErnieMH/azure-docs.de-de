---
title: Microsoft Azure FXT Edge Filer – Übersicht
description: Beschreibt Azure FXT Edge Filer-Hybridspeichercache, eine aktive Archiv- und Dateizugriffsbeschleuniger-Lösung für High Performance Computing.
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: overview
ms.date: 07/01/2019
ms.author: rohogue
ms.openlocfilehash: 72308bf64246a8041bd40fa557280014a13c95eb
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2020
ms.locfileid: "92218781"
---
# <a name="what-is-azure-fxt-edge-filer-hybrid-storage-cache"></a>Was ist Azure FXT Edge Filer-Hybridspeichercache?

Azure FXT Edge Filer ist eine Hybridspeichercache-Appliance, die schnellen Dateizugriff und ein aktives Archiv für High Performance Computing-Aufgaben (HPC) bietet.

Er arbeitet mit mehreren Datenquellen zusammen, unabhängig davon, ob sie in einem lokalen Rechenzentrum, remote oder in der Cloud gespeichert sind. Der Azure FXT Edge Filer kann einen einheitlichen Namespace für Daten in verschiedenen Speichersystemen bereitstellen.

Drei oder mehr FXT Edge Filer-Hardwaregeräte arbeiten als gruppiertes Dateisystem zusammen, um den Cache bereitzustellen. Weitere Informationen zum Kauf der erforderlichen Hardware erhalten Sie von Ihrem Microsoft-Vertreter.

Weitere Informationen finden Sie in den Produktinformationen und dem Datenblatt zu [Azur FXT Edge Filer](https://azure.microsoft.com/services/fxt-edge-filer/).

## <a name="use-cases"></a>Anwendungsfälle

Der Azure FXT Edge Filer verbessert die Produktivität am besten für solche Workflows:

* Workflow mit vielen Lesedateizugriffen
* NFSv3- oder SMB2-Protokoll
* Computefarmen mit 1.000 bis 100.000 CPU-Kernen

### <a name="nas-optimization-and-scaling"></a>NAS-Optimierung und -Skalierung

Mit dem Azure FXT Edge Filer-Cache können Sie den Zugriff auf bestehende NetApp- und Dell EMC Isilon NAS-Systeme erleichtern. Sie können auch Azure Blob oder anderen Cloudspeicher hinzufügen, um Skalierbarkeit zu gewährleisten, ohne die Datenzugriffsprozesse auf der Clientseite überarbeiten zu müssen.

### <a name="wan-caching"></a>WAN-Zwischenspeichern

Der Azure FXT Edge Filer kann verwendet werden, um den schnellen Dateizugriff von Powerusern zu unterstützen, wenn die benötigten Daten an anderem Ort gespeichert sind. Zugriff wird ermöglicht, während Sicherungs- und andere Datenverwaltungssysteme in einem zentralen Rechenzentrum verbleiben.

### <a name="active-archive-in-azure-blob"></a>Aktives Archiv im Azure-Blob

Erweitern Sie Ihr Rechenzentrum mit Azure FXT Edge Filer als Zugriffspunkt in den Cloudspeicher.

## <a name="features"></a>Features

Es sind zwei Hardwaremodelle verfügbar.

| Modell | DRAM | NVMe-SSD | Netzwerkports |
|-------|------|----------|---------------|
| FXT 6600 | 1\.536 GB | 25,6 TB | 6 x 25Gb/10Gb + 2 x 1Gb |
| FXT 6400 | 768 GB | 12,8 TB | 6 x 25Gb/10Gb + 2 x 1Gb |

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen über den Azure FXT Edge Filer finden Sie in den [Spezifikationen](fxt-specs.md) oder im [Installationstutorial](fxt-install.md).
* Erfahren Sie, wie Sie Azure FXT Edge Filer auf der [Azure FXT Edge Filer-Produktseite](https://azure.microsoft.com/services/fxt-edge-filer/) kaufen können.
