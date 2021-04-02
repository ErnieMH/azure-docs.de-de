---
title: Anzeigen von Repositorys im Portal
description: Verwenden Sie das Azure-Portal, um Azure Container Registry-Repositorys anzuzeigen, die Docker-Containerimages und andere unterstützte Artefakte hosten.
ms.topic: article
ms.date: 01/05/2018
ms.openlocfilehash: 1da72706d2554610a685f71199ab14af5e30ce1a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "74456298"
---
# <a name="view-container-registry-repositories-in-the-azure-portal"></a>Anzeigen von Azure Container Registry-Repositorys im Azure-Portal

Mit Azure Container Registry können Sie Docker-Containerimages in Repositorys speichern. Indem Sie Images in Repositorys speichern, können Sie Gruppen von Images (oder Versionen von Images) in einer isolierten Umgebung speichern. Sie können diese Repositorys angeben, wenn Sie Images per Push in Ihre Registrierung übertragen, und ihren Inhalt im Azure-Portal anzeigen.

## <a name="prerequisites"></a>Voraussetzungen

* **Containerregistrierung**: Erstellen Sie in Ihrem Azure-Abonnement eine Containerregistrierung. Verwenden Sie beispielsweise das [Azure-Portal](container-registry-get-started-portal.md) oder die [Azure CLI](container-registry-get-started-azure-cli.md).
* **Docker CLI**: Installieren Sie [Docker][docker-install] auf dem lokalen Computer. Anschließend steht Ihnen die Docker-Befehlszeilenschnittstelle zur Verfügung.
* **Containerimage**: Übertragen Sie ein Image per Push an Ihre Containerregistrierung. Anleitungen für Push- und Pull-Vorgänge für Images finden Sie unter [Push- und Pull-Vorgänge für Images](container-registry-get-started-docker-cli.md).

## <a name="view-repositories-in-azure-portal"></a>Anzeigen von Repositorys im Azure-Portal

Sie können eine Liste der Repositorys, in denen Ihre Bilder gespeichert sind, sowie die Imagetags im Azure-Portal anzeigen.

Wenn Sie die Schritte unter [Push- und Pull-Vorgänge für Images](container-registry-get-started-docker-cli.md) befolgt haben (und das Image anschließend nicht gelöscht haben), sollten Sie in der Containerregistrierung über ein Nginx-Image verfügen. Die Anweisungen in diesem Artikel besagen, dass Sie das Image mit einem Namespace, „samples“ in `/samples/nginx`, versehen. Zur Erinnerung: Der [docker push][docker-push]-Befehl, der in diesem Artikel angegeben war, lautete:

```Bash
docker push myregistry.azurecr.io/samples/nginx
```

 Da Azure Container Registry Namespaces mit mehreren Ebenen unterstützt, können Sie Sammlungen mit Images für eine bestimmte App oder eine Sammlung von Apps auf verschiedene Entwicklungs- oder Betriebsteams beschränken. Weitere Informationen zu Repositorys in Containerregistrierungen finden Sie unter [Private Docker-Containerregistrierungen in Azure](container-registry-intro.md).

So zeigen Sie ein Repository an

1. Melden Sie sich beim [Azure-Portal][portal]
1. Wählen Sie die **Azure Container Registry** aus, an die Sie das Nginx-Image per Push übertragen haben.
1. Wählen Sie **Repositorys** aus, um eine Liste der Repositorys anzuzeigen, die die Images in der Registrierung enthalten.
1. Wählen Sie ein Repository aus, um die Imagetags in diesem Repository anzuzeigen.

Wenn Sie beispielsweise das Nginx-Image entsprechend den Anweisungen unter [Push- und Pull-Vorgänge für Images](container-registry-get-started-docker-cli.md) per Push übermittelt haben, sollte Folgendes angezeigt werden:

![Repositorys im Portal](./media/container-registry-repositories/container-registry-repositories.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun mit den Grundlagen des Anzeigens und Arbeitens mit Repositorys im Portal vertraut sind, versuchen Sie, Azure Container Registry mit einem [Azure Kubernetes Service](../aks/tutorial-kubernetes-prepare-app.md)-Cluster (AKS) zu verwenden.

<!-- LINKS - External -->
[docker-install]: https://docs.docker.com/engine/installation/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[portal]: https://portal.azure.com
