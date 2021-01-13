---
title: 'Verwalten der Aufzeichnungsrichtlinie: Azure'
description: In diesem Thema wird das Verwalten einer Aufzeichnungsrichtlinie erläutert.
ms.topic: how-to
ms.date: 04/27/2020
ms.openlocfilehash: d3a1be915dc1cc8714e49cc7b2fe68bbe9cad161
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87011480"
---
# <a name="manage-recording-policy"></a>Verwalten der Aufzeichnungsrichtlinie

Sie können Live Video Analytics in IoT Edge für die [fortlaufende Videoaufzeichnung](continuous-video-recording-concept.md) verwenden. Die Videoaufzeichnungen können Sie für Wochen oder Monate in der Cloud speichern. Sie können die Dauer (in Tagen) dieses Cloudarchivs mithilfe der in Azure Storage integrierten [Tools zur Lebenszyklusverwaltung](../../storage/blobs/storage-lifecycle-management-concepts.md?tabs=azure-portal) verwalten.  

Ihr Media Service-Konto ist mit einem Azure Storage-Konto verknüpft. Wenn Sie Videos in der Cloud aufzeichnen, wird der Inhalt in ein [Medienobjekt](../latest/assets-concept.md) in Media Services geschrieben. Jedes Medienobjekt wird einem Container im Speicherkonto zugeordnet. Mit der Lebenszyklusverwaltung können Sie eine [Richtlinie](../../storage/blobs/storage-lifecycle-management-concepts.md?tabs=azure-portal#policy) für ein Speicherkonto definieren, in der Sie eine [Regel](../../storage/blobs/storage-lifecycle-management-concepts.md?tabs=azure-portal#rules) wie die folgende angeben können.

```
{
  "rules": [
    {
      "name": "NinetyDayRule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "delete": { "daysAfterModificationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}
```

Für die obige Regel gilt Folgendes:

* Sie gilt für alle Blockblobs im Speicherkonto.
* Sie gibt an, dass die Blobs, die älter als 30 Tage sind, von der [heißen in die kalte Speicherebene](../../storage/blobs/storage-blob-storage-tiers.md?tabs=azure-portal) verschoben werden.
* Wenn Blobs älter als 90 Tage sind, müssen sie gelöscht werden.

Da Ihre Videos in Live Video Analytics in festgelegten Zeiteinheiten archiviert werden, enthält Ihr Medienobjekt eine Reihe von Blobs (jeweils ein Blob pro Segment). Wenn die Richtlinie zur Lebenszyklusverwaltung aktiv wird und ältere Blobs löscht, können Sie weiterhin über die Media Services-APIs auf die verbleibenden Blobs zugreifen und sie wiedergeben. Weitere Informationen finden Sie unter [Wiedergeben von Aufzeichnungen](playback-recordings-how-to.md). 

## <a name="limitations"></a>Einschränkungen

Im Folgenden finden Sie einige bekannte Einschränkungen bei der Lebenszyklusverwaltung:

* In der Richtlinie können maximal 100 Regeln festgelegt werden, und in jeder Regel können bis zu 10 Container angegeben werden. Wenn Sie also unterschiedliche Aufzeichnungsrichtlinien benötigen (z. B. ein Archiv von 3 Tagen für die Kamera auf dem Parkplatz, eines von 30 Tagen für die Kamera im Ladebereich und eines von 180 Tagen für die Kamera hinter der Kasse), können Sie mit einem Media Services-Konto die Regeln für maximal 1.000 Kameras anpassen.
* Updates für die Richtlinie zur Lebenszyklusverwaltung werden nicht sofort angewandt. Weitere Einzelheiten finden Sie in [diesem Abschnitt der häufig gestellten Fragen (FAQ)](../../storage/blobs/storage-lifecycle-management-concepts.md?tabs=azure-portal#faq).
* Wenn Sie eine Richtlinie anwenden, mit der Blobs auf die kalte Speicherebene verschoben werden, hat dies möglicherweise Einfluss auf die Wiedergabe dieses Teils des Archivs. Es können zusätzliche Wartezeiten oder sporadische Fehler auftreten. Die Wiedergabe von Inhalten auf Archivspeicherebene wird von Media Services nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

[Wiedergeben von Aufzeichnungen](playback-recordings-how-to.md)
