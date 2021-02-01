---
title: Skalierbarkeits- und Leistungsziele für Azure Files
description: Informationen zu den Skalierbarkeits- und Leistungszielen für Azure Files, einschließlich Kapazität, Anforderungsrate sowie Grenzwerte für ein- und ausgehende Bandbreite.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 10/16/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: e10f45af89e19f6fe62ff729f96d870e008c96ec
ms.sourcegitcommit: 8a74ab1beba4522367aef8cb39c92c1147d5ec13
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/20/2021
ms.locfileid: "98611099"
---
# <a name="azure-files-scalability-and-performance-targets"></a>Skalierbarkeits- und Leistungsziele für Azure Files

[Azure Files](storage-files-introduction.md) bietet vollständig verwaltete Dateifreigaben in der Cloud, auf die über das Branchenstandardprotokoll SMB zugegriffen werden kann. Dieser Artikel behandelt die Skalierbarkeits- und Leistungsziele für Azure Files und Azure-Dateisynchronisierung.

Die hier aufgelisteten Ziele für Skalierbarkeit und Leistung sind hochgesteckt, können jedoch von anderen Variablen in Ihrer Bereitstellung beeinflusst werden. Beispielsweise kann der Durchsatz für eine Datei auch durch die verfügbare Netzwerkbandbreite und nicht nur durch die Server begrenzt sein, die den Azure Files-Dienst hosten. Wir empfehlen Ihnen ausdrücklich, Ihr Nutzungsverhalten zu testen, um festzustellen, ob Skalierbarkeit und Leistung von Azure Files Ihren Anforderungen entspricht. Wir verpflichten uns auch, diese Grenzwerte mit der Zeit zu erhöhen. Zögern Sie nicht, uns entweder in den Kommentaren unten oder über [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files) Feedback dazu zu geben, welche Grenzwerte wir für Sie erhöhen sollen.

## <a name="azure-storage-account-scale-targets"></a>Skalierbarkeitsziele für das Azure-Speicherkonto

Die übergeordnete Ressource einer Azure-Dateifreigabe ist ein Azure-Speicherkonto. Ein Speicherkonto stellt einen Speicherpool in Azure dar, der von mehreren Speicherdiensten, einschließlich Azure Files, zum Speichern von Daten verwendet werden kann. Andere Dienste, die Daten in Speicherkonten speichern, sind Azure Blob Storage, Azure Queue Storage und Azure Table Storage. Die folgenden Ziele gelten für alle Speicherdienste, die Daten in einem Speicherkonto speichern:

[!INCLUDE [azure-storage-account-limits-standard](../../../includes/azure-storage-account-limits-standard.md)]

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

> [!Important]  
> Die Belegung von allgemeinen Speicherkonten durch andere Speicherdienste wirkt sich auf die Azure-Dateifreigaben in Ihrem Speicherkonto aus. Wenn Sie z.B. mit Azure Blob Storage die maximale Speicherkapazität erreichen, können Sie in Ihrer Azure-Dateifreigabe keine neuen Dateien erstellen, auch wenn Ihre Azure-Dateifreigabe kleiner als die maximale Freigabegröße ist.

## <a name="azure-files-scale-targets"></a>Skalierbarkeitsziele für Azure Files

Es gibt drei Kategorien von Einschränkungen, die für Azure Files zu berücksichtigen sind: Speicherkonten, Freigaben und Dateien.

Beispiel: Bei Premium-Dateifreigaben kann eine einzige Freigabe einen IOPS-Wert von 100.000 erreichen, und eine einzelne Datei kann bis zu 5.000 IOPS hochskaliert werden. Wenn Sie drei Dateien in einer Freigabe haben, entspricht der maximale IOPS-Wert aus dieser Freigabe 15.000.

### <a name="standard-storage-account-limits"></a>Begrenzungen beim Standard-Speicherkonto

