---
title: 'Schnellstart: Erstellen Ihrer ersten statischen Web-App mit Azure Static Web Apps und der Azure CLI'
description: Hier wird beschrieben, wie Sie mit der Azure Static Web Apps-Befehlszeilenschnittstelle eine Instanz von Azure Static Web Apps erstellen.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: quickstart
ms.date: 08/13/2020
ms.author: cshoe
ms.openlocfilehash: 7e0fdbc50dd36e4ea23903a5929735c1c83bd394
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "88752828"
---
# <a name="quickstart-building-your-first-static-web-app-using-the-azure-cli"></a>Schnellstart: Erstellen Ihrer ersten statischen Web-App mithilfe der Azure CLI

Azure Static Web Apps veröffentlicht eine Website in einer Produktionsumgebung, indem Apps aus einem GitHub-Repository erstellt werden. In dieser Schnellstartanleitung stellen Sie über die Azure CLI eine Web-Anwendung in Azure Static Web Apps bereit.

Falls Sie noch nicht über ein Azure-Abonnement verfügen, können Sie ein [kostenloses Testkonto](https://azure.microsoft.com/free) erstellen.

## <a name="prerequisites"></a>Voraussetzungen

- [GitHub](https://github.com) -Konto
- [Persönliches GitHub-Zugriffstoken](https://docs.github.com/github/authenticating-to-github/creating-a-personal-access-token)
- [Azure](https://portal.azure.com)-Konto
- Installation der [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) (mindestens Version 2.8.0)

[!INCLUDE [create repository from template](../../includes/static-web-apps-get-started-create-repo.md)]

[!INCLUDE [clone the repository](../../includes/static-web-apps-get-started-clone-repo.md)]

Wechseln Sie anschließend mit dem folgenden Befehl in den neuen Ordner:

```bash
cd my-first-static-web-app
```

## <a name="create-a-static-web-app"></a>Erstellen einer statischen Web-App

Nachdem das Repository erstellt wurde, können Sie nun über die Azure CLI eine statische Web-App erstellen.

> [!IMPORTANT]
> Vergewissern Sie sich, dass Sie sich im Terminal im Ordner _my-first-static-web-app_ befinden.

1. Melden Sie sich mit dem folgenden Befehl bei der Azure CLI an:

    ```bash
    az login
    ```

1. Erstellen Sie eine neue statische Web-App aus Ihrem Repository:

    # <a name="no-framework"></a>[Kein Framework](#tab/vanilla-javascript)

    ```bash
    az staticwebapp create \
        -n my-first-static-web-app \
        -g <RESOURCE_GROUP_NAME> \
        -s https://github.com/<YOUR_GITHUB_ACCOUNT_NAME>/my-first-static-web-app \
        -l <LOCATION> \
        -b master \
        --token <YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>
    ```

    # <a name="angular"></a>[Angular](#tab/angular)

    ```bash
    az staticwebapp create \
        -n my-first-static-web-app \
        -g <RESOURCE_GROUP_NAME> \
        -s https://github.com/<YOUR_GITHUB_ACCOUNT_NAME>/my-first-static-web-app \
        -l <LOCATION> \
        -b master \
        --app-artifact-location "dist/angular-basic" \
        --token <YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>
    ```

    # <a name="react"></a>[React](#tab/react)

    ```bash
    az staticwebapp create \
        -n my-first-static-web-app \
        -g <RESOURCE_GROUP_NAME> \
        -s https://github.com/<YOUR_GITHUB_ACCOUNT_NAME>/my-first-static-web-app \
        -l <LOCATION> \
        -b master \
        --app-artifact-location "build" \
        --token <YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>
    ```

    # <a name="vue"></a>[Vue](#tab/vue)

    ```bash
    az staticwebapp create \
        -n my-first-static-web-app \
        -g <RESOURCE_GROUP_NAME> \
        -s https://github.com/<YOUR_GITHUB_ACCOUNT_NAME>/my-first-static-web-app \
        -l <LOCATION> \
        -b master \
        --app-artifact-location "dist" \
        --token <YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>
    ```

    ---

    - `<RESOURCE_GROUP_NAME>`: Ersetzen Sie diesen Wert durch einen vorhandenen Azure-Ressourcengruppennamen.

    - `<YOUR_GITHUB_ACCOUNT_NAME>`: Ersetzen Sie diesen Wert durch den GitHub-Benutzernamen.

    - `<LOCATION>`: Ersetzen Sie diesen Wert durch den nächstgelegenen Standort. Beispiele für Optionen: _CentralUS_, _EastAsia_, _EastUS2_, _WestEurope_ und _WestUS2_

    - `<YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>`: Ersetzen Sie diesen Wert durch das zuvor generierte [persönliche GitHub-Zugriffstoken](https://docs.github.com/github/authenticating-to-github/creating-a-personal-access-token).

    Sie können jetzt die erstellte App in Azure anzeigen.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).

1. Suchen Sie über die obere Suchleiste nach **my-first-web-static-app**.

1. Wählen Sie **my-first-web-static-app** aus.

[!INCLUDE [view website](../../includes/static-web-apps-get-started-view-website.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht weiter nutzen möchten, können Sie die Azure Static Web Apps-Instanz mit dem folgenden Befehl löschen:

```bash
az staticwebapp delete \
    --name my-first-static-web-app
    --resource-group my-first-static-web-app
```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Hinzufügen einer API zu Azure Static Web Apps (Vorschauversion) mit Azure Functions](add-api.md)
