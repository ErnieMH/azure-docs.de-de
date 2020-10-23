---
title: Neues in Azure Load Balancer
description: Hier finden Sie Informationen zu den Neuerungen in Azure Load Balancer, z. B. aktuelle Versionshinweise, bekannte Probleme, Fehlerbehebungen, veraltete Funktionen und anstehende Änderungen.
services: load-balancer
author: anavinahar
ms.service: load-balancer
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: anavin
ms.openlocfilehash: 3a20b69189226486de8cfceb95e2fa79a976cb12
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91841032"
---
# <a name="whats-new-in-azure-load-balancer"></a>Neues in Azure Load Balancer

Azure Load Balancer wird regelmäßig aktualisiert. Bleiben Sie bei aktuellen Ankündigungen auf dem neuesten Stand. Dieser Artikel bietet Folgendes:

- Neueste Versionen
- Bekannte Probleme
- Behebung von Programmfehlern
- Veraltete Funktionen (sofern zutreffend)

Die aktuellen Updates für Azure Load Balancer finden Sie [hier](https://azure.microsoft.com/updates/?category=networking&query=load%20balancer). Unter diesem Link können Sie auch den RSS-Feed abonnieren.

## <a name="recent-releases"></a>Aktuelle Releases

| type |Name |BESCHREIBUNG  |Hinzufügedatum  |
| ------ |---------|---------|---------|
| Funktion | Unterstützung für Verschiebungen über Ressourcengruppen hinweg | Unterstützung von Load Balancer Standard und öffentlichen IP-Standardadressen für [Verschiebungen von Ressourcengruppen](https://azure.microsoft.com/updates/standard-resource-group-move/). | Oktober 2020 |
| Funktion | Support for IP-based backend pool management (Preview) (Unterstützung für die IP-basierte Back-End-Poolverwaltung (Vorschau)) | Azure Load Balancer unterstützt das Hinzufügen und Entfernen von Ressourcen zu bzw. aus einem Back-End-Pool über IPv4- oder IPv6-Adressen. Dies ermöglicht die einfache Verwaltung von Containern, virtuellen Computern und VM-Skalierungsgruppen, die Load Balancer zugeordnet sind. Darüber hinaus können IP-Adressen als Teil eines Back-End-Pools reserviert werden, bevor die zugehörigen Ressourcen erstellt werden. [Hier](backend-pool-management.md) erhalten Sie weitere Informationen.|Juli 2020 |
| Funktion| Azure Load Balancer Insights using Azure Monitor (Azure Load Balancer-Erkenntnisse mithilfe von Azure Monitor) | Kunden können jetzt für alle ihre Load Balancer-Konfigurationen und Integritätsdashboards topologische Karten nutzen, die als Teil von Azure Monitor für Netzwerke erstellt wurden und sich für Load Balancer Standard-Instanzen eignen, die mit entsprechenden Metriken im Azure-Portal vorkonfiguriert wurden. [Erste Schritte und weitere Informationen](https://azure.microsoft.com/blog/introducing-azure-load-balancer-insights-using-azure-monitor-for-networks/) | Juni 2020 |
| Überprüfen | Hinzufügung der Überprüfung für Hochverfügbarkeitsports | Es wurde eine Überprüfung hinzugefügt, um sicherzustellen, dass Hochverfügbarkeitsportregeln und Nicht-Hochverfügbarkeitsportregeln nur konfigurierbar sind, wenn Floating IP aktiviert ist. Bisher wurde diese Konfiguration akzeptiert, funktionierte jedoch nicht wie vorgesehen. Es wurde keine Änderung an der Funktionalität vorgenommen. Weitere Informationen finden Sie [hier](load-balancer-ha-ports-overview.md#limitations).| Juni 2020 |
| Funktion| IPv6 support for Azure Load Balancer (generally available) (IPv6-Unterstützung für Azure Load Balancer (allgemein verfügbar)) | Sie können IPv6-Adressen als Front-End für Ihre Azure Load Balancer-Instanzen verwenden. [Hier](../virtual-network/virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell.md) erfahren Sie, wie Sie eine Anwendung mit dualem Stapel erstellen. |April 2020|
| Funktion| TCP Resets on Idle Timeout (generally available) (TCP-Zurücksetzungen bei Leerlauftimeout (allgemein verfügbar))| Verwenden Sie TCP-Zurücksetzungen, um ein besser vorhersagbares Anwendungsverhalten zu ermöglichen. [Weitere Informationen](load-balancer-tcp-reset.md)| Februar 2020 |

## <a name="known-issues"></a>Bekannte Probleme

Die Produktgruppe arbeitet aktiv an Lösungen für die folgenden bekannten Probleme:

|Problem |BESCHREIBUNG  |Minderung  |
| ---------- |---------|---------|
| Log Analytics-Export | Von Log Analytics können keine Metriken für Load Balancer Standard-Instanzen und keine Integritätstest-Statusprotokolle für Load Balancer Basic exportiert werden.  | [Verwenden Sie Azure Monitor für mehrdimensionale Metriken für Load Balancer Standard.](load-balancer-standard-diagnostics.md) Zwar kann Log Analytics nicht für die Überwachung verwendet werden, in Azure Monitor kann jedoch ein breites Spektrum mehrdimensionaler Metriken visualisiert werden. Das vorkonfigurierte Metrikdashboard steht auf dem untergeordneten Blatt „Erkenntnisse“ Ihrer Load Balancer-Instanz zur Verfügung. Führen Sie bei Verwendung von Load Balancer Basic ein [Upgrade auf Standard](upgrade-basic-standard.md) durch, um die Metriküberwachung auf Produktionsebene zu ermöglichen.

  

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Load Balancer finden Sie unter [Was versteht man unter Azure Load Balancer?](load-balancer-overview.md) und [Häufig gestellte Fragen zu Load Balancer](load-balancer-faqs.md).
