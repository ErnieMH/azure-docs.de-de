---
ms.openlocfilehash: 673fd1efde5c60600a0f802637088068a063b2c4
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110385387"
---
1. Rufen Sie die IP-Adresse Ihres lokalen Computers ab. Dieser Schritt ist erforderlich, wenn die Einstellung für den direkten Modus mithilfe von Cosmos DB-SDKs (.NET, Java) konfiguriert wird.

   ```bash
   ipaddr="`ifconfig | grep "inet " | grep -Fv 127.0.0.1 | awk '{print $2}' | head -n 1`"
   ```

1. Pullen Sie das Docker-Image aus der Registrierung.

   ```bash
   docker pull mcr.microsoft.com/cosmosdb/linux/azure-cosmos-emulator
   ```

1. Führen Sie das Docker-Image mit den folgenden Konfigurationen aus:

   ```bash
   docker run -p 8081:8081 -p 10251:10251 -p 10252:10252 -p 10253:10253 -p 10254:10254  -m 3g --cpus=2.0 --name=test-linux-emulator -e AZURE_COSMOS_EMULATOR_PARTITION_COUNT=10 -e AZURE_COSMOS_EMULATOR_ENABLE_DATA_PERSISTENCE=true -e AZURE_COSMOS_EMULATOR_IP_ADDRESS_OVERRIDE=$ipaddr -it mcr.microsoft.com/cosmosdb/linux/azure-cosmos-emulator
   ```