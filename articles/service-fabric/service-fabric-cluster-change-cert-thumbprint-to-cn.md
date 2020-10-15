---
title: Aktualisieren eines Clusters, um den allgemeinen Namen des Zertifikats zu verwenden
description: Erfahren Sie, wie Sie einen Service Fabric-Cluster so ändern, dass anstelle des Zertifikatfingerabdrucks der allgemeine Name des Zertifikats verwendet wird.
ms.topic: conceptual
ms.date: 09/06/2019
ms.openlocfilehash: a90290430616302dbbe9ab9cf717510070936529
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86247913"
---
# <a name="change-cluster-from-certificate-thumbprint-to-common-name"></a>Ändern des Clusters von „Zertifikatfingerabdruck“ zu „Allgemeiner Name“
Keine zwei Zertifikate können den gleichen Fingerabdruck haben, was ein Clusterzertifikatrollover oder die Verwaltung erschwert. Mehrere Zertifikate können jedoch den gleichen allgemeinen Namen oder den gleichen Antragsteller haben.  Durch den Wechsel von „Zertifikatfingerabdruck“ zu „Allgemeiner Name“ bei einem bereitgestellten Cluster wird die Zertifikatverwaltung vereinfacht. In diesem Artikel wird beschrieben, wie Sie einen aktuell ausgeführten Service Fabric-Cluster für die Verwendung des allgemeinen Namens (anstelle des Zertifikatfingerabdrucks) aktualisieren.

>[!NOTE]
> Wenn in Ihrer Vorlage zwei Fingerabdrücke deklariert sind, müssen Sie zwei Bereitstellungen durchführen.  Die erste Bereitstellung muss abgeschlossen sein, bevor Sie die Schritte in diesem Artikel ausführen.  Die erste Bereitstellung legt Ihre **thumbprint**-Eigenschaft in der Vorlage auf das verwendete Zertifikat fest und entfernt die **thumbprintSecondary**-Eigenschaft.  Für die zweite Bereitstellung befolgen Sie die Schritte in diesem Artikel.
 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="get-a-certificate"></a>Abrufen eines Zertifikats
