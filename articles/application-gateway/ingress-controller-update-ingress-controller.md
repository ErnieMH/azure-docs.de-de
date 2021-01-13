---
title: Upgraden des Eingangscontrollers mit Helm
description: Dieser Artikel enthält Informationen zum Upgraden eines Application Gateway-Eingangscontrollers mithilfe von Helm.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: how-to
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: f20302a4993da1754255254ce6d69c000750d4ab
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84806775"
---
# <a name="how-to-upgrade-application-gateway-ingress-controller-using-helm"></a>Upgraden des Application Gateway-Eingangscontrollers mithilfe von Helm 

Das Upgrade eines Azure Application Gateway-Eingangscontrollers für Kubernetes (AGIC) kann mithilfe eines Helm-Repositorys erfolgen, das in Azure Storage gehostet wird.

Bevor wir mit dem Upgradevorgang beginnen, müssen Sie sicherstellen, dass Sie das erforderliche Repository hinzugefügt haben:

- Zeigen Sie die derzeit hinzugefügten Helm-Repositorys folgendermaßen an:

    ```bash
    helm repo list
    ```

- Fügen Sie das AGIC-Repository wie folgt hinzu:

    ```bash
    helm repo add \
        application-gateway-kubernetes-ingress \
        https://appgwingress.blob.core.windows.net/ingress-azure-helm-package/
    ```

## <a name="upgrade"></a>Aktualisieren

1. Aktualisieren Sie das AGIC Helm-Repository, um das neueste Release zu erhalten:

    ```bash
    helm repo update
    ```

1. Zeigen Sie verfügbare Versionen des `application-gateway-kubernetes-ingress`-Diagramms an:

    ``` bash
    helm search -l application-gateway-kubernetes-ingress
    ```

    Beispiel für eine Antwort:

    ```bash
    NAME                                                    CHART VERSION   APP VERSION     DESCRIPTION
    application-gateway-kubernetes-ingress/ingress-azure    0.7.0-rc1       0.7.0-rc1       Use Azure Application Gateway as the ingress for an Azure...
    application-gateway-kubernetes-ingress/ingress-azure    0.6.0           0.6.0           Use Azure Application Gateway as the ingress for an Azure...
    ```

    Die neueste verfügbare Version aus der Liste oben ist: `0.7.0-rc1`

1. Zeigen Sie die derzeit installierten Helm-Diagramme an:

    ```bash
    helm list
    ```

    Beispiel für eine Antwort:

    ```bash
    NAME            REVISION        UPDATED                         STATUS  CHART                   APP VERSION     NAMESPACE
    odd-billygoat   22              Fri Jun 21 15:56:06 2019        FAILED  ingress-azure-0.7.0-rc1 0.7.0-rc1       default
    ```

    Die Helm-Diagramminstallation aus der Beispielantwort oben trägt den Namen `odd-billygoat`. Dieser Name wird für die restlichen Befehle verwendet. Ihr tatsächlicher Bereitstellungsname unterscheidet sich wahrscheinlich.

1. Upgraden Sie die Helm-Bereitstellung auf eine neue Version:

    ```bash
    helm upgrade \
        odd-billygoat \
        application-gateway-kubernetes-ingress/ingress-azure \
        --version 0.9.0-rc2
    ```

## <a name="rollback"></a>Rollback

Wenn bei der Helm-Bereitstellung ein Fehler auftritt, können Sie ein Rollback auf ein vorheriges Release ausführen.

1. Rufen Sie die letzte bekannte fehlerfreie Releasenummer ab:

    ```bash
    helm history odd-billygoat
    ```

    Beispielausgabe:

    ```bash
    REVISION        UPDATED                         STATUS          CHART                   DESCRIPTION
    1               Mon Jun 17 13:49:42 2019        DEPLOYED        ingress-azure-0.6.0     Install complete
    2               Fri Jun 21 15:56:06 2019        FAILED          ingress-azure-xx        xxxx
    ```

    Die Beispielausgabe des Befehls `helm history` zeigt, dass die letzte erfolgreiche Bereitstellung von `odd-billygoat` Revision `1` war.

1. Führen Sie ein Rollback zur letzten erfolgreichen Revision aus:

    ```bash
    helm rollback odd-billygoat 1
    ```
