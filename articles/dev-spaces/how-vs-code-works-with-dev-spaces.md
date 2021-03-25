---
title: Funktion von Visual Studio Code mit Azure Dev Spaces
services: azure-dev-spaces
ms.date: 07/08/2019
ms.topic: conceptual
description: Hier erfahren Sie, wie Sie mit Visual Studio Code und Azure Dev Spaces Ihre Kubernetes-Anwendungen debuggen und schnell durchlaufen.
keywords: Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container
ms.openlocfilehash: edfcb8107280bb86144b798da2d5b1c16371528e
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91960744"
---
# <a name="how-visual-studio-code-works-with-azure-dev-spaces"></a>Funktion von Visual Studio Code mit Azure Dev Spaces

[!INCLUDE [Azure Dev Spaces deprecation](../../includes/dev-spaces-deprecation.md)]

Sie können Visual Studio Code und die [Erweiterung Azure Dev Spaces][azds-extension] verwenden, um Ihre Dienste mit Azure Dev Spaces vorzubereiten, auszuführen und zu debuggen. Mit Visual Studio Code und der Erweiterung Azure Dev Spaces können Sie:

* Ressourcen zum Ausführen und Debuggen von Diensten in AKS generieren
* Ihre Java-, Node.js- und .NET Core-Dienste in einem Entwicklungsbereich ausführen
* Ihre in einem Entwicklungsbereich ausgeführten Java-, Node.js- und .NET Core-Dienste direkt debuggen

## <a name="generate-assets"></a>Ressourcen generieren

Visual Studio Code und die Erweiterung Azure Dev Spaces generieren die folgenden Ressourcen für Ihr Projekt:

* Dockerfiles für Java-Anwendungen, die Maven, Node.js- und .NET Core-Anwendungen verwenden
* Helm-Diagramme für fast jede Sprache mit einer Dockerfile-Datei
* Eine `azds.yaml`-Datei, d.h. die [Azure Dev Spaces-Konfigurationsdatei][azds-yaml] für Ihr Projekt
* Ein `.vscode`-Ordner mit der Visual Studio Code-Startkonfiguration Ihres Projekts für Java-Anwendungen mit Maven, Node.js-Anwendungen und .NET Core-Anwendungen

Die Dockerfile-, Helm-Diagramm- und `azds.yaml`-Dateien sind die gleichen Ressourcen, die bei der Ausführung von `azds prep` generiert werden. Diese Dateien können auch außerhalb von Visual Studio Code verwendet werden, um Ihr Projekt in AKS auszuführen, wie z.B. die Ausführung von `azds up`. Der Ordner `.vscode` wird nur von Visual Studio Code verwendet, um Ihr Projekt von Visual Studio Code aus in AKS auszuführen.

## <a name="run-your-service-in-aks"></a>Ausführen Ihres Diensts in AKS

Nachdem Sie die Ressourcen für Ihr Projekt generiert haben, können Sie Ihre Java-, Node.js- und .NET Core-Dienste in einem vorhandenen Entwicklungsbereich von Visual Studio Code aus ausführen. Auf der Seite *Debuggen* von Visual Studio Code können Sie die Startkonfiguration aus dem `.vscode`-Verzeichnis aufrufen, um Ihr Projekt auszuführen.

Sie müssen Ihren AKS-Cluster erstellen und Azure Dev Spaces in Ihrem Cluster außerhalb von Visual Studio Code aktivieren. Sie können vorhandene Dockerfiles, Helm-Diagramme und `azds.yaml`-Dateien, die außerhalb von Visual Studio Code erstellt wurden, wie beispielsweise die durch Ausführung von `azds prep` erzeugten Ressourcen, wiederverwenden. Wenn Sie Ressourcen wiederverwenden, die außerhalb von Visual Studio Code erstellt wurden, benötigen Sie immer noch ein `.vscode`-Verzeichnis. Dieses `.vscode`-Verzeichnis kann durch Visual Studio Code und die Erweiterung Azure Dev Spaces regeneriert werden und überschreibt nicht Ihre vorhandenen Ressourcen.

Für .NET Core-Projekte muss die [C#-Erweiterung][csharp-extension] installiert sein, um Ihren .NET-Dienst von Visual Studio Code aus auszuführen. Auch für Java-Projekte, die Maven verwenden, müssen die [Java-Debugger für Azure Dev Spaces-Erweiterung][java-extension] installiert und [Maven installiert und konfiguriert][maven] sein, damit Sie Ihren Java-Dienst von Visual Studio Code aus ausführen können.

## <a name="debug-your-service-in-aks"></a>Debuggen Ihres Diensts in AKS

Nachdem Sie Ihr Projekt gestartet haben, können Sie Ihre Java-, Node.js- und.NET-Core-Dienste, die in einem Entwicklungsbereich direkt aus Visual Studio Code ausgeführt werden, debuggen. Die Startkonfiguration im Verzeichnis `.vscode` stellt die zusätzlichen Debuginformationen für die Ausführung eines Dienstes mit aktiviertem Debugging in einem Entwicklungsbereich zur Verfügung. Visual Studio Code wird auch dem Debugprozess in dem in Ihrem Entwicklungsbereich ausgeführten Container angefügt, sodass Sie Haltepunkte setzen, Variablen überprüfen und andere Debugoperationen durchführen können.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Funktionsweise von Azure Dev Spaces:

> [!div class="nextstepaction"]
> [Funktionsweise von Azure Dev Spaces](how-dev-spaces-works.md)

[azds-extension]: https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds
[azds-yaml]: how-dev-spaces-works-prep.md#prepare-your-code
[csharp-extension]: https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp
[java-extension]: https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debugger-azds
[maven]: https://maven.apache.org
