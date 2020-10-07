---
title: 'Schnellstart: Erstellen einer georeplizierten Registrierung – Azure Resource Manager-Vorlage'
description: Erfahren Sie, wie Sie mit einer Azure Resource Manager-Vorlage eine georeplizierte Azure Container Registry-Instanz erstellen.
services: azure-resource-manager
author: dlepow
ms.service: azure-resource-manager
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: danlep
ms.date: 05/26/2020
ms.openlocfilehash: 2cfce37ff63a8321f40843ced2a7b786bcfc013e
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "88649600"
---
# <a name="quickstart-create-a-geo-replicated-container-registry-by-using-an-arm-template"></a>Schnellstart: Erstellen einer georeplizierten Containerregistrierung mithilfe einer ARM-Vorlage

In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe einer Azure Resource Manager-Vorlage (ARM-Vorlage) eine Azure Container Registry-Instanz erstellen. Die Vorlage richtet eine [georeplizierte](container-registry-geo-replication.md) Registrierung ein, die automatisch Registrierungsinhalte über mehr als eine Azure-Region synchronisiert. Georeplikation ermöglicht den netzwerknahen Zugriff auf Images aus regionalen Bereitstellungen, bei gleichzeitiger Bereitstellung einer einzigen Verwaltungserfahrung. Es handelt sich um eine Funktion des Registrierungstarifs [Premium](container-registry-skus.md).

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Wenn Ihre Umgebung die Voraussetzungen erfüllt und Sie mit der Verwendung von ARM-Vorlagen vertraut sind, klicken Sie auf die Schaltfläche **In Azure bereitstellen**. Die Vorlage wird im Azure-Portal geöffnet.

[![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-container-registry-geo-replication%2Fazuredeploy.json)

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="review-the-template"></a>Überprüfen der Vorlage

Die in dieser Schnellstartanleitung verwendete Vorlage stammt von der Seite mit den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-container-registry-geo-replication/). Die Vorlage richtet eine Registrierung und ein zusätzliches regionales Replikat ein.

:::code language="json" source="~/quickstart-templates/101-container-registry-geo-replication/azuredeploy.json":::

Die folgenden Ressourcen sind in der Vorlage definiert:

* **[Microsoft.ContainerRegistry/registries](/azure/templates/microsoft.containerregistry/registries)** : Erstellen einer Azure Container Registry-Instanz.
* **[Microsoft.ContainerRegistry/registries/replications](/azure/templates/microsoft.containerregistry/registries/replications)** : Erstellen eines Containerregistrierungsreplikats.

Weitere Beispiele für Azure Container Registry-Vorlagen finden Sie im [Schnellstartvorlagenkatalog](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Containerregistry&pageNumber=1&sort=Popular).

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

 1. Klicken Sie auf das folgende Bild, um sich bei Azure anzumelden und eine Vorlage zu öffnen.

    [![In Azure bereitstellen](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-container-registry-geo-replication%2Fazuredeploy.json)

 2. Wählen Sie die folgenden Werte aus, bzw. geben Sie sie ein.

    * **Abonnement**: Wählen Sie ein Azure-Abonnement aus.
    * **Ressourcengruppe**: Wählen Sie die Option **Neu erstellen** aus, geben Sie einen eindeutigen Namen für die Ressourcengruppe ein, und wählen Sie dann **OK** aus.
    * **Standort**: Wählen Sie einen Standort für die Ressourcengruppe aus. Beispiel: **USA, Mitte**
    * **ACR-Name**: Übernehmen Sie den generierten Namen für die Registrierung, oder geben Sie einen Namen ein. Der Wert muss global eindeutig sein.
    * **Standort**: Akzeptieren Sie den generierten Standort für das Stammreplikat der Registrierung, oder geben Sie einen Standort ein, z. B. **USA, Mitte**. 
    * **ACR-Replikatstandort**: Geben Sie einen Standort für das Registrierungsreplikat an, indem Sie den Kurznamen der Region verwenden. Er muss von dem des Standorts der Stammregistrierung abweichen. Beispiel: **Europa, Westen (westeurope)** .
    * **Ich stimme den oben genannten Geschäftsbedingungen zu**: Aktivieren Sie dieses Kontrollkästchen.

        :::image type="content" source="media/container-registry-get-started-geo-replication-template/template-properties.png" alt-text="Vorlageneigenschaften":::

 3. Wenn Sie die Bedingungen akzeptieren, wählen Sie **Kaufen** aus. Nach der erfolgreichen Erstellung der Registrierung erhalten Sie eine Benachrichtigung:

     :::image type="content" source="media/container-registry-get-started-geo-replication-template/deployment-notification.png" alt-text="Vorlageneigenschaften":::

 Zum Bereitstellen der Vorlage wird das Azure-Portal verwendet. Neben dem Azure-Portal können Sie Azure PowerShell, die Azure-Befehlszeilenschnittstelle (Azure CLI) und die REST-API verwenden. Informationen zu anderen Bereitstellungsmethoden finden Sie unter [Bereitstellen von Vorlagen](../azure-resource-manager/templates/deploy-cli.md).

## <a name="review-deployed-resources"></a>Überprüfen der bereitgestellten Ressourcen

Verwenden Sie das Azure-Portal oder ein Tool wie die Azure CLI, um die Eigenschaften der Containerregistrierung zu überprüfen.

1. Suchen Sie im Portal nach „Containerregistrierungen“, und wählen Sie die von Ihnen erstellte Containerregistrierung aus.

1. Notieren Sie sich auf der Seite **Übersicht** den **Anmeldeserver** der Registrierung. Verwenden Sie diesen URI, wenn Sie Docker verwenden, um Images zu markieren und in Ihre Registrierung zu pushen. Informationen finden Sie unter [Pushen Ihres ersten Images mit der Docker CLI](container-registry-get-started-docker-cli.md).

    :::image type="content" source="media/container-registry-get-started-geo-replication-template/registry-overview.png" alt-text="Vorlageneigenschaften":::

1. Bestätigen Sie auf der Seite **Replikationen** die Standorte des Stammreplikats und des über die Vorlage hinzugefügten Replikats. Fügen Sie bei Bedarf weitere Replikate auf dieser Seite hinzu.

    :::image type="content" source="media/container-registry-get-started-geo-replication-template/registry-replications.png" alt-text="Vorlageneigenschaften":::

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können die Ressourcengruppe, die Registrierung und das Registrierungsreplikat löschen, wenn Sie sie nicht mehr benötigen. Wechseln Sie hierzu zum Azure-Portal, wählen Sie die Ressourcengruppe aus, die die Registrierung enthält, und wählen Sie anschließend **Ressourcengruppe löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie eine Azure Container Registry-Instanz mithilfe einer ARM-Vorlage erstellt sowie an einem anderen Standort ein Registrierungsreplikat konfiguriert. Fahren Sie mit den Azure Container Registry-Tutorials fort, um eingehendere Informationen zu ACR zu erhalten.

> [!div class="nextstepaction"]
> [Tutorials zu Azure Container Registry](container-registry-tutorial-prepare-registry.md)

Ein Schritt-für-Schritt-Tutorial mit Anleitungen zum Erstellen einer Vorlage finden Sie unter folgendem Link:

> [!div class="nextstepaction"]
> [Tutorial: Erstellen und Bereitstellen Ihrer ersten ARM-Vorlage](../azure-resource-manager/templates/template-tutorial-create-first-template.md)
