---
title: Festlegen eines Azure Resource Manager-Kennworts auf Ihrem Azure Stack Edge Pro-GPU-Gerät
description: In diesem Artikel wird beschrieben, wie Sie über Azure PowerShell eine Verbindung mit der Azure Resource Manager-Instanz herstellen, die auf Ihrem Azure Stack Edge Pro-GPU-Gerät ausgeführt wird.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/28/2020
ms.author: alkohli
ms.openlocfilehash: df5fea8101834dae089ab97354c438363321a707
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90904495"
---
# <a name="set-azure-resource-manager-password-on-azure-stack-edge-pro-gpu-device"></a>Festlegen eines Azure Resource Manager-Kennworts auf einem Azure Stack Edge Pro-GPU-Gerät

<!--[!INCLUDE [applies-to-skus](../../includes/azure-stack-edge-applies-to-all-sku.md)]-->

In diesem Artikel wird beschrieben, wie Sie Ihr Azure Resource Manager-Kennwort festlegen. Sie müssen dieses Kennwort festlegen, wenn Sie sich über den Azure Resource Manager mit den lokalen APIs des Geräts verbinden.

Das Verfahren zum Festlegen des Kennworts kann unterschiedlich sein, je nachdem, ob Sie das Azure-Portal oder die PowerShell-Cmdlets verwenden. Alle diese Verfahren werden in den folgenden Abschnitten beschrieben.


## <a name="reset-password-via-the-azure-portal"></a>Zurücksetzen des Kennworts im Azure-Portal

1. Wechseln Sie im Azure-Portal zur Azure Stack Edge-Ressource, die Sie zum Verwalten Ihres Geräts erstellt haben. Navigieren Sie zu **Edgecomputing > Erste Schritte**.

2. Wählen Sie im rechten Bereich auf der Befehlsleiste **EdgeARM-Kennwort zurücksetzen** aus. 

    ![EdgeARM-Kennwort zurücksetzen 1](media/azure-stack-edge-j-series-set-azure-resource-manager-password/set-edgearm-password-1.png)

3. Geben Sie auf dem Blatt **EdgeARM-Kennwort zurücksetzen** ein Kennwort an, um über den Azure Resource Manager eine Verbindung mit den lokalen APIs auf Ihrem Gerät herzustellen. Bestätigen Sie das Kennwort, und wählen Sie **Zurücksetzen** aus.

    ![EdgeARM-Kennwort zurücksetzen 2](media/azure-stack-edge-j-series-set-azure-resource-manager-password/set-edgearm-password-2.png)



## <a name="reset-password-via-powershell"></a>Zurücksetzen des Kennworts über PowerShell

1. Wechseln Sie im Azure-Portal zur Azure Stack Edge-Ressource, die Sie zum Verwalten Ihres Geräts erstellt haben. Notieren Sie sich auf der Seite **Übersicht** die folgenden Parameter.

    - Name der Azure Stack Edge-Ressource
    - Abonnement-ID

2. Wechseln Sie zu **Einstellungen > Eigenschaften**. Notieren Sie sich auf der Seite **Eigenschaften** die folgenden Parameter.

    - Resource group
    - CIK-Verschlüsselungsschlüssel: Wählen Sie „Anzeigen“ aus, und kopieren Sie dann den **Verschlüsselungsschlüssel**.

    ![CIK-Verschlüsselungsschlüssel abrufen](media/azure-stack-edge-j-series-set-azure-resource-manager-password/get-cik-portal.png)
 
3. Bestimmen Sie ein Kennwort, mit dem Sie eine Verbindung mit Azure Resource Manager herstellen können.

4. Starten Sie die Cloud Shell. Wählen Sie rechts oben das Symbol aus:

    ![Cloud Shell starten](media/azure-stack-edge-j-series-set-azure-resource-manager-password/start-cloud-shell.png) 

    Nachdem Sie die Cloud Shell gestartet haben, müssen Sie möglicherweise zu PowerShell wechseln.

    ![Cloud Shell](media/azure-stack-edge-j-series-set-azure-resource-manager-password/cloud-shell.png)   


5. Legen Sie den Kontext fest. Typ:

    `Set-AzContext -SubscriptionId <Subscription ID>`

    Hier ist eine Beispielausgabe:

    
    ```azurepowershell
    PS Azure:\> Set-AzContext -SubscriptionId 8eb87630-972c-4c36-a270-f330e6c063df
    
        Name        Account   SubscriptionName   Environment  TenantId
        ----       -------    ----------------   -----------  --------
        DataBox_Edge_Test (8eb87630-972c-4c36-a… MSI@50342 DataBox_Edge_Tes AzureCloud           72f988bf-86f1-41af-91ab-2d7…
    
        PS Azure:/
    ```
    
