---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/17/2020
ms.author: inhenkel
ms.custom: CLI
ms.openlocfilehash: aa76f7b85302651f6874747610c3355f0572a7ee
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88556153"
---
<!-- List and set subscriptions -->

1. Rufen Sie eine Liste Ihrer Abonnements mithilfe des Befehls [az account list](/cli/azure/account#az-account-list) ab.

    ```
    az account list --output table
    ```

2. Verwenden Sie `az account set` mit der ID oder dem Namen des Abonnements, zu dem Sie wechseln möchten.

    ```
    az account set --subscription "My Demos"
    ```
