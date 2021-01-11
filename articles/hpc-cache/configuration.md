---
title: Konfigurieren von Azure HPC Cache-Einstellungen
description: Erläutert, wie zusätzliche Einstellungen für den Cache konfiguriert werden, z. B. MTU und no-root-squash, und wie auf die Expressmomentaufnahmen von Azure Blob Storage-Zielen zugegriffen wird.
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 12/21/2020
ms.author: v-erkel
ms.openlocfilehash: 02bf862cdc3b20ef3e5fdb024f474267efa0c70d
ms.sourcegitcommit: 6cca6698e98e61c1eea2afea681442bd306487a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/24/2020
ms.locfileid: "97760502"
---
# <a name="configure-additional-azure-hpc-cache-settings"></a>Konfigurieren zusätzlicher Azure HPC Cache-Einstellungen

Auf der Seite **Konfiguration** im Azure-Portal finden Sie Optionen zum Anpassen mehrerer Einstellungen. Die meisten Benutzer müssen für diese Einstellungen keine Änderungen gegenüber den Standardwerten vornehmen.

In diesem Artikel wird außerdem beschrieben, wie Sie die Momentaufnahmefunktion für Azure Blob Storage-Ziel verwenden. Die Momentaufnahmefunktion besitzt keine konfigurierbaren Einstellungen.

Um die Einstellungen anzuzeigen, öffnen Sie die Seite **Konfiguration** des Caches im Azure-Portal.

![Screenshot der Seite „Konfiguration“ im Azure-Portal](media/configuration.png)

> [!TIP]
> Im [Video „Verwalten von Azure HPC Cache“](https://azure.microsoft.com/resources/videos/managing-hpc-cache/) sehen Sie die Konfigurationsseite und deren Einstellungen.

## <a name="adjust-mtu-value"></a>Anpassen des MTU-Werts
<!-- linked from troubleshoot-nas article -->

Sie können die maximale Größe der Übertragungseinheit für den Cache mithilfe des Dropdownmenüs namens **MTU-Größe** auswählen.

Der Standardwert beträgt 1500 Bytes, aber Sie können ihn in 1400 ändern.

> [!NOTE]
> Wenn Sie die MTU-Größe des Caches verringern, stellen Sie sicher, dass die Clients und Speichersysteme, die mit dem Cache kommunizieren, dieselbe MTU-Einstellung oder einen niedrigeren Wert besitzen.

Wenn Sie den Wert für die Cache-MTU herabsetzen, kann dies hilfreich sein bei der Umgehung von Paketgrößeneinschränkungen im restlichen Netzwerk des Caches. Beispielsweise können einige VPNs keine 1500-Byte-Pakete in voller Größe erfolgreich übertragen. Wenn Sie die Größe der über das VPN gesendeten Pakete verringern, könnte dies das Problem beseitigen. Beachten Sie jedoch, dass eine niedrigere MTU-Einstellung des Caches bedeutet, dass alle anderen Komponenten, die mit dem Cache kommunizieren, einschließlich Clients und Speichersystemen, ebenfalls eine niedrigere MTU-Einstellung aufweisen müssen, um Kommunikationsprobleme zu vermeiden.

Wenn Sie die MTU-Einstellungen für andere Systemkomponenten nicht ändern möchten, sollten Sie die MTU-Einstellung des Caches nicht verringern. Es gibt noch andere Lösungen, um Einschränkungen der VPN-Paketgröße zu umgehen. Weitere Informationen zur Diagnose und Behebung dieses Problems finden Sie unter [Anpassen von Einschränkungen der VPN-Paketgröße](troubleshoot-nas.md#adjust-vpn-packet-size-restrictions) im Artikel zur Problembehandlung bei NAS.

Weitere Informationen zu den MTU-Einstellungen in virtuellen Azure-Netzwerken finden Sie unter [Optimierung der TCP/IP-Leistung für Azure-VMs](../virtual-network/virtual-network-tcpip-performance-tuning.md).

## <a name="configure-root-squash"></a>Konfigurieren von Root-Squash
<!-- linked from troubleshoot and from access policies -->

Die Einstellung **Root-Squash aktivieren** steuert, wie Azure HPC Cache Anforderungen des Stammbenutzers auf Clientcomputern verarbeitet.

Wenn der Root-Squash aktiviert ist, werden Stammbenutzer auf einem Client automatisch dem Benutzer „Nobody“ (niemand) zugeordnet, wenn sie Anforderungen über Azure HPC Cache senden. Die Einstellung verhindert außerdem, dass Clientanforderungen set-UID-Berechtigungsbits verwenden.

Bei deaktiviertem Root-Squash werden Anforderungen vom Stammbenutzer des Clients (UID 0) als Stamm an ein Back-End-NFS-Speichersystem übergeben. Diese Konfiguration kann einen unangemessenen Dateizugriff ermöglichen.

Wenn der Root-Squash für den Cache festgelegt wird, kann diese Einstellung dazu beitragen, die erforderliche Einstellung ``no_root_squash`` in NAS-Systemen auszugleichen, die als Speicherziele verwendet werden. (Weitere Informationen zu [Voraussetzungen für NFS-Speicherziele](hpc-cache-prerequisites.md#nfs-storage-requirements).) Außerdem kann dadurch die Sicherheit verbessert werden, wenn sie mit Azure-Blobspeicherzielen verwendet wird.

Die Standardeinstellung ist **Ja**. (Caches, die vor April 2020 erstellt wurden, haben die Standardeinstellung **Nein**.)

> [!TIP]
> Sie können auch Root-Squash für bestimmte Speicherexporte festlegen, indem Sie [Clientzugriffsrichtlinien](access-policies.md#root-squash) anpassen.

## <a name="view-snapshots-for-blob-storage-targets"></a>Anzeigen von Momentaufnahmen für Blobspeicherziele

Azure HPC Cache speichert automatisch Speichermomentaufnahmen für Azure-Blobspeicherziele. Momentaufnahmen bieten einen schnellen Bezugspunkt für den Inhalt des Back-End-Speichercontainers.

Momentaufnahmen sind kein Ersatz für Datensicherungen und enthalten keine Informationen zum Zustand der zwischengespeicherten Daten.

> [!NOTE]
> Diese Momentaufnahmefunktion unterscheidet sich von der Momentaufnahmenfunktion, die in NetApp- oder Isilon-Speichersoftware enthalten ist. Diese Momentaufnahmeimplementierungen leeren Änderungen aus dem Cache in das Back-End-Speichersystem, bevor die Momentaufnahme erstellt wird.
>
> Aus Effizienzgründen leert die Azure HPC Cache-Momentaufnahme nicht zuerst Änderungen, sondern zeichnet nur Daten auf, die in den Blob-Container geschrieben wurden. Diese Momentaufnahme stellt nicht den Zustand von zwischengespeicherten Daten dar, weshalb aktuelle Änderungen möglicherweise nicht enthalten sind.

Diese Funktion ist nur für Azure-Blobspeicherziele verfügbar, und Ihre Konfiguration kann nicht geändert werden.

Momentaufnahmen werden alle acht Stunden (UTC 0:00, 08:00 und 16:00) erstellt.

Azure HPC Cache speichert tägliche, wöchentliche und monatliche Momentaufnahmen, bis sie durch neue ersetzt werden. Die Limits sind:

* bis zu 20 tägliche Momentaufnahmen
* bis zu 8 wöchentliche Momentaufnahmen
* bis zu 3 monatliche Momentaufnahmen

Der Zugriff auf die Momentaufnahmen erfolgt über das `.snapshot`-Verzeichnis im Namespace Ihres Blobspeicherziels.
