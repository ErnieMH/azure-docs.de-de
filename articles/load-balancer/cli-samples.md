---
title: Azure CLI-Beispiele für Load Balancer
titleSuffix: Azure Load Balancer
description: Azure CLI-Beispiele
services: load-balancer
documentationcenter: load-balancer
author: asudbring
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18, devx-track-azurecli
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 06/14/2018
ms.author: allensu
ms.openlocfilehash: fcc2a579f2fe9048dd58cd8b52c8c704c894936b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "87499346"
---
# <a name="azure-cli-samples-for-load-balancer"></a>Azure CLI-Beispiele für Load Balancer

Die folgende Tabelle enthält Links zu Bash-Skripts, die mithilfe der Azure CLI erstellt wurden.

* [Lastenausgleich für den Datenverkehr zu virtuellen Computern für Hochverfügbarkeit](./scripts/load-balancer-linux-cli-sample-nlb.md)

  Erstellt mehrere virtuelle Computer in einer Konfiguration mit hoher Verfügbarkeit und Lastenausgleich.

* [Lastenausgleich für virtuelle Computer über Verfügbarkeitszonen hinweg](./scripts/load-balancer-linux-cli-sample-zone-redundant-frontend.md)

  Erstellt drei virtuelle Computer in verschiedenen Verfügbarkeitszonen innerhalb einer Region und Load Balancer Standard mit einer zonenredundanten Front-End-IP-Adresse. Diese Konfiguration für den Lastenausgleich hilft Ihnen, Ihre Anwendungen und Daten vor einem unwahrscheinlichen Ausfall oder Verlust eines gesamten Rechenzentrums zu schützen.

* [Vornehmen eines Lastausgleichs für virtuelle Computer innerhalb einer bestimmten Verfügbarkeitszone](./scripts/load-balancer-linux-cli-sample-zonal-frontend.md)

  Erstellt drei virtuelle Computer, einen Load Balancer Standard mit zonenbasierter Front-End-IP, der bei der Ausrichtung von Datenpfad und Ressourcen in einer einzigen Zone für eine bestimmte Region hilft.

* [Lastenausgleich für mehrere Websites auf VMs](./scripts/load-balancer-linux-cli-load-balance-multiple-websites-vm.md)

  Erstellt zwei VMs mit verschiedenen IP-Konfigurationen, die zu einer über Azure Load Balancer aufrufbaren Verfügbarkeitsgruppe hinzugefügt werden.
