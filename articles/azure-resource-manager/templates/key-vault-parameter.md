---
title: Key Vault-Geheimnis mit Vorlage
description: Informationen zum Übergeben eines geheimen Schlüssels aus einem Schlüsseltresor als Parameter während der Bereitstellung.
ms.topic: conceptual
ms.date: 12/17/2020
ms.openlocfilehash: 05749fe2e9179051c3183ea2e592cf7190ddb347
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104889857"
---
# <a name="use-azure-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Verwenden von Azure Key Vault zum Übergeben eines sicheren Parameterwerts während der Bereitstellung

Anstatt einen sicheren Wert (wie ein Kennwort) direkt in Ihre Vorlage oder Parameterdatei einzufügen, können Sie den Wert während einer Bereitstellung aus einem [Azure Key Vault](../../key-vault/general/overview.md) abrufen. Sie rufen den Wert ab, indem Sie den Schlüsseltresor und das Geheimnis in Ihrer Parameterdatei angeben. Der Wert wird nie offengelegt, da Sie nur auf die Schlüsseltresor-ID verweisen. Der Schlüsseltresor kann in einem anderen Abonnement als die Ressourcengruppe vorhanden sein, für die Sie ihn bereitstellen.

Dieser Artikel konzentriert sich auf das Szenario, bei dem ein sensibler Wert als Vorlagenparameter zur Eingabe übergeben wird. Er behandelt nicht das Szenario, in dem eine Eigenschaft eines virtuellen Computers auf die URL eines Zertifikats in einem Schlüsseltresor festgelegt wird. Eine Schnellstartvorlage für dieses Szenario finden Sie unter [Installieren eines Zertifikats aus Azure Key Vault auf einem virtuellen Computer](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows).

## <a name="deploy-key-vaults-and-secrets"></a>Bereitstellen von Schlüsseltresoren und Geheimnissen

Um während der Vorlagenbereitstellung auf einen Schlüsseltresor zuzugreifen, legen Sie `enabledForTemplateDeployment` für den Schlüsseltresor auf `true` fest.

Wenn Sie bereits über einen Schlüsseltresor verfügen, stellen Sie sicher, dass er Vorlagenbereitstellungen zulässt.

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az keyvault update  --name ExampleVault --enabled-for-template-deployment true
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Set-AzKeyVaultAccessPolicy -VaultName ExampleVault -EnabledForTemplateDeployment
```

---

Verwenden Sie zum Erstellen eines neuen Schlüsseltresors Folgendes:

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az group create --name ExampleGroup --location centralus
az keyvault create \
  --name ExampleVault \
  --resource-group ExampleGroup \
  --location centralus \
  --enabled-for-template-deployment true
az keyvault secret set --vault-name ExampleVault --name "ExamplePassword" --value "hVFkk965BuUv"
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
New-AzResourceGroup -Name ExampleGroup -Location centralus
New-AzKeyVault `
  -VaultName ExampleVault `
  -resourceGroupName ExampleGroup `
  -Location centralus `
  -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString 'hVFkk965BuUv' -AsPlainText -Force
$secret = Set-AzKeyVaultSecret -VaultName ExampleVault -Name 'ExamplePassword' -SecretValue $secretvalue
```

---

Als Besitzer des Schlüsseltresors haben Sie automatisch Zugriff auf die Erstellung von Geheimnissen. Wenn der Benutzer, der mit Geheimnissen arbeitet, nicht der Besitzer des Schlüsseltresors ist, gewähren Sie den Zugriff mit:

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az keyvault set-policy \
  --upn <user-principal-name> \
  --name ExampleVault \
  --secret-permissions set delete get list
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
$userPrincipalName = "<Email Address of the deployment operator>"

Set-AzKeyVaultAccessPolicy `
  -VaultName ExampleVault `
  -UserPrincipalName <user-principal-name> `
  -PermissionsToSecrets set,delete,get,list
```

---

Weitere Informationen zum Erstellen von Schlüsseltresoren und zum Hinzufügen von Geheimnissen finden Sie unter:

- [Festlegen und Abrufen eines Geheimnisses über die Befehlszeilenschnittstelle](../../key-vault/secrets/quick-create-cli.md)
- [Festlegen und Abrufen eines Geheimnisses mit PowerShell](../../key-vault/secrets/quick-create-powershell.md)
- [Festlegen und Abrufen eines Geheimnisses über das Portal](../../key-vault/secrets/quick-create-portal.md)
- [Festlegen und Abrufen eines Geheimnisses mit .NET](../../key-vault/secrets/quick-create-net.md)
- [Festlegen und Abrufen eines Geheimnisses mit Node.js](../../key-vault/secrets/quick-create-node.md)

## <a name="grant-access-to-the-secrets"></a>Gewähren des Zugriffs auf die Geheimnisse

