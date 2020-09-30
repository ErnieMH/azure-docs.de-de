---
title: 'Tutorial: Reagieren auf Incidents – Azure Security Center'
description: In diesem Tutorial erfahren Sie, wie Sie Sicherheitswarnungen selektieren, die Grundursache und den Bereich eines Vorfalls ermitteln und Sicherheitsdaten durchsuchen.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 181e3695-cbb8-4b4e-96e9-c4396754862f
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/30/2018
ms.author: memildin
ms.openlocfilehash: 08e04749eae7158abb501f9a4d127cdd7a89a391
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91336274"
---
# <a name="tutorial-respond-to-security-incidents"></a>Tutorial: Reagieren auf Sicherheitsvorfälle
Security Center analysiert Ihre Hybrid Cloud-Workloads ständig mithilfe von Advanced Analytics- und Threat Intelligence-Funktionen, um Sie vor schädlichen Aktivitäten warnen zu können. Darüber hinaus können Sie Warnungen aus anderen Sicherheitsprodukten und Diensten in Security Center integrieren und basierend auf Ihren eigenen Indikatoren oder Intelligence-Quellen benutzerdefinierte Warnungen erstellen. Nachdem eine Warnung generiert wurde, sind schnelle Maßnahmen erforderlich, um das Problem zu untersuchen und zu beheben. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Selektieren von Sicherheitswarnungen
> * Durchführen von weiteren Untersuchungen, um die Grundursache und den Umfang eines Sicherheitsincidents zu ermitteln
> * Durchsuchen von Sicherheitsdaten zur Unterstützung der Untersuchung

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen
Zum Durchlaufen der in diesem Tutorial behandelten Features muss Azure Defender aktiviert sein. Sie können Azure Defender kostenlos testen. Weitere Informationen finden Sie auf der [Preisseite](https://azure.microsoft.com/pricing/details/security-center/). In der Schnellstartanleitung [Erste Schritte mit Security Center](security-center-get-started.md) werden die erforderlichen Schritte für das Upgrade erläutert.

## <a name="scenario"></a>Szenario
Contoso hat vor Kurzem einige lokale Ressourcen zu Azure migriert. Darunter sind auch einige VM-basierte Branchenworkloads und SQL-Datenbanken. Das Core Computer Security Incident Response Team (CSIRT) von Contoso hat ein Problem mit der Untersuchung von Sicherheitsvorfällen, da Security Intelligence-Funktionen nicht in die aktuellen Tools für die Reaktion auf Vorfälle integriert sind. Diese unzureichende Integration führt zu einem Problem während der Erkennungsphase (zu viele falsch positive Ergebnisse) und auch während der Bewertungs- und Diagnosephase. Im Rahmen der Migration wurde die Entscheidung getroffen, Security Center zu verwenden, um dieses Problem zu beheben.

Die erste Phase dieser Migration endete nach der Einbindung aller Ressourcen und der Umsetzung aller Sicherheitsempfehlungen aus Security Center. Contoso CSIRT ist der zentrale Anlaufpunkt für Vorfälle, die sich auf die Computersicherheit beziehen. Das Team besteht aus einer Gruppe von Personen, die für die Bearbeitung aller Sicherheitsvorfälle zuständig sind. Die Teammitglieder haben klar definierte Pflichten, damit sichergestellt ist, dass kein Verantwortungsbereich offen bleibt.

In diesem Szenario konzentrieren wir uns auf die Rollen der folgenden „Personae“, die Teil des Contoso CSIRT sind:

![Lebenszyklus der Reaktion auf Vorfälle](./media/tutorial-security-incident/security-center-incident-response.png)

Judy arbeitet im Security Operations-Bereich. Zu ihren Aufgaben zählen:

* Überwachen von und Reagieren auf Sicherheitsbedrohungen rund um die Uhr
* Eskalieren an den Besitzer der Cloudworkload oder den Sicherheitsanalysten (falls erforderlich)

Sam ist Security Analyst und für folgende Aufgaben verantwortlich:

* Untersuchen von Angriffen
* Lösen von Warnungen
* Zusammenarbeiten mit Besitzern von Workloads zur Ermittlung und Anwendung von Lösungen

Sie sehen, dass Judy und Sam für unterschiedliche Aufgaben verantwortlich sind und zusammenarbeiten müssen, um Informationen aus Security Center auszutauschen.

## <a name="triage-security-alerts"></a>Selektieren von Sicherheitswarnungen
Mit Security Center erhalten Sie einen einheitlichen Überblick über alle Sicherheitswarnungen. Sicherheitswarnungen werden basierend auf dem Schweregrad und bei einer möglichen Kombination von verwandten Warnungen in einem Sicherheitsincident eingestuft. Beachten Sie beim Selektieren von Warnungen und Incidents Folgendes:

- Schließen Sie Warnungen, für die keine zusätzliche Aktion erforderlich ist, z.B. bei einer falsch positiven Warnung.
- Treffen Sie Vorkehrungen zur Begegnung von bekannten Angriffen, z.B. Blockierung von Netzwerkdatenverkehr von einer schädlichen IP-Adresse.
- Ermitteln Sie Warnungen, die näher untersucht werden müssen.


1. Wählen Sie im Hauptmenü von Security Center unter **ERKENNUNG** die Option **Sicherheitswarnungen**:

   ![Sicherheitswarnungen](./media/tutorial-security-incident/tutorial-security-incident-fig1.png)

2. Wählen Sie in der Liste mit den Warnungen einen Sicherheitsincident (eine Sammlung von Warnungen) aus, um weitere Informationen zu diesem Incident zu erhalten. **Security incident detected** (Sicherheitsincident erkannt) wird geöffnet.

   ![Sicherheitsvorfall erkannt](./media/tutorial-security-incident/tutorial-security-incident-fig2.png)

3. Auf diesem Bildschirm ist oben die Beschreibung des Sicherheitsincidents und die Liste mit den Warnungen angegeben, die Teil des Incidents sind. Klicken Sie auf die Warnung, die Sie näher untersuchen möchten, um weitere Informationen zu erhalten.

   ![Warnungsdetails des Incidents](./media/tutorial-security-incident/tutorial-security-incident-fig3.png)

   Die Art der Warnung kann variieren. Weitere Informationen zur Art der Warnung und zu potenziellen Lösungsschritten finden Sie unter [Verstehen der Sicherheitswarnungen in Azure Security Center](security-center-alerts-type.md). Für Warnungen, die ohne Probleme geschlossen werden können, können Sie mit der rechten Maustaste auf die Warnung klicken und die Option **Schließen** wählen:

   ![Warnung](./media/tutorial-security-incident/tutorial-security-incident-fig4.png)

4. Wenn die Grundursache und der Umfang der schädlichen Aktivität unbekannt sind, können Sie mit dem nächsten Schritt fortfahren, um dies weiter zu untersuchen.

## <a name="investigate-an-alert-or-incident"></a>Untersuchen einer Warnung oder eines Incidents
1. Klicken Sie auf der Seite **Sicherheitswarnung** auf die Schaltfläche **Untersuchung starten**. (Falls Sie den Vorgang bereits gestartet haben, ändert sich der Name in **Untersuchung fortsetzen**.)

   ![Untersuchung](./media/tutorial-security-incident/tutorial-security-incident-fig5.png)

   Der Bereich für die Untersuchung ist eine grafische Darstellung der Entitäten (in Form einer Übersichtskarte), die mit dieser Sicherheitswarnung bzw. dem Sicherheitsincident verbunden sind. Wenn Sie in diesem Bereich auf eine Entität klicken, werden in den Informationen zur Entität die neuen Entitäten angezeigt, und der Bereich wird erweitert. Für die Entität, die im Bereich ausgewählt ist, werden die Eigenschaften rechts auf der Seite hervorgehoben. Die Informationen, die auf den einzelnen Registerkarten verfügbar sind, variieren je nach der ausgewählten Entität. Überprüfen Sie während des Untersuchungsvorgangs alle relevanten Informationen, um die Bewegungen des Angreifers besser zu verstehen.

2. Fahren Sie mit dem nächsten Schritt fort, wenn Sie mehr Beweise benötigen oder Entitäten, die während der Untersuchung gefunden wurden, näher untersuchen möchten.

## <a name="search-data-for-investigation"></a>Durchsuchen von Daten zu Untersuchungszwecken

Sie können Suchfunktionen in Security Center verwenden, um weitere Beweise für kompromittierte Systeme und mehr Details zu den Entitäten zu finden, die Teil der Untersuchung sind.

Öffnen Sie zum Durchführen einer Suche das Dashboard **Security Center**, klicken Sie im linken Navigationsbereich auf **Suchen**, wählen Sie den Arbeitsbereich aus, der die zu durchsuchenden Entitäten enthält, geben Sie die Suchabfrage ein, und klicken Sie auf die Suchschaltfläche.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Schnellstartanleitungen und Tutorials in dieser Sammlung bauen auf dieser Schnellstartanleitung auf. Falls Sie weitere Schnellstartanleitungen und Tutorials durchgehen möchten, lassen Sie die automatische Bereitstellung sowie Azure Defender aktiviert. Falls Sie keine weiteren Schritte ausführen oder Azure Defender deaktivieren möchten, gehen Sie wie folgt vor:

1. Kehren Sie zum Hauptmenü von Security Center zurück, und wählen Sie **Preise und Einstellungen** aus.
1. Wählen Sie das Abonnement aus, das Sie herabstufen möchten.
1. Legen Sie **Azure Defender** auf „Aus“ fest.
1. Wählen Sie **Speichern** aus.

Gehen Sie wie folgt vor, um die automatische Bereitstellung zu deaktivieren:

1. Kehren Sie zum Hauptmenü von Security Center zurück, und wählen Sie die Option **Sicherheitsrichtlinie**.
2. Wählen Sie das Abonnement aus, für das Sie die automatische Bereitstellung deaktivieren möchten.
3. Wählen Sie im Bereich **Sicherheitsrichtlinie – Datensammlung** unter **Onboarding** die Option **Aus**, um die automatische Bereitstellung zu deaktivieren.
4. Wählen Sie **Speichern** aus.

>[!NOTE]
> Wenn Sie die automatische Bereitstellung deaktivieren, wird der Log Analytics-Agent nicht von virtuellen Azure-Computern entfernt, auf denen der Agent bereitgestellt wurde. Wenn Sie die automatische Bereitstellung deaktivieren, schränkt dies die Sicherheitsüberwachung für Ihre Ressourcen ein.
>

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Informationen zu Security Center-Features erhalten, die zum Reagieren auf einen Sicherheitsincident verwendet werden, z.B.:

> [!div class="checklist"]
> * Sicherheitsincident, bei dem es sich um eine Aggregation von verwandten Warnungen für eine Ressource handelt
> * Bereich für die Untersuchung, bei dem es sich um eine grafische Darstellung der Entitäten handelt, die mit einer Sicherheitswarnung bzw. einem Sicherheitsincident verbunden sind
> * Suchfunktionen zur Ermittlung von weiteren Beweisen für kompromittierte Systeme