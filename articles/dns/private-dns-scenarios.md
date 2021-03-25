---
title: Szenarien für private Zonen – Azure DNS
description: In diesem Artikel lernen Sie allgemeine Szenarien für das Verwenden von Azure DNS Private Zones kennen.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 10/05/2019
ms.author: rohink
ms.openlocfilehash: ab850adb2e9a25778d5f44ba711eb0762fe562c8
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "76939337"
---
# <a name="azure-dns-private-zones-scenarios"></a>Private Azure DNS-Zonen: Szenarien

Azure DNS Private Zones bieten Namensauflösung in einem virtuellen Netzwerk sowie zwischen virtuellen Netzwerken. In diesem Artikel werden einige allgemeine Szenarien untersucht, die mit diesem Feature realisiert werden können.

## <a name="scenario-name-resolution-scoped-to-a-single-virtual-network"></a>Szenario: Namensauflösung für ein einzelnes virtuelles Netzwerk
In diesem Szenario haben Sie ein virtuelles Netzwerk in Azure, das eine Reihe von Azure-Ressourcen enthält, wozu auch virtuelle Computer gehören. Sie möchten die Ressourcen aus dem virtuellen Netzwerk über einen bestimmten Domänennamen (DNS-Zone) auflösen, und Sie fordern, dass die Namensauflösung privat und nicht über das Internet zugänglich ist. Außerdem fordern Sie für die virtuellen Computer im virtuellen Netzwerk, dass Azure sie automatisch in der DNS-Zone registriert. 

Dieses Szenario ist nachstehend dargestellt. Das Microsoft Azure Virtual Network namens „A“ enthält zwei virtuelle Computer (VNETA-VM1 und VNETA-VM2). Jedem dieser Computer ist eine private IP-Adresse zugeordnet. Sobald Sie eine private Zone namens „contoso.com“ erstellt und dieses virtuelle Netzwerk als virtuelles Registrierungsnetzwerk verknüpft haben, erstellt Azure DNS automatisch zwei A-Datensätze in der Zone (siehe Abbildung). Jetzt erhalten DNS-Abfragen von VNETA-VM1 zur Auflösung von „VNETA-VM2.contoso.com“ eine DNS-Antwort, die die private IP-Adresse von VNETA-VM2 enthält. Außerdem erhält eine Reverse DNS-Abfrage (PTR) für die private IP-Adresse von VNETA-VM1 (10.0.0.0.1), die von VNETA-VM2 gesendet wurde, erwartungsgemäß eine DNS-Antwort, die den Namen von VNETA-VM1 enthält. 

![Auflösung für einzelnes virtuelles Netzwerk](./media/private-dns-scenarios/single-vnet-resolution.png)

## <a name="scenario-name-resolution-across-virtual-networks"></a>Szenario: Namensauflösung bei mehreren virtuellen Netzwerken

Dieses Szenario ist der üblichere Fall, in dem Sie eine private Zone mit mehreren virtuellen Netzwerken verbinden müssen. Dieses Szenario eignet sich für Architekturen wie das Hub-and-Spoke-Modell, bei dem es ein zentrales virtuelles Hubnetzwerk gibt, mit dem mehrere virtuelle Spoke-Netzwerke verbunden sind. Das zentrale virtuelle Hubnetzwerk kann als das virtuelle Registrierungsnetzwerk mit einer privaten Zone verknüpft werden, und die virtuellen Spoke-Netzwerke können als virtuelle Auflösungsnetzwerke verknüpft werden. 

