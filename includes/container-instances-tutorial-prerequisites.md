---
title: include file
description: include file
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 03/20/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 10fb9e8169b7f4159ccbf4a0ff36021f6033f811
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "75552415"
---
Für dieses Tutorial müssen folgende Voraussetzungen erfüllt sein:

**Azure CLI**: Auf dem lokalen Computer muss mindestens Version 2.0.29 der Azure-Befehlszeilenschnittstelle installiert sein. Führen Sie `az --version` aus, um die Version zu ermitteln. Installations- und Upgradeinformationen finden Sie bei Bedarf unter [Installieren von Azure CLI][azure-cli-install].

**Docker**: In diesem Tutorial wird vorausgesetzt, dass zentrale Docker-Konzepte wie Container und Containerimages sowie grundlegende `docker`-Befehle bekannt sind. Eine Einführung in Docker und Container finden Sie in der [Docker-Übersicht][docker-get-started].

**Docker**: Für dieses Tutorial muss Docker lokal installiert sein. Für die Docker-Umgebung stehen Konfigurationspakete für [macOS][docker-mac], [Windows][docker-windows] und [Linux][docker-linux] zur Verfügung.

> [!IMPORTANT]
> Da Azure Cloud Shell den Docker-Daemon nicht beinhaltet, *müssen* Sie sowohl die Azure-Befehlszeilenschnittstelle als auch die Docker-Engine auf Ihrem *lokalen Computer* installieren, um dieses Tutorial absolvieren zu können. Azure Cloud Shell kann für dieses Tutorial nicht verwendet werden.

<!-- LINKS - External -->
[docker-get-started]: https://docs.docker.com/engine/docker-overview/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - Internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
