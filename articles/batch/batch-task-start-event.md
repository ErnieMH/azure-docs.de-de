---
title: 'Azure Batch: Taskstartereignis'
description: Referenzinformationen zum Taskstartereignis in Batch. Dieses Ereignis wird einmal ausgegeben, nachdem ein Task vom Scheduler für den Start auf einem Computeknoten geplant wurde.
ms.topic: reference
ms.date: 10/08/2020
ms.openlocfilehash: 3a57ffbb1e1659cff54d101aa4b90ca1bd5d3a57
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91851015"
---
# <a name="task-start-event"></a>Taskstartereignis

 Dieses Ereignis wird einmal ausgegeben, nachdem ein Task vom Scheduler für den Start auf einem Computeknoten geplant wurde. Wenn dieser Task wiederholt oder erneut in die Warteschlange gestellt wird, wird dieses Ereignis erneut für denselben Task ausgegeben, doch der Wiederholungszähler und die Systemtaskversion werden entsprechend aktualisiert.


 Das folgende Beispiel zeigt den Text eines Taskstartereignisses.

```
{
    "jobId": "myJob",
    "id": "myTask",
    "taskType": "User",
    "systemTaskVersion": 220192842,
    "requiredSlots": 1,
    "nodeInfo": {
        "poolId": "pool-001",
        "nodeId": "tvm-257509324_1-20160908t162728z"
    },
    "multiInstanceSettings": {
        "numberOfInstances": 1
    },
    "constraints": {
        "maxTaskRetryCount": 2
    },
    "executionInfo": {
        "retryCount": 0
    }
}
```

|Elementname|type|Notizen|
|------------------|----------|-----------|
|`jobId`|String|Die ID des Auftrags, der den Task enthält.|
|`id`|String|Die ID des Tasks.|
|`taskType`|String|Der Typ des Tasks. Entweder „JobManager“, was bedeutet, dass dies ein Auftrags-Manager-Task ist, oder „User“, was bedeutet, dass dies nicht der Fall ist.|
|`systemTaskVersion`|Int32|Dies ist der interne Wiederholungszähler für einen Task. Der Batch-Dienst kann intern einen Task wiederholen, um vorübergehende Probleme zu berücksichtigen. Bei diesen Problemen kann es sich um interne Planungsfehler oder Versuche handeln, Computeknoten mit einem fehlerhaften Status wiederherzustellen.|
|`requiredSlots`|Int32|Die erforderlichen Slots zum Ausführen des Tasks|
|[`nodeInfo`](#nodeInfo)|Komplexer Typ|Enthält Informationen zu den Computeknoten, auf dem der Task ausgeführt wurde.|
|[`multiInstanceSettings`](#multiInstanceSettings)|Komplexer Typ|Gibt an, dass der Task ein Task mit mehreren Instanzen ist, für den mehrere Computeknoten erforderlich sind.  Details finden Sie unter [multiInstanceSettings](/rest/api/batchservice/get-information-about-a-task).|
|[`constraints`](#constraints)|Komplexer Typ|Die Ausführungseinschränkungen, die für diesen Task gelten.|
|[`executionInfo`](#executionInfo)|Komplexer Typ|Enthält Informationen zur Ausführung des Tasks.|

###  <a name="nodeinfo"></a><a name="nodeInfo"></a> nodeInfo

|Elementname|type|Notizen|
|------------------|----------|-----------|
|`poolId`|String|Die ID des Pools, auf den der Task angewendet wurde.|
|`nodeId`|Zeichenfolge|Die ID des Knotens, auf dem der Task ausgeführt wurde.|

###  <a name="multiinstancesettings"></a><a name="multiInstanceSettings"></a> multiInstanceSettings

|Elementname|type|Notizen|
|------------------|----------|-----------|
|`numberOfInstances`|Int|Die Anzahl der Computeknoten, die vom Task benötigt werden.|

###  <a name="constraints"></a><a name="constraints"></a> Einschränkungen

|Elementname|type|Notizen|
|------------------|----------|-----------|
|`maxTaskRetryCount`|Int32|Gibt an, wie oft der Task maximal wiederholt werden kann. Der Batch-Dienst wiederholt einen Task, wenn sein Exitcode ungleich null ist.<br /><br /> Beachten Sie, dass dieser Wert die Anzahl der Wiederholungen ausdrücklich steuert. Der Batch-Dienst wiederholt den Task einmal und kann ihn anschließend bis zu diesem Grenzwert wiederholen. Wenn beispielsweise die maximale Anzahl von Wiederholungsversuchen 3 ist, versucht der Batch-Dienst einen Task bis zu viermal (ein erster Versuch und drei Wiederholungsversuche).<br /><br /> Wenn die maximale Anzahl von Wiederholungsversuchen 0 ist, wiederholt der Batch-Dienst Tasks nicht.<br /><br /> Wenn die maximale Anzahl von Wiederholungsversuchen -1 ist, wiederholt der Batch-Dienst Tasks unbegrenzt.<br /><br /> Der Standardwert ist 0 (keine Wiederholungsversuche).|

###  <a name="executioninfo"></a><a name="executionInfo"></a> executionInfo

|Elementname|type|Notizen|
|------------------|----------|-----------|
|`retryCount`|Int32|Die Häufigkeit, mit der der Task vom Batch-Dienst wiederholt wurde. Der Vorgang wird wiederholt, wenn der Exitcode ungleich null ist, und zwar bis zum angegebenen Wert von „MaxTaskRetryCount“.|