Diese Grenzwerte finden Sie im Abschnitt [Skalierbarkeitsziele für das Azure-Speicherkonto](#azure-storage-account-scale-targets).

### <a name="premium-filestorage-account-limits"></a>Grenzwerte für FileStorage Premium-Konten

[!INCLUDE [azure-storage-limits-filestorage](../../../includes/azure-storage-limits-filestorage.md)]

> [!IMPORTANT]
> Die Einschränkungen für Speicherkonten gelten für alle Freigaben. Zentrales Hochskalieren bis zum Maximalwert für FileStorage-Konten ist nur möglich, wenn es nur eine Freigabe pro FileStorage-Konto gibt.

### <a name="file-share-and-file-scale-targets"></a>Skalierungsziele für Dateifreigaben und Dateien

> [!NOTE]
> Für Standarddateifreigaben über 5 TiB gelten bestimmte Einschränkungen. Eine Liste der Einschränkungen und Anweisungen dazu, wie Sie Dateifreigaben vergrößern, finden Sie im Abschnitt [Aktivieren größerer Dateifreigaben für Standarddateifreigaben](storage-files-planning.md#enable-standard-file-shares-to-span-up-to-100-tib) des Planungshandbuchs.

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

[!INCLUDE [storage-files-premium-scale-targets](../../../includes/storage-files-premium-scale-targets.md)]

## <a name="azure-file-sync-scale-targets"></a>Skalierbarkeitsziele für die Azure-Dateisynchronisierung

Azure-Dateisynchronisierung wurde mit dem Ziel der unbegrenzten Nutzung entwickelt, aber unbegrenzte Nutzung ist nicht immer möglich. Die folgende Tabelle gibt an, welche Grenzen für Tests von Microsoft gelten und welche Ziele feste Grenzwerte sind:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

### <a name="azure-file-sync-performance-metrics"></a>Leistungsmetriken der Azure-Dateisynchronisierung

Da der Azure-Dateisynchronisierungs-Agent auf einem Windows Server-Computer ausgeführt wird, der mit den Azure-Dateifreigaben verbunden wird, hängt die effektive Synchronisierungsleistung von einer Reihe von Faktoren in Ihrer Infrastruktur ab: von Windows Server und der zugrunde liegenden Datenträgerkonfiguration, der Netzwerkbandbreite zwischen dem Server und Azure Storage, der Dateigröße, der gesamten Datasetgröße und der Aktivität im Dataset. Da die Azure-Dateisynchronisierung auf Dateiebene ausgeführt wird, werden die Leistungsmerkmale einer auf der Azure-Dateisynchronisierung basierenden Lösung besser in der Anzahl von Objekten (Dateien und Verzeichnisse) gemessen, die pro Sekunde verarbeitet werden.

Bei der Azure-Dateisynchronisierung ist die Leistung in zwei Phasen entscheidend:

1. **Erste einmalige Bereitstellung**: Informationen zum Optimieren der Leistung bei der ersten Bereitstellung finden Sie unter [Onboarding bei der Azure-Dateisynchronisierung](storage-sync-files-deployment-guide.md#onboarding-with-azure-file-sync).
2. **Laufende Synchronisierung**: Nachdem zunächst ein Seeding für die Daten in den Azure-Dateifreigaben ausgeführt wird, synchronisiert die Azure-Dateisynchronisierung fortlaufend mehrere Endpunkte.

Wenn Sie die Bereitstellung für jede der Phasen planen, sehen Sie sich im Folgenden die Ergebnisse an, die bei den internen Tests auf einem System mit einer Konfiguration zustande kamen.

| Systemkonfiguration | Details |
|-|-|
| CPU | 64 virtuelle Kerne mit 64-MiB-L3-Cache |
| Arbeitsspeicher | 128 GB |
| Datenträger | SAS-Datenträger mit RAID 10 mit batteriegepuffertem Cache |
| Netzwerk | 1-GBit/s-Netzwerk |
| Workload | Allgemeiner Dateiserver|

| Erste einmalige Bereitstellung  | Details |
|-|-|
| Anzahl der Objekte | 25 Millionen Objekte |
| Datasetgröße| ca. 4,7 TiB |
| Durchschnittliche Dateigröße | ca. 200 KiB (größte Datei: 100 GiB) |
| Anfängliche Enumeration von Cloudänderungen | 20 Objekte pro Sekunde  |
| Uploaddurchsatz | 20 Objekte pro Sekunde pro Synchronisierungsgruppe |
| Durchsatz beim Download von Namespaces | 400 Objekte pro Sekunde |

### <a name="initial-one-time-provisioning"></a>Erste einmalige Bereitstellung

**Anfängliche Enumeration von Cloudänderungen**: Wenn eine neue Synchronisierungsgruppe erstellt wird, ist die anfängliche Enumeration von Cloudänderungen der erste durchgeführte Schritt. In diesem Prozess listet das System alle Elemente in der Azure-Dateifreigabe auf. Während dieses Prozesses findet keine Synchronisierungsaktivität statt. Es werden also keine Elemente vom Cloudendpunkt auf den Serverendpunkt heruntergeladen und keine Elemente vom Serverendpunkt auf den Cloudendpunkt hochgeladen. Die Synchronisierungsaktivität beginnt erst, wenn die anfängliche Enumeration von Cloudänderungen abgeschlossen ist.
Der Durchsatz liegt bei 20 Objekten pro Sekunde. Kunden können die Dauer der anfänglichen Enumeration von Cloudänderungen schätzen, indem sie die Anzahl von Elementen in der Cloudfreigabe bestimmen und den Zeitraum (in Tagen) anhand der folgenden Formel berechnen. 

   **Zeitraum (in Tagen) für die anfängliche Cloudenumeration = (Anzahl von Objekten am Cloudendpunkt) / (20 × 60 × 60 × 24)**

**Durchsatz beim Download von Namespaces**: Wenn einer vorhandenen Synchronisierungsgruppe ein neuer Serverendpunkt hinzugefügt wird, lädt der Azure-Dateisynchronisierungs-Agent keine Dateiinhalte vom Cloudendpunkt herunter. Zuerst synchronisiert er den vollständigen Namespace und löst dann im Hintergrund einen Rückruf aus, um die Dateien herunterzuladen, entweder in ihrer Gesamtheit oder bei aktiviertem Cloudtiering in der Cloudtieringrichtliniengruppe für den Serverendpunkt.


| Laufende Synchronisierung  | Details  |
|-|--|
| Anzahl der synchronisierten Objekte| 125.000 Objekte (Änderungsumfang ca. 1 %) |
| Datasetgröße| 50 GiB |
| Durchschnittliche Dateigröße | Ca. 500 KiB |
| Uploaddurchsatz | 20 Objekte pro Sekunde pro Synchronisierungsgruppe |
| Durchsatz bei vollständigen Downloads* | 60 Objekte pro Sekunde |

*Wenn Cloudtiering aktiviert ist, werden Sie wahrscheinlich eine bessere Leistung beobachten, da nur ein Teil der Dateidaten heruntergeladen wird. Die Azure-Dateisynchronisierung lädt die Daten zwischengespeicherter Dateien nur dann herunter, wenn sie auf einem der Endpunkte geändert werden. Bei mehrstufigen oder neu erstellten Dateien lädt der Agent nicht die Dateidaten herunter, sondern synchronisiert lediglich den Namespace auf allen Serverendpunkten. Der Agent unterstützt auch teilweise Downloads von mehrstufigen Dateien, wenn Benutzer auf diese zugreifen. 

> [!Note]  
> Die oben genannten Zahlen sind kein Hinweis auf die Leistung, die bei Ihnen zu erwarten ist. Die tatsächliche Leistung hängt von mehreren Faktoren ab, die am Anfang dieses Abschnitts beschrieben werden.

Als allgemeine Richtlinie für Ihre Bereitstellung sollten Sie einige Dinge berücksichtigen:

- Der Objektdurchsatz wird ungefähr relativ zur Anzahl der Synchronisierungsgruppen auf dem Server skaliert. Das Aufteilen von Daten in mehrere Synchronisierungsgruppen auf einem Server führt zu einem besseren Durchsatz, der zudem durch den Server und Netzwerk begrenzt wird.
- Der Objektdurchsatz ist umgekehrt proportional zum Durchsatz in MiB pro Sekunde. Bei kleineren Dateien tritt ein höherer Durchsatz im Hinblick auf die Anzahl der verarbeiteten Objekte pro Sekunde auf, jedoch einen niedrigeren Durchsatz in MiB pro Sekunde aufweist. Im Gegensatz dazu werden bei größeren Dateien weniger Objekte pro Sekunde verarbeitet, dafür jedoch ein höherer Durchsatz in MiB pro Sekunde erzielt. Der Durchsatz in MiB pro Sekunde wird durch die Azure Files-Skalierungsziele beschränkt.

## <a name="see-also"></a>Weitere Informationen

- [Planung für eine Azure Files-Bereitstellung](storage-files-planning.md)
- [Planung für die Bereitstellung einer Azure-Dateisynchronisierung](storage-sync-files-planning.md)
