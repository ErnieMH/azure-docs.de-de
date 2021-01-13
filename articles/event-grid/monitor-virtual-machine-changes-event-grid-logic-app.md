---
title: 'Überwachen von Änderungen an virtuellen Computern: Azure Event Grid und Logic Apps'
description: Es wird beschrieben, wie Sie mit Azure Event Grid und Logic Apps eine Überprüfung auf Änderungen an virtuellen Computern (VMs) durchführen.
services: logic-apps, event-grid
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: estfan, LADocs
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 1a5d8c36382433024efd1f1cc6ba9fd878d28ddc
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2020
ms.locfileid: "92329524"
---
# <a name="tutorial-monitor-virtual-machine-changes-by-using-azure-event-grid-and-logic-apps"></a>Tutorial: Überwachen von Änderungen an virtuellen Computern mit Azure Event Grid und Logic Apps

Zum Überwachen und Reagieren auf bestimmte Ereignisse, die für Azure-Ressourcen oder Drittanbieterressourcen auftreten, können Sie Aufgaben automatisieren und als Workflow ausführen, indem Sie eine [Logik-App](../logic-apps/logic-apps-overview.md) mit minimalem Code erstellen. Diese Ressourcen können derartige Ereignisse in einem [Azure-Ereignisraster](../event-grid/overview.md) (Azure Event Grid) veröffentlichen. Das Ereignisraster überträgt diese Ereignisse wiederum mithilfe von Push an Abonnenten, die als Endpunkte Warteschlangen, Webhooks, oder [Event Hubs](../event-hubs/event-hubs-about.md) aufweisen. Als Abonnent kann Ihre Logik-App auf diese Ereignisse vom Ereignisraster warten, bevor automatisierte Workflows zur Durchführung von Aufgaben ausgeführt werden.

Im Folgenden werden einige Beispiele für Ereignisse aufgeführt, die Herausgeber mit dem Azure Event Grid-Dienst an Abonnenten senden können:

* Eine Ressource wird erstellt, gelesen, aktualisiert oder gelöscht. Beispielsweise können Sie Änderungen überwachen, bei denen möglicherweise Gebühren für Ihr Azure-Abonnement anfallen und die sich auf Ihre Abrechnung auswirken könnten.

* Eine Person wird zu einem Azure-Abonnement hinzugefügt oder aus dieser entfernt.

* Ihre App führt eine bestimmte Aktion durch.

* Eine neue Meldung wird in einer Warteschlange angezeigt.

In diesem Tutorial wird eine Logik-App erstellt, die Änderungen an einer VM überwacht und E-Mails über diese Änderungen sendet. Wenn Sie eine Logik-App mit einem Ereignisabonnement für eine Azure-Ressource erstellen, werden Ereignisse aus dieser Ressource über ein Ereignisraster an die Logik-App weitergeleitet. Das Tutorial führt Sie durch das Erstellen dieser Logik-App:

