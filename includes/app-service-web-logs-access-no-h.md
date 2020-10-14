---
title: include file
description: include file
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/27/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: e6c4b07d01a4992e22107cb7d524646f439c37c6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "84905866"
---
Aktivieren Sie die Diagnoseprotokollierung, indem Sie den folgenden Befehl in [Cloud Shell](https://shell.azure.com)ausführen, um auf die Konsolenprotokolle zuzugreifen, die innerhalb des Anwendungscodes in App Service generiert wurden:

```azurecli-interactive
az webapp log config --resource-group <resource-group-name> --name <app-name> --application-logging true --level Verbose
```

Mögliche Werte für `--level` sind `Error`, `Warning` `Info` oder `Verbose`. Jede nachfolgende Ebene enthält die vorherige Ebene. Beispiel: `Error` enthält nur Fehlermeldungen, und `Verbose` enthält alle Meldungen.

Führen Sie den folgenden Befehl aus, um den Protokolldatenstrom anzuzeigen, nachdem die Diagnoseprotokollierung aktiviert wurde:

```azurecli-interactive
az webapp log tail --resource-group <resource-group-name> --name <app-name>
```

Falls Sie nicht sofort Konsolenprotokolle sehen, können Sie es nach 30 Sekunden noch einmal versuchen.

> [!NOTE]
> Sie können die Protokolldateien auch im Browser unter `https://<app-name>.scm.azurewebsites.net/api/logs/docker` untersuchen.

Um das Protokollstreaming jederzeit zu beenden, geben Sie `Ctrl`+`C` ein.
