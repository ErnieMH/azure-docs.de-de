---
title: Verwaltetes virtuelles Netzwerk und verwaltete private Endpunkte
description: Erfahren Sie mehr über das verwaltete virtuelle Netzwerk und verwaltete private Endpunkte in Azure Data Factory.
ms.author: abnarain
author: nabhishek
ms.service: data-factory
ms.topic: conceptual
ms.custom:
- seo-lt-2019
- references_regions
ms.date: 07/15/2020
ms.openlocfilehash: 34211feb34b3e2ec21b9bc0e6a180df0febd8c76
ms.sourcegitcommit: aba63ab15a1a10f6456c16cd382952df4fd7c3ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107987936"
---
# <a name="azure-data-factory-managed-virtual-network-preview"></a>Verwaltetes virtuelles Netzwerk in Azure Data Factory (Vorschauversion)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

In diesem Artikel werden das verwaltete virtuelle Netzwerk und verwaltete private Endpunkte in Azure Data Factory erläutert.


## <a name="managed-virtual-network"></a>Verwaltetes virtuelles Netzwerk

Wenn Sie eine Azure Integration Runtime in einem verwalteten virtuellen Azure Data Factory-Netzwerk (VNET) erstellen, wird die Integration Runtime mit dem verwalteten virtuellen Netzwerk bereitgestellt und nutzt private Endpunkte, um sichere Verbindungen mit unterstützten Datenspeichern herzustellen. 

Durch das Erstellen einer Azure IR in einem verwaltetem virtuellen Netzwerk wird sichergestellt, dass der Datenintegrationsprozess isoliert und sicher ist. 

Vorteile der Verwendung eines verwalteten virtuellen Netzwerks:

- Mit einem verwalteten virtuellen Netzwerk können Sie die Verwaltung des virtuellen Netzwerks an Azure Data Factory auslagern. Es ist nicht erforderlich, ein Subnetz für die Azure Integration Runtime zu erstellen, das letztendlich viele private IP-Adressen Ihres virtuellen Netzwerks verwenden könnte, sodass eine vorherige Planung der Netzwerkinfrastruktur erforderlich wäre. 
- Für sichere Datenintegrationen sind keine tief greifenden Azure-Netzwerkkenntnisse erforderlich. Die ersten Schritte mit sicherem ETL sind für Datentechniker im Vergleich viel einfacher. 
- Das verwaltete virtuelle Netzwerk schützt zusammen mit verwalteten privaten Endpunkten vor Datenexfiltration. 

> [!IMPORTANT]
>Derzeit wird das verwaltete VNET nur in derselben Region wie Azure Data Factory unterstützt.
 

![Architektur des verwalteten virtuellen ADF-Netzwerks](./media/managed-vnet/managed-vnet-architecture-diagram.png)

## <a name="managed-private-endpoints"></a>Verwaltete private Endpunkte

Verwaltete private Endpunkte sind private Endpunkte, die im verwalteten virtuellen Netzwerk von Azure Data Factory erstellt werden und einen privaten Link zu Azure-Ressourcen herstellen. Azure Data Factory verwaltet diese privaten Endpunkte für Sie. 

![Neuer verwalteter privater Endpunkt](./media/tutorial-copy-data-portal-private/new-managed-private-endpoint.png)

Azure Data Factory unterstützt private Links. Der private Link ermöglicht Ihnen den Zugriff auf Azure-Dienste (PaaS, z. B. Azure Storage, Azure Cosmos DB, Azure Synapse Analytics).

Bei der Verwendung eines privaten Links wird der Datenverkehr zwischen Ihrem Datenspeicher und dem verwalteten virtuellen Netzwerk vollständig über das Microsoft-Backbone-Netzwerk übertragen. Private Link schützt vor einer möglichen Datenexfiltration. Ein privater Link zu einer Ressource wird durch die Erstellung eines privaten Endpunkts eingerichtet.

Der private Endpunkt nutzt eine private IP-Adresse im verwalteten virtuellen Netzwerk, um den Dienst effektiv darin einzubinden. Private Endpunkte werden einer bestimmten Ressource in Azure und nicht dem gesamten Dienst zugeordnet. Kunden können die Konnektivität auf eine bestimmte Ressource beschränken, die von Ihrer Organisation genehmigt wurde. Weitere Informationen finden Sie in der [Dokumentation zu Private Link](../private-link/index.yml).

