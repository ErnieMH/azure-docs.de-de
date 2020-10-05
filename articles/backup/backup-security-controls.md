---
title: Sicherheitskontrollen
description: Erfahren Sie mehr über die Sicherheitskontrollen, die im Azure Backup-Dienst verwendet werden. Diese Kontrollen helfen dem Dienst, Sicherheitsrisiken zu verhindern, zu erkennen und darauf zu reagieren.
ms.topic: conceptual
ms.date: 09/23/2019
ms.openlocfilehash: 7ff3ff5c1b024a228778b0214e67239d3c8ab721
ms.sourcegitcommit: 9c262672c388440810464bb7f8bcc9a5c48fa326
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2020
ms.locfileid: "89418754"
---
# <a name="security-controls-for-azure-backup"></a>Sicherheitskontrollen für Azure Backup

In diesem Artikel werden die in Azure Backup integrierten Sicherheitskontrollen beschrieben.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Netzwerk

| Sicherheitskontrolle | Ja/Nein | Notizen | Dokumentation
|---|---|--|--|
| Unterstützung des Dienstendpunkts| Nein |  |  |
| Unterstützung der VNet-Einschleusung| Nein |  |  |
| Unterstützung von Netzwerkisolation und Firewall| Ja | |  |
| Unterstützung der Tunnelerzwingung für virtuelle Azure-Computer | Ja  |  |  |
| Unterstützung für Tunnelerzwingung für Anwendungen, die auf virtuellen Azure-Computern ausgeführt werden| Nein  |  |  |

## <a name="monitoring--logging"></a>Überwachung und Protokollierung

| Sicherheitskontrolle | Ja/Nein | Notizen| Dokumentation
|---|---|--|--|
| Unterstützung der Azure-Überwachung (z. B. Log Analytics, App Insights usw.)| Ja | Log Analytics wird über Ressourcenprotokolle unterstützt. Weitere Informationen finden Sie unter [Überwachen von Azure Backup-geschützten Workloads mit Log Analytics](https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/). |  |
| Protokollierung und Überwachung auf Steuerungs- und Verwaltungsebene| Ja | Alle vom Kunden über das Azure-Portal ausgelösten Aktionen werden in Aktivitätsprotokollen protokolliert. |  |
| Protokollierung und Überwachung auf Datenebene| Nein | Die Azure Backup-Datenebene kann nicht direkt aufgerufen werden.  |  |

## <a name="identity"></a>Identity

| Sicherheitskontrolle | Ja/Nein | Notizen| Dokumentation
|---|---|--|--|
| Authentifizierung| Ja | Die Authentifizierung erfolgt über Azure Active Directory. |  |
| Authorization| Ja | Es werden vom Kunden erstellte und integrierte Azure-Rollen verwendet. Weitere Informationen finden Sie unter [Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten von Azure Backup-Wiederherstellungspunkten](./backup-rbac-rs-vault.md). |  |

## <a name="data-protection"></a>Schutz von Daten

| Sicherheitskontrolle | Ja/Nein | Notizen | Dokumentation
|---|---|--|--|
| Serverseitige Verschlüsselung ruhender Daten: Von Microsoft verwaltete Schlüssel | Ja | Verwenden der Speicherdienstverschlüsselung für Speicherkonten |  |
| Serverseitige Verschlüsselung ruhender Daten: vom Kunden verwaltete Schlüssel (BYOK) | Nein |  |  |
| Verschlüsselung auf Spaltenebene (Azure Data Services)| Nein |  |  |
| Verschlüsselung während der Übertragung (z. B. ExpressRoute-Verschlüsselung, VNET-Verschlüsselung und VNET-VNET-Verschlüsselung)| Nein | Mithilfe von HTTPS |  |
| Verschlüsselte API-Aufrufe| Ja |  |  |

## <a name="configuration-management"></a>Konfigurationsverwaltung

| Sicherheitskontrolle | Ja/Nein | Notizen| Dokumentation
|---|---|--|--|
| Unterstützung der Konfigurationsverwaltung (Versionsverwaltung der Konfiguration usw.)| Ja|  |  |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über die [integrierten Sicherheitskontrollen in Azure-Diensten](../security/fundamentals/security-controls.md).
