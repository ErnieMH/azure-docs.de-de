---
title: Überwachen von geschützten Azure Backup-Workloads
description: In diesem Artikel erfahren Sie mehr über die Überwachungs- und Benachrichtigungsfunktionen für Azure Backup-Workloads im Azure-Portal.
ms.topic: conceptual
ms.date: 03/05/2019
ms.assetid: 86ebeb03-f5fa-4794-8a5f-aa5cbbf68a81
ms.openlocfilehash: d04f57c19e31b946f7c360edb796bc4f0f5fcf71
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89377402"
---
# <a name="monitoring-azure-backup-workloads"></a>Überwachen von Azure Backup-Workloads

Azure Backup enthält mehrere Sicherheitslösungen, deren Einsatz von den Anforderungen an die Sicherheit und der Infrastrukturtopologie (lokal oder Azure) abhängt. Alle Benutzer und Administratoren der Sicherung sollten die Vorgänge in allen Lösungen verfolgen können und in wichtigen Fällen benachrichtigt werden. In diesem Artikel werden die Überwachungs- und Benachrichtigungsfunktionen in Azure Backup näher erläutert.

## <a name="backup-jobs-in-recovery-services-vault"></a>Sicherungsaufträge im Recovery Services-Tresor

Azure Backup bietet integrierte Überwachungs- und Warnungsfunktionen für durch Azure Backup geschützte Workloads. In den Einstellungen für den Recovery Services-Tresor finden Sie im Abschnitt **Überwachung** die integrierten Aufträge und Warnungen.

![Integrierte Überwachung im Recovery Services-Tresor](media/backup-azure-monitoring-laworkspace/rs-vault-inbuiltmonitoring.png)

Aufträge werden generiert, wenn Vorgänge wie das Konfigurieren der Sicherung, die Sicherung, Wiederherstellung oder das Löschen der Sicherung durchgeführt werden.

Im Folgenden werden die Aufträge der folgenden Azure Backup-Lösungen vorgestellt:

- Azure VM Backup
- Azure-Dateisicherung
- Azure-Workloadsicherung wie SQL und SAP HANA
- Microsoft Azure Recovery Services-Agent (MARS)

Aufträge aus System Center Data Protection Manager (SC-DPM) und Microsoft Azure Backup Server (MABS) werden nicht aufgeführt.

> [!NOTE]
> Azure-Workloads wie SQL- und SAP HANA-Sicherungen in Azure-VMs umfassen viele Sicherungsaufträge. Protokollsicherungen können beispielsweise alle 15 Minuten ausgeführt werden. Für solche Datenbankworkloads werden darum nur vom Benutzer ausgelöste Vorgänge angezeigt. Geplante Sicherungsvorgänge werden nicht angezeigt.

## <a name="backup-alerts-in-recovery-services-vault"></a>Sicherungswarnungen im Recovery Services-Tresor

Bei Warnungen handelt es sich in erster Linie um Szenarios, in denen Benutzer benachrichtigt werden, damit Sie entsprechende Maßnahmen ergreifen können. Im Abschnitt **Sicherungswarnungen** werden die von Azure Backup generierten Warnungen angezeigt. Diese Warnungen werden vom Dienst definiert. Es können keine benutzerdefinierten Warnungen erstellt werden.

### <a name="alert-scenarios"></a>Warnungsszenarios

Die folgenden Szenarios werden vom Dienst als solche definiert, in denen Warnungen ausgelöst werden können.

- Fehler bei der Sicherung/Wiederherstellung
- Sicherung für den Microsoft Azure Recovery Services-Agent (MARS) mit Warnungen abgeschlossen
- Beenden des Schutzes unter Beibehaltung der Daten/Beenden des Schutzes inklusive Löschung der Daten

### <a name="alerts-from-the-following-azure-backup-solutions-are-shown-here"></a>Hier sehen Sie die Warnungen der folgenden Azure Backup-Lösungen:

- Azure-VM-Sicherungen
- Azure-Dateisicherungen
- Azure-Workloadsicherungen wie SQL, SAP HANA
- Microsoft Azure Recovery Services-Agent (MARS)

> [!NOTE]
> Warnungen aus System Center Data Protection Manager (SC-DPM) und Microsoft Azure Backup Server (MABS) werden hier nicht aufgeführt.

### <a name="consolidated-alerts"></a>Konsolidierte Warnungen

Für Azure-Workloadsicherungslösungen wie SQL und SAP Hana können Protokollsicherungen sehr häufig generiert werden (gemäß der Richtlinie bis zu alle 15 Minuten). Es ist also auch möglich, dass die Fehler bei der Protokollsicherung sehr häufig auftreten (bis zu alle 15 Minuten). In diesem Szenario ist der Endbenutzer überfordert, wenn für jeden auftretenden Fehler eine Warnung ausgelöst wird. Daher wird für das erste Vorkommen eine Warnung gesendet, und wenn spätere Fehler aufgrund derselben Grundursache auftreten, werden keine weiteren Warnungen generiert. Die erste Warnung wird mit der Fehleranzahl aktualisiert. Wenn die Warnung jedoch vom Benutzer deaktiviert wird, löst das nächste Vorkommen eine andere Warnung aus, und diese wird als erste Warnung für dieses Vorkommen behandelt. Auf diese Weise führt Azure Backup eine Warnungskonsolidierung für SQL- und SAP Hana-Sicherungen durch.