> [!NOTE]
> Es wird empfohlen, verwaltete private Endpunkte zu erstellen, um eine Verbindung mit all Ihren Azure-Datenquellen herzustellen. 
 
> [!WARNING]
> Wenn für einen PaaS-Datenspeicher (Blob, ADLS Gen2, Azure Synapse Analytics) bereits ein privater Endpunkt erstellt wurde, kann ADF nur mithilfe des verwalteten privaten Endpunkts darauf zugreifen, selbst wenn er den Zugriff aus allen Netzwerken zulässt. Stellen Sie in solchen Szenarien sicher, einen privaten Endpunkt zu erstellen. 

Wenn Sie einen verwalteten privaten Endpunkt in Azure Data Factory erstellen, wird eine Verbindung mit einem privaten Endpunkt mit dem Status „Ausstehend“ erstellt. Ein Genehmigungsworkflow wird ausgelöst. Der Besitzer der Ressource für private Links ist für die Genehmigung oder Ablehnung der Verbindung verantwortlich.

![Verwalten eines privaten Endpunkts](./media/tutorial-copy-data-portal-private/manage-private-endpoint.png)

Wenn der Besitzer die Verbindung genehmigt, wird der private Link eingerichtet. Andernfalls wird der private Link nicht eingerichtet. In beiden Fällen wird der verwaltete private Endpunkt mit dem Status der Verbindung aktualisiert.

![Genehmigter verwalteter privater Endpunkt](./media/tutorial-copy-data-portal-private/approve-private-endpoint.png)

Nur ein verwalteter privater Endpunkt im genehmigten Zustand kann Datenverkehr an eine angegebene Ressource für private Links senden.

## <a name="interactive-authoring"></a>Interaktive Erstellung
Interaktive Erstellungsfunktionen werden beispielsweise für Testverbindungen, das Durchsuchen von Ordner- und Tabellenlisten, das Abrufen von Schemas und die Vorschau von Daten verwendet. Sie können die interaktive Erstellung aktivieren, wenn Sie eine Azure Integration Runtime erstellen, die sich in einem per ADF verwalteten virtuellen Netzwerk befindet. Der Back-End-Dienst weist vorab Computeressourcen für die interaktive Erstellung zu. Andernfalls werden Computeressourcen jedes Mal zugewiesen, wenn eine interaktive ausgeführt wird. Dies nimmt mehr Zeit in Anspruch. Die Gültigkeitsdauer für die interaktive Erstellung beträgt 60 Minuten. Das bedeutet, dass das Feature 60 Minuten nach dem letzten interaktiven Erstellungsvorgang deaktiviert wird.

![Interaktive Erstellung](./media/managed-vnet/interactive-authoring.png)

## <a name="create-managed-virtual-network-via-azure-powershell"></a>Erstellen eines verwalteten virtuellen Netzwerks über Azure PowerShell
```powershell
$subscriptionId = ""
$resourceGroupName = ""
$factoryName = ""
$managedPrivateEndpointName = ""
$integrationRuntimeName = ""
$apiVersion = "2018-06-01"
$privateLinkResourceId = ""

$vnetResourceId = "subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.DataFactory/factories/${factoryName}/managedVirtualNetworks/default"
$privateEndpointResourceId = "subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.DataFactory/factories/${factoryName}/managedVirtualNetworks/default/managedprivateendpoints/${managedPrivateEndpointName}"
$integrationRuntimeResourceId = "subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.DataFactory/factories/${factoryName}/integrationRuntimes/${integrationRuntimeName}"

# Create managed Virtual Network resource
New-AzResource -ApiVersion "${apiVersion}" -ResourceId "${vnetResourceId}"

# Create managed private endpoint resource
New-AzResource -ApiVersion "${apiVersion}" -ResourceId "${privateEndpointResourceId}" -Properties @{
        privateLinkResourceId = "${privateLinkResourceId}"
        groupId = "blob"
    }

# Create integration runtime resource enabled with VNET
New-AzResource -ApiVersion "${apiVersion}" -ResourceId "${integrationRuntimeResourceId}" -Properties @{
        type = "Managed"
        typeProperties = @{
            computeProperties = @{
                location = "AutoResolve"
                dataFlowProperties = @{
                    computeType = "General"
                    coreCount = 8
                    timeToLive = 0
                }
            }
        }
        managedVirtualNetwork = @{
            type = "ManagedVirtualNetworkReference"
            referenceName = "default"
        }
    }

```

