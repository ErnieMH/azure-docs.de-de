---
title: 'Erstellen eines Windows Virtual Desktop-Hostpools (klassisch) – PowerShell: Azure'
description: Informationen zum Erstellen eines Hostpools in Windows Virtual Desktop (klassisch) mit PowerShell-Cmdlets
author: Heidilohr
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: c035a7fbafe9b3a42fbd16e3f8377014010ddd49
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "88003542"
---
# <a name="create-a-host-pool-in-windows-virtual-desktop-classic-with-powershell"></a>Erstellen eines Hostpools in Windows Virtual Desktop (klassisch) mit PowerShell

>[!IMPORTANT]
>Dieser Inhalt gilt für den Windows Virtual Desktop-Dienst (klassisch), der keine Windows Virtual Desktop-Objekte in Azure Resource Manager unterstützt. Wenn Sie Windows Virtual Desktop-Objekte in Azure Resource Manager verwalten möchten, finden Sie weitere Informationen in [diesem Artikel](../create-host-pools-powershell.md).

Hostpools sind eine Sammlung identischer virtueller Computer innerhalb von Windows Virtual Desktop-Mandantenumgebungen. Jeder Hostpool kann eine App-Gruppe enthalten, mit der Benutzer genau wie auf einem physischen Desktop interagieren können.

## <a name="use-your-powershell-client-to-create-a-host-pool"></a>Verwenden Ihres PowerShell-Clients zum Erstellen eines Hostpools

Zunächst müssen Sie das [Windows Virtual Desktop-PowerShell-Modul herunterladen und importieren](/powershell/windows-virtual-desktop/overview/), um es in Ihrer PowerShell-Sitzung verwenden zu können.

Führen Sie das folgende Cmdlet aus, um sich an der Windows Virtual Desktop-Umgebung anzumelden:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Führen Sie als Nächstes dieses Cmdlet aus, um für Ihren Windows Virtual Desktop-Mandanten einen neuen Hostpool zu erstellen:

```powershell
New-RdsHostPool -TenantName <tenantname> -Name <hostpoolname>
```

Führen Sie das nächste Cmdlet aus, um ein Registrierungstoken zum Autorisieren eines Sitzungshosts zu erstellen und den Hostpool einzubinden und in einer neuen Datei auf Ihrem lokalen Computer zu speichern. Sie können mit dem Parameter „-ExpirationHours“ angeben, wie lange das Registrierungstoken gültig ist.

```powershell
New-RdsRegistrationInfo -TenantName <tenantname> -HostPoolName <hostpoolname> -ExpirationHours <number of hours> | Select-Object -ExpandProperty Token | Out-File -FilePath <PathToRegFile>
```

Führen Sie anschließend dieses Cmdlet aus, um Azure Active Directory-Benutzer der Desktop-App-Standardgruppe für den Hostpool hinzuzufügen.

```powershell
Add-RdsAppGroupUser -TenantName <tenantname> -HostPoolName <hostpoolname> -AppGroupName "Desktop Application Group" -UserPrincipalName <userupn>
```

Das Cmdlet **Add-RdsAppGroupUser** weist keine Unterstützung für das Hinzufügen von Sicherheitsgruppen auf und fügt der App-Gruppe nur jeweils einen Benutzer hinzu. Falls Sie der App-Gruppe mehrere Benutzer hinzufügen möchten, können Sie das Cmdlet mit den passenden Benutzerprinzipalnamen erneut ausführen.