![Screenshot von Logic Apps Designer mit einem Workflow zum Überwachen einer VM mit Event Grid.](./media/monitor-virtual-machine-changes-event-grid-logic-app/monitor-virtual-machine-event-grid-logic-app-overview.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen Sie eine Logik-App zur Überwachung von Ereignissen über ein Ereignisraster.
> * Fügen Sie eine Bedingung zur Prüfung auf Änderungen an einem virtuellen Computer hinzu.
> * Senden Sie bei Änderungen an Ihrem virtuellen Computer E-Mails.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Ein E-Mail-Konto bei einem von Logic Apps unterstützten E-Mail-Anbieter wie Office 365 Outlook, Outlook.com oder Gmail zum Senden von Benachrichtigungen. Informationen zu Connectors für andere Anbieter finden Sie in [dieser Liste](/connectors/).

  Dieses Tutorial verwendet ein Office 365 Outlook-Konto. Bei Verwendung eines anderen E-Mail-Kontos bleiben die allgemeinen Schritte zwar gleich, die Benutzeroberfläche sieht aber unter Umständen etwas anders aus.

  > [!IMPORTANT]
  > Wenn Sie den Gmail-Connector verwenden möchten, können nur G-Suite-Geschäftskonten diesen Connector ohne Einschränkung in Logik-Apps verwenden. Wenn Sie über ein Gmail-Consumerkonto verfügen, können Sie diesen Connector nur mit bestimmten von Google genehmigten Diensten verwenden, oder Sie können [eine Google-Client-App erstellen, die für die Authentifizierung mit Ihrem Gmail-Connector verwendet werden soll](/connectors/gmail/#authentication-and-bring-your-own-application). Weitere Informationen finden Sie unter [Datensicherheit und Datenschutzrichtlinien für Google-Connectors in Azure Logic Apps](../connectors/connectors-google-data-security-privacy-policy.md).

* Ein [virtueller Computer](https://azure.microsoft.com/services/virtual-machines), der als einziger Computer in einer eigenen Azure-Ressourcengruppe enthalten ist. Erstellen Sie eine VM mithilfe des [Tutorials zum Erstellen eines virtuellen Computers](../virtual-machines/windows/quick-create-portal.md), wenn Sie dies nicht bereits getan haben. Zur Veröffentlichung von Ereignissen auf dem virtuellen Computer [müssen Sie keine weiteren Aktionen durchführen](../event-grid/overview.md).

## <a name="create-blank-logic-app"></a>Erstellen einer leeren Logik-App

1. Melden Sie sich mit den Anmeldeinformationen Ihres Azure-Kontos beim [Azure-Portal](https://portal.azure.com) an.

1. Klicken Sie im Hauptmenü von Azure auf **Ressource erstellen** > **Integration** > **Logik-App**.

   ![Screenshot des Azure-Portals mit der Schaltfläche zum Erstellen einer Logik-App-Ressource.](./media/monitor-virtual-machine-changes-event-grid-logic-app/azure-portal-create-logic-app.png)

1. Geben Sie unter **Logik-App** Informationen zu Ihrer Logik-App-Ressource ein. Wählen Sie **Erstellen**, wenn Sie fertig sind.

   ![Screenshot des Menüs zum Erstellen von Logik-Apps, das Details wie Name, Abonnement, Ressourcengruppe und Standort zeigt.](./media/monitor-virtual-machine-changes-event-grid-logic-app/create-logic-app-for-event-grid.png)

   | Eigenschaft | Erforderlich | Wert | BESCHREIBUNG |
   |----------|----------|-------|-------------|
   | **Name** | Ja | <*logic-app-name*> | Geben Sie einen eindeutigen Namen für Ihre Logik-App an. |
   | **Abonnement** | Ja | <*Name des Azure-Abonnements*> | Wählen Sie für alle Dienste in diesem Tutorial dasselbe Azure-Abonnement aus. |
   | **Ressourcengruppe** | Ja | <*Azure-resource-group*> | Der Name der Azure-Ressourcengruppe für Ihre Logik-App, den Sie für alle Dienste in diesem Tutorial auswählen können. |
   | **Location** | Ja | <*Azure-Region*> | Wählen Sie für alle Dienste in diesem Tutorial die gleiche Azure-Region. |
   |||

1. Nachdem Ihre Logik-App von Azure bereitgestellt wurde, zeigt der Designer für Logik-Apps eine Seite mit einem Einführungsvideo und häufig verwendeten Triggern an. Scrollen Sie nach unten, bis der Bereich unter dem Video und den Triggern angezeigt wird.

1. Wählen Sie unter **Vorlagen** die Option **Leere Logik-App**.

   ![Screenshot von Logic Apps-Vorlagen, der die Auswahl zum Erstellen einer leeren Logik-App zeigt.](./media/monitor-virtual-machine-changes-event-grid-logic-app/choose-logic-app-template.png)

   Im Designer für Logik-Apps werden jetzt die [*Trigger*](../logic-apps/logic-apps-overview.md#logic-app-concepts) angezeigt, die Sie verwenden können, um Ihre Logik-App zu starten. Jede Logik-App muss mit einem Trigger beginnen, der ausgelöst wird, wenn ein bestimmtes Ereignis eintritt oder eine bestimmte Bedingung erfüllt wird. Bei jeder Auslösung des Triggers erstellt Azure Logic Apps eine Workflowinstanz, die Ihre Logik-App ausführt.

## <a name="add-an-event-grid-trigger"></a>Hinzufügen eines Event Grid-Triggers

Fügen Sie nun den Event Grid-Trigger hinzu, den Sie zum Überwachen der Ressourcengruppe für Ihren virtuellen Computer verwenden.

1. Geben Sie im Suchfeld des Designers `event grid` als Filter ein. Wählen Sie in der Triggerliste den Trigger **Bei Eintritt eines Ressourcenereignisses** aus.

   ![Screenshot von Logic Apps Designer, der die Auswahl von Event Grid-Triggern für ein Ressourcenereignis zeigt.](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger.png)

1. Wenn Sie aufgefordert werden, melden Sie sich mit den Anmeldeinformationen Ihres Azure-Kontos bei Azure Event Grid an. Überprüfen Sie in der Liste **Mandant**, in der der zugeordnete Azure Active Directory-Mandant für Ihr Azure-Abonnement angezeigt wird, dass der richtige Mandant angegeben ist. Beispiel:

   ![Screenshot von Logic Apps Designer, der die Azure-Anmeldeaufforderung zum Herstellen einer Verbindung mit Event Grid zeigt.](./media/monitor-virtual-machine-changes-event-grid-logic-app/sign-in-event-grid.png)

   > [!NOTE]
   > Wenn Sie mit einem persönlichen Microsoft-Konto wie @outlook.com oder @hotmail.com angemeldet sind, wird der Event Grid-Trigger möglicherweise nicht ordnungsgemäß angezeigt. Um dieses Problem zu umgehen, wählen Sie [Verbindung über Dienstprinzipal herstellen](../active-directory/develop/howto-create-service-principal-portal.md) aus oder authentifizieren sich als Mitglied der Azure Active Directory-Instanz, die Ihrem Azure-Abonnement zugeordnet ist, z. B. *Benutzername*@emailoutlook.onmicrosoft.com.

1. Abonnieren Sie für Ihre Logik-App jetzt die Ereignisse des Herausgebers. Geben Sie die Details zu Ihrem Ereignisabonnement wie in der folgenden Tabelle beschrieben an, z. B.:

   ![Screenshot von Logic Apps Designer, der den Detail-Editor für den Trigger beim Eintreten eines Ressourcenereignisses zeigt.](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details.png)

   | Eigenschaft | Erforderlich | Wert | BESCHREIBUNG |
   | -------- | -------- | ----- | ----------- |
   | **Abonnement** | Ja | <*event-publisher-Azure-subscription-name*> | Wählen Sie den Namen für das Azure-Abonnement aus, das dem *Ereignisherausgeber* zugeordnet ist. Wählen Sie für dieses Tutorial den Namen des Azure-Abonnements für Ihren virtuellen Computer aus. |
   | **Ressourcentyp** | Ja | <*event-publisher-Azure-resource-type*> | Wählen Sie den Azure-Ressourcentyp für den Ereignisherausgeber aus. Weitere Informationen zu Azure-Ressourcentypen finden Sie unter [Azure-Ressourcenanbieter und -typen](../azure-resource-manager/management/resource-providers-and-types.md). Wählen Sie für dieses Tutorial den Wert `Microsoft.Resources.ResourceGroups` aus, um Azure-Ressourcengruppen zu überwachen. |
   | **Ressourcenname** |  Ja | <*event-publisher-Azure-resource-name*> | Wählen Sie den Azure-Ressourcennamen für den Ereignisherausgeber aus. Diese Liste variiert, basierend auf dem von Ihnen ausgewählten Ressourcentyp. Wählen Sie für dieses Tutorial den Namen der Azure-Ressourcengruppe aus, die Ihren virtuellen Computer enthält. |
   | **Ereignistypelement** |  Nein | <*event-types*> | Wählen Sie mindestens einen spezifischen Ereignistyp aus, um zu filtern und an Ihr Event Grid zu senden. Beispielsweise können Sie optional diese Ereignistypen hinzufügen, um zu erkennen, wenn Ressourcen geändert oder gelöscht werden: <p><p>- `Microsoft.Resources.ResourceActionSuccess` <br>- `Microsoft.Resources.ResourceDeleteSuccess` <br>- `Microsoft.Resources.ResourceWriteSuccess` <p>Weitere Informationen finden Sie in den folgenden Themen: <p><p>- [Azure Event Grid-Ereignisschema für Ressourcengruppen](../event-grid/event-schema-resource-groups.md) <br>- [Grundlegendes zur Ereignisfilterung](../event-grid/event-filtering.md) <br>- [Filtern von Ereignissen für das Event Grid](../event-grid/how-to-filter-events.md) |
   | Wählen Sie zum Hinzufügen von optionalen Eigenschaften die Option **Neuen Parameter hinzufügen** und dann die gewünschten Eigenschaften aus. | Nein | {siehe Beschreibungen} | * **Präfixfilter:** Lassen Sie diese Eigenschaft für dieses Tutorial leer. Das Standardverhalten entspricht allen Werten. Allerdings können Sie als Filter eine Präfixzeichenfolge wie einen Pfad und einen Parameter für eine bestimmte Ressource angeben. <p>* **Suffixfilter:** Lassen Sie diese Eigenschaft für dieses Tutorial leer. Das Standardverhalten entspricht allen Werten. Wenn nur bestimmte Dateitypen angezeigt werden sollen, können Sie als Filter jedoch eine Suffixzeichenfolge wie eine Dateinamenerweiterung angeben. <p>* **Abonnementname:** In diesem Tutorial können Sie einen eindeutigen Namen für Ihr Ereignisabonnement angeben. |
   |||

1. Speichern Sie Ihre Logik-App. Wählen Sie auf der Symbolleiste des Designers **Speichern** aus. Um die Details einer Aktion in Ihrer Logik-App zu reduzieren und auszublenden, wählen Sie die Titelleiste der Aktion aus.

   ![Screenshot von Logic Apps Designer, der die Schaltfläche „Speichern“ zum Speichern von Workflowänderungen zeigt.](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save.png)

   Wenn Sie Ihre Logik-App mit einen Ereignisrastertrigger speichern, erstellt Azure automatisch ein Ereignisabonnement für Ihre Logik-App in Ihrer ausgewählten Ressource. Veröffentlicht die Ressource nun ein Ereignis im Ereignisraster, überträgt dieses Ereignisraster das Ereignis mithilfe von Push automatisch an Ihre Logik-App. Dieses Ereignis löst Ihre Logik-App aus, erstellt dann eine Instanz des in den folgenden Schritten definierten Workflows und startet deren Ausführung.

Ihre Logik-App ist nun live geschaltet und lauscht auf Ereignisse im Ereignisraster, führt allerdings erst Aktionen durch, wenn Sie diese dem Workflow hinzufügen.

## <a name="add-a-condition"></a>Bedingung hinzufügen

Falls Sie möchten, dass Ihre Logik-App nur ausgeführt wird, wenn ein bestimmtes Ereignis oder ein Vorgang eintritt, sollten Sie eine Bedingung zur Überprüfung auf den Vorgang `Microsoft.Compute/virtualMachines/write` hinzufügen. Wenn diese Bedingung erfüllt ist, sendet Ihre Logik-App Ihnen per E-Mail Details zum aktualisierten virtuellen Computer.

1. Wählen Sie im Designer für Logik-Apps unter dem Event Grid-Trigger die Option **Neuer Schritt** aus.

   ![Screenshot von Logic Apps Designer, der die Schaltfläche zum Hinzufügen neuer Schritte zum Workflow zeigt.](./media/monitor-virtual-machine-changes-event-grid-logic-app/choose-new-step-condition.png)

1. Geben Sie unter **Aktion auswählen** im Suchfeld `condition` als Filter ein. Wählen Sie in der Liste „Aktionen“ die Aktion **Bedingung** aus.

   ![Screenshot von Logic Apps Designer, der die Schaltfläche zum Hinzufügen einer neuen Bedingungsaktion zeigt.](./media/monitor-virtual-machine-changes-event-grid-logic-app/select-condition.png)

   Der Designer für Logik-Apps fügt Ihrem Workflow eine leere Bedingung einschließlich der abhängig davon, ob die Bedingung „true“ oder „false“ ist, zu befolgenden Aktionspfade hinzu.

   ![Screenshot von Logic Apps Designer, der eine leere Bedingung anzeigt, die dem Workflow hinzugefügt wurde.](./media/monitor-virtual-machine-changes-event-grid-logic-app/empty-condition.png)

1. Benennen Sie den Titel der Bedingung in `If a virtual machine in your resource group has changed` um. Wählen Sie in der Titelleiste der Bedingung die Schaltfläche mit den Auslassungspunkten ( **...** ) und anschließend **Umbenennen** aus.

   ![Screenshot von Logic Apps Designer, der das Kontextmenü des Bedingungs-Editors anzeigt, bei dem die Option „Umbenennen“ ausgewählt ist.](./media/monitor-virtual-machine-changes-event-grid-logic-app/rename-condition.png)

1. Erstellen Sie eine Bedingung, die das `body`-Ereignis auf ein `data`-Objekt überprüft, bei dem die Eigenschaft `operationName` dem Vorgang `Microsoft.Compute/virtualMachines/write` entspricht. Weitere Informationen finden Sie unter [Event Grid-Ereignisschema](../event-grid/event-schema.md).

   1. Klicken Sie in der ersten Zeile unter **Und** innerhalb des linken Felds. Wählen Sie in der angezeigten Liste mit dynamischem Inhalt die Option **Ausdruck** aus.

      ![Screenshot von Logic Apps Designer, der die Bedingung mit ausgewähltem Ausdrucks-Editor zeigt.](./media/monitor-virtual-machine-changes-event-grid-logic-app/condition-choose-expression.png)

   1. Geben Sie im Ausdrucks-Editor den folgenden Ausdruck ein, mit dem der Vorgangsname für den Trigger zurückgegeben wird, und wählen Sie **OK** aus:

      `triggerBody()?['data']['operationName']`

      Beispiel:

      ![Screenshot von Logic Apps Designer, der den Bedingungs-Editor mit Ausdruck anzeigt, um den Vorgangsnamen zu extrahieren.](./media/monitor-virtual-machine-changes-event-grid-logic-app/condition-add-data-operation-name.png)

   1. Behalten Sie im mittleren Feld den Operator **gleich** bei.

   1. Geben Sie im rechten Feld diesen Wert ein. Dies ist der jeweilige Vorgang, den Sie überwachen möchten:

      `Microsoft.Compute/virtualMachines/write`

   Ihre fertig gestellte Bedingung sieht nun wie im folgenden Beispiel aus:

   ![Screenshot von Logic Apps Designer, der eine Bedingung anzeigt, die den Vorgang vergleicht.](./media/monitor-virtual-machine-changes-event-grid-logic-app/complete-condition.png)

   Wenn Sie von der Entwurfsansicht zur Programmieransicht und wieder zurück in die Entwurfsansicht wechseln, wird der Ausdruck, den Sie in der Bedingung angegeben haben, in das Token **data.operationName** aufgelöst:

   ![Screenshot von Logic Apps Designer, der eine Bedingung mit aufgelösten Token anzeigt.](./media/monitor-virtual-machine-changes-event-grid-logic-app/resolved-condition.png)

1. Speichern Sie Ihre Logik-App.

## <a name="send-email-notifications"></a>Senden von E-Mail-Benachrichtigungen

Fügen Sie nun eine [*Aktion*](../logic-apps/logic-apps-overview.md#logic-app-concepts) hinzu, damit Sie eine E-Mail erhalten, wenn die angegebene Bedingung erfüllt ist.

1. Wählen Sie im Feld **Bei TRUE** der Bedingung die Option **Aktion hinzufügen** aus.

   ![Screenshot des Bedingungs-Editors von Logic Apps Designer, der die Schaltfläche zum Hinzufügen einer Aktion anzeigt, wenn die Bedingung zutrifft.](./media/monitor-virtual-machine-changes-event-grid-logic-app/condition-true-add-action.png)

1. Geben Sie unter **Aktion auswählen** im Suchfeld `send an email` als Filter ein. Suchen Sie abhängig von Ihrem E-Mail-Anbieter nach dem entsprechenden Connector, und wählen Sie diesen aus. Wählen Sie anschließend die Aktion „E-Mail senden“ für Ihren Connector aus. Beispiel:

   * Wählen Sie für ein Geschäfts-, Schul- oder Unikonto in Azure den Office 365 Outlook-Connector aus.

   * Wählen Sie für persönliche Microsoft-Konten den Outlook.com-Connector aus.

   * Wählen Sie für Gmail-Konten den Gmail-Connector aus.

   Dieses Tutorial fährt mit dem Office 365 Outlook-Connector fort. Wenn Sie einen anderen Anbieter verwenden, werden dieselben Schritte durchgeführt, allerdings kann das Erscheinungsbild der Benutzeroberfläche eventuell etwas abweichen.

   ![Screenshot von Logic Apps Designer, der die Suche für die Aktion „E-Mail senden“ im Office 365 Outlook-Connector anzeigt.](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email.png)

1. Wenn noch keine Verbindung für Ihren E-Mail-Anbieter besteht, melden Sie sich nach Aufforderung zur Authentifizierung bei Ihrem E-Mail-Konto an.

1. Benennen Sie die Aktion „E-Mail senden“ in diesen Titel um: `Send email when virtual machine updated`.

1. Geben Sie die Informationen zur E-Mail gemäß der folgenden Tabelle an:

   ![Screenshot von Logic Apps Designer, der den dynamischen Inhalt anzeigt, der bei einer zutreffenden Bedingung zur Betreffzeile einer E-Mail hinzugefügt wird.](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-empty-email-action.png)

   > [!TIP]
   > Klicken Sie zum Auswählen der Ausgabe aus vorherigen Schritten in Ihrem Workflow in ein Bearbeitungsfeld, um die Liste mit dynamischen Inhalten anzuzeigen, oder wählen Sie die Option **Dynamischen Inhalt hinzufügen** aus. Wählen Sie für jeden Abschnitt in der Liste **Mehr anzeigen** aus, um weitere Ergebnisse anzuzeigen. Um die Liste mit dynamischen Inhalten zu schließen, wählen Sie erneut **Dynamischen Inhalt hinzufügen** aus.

   | Eigenschaft | Erforderlich | Wert | BESCHREIBUNG |
   | -------- | -------- | ----- | ----------- |
   | **An** | Ja | <*Empfänger\@Domäne*> | Geben Sie die E-Mail-Adresse des Empfängers ein. Zu Testzwecken können Sie hier Ihre eigene E-Mail-Adresse angeben. |
   | **Subject** | Ja | `Resource updated:` **Betreff** | Geben Sie den Inhalt des Betreffs der E-Mail ein. Geben Sie für dieses Tutorial den angegebenen Text ein, und wählen Sie das Feld **Betreff** des Ereignisses aus. Hier enthält Ihr E-Mail-Betreff den Namen für die aktualisierte Ressource (virtueller Computer). |
   | **Text** | Ja | `Resource:` **Thema** <p>`Event type:` **Ereignistyp**<p>`Event ID:` **ID**<p>`Time:` **Ereigniszeit** | Geben Sie den Inhalt des Texts der E-Mail ein. Geben Sie für dieses Tutorial den angegebenen Text ein, und wählen Sie die Felder **Thema**, **Ereignistyp**, **ID** und **Ereigniszeit** des Ereignisses, damit Ihre E-Mail die Ressource enthält, die das Ereignis ausgelöst hat, den Ereignistyp, den Ereigniszeitstempel sowie die Ereignis-ID für das Update. Für dieses Tutorial ist die Ressource die im Trigger ausgewählte Azure-Ressourcengruppe. <p>Um Ihrem Inhalt leere Zeilen hinzuzufügen, drücken Sie UMSCHALT + EINGABETASTE. |
   ||||

   > [!NOTE]
   > Wenn Sie ein Feld auswählen, das ein Array darstellt, fügt der Designer automatisch eine **ForEach**-Schleife für die Aktion hinzu, die auf das Array verweist. Auf diese Weise führt Ihre Logik-App diese Aktion für jedes Arrayelement durch.

   Ihre E-Mail-Aktion könnte nun folgendem Beispiel entsprechen:

   ![Screenshot von Logic Apps Designer, der ausgewählte Ausgaben anzeigt, die beim Aktualisieren einer VM in einer E-Mail gesendet werden.](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email-details.png)

   Anschließend sieht Ihre Logik-App in etwa wie im folgenden Beispiel aus:

   ![Screenshot von Logic Apps Designer, der die erstellte Logik-App mit Details zu Triggern und Aktionen anzeigt.](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-completed.png)

1. Speichern Sie Ihre Logik-App. Um die Details der einzelnen Aktionen in Ihrer Logik-App zu reduzieren und auszublenden, wählen Sie die Titelleiste der Aktion aus.

   Die Logik-App ist nun live geschaltet, wartet jedoch vor der Durchführung von Aktionen auf Änderungen an Ihrem virtuellen Computer. Um nun Ihre Logik-App zu testen, fahren Sie mit dem nächsten Abschnitt fort.

## <a name="test-your-logic-app-workflow"></a>Testen Ihres Logik-App-Workflows

1. Um zu überprüfen, ob Ihre Logik-App die angegebenen Ereignisse abruft, aktualisieren Sie Ihren virtuellen Computer.

   Beispielsweise können Sie die Größe Ihrer VM im Azure-Portal oder [mit Azure PowerShell ändern](../virtual-machines/windows/resize-vm.md).

   Nach nur wenigen Minuten sollten Sie eine E-Mail erhalten. Beispiel:

   ![Screenshot einer Beispiel-E-Mail von Outlook, die Details zur VM-Aktualisierung zeigt.](./media/monitor-virtual-machine-changes-event-grid-logic-app/email.png)

1. Um den Verlauf der Ausführungen und Trigger für Ihre Logik-App zu überprüfen, wählen Sie im Menü Ihrer Logik-App **Übersicht** aus. Um weitere Details zu einer Ausführung anzuzeigen, wählen Sie die Zeile dieser Ausführung aus.

   ![Screenshot der Übersichtsseite der Logik-App, die eine ausgewählte erfolgreiche Ausführung zeigt.](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history.png)

1. Um die Eingaben und Ausgaben für die einzelnen Schritte anzuzeigen, erweitern Sie den Schritt, den Sie überprüfen möchten. Diese Informationen helfen Ihnen bei der Diagnose und beim Debuggen von Problemen in Ihrer Logik-App.

   ![Screenshot des Verlaufs der Logik-App-Ausführung, der Details für jede Ausführung zeigt.](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history-details.png)

Herzlichen Glückwunsch! Sie haben eine Logik-App erstellt und ausgeführt, die Ressourcenereignisse durch einen Ereignisraster überwacht und Ihnen eine E-Mail sendet, wenn derartige Ereignisse auftreten sollten. Außerdem haben Sie erfahren, wie einfach Sie Workflows zur Automatisierung von Prozessen und Integration von Systemen sowie Clouddiensten erstellen können.

Sie können andere Konfigurationsänderungen mit Ereignisrastern und Logik-Apps überwachen wie etwas Folgende:

* Ein virtueller Computer erhält rollenbasierte Zugriffssteuerungsrechte in Azure (Role-Based Access Control, Azure RBAC).
* Es werden Änderungen an einer Netzwerksicherheitsgruppe (Network Security Group, NSG) in einer Netzwerkschnittstelle (Network Interface, NIC) vorgenommen.
* Datenträger für einen virtuellen Computer werden hinzugefügt oder entfernt.
* Der NIC eines virtuellen Computers wird eine öffentliche IP-Adresse zugewiesen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In diesem Tutorial werden Ressourcen verwendet und Aktionen durchgeführt, für die Gebühren in Ihrem Azure-Abonnement anfallen. Wenn Sie mit der Durchführung des Tutorials und dem Testen fertig sind, deaktivieren oder löschen Sie unbedingt alle Ressourcen, für deren Gebühren Sie nicht aufkommen möchten.

* Deaktivieren Sie Ihre Logik-App, um die Ausführung zu beenden, ohne Ihre Arbeit zu löschen. Wählen Sie in Ihrem Logik-App-Menü **Übersicht** aus. Wählen Sie in der Symbolleiste die Option **Deaktivieren** aus.

  ![Screenshot der Übersicht der Logik-App, der die Schaltfläche „Deaktivieren“ zeigt, die zum Deaktivieren der Logik-App ausgewählt wurde.](./media/monitor-virtual-machine-changes-event-grid-logic-app/turn-off-disable-logic-app.png)

  > [!TIP]
  > Falls das Logik-App-Menü nicht angezeigt wird, können Sie zum Azure-Dashboard zurückkehren und Ihre Logik-App erneut öffnen.

* Wählen Sie im Menü der Logik-App **Übersicht** aus, um Ihre Logik-App endgültig zu löschen. Wählen Sie in der Symbolleiste die Option **Löschen** aus. Bestätigen Sie, dass Sie Ihre Logik-App löschen möchten, und wählen Sie **Löschen** aus.

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Event Grid](../event-grid/custom-event-quickstart.md)
