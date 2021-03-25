---
title: Empfehlungsdigest für Azure Advisor
description: Erhalten einer regelmäßigen Zusammenfassung für Ihre aktiven Empfehlungen
ms.topic: article
ms.date: 03/16/2020
ms.author: sagupt
ms.openlocfilehash: 1c82abf3c8bea07941f860b94fcfe92954fb7641
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "100579811"
---
# <a name="configure-periodic-summary-for-recommendations"></a>Konfigurieren einer regelmäßigen Zusammenfassung für Empfehlungen

Advisor-**Empfehlungsdigests** bieten eine einfache und proaktive Möglichkeit, den Überblick über Ihre aktiven Empfehlungen in verschiedenen Kategorien zu behalten. Die-Funktion bietet die Möglichkeit, regelmäßige Benachrichtigungen für die Zusammenfassung aller ihrer aktiven Empfehlungen in verschiedenen Kategorien zu konfigurieren. Sie können den gewünschten Kanal für Benachrichtigungen wie E-Mail, SMS oder andere Optionen mithilfe von Aktionsgruppen auswählen. In diesem Artikel erfahren Sie, wie Sie **Empfehlungsdigests** für Ihre Advisor-Empfehlungen einrichten.


## <a name="setting-up-your-recommendation-digest"></a>Einrichten eines Empfehlungsdigests 

Die Unterstützung bei der Erstellung von **Empfehlungsdigests** hilft Ihnen beim Konfigurieren der Zusammenfassung. Sie können die folgenden Parameter für Konfigurationen auswählen:
1. Kategorie: Wir verwenden Empfehlungskategorien wie Kosten, Hochverfügbarkeit, Leistung und optimaler Betrieb. Die Funktion ist noch nicht für Sicherheitsempfehlungen verfügbar.
2. Häufigkeit des Digests: Die Häufigkeit für die Zusammenfassungsbenachrichtigungen kann wöchentlich, zweiwöchentlich und monatlich sein.
3. Aktionsgruppe: Sie können eine vorhandene Aktionsgruppe auswählen oder eine neue Aktionsgruppe erstellen. Weitere Informationen zu Aktionsgruppen finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](../azure-monitor/alerts/action-groups.md).
4. Sprache für den Digest
5. Name des Empfehlungsdigests: Sie können eine benutzerfreundliche Zeichenfolge verwenden, um die Digests besser nachverfolgen und überwachen zu können.

## <a name="steps-to-create-recommendation-digest-in-azure-portal"></a>Schritte zum Erstellen eines Empfehlungsdigests im Azure-Portal

Mit den folgenden Schritten erstellen Sie einen **Empfehlungsdigest**:
* **Schritt 1:** Navigieren Sie im Azure-Portal zu **Advisor**, und wählen Sie im Abschnitt **Überwachung** die Option **Empfehlungsdigest** aus. 

   ![Einstiegspunkt des Empfehlungsdigests](./media/digest-0.png)

* **Schritt 2:** Wählen Sie **Neuer Empfehlungsdigest** in der oberen Leiste aus, wie unten gezeigt:

   ![Erstellen eines Empfehlungsdigests](./media/digest-5.png)

* **Schritt 3:** Wählen Sie im Abschnitt **Bereich** das **Abonnement** für den Digest aus.

   ![Bereitstellen von Eingaben für den Empfehlungsdigest](./media/digest-1.png)

* **Schritt 4:** Wählen Sie im Abschnitt **Bedingung** Konfigurationen wie **Kategorie**, **Häufigkeit** und **Sprache** aus.

   ![Bereitstellen von Eingabebedingungen für den Empfehlungsdigest](./media/digest-2.png)

* **Schritt 5:** Wählen Sie im Abschnitt **Aktionsgruppe** die **Aktionsgruppe** für den Digest aus. Weitere Informationen finden Sie unter [Erstellen und Verwalten von Aktionsgruppen](../azure-monitor/alerts/action-groups.md).

   ![Bereitstellen einer Aktionsgruppe für Eingaben für den Empfehlungsdigest](./media/digest-3.png)

* **Schritt 6:** In diesem abschließenden Abschnitt für **Digestdetails** können Sie dem Empfehlungsdigest einen Namen und Status zuweisen. Klicken Sie auf **Empfehlungsdigest erstellen**, um die Einrichtung abzuschließen.
   ![Abschließen der Erstellung des Empfehlungsdigests](./media/digest-4.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Advisor-Empfehlungen finden Sie unter:
* [Einführung in Azure Advisor](advisor-overview.md)
* [Erste Schritte mit Advisor](advisor-get-started.md)
* [Advisor-Empfehlungen zu Kosten](advisor-cost-recommendations.md)
* [Advisor-Empfehlungen zur Leistung](advisor-performance-recommendations.md)
* [Advisor-Empfehlungen zur Sicherheit](advisor-security-recommendations.md)
* [Advisor-Empfehlungen für einen optimalen Betrieb](advisor-operational-excellence-recommendations.md)
* [Advisor-REST-API](/rest/api/advisor/)