5.  Wenn Sie über alte PS-Module verfügen, müssen Sie diese installieren.

    `Remove-Module  Az.DataBoxEdge -force`

    Hier ist eine Beispielausgabe. Bei diesem Beispiel waren keine alten zu installierenden Module vorhanden.

    
    ```azurepowershell
        PS Azure:\> Remove-Module  Az.DataBoxEdge -force
        Remove-Module : No modules were removed. Verify that the specification of modules to remove is correct and those modules exist in the runspace.
        At line:1 char:1
        + Remove-Module  Az.DataBoxEdge -force
        + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : ResourceUnavailable: (:) [Remove-Module], InvalidOperationException
        + FullyQualifiedErrorId : Modules_NoModulesRemoved,Microsoft.PowerShell.Commands.RemoveModuleCommand
    
        PS Azure:\
    ```

6. Mit dem nächsten Satz von Befehlen wird ein Skript zum Installieren von PowerShell-Modulen heruntergeladen und ausgeführt.
    
    ```azurepowershell
        cd ~/clouddrive
        wget https://aka.ms/dbe-cmdlet-beta -O Az.DataBoxEdge.zip
        unzip ./Az.DataBoxEdge.zip
        Import-Module ~/clouddrive/Az.DataBoxEdge/Az.DataBoxEdge.psd1 -Force
    ```

7. Im nächsten Satz von Befehlen müssen Sie den Ressourcennamen, Ressourcengruppennamen, Verschlüsselungsschlüssel und das Kennwort angeben, das Sie im vorherigen Schritt bestimmt haben.

    ```azurepowershell
    $devicename = "<Azure Stack Edge resource name>"
    $resourceGroup = "<Resource group name>"
    $cik = "<Encryption key>"
    $password = "<Password>"
    ```
    Die Parameter für Kennwort und Verschlüsselungsschlüssel müssen als sichere Zeichenfolgen übergeben werden. Mit den folgenden Cmdlets können Sie Kennwort und Verschlüsselungsschlüssel in sichere Zeichenfolgen konvertieren.

    ```azurepowershell
    $pass = ConvertTo-SecureString $password -AsPlainText -Force
    $key = ConvertTo-SecureString $cik -AsPlainText -Force
    ```
    Nutzen Sie die zuvor generierten sicheren Zeichenfolgen als Parameter im Cmdlet Set-AzDataBoxEdgeUser, um das Kennwort zurückzusetzen. Verwenden Sie die Ressourcengruppe, die Sie zum Erstellen der Azure Stack Edge Pro-/Data Box Gateway-Ressource verwendet haben.

    ```azurepowershell
    Set-AzDataBoxEdgeUser -ResourceGroupName $resourceGroup -DeviceName $devicename -Name EdgeARMUser  -Password $pass -EncryptionKey $key
    ```
    Hier ist die Beispielausgabe.
    
    ```azurepowershell
    PS /home/aseuser/clouddrive> $devicename = "myaseresource"
    PS /home/aseuser/clouddrive> $resourceGroup = "myaserg"
    PS /home/aseuser/clouddrive> $cik = "54a7450fd7b3c506e10efea4e0c88a9390f37e299fbf43e01fb5dfe483ac036b6d0f85a6246e1926e297f98c0ff84c20a57348c689eff179ce31571fc787ac0a"
    PS /home/aseuser/clouddrive> $password = "Password2"
    PS /home/aseuser/clouddrive> $pass = ConvertTo-SecureString $password -AsPlainText -Force
    PS /home/aseuser/clouddrive> $key = ConvertTo-SecureString $cik -AsPlainText -Force
    PS /home/aseuser/clouddrive> Set-AzDataBoxEdgeUser -ResourceGroupName $resourceGroup -DeviceName $devicename -Name EdgeARMUser  -Password $pass -EncryptionKey $key
    
        User name   Type ResourceGroupName DeviceName
    ---------   ---- ----------------- ----------
        EdgeARMUser ARM  myaserg        myaseresource
    
        PS /home/aseuser/clouddrive>
    ```
Verwenden Sie das neue Kennwort, um eine Verbindung mit Azure Resource Manager herzustellen.

## <a name="next-steps"></a>Nächste Schritte

[Herstellen einer Verbindung mit Azure Resource Manager](azure-stack-edge-j-series-connect-resource-manager.md)
