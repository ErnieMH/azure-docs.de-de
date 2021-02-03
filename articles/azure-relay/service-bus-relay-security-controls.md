---
title: Sicherheitskontrollen für Azure Relay
description: In diesem Artikel wird eine Checkliste mit integrierten Sicherheitskontrollen für die Überprüfung von Azure Relay bereitgestellt.
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: ce5053366ac1d3536a152610d8ed7f76fad62b84
ms.sourcegitcommit: 436518116963bd7e81e0217e246c80a9808dc88c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/27/2021
ms.locfileid: "98919577"
---
# <a name="security-controls-for-azure-relay"></a>Sicherheitskontrollen für Azure Relay

In diesem Artikel werden die in Azure Relay integrierten Sicherheitskontrollen beschrieben.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Netzwerk

| Sicherheitskontrolle | Ja/Nein | Notizen | Dokumentation |
|---|---|--|--|
| Unterstützung für private Endpunkte| Ja |  |   |
| Unterstützung von Netzwerkisolation und Firewall| Ja |  |   |
| Unterstützung der Tunnelerzwingung| – | Relay ist der TLS-Tunnel  |   |

## <a name="monitoring--logging"></a>Überwachung und Protokollierung

| Sicherheitskontrolle | Ja/Nein | Notizen| Dokumentation |
|---|---|--|--|
| Unterstützung der Azure-Überwachung (Log Analytics, Application Insights usw.)| Ja | |   |
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Über [Azure Resource Manager](../azure-resource-manager/index.yml). |   |
| Protokollierung und Überwachung auf Datenebene| Ja | Verbindungserfolg/-fehler und Fehler werden protokolliert.  |   |

## <a name="identity"></a>Identity

| Sicherheitskontrolle | Ja/Nein | Notizen| Dokumentation |
|---|---|--|--|
| Authentifizierung| Ja | Über SAS. | [Azure Relay-Authentifizierung und -Autorisierung](relay-authentication-and-authorization.md) |
| Authorization|  Ja | Über SAS. | [Azure Relay-Authentifizierung und -Autorisierung](relay-authentication-and-authorization.md) |

## <a name="data-protection"></a>Schutz von Daten

| Sicherheitskontrolle | Ja/Nein | Notizen | Dokumentation |
|---|---|--|--|
| Serverseitige Verschlüsselung ruhender Daten: Von Microsoft verwaltete Schlüssel |  – | Relay ist ein Websocket, das Daten nicht beibehält. |   |
| Serverseitige Verschlüsselung ruhender Daten: vom Kunden verwaltete Schlüssel (BYOK) | Nein | Verwendet nur Microsoft-TLS-Zertifikate.  |   |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| – | |   |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, VNET-Verschlüsselung und VNET-VNET-Verschlüsselung)| Ja | Der Dienst erfordert TLS. |   |
| Verschlüsselte API-Aufrufe| Ja | HTTPS. |


## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitskontrolle | Ja/Nein | Notizen| Dokumentation |
|---|---|--|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja | Über [Azure Resource Manager](../azure-resource-manager/index.yml).|   |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [integrierten Sicherheitskontrollen in Azure-Diensten](../security/fundamentals/security-controls.md).