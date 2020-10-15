---
title: Sicherheitskontrollen für Azure Virtual Machine Scale Sets-Instanzen
description: Eine Prüfliste der Sicherheitskontrollen für die Auswertung von Azure Virtual Machine Scale Sets-Instanzen
author: mimckitt
ms.author: mimckitt
ms.topic: conceptual
ms.service: virtual-machine-scale-sets
ms.subservice: security
ms.date: 09/05/2019
ms.reviewer: jushiman
ms.custom: mimckitt
ms.openlocfilehash: 4480ad425d9a3953fd5779f99d27b5b6b037e61e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87029415"
---
# <a name="security-controls-for-azure-virtual-machine-scale-sets"></a>Sicherheitskontrollen für Azure Virtual Machine Scale Sets-Instanzen

In diesem Artikel werden die in Azure Virtual Machine Scale Sets-Instanzen integrierten Sicherheitskontrollen beschrieben.

[!INCLUDE [Security controls header](../../includes/security-controls-header.md)]

## <a name="network"></a>Netzwerk

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Unterstützung des Dienstendpunkts| Ja | |
| Unterstützung der VNet-Einschleusung| Ja | |
| Netzwerkisolation und Firewallunterstützung| Ja |  |
| Unterstützung der Tunnelerzwingung| Ja | Siehe [Konfigurieren der Tunnelerzwingung mit dem Azure Resource Manager-Bereitstellungsmodell](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md). |

## <a name="monitoring--logging"></a>Überwachung und Protokollierung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | Siehe [Überwachen und Aktualisieren eines virtuellen Linux-Computers in Azure](../virtual-machines/linux/tutorial-monitor.md) und [Überwachen und Aktualisieren eines virtuellen Windows-Computers in Azure](../virtual-machines/windows/tutorial-monitor.md). |
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja |  |
| Protokollierung und Überwachung auf Datenebene | Nein |  |

## <a name="identity"></a>Identity

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Authentifizierung| Ja |  |
| Authorization| Ja |  |

## <a name="data-protection"></a>Schutz von Daten

| Sicherheitskontrolle | Ja/Nein | Notizen |
|---|---|--|
| Serverseitige Verschlüsselung ruhender Daten: Von Microsoft verwaltete Schlüssel | Ja | Siehe [Azure Disk Encryption für VM-Skalierungsgruppen](disk-encryption-overview.md). |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, Verschlüsselung im VNET und VNET-zu-VNET-Verschlüsselung)| Ja | Azure Virtual Machines unterstützt [ExpressRoute](../expressroute/index.yml)- und VNET-Verschlüsselung. Siehe [Verschlüsselung während der Übertragung zwischen virtuellen Computern](../security/fundamentals/encryption-overview.md#in-transit-encryption-in-vms). |
| Serverseitige Verschlüsselung ruhender Daten: vom Kunden verwaltete Schlüssel (BYOK) | Ja | „Vom Kunden verwaltete Schlüssel“ ist ein unterstütztes Azure-Verschlüsselungsszenario. Siehe [Azure Disk Encryption für VM-Skalierungsgruppen](disk-encryption-overview.md).|
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |
| Verschlüsselte API-Aufrufe| Ja | Per HTTPS und TLS |

## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitskontrolle | Ja/Nein | Notizen|
|---|---|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja |  | 

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [integrierten Sicherheitskontrollen in Azure-Diensten](../security/fundamentals/security-controls.md).
