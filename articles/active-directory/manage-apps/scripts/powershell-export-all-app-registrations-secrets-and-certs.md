---
title: 'PowerShell-Beispiel: Exportieren von Geheimnissen und Zertifikaten für App-Registrierungen im Azure Active Directory-Mandanten'
description: PowerShell-Beispiel, das alle Geheimnisse und Zertifikate für die angegebenen App-Registrierungen im Azure Active Directory-Mandanten exportiert
services: active-directory
author: mtillman
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: sample
ms.date: 03/09/2021
ms.author: mtillman
ms.reviewer: mifarca
ms.openlocfilehash: e358d4315a9f5095785e87f702ecad0c8448e1fd
ms.sourcegitcommit: 3bb9f8cee51e3b9c711679b460ab7b7363a62e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112076850"
---
# <a name="export-secrets-and-certificates-for-app-registrations"></a>Exportieren von Geheimnissen und Zertifikaten für App-Registrierungen

Mit diesem PowerShell-Skriptbeispiel werden alle Geheimnisse und Zertifikate für die angegebenen App-Registrierungen aus Ihrem Verzeichnis in eine CSV-Datei exportiert.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

Für dieses Beispiel ist das [Azure AD PowerShell V2-Modul für Graph](/powershell/azure/active-directory/install-adv2) (AzureAD) oder das [Azure AD PowerShell V2-Modul in der Vorschauversion für Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview&preserve-view=true) (AzureADPreview) erforderlich.

## <a name="sample-script"></a>Beispielskript

[!code-azurepowershell[main](~/powershell_scripts/application-management/export-all-app-registrations-secrets-and-certs.ps1 "Exports all secrets and certificates for the specified app registrations in your directory.")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Mit dem Befehl „Add-Member“ werden die Spalten in der CSV-Datei erstellt.
Sie können die Variable „$Path“ direkt in PowerShell mit einem CSV-Dateipfad ändern, falls der Export nicht interaktiv sein soll.

| Get-Help | Notizen |
|---|---|
| [Get-AzureADApplication](/powershell/module/azuread/get-azureadapplication) | Ruft eine Anwendung aus Ihrem Verzeichnis ab. |
| [Get-AzureADApplicationOwner](/powershell/module/azuread/Get-AzureADApplicationOwner) | Ruft die Besitzer einer Anwendung aus Ihrem Verzeichnis ab. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure AD PowerShell-Modul finden Sie in der [Übersicht über das Azure AD PowerShell-Modul](/powershell/azure/active-directory/overview).

Weitere PowerShell-Beispiele für die Anwendungsverwaltung finden Sie unter [Azure AD PowerShell-Beispiele für die Anwendungsverwaltung](../app-management-powershell-samples.md).