Der Benutzer, der die Vorlage bereitstellt, muss die Berechtigung `Microsoft.KeyVault/vaults/deploy/action` für den Bereich der Ressourcengruppe und des Schlüsseltresors besitzen. Die Rollen [Besitzer](../../role-based-access-control/built-in-roles.md#owner) und [Mitwirkender](../../role-based-access-control/built-in-roles.md#contributor) gewähren diesen Zugriff. Wenn Sie den Schlüsseltresor erstellt haben, sind Sie der Besitzer und verfügen somit über die Berechtigung.

Die folgende Prozedur zeigt das Erstellen einer Rolle mit der Mindestberechtigung und das Zuweisen des Benutzers

1. Erstellen einer benutzerdefinierten Rollendefinition (JSON-Datei):

    ```json
    {
      "Name": "Key Vault resource manager template deployment operator",
      "IsCustom": true,
      "Description": "Lets you deploy a resource manager template with the access to the secrets in the Key Vault.",
      "Actions": [
        "Microsoft.KeyVault/vaults/deploy/action"
      ],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/subscriptions/00000000-0000-0000-0000-000000000000"
      ]
    }
    ```
    Ersetzen Sie „00000000-0000-0000-0000-000000000000“ durch die Abonnement-ID.

2. Erstellen Sie die neue Rolle mithilfe der JSON-Datei:

    # <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

    ```azurecli-interactive
    az role definition create --role-definition "<path-to-role-file>"
    az role assignment create \
      --role "Key Vault resource manager template deployment operator" \
      --assignee <user-principal-name> \
      --resource-group ExampleGroup
    ```

    # <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

    ```azurepowershell-interactive
    New-AzRoleDefinition -InputFile "<path-to-role-file>"
    New-AzRoleAssignment `
      -ResourceGroupName ExampleGroup `
      -RoleDefinitionName "Key Vault resource manager template deployment operator" `
      -SignInName <user-principal-name>
    ```

    ---

    Die Beispiele weisen dem Benutzer auf Ressourcengruppenebene die benutzerdefinierte Rolle zu.

Wenn ein Schlüsseltresor zusammen mit der Vorlage für eine [verwaltete Anwendung](../managed-applications/overview.md) verwendet wird, müssen Sie Zugriff auf den Dienstprinzipal des **Ressourcenanbieters der Appliance** erteilen. Weitere Informationen finden Sie unter [Zugreifen auf das Geheimnis im Schlüsseltresor bei der Bereitstellung von Azure Managed Applications](../managed-applications/key-vault-access.md).

## <a name="reference-secrets-with-static-id"></a>Verweisen auf Geheimnisse mit einer statischen ID

Bei dieser Herangehensweise verweisen Sie auf den Schlüsseltresor in der Parameterdatei, nicht in der Vorlage. Die folgende Abbildung zeigt, wie die Parameterdatei auf das Geheimnis verweist und diesen Wert an die Vorlage übergibt.

![Diagramm der Resource Manager-Integration des Schlüsseltresors mit statischer ID](./media/key-vault-parameter/statickeyvault.png)

[Tutorial: Integrieren von Azure Key Vault in die Resource Manager-Vorlagenbereitstellung](./template-tutorial-use-key-vault.md) verwendet diese Methode.

Die folgende Vorlage stellt einen SQL-Server bereit, der ein Administratorkennwort enthält. Der Kennwortparameter ist auf eine sichere Zeichenfolge festgelegt. Aber die Vorlage gibt nicht an, woher dieser Wert stammt.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminLogin": {
      "type": "string"
    },
    "adminPassword": {
      "type": "securestring"
    },
    "sqlServerName": {
      "type": "string"
    }
  },
  "resources": [
    {
      "type": "Microsoft.Sql/servers",
      "apiVersion": "2015-05-01-preview",
      "name": "[parameters('sqlServerName')]",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
        "administratorLogin": "[parameters('adminLogin')]",
        "administratorLoginPassword": "[parameters('adminPassword')]",
        "version": "12.0"
      }
    }
  ],
  "outputs": {
  }
}
```

Erstellen Sie jetzt eine Parameterdatei für der vorherige Vorlage. Geben Sie in der Parameterdatei einen Parameter an, der dem Namen des Parameters in der Vorlage entspricht. Verweisen Sie für den Parameterwert auf das Geheimnis aus dem Schlüsseltresor. Sie verweisen auf den geheimen Schlüssel, indem Sie den Ressourcenbezeichner des Schlüsseltresors und den Namen des Geheimnisses übergeben:

In der folgenden Parameterdatei muss das Schlüsseltresorgeheimnis bereits vorhanden sein, und Sie geben einen statischen Wert für seine Ressourcen-ID an.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "adminLogin": {
        "value": "exampleadmin"
      },
      "adminPassword": {
        "reference": {
          "keyVault": {
          "id": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.KeyVault/vaults/<vault-name>"
          },
          "secretName": "ExamplePassword"
        }
      },
      "sqlServerName": {
        "value": "<your-server-name>"
      }
  }
}
```

Wenn Sie den geheimen Schlüssel in einer Version verwenden müssen, die nicht die aktuelle Version ist, verwenden Sie die `secretVersion`-Eigenschaft.

```json
"secretName": "ExamplePassword",
"secretVersion": "cd91b2b7e10e492ebb870a6ee0591b68"
```

Stellen Sie die Vorlage bereit, und übergeben Sie sie in der Parameterdatei:

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

```azurecli-interactive
az group create --name SqlGroup --location westus2
az deployment group create \
  --resource-group SqlGroup \
  --template-uri <template-file-URI> \
  --parameters <parameter-file>
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
  -ResourceGroupName $resourceGroupName `
  -TemplateUri <template-file-URI> `
  -TemplateParameterFile <parameter-file>
