---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 07/20/2020
ms.author: msmbaldwin
ms.openlocfilehash: efa380ac243338a91354f948fdc77b2da8dd5406
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87512801"
---
Erstellen Sie ein Geheimnis mit dem Namen **mySecret** und dem Wert **Success!** . Bei einem Geheimnis kann es sich beispielsweise um ein Kennwort, eine SQL-Verbindungszeichenfolge oder um beliebige andere Informationen handeln, die sowohl sicher aufbewahrt werden als auch für Ihre Anwendung verfügbar sein müssen. 

Fügen Sie Ihrem neu erstellten Schlüsseltresor mit dem Azure CLI-Befehl [az keyvault secret set](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-set) ein Geheimnis hinzu.

```azurecli
az keyvault secret set --vault-name "<your-unique-keyvault-name>" --name "mySecret" --value "Success!"
```