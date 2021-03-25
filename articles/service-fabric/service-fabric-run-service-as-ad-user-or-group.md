---
title: Ausführen eines Azure Service Fabric-Diensts als AD-Benutzer oder -Gruppe
description: Hier erfahren Sie, wie Sie einen Dienst als Active Directory-Benutzer oder -Gruppe auf einem eigenständigen, Windows-basierten Service Fabric-Cluster ausführen.
ms.topic: conceptual
ms.date: 03/29/2018
ms.openlocfilehash: d4a7afc2ddb0f39014a7cf0fd006d7fe23673a95
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91840726"
---
# <a name="run-a-service-as-an-active-directory-user-or-group"></a>Ausführen eines Diensts als Active Directory-Benutzer oder -Gruppe
Auf einem eigenständigen Windows Server-Cluster können Sie einen Dienst unter Verwendung einer RunAs-Richtlinie als Active Directory-Benutzer oder -Gruppe ausführen.  Standardmäßig werden Service Fabric-Anwendungen unter dem Konto ausgeführt, unter dem der Prozess „Fabric.exe“ ausgeführt wird. Die Ausführung von Anwendungen unter verschiedenen Konten sorgt dafür, dass die Anwendungen besser voreinander geschützt sind – sogar in einer gehosteten Umgebung mit gemeinsamer Nutzung. Hinweis: Hierbei wird Active Directory lokal in Ihrer Domäne verwendet, nicht Azure Active Directory (Azure AD).  Sie können einen Dienst auch als [group Managed Service Account (gMSA) (gruppenverwaltetes Dienstkonto)](service-fabric-run-service-as-gmsa.md) ausführen.

Indem Sie einen Domänenbenutzer oder eine -gruppe verwenden, können Sie anschließend auf andere Ressourcen in der Domäne (z.B. Dateifreigaben) zugreifen, für die Berechtigungen gewährt wurden.

Im folgenden Beispiel wird ein Active Directory-Benutzer mit dem Namen *TestUser* verwendet, dessen Domänenkennwort mit dem Zertifikat *MyCert* verschlüsselt wird. Sie können den PowerShell-Befehl `Invoke-ServiceFabricEncryptText` verwenden, um den Verschlüsselungstext für den geheimen Schlüssel zu erstellen. Weitere Informationen finden Sie unter [Verwalten von Geheimnissen in Service Fabric-Anwendungen](service-fabric-application-secret-management.md).

Sie müssen den privaten Schlüssel des Zertifikats zum Entschlüsseln des Kennworts mit einer Out-of-Band-Methode auf dem lokalen Computer bereitstellen (in Azure erfolgt dies über Azure Resource Manager). Wenn Service Fabric das Dienstpaket dann auf dem Computer bereitstellt, kann Service Fabric den geheimen Schlüssel entschlüsseln und (zusammen mit dem Benutzernamen) zum Authentifizieren bei Active Directory verwenden, sodass die Ausführung mit diesen Anmeldeinformationen erfolgt.

```xml
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
```

> [!NOTE] 
> Wenn Sie eine RunAs-Richtlinie auf einen Dienst anwenden und das Dienstmanifest Endpunktressourcen mit dem HTTP-Protokoll deklariert, müssen Sie auch eine Richtlinie vom Typ **SecurityAccessPolicy** angeben.  Weitere Informationen finden Sie unter [Zuweisen einer Sicherheitszugriffsrichtlinie für HTTP- und HTTPS-Endpunkte](service-fabric-assign-policy-to-endpoint.md). 
>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
Lesen Sie als Nächstes die folgenden Artikel:
* [Informationen zum Anwendungsmodell](service-fabric-application-model.md)
* [Angeben von Ressourcen in einem Dienstmanifest](service-fabric-service-manifest-resources.md)
* [Bereitstellen von Anwendungen](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
