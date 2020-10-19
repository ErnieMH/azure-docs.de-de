---
title: Verwenden des FreeBSD-Paketfilters zum Erstellen einer Firewall in Azure
description: Lernen Sie, wie Sie eine NAT-Firewall mithilfe des FreeBSD-PF in Azure bereitstellen.
author: KylieLiang
ms.service: virtual-machines-linux
ms.topic: how-to
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/20/2017
ms.author: kyliel
ms.openlocfilehash: 6a20708c5564075c24eb031a39292b020a2ecc00
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91371319"
---
# <a name="how-to-use-freebsds-packet-filter-to-create-a-secure-firewall-in-azure"></a>Verwenden des FreeBSD-Paketfilters zum Erstellen einer sicheren Firewall in Azure
In diesem Artikel wird erläutert, wie Sie eine NAT-Firewall mithilfe eines FreeBSD-Paketfilters über die Azure Resource Manager-Vorlage für allgemeine Webserverszenarien bereitstellen.

## <a name="what-is-pf"></a>Was ist PF?
PF (Paketfilter) ist ein von BSD lizenzierter zustandsbehafteter Paketfilter, ein zentraler Bestandteil einer Firewallsoftware. PF hat sich seither schnell entwickelt und weist mittlerweile mehrere Vorteile gegenüber anderen Firewalls auf. NAT (Network Address Translation, Netzwerkadressenübersetzung) ist seit dem ersten Tag in PF enthalten. Anschließend wurden der Paketplaner und die aktive Warteschlangenverwaltung durch Integration von ALTQ in PF integriert. Dadurch wurde eine Konfiguration über die Konfiguration des PF ermöglicht. PF wurde um Funktionen wie z.B. pfsync und CARP für Failover und Redundanz, authpf für die Sitzungsauthentifizierung und FTP-Proxy für die Vereinfachung des Firewallschutzes schwieriger FTP-Protokolle erweitert. Kurz gesagt ist PF eine leistungsstarke und funktionsreiche Firewall. 

## <a name="get-started"></a>Erste Schritte
Wenn Sie eine sichere Firewall in der Cloud für Ihre Webserver einrichten möchten, können Sie jetzt loslegen. Sie können auch die in dieser Azure Resource Manager-Vorlage verwendeten Skripts zum Einrichten Ihrer Netzwerktopologie anwenden.
Mit der Azure Resource Manager-Vorlage werden ein virtueller FreeBSD-Computer, der eine NAT/Umleitung mit dem PF durchführt, sowie zwei virtuelle FreeBSD-Computer mit installiertem und konfiguriertem Nginx-Webserver eingerichtet. Zusätzlich zum Durchführen der NAT für die zwei Webserver mit ausgehendem Datenverkehr fängt der virtuelle Computer für die NAT/Umleitung HTTP-Anfragen ab und leitet sie zu den zwei Round-Robin-Webservern weiter. Das VNet verwendet den privaten, nicht weiterleitbaren IP-Adressraum 10.0.0.2/24. Außerdem können Sie die Parameter der Vorlage ändern. Die Azure Resource Manager-Vorlage definiert zudem eine Routingtabelle für das gesamte VNet, eine Auflistung der einzelnen Routen, die zum Außerkraftsetzen der Azure-Standardrouten auf Grundlage der Ziel-IP-Adresse verwendet werden. 

![Das Diagramm zeigt eine öffentliche IP-Adresse einer NAT-Instanz, die über die Round-Robin-Methode zu zwei virtuellen Back-End-Computern umgeleitet wird, die Nginx-Webserver hosten.](./media/freebsd-pf-nat/pf_topology.jpg)
    
### <a name="deploy-through-azure-cli"></a>Bereitstellen über die Azure CLI
Die neueste Version der [Azure CLI](/cli/azure/install-az-cli2) muss installiert sein, und Sie müssen mithilfe von [az login](/cli/azure/reference-index) bei einem Azure-Konto angemeldet sein. Erstellen Sie mit [az group create](/cli/azure/group) eine Ressourcengruppe. Im folgenden Beispiel wird eine Ressourcengruppe namens `myResourceGroup` am Standort `West US` erstellt.

```azurecli
az group create --name myResourceGroup --location westus
```

Als Nächstes stellen Sie die Vorlage „pf-freebsd-setup“ mit [az group deployment create](/cli/azure/group/deployment) bereit. Laden Sie „azuredeploy.parameters.json“ unter demselben Pfad herunter, und definieren Sie Ihre eigenen Ressourcenwerte, wie etwa `adminPassword`, `networkPrefix` und `domainNamePrefix`. 

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeploymentName \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/pf-freebsd-setup/azuredeploy.json \
    --parameters '@azuredeploy.parameters.json' --verbose
```

Nach etwa fünf Minuten erhalten Sie die Informationen von `"provisioningState": "Succeeded"`. Danach können Sie einen SSH-Vorgang am virtuellen Frontend-Computer (NAT) durchführen oder den Nginx-Webserver in einem Browser mithilfe der öffentlichen IP-Adresse oder des FQDN des virtuellen Front-End-Computers (NAT) aufrufen. Im folgenden Beispiel sind FQDN und öffentliche IP-Adressen aufgeführt, die dem virtuellen Front-End-Computer (NAT) in der `myResourceGroup`-Ressourcengruppe zugewiesen sind. 

```azurecli
az network public-ip list --resource-group myResourceGroup
```
    
## <a name="next-steps"></a>Nächste Schritte
Möchten Sie Ihre eigene NAT in Azure einrichten? Open Source: kostenlos, aber leistungsstark? PF ist eine gute Wahl. Mit der Vorlage „pf-freebsd-setup“ benötigen Sie nur fünf Minuten für die Einrichtung einer NAT-Firewall mit Round-Robin-Lastenausgleich, indem Sie den kostenlosen FreeBSD-PF in Azure für allgemeine Webserverszenarien verwenden. 

Informationen über das Angebot von FreeBSD in Azure finden Sie unter [Einführung in FreeBSD in Azure](freebsd-intro-on-azure.md).

Weitere Informationen zu PF finden Sie im [FreeBSD Handbuch](https://www.freebsd.org/doc/handbook/firewalls-pf.html) oder [PF-Benutzerhandbuch](https://www.freebsd.org/doc/handbook/firewalls-pf.html).
