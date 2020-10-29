---
title: Bearbeiten Ihrer Gruppeninformationen – Azure Active Directory | Microsoft-Dokumentation
description: Hier finden Sie Anweisungen zum Bearbeiten Ihrer Gruppeninformationen in Azure Active Directory.
services: active-directory
author: ajburnle
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: how-to
ms.date: 08/27/2018
ms.author: ajburnle
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: c346877d29c5fef03b45c895cbf8d462f90d0979
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2020
ms.locfileid: "92369130"
---
# <a name="edit-your-group-information-using-azure-active-directory"></a>Bearbeiten Ihrer Gruppeninformationen in Azure Active Directory

In Azure Active Directory (Azure AD) können Sie die Einstellungen einer Gruppe bearbeiten. Dies umfasst das Aktualisieren des jeweiligen Namens, der Beschreibung oder des Mitgliedschaftstyps.

## <a name="to-edit-your-group-settings"></a>So bearbeiten Sie Ihre Gruppeneinstellungen
1. Melden Sie sich mit dem Konto eines globalen Administrators für das Verzeichnis beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie **Azure Active Directory** und dann **Gruppen** aus.

    Die Seite **Gruppen – Alle Gruppen** wird mit allen Ihren aktiven Gruppen angezeigt.

3. Geben Sie auf der Seite **Gruppen – Alle Gruppen** so viel von dem Gruppennamen in das Feld **Suche** ein, wie möglich. Für die Zwecke dieses Artikels suchen wir nach der Gruppe **MDM policy – West** (MDM-Richtlinie – Westen).

    Die Suchergebnisse werden unterhalb des Felds **Suche** angezeigt. Sie werden aktualisiert, während Sie mehr Zeichen eingeben.

    ![Seite „Alle Gruppen“ mit Suchtext im Suchfeld](media/active-directory-groups-settings-azure-portal/search-for-specific-group.png)

4. Wählen Sie die Gruppe **MDM policy – West** (MDM-Richtlinie – Westen) aus, und wählen Sie dann **Eigenschaften** im Bereich **Verwalten** aus.

    ![Seite „Gruppenübersicht“ mit hervorgehobener Option „Mitglied“ und weiteren Informationen](media/active-directory-groups-settings-azure-portal/group-overview-blade.png)

5. Aktualisieren Sie die Informationen unter **Allgemeine Einstellungen** nach Bedarf, einschließlich:

    ![Einstellungen für die Eigenschaften einer Gruppe](media/active-directory-groups-settings-azure-portal/group-properties-settings.png)

    - **Gruppenname.** Bearbeiten Sie den vorhandenen Gruppennamen.
    
    - **Gruppenbeschreibung** : Bearbeiten Sie die vorhandene Gruppenbeschreibung.

    - **Gruppentyp.** Sie können den Typ der Gruppe nach deren Erstellung nicht mehr ändern. Um den **Gruppentyp** zu ändern, müssen Sie die Gruppe löschen und eine neue erstellen.
    
    - **Mitgliedschaftstyp.** Ändern Sie den Typs der Mitgliedschaft. Weitere Informationen zu den verschiedenen verfügbaren Mitgliedschaften finden Sie unter [ Erstellen einer Basisgruppe und Hinzufügen von Mitgliedern mit Azure Active Directory](active-directory-groups-create-azure-portal.md).
    
    - **Objekt-ID.** Sie können die Objekt-ID nicht ändern, aber Sie können sie kopieren, um sie in Ihren PowerShell-Befehlen für die Gruppe zu verwenden. Weitere Informationen zur Verwendung von PowerShell-Cmdlets finden Sie unter [Azure Active Directory-Cmdlets zum Konfigurieren von Gruppeneinstellungen](../enterprise-users/groups-settings-v2-cmdlets.md).

## <a name="next-steps"></a>Nächste Schritte
Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

- [Anzeigen Ihrer Gruppen und Mitglieder](active-directory-groups-view-azure-portal.md)

- [Erstellen einer einfachen Gruppe und Hinzufügen von Mitgliedern](active-directory-groups-create-azure-portal.md)

- [Hinzufügen oder Entfernen von Mitgliedern zu/aus einer Gruppe](active-directory-groups-members-azure-portal.md)

- [Verwalten dynamischer Regeln für Benutzer in einer Gruppe](../enterprise-users/groups-create-rule.md)

- [Verwalten der Mitgliedschaften einer Gruppe](active-directory-groups-membership-azure-portal.md)

- [Verwalten des Zugriffs auf Ressourcen mittels Gruppen](active-directory-manage-groups.md)

- [Zuweisen oder Hinzufügen eines Azure-Abonnements zu Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
