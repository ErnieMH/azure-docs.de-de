---
title: Bereitstellen einer Service Fabric-App in einem Cluster in Azure
description: Hier erfahren Sie, wie Sie über Visual Studio eine vorhandene Anwendung in einem neu erstellten Azure Service Fabric-Cluster bereitstellen.
author: athinanthny
ms.topic: tutorial
ms.date: 07/22/2019
ms.author: mikhegn
ms.custom: mvc
ms.openlocfilehash: e35b655dc8b735214de891884fe40fb951dd16cd
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91441292"
---
# <a name="tutorial-deploy-a-service-fabric-application-to-a-cluster-in-azure"></a>Tutorial: Bereitstellen einer Service Fabric-Anwendung in einem Cluster in Azure

Dieses Tutorial ist der zweite Teil einer Reihe. Sie lernen, wie Sie eine Azure Service Fabric-Anwendung in einem neuen Cluster in Azure bereitstellen.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Erstellen eines Clusters
> * Bereitstellen einer Anwendung in einem Remotecluster mithilfe von Visual Studio

In dieser Tutorialreihe lernen Sie Folgendes:
> [!div class="checklist"]
> * [Erstellen einer .NET Service Fabric-Anwendung](service-fabric-tutorial-create-dotnet-app.md)
> * Bereitstellen der Anwendung in einem Remotecluster
> * [Hinzufügen eines HTTPS-Endpunkts zu einem ASP.NET Core-Front-End-Dienst](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
> * [Konfigurieren von CI/CD mit Azure Pipelines](service-fabric-tutorial-deploy-app-with-cicd-vsts.md).
> * [Einrichten der Überwachung und Diagnose für die Anwendung](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen.
* [Installieren Sie Visual Studio 2019](https://www.visualstudio.com/) und die Workloads **Azure-Entwicklung** und **ASP.NET und Webentwicklung**.
* [Installieren Sie das Service Fabric SDK.](service-fabric-get-started.md)

> [!NOTE]
> Ein kostenloses Konto erfüllt unter Umständen nicht die Anforderungen zum Erstellen eines virtuellen Computers. Aufgrund dessen kann das Tutorial nicht abgeschlossen werden. Darüber hinaus kann es bei einem Konto, das kein Geschäfts-, Schul- oder Unikonto ist, zu Berechtigungsproblemen beim Erstellen des Zertifikats für den Schlüsseltresor kommen, der dem Cluster zugeordnet ist. Wenn ein Fehler im Zusammenhang mit der Zertifikaterstellung auftritt, verwenden Sie das Portal, um stattdessen den Cluster zu erstellen. 

## <a name="download-the-voting-sample-application"></a>Herunterladen der Beispielanwendung „Voting“

Falls Sie die Beispielanwendung „Voting“ aus [Teil 1 dieser Tutorialreihe](service-fabric-tutorial-create-dotnet-app.md) nicht erstellt haben, können Sie sie herunterladen. Führen Sie in einem Befehlsfenster den folgenden Code aus, um das Beispielanwendungsrepository auf Ihrem lokalen Computer zu klonen.

```git
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart 
```

Öffnen Sie die Anwendung in Visual Studio als Administrator, und erstellen Sie die Anwendung.

## <a name="create-a-cluster"></a>Erstellen eines Clusters

Die Anwendung ist nun bereit, und Sie können einen Service Fabric-Cluster erstellen und die Anwendung im Cluster bereitstellen. Ein [Service Fabric-Cluster](./service-fabric-deploy-anywhere.md) enthält eine per Netzwerk verbundene Gruppe von virtuellen oder physischen Computern, auf denen Ihre Microservices bereitgestellt und verwaltet werden.

In diesem Tutorial erstellen Sie einen neuen Testcluster mit drei Knoten in der Visual Studio-IDE und veröffentlichen dann die Anwendung in diesem Cluster. Informationen zum Erstellen eines Produktionsclusters finden Sie unter [Tutorial: Bereitstellen eines Service Fabric-Windows-Clusters in einem virtuellen Azure-Netzwerk](service-fabric-tutorial-create-vnet-and-windows-cluster.md). Sie können die Anwendung auch in einem vorhandenen Cluster bereitstellen, den Sie zuvor mithilfe von [PowerShell](https://portal.azure.com)- oder [Azure CLI](./scripts/service-fabric-powershell-create-secure-cluster-cert.md)-Skripts im [Azure-Portal](./scripts/cli-create-cluster.md) oder über eine [Azure Resource Manager-Vorlage](service-fabric-tutorial-create-vnet-and-windows-cluster.md) erstellt haben.

> [!NOTE]
> Die Voting-Anwendung und zahlreiche andere Anwendungen verwenden den Service Fabric-Reverseproxy für die Kommunikation zwischen Diensten. Bei Clustern, die über Visual Studio erstellt werden, ist der Reverseproxy standardmäßig aktiviert. Bei der Bereitstellung in einem vorhandenen Cluster müssen Sie [den Reverseproxy im Cluster aktivieren](service-fabric-reverseproxy-setup.md), damit die Voting-Anwendung funktioniert.


### <a name="find-the-votingweb-service-endpoint"></a>Suchen des VotingWeb-Dienstendpunkts

Der Front-End-Webdienst der Voting-Anwendung lauscht an einem bestimmten Port (Port 8080, wenn Sie die Schritte in [Teil 1 dieser Tutorialreihe](service-fabric-tutorial-create-dotnet-app.md) ausgeführt haben). Bei der Bereitstellung eines Clusters in Azure durch die Anwendung werden sowohl der Cluster als auch die Anwendung hinter einem Azure-Lastenausgleichsmodul ausgeführt. Der Anwendungsport muss mithilfe eine Regel im Azure-Lastenausgleich geöffnet werden. Die Regel sendet eingehenden Datenverkehr über den Lastenausgleich an den Webdienst. Den Port finden Sie in der Datei **VotingWeb/PackageRoot/ServiceManifest.xml** im **Endpoint**-Element. 

```xml
<Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
```

Notieren Sie den Dienstendpunkt. Dieser wird in einem späteren Schritt benötigt.  Öffnen Sie diesen Port, wenn Sie als Bereitstellungsziel einen bereits vorhandenen Cluster verwenden. Erstellen Sie hierzu eine Lastenausgleichsregel und einen Test im Azure-Lastenausgleich – entweder per [PowerShell-Skript](./scripts/service-fabric-powershell-open-port-in-load-balancer.md) oder im [Azure-Portal](https://portal.azure.com) über den Lastenausgleich für diesen Cluster.

### <a name="create-a-test-cluster-in-azure"></a>Erstellen eines Testclusters in Azure
Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **Voting**, und klicken Sie auf **Veröffentlichen**.

Wählen Sie unter **Verbindungsendpunkt** die Option **Neuen Cluster erstellen**.  Wählen Sie bei der Bereitstellung in einem vorhandenen Cluster den Clusterendpunkt in der Liste aus.  Das Dialogfeld „Service Fabric-Cluster erstellen“ wird geöffnet.

Geben Sie auf der Registerkarte **Cluster** unter **Clustername** einen Namen (etwa „mytestcluster“) ein, und wählen Sie Ihr Abonnement und eine Region für den Cluster (beispielsweise „USA, Süden-Mitte“) aus. Geben Sie außerdem die Anzahl von Clusterknoten (empfohlene Anzahl für einen Testcluster: drei Knoten) und eine Ressourcengruppe (etwa „mytestclustergroup“) ein. Klicken Sie auf **Weiter**.

![Screenshot der Registerkarte „Cluster“ des Dialogfelds „Service Fabric-Cluster erstellen“.](./media/service-fabric-tutorial-deploy-app-to-party-cluster/create-cluster.png)

Geben Sie auf der Registerkarte **Zertifikat** das Kennwort und den Ausgabepfad für das Clusterzertifikat ein. Ein selbstsigniertes Zertifikat wird als PFX-Datei erstellt und am angegebenen Ausgabepfad gespeichert.  Das Zertifikat wird für sowohl die Knoten-zu-Knoten-Sicherheit als auch für die Client-zu-Knoten-Sicherheit verwendet.  Verwenden Sie kein selbstsigniertes Zertifikat für Produktionscluster.  Dieses Zertifikat wird von Visual Studio für die Authentifizierung beim Cluster und für die Bereitstellung von Anwendungen verwendet. Wählen Sie **Zertifikat importieren**, um die PFX-Datei auf Ihrem Computer unter „CurrentUser\My certificate store“ zu installieren.  Klicken Sie auf **Weiter**.

![Screenshot der Registerkarte „Zertifikat“ des Dialogfelds „Service Fabric-Cluster erstellen“.](./media/service-fabric-tutorial-deploy-app-to-party-cluster/certificate.png)

Geben Sie auf der Registerkarte **VM-Detail** unter **Benutzername** und **Kennwort** den Benutzernamen und das Kennwort für das Clusteradministratorkonto ein.  Wählen Sie unter **Image des virtuellen Computers** das VM-Image für die Clusterknoten und unter **Größe des virtuellen Computers** die VM-Größe für die einzelnen Clusterknoten aus.  Klicken Sie auf die Registerkarte **Erweitert**.

![Screenshot der Registerkarte „VM-Details“ des Dialogfelds „Service Fabric-Cluster erstellen“.](./media/service-fabric-tutorial-deploy-app-to-party-cluster/vm-detail.png)

Geben Sie unter **Ports** den VotingWeb-Dienstendpunkt aus dem vorherigen Schritt (beispielsweise 8080) ein.  Bei der Erstellung des Clusters werden diese Anwendungsports im Azure-Lastenausgleich geöffnet, damit Datenverkehr an den Cluster weitergeleitet werden kann.  Klicken Sie auf **Erstellen**, um den Cluster zu erstellen. Dieser Vorgang dauert mehrere Minuten.

![Screenshot der Registerkarte „Erweitert“ des Dialogfelds „Service Fabric-Cluster erstellen“.](./media/service-fabric-tutorial-deploy-app-to-party-cluster/advanced.png)

## <a name="publish-the-application-to-the-cluster"></a>Veröffentlichen der Anwendung im Cluster

Wenn der neue Cluster bereit ist, können Sie die Voting-Anwendung direkt aus Visual Studio bereitstellen.

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **Voting**, und klicken Sie auf **Veröffentlichen**. Das Dialogfeld **Veröffentlichen** wird angezeigt.

Wählen Sie unter **Verbindungsendpunkt** den Endpunkt des im vorherigen Schritt erstellten Clusters aus.  Beispiel: mytestcluster.southcentral.cloudapp.azure.com:19000. Wenn Sie **Erweiterte Verbindungsparameter** auswählen, sollten die Zertifikatinformationen automatisch ausgefüllt werden.  
![Veröffentlichen einer Service Fabric-Anwendung](./media/service-fabric-tutorial-deploy-app-to-party-cluster/publish-app.png)

Wählen Sie **Veröffentlichen**.

Öffnen Sie nach der Bereitstellung der Anwendung einen Browser, und geben Sie die Clusteradresse gefolgt von **:8080** ein. Oder geben Sie einen anderen Port ein, sofern einer konfiguriert ist. z. B. `http://mytestcluster.southcentral.cloudapp.azure.com:8080`. Sie sehen jetzt, dass die Anwendung im Cluster in Azure ausgeführt wird. Fügen Sie auf der Voting-Webseite Abstimmungsoptionen hinzu, löschen Sie Abstimmungsoptionen, und stimmen Sie für einzelne oder mehrere dieser Optionen ab.

![Service Fabric-Beispiel „Voting“](./media/service-fabric-tutorial-deploy-app-to-party-cluster/application-screenshot-new-azure.png)


## <a name="next-steps"></a>Nächste Schritte
In diesem Teil des Tutorials haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen eines Clusters
> * Bereitstellen einer Anwendung in einem Remotecluster mithilfe von Visual Studio

Fahren Sie mit dem nächsten Tutorial fort:
> [!div class="nextstepaction"]
> [Aktivieren von HTTPS](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md)