Im folgenden Diagramm ist eine einfache Version dieses Szenarios dargestellt, in dem es nur zwei virtuelle Netzwerke gibt: A und B. A ist als ein virtuelles Registrierungsnetzwerk und B als ein virtuelles Auflösungsnetzwerk festgelegt. Vorgesehen ist, dass für beide virtuellen Netzwerke eine gemeinsame Zone „contoso.com“ verwendet wird. Wenn die Zone erstellt ist und das virtuelle Auflösungs- und das virtuelle Registrierungsnetzwerk mit der Zone verknüpft sind, registriert Azure automatisch DNS-Einträge für die virtuellen Computer (VNETA-VM1 und VNETA-VM2) aus dem virtuellen Netzwerk A. Sie können auch manuell DNS-Einträge in der Zone für virtuelle Computer im virtuellen Auflösungsnetzwerk B hinzufügen. Bei dieser Konfiguration ergibt sich das folgende Verhalten für Weiterleitungs- und Reverse-DNS-Abfragen:
* Eine DNS-Abfrage von VNETB-VM1 im virtuellen Auflösungsnetzwerk B für „VNETA-VM1.contoso.com“ erhält eine DNS-Antwort, die die private IP-Adresse von VNETA-VM1 enthält.
* Eine Reverse-DNS-Abfrage (PTR-Abfrage) von VNETB-VM2 im virtuellen Auflösungsnetzwerk B für 10.1.0.1 erhält eine DNS-Antwort, die „FQDN VNETB-VM1.contoso.com“ enthält.  
* Eine Reverse-DNS-Abfrage (PTR-Abfrage) von VNETB-VM3 im virtuellen Auflösungsnetzwerk B für 10.0.0.1 erhält NXDOMAIN. Der Grund ist, dass Reverse-DNS-Abfragen auf lediglich dasselbe virtuelle Netzwerk beschränkt sind. 


![Mehrere Auflösungen für ein virtuelles Netzwerk](./media/private-dns-scenarios/multi-vnet-resolution.png)

## <a name="scenario-split-horizon-functionality"></a>Szenario: Split-Horizon-Funktionalität

In diesem Szenario haben Sie einen Anwendungsfall, bei dem Sie je nachdem, wo sich der Client befindet (in Azure oder im Internet), für dieselbe DNS-Zone ein unterschiedliches Auflösungsverhalten realisieren möchten. Beispielsweise könnten Sie eine private und eine öffentliche Version Ihrer Anwendung haben, die jeweils unterschiedliche Funktionen oder Verhaltensweisen haben, aber Sie möchten für beide Versionen denselben Domänennamen verwenden. Dieses Szenario kann mit Azure DNS realisiert werden, indem sowohl eine öffentlichen DNS-Zone als auch eine private Zone mit demselben Namen erstellt werden.

Im folgenden Diagramm ist dieses Szenario dargestellt. Sie haben ein virtuelles Netzwerk mit zwei virtuellen Computern (VNETA-VM1 und VNETA-VM2), denen jeweils sowohl eine private als auch eine öffentliche IP-Adresse zugeordnet sind. Sie erstellen eine öffentliche DNS-Zone namens „contoso.com“ und registrieren die öffentlichen IP-Adressen für diese virtuellen Computer als DNS-Einträge in der Zone. Sie erstellen außerdem eine private DNS-Zone, die ebenfalls den Namen „contoso.com“ hat, wobei Sie A als das virtuelle Registrierungsnetzwerk angeben. Azure registriert die virtuellen Computer automatisch als A-Datensätze in der privaten Zone, die auf ihre privaten IP-Adressen verweisen.

Ab jetzt gibt Azure, wenn ein Internetclient eine DNS-Abfrage sendet, um „VNETA-VM1.contoso.com“ nachzuschlagen, den öffentlichen IP-Eintrag aus der öffentlichen Zone zurück. Wird die gleiche DNS-Abfrage von einem anderen virtuellen Computer (z. B. VNETA-VM2) im selben virtuellen Netzwerk gesendet, gibt Azure den privaten IP-Eintrag aus der privaten Zone zurück. 

![Split-Horizon-Auflösung](./media/private-dns-scenarios/split-brain-resolution.png)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu privaten DNS-Zonen finden Sie unter [Using Azure DNS for private domains](private-dns-overview.md) (Verwenden von Azure DNS für private Domänen).

Erfahren Sie, wie Sie in Azure DNS [eine private DNS-Zone erstellen](./private-dns-getstarted-powershell.md).

Erfahren Sie mehr zu DNS-Zonen und -Einträgen im folgenden Artikel: [DNS-Zonen und -Einträge: Übersicht](dns-zones-records.md).

Erfahren Sie mehr über die anderen zentralen [Netzwerkfunktionen](../networking/networking-overview.md) von Azure.

