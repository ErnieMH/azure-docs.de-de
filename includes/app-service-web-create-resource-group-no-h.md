---
title: include file
description: include file
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: b59b45de04ebb717dfe55eb17c9dbd92f7523976
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "67178572"
---
[!INCLUDE [resource group intro text](resource-group.md)]

Erstellen Sie in Cloud Shell mit dem Befehl [`az group create`](/cli/azure/group?view=azure-cli-latest) eine Ressourcengruppe. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *Europa, Westen*. Wenn Sie alle unterstützten Standorte für App Service im **Free**-Tarif anzeigen möchten, führen Sie den Befehl [`az appservice list-locations --sku FREE`](/cli/azure/appservice?view=azure-cli-latest) aus.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Im Allgemeinen erstellen Sie Ressourcengruppen und Ressourcen in einer Region in Ihrer Nähe. 

Nach Ausführung dieses Befehls werden die Ressourcengruppeneigenschaften in einer JSON-Ausgabe angezeigt.