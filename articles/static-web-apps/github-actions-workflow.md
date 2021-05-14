---
title: GitHub Actions-Workflows für Azure Static Web Apps
description: Es wird beschrieben, wie Sie GitHub-Repositorys zum Einrichten von Continuous Deployment für Azure Static Web Apps verwenden.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: conceptual
ms.date: 04/09/2021
ms.author: cshoe
ms.openlocfilehash: b20a1670c13a272ed48088567a205d854ac99179
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/20/2021
ms.locfileid: "107791245"
---
# <a name="github-actions-workflows-for-azure-static-web-apps-preview"></a>GitHub Actions-Workflows für Azure Static Web Apps (Vorschau)

Wenn Sie eine neue Azure Static Web Apps-Ressource erstellen, generiert Azure einen GitHub Actions-Workflow, um die Continuous Deployment-Vorgänge für die App zu steuern. Der Workflow basiert auf einer YAML-Datei. In diesem Artikel werden die Struktur und die Optionen der Workflowdatei ausführlich erläutert.

Bereitstellungen werden durch [Trigger](#triggers) initiiert, von denen [Aufträge](#jobs) ausgeführt werden, die durch einzelne [Schritte](#steps) definiert sind.

> [!NOTE]
> Azure Static Web Apps unterstützt auch Azure DevOps. Informationen zum Einrichten einer Pipeline finden Sie unter [Tutorial: Veröffentlichen von Azure Static Web Apps mit Azure DevOps](publish-devops.md).

## <a name="file-location"></a>Dateispeicherort

Wenn Sie Ihr GitHub-Repository mit Azure Static Web Apps verknüpfen, wird dem Repository eine Workflowdatei hinzugefügt.

Führen Sie diese Schritte aus, um die generierte Workflowdatei anzuzeigen.

1. Öffnen Sie das Repository der App auf GitHub.
1. Klicken Sie auf der Registerkarte _Code_ auf den Ordner `.github/workflows`.
1. Klicken Sie auf die Datei mit einem Namen wie `azure-static-web-apps-<RANDOM_NAME>.yml`.

Die YAML-Datei in Ihrem Repository ähnelt dem folgenden Beispiel:

```yml
name: Azure Static Web Apps CI/CD

on:
  push:
    branches:
      - main
  pull_request:
    types: [opened, synchronize, reopened, closed]
    branches:
      - main

jobs:
  build_and_deploy_job:
    if: github.event_name == 'push' || (github.event_name == 'pull_request' && github.event.action != 'closed')
    runs-on: ubuntu-latest
    name: Build and Deploy Job
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true
      - name: Build And Deploy
        id: builddeploy
        uses: Azure/static-web-apps-deploy@v0.0.1-preview
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN_MANGO_RIVER_0AFDB141E }}
          repo_token: ${{ secrets.GITHUB_TOKEN }} # Used for GitHub integrations (i.e. PR comments)
          action: 'upload'
          ###### Repository/Build Configurations - These values can be configured to match you app requirements. ######
          app_location: '/' # App source code path
          api_location: 'api' # Api source code path - optional
          output_location: 'dist' # Built app content directory - optional
          ###### End of Repository/Build Configurations ######

  close_pull_request_job:
    if: github.event_name == 'pull_request' && github.event.action == 'closed'
    runs-on: ubuntu-latest
    name: Close Pull Request Job
    steps:
      - name: Close Pull Request
        id: closepullrequest
        uses: Azure/static-web-apps-deploy@v0.0.1-preview
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN_MANGO_RIVER_0AFDB141E }}
          action: 'close'
```

## <a name="triggers"></a>Trigger

Ein GitHub Actions-[Trigger](https://help.github.com/actions/reference/events-that-trigger-workflows) benachrichtigt einen GitHub Actions-Workflow darüber, dass basierend auf Ereignisauslösern ein Auftrag ausgeführt werden soll. Trigger werden mit der Eigenschaft `on` in der Workflowdatei aufgelistet.

```yml
on:
  push:
    branches:
      - main
  pull_request:
    types: [opened, synchronize, reopened, closed]
    branches:
      - main
```

Mit Einstellungen, die der Eigenschaft `on` zugeordnet sind, können Sie definieren, welche Branches einen Auftrag auslösen. Darüber hinaus können Sie Trigger festlegen, die für unterschiedliche Pull Request-Zustände ausgelöst werden.

In diesem Beispiel wird ein Workflow gestartet, wenn sich der Branch _main_ ändert. Zu den Änderungen, die zum Starten des Workflows führen, gehören das Pushen von Commits und das Öffnen von Pull Requests für den ausgewählten Branch.

## <a name="jobs"></a>Aufträge

Für jeden Ereignisauslöser wird ein Ereignishandler benötigt. Mit [Aufträgen](https://help.github.com/actions/reference/workflow-syntax-for-github-actions#jobs) wird definiert, was passiert, wenn ein Ereignis ausgelöst wird.

In der Workflowdatei für Static Web Apps sind zwei verfügbare Aufträge enthalten.

| Name                     | BESCHREIBUNG                                                                                                    |
| ------------------------ | -------------------------------------------------------------------------------------------------------------- |
| `build_and_deploy_job`   | Wird ausgeführt, wenn Sie Commits pushen oder einen Pull Request für den Branch öffnen, der in der Eigenschaft `on` aufgelistet ist.          |
| `close_pull_request_job` | Dieser Auftrag wird NUR ausgeführt, wenn Sie einen Pull Request schließen, der die aus den Pull Requests erstellte Stagingumgebung entfernt. |

## <a name="steps"></a>Schritte

Schritte sind sequenzielle Aufgaben für einen Auftrag. In einem Schritt werden Aktionen durchgeführt, z. B. das Installieren von Abhängigkeiten, Ausführen von Tests und Bereitstellen Ihrer Anwendung in der Produktion.

Mit einer Workflowdatei werden die folgenden Schritte definiert.

| Auftrag                      | Schritte                                                                                                                              |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| `build_and_deploy_job`   | <ol><li>Führt das Auschecken des Repositorys in der Umgebung der Aktion durch.<li>Erstellt das Repository für Azure Static Web Apps und führt die Bereitstellung dafür durch.</ol> |
| `close_pull_request_job` | <ol><li>Benachrichtigt Azure Static Web Apps darüber, dass ein Pull Request geschlossen wurde.</ol>                                                        |

## <a name="build-and-deploy"></a>Erstellen und Bereitstellen

Im Schritt mit dem Namen `Build and Deploy` wird Ihre Azure Static Web Apps-Instanz erstellt und die Bereitstellung dafür durchgeführt. Im Abschnitt `with` können Sie die folgenden Werte für Ihre Bereitstellung anpassen.

```yml
with:
  azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN_MANGO_RIVER_0AFDB141E }}
  repo_token: ${{ secrets.GITHUB_TOKEN }} # Used for GitHub integrations (i.e. PR comments)
  action: 'upload'
  ###### Repository/Build Configurations - These values can be configured to match you app requirements. ######
  app_location: '/' # App source code path
  api_location: 'api' # Api source code path - optional
  output_location: 'dist' # Built app content directory - optional
  ###### End of Repository/Build Configurations ######
```

[!INCLUDE [static-web-apps-folder-structure](../../includes/static-web-apps-folder-structure.md)]

Die Werte `repo_token`, `action` und `azure_static_web_apps_api_token` werden für Sie von Azure Static Web Apps festgelegt und sollten nicht manuell geändert werden.

## <a name="custom-build-commands"></a>Benutzerdefinierte Buildbefehle

Sie können präzise steuern, welche Befehle während einer Bereitstellung ausgeführt werden. Die folgenden Befehle können im Abschnitt `with` eines Auftrags definiert werden.

Für die Bereitstellung wird vor einem benutzerdefinierten Befehl immer `npm install` aufgerufen.

| Get-Help             | BESCHREIBUNG                                                                                                                                                                                                                                                                                                                                                                                |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `app_build_command` | Definiert einen benutzerdefinierten Befehl, der während der Bereitstellung der Anwendung für statischen Inhalt ausgeführt werden soll.<br><br>Wenn Sie beispielsweise einen Produktionsbuild für eine Angular-Anwendung konfigurieren möchten, erstellen Sie ein npm-Skript mit dem Namen `build-prod`, um `ng build --prod` auszuführen, und geben Sie `npm run build-prod` als benutzerdefinierten Befehl ein. Wenn Sie das Feld leer lassen, versucht der Workflow, den Befehl `npm run build` oder `npm run build:azure` auszuführen. |
| `api_build_command` | Definiert einen benutzerdefinierten Befehl, der während der Bereitstellung der Azure Functions-API-Anwendung ausgeführt werden soll.                                                                                                                                                                                                                                                                                                  |

## <a name="skip-app-build"></a>Überspringen der App-Erstellung

Wenn Sie vollständige Kontrolle darüber benötigen, wie Ihre Front-End-Anwendung erstellt wird, können Sie ihrem Workflow benutzerdefinierte Buildschritte hinzufügen. Anschließend können Sie die Static Web Apps-Aktion so konfigurieren, dass sie den automatischen Buildprozess umgeht und einfach die App bereitstellt, die in einem vorherigen Schritt erstellt wurde.

Um das Erstellen der App zu überspringen, legen Sie `skip_app_build` auf `true` und `app_location` auf den Speicherort des bereitzustellenden Ordners fest.

```yml
with:
  azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN_MANGO_RIVER_0AFDB141E }}
  repo_token: ${{ secrets.GITHUB_TOKEN }} # Used for GitHub integrations (i.e. PR comments)
  action: 'upload'
  ###### Repository/Build Configurations - These values can be configured to match you app requirements. ######
  app_location: 'dist' # Application build output generated by a previous step
  api_location: 'api' # Api source code path - optional
  output_location: '' # Leave this empty
  skip_app_build: true
  ###### End of Repository/Build Configurations ######
```

| Eigenschaft         | Beschreibung                                                 |
| ---------------- | ----------------------------------------------------------- |
| `skip_app_build` | Legen Sie den Wert auf `true` fest, um das Erstellen der Front-End-App zu überspringen. |

> [!NOTE]
> Sie können das Erstellen nur für die Front-End-App überspringen. Wenn Ihre App über eine API verfügt, wird sie weiterhin von der GitHub-Aktion von Static Web Apps erstellt.

## <a name="route-file-location"></a>Speicherort der Routendatei

Sie können den Workflow so anpassen, dass in allen Ordnern Ihres Repositorys nach [routes.json](routes.md) gesucht wird. Die folgende Eigenschaft kann im Abschnitt `with` eines Auftrags definiert werden.

| Eigenschaft          | BESCHREIBUNG                                                                                                                                 |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `routes_location` | Definiert den Verzeichnisspeicherort, an dem die Datei _routes.json_ zu finden ist. Dieser Speicherort ist relativ zum Stammverzeichnis des Repositorys. |

Die explizite Angabe des Speicherorts Ihrer Datei _routes.json_ ist besonders wichtig, falls diese Datei während des Buildschritts Ihres Front-End-Frameworks nicht standardmäßig nach `output_location` verschoben wird.

> [!IMPORTANT]
> Die in der Datei _routes.json_ definierten Funktionen sind nun veraltet. Informationen zu _staticwebapp.config.json_ finden Sie in der [Konfigurationsdatei](./configuration.md) von Azure Static Web Apps.

## <a name="environment-variables"></a>Umgebungsvariablen

Umgebungsvariablen für Ihren Build können über den Abschnitt `env` einer Auftragskonfiguration festgelegt werden.

```yaml
jobs:
  build_and_deploy_job:
    if: github.event_name == 'push' || (github.event_name == 'pull_request' && github.event.action != 'closed')
    runs-on: ubuntu-latest
    name: Build and Deploy Job
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true
      - name: Build And Deploy
        id: builddeploy
        uses: Azure/static-web-apps-deploy@v0.0.1-preview
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN }}
          repo_token: ${{ secrets.GITHUB_TOKEN }}
          action: 'upload'
          ###### Repository/Build Configurations
          app_location: '/'
          api_location: 'api'
          output_location: 'public'
          ###### End of Repository/Build Configurations ######
        env: # Add environment variables here
          HUGO_VERSION: 0.58.0
```

## <a name="monorepo-support"></a>Unterstützung für Monorepos

Ein Monorepo ist ein Repository, das Code für mehr als eine Anwendung enthält. Eine Static Web Apps-Workflowdatei verfolgt standardmäßig alle Dateien in einem Repository nach, aber Sie können sie auch so anpassen, dass sie eine einzelne App als Ziel hat. Daher verfügt jede statische App für Monorepos über eine eigene Konfigurationsdatei, die sich ebenfalls im Ordner _.github/workflows_ des Repositorys befindet.

```files
├── .github
│   └── workflows
│       ├── azure-static-web-apps-purple-pond.yml
│       └── azure-static-web-apps-yellow-shoe.yml
│
├── app1  👉 controlled by: azure-static-web-apps-purple-pond.yml
├── app2  👉 controlled by: azure-static-web-apps-yellow-shoe.yml
│
├── api1  👉 controlled by: azure-static-web-apps-purple-pond.yml
├── api2  👉 controlled by: azure-static-web-apps-yellow-shoe.yml
│
└── README.md
```

Geben Sie die Pfade in den Abschnitten `push` und `pull_request` an, um für eine Workflowdatei eine einzelne App als Ziel festzulegen.

Im folgenden Beispiel wird veranschaulicht, wie Sie einen `paths`-Knoten zu den Abschnitten `push` und `pull_request` einer Datei namens _azure-static-web-apps-purple-pond.yml_ hinzufügen.

```yml
on:
  push:
    branches:
      - main
    paths:
      - app1/**
      - api1/**
      - .github/workflows/azure-static-web-apps-purple-pond.yml
  pull_request:
    types: [opened, synchronize, reopened, closed]
    branches:
      - main
    paths:
      - app1/**
      - api1/**
      - .github/workflows/azure-static-web-apps-purple-pond.yml
```

In diesem Fall wird nur bei Änderungen, die an den folgenden Dateien vorgenommen wurden, ein neuer Build ausgelöst:

- alle Dateien im Ordner _app1_
- alle Dateien im Ordner _api1_
- Änderungen an der Workflowdatei _azure-static-web-apps-purple-pond.yml_ der App

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Überprüfen von Pull Requests in Präproduktionsumgebungen](review-publish-pull-requests.md)
