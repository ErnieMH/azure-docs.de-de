---
title: 'Azure-Portal: Bereitstellen des Open-Source-FHIR-Servers für Azure – Azure API for FHIR'
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie den Microsoft Open-Source-FHIR-Server über das Azure-Portal bereitstellen.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: matjazl
ms.openlocfilehash: 57ab6bca820c4c25a9a56e4a801aa7d917d317ec
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90978570"
---
# <a name="quickstart-deploy-open-source-fhir-server-using-azure-portal"></a>Schnellstart: Bereitstellen des Open-Source-FHIR-Servers über das Azure-Portal

In dieser Schnellstartanleitung erfahren Sie, wie Sie einen Open-Source-FHIR-Server in Azure über das Azure-Portal bereitstellen. Hierzu werden Links für die einfache Bereitstellung im [Open-Source-Repository](https://github.com/Microsoft/fhir-server) verwendet.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="github-open-source-repository"></a>Open-Source-Repository auf GitHub

Navigieren Sie zur [Bereitstellungsseite auf GitHub](https://github.com/Microsoft/fhir-server/blob/master/docs/DefaultDeployment.md), und suchen Sie die Schaltfläche „Bereitstellung in Azure“:

>[!div class="mx-imgBorder"]
>![Open-Source-Bereitstellungsseite](media/quickstart-oss-portal/deployment-page-oss.png)

Klicken Sie auf die Schaltfläche für die Bereitstellung, und das Azure-Portal wird geöffnet.

## <a name="fill-in-deployment-parameters"></a>Ausfüllen von Bereitstellungsparametern

Wählen Sie das Erstellen eine neuen Ressourcengruppe aus, und geben Sie ihr einen Namen. Als weitere Parameter werden nur noch ein Name für den Dienst und das SQL-Administratorkennwort benötigt.

>[!div class="mx-imgBorder"]
>![Benutzerdefinierte Bereitstellungsparameter](media/quickstart-oss-portal/deployment-custom-parameters.png)

Nachdem Sie die Details ausgefüllt haben, können Sie die Bereitstellung starten.

## <a name="validate-fhir-server-is-running"></a>Überprüfen, ob der FHIR-Server ausgeführt wird

Sobald die Bereitstellung abgeschlossen ist, können Sie in Ihrem Browser zu `https://SERVICENAME.azurewebsites.net/metadata` navigieren, um eine Funktionsbestätigung abzurufen. Es dauert etwa eine Minute, bis der Server zum ersten Mal antwortet.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn die Ressourcengruppe und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie löschen. Wählen Sie dazu die Ressourcengruppe mit den bereitgestellten Ressourcen und anschließend **Ressourcengruppe löschen** aus. Dann bestätigen Sie den Namen der zu löschenden Ressourcengruppe.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie den Microsoft Open-Source-FHIR-Server für Azure in Ihrem Abonnement bereitgestellt. Informationen zum Zugriff auf die FHIR-API mit Postman finden Sie im Tutorial zu Postman.
 
>[!div class="nextstepaction"]
>[Zugriff auf die FHIR-API mit Postman](access-fhir-postman-tutorial.md)