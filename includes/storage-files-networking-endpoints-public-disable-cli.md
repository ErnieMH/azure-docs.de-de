---
title: include file
description: include file
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 6/2/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 195caa6f8d7c00c741741fbf80a77bd7fe5579b0
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "84465055"
---
Mit dem folgenden CLI-Befehl wird der gesamte Datenverkehr an den öffentlichen Endpunkt des Speicherkontos abgelehnt. Beachten Sie, dass für diesen Befehl der Parameter `-bypass` auf `AzureServices` festgelegt ist. Hierdurch können vertrauenswürdige Erstanbieterdienste, z. B. die Azure-Dateisynchronisierung, über den öffentlichen Endpunkt auf das Speicherkonto zugreifen.

```bash
# This assumes $storageAccountResourceGroupName and $storageAccountName 
# are still defined from the beginning of this guide.
az storage account update \
    --resource-group $storageAccountResourceGroupName \
    --name $storageAccountName \
    --bypass "AzureServices" \
    --default-action "Deny" \
    --output none
```