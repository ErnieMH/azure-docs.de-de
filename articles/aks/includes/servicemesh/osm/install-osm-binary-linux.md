---
author: phillipgibson
ms.topic: include
ms.date: 03/15/2021
ms.author: phillipgibson
ms.openlocfilehash: 6a4871b4aca10dddae46ed8f732090d9a258b3d6
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110468117"
---
## <a name="download-and-install-the-osm-client-binary"></a>Laden Sie das OSM-Client-Binary herunter und installieren Sie es

Verwenden Sie in einer Bash-basierten Shell unter Linux oder dem [Windows-Subsystem für Linux][install-wsl]`curl`, um das OSM-Release herunterzuladen, und extrahieren Sie dieses dann wie folgt mit `tar`:

```bash
# Specify the OSM version that will be leveraged throughout these instructions
OSM_VERSION=v0.8.4

curl -sL "https://github.com/openservicemesh/osm/releases/download/$OSM_VERSION/osm-$OSM_VERSION-linux-amd64.tar.gz" | tar -vxzf -
```

Die `osm` Clientbinärdatei läuft auf Ihrem Client-Rechner und ermöglicht Ihnen die Verwaltung von OSM in Ihrem AKS-Cluster. Verwenden Sie die folgenden Befehle, um die OSM-Clientbinärdatei `osm` in einer Bash-basierten Shell unter Linux oder [Windows-Subsystem für Linux][install-wsl] zu installieren. Diese Befehle kopieren die `osm`-Clientbinärdatei an den Speicherort des Standardbenutzerprogramms in Ihrem `PATH`.

```bash
sudo mv ./linux-amd64/osm /usr/local/bin/osm
sudo chmod +x /usr/local/bin/osm
```

Mithilfe des folgenden Befehls können Sie überprüfen, ob die `osm` Clientbibliothek ordnungsgemäß zu Ihrem Pfad und ihrer Versionsnummer hinzugefügt wurde.

```
osm version
```

<!-- LINKS - external -->

[install-wsl]: /windows/wsl/install-win10