## <a name="limitations-and-known-issues"></a>Einschränkungen und bekannte Probleme
### <a name="supported-data-sources"></a>Unterstützte Datenquellen
Die nachstehenden Datenquellen werden für eine Verbindung über einen privaten Link aus einem verwalteten virtuellen ADF-Netzwerk unterstützt.
- Azure Blob Storage (ohne Speicherkonto V1)
- Azure Table Storage (ohne Speicherkonto V1)
- Azure Files (ohne Speicherkonto V1)
- Azure Data Lake Gen2
- Azure SQL-Datenbank (ohne Azure SQL Managed Instance)
- Azure Synapse Analytics
- Azure Cosmos DB SQL
- Azure-Schlüsseltresor
- Azure Private Link-Dienst
- Azure Search
- Azure Database for MySQL
- Azure Database for PostgreSQL
- Azure Database for MariaDB

### <a name="azure-data-factory-managed-virtual-network-is-available-in-the-following-azure-regions"></a>Verwaltetes virtuelles Netzwerk in Azure Data Factory ist in den folgenden Azure-Regionen verfügbar:
- Australien (Osten)
- Australien, Südosten
- Brasilien Süd
- Kanada, Mitte
- Kanada, Osten
- Indien, Mitte
- USA (Mitte)
- East US
- USA (Ost 2)
- Frankreich, Mitte
- Japan, Osten
- Japan, Westen
- Korea, Mitte
- Nordeuropa
- Norwegen, Osten
- Südafrika, Norden
- USA Süd Mitte
- Südostasien
- Schweiz, Norden
- Vereinigte Arabische Emirate, Norden
- UK, Süden
- UK, Westen
- USA, Westen-Mitte
- Europa, Westen
- USA (Westen)
- USA, Westen 2


### <a name="outbound-communications-through-public-endpoint-from-adf-managed-virtual-network"></a>Ausgehende Kommunikation über einen öffentlichen Endpunkt von einem verwaltetem virtuellen ADF-Netzwerk
- Alle Ports sind für die ausgehende Kommunikation geöffnet.
- Azure Storage und Azure Data Lake Gen2 werden für eine Verbindung über einen öffentlichen Endpunkt von einem verwalteten virtuellen ADF-Netzwerk nicht unterstützt.

### <a name="linked-service-creation-of-azure-key-vault"></a>Erstellung eines verknüpften Diensts von Azure Key Vault 
- Wenn Sie einen verknüpften Dienst für Azure Key Vault erstellen, gibt es keinen Verweis auf Azure Integration Runtime. Daher können Sie während der Erstellung eines verknüpften Diensts von Azure Key Vault keinen privaten Endpunkt erstellen. Wenn Sie jedoch einen verknüpften Dienst für Datenspeicher erstellen, der auf den verknüpften Dienst von Azure Key Vault verweist, und dieser verknüpfte Dienst auf Azure Integration Runtime mit aktiviertem verwalteten virtuellen Netzwerk verweist, können Sie während der Erstellung einen privaten Endpunkt für den verknüpften Dienst von Azure Key Vault erstellen. 
- Bei dem Vorgang **Verbindung testen** für den verknüpften Dienst von Azure Key Vault wird nur das URL-Format überprüft. Es werden dabei jedoch keine Netzwerkvorgänge ausgeführt.
- Die Spalte **Privater Endpunkt wird verwendet** ist immer leer, selbst wenn Sie einen privaten Endpunkt für Azure Key Vault erstellen.
![Privater Endpunkt für Azure Key Vault](./media/managed-vnet/akv-pe.png)

## <a name="next-steps"></a>Nächste Schritte

- Tutorial: [Erstellen einer Kopierpipeline mithilfe eines verwalteten virtuellen Netzwerks und privater Endpunkte](tutorial-copy-data-portal-private.md) 
- Tutorial: [Erstellen einer Pipeline für den Zuordnungsdatenfluss mithilfe eines verwalteten virtuellen Netzwerks und privater Endpunkte](tutorial-data-flow-private.md)