### <a name="exceptions-when-an-alert-is-not-raised"></a>Ausnahmen, bei denen keine Warnung ausgelöst wird

Es gibt folgende Ausnahmefälle, in denen keine Warnung ausgelöst wird. Sie lauten wie folgt:

- Wenn der Benutzer den ausgeführten Auftrag explizit abgebrochen hat
- Wenn der Auftrag fehlschlägt, weil ein anderer Sicherungsauftrag gerade ausgeführt wird (keine Handlung erforderlich, da gewartet werden muss, bis der aktuelle Auftrag abgeschlossen ist)
- Wenn der Sicherungsauftrag der VM fehlschlägt, weil die gesicherte Azure-VM nicht mehr existiert
- [Konsolidierte Warnungen](#consolidated-alerts)

Diese Ausnahmen wurden eingeführt, da das Ergebnis der oben aufgeführten Vorgänge, die in erster Linie vom Benutzer ausgelöst werden, direkt im Portal bzw. im PowerShell- oder CLI-Client dargestellt wird. Sie sind dem Benutzer also bekannt, sodass keine Benachrichtigung erforderlich ist.

### <a name="alert-types"></a>Warnungstypen

Je nach Schweregrad der Warnung werden diese drei Arten zugeordnet:

- **Kritisch**: Im Prinzip führt jeder Fehler bei der Sicherung oder Wiederherstellung (geplant oder vom Benutzer ausgelöst) zu einer Warnung, die als „Kritisch“ angezeigt wird. Das gilt auch für destruktive Vorgänge wie das Löschen der Sicherung.
- **Warnung:** Wenn der Sicherungsvorgang erfolgreich durchgeführt wurde, aber einige Warnungen auslöst werden, werden diese als „Warnung“ aufgeführt. Warnungen sind zurzeit nur für Sicherungen des Azure Backup-Agents verfügbar.
- **Informativ:** Derzeit werden keine informativen Warnungen von Azure Backup generiert.

## <a name="notification-for-backup-alerts"></a>Benachrichtigungen für Azure Backup-Warnungen

> [!NOTE]
> Sie können die Benachrichtigungen nur über das Azure-Portal konfigurieren. PowerShell, Befehlszeilenschnittstelle, REST-API und Azure Resource Manager-Vorlagen werden nicht unterstützt.

Wenn eine Warnung ausgelöst wird, werden die Benutzer benachrichtigt. Azure Backup enthält einen Mechanismus, der Benachrichtigungen per E-Mail sendet. Sie können einzelne E-Mail-Adressen oder Verteilerlisten angeben, die benachrichtigt werden sollen, wenn eine Warnung generiert wurde. Sie können ebenfalls Benachrichtigungen für jede einzelne Warnung aktivieren oder diese in einer stündlichen Übersicht gruppieren.

![Im Recovery Services-Tresor integrierte E-Mail-Benachrichtigung](media/backup-azure-monitoring-laworkspace/rs-vault-inbuiltnotification.png)

Wenn Sie die Benachrichtigungen konfiguriert haben, erhalten Sie eine Begrüßungs-E-Mail bzw. eine Einführungs-E-Mail. Dadurch wird bestätigt, dass Azure Backup E-Mails an diese Adressen senden kann, wenn eine Warnung ausgelöst wird.<br>

Wenn die Häufigkeit auf eine stündliche Übersicht festgelegt und eine Warnung innerhalb einer Stunde ausgelöst und behoben wurde, ist diese nicht in der stündlichen Übersicht enthalten.

> [!NOTE]
>
> - Wenn ein destruktiver Vorgang wie das **Beenden des Schutzes inklusive Löschung** der Daten durchgeführt wird, wird eine Warnung ausgelöst und eine E-Mail an den Besitzer des Abonnements sowie an die Administratoren und Co-Admins des Abonnements gesendet, auch wenn die Benachrichtigungen für den Recovery Services-Tresor nicht konfiguriert wurden.
> - Verwenden Sie [Log Analytics](backup-azure-monitoring-use-azuremonitor.md#using-log-analytics-workspace), um Benachrichtigungen für erfolgreiche Aufträge zu konfigurieren.

## <a name="inactivating-alerts"></a>Deaktivieren von Warnungen

Um eine aktive Warnung zu deaktivieren bzw. aufzulösen, können Sie das Listenelement auswählen, das der Warnung entspricht, die Sie deaktivieren möchten. Dadurch wird ein Bildschirm geöffnet, auf dem detaillierte Informationen zur Warnung angezeigt werden. Oberhalb der Warnung finden Sie eine Schaltfläche zum **Deaktivieren**. Wenn Sie diese Schaltfläche auswählen, ändert sich der Status der Warnung zu **Inaktiv**. Sie können eine Warnung auch deaktivieren, indem Sie mit der rechten Maustaste auf das Listenelement klicken, das dieser Warnung entspricht, und dann **Deaktivieren** auswählen.

![Deaktivieren einer Warnung zum Recovery Services-Tresor](media/backup-azure-monitoring-laworkspace/vault-alert-inactivation.png)

## <a name="next-steps"></a>Nächste Schritte

[Überwachen von Azure Backup-Workloads mithilfe von Azure Monitor](backup-azure-monitoring-use-azuremonitor.md)
