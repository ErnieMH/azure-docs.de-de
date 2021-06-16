---
title: Support für das Programm „Kommerzieller Marketplace“ im Partner Center
description: Erfahren Sie mehr über Ihre Supportoptionen für das Programm „Kommerzieller Marketplace“ im Partner Center, einschließlich der Möglichkeit, eine Supportanfrage zu stellen.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: navits09
ms.author: navits
ms.date: 01/19/2020
ms.openlocfilehash: 47ed6f8e8756d8fe317b932edf360b710b58e26b
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110706857"
---
# <a name="support-for-the-commercial-marketplace-program-in-partner-center"></a>Support für das Programm „Kommerzieller Marketplace“ in Partner Center

Microsoft bietet Support für eine Vielzahl von Produkten und Diensten. Um eine hilfreiche und zeitnahe Antwort zu erhalten, muss Ihre Anfrage natürlich das richtige Support-Team erreichen. Die folgenden Szenarien sollen Ihnen dabei helfen, Ihre Anfrage an das richtige Team zu richten:

- Wenn Sie als Herausgeber eine Frage von einem Kunden erhalten haben, bitten Sie ihn darum, über die Supportlinks im  [Azure-Portal](https://portal.azure.com/) Unterstützung anzufordern.
- Wenn Sie ein Herausgeber sind und ein Sicherheitsproblem mit einer in Azure ausgeführten Anwendung festgestellt haben, finden Sie weitere Informationen unter [Erstellen eines Tickets beim Support für Sicherheitsereignisse](../security/fundamentals/event-support-ticket.md). Herausgeber müssen mutmaßliche sicherheitsrelevante Ereignisse einschließlich Sicherheitsincidents und Sicherheitsrisiken, die ihre Azure Marketplace-Software und Dienstangebote betreffen, bei nächstmöglicher Gelegenheit melden.
- Wenn Sie ein Herausgeber sind und eine Frage hinsichtlich Ihrer App oder Ihres Diensts haben, nutzen Sie die folgenden Supportoptionen.

## <a name="get-help-or-open-a-support-ticket"></a>Anfordern von Hilfe oder Eröffnen eines Supporttickets

1. Melden Sie sich mit Ihrem Geschäftskonto an. [Erstellen Sie ein Partner Center-Konto](create-account.md), falls Sie dies noch nicht getan haben.

1. Wählen Sie im Menü oben rechts auf der Seite das Symbol **Support** aus. Der Bereich **Hilfe und Support** wird rechts auf der Seite angezeigt.

1. Wenn Sie Hilfe zum kommerziellen Marketplace benötigen, wählen Sie **Kommerzieller Marketplace** aus.

   ![Dropdownmenü „Support“](./media/support/commercial-marketplace-support-pane.png)

1. Geben Sie im Feld **Problemzusammenfassung** eine kurze Beschreibung des Problems ein.

1. Führen Sie im Feld **Problemtyp** einen der folgenden Schritte aus:

    - **Option 1**: Geben Sie Schlüsselwörter wie die Folgenden ein: Marketplace, Azure-App, SaaS-Angebot, Kontoverwaltung, Leadverwaltung, Bereitstellungsproblem, Auszahlung oder Migration eines Co-Selling-Angebots. Wählen Sie dann in der eingeblendeten Liste mit Empfehlungen einen Problemtyp aus.

    - **Option 2**: Wählen Sie in der Liste **Kategorie** die Option **Themen durchsuchen** aus, und wählen Sie dann **Kommerzieller Marketplace** aus. Wählen Sie dann das geeignete **Thema** und **Unterthema** aus.

1. Nachdem Sie das gewünschte Thema gefunden haben, wählen Sie **Lösungen überprüfen** aus.

    ![Nächster Schritt](./media/support/next-step.png)

Die folgenden Optionen werden angezeigt:

- Wenn Sie ein anderes Thema auswählen möchten, klicken Sie auf **Select a different issue** (Anderes Problem auswählen).
- Um das Problem zu beheben, lesen Sie die empfohlenen Schritte und Dokumente, falls verfügbar.

    ![Empfohlene Lösungen](./media/support/recommended-solutions.png)

Falls Sie in der Selbsthilfe keine Antwort finden, wählen Sie **Problemdetails angeben** aus. Füllen Sie alle erforderlichen Felder aus, um den Lösungsprozess zu beschleunigen, und wählen Sie dann **Übermitteln** aus.

>[!Note]
>Wenn Sie sich noch nicht beim Partner Center angemeldet haben, müssen Sie sich möglicherweise anmelden, bevor Sie ein Ticket erstellen können.

## <a name="track-your-existing-support-requests"></a>Verfolgen Sie Ihre vorhandene Supportanfragen

Wählen Sie im linken Navigationsmenü **Kommerzieller Marketplace** > **Support** aus, um Ihre geöffneten und geschlossenen Tickets zu überprüfen.

## <a name="record-issue-details-with-a-har-file"></a>Aufzeichnen von Problemdetails mit einer HAR-Datei

Um die Supportmitarbeiter bei der Behandlung Ihres Problems zu unterstützen, können Sie eine Datei im Format „HTTP-Archiv“ (HAR) an Ihr Supportticket anfügen. Bei HAR-Dateien handelt es sich um Protokolle der Netzwerkanforderungen eines Browsers.

> [!WARNING]
> Unter Umständen werden in HAR-Dateien vertrauliche Daten zu Ihrem Partner Center-Konto aufgezeichnet.

### <a name="microsoft-edge-and-google-chrome"></a>Microsoft Edge und Google Chrome

Generieren Sie eine HAR-Datei mit **Microsoft Edge** oder **Google Chrome** wie folgt:

1. Navigieren Sie zu der Webseite, auf der das Problem aufgetreten ist.
2. Wählen Sie oben rechts das Symbol mit den Auslassungszeichen und dann **Weitere Tools** > **Entwicklertools** aus. Sie können auch die F12-TASTE drücken.
3. Wählen Sie im Bereich „Entwicklertools“ die Registerkarte **Netzwerk** aus.
4. Wählen Sie **Aufzeichnung des Netzwerkprotokolls beenden** und **Löschen** aus, um vorhandene Protokolle zu entfernen. Das Datensatzsymbol wird nun grau angezeigt.

    ![Entfernen von vorhandenen Protokollen in Microsoft Edge oder Google Chrome](media/support/chromium-stop-clear-session.png)

5. Wählen Sie **Netzwerkprotokoll aufzeichnen** aus, um die Aufzeichnung zu starten. Nach dem Starten der Aufzeichnung wird das Aufzeichnungssymbol rot angezeigt.

    ![Starten der Aufzeichnung in Microsoft Edge oder Google Chrome](media/support/chromium-start-session.png)

6. Reproduzieren Sie das Problem, das Sie beheben möchten.
7. Wählen Sie nach dem Reproduzieren des Problems die Option **Aufzeichnung des Netzwerkprotokolls beenden** aus.
8. Wählen Sie die Option **HAR exportieren** (Symbol mit Abwärtspfeil) aus, und speichern Sie die Datei.

    ![Exportieren einer HAR-Datei in Microsoft Edge oder Google Chrome](media/support/chromium-network-export-har.png)

### <a name="mozilla-firefox"></a>Mozilla Firefox

Generieren Sie eine HAR-Datei mit **Mozilla Firefox** wie folgt:

1. Navigieren Sie zu der Webseite, auf der das Problem aufgetreten ist.
1. Wählen Sie oben rechts das Symbol mit den Auslassungszeichen und dann **Web-Entwickler** > **Werkzeuge ein-/ausblenden** aus. Sie können auch die F12-TASTE drücken.
1. Wählen Sie die Registerkarte **Netzwerkanalyse** und dann **Leeren** aus, um die vorhandenen Protokolle zu entfernen.

    ![Entfernen von vorhandenen Protokollen in Mozilla Firefox](media/support/firefox-clear-session.png)

1. Reproduzieren Sie das Problem, das Sie beheben möchten.
1. Wählen Sie nach dem Reproduzieren des Problems **HAR Export/Import** > **Save All As HAR** (HAR-Export/Import > Alles als HAR speichern) aus.

    ![Exportieren einer HAR-Datei in Mozilla Firefox](media/support/firefox-network-export-har.png)

### <a name="apple-safari"></a>Apple Safari

Generieren Sie eine HAR-Datei mit **Safari** wie folgt:

1. Aktivieren Sie die Entwicklertools in Safari: Wählen Sie **Safari** > **Einstellungen** aus. Navigieren Sie zur Registerkarte **Erweitert**, und wählen Sie dann **Entwicklungsmenü in Menüleiste anzeigen** aus.
1. Navigieren Sie zu der Webseite, auf der das Problem aufgetreten ist.
1. Wählen Sie **Entwicklung** aus, und wählen Sie dann **Webinformationen anzeigen** aus.
1. Wählen Sie die Registerkarte **Netzwerkanalyse** und dann **Netzwerkelemente löschen** aus, um die vorhandenen Protokolle zu entfernen.

    ![Entfernen von vorhandenen Protokollen in Safari](media/support/safari-clear-session.png)

1. Reproduzieren Sie das Problem, das Sie beheben möchten.
1. Wählen Sie nach dem Reproduzieren des Problems die Option **Exportieren** aus, und speichern Sie die Datei.

    ![Exportieren einer HAR-Datei in Safari](media/support/safari-network-export-har.png)

## <a name="next-steps"></a>Nächste Schritte

- [Aktualisieren eines bestehenden Commercial Marketplace-Angebots](update-existing-offer.md)