Führen Sie das folgende Cmdlet aus, um das Registrierungstoken in eine Variable zu exportieren. Sie verwenden sie später unter [Registrieren der virtuellen Computer unter dem Windows Virtual Desktop-Hostpool (Vorschauversion)](#register-the-virtual-machines-to-the-windows-virtual-desktop-host-pool).

```powershell
$token = (Export-RdsRegistrationInfo -TenantName <tenantname> -HostPoolName <hostpoolname>).Token
```

## <a name="create-virtual-machines-for-the-host-pool"></a>Erstellen von virtuellen Computern für den Hostpool

Sie können jetzt einen virtuellen Azure-Computer erstellen, der in Ihren Windows Virtual Desktop-Hostpool eingebunden werden kann.

Sie können einen virtuellen Computer auf unterschiedliche Arten erstellen:

- [Erstellen eines virtuellen Computers aus einem Azure-Katalogimage](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Erstellen eines virtuellen Computers aus einem verwalteten Image](../../virtual-machines/windows/create-vm-generalized-managed.md)
- [Erstellen eines virtuellen Computers aus einem nicht verwalteten Image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

>[!NOTE]
>Wenn Sie einen virtuellen Computer mit Windows 7 als Hostbetriebssystem bereitstellen, ist der Erstellungs- und Bereitstellungsprozess ein wenig anders. Weitere Informationen finden Sie unter [Bereitstellen eines virtuellen Windows 7-Computers in Windows Virtual Desktop](deploy-windows-7-virtual-machine.md).

Nachdem Sie Ihre virtuellen Sitzungshostcomputer erstellt haben, [wenden Sie eine Windows-Lizenz auf einen virtuellen Sitzungshostcomputer an](../apply-windows-license.md#apply-a-windows-license-to-a-session-host-vm), um Ihre virtuellen Windows- oder Windows Server-Computer auszuführen, ohne für eine weitere Lizenz zu bezahlen.

## <a name="prepare-the-virtual-machines-for-windows-virtual-desktop-agent-installations"></a>Vorbereiten der virtuellen Computer für Installationen von Windows Virtual Desktop-Agents

Sie müssen die folgenden Schritte zum Vorbereiten Ihrer virtuellen Computer ausführen, bevor Sie die Windows Virtual Desktop-Agents installieren und die virtuellen Computer für Ihren Windows Virtual Desktop-Hostpool registrieren können:

- Sie müssen für den Computer den Domänenbeitritt durchführen. Auf diese Weise können eingehende Windows Virtual Desktop-Benutzer von ihrem Azure Active Directory-Konto dem Active Directory-Konto zugeordnet werden, und ihnen kann der Zugriff auf den virtuellen Computer erfolgreich gewährt werden.
- Sie müssen die Rolle „Remotedesktop-Sitzungshost (RDSH)“ installieren, wenn auf dem virtuellen Computer ein Windows Server-Betriebssystem ausgeführt wird. Mit der RDSH-Rolle können die Windows Virtual Desktop-Agents richtig installiert werden.

Führen Sie auf jedem virtuellen Computer die folgenden Schritte aus, damit der Domänenbeitritt erfolgreich ist:

1. [Stellen Sie eine Verbindung mit dem virtuellen Computer her](../../virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine), indem Sie die Anmeldeinformationen verwenden, die Sie beim Erstellen des virtuellen Computers angegeben haben.
2. Starten Sie auf dem virtuellen Computer die **Systemsteuerung**, und wählen Sie **System**.
3. Wählen Sie **Computername**, **Einstellungen ändern** und dann **Ändern...** .
4. Wählen Sie **Domäne**, und geben Sie dann die Active Directory-Domäne im virtuellen Netzwerk ein.
5. Authentifizieren Sie sich mit einem Domänenkonto, das über Berechtigungen für in die Domäne eingebundene Computer verfügt.

    >[!NOTE]
    > Wenn Sie Ihre VMs mit einer Azure Active Directory Domain Services (Azure AD DS)-Umgebung verknüpfen, stellen Sie sicher, dass Ihre Domänenbeitrittsbenutzer ebenfalls Mitglied der [Gruppe „AAD DC-Administratoren“](../../active-directory-domain-services/tutorial-create-instance-advanced.md#configure-an-administrative-group) ist.

## <a name="register-the-virtual-machines-to-the-windows-virtual-desktop-host-pool"></a>Registrieren der virtuellen Computer für den Windows Virtual Desktop-Hostpool

Das Registrieren der virtuellen Computer für einen Windows Virtual Desktop-Hostpool ist so einfach wie das Installieren der Windows Virtual Desktop-Agents.

Führen Sie auf jedem virtuellen Computer die folgenden Schritte aus, um die Windows Virtual Desktop-Agents zu registrieren:

1. [Stellen Sie eine Verbindung mit dem virtuellen Computer her](../../virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine), indem Sie die Anmeldeinformationen verwenden, die Sie beim Erstellen des virtuellen Computers angegeben haben.
2. Führen Sie den Download und die Installation des Windows Virtual Desktop-Agents durch.
   - Laden Sie den [Windows Virtual Desktop-Agent](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrmXv) herunter.
   - Klicken Sie mit der rechten Maustaste auf das heruntergeladene Installationsprogramm, und wählen Sie **Eigenschaften**, **Blockierung aufheben** und dann **OK**. So wird festgelegt, dass Ihr System dem Installationsprogramm vertraut.
   - Führen Sie das Installationsprogramm aus. Wenn Sie vom Installationsprogramm nach dem Registrierungstoken gefragt werden, können Sie den Wert eingeben, den Sie über das Cmdlet **Export-RdsRegistrationInfo** erhalten haben.
3. Führen Sie den Download und die Installation des Bootloaders für Windows Virtual Desktop-Agents durch.
   - Führen Sie den Download des [Bootloaders für Windows Virtual Desktop-Agents](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrxrH) durch.
   - Klicken Sie mit der rechten Maustaste auf das heruntergeladene Installationsprogramm, und wählen Sie **Eigenschaften**, **Blockierung aufheben** und dann **OK**. So wird festgelegt, dass Ihr System dem Installationsprogramm vertraut.
   - Führen Sie das Installationsprogramm aus.

>[!IMPORTANT]
>Zum Schutz Ihrer Windows Virtual Desktop-Umgebung in Azure empfiehlt es sich, den eingehenden Port 3389 auf Ihren virtuellen Computern nicht zu öffnen. Für Windows Virtual Desktop muss der eingehende Port 3389 nicht geöffnet sein, damit Benutzer auf die virtuellen Computer des Hostpools zugreifen können. Wenn Sie den Port 3389 zur Problembehandlung öffnen müssen, verwenden Sie am besten den [Just-In-Time-Zugriff auf virtuelle Computer](../../security-center/security-center-just-in-time.md).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun einen Hostpool erstellt haben, können Sie ihn mit RemoteApps füllen. Weitere Informationen zum Verwalten von Apps in Windows Virtual Desktop finden Sie im folgenden Tutorial:

> [!div class="nextstepaction"]
> [Manage app groups for Windows Virtual Desktop Preview](../manage-app-groups.md) (Verwalten von App-Gruppen für Windows Virtual Desktop (Vorschauversion))
