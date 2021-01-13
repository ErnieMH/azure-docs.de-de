---
title: Verwenden eines benutzerdefinierten NuGet-Feeds
services: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 07/17/2019
ms.topic: conceptual
description: Verwenden Sie einen benutzerdefinierten NuGet-Feed für den Zugriff auf und die Verwendung von NuGet-Paketen in Azure Dev Spaces.
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, Container
manager: gwallace
ms.openlocfilehash: d60d7142d9b9979be76eebb3d324a448bd76638f
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91960217"
---
# <a name="use-a-custom-nuget-feed-with-azure-dev-spaces"></a>Verwenden eines benutzerdefinierten NuGet-Feeds mit Azure Dev Spaces

[!INCLUDE [Azure Dev Spaces deprecation](../../../includes/dev-spaces-deprecation.md)]

Ein NuGet-Feed stellt eine einfache Möglichkeit dar, Paketquellen zu einem Projekt hinzuzufügen. Azure Dev Spaces muss auf diesen Feed zugreifen, damit die Abhängigkeiten im Docker-Container korrekt installiert werden.

## <a name="set-up-a-nuget-feed"></a>Einrichten eines NuGet-Feeds

Fügen Sie einen [Paketverweis](/nuget/consume-packages/package-references-in-project-files) für Ihre Abhängigkeit der `*.csproj`-Datei unter dem `PackageReference`-Knoten hinzu. Beispiel:

```xml
<ItemGroup>
    <!-- ... -->
    <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0" />
    <!-- ... -->
</ItemGroup>
```

Erstellen Sie eine [NuGet.Config](/nuget/reference/nuget-config-file)-Datei im Projektordner, und legen Sie die Abschnitte `packageSources` und `packageSourceCredentials` für den NuGet-Feed fest. Der `packageSources`-Abschnitt enthält die Feed-URL, auf die Zugriff von Ihrem AKS-Cluster möglich sein muss. Die `packageSourceCredentials` sind die Anmeldeinformationen für den Zugriff auf den Feed. Beispiel:

```xml
<packageSources>
    <add key="Contoso" value="https://contoso.com/packages/" />
</packageSources>

<packageSourceCredentials>
    <Contoso>
        <add key="Username" value="user@contoso.com" />
        <add key="ClearTextPassword" value="33f!!lloppa" />
    </Contoso>
</packageSourceCredentials>
```

Aktualisieren Sie die Dockerfile-Dateien, um die `NuGet.Config`-Datei in das Image zu kopieren. Beispiel:

```console
COPY ["<project folder>/NuGet.Config", "./NuGet.Config"]
```

> [!TIP]
> Unter Windows werden `NuGet.Config`, `Nuget.Config` und `nuget.config` als gültige Dateinamen verwendet. Unter Linux ist nur `NuGet.Config` ein gültiger Dateiname für diese Datei. Da Azure Dev Spaces Docker und Linux verwendet, muss diese Datei den Namen `NuGet.Config` haben. Sie können die Benennung manuell oder durch Ausführen von `dotnet restore --configfile nuget.config` korrigieren.


Wenn Sie Git verwenden, sollte die Versionskontrolle nicht die Anmeldeinformationen für den NuGet-Feed enthalten. Fügen Sie `NuGet.Config` der `.gitignore`-Datei für Ihr Projekt hinzu, damit die `NuGet.Config`-Datei nicht der Versionskontrolle hinzugefügt wird. Azure Dev Spaces benötigt diese Datei während des Containerimage-Buildprozesses, berücksichtigt jedoch standardmäßig die in `.gitignore` und `.dockerignore` während der Synchronisierung definierten Regeln. Um die Standardeinstellung zu ändern und Azure Dev Spaces die Synchronisierung der `NuGet.Config`-Datei zu ermöglichen, aktualisieren Sie die `azds.yaml`-Datei:

```yaml
build:
useGitIgnore: true
ignore:
- "!NuGet.Config"
```

Wenn Sie Git nicht verwenden, können Sie diesen Schritt überspringen.

Wenn Sie das nächste Mal `azds up` ausführen oder `F5` in Visual Studio Code oder Visual Studio drücken, synchronisiert Azure Dev Spaces die Datei `NuGet.Config`, mit der Sie Paketabhängigkeiten installieren.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [NuGet und seine Funktionsweise](/nuget/what-is-nuget).