```

---

## <a name="reference-secrets-with-dynamic-id"></a>Verweisen auf Geheimnisse mit einer dynamischen ID

Im vorherigen Abschnitt wurde für das Geheimnis des Schlüsseltresors eine statische Ressourcen-ID aus dem Parameter übergeben. Manchmal muss jedoch auf einen geheimen Schlüsseltresorschlüssel verwiesen werden, der je nach aktueller Bereitstellung variiert. Oder Sie möchten Parameterwerte an die Vorlage übergeben, statt einen Referenzparameter in der Parameterdatei zu erstellen. In beiden Fällen können Sie die Ressourcen-ID für ein Schlüsseltresorgeheimnis mithilfe einer verknüpften Vorlage dynamisch erstellen.

Da in der Parameterdatei keine Vorlagenausdrücke zulässig sind, kann die Ressourcen-ID nicht dynamisch in der Parameterdatei generiert werden.

In Ihrer übergeordneten Vorlage fügen Sie die geschachtelte Vorlage hinzu und übergeben einen Parameter, der die dynamisch generierte Ressourcen-ID enthält. Die folgende Abbildung zeigt, wie ein Parameter in der verknüpften Vorlage auf das Geheimnis verweist.

![Dynamische ID](./media/key-vault-parameter/dynamickeyvault.png)

Die folgende Vorlage erstellt dynamisch die Schlüsseltresor-ID und übergibt sie als Parameter.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "location": {
        "type": "string",
        "defaultValue": "[resourceGroup().location]",
        "metadata": {
          "description": "The location where the resources will be deployed."
        }
      },
      "vaultName": {
        "type": "string",
        "metadata": {
          "description": "The name of the keyvault that contains the secret."
        }
      },
      "secretName": {
        "type": "string",
        "metadata": {
          "description": "The name of the secret."
        }
      },
      "vaultResourceGroupName": {
        "type": "string",
        "metadata": {
          "description": "The name of the resource group that contains the keyvault."
        }
      },
      "vaultSubscription": {
        "type": "string",
        "defaultValue": "[subscription().subscriptionId]",
        "metadata": {
          "description": "The name of the subscription that contains the keyvault."
        }
      }
  },
  "resources": [
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2020-10-01",
      "name": "dynamicSecret",
      "properties": {
        "mode": "Incremental",
        "expressionEvaluationOptions": {
          "scope": "inner"
        },
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminLogin": {
              "type": "string"
            },
            "adminPassword": {
              "type": "securestring"
            },
            "location": {
              "type": "string"
            }
          },
          "variables": {
            "sqlServerName": "[concat('sql-', uniqueString(resourceGroup().id, 'sql'))]"
          },
          "resources": [
            {
              "type": "Microsoft.Sql/servers",
              "apiVersion": "2018-06-01-preview",
              "name": "[variables('sqlServerName')]",
              "location": "[parameters('location')]",
              "properties": {
                "administratorLogin": "[parameters('adminLogin')]",
                "administratorLoginPassword": "[parameters('adminPassword')]"
              }
            }
          ],
          "outputs": {
            "sqlFQDN": {
              "type": "string",
              "value": "[reference(variables('sqlServerName')).fullyQualifiedDomainName]"
            }
          }
        },
        "parameters": {
          "location": {
            "value": "[parameters('location')]"
          },
          "adminLogin": {
            "value": "ghuser"
          },
          "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "[resourceId(parameters('vaultSubscription'), parameters('vaultResourceGroupName'), 'Microsoft.KeyVault/vaults', parameters('vaultName'))]"
              },
              "secretName": "[parameters('secretName')]"
            }
          }
        }
      }
    }
  ],
  "outputs": {
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

- Allgemeine Informationen zu Schlüsseltresoren finden Sie unter [Was ist Azure Key Vault?](../../key-vault/general/overview.md)
- Vollständige Beispiele für Verweise auf geheime Schlüssel finden Sie unter [Key Vault examples](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples)(in englischer Sprache).
- Ein Microsoft Learn-Modul, das das Übergeben eines sicheren Werts aus einem Schlüsseltresor behandelt, finden Sie unter [Verwalten komplexer Cloudbereitstellungen mithilfe erweiterter ARM-Vorlagenfunktionen](/learn/modules/manage-deployments-advanced-arm-template-features/).
