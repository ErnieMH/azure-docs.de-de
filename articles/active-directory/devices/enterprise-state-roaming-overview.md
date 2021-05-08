---
title: Was ist Enterprise State Roaming in Azure Active Directory?
description: Bei Enterprise State Roaming profitieren Benutzer von einer einheitlichen Benutzererfahrung auf allen Windows-Geräten.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: overview
ms.date: 02/12/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: na
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5cd64a9372f8f987ae79ed6c2927d179d71d141c
ms.sourcegitcommit: fc9fd6e72297de6e87c9cf0d58edd632a8fb2552
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108286580"
---
# <a name="what-is-enterprise-state-roaming"></a>Was ist Enterprise State Roaming?

Unter Windows 10 haben Benutzer von [Azure Active Directory (Azure AD)](../fundamentals/active-directory-whatis.md) die Möglichkeit, ihre Benutzereinstellungen und die Einstellungsdaten ihrer Anwendungen sicher zur Cloud zu synchronisieren. Beim Enterprise State Roaming profitieren Benutzer von einer einheitlichen Benutzererfahrung auf allen Windows-Geräten. Zudem wird für das Konfigurieren eines neuen Geräts weniger Zeit benötigt. Enterprise State Roaming funktioniert ähnlich wie die standardmäßige [Synchronisierung von Verbrauchereinstellungen,](https://go.microsoft.com/fwlink/?linkid=2015135) die unter Windows 8 eingeführt wurde. Enterprise State Roaming bietet außerdem:

* **Trennung von Unternehmens- und Verbraucherdaten**. Unternehmen haben die Kontrolle über ihre Daten, und es erfolgt kein Vermischen von Unternehmensdaten in einem Verbraucher-Cloudkonto oder Verbraucherdaten in einem Unternehmens-Cloudkonto.
* **Erweiterte Sicherheit.** Bevor Daten das Windows 10-Gerät des Benutzers verlassen, werden sie mithilfe von Azure Rights Management (Azure RMS) automatisch verschlüsselt. Mit Ausnahme der Namespaces – wie die Namen von Einstellungen und Windows-Apps – bleiben alle Inhalte, die sich im Ruhezustand in der Cloud befinden, auch verschlüsselt.  
* **Bessere Verwaltung und Überwachung.** Dank Azure AD-Portalintegration können Sie besser nachvollziehen und steuern, wer in Ihrem Unternehmen Einstellungen auf welchen Geräten synchronisiert. 

Enterprise State Roaming ist in vielen Azure-Regionen verfügbar. Die aktuelle Liste mit verfügbaren Regionen finden Sie auf der Seite [Azure-Dienste nach Region](https://azure.microsoft.com/regions/#services) unter „Azure Active Directory“.

| Artikel | BESCHREIBUNG |
| --- | --- |
| [Aktivieren von Enterprise State Roaming in Azure Active Directory](enterprise-state-roaming-enable.md) |Enterprise State Roaming ist für alle Organisationen mit einem Azure Active Directory Premium-Abonnement (Azure AD) verfügbar. Weitere Informationen zum Erhalt eines Azure AD-Abonnements finden Sie auf der [Azure AD-Produktseite](https://azure.microsoft.com/services/active-directory). |
| [Roaming von Einstellungen und Daten: Häufig gestellte Fragen](enterprise-state-roaming-faqs.yml) |Dieser Artikel enthält Antworten auf Fragen zur Synchronisierung von Einstellungen und App-Daten, die von IT-Administratoren häufig gestellt werden. |
| [Gruppenrichtlinien- und MDM-Einstellungen für Einstellungssynchronisierung](enterprise-state-roaming-group-policy-settings.md) |Windows 10 bietet Gruppenrichtlinien- und MDM-Einstellungen (Mobile Device Management, Verwaltung mobiler Geräte), um die Synchronisierung von Einstellungen zu beschränken. |
| [Windows 10-Roamingeinstellungen – Referenz](enterprise-state-roaming-windows-settings-reference.md) |Eine Liste der Einstellungen, für die unter Windows 10 das Roaming und/oder eine Sicherung durchgeführt wird |
| [Problembehandlung](enterprise-state-roaming-troubleshooting.md) |In diesem Artikel werden einige grundlegende Schritte zur Problembehandlung erläutert. Außerdem finden Sie hier eine Liste der bekannten Probleme. |

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Aktivieren von Enterprise State Roaming finden Sie unter [Aktivieren von Enterprise State Roaming](enterprise-state-roaming-enable.md).