Fordern Sie zunächst ein Zertifikat von einer [Zertifizierungsstelle (CA)](https://wikipedia.org/wiki/Certificate_authority) an.  Der allgemeine Name des Zertifikats sollte für die benutzerdefinierte Domäne bestimmt sein, die Sie besitzen, und bei einer Domänenregistrierungsstelle gekauft sein. Beispiel: „azureservicefabricbestpractices.com“; da nur Microsoft-Mitarbeiter Zertifikate für MS-Domänen bereitstellen können, können Sie die DNS-Namen von Ihrem LB oder Traffic Manager nicht als allgemeine Namen für Ihr Zertifikat verwenden, und Sie müssen eine [Azure DNS-Zone](../dns/dns-delegate-domain-azure-dns.md) bereitstellen, wenn Ihre benutzerdefinierte Domäne in Azure auflösbar sein soll. Sie sollten auch Ihre benutzerdefinierte Domäne als „managementEndpoint“ Ihres Clusters deklarieren, wenn das Portal den Alias der benutzerdefinierten Domäne für Ihren Cluster reflektieren soll.

Zu Testzwecken können Sie von einer kostenlosen oder offenen Zertifizierungsstelle ein von der Zertifizierungsstelle signiertes Zertifikat abrufen.

> [!NOTE]
> Selbstsignierte Zertifikate, einschließlich der beim Bereitstellen eines Service Fabric-Clusters im Azure-Portal generierten Zertifikate, werden nicht unterstützt. 

## <a name="upload-the-certificate-and-install-it-in-the-scale-set"></a>Hochladen des Zertifikats und Installieren in der Skalierungsgruppe
In Azure wird ein Service Fabric-Cluster in einer VM-Skalierungsgruppe bereitgestellt.  Laden Sie das Zertifikat in einen Schlüsseltresor hoch, und installieren Sie es in der VM-Skalierungsgruppe, in welcher der Cluster ausgeführt wird.

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser -Force

$SubscriptionId  =  "<subscription ID>"

# Sign in to your Azure account and select your subscription
Login-AzAccount -SubscriptionId $SubscriptionId

$region = "southcentralus"
$KeyVaultResourceGroupName  = "mykeyvaultgroup"
$VaultName = "mykeyvault"
$certFilename = "C:\users\sfuser\myclustercert.pfx"
$certname = "myclustercert"
$Password  = "P@ssw0rd!123"
$VmssResourceGroupName     = "myclustergroup"
$VmssName                  = "prnninnxj"

# Create new Resource Group 
New-AzResourceGroup -Name $KeyVaultResourceGroupName -Location $region

# Create the new key vault
$newKeyVault = New-AzKeyVault -VaultName $VaultName -ResourceGroupName $KeyVaultResourceGroupName `
    -Location $region -EnabledForDeployment 
$resourceId = $newKeyVault.ResourceId 

# Add the certificate to the key vault.
$PasswordSec = ConvertTo-SecureString -String $Password -AsPlainText -Force
$KVSecret = Import-AzKeyVaultCertificate -VaultName $vaultName -Name $certName `
    -FilePath $certFilename -Password $PasswordSec

$CertificateThumbprint = $KVSecret.Thumbprint
$CertificateURL = $KVSecret.SecretId
$SourceVault = $resourceId
$CommName    = $KVSecret.Certificate.SubjectName.Name

Write-Host "CertificateThumbprint    :"  $CertificateThumbprint
Write-Host "CertificateURL           :"  $CertificateURL
Write-Host "SourceVault              :"  $SourceVault
Write-Host "Common Name              :"  $CommName    

Set-StrictMode -Version 3
$ErrorActionPreference = "Stop"

$certConfig = New-AzVmssVaultCertificateConfig -CertificateUrl $CertificateURL -CertificateStore "My"

# Get current VM scale set 
$vmss = Get-AzVmss -ResourceGroupName $VmssResourceGroupName -VMScaleSetName $VmssName

# Add new secret to the VM scale set.
$vmss = Add-AzVmssSecret -VirtualMachineScaleSet $vmss -SourceVaultId $SourceVault `
    -VaultCertificate $certConfig

# Update the VM scale set 
Update-AzVmss -ResourceGroupName $VmssResourceGroupName -Verbose `
    -Name $VmssName -VirtualMachineScaleSet $vmss 
```

>[!NOTE]
> Geheimnisse von VM-Skalierungsgruppen unterstützen nicht die gleiche Ressourcen-ID für zwei separate Geheimnisse, da jedes Geheimnis eine eindeutige Ressource mit Versionsverwaltung ist. 

## <a name="download-and-update-the-template-from-the-portal"></a>Herunterladen der Vorlage aus dem Portal und Aktualisieren der Vorlage
Das Zertifikat wurde zwar in der zugrundeliegenden Skalierungsgruppe installiert, Sie müssen aber auch den Service Fabric-Cluster aktualisieren, damit er dieses Zertifikat und den entsprechenden allgemeinen Namen verwendet.  Laden Sie nun die Vorlage für die Clusterbereitstellung herunter.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zu der Ressourcengruppe, die den Cluster hostet.  Wählen Sie unter **Einstellungen** die Option **Bereitstellungen** aus.  Wählen Sie die neueste Bereitstellung aus, und klicken Sie auf **Vorlage anzeigen**.

![Anzeigen von Vorlagen][image1]

Laden Sie die Vorlagedatei und die JSON-Parameterdatei auf Ihren lokalen Computer herunter.

Öffnen Sie zunächst die Parameterdatei in einem Text-Editor, und fügen Sie den folgenden Parameterwert hinzu:
```json
"certificateCommonName": {
    "value": "myclustername.southcentralus.cloudapp.azure.com"
},
```

Öffnen Sie als Nächstes die Vorlagendatei in einem Text-Editor, und nehmen Sie drei Aktualisierungen vor, damit der allgemeine Name des Zertifikats unterstützt wird.

1. Fügen Sie im Abschnitt **parameters** einen Parameter *certificateCommonName* hinzu:
    ```json
    "certificateCommonName": {
        "type": "string",
        "metadata": {
            "description": "Certificate Commonname"
        }
    },
    ```

    Außerdem könnten Sie *certificateThumbprint* entfernen, da dieser Parameter in der Resource Manager-Vorlage wahrscheinlich nicht mehr referenziert wird.

2. Aktualisieren Sie in der Ressource **Microsoft.Compute/virtualMachineScaleSets** die VM-Erweiterung, damit in den Zertifikateinstellungen anstelle des Fingerabdrucks der allgemeine Name verwendet wird.  Fügen Sie unter **virtualMachineProfile**->**extensionProfile**->**extensions**->**properties**->**settings**->**certificate** den Eintrag `"commonNames": ["[parameters('certificateCommonName')]"],` hinzu, und entfernen Sie `"thumbprint": "[parameters('certificateThumbprint')]",`.
    ```json
        "virtualMachineProfile": {
        "extensionProfile": {
            "extensions": [
                {
                    "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
                    "properties": {
                        "type": "ServiceFabricNode",
                        "autoUpgradeMinorVersion": true,
                        "protectedSettings": {
                            "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
                            "StorageAccountKey2": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key2]"
                        },
                        "publisher": "Microsoft.Azure.ServiceFabric",
                        "settings": {
                            "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                            "nodeTypeRef": "[variables('vmNodeType0Name')]",
                            "dataPath": "D:\\SvcFab",
                            "durabilityLevel": "Bronze",
                            "enableParallelJobs": true,
                            "nicPrefixOverride": "[variables('subnet0Prefix')]",
                            "certificate": {
                                "commonNames": [
                                    "[parameters('certificateCommonName')]"
                                ],
                                "x509StoreName": "[parameters('certificateStoreValue')]"
                            }
                        },
                        "typeHandlerVersion": "1.0"
                    }
                },
    ```

3.  Aktualisieren Sie in der Ressource **Microsoft.ServiceFabric/clusters** die API-Version auf „2018-02-01“.  Fügen Sie auch eine Einstellung **certificateCommonNames** mit der Eigenschaft **commonNames** hinzu, und entfernen Sie die Einstellung **certificate** (mit der Eigenschaft „Thumbprint“), wie im folgenden Beispiel gezeigt:
    ```json
    {
        "apiVersion": "2018-02-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
        ],
        "properties": {
            "addonFeatures": [
                "DnsService",
                "RepairManager"
            ],
            "certificateCommonNames": {
                "commonNames": [
                    {
                        "certificateCommonName": "[parameters('certificateCommonName')]",
                        "certificateIssuerThumbprint": ""
                    }
                ],
                "x509StoreName": "[parameters('certificateStoreValue')]"
            },
        ...
    ```

Weitere Informationen finden Sie unter [Bereitstellen eines Service Fabric-Clusters mit allgemeinem Zertifikatnamen anstelle eines Fingerabdrucks](./service-fabric-create-cluster-using-cert-cn.md).

## <a name="deploy-the-updated-template"></a>Bereitstellen der aktualisierten Vorlage
Stellen Sie nach Abschluss der Änderungen die aktualisierte Vorlage erneut bereit.

```powershell
$groupname = "sfclustertutorialgroup"

New-AzResourceGroupDeployment -ResourceGroupName $groupname -Verbose `
    -TemplateParameterFile "C:\temp\cluster\parameters.json" -TemplateFile "C:\temp\cluster\template.json" 
```

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [Clustersicherheit](service-fabric-cluster-security.md).
* Erfahren Sie, wie Sie einen [Rollover für ein Cluster-Zertifikat ausführen](service-fabric-cluster-rollover-cert-cn.md).
* [Aktualisieren und Verwalten von Clusterzertifikaten](service-fabric-cluster-security-update-certs-azure.md)

[image1]: ./media/service-fabric-cluster-change-cert-thumbprint-to-cn/PortalViewTemplates.png
