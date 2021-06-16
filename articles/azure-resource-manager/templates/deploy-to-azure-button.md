---
title: Schaltfläche zum Bereitstellen in Azure
description: Verwenden Sie diese Schaltfläche, um Azure Resource Manager-Vorlagen aus einem GitHub-Repository bereitzustellen.
ms.topic: conceptual
ms.date: 05/05/2021
ms.openlocfilehash: 5074d21867e931560367a6ab62c46e5753f1b00e
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111957805"
---
# <a name="use-a-deployment-button-to-deploy-templates-from-github-repository"></a>Verwenden einer Bereitstellungsschaltfläche zum Bereitstellen von Vorlagen aus einem GitHub-Repository

In diesem Artikel wird beschrieben, wie Sie die Schaltfläche **In Azure bereitstellen** verwenden, um Vorlagen aus einem GitHub-Repository bereitzustellen. Sie können die Schaltfläche direkt der Datei _README.md_ in Ihrem GitHub-Repository hinzufügen. Oder Sie können die Schaltfläche zu einer Webseite hinzufügen, die auf das Repository verweist.

Der Bereitstellungsumfang wird durch das Vorlagenschema bestimmt. Weitere Informationen finden Sie unter

- [resource groups](deploy-to-resource-group.md)
- [subscriptions](deploy-to-subscription.md)
- [Verwaltungsgruppen](deploy-to-management-group.md)
- [tenants](deploy-to-tenant.md)

## <a name="use-common-image"></a>Verwenden eines einheitlichen Bilds

Verwenden Sie das folgende Bild, wenn Sie die Schaltfläche zu Ihrer Webseite oder Ihrem Repository hinzufügen:

```markdown
![Deploy to Azure](https://aka.ms/deploytoazurebutton)
```

```html
<img src="https://aka.ms/deploytoazurebutton"/>
```

Das Bild sieht so aus:

![Schaltfläche zum Bereitstellen in Azure](https://aka.ms/deploytoazurebutton)

## <a name="create-url-for-deploying-template"></a>Erstellen einer URL zum Bereitstellen einer Vorlage

Beginnen Sie mit der unformatierten URL für die Vorlage in Ihrem Repository, um die URL für Ihre Vorlage zu erstellen: Klicken Sie auf **Roh**, um die unformatierte URL anzuzeigen.

:::image type="content" source="./media/deploy-to-azure-button/select-raw.png" alt-text="Schaltfläche „Roh“ anklicken":::

Die URL hat folgendes Format:

```html
https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/quickstarts/microsoft.storage/storage-account-create/azuredeploy.json
```

Konvertieren Sie dann die URL in einen URL-codierten Wert. Sie können einen Onlineencoder verwenden oder einen Befehl ausführen. Das folgende PowerShell-Beispiel zeigt, wie ein Wert URL-codiert wird.

```powershell
$url = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/quickstarts/microsoft.storage/storage-account-create/azuredeploy.json"
[uri]::EscapeDataString($url)
```

Die Beispiel-URL weist mit URL-Codierung folgenden Wert auf.

```html
https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.storage%2Fstorage-account-create%2Fazuredeploy.json
```

Jeder Link beginnt mit der gleichen Basis-URL:

```html
https://portal.azure.com/#create/Microsoft.Template/uri/
```

Fügen Sie Ihren URL-codierten Link zur Vorlage am Ende der Basis-URL hinzu.

```html
https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.storage%2Fstorage-account-create%2Fazuredeploy.json
```

Damit ist die URL für Ihren Link vollständig.

[!INCLUDE [Deploy templates in private GitHub repo](../../../includes/resource-manager-private-github-repo-templates.md)]

Wenn Sie [Git mit Azure Repos](/azure/devops/repos/git/) anstelle eines GitHub-Repositorys verwenden, können Sie die Schaltfläche **In Azure bereitstellen** dennoch verwenden. Sorgen Sie dafür, dass es sich bei Ihrem Repository um ein öffentliches handelt. Verwenden Sie den [Items-Vorgang](/rest/api/azure/devops/git/items/get), um die Vorlage abzurufen. Ihre Anforderung sollte das folgende Format aufweisen:

```http
https://dev.azure.com/{organization-name}/{project-name}/_apis/git/repositories/{repository-name}/items?scopePath={url-encoded-path}&api-version=6.0
```

Codieren Sie diese Anforderungs-URL.

## <a name="create-deploy-to-azure-button"></a>Erstellen der Schaltfläche für die Bereitstellung in Azure

Fügen Sie zum Schluss Link und Bild zusammen.

Um die Schaltfläche über Markdown der Datei _README.md_ in Ihrem GitHub-Repository oder einer Webseite hinzuzufügen, verwenden Sie folgenden Befehl:

```markdown
[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.storage%2Fstorage-account-create%2Fazuredeploy.json)
```

Verwenden Sie für HTML folgenden Code:

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.storage%2Fstorage-account-create%2Fazuredeploy.json" target="_blank">
  <img src="https://aka.ms/deploytoazurebutton"/>
</a>
```

Bei Git mit einem Azure-Repository weist die Schaltfläche das folgende Format auf:

```markdown
[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fdev.azure.com%2Forgname%2Fprojectname%2F_apis%2Fgit%2Frepositories%2Freponame%2Fitems%3FscopePath%3D%2freponame%2fazuredeploy.json%26api-version%3D6.0)
```

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Um die vollständige Lösung zu testen, klicken Sie auf die folgende Schaltfläche:

[![In Azure bereitstellen](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.storage%2Fstorage-account-create%2Fazuredeploy.json)

Das Portal zeigt einen Bereich an, indem Sie ganz einfach Parameterwerte angeben können. Die Parameter sind mit den Standardwerten aus der Vorlage vorab ausgefüllt. Der in der Vorlage definierte Camel-Case-Parametername *storageAccountType* wird im Portal in eine durch Leerzeichen getrennte Zeichenfolge eingereiht.

![Verwenden des Portals für die Bereitstellung](./media/deploy-to-azure-button/portal.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Vorlagen finden Sie unter [Verstehen der Struktur und Syntax von ARM-Vorlagen](./syntax.md).