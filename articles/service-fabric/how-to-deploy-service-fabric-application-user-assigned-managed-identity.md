---
title: Bereitstellen einer App mit einer benutzerseitig zugewiesenen verwalteten Identität
description: In diesem Artikel wird das Bereitstellen einer Service Fabric-Anwendung mit einer benutzerseitig zugewiesenen verwalteten Identität beschrieben.
ms.topic: article
ms.date: 12/09/2019
ms.openlocfilehash: 79d8654733b580be96d59e78f31105077929ac78
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "86260092"
---
# <a name="deploy-service-fabric-application-with-a-user-assigned-managed-identity"></a>Bereitstellen einer Service Fabric-Anwendung mit einer benutzerseitig zugewiesenen verwalteten Identität

Zum Bereitstellen einer Service Fabric Anwendung mit verwalteter Identität muss die Anwendung über Azure Resource Manager bereitgestellt werden – in der Regel mit einer Azure Resource Manager-Vorlage. Weitere Informationen zum Bereitstellen von Service Fabric Anwendung über Azure Resource Manager finden Sie unter [Verwalten von Anwendungen und Diensten als Azure Resource Manager-Ressourcen](service-fabric-application-arm-resource.md).

> [!NOTE] 
> 
> Anwendungen, die nicht als Azure-Ressource bereitgestellt werden, können **keine** verwalteten Identitäten besitzen. 
>
> Die Bereitstellung von Service Fabric-Anwendungen mit verwalteten Identitäten wird mit der API-Version `"2019-06-01-preview"` unterstützt. Sie können auch die gleiche API-Version für den Anwendungstyp, die Anwendungstypversion und die Dienstressourcen verwenden.
>

## <a name="user-assigned-identity"></a>Vom Benutzer zugewiesene Identität

Wenn Sie die Anwendung mit einer vom Benutzer zugewiesenen Identität aktivieren möchten, fügen Sie zunächst der Anwendungsressource mit dem Typ **userAssigned** und den referenzierten benutzerseitig zugewiesenen Identitäten die **identity**-Eigenschaft hinzu. Fügen Sie dann im Abschnitt **properties** für die **Anwendungsressource** einen Abschnitt **managedIdentities** ein, der eine Liste von Zuordnungen von Anzeigename zu principalId für jede vom Benutzer zugewiesenen Identität enthält. Weitere Informationen zu benutzerseitig zugewiesenen Identitäten finden Sie unter [Erstellen, Auflisten oder Löschen einer benutzerseitig zugewiesenen verwalteten Identität](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md).

### <a name="application-template"></a>Anwendungsvorlage

Wenn Sie die Anwendung mit einer vom Benutzer zugewiesenen Identität aktivieren möchten, fügen Sie zunächst der Anwendungsressource mit dem Typ **userAssigned** und den referenzierten vom Benutzer zugewiesenen Identitäten die **identity**-Eigenschaft hinzu. Fügen Sie dann ein **managedIdentities**-Objekt innerhalb des Abschnitts **properties** hinzu, das eine Liste der Zuordnungen von Anzeigename zu principalId für jede vom Benutzer zugewiesene Identität enthält.

```json
{
  "apiVersion": "2019-06-01-preview",
  "type": "Microsoft.ServiceFabric/clusters/applications",
  "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applicationTypes/', parameters('applicationTypeName'), '/versions/', parameters('applicationTypeVersion'))]",
    "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities/', parameters('userAssignedIdentityName'))]"
  ],
  "identity": {
    "type" : "userAssigned",
    "userAssignedIdentities": {
        "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities/', parameters('userAssignedIdentityName'))]": {}
    }
  },
  "properties": {
    "typeName": "[parameters('applicationTypeName')]",
    "typeVersion": "[parameters('applicationTypeVersion')]",
    "parameters": {
    },
    "managedIdentities": [
      {
        "name" : "[parameters('userAssignedIdentityName')]",
        "principalId" : "[reference(resourceId('Microsoft.ManagedIdentity/userAssignedIdentities/', parameters('userAssignedIdentityName')), '2018-11-30').principalId]"
      }
    ]
  }
}
```

Im Beispiel oben wird der Ressourcenname der vom Benutzer zugewiesenen Identität als Anzeigename der verwalteten Identität für die Anwendung verwendet. In den folgenden Beispielen wird davon ausgegangen, dass der tatsächliche Anzeigename „AdminUser“ lautet.

### <a name="application-package"></a>Anwendungspaket

1. Fügen Sie für jede Identität, die im Abschnitt `managedIdentities` der Azure Resource Manager-Vorlage definiert ist, im Anwendungsmanifest im Abschnitt **Principals** ein `<ManagedIdentity>`-Tag hinzu. Das `Name`-Attribut muss der im Abschnitt `managedIdentities` definierten `name`-Eigenschaft entsprechen.

    **ApplicationManifest.xml**

    ```xml
      <Principals>
        <ManagedIdentities>
          <ManagedIdentity Name="AdminUser" />
        </ManagedIdentities>
      </Principals>
    ```

2. Fügen Sie im Abschnitt **ServiceManifestImport** eine **IdentityBindingPolicy** für den Dienst hinzu, der die verwaltete Identität verwendet. Diese Richtlinie ordnet die Identität `AdminUser` einem dienstspezifischen Identitätsnamen zu, der später dem Dienstmanifest hinzugefügt werden muss.

    **ApplicationManifest.xml**

    ```xml
      <ServiceManifestImport>
        <Policies>
          <IdentityBindingPolicy ServiceIdentityRef="WebAdmin" ApplicationIdentityRef="AdminUser" />
        </Policies>
      </ServiceManifestImport>
    ```

3. Aktualisieren Sie das Dienstmanifest, um im Abschnitt **Resources** eine **ManagedIdentity** hinzuzufügen, deren Name dem `ServiceIdentityRef` in der `IdentityBindingPolicy` des Anwendungsmanifests entspricht:

    **ServiceManifest.xml**

    ```xml
      <Resources>
        ...
        <ManagedIdentities DefaultIdentity="WebAdmin">
          <ManagedIdentity Name="WebAdmin" />
        </ManagedIdentities>
      </Resources>
    ```

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden verwalteter Identitäten in Service Fabric-Anwendungscode](how-to-managed-identity-service-fabric-app-code.md)
* [Gewähren des Zugriffs auf andere Azure-Ressourcen für eine Service Fabric-Anwendung](how-to-grant-access-other-resources.md)
