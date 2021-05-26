---
author: phillipgibson
ms.topic: include
ms.date: 03/15/2021
ms.author: pgibson
ms.openlocfilehash: 990a1f7d6e81c888d9a9901bf651e61cd7072fd4
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110468116"
---
## <a name="download-and-install-the-osm-client-binary"></a>Laden Sie das OSM-Client-Binary herunter und installieren Sie es

Verwenden Sie in einer PowerShell-basierten Shell unter Windows `Invoke-WebRequest`, um das Istio-Release herunterzuladen, und führen Sie das Extrahieren dann wie folgt mit `Expand-Archive` durch:

```powershell
# Specify the OSM version that will be leveraged throughout these instructions
$OSM_VERSION="v0.8.4"

[Net.ServicePointManager]::SecurityProtocol = "tls12"
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/openservicemesh/osm/releases/download/$OSM_VERSION/osm-$OSM_VERSION-windows-amd64.zip&quot; -OutFile &quot;osm-$OSM_VERSION.zip"
Expand-Archive -Path "osm-$OSM_VERSION.zip" -DestinationPath .
```

Das `osm`Clientbinär läuft auf Ihrem Client-Rechner und ermöglicht Ihnen die Verwaltung des OSM-Controllers in Ihrem AKS-Cluster. Verwenden Sie die folgenden Befehle, um die OSM-Clientbinärdatei `osm` in einer PowerShell-basierten Shell unter Windows zu installieren. Mit diesen Befehlen wird die `osm`Clientbinärdatei in einen Ordner „OSM“ kopiert und dann sowohl sofort (in der aktuellen Shell) als auch dauerhaft (über Neustarts der Shell) über Ihren `PATH` verfügbar gemacht. Sie benötigen keine erhöhten Rechte (Administrator), um diese Befehle auszuführen, und Sie müssen Ihre Shell nicht neu starten.

```powershell
# Copy osm.exe to C:\OSM
New-Item -ItemType Directory -Force -Path "C:\OSM"
Move-Item -Path .\windows-amd64\osm.exe -Destination "C:\OSM\"

# Add C:\OSM to PATH.
# Make the new PATH permanently available for the current User
$USER_PATH = [environment]::GetEnvironmentVariable("PATH&quot;, &quot;User&quot;) + &quot;;C:\OSM\"
[environment]::SetEnvironmentVariable("PATH", $USER_PATH, "User")
# Make the new PATH immediately available in the current shell
$env:PATH += ";C:\OSM\"
```
