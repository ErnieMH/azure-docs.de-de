---
title: 'Azure VMware Solution by CloudSimple: Einrichten von Workload-DNS und -DHCP für die private Cloud'
description: Beschreibt das Einrichten von DNS und DHCP für Anwendungen und Workloads, die in ihrer privaten CloudSimple-Cloudumgebung ausgeführt werden.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/16/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: d7531a22d5b0f44fdaae837a75d17ea18852acf0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87077300"
---
# <a name="set-up-dns-and-dhcp-applications-and-workloads-in-your-cloudsimple-private-cloud"></a>Einrichten von DNS- und DHCP-Anwendungen und -Workloads in der privaten CloudSimple-Cloud

Anwendungen und Workloads, die in einer privaten Cloudumgebung ausgeführt werden, erfordern Namensauflösung und DHCP-Dienste für Nachschlagen und IP-Adresszuweisung.  Zur Bereitstellung dieser Dienste ist eine ordnungsgemäße DHCP- und DNS-Infrastruktur erforderlich.  Sie können einen virtuellen Computer konfigurieren, um diese Dienste in Ihrer privaten Cloudumgebung bereitzustellen.  

## <a name="prerequisites"></a>Voraussetzungen

* Eine verteilte Portgruppe mit konfiguriertem VLAN
* Routeneinrichtung zu lokalen oder internetbasierten DNS-Servern
* Vorlage für virtuelle Maschinen oder ISO zum Erstellen eines virtuellen Computers

## <a name="linux-based-dns-server-setup"></a>Setup für Linux-basiertes DNS-Server

Linux bietet verschiedene Pakete für das Einrichten von DNS-Servern.  Dies ist ein [Beispielsetup von DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-private-network-dns-server-on-ubuntu-18-04) mit Anleitungen zu Einrichten eines Open-Source-BIND-DNS-Servers.

## <a name="windows-based-setup"></a>Windows-basiertes Setup

In diesen Microsoft-Themen wird beschrieben, wie Sie einen Windows-Server als DNS-Server und als DHCP-Server einrichten.

* [Windows-Server als DNS-Server](/windows-server/networking/dns/dns-top)
* [Windows-Server als DHCP-Server](/windows-server/networking/technologies/dhcp/dhcp-top)
