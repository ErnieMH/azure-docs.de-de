---
title: Azure Files-Volumetreiber für Service Fabric
description: Service Fabric unterstützt die Verwendung von Azure Files zur Sicherung von Volumes aus Ihrem Container.
ms.topic: conceptual
ms.date: 6/10/2018
ms.openlocfilehash: a5125dbd88a2fe236196c427244f1311d9b73b9f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "86247692"
---
# <a name="azure-files-volume-driver-for-service-fabric"></a>Azure Files-Volumetreiber für Service Fabric

Der Azure Files-Volumetreiber ist ein [Docker-Volume-Plug-In](https://docs.docker.com/engine/extend/plugins_volume/), das auf [Azure Files](../storage/files/storage-files-introduction.md) basierende Volumes für Docker-Container bereitstellt. Es wird als Service Fabric-Anwendung gepackt, die auf einem Service Fabric-Cluster bereitgestellt werden kann, um Volumes für andere Service Fabric-Containeranwendungen im Cluster bereitzustellen.

> [!NOTE]
> Version 6.5.661.9590 des Azure Files-Volume-Plug-Ins ist jetzt allgemein verfügbar.
>

## <a name="prerequisites"></a>Voraussetzungen
* Die Windows-Version des Azure Files-Volume-Plug-Ins funktioniert nur mit [Windows Server, Version 1709](/windows-server/get-started/whats-new-in-windows-server-1709), [Windows 10, Version 1709](/windows/whats-new/whats-new-windows-10-version-1709), oder höheren Betriebssystemversionen.

* Die Linux-Version des Azure Files-Volume-Plug-Ins funktioniert auf allen Betriebssystemversionen, die von Service Fabric unterstützt werden.

* Das Azure Files-Volume-Plug-In funktioniert nur in Service Fabric-Version 6.2 und höher.

* Befolgen Sie die Anweisungen in der [Azure Files-Dokumentation](../storage/files/storage-how-to-create-file-share.md), um eine Dateifreigabe für die Service Fabric-Containeranwendung zu erstellen, die als Volume verwendet werden soll.

* [PowerShell mit dem Service Fabric-Modul](./service-fabric-get-started.md) oder [SFCTL](./service-fabric-cli.md) muss installiert sein.

* Wenn Sie Hyper-V-Container verwenden, müssen in der Azure Resource Manager-Vorlage (Azure-Cluster) oder der Datei „ClusterConfig.json“ (eigenständiger Cluster) im Abschnitt „ClusterManifest“ (lokaler Cluster) oder „fabricSettings“ die folgenden Codeausschnitte eingefügt werden.

In „ClusterManifest“ muss Folgendes im Abschnitt „Hosting“ hinzugefügt werden. In diesem Beispiel lautet der Volumename **sfazurefile** und der Port, an dem auf dem Cluster gelauscht wird, ist **19100**. Ersetzen Sie diese durch die richtigen Werte für Ihren Cluster.

``` xml 
<Section Name="Hosting">
  <Parameter Name="VolumePluginPorts" Value="sfazurefile:19100" />
</Section>
```

Im Abschnitt „fabricSettings“ der Azure Resource Manager-Vorlage (für Azure-Bereitstellungen) oder der Datei „ClusterConfig.json“ (für eigenständige Bereitstellungen) muss der folgende Codeausschnitt hinzugefügt werden. Ersetzen Sie auch hier die Werte für Volumename und Port durch Ihre eigenen Werte.

```json
"fabricSettings": [
  {
    "name": "Hosting",
    "parameters": [
      {
          "name": "VolumePluginPorts",
          "value": "sfazurefile:19100"
      }
    ]
  }
]
```

## <a name="deploy-a-sample-application-using-service-fabric-azure-files-volume-driver"></a>Bereitstellen einer Beispielanwendung mithilfe des Azure Files-Volumetreibers für Service Fabric

### <a name="using-azure-resource-manager-via-the-provided-powershell-script-recommended"></a>Verwenden von Azure Resource Manager über das bereitgestellte PowerShell-Skript (empfohlen)

Wenn Ihr Cluster auf Azure basiert, wird empfohlen, Anwendungen mithilfe des Azure Resource Manager-Anwendungsressourcenmodells bereitzustellen, um die Benutzerfreundlichkeit zu erhöhen und den Übergang zum Modell der Wartung der Infrastruktur als Code zu erleichtern. Bei diesem Ansatz entfällt die Notwendigkeit, die App-Version für den Azure Files-Volumetreiber nachzuverfolgen. Außerdem können Sie separate Azure Resource Manager-Vorlagen für jedes unterstützte Betriebssystem verwalten. Beim Skript wird davon ausgegangen, dass Sie die neueste Version der Azure Files-Anwendung bereitstellen, und es akzeptiert Parameter für den Betriebssystemtyp, die Cluster-Abonnement-ID und die Ressourcengruppe. Sie können das Skript von der [Service Fabric-Downloadwebsite](https://sfazfilevd.blob.core.windows.net/sfazfilevd/DeployAzureFilesVolumeDriver.zip) herunterladen. Beachten Sie, dass dadurch automatisch der Wert für „ListenPort“ auf 19100 festgelegt wird. Hierbei handelt es sich um den Port, an dem das Azure Files-Volume-Plug-In auf Anforderungen des Docker-Daemons lauscht. Sie können dies ändern, indem Sie einen Parameter mit dem Namen „listenPort“ hinzufügen. Stellen Sie sicher, dass der Port keinen Konflikt mit einem anderen Port verursacht, der vom Cluster oder Ihren Anwendungen verwendet wird.
 

Azure Resource Manager-Bereitstellungsbefehl für Windows:
```powershell
.\DeployAzureFilesVolumeDriver.ps1 -subscriptionId [subscriptionId] -resourceGroupName [resourceGroupName] -clusterName [clusterName] -windows
```

Azure Resource Manager-Bereitstellungsbefehl für Linux:
```powershell
.\DeployAzureFilesVolumeDriver.ps1 -subscriptionId [subscriptionId] -resourceGroupName [resourceGroupName] -clusterName [clusterName] -linux
```

Nachdem Sie das Skript erfolgreich ausgeführt haben, können Sie mit dem [Abschnitt zum Konfigurieren Ihrer Anwendung](#configure-your-applications-to-use-the-volume) fortfahren.


### <a name="manual-deployment-for-standalone-clusters"></a>Manuelle Bereitstellung für eigenständige Cluster

Die Service Fabric-Anwendung, die die Volumes für die Container bereitstellt, kann von der [Service Fabric-Downloadwebsite](https://sfazfilevd.blob.core.windows.net/sfazfilevd/AzureFilesVolumePlugin.6.5.661.9590.zip) heruntergeladen werden. Die Anwendung kann dem Cluster mit [PowerShell](./service-fabric-deploy-remove-applications.md), [CLI](./service-fabric-application-lifecycle-sfctl.md) oder [FabricClient-APIs](./service-fabric-deploy-remove-applications-fabricclient.md) bereitgestellt werden.

1. Wechseln Sie über die Befehlszeile in das Stammverzeichnis des heruntergeladenen Anwendungspakets.

    ```powershell
    cd .\AzureFilesVolume\
    ```

    ```bash
    cd ~/AzureFilesVolume
    ```

2. Als Nächstes kopieren Sie das Anwendungspaket in den Imagespeicher mit den entsprechenden Werten für [ApplicationPackagePath] und [ImageStoreConnectionString]:

    ```powershell
    Copy-ServiceFabricApplicationPackage -ApplicationPackagePath [ApplicationPackagePath] -ImageStoreConnectionString [ImageStoreConnectionString] -ApplicationPackagePathInImageStore AzureFilesVolumePlugin
    ```

    ```bash
    sfctl cluster select --endpoint https://testcluster.westus.cloudapp.azure.com:19080 --pem test.pem --no-verify
    sfctl application upload --path [ApplicationPackagePath] --show-progress
    ```

3. Registrierung des Anwendungstyps

    ```powershell
    Register-ServiceFabricApplicationType -ApplicationPathInImageStore AzureFilesVolumePlugin
    ```

    ```bash
    sfctl application provision --application-type-build-path [ApplicationPackagePath]
    ```

4. Erstellen Sie die Anwendung, und achten Sie dabei auf den Wert des Anwendungsparameters **ListenPort**. Dieser Wert ist der Port, an dem das Azure Files-Volume-Plug-In auf Anforderungen des Docker-Daemons lauscht. Stellen Sie sicher, dass der für die Anwendung angegebene Port mit den VolumePluginPorts im ClusterManifest übereinstimmt und keinen Konflikt mit einem anderen Port verursacht, der vom Cluster oder Ihren Anwendungen verwendet wird.

    ```powershell
    New-ServiceFabricApplication -ApplicationName fabric:/AzureFilesVolumePluginApp -ApplicationTypeName AzureFilesVolumePluginType -ApplicationTypeVersion 6.5.661.9590   -ApplicationParameter @{ListenPort='19100'}
    ```

    ```bash
    sfctl application create --app-name fabric:/AzureFilesVolumePluginApp --app-type AzureFilesVolumePluginType --app-version 6.5.661.9590  --parameter '{"ListenPort":"19100"}'
    ```

> [!NOTE]
> 
> Windows Server 2016 Datacenter unterstützt keine Zuordnung von SMB-Bereitstellungen für Container ([Dies wird nur unter Windows Server Version 1709 unterstützt](/virtualization/windowscontainers/manage-containers/container-storage)). Aufgrund dieser Einschränkung sind keine Netzwerkvolumezuordnungen oder Installationen von Azure Files-Volumetreibern auf älteren Versionen als 1709 möglich.

#### <a name="deploy-the-application-on-a-local-development-cluster"></a>Bereitstellen der Anwendung in einem lokalen Bereitstellungscluster
Führen Sie die Schritte 1 bis 3 [oben](#manual-deployment-for-standalone-clusters) aus.

 Die Standardanzahl von Dienstinstanzen für die Azure Files-Volume-Plug-In-Anwendung ist -1. Dies bedeutet, dass auf jedem Knoten im Cluster eine Instanz des Diensts bereitgestellt wird. Bei der Bereitstellung der Azure Files-Volume-Plug-In-Anwendung in einem lokalen Bereitstellungscluster sollte jedoch als Anzahl der Dienstinstanzen 1 angegeben werden. Dazu kann der Anwendungsparameter **InstanceCount** verwendet werden. Daher lautet der Befehl für das Erstellen der Azure Files-Volume-Plug-In-Anwendung in einem lokalen Bereitstellungscluster:

```powershell
New-ServiceFabricApplication -ApplicationName fabric:/AzureFilesVolumePluginApp -ApplicationTypeName AzureFilesVolumePluginType -ApplicationTypeVersion 6.5.661.9590  -ApplicationParameter @{ListenPort='19100';InstanceCount='1'}
```

```bash
sfctl application create --app-name fabric:/AzureFilesVolumePluginApp --app-type AzureFilesVolumePluginType --app-version 6.5.661.9590  --parameter '{"ListenPort": "19100","InstanceCount": "1"}'
```

## <a name="configure-your-applications-to-use-the-volume"></a>Konfigurieren Ihrer Anwendungen für die Verwendung des Volumes
Der folgende Codeausschnitt zeigt, wie ein auf Azure Files basierendes Volume in der Anwendungsmanifestdatei der Anwendung angegeben werden kann. Das interessante Element ist das **Volume**-Tag:

```xml
?xml version="1.0" encoding="UTF-8"?>
<ApplicationManifest ApplicationTypeName="WinNodeJsApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
    <Description>Calculator Application</Description>
    <Parameters>
      <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
      <Parameter Name="MyCpuShares" DefaultValue="3"></Parameter>
      <Parameter Name="MyStorageVar" DefaultValue="c:\tmp"></Parameter>
    </Parameters>
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="NodeServicePackage" ServiceManifestVersion="1.0"/>
     <Policies>
       <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
            <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
            <Volume Source="azfiles" Destination="c:\VolumeTest\Data" Driver="sfazurefile">
                <DriverOption Name="shareName" Value="" />
                <DriverOption Name="storageAccountName" Value="" />
                <DriverOption Name="storageAccountKey" Value="" />
                <DriverOption Name="storageAccountFQDN" Value="" />
            </Volume>
       </ContainerHostPolicies>
   </Policies>
    </ServiceManifestImport>
    <ServiceTemplates>
        <StatelessService ServiceTypeName="StatelessNodeService" InstanceCount="5">
            <SingletonPartition></SingletonPartition>
        </StatelessService>
    </ServiceTemplates>
</ApplicationManifest>
```

Der Treibername für das Azure Files-Volume-Plug-In lautet **sfazurefile**. Dieser Wert wird für das Attribut **Driver** des Tagelements **Volume** im Anwendungsmanifest festgelegt.

Im Tag **Volume** im obigen Codeausschnitt erfordert das Azure Files-Volume-Plug-In die folgenden Attribute:
- **Source**: Dies ist der Name des Volumes. Der Benutzer kann einen beliebigen Namen für sein Volume auswählen.
- **Destination**: Dieses Attribut ist der Speicherort, dem das Volume im ausgeführten Container zugeordnet ist. Das Ziel kann also nicht ein bereits vorhandener Speicherort innerhalb des Containers sein.

Wie mit den **DriverOption**-Elementen im obigen Codeausschnitt dargestellt, unterstützt das Azure Files-Volume-Plug-In die folgenden Treiberoptionen:
- **shareName**: Der Name der Azure Files-Dateifreigabe, die das Volume für den Container bereitstellt
- **storageAccountName**: Der Name des Azure-Speicherkontos, das die Azure Files-Dateifreigabe enthält
- **storageAccountKey**: Der Zugriffsschlüssel des Azure-Speicherkontos, das die Azure Files-Dateifreigabe enthält
- **storageAccountFQDN**: Der dem Speicherkonto zugeordnete Domänenname. Wenn storageAccountFQDN nicht angegeben ist, wird der Domänenname mit dem Standardsuffix (.file.core.windows.net) und dem storageAccountName gebildet.  

    ```xml
    - Example1: 
        <DriverOption Name="shareName" Value="myshare1" />
        <DriverOption Name="storageAccountName" Value="myaccount1" />
        <DriverOption Name="storageAccountKey" Value="mykey1" />
        <!-- storageAccountFQDN will be "myaccount1.file.core.windows.net" -->
    - Example2: 
        <DriverOption Name="shareName" Value="myshare2" />
        <DriverOption Name="storageAccountName" Value="myaccount2" />
        <DriverOption Name="storageAccountKey" Value="mykey2" />
        <DriverOption Name="storageAccountFQDN" Value="myaccount2.file.core.chinacloudapi.cn" />
    ```

## <a name="using-your-own-volume-or-logging-driver"></a>Verwenden Ihres eigenen Volume- oder Protokollierungstreibers
Service Fabric ermöglicht auch die Verwendung von benutzerdefinierten [Volume](https://docs.docker.com/engine/extend/plugins_volume/)- oder [Protokollierungstreibern](https://docs.docker.com/engine/admin/logging/overview/). Wenn der Docker-Volume-/Protokollierungstreiber nicht im Cluster installiert ist, können Sie ihn mithilfe der RDP/SSH-Protokolle manuell installieren. Sie können die Installation mit diesen Protokollen über ein [VMSS-Startskript](https://azure.microsoft.com/resources/templates/201-vmss-custom-script-windows/) oder ein [SetupEntryPoint-Skript](./service-fabric-application-model.md) durchführen.

Beispiel für das Skript zum Installieren des [Docker-Volumetreibers für Azure](https://docs.docker.com/docker-for-azure/persistent-data-volumes/):

```bash
docker plugin install --alias azure --grant-all-permissions docker4x/cloudstor:17.09.0-ce-azure1  \
    CLOUD_PLATFORM=AZURE \
    AZURE_STORAGE_ACCOUNT="[MY-STORAGE-ACCOUNT-NAME]" \
    AZURE_STORAGE_ACCOUNT_KEY="[MY-STORAGE-ACCOUNT-KEY]" \
    DEBUG=1
```

Um in Ihren Anwendungen den Volume- oder Protokollierungstreiber zu verwenden, den Sie installiert haben, müssten Sie die entsprechenden Werte in den Elementen **Volume** und **LogConfig** unter **ContainerHostPolicies** im Anwendungsmanifest angeben.

```xml
<ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
    <RepositoryCredentials PasswordEncrypted="false" Password="****" AccountName="test"/>
    <LogConfig Driver="[YOUR_LOG_DRIVER]" >
        <DriverOption Name="test" Value="vale"/>
    </LogConfig>
    <Volume Source="c:\workspace" Destination="c:\testmountlocation1" IsReadOnly="false"></Volume>
    <Volume Source="[MyStorageVar]" Destination="c:\testmountlocation2" IsReadOnly="true"> </Volume>
    <Volume Source="myvolume1" Destination="c:\testmountlocation2" Driver="[YOUR_VOLUME_DRIVER]" IsReadOnly="true">
        <DriverOption Name="[name]" Value="[value]"/>
    </Volume>
</ContainerHostPolicies>
```

Wenn Sie ein Volume-Plug-In angeben, erstellt Service Fabric das Volume automatisch mit den angegebenen Parametern. Das **Source**-Tag für das **Volume**-Element ist der Name des Volumes, und das **Driver**-Tag gibt das Volumetreiber-Plug-In an. Das **Destination**-Tag ist der Speicherort, dem **Source** im ausgeführten Container zugeordnet ist. Das Ziel kann also nicht ein bereits vorhandener Speicherort innerhalb des Containers sein. Optionen können mithilfe des **DriverOption**-Tags wie folgt angegeben werden:

```xml
<Volume Source="myvolume1" Destination="c:\testmountlocation4" Driver="azure" IsReadOnly="true">
           <DriverOption Name="share" Value="models"/>
</Volume>
```

Anwendungsparameter werden für Volumes unterstützt, wie im vorherigen Manifestausschnitt dargestellt. (Suchen Sie nach `MyStorageVar` für ein Anwendungsbeispiel.)

Wenn ein Docker-Protokolltreiber angegeben wird, müssen Sie Agents (oder Container) für die Verarbeitung der Protokolle im Cluster bereitstellen. Mit dem **DriverOption**-Tag können Optionen für den Protokolltreiber angegeben werden.

## <a name="next-steps"></a>Nächste Schritte
* Containerbeispiele, einschließlich des Volumetreibers, finden Sie unter [Service Fabric-Containerbeispiele](https://github.com/Azure-Samples/service-fabric-containers)
* Informationen zum Bereitstellen von Containern in einem Service Fabric-Cluster finden Sie im Artikel [Bereitstellen eines Containers in Service Fabric](./service-fabric-get-started-containers.md).
