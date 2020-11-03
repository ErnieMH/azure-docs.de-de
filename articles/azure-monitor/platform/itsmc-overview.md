---
title: IT Service Management Connector in Azure Log Analytics | Microsoft-Dokumentation
description: Dieser Artikel bietet eine Übersicht über den ITSM-Connector (IT Service Management-Connector) sowie Informationen zur Verwendung dieser Lösung, um die ITSM-Arbeitselemente in Azure Log Analytics zu überwachen und zu verwalten und um etwaige Probleme schnell zu lösen.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 05/24/2018
ms.openlocfilehash: 5c18a904f0ec0f100312ee3fafb53038bd2ccf19
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92745681"
---
# <a name="connect-azure-to-itsm-tools-using-it-service-management-connector"></a>Verbinden von Azure mit ITSM-Tools mithilfe des ITSM-Connectors

![Symbol für den IT Service Management Connector](media/itsmc-overview/itsmc-symbol.png)

Der ITSM-Connector ermöglicht Ihnen, Azure und ein unterstütztes ITSM-Produkt bzw. einen unterstützten ITSM-Dienst zu verbinden.

Azure-Dienste wie Log Analytics und Azure Monitor stellen Tools zur Verfügung, mit denen Sie Probleme mit Ihren Azure- und Nicht-Azure-Ressourcen erkennen, analysieren und beheben können. Die Arbeitselemente, die sich auf ein Problem beziehen, befinden sich jedoch typischerweise in einem ITSM-Produkt bzw. -Service. Der ITSM-Connector bietet eine bidirektionale Verbindung zwischen Azure und ITSM-Tools, sodass Sie Probleme schneller lösen können.

Der ITSM-Connector unterstützt Verbindungen mit den folgenden ITSM-Tools:

-   ServiceNow
-   System Center Service Manager
-   Provance
-   Cherwell

Mit dem ITSMC können Sie folgende Aktionen ausführen:

-  Sie können Arbeitselemente im ITSM-Tool basierend auf Ihren Azure-Warnungen (Metrikwarnungen, Aktivitätsprotokollwarnungen und Log Analytics-Warnungen) erstellen.
-  Optional können Sie Incident- und Änderungsanforderungsdaten aus Ihrem ITSM-Tool mit einem Azure Log Analytics-Arbeitsbereich synchronisieren.

Erfahren Sie mehr über die [rechtlichen Bedingungen und Datenschutzrichtlinien](https://go.microsoft.com/fwLink/?LinkID=522330&clcid=0x9).

Führen Sie die folgenden Schritte aus, um den ITSM-Connector zu verwenden:

1.  [Hinzufügen der ITSM-Connector-Lösung](#adding-the-it-service-management-connector-solution)
2.  Erstellen einer ITSM-Verbindung
3.  [Verwenden der Verbindung](#using-the-solution)


##  <a name="adding-the-it-service-management-connector-solution"></a>Hinzufügen der IT Service Management Connector-Lösung

Bevor Sie eine Verbindung herstellen können, müssen Sie die ITSM-Connector-Lösung hinzufügen.

1. Klicken Sie im Azure-Portal auf das Symbol **+ Neu**.

   ![Neue Ressource in Azure](media/itsmc-overview/azure-add-new-resource.png)

2. Suchen Sie im Marketplace nach **ITSM-Connector** und klicken Sie auf **Erstellen**.

   ![Hinzufügen der ITSM-Connector-Lösung](media/itsmc-overview/add-itsmc-solution.png)

3. Wählen Sie im Abschnitt **OMS-Arbeitsbereich** den Azure Log Analytics-Arbeitsbereich aus, in dem Sie die Lösung installieren möchten.
   >[!NOTE]
   > * Im Rahmen der laufenden Umstellung von der Microsoft Operations Management Suite (OMS) auf Azure Monitor werden OMS-Arbeitsbereiche nun als Log Analytics-Arbeitsbereiche bezeichnet.
   > * Der ITSM-Connector kann nur in Log Analytics-Arbeitsbereichen in den folgenden Regionen installiert werden: „USA, Osten“, „USA, Westen 2“, „USA, Süden-Mitte“, „USA, Westen-Mitte“, „Fairfax“, „Kanada, Mitte“, „Europa, Westen“, „Vereinigtes Königreich, Süden“, „Asien, Südosten“, „Japan, Osten“, „Indien, Mitte“, „Australien, Südosten“.

4. Wählen Sie im Abschnitt **OMS-Arbeitsbereichseinstellungen** die Ressourcengruppe aus, in der Sie die Ressource für die Lösung erstellen möchten.

   ![Arbeitsbereich des ITSM-Connectors](media/itsmc-overview/itsmc-solution-workspace.png)
   >[!NOTE]
   >Im Rahmen der laufenden Umstellung von der Microsoft Operations Management Suite (OMS) auf Azure Monitor werden OMS-Arbeitsbereiche nun als Log Analytics-Arbeitsbereiche bezeichnet.

5. Klicken Sie auf **OK**.

Sobald die Ressource für die Lösung bereitgestellt ist, wird oben rechts im Fenster eine Benachrichtigung angezeigt.


## <a name="creating-an-itsm--connection"></a>Erstellen einer ITSM-Verbindung

Nachdem Sie die Lösung installiert haben, können Sie eine Verbindung erstellen.

Um eine Verbindung herzustellen, müssen Sie Ihr ITSM-Tool vorbereiten, damit die Verbindung von der ITSM-Connector-Lösung aus möglich ist.  

Abhängig von dem ITSM-Produkt, mit dem die Verbindung hergestellt werden soll, führen Sie die folgenden Schritte aus:

- [System Center Service Manager (SCSM)](./itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-azure)
- [ServiceNow](./itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-azure)
- [Provance](./itsmc-connections.md#connect-provance-to-it-service-management-connector-in-azure)  
- [Cherwell](./itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-azure)

Nachdem Sie Ihre ITSM-Tools vorbereitet haben, führen Sie die folgenden Schritte zum Erstellen einer Verbindung aus:

1. Wechseln Sie zu **Alle Ressourcen** , und suchen Sie nach **ServiceDesk(YourWorkspaceName)** .
2. Klicken Sie im linken Bereich unter **ARBEITSBEREICHSDATENQUELLEN** auf **ITSM-Verbindungen**.
   ![ITSM-Verbindungen](media/itsmc-overview/itsm-connections.png)

   Diese Seite zeigt die Liste der Verbindungen.
3. Klicken Sie auf **Verbindung hinzufügen**.

   ![Hinzufügen einer ITSM-Verbindung](media/itsmc-overview/add-new-itsm-connection.png)

4. Legen Sie die Verbindungseinstellungen fest, wie in [Verbinden von ITSM-Produkten/-Diensten mit dem IT Service Management Connector (Vorschau)](./itsmc-connections.md) beschrieben.

   > [!NOTE]
   >
   > Standardmäßig aktualisiert der ITSM-Connector die Konfigurationsdaten der Verbindung einmal alle 24 Stunden. Um die Daten Ihrer Verbindung bei Änderungen oder Vorlagenupdates, die Sie vornehmen, sofort zu aktualisieren, klicken Sie auf die Schaltfläche **Synchronisieren** auf dem Blatt Ihrer Verbindung.

   ![Aktualisieren der Verbindung](media/itsmc-overview/itsmc-connections-refresh.png)


## <a name="using-the-solution"></a>Verwenden der Lösung
   Mit der ITSM-Connector-Lösung können Sie Arbeitselemente aus Azure-Warnungen, Log Analytics-Warnungen und Log Analytics-Protokolldatensätzen erstellen.

## <a name="template-definitions"></a>Vorlagendefinitionen
   Hierbei handelt es sich um Arten von Arbeitselementen, die vom ITSM-Tool definierte Vorlagen verwenden können.
Mithilfe von Vorlagen können Kunden Felder definieren, die anhand von festgelegten Werten automatisch aufgefüllt werden. Diese Werte sind als Teil der Aktionsgruppe definiert. Die Definition der Vorlagen erfolgt im ITSM-Tool.
      
## <a name="create-itsm-work-items-from-azure-alerts"></a>Erstellen von ITSM-Arbeitselementen aus Azure-Warnungen

Nach dem Einrichten der ITSM-Verbindung können Sie in Ihrem ITSM-Tool Arbeitselemente basierend auf Azure-Warnungen erstellen, indem Sie die **ITSM-Aktion** in **Aktionsgruppen** verwenden.

Die wiederverwendbaren Aktionsgruppen bieten Ihnen die Möglichkeit, modular Aktionen für Ihre Azure-Warnungen auszulösen. Sie können Aktionsgruppen mit metrischen Warnungen, Aktivitätsprotokollwarnungen und Azure Log Analytics-Warnungen im Azure-Portal verwenden.

> [!NOTE]
> Nachdem die ITSM-Verbindung erstellt wurde, müssen Sie 30 Minuten warten, bis der Connector, der für den Synchronisierungsprozess erstellt wurde, abgeschlossen ist.
> 

Gehen Sie dazu wie folgt vor:

1. Klicken Sie im Azure-Portal auf **Warnungen**.
2. Klicken Sie im oberen Bereich auf **Aktionen verwalten**. Das Fenster **Aktionsgruppe hinzufügen** wird angezeigt.

    [![Aktionsgruppen](media/itsmc-overview/action-groups-selection.png)](media/itsmc-overview/action-groups-selection-big.png)

3. Wählen Sie das **Abonnement** und die **Ressourcengruppe** aus, in dem bzw. der Sie Ihre Aktionsgruppe erstellen möchten. Geben Sie den **Aktionsgruppennamen** und den **Anzeigenamen** für Ihre Aktionsgruppe an. Klicken Sie auf **Weiter: Benachrichtigungen**.

    ![Aktionsgruppendetail](media/itsmc-overview/action-groups-details.png)

4. Klicken Sie in der Benachrichtigungsliste auf **Weiter: Aktionen**.
5. Wählen Sie in der Liste „Aktionen“ in der Dropdownliste für **Aktionstyp** die Option **ITSM** aus. Geben Sie einen **Namen** für die Aktion an, und klicken Sie auf das Stiftsymbol für **Details bearbeiten**.
6. Wählen Sie das **Abonnement** , in dem sich Ihr Log Analytics-Arbeitsbereich befindet. Wählen Sie den Namen der **Verbindung** (Ihr ITSM-Connectorname) gefolgt vom Namen Ihres Arbeitsbereichs aus. Beispiel: „MyITSMMConnector(MyWorkspace)“.

    ![ITSM-Aktionsdetails](media/itsmc-overview/itsm-action-configuration.png)

7. Wählen Sie im Dropdownmenü den Typ **Arbeitselement** aus.

8. Wenn Sie die Felder mit festgelegten Werten auffüllen möchten, aktivieren Sie das Kontrollkästchen „Benutzerdefinierte Vorlage verwenden“. Andernfalls wählen Sie eine vorhandene [Vorlage](https://docs.microsoft.com/azure/azure-monitor/platform/itsmc-overview#template-definitions) aus der Dropdownliste aus, und füllen Sie die Vorlagenfelder mit festgelegten Werten auf.

9. Durch Aktivieren des Kontrollkästchens **Einzelne Arbeitselemente für jedes Konfigurationselement erstellen** erhält jedes Konfigurationselement ein eigenes Arbeitselement. Das bedeutet, dass für jedes Konfigurationselement nur ein Arbeitselement existiert und gemäß den erstellten Warnungen aktualisiert wird.
Wenn Sie das Kontrollkästchen **Einzelne Arbeitselemente für jedes Konfigurationselement erstellen** deaktivieren, erstellt jede Warnung ein neues Arbeitselement. Das bedeutet, dass pro Konfigurationselement mehr als eine Warnung vorliegen kann.

10. Klicken Sie auf **OK**.

Verwenden Sie beim Erstellen/Bearbeiten einer Azure-Warnungsregel eine Aktionsgruppe mit einer ITSM-Aktion. Wenn die Warnung ausgelöst wird, wird das Arbeitselement im ITSM-Tool erstellt bzw. aktualisiert.

> [!NOTE]
>
> Informationen zu den Preisen für ITSM-Aktionen finden Sie auf der [Seite mit der Preisübersicht](https://azure.microsoft.com/pricing/details/monitor/) für Aktionsgruppen.

> [!NOTE]
>
> Das Feld für Kurzbeschreibungen in der Warnungsregeldefinition ist auf 40 Zeichen begrenzt, wenn es über ein ITSM-Aktion gesendet wird.


## <a name="visualize-and-analyze-the-incident-and-change-request-data"></a>Visualisieren und Analysieren der Incident- und Änderungsanforderungsdaten

Basierend auf Ihrer Konfiguration, die Sie beim Einrichten einer Verbindung vorgenommenen haben, kann der ITSM-Connector bis zu 120 Tage von Incident- und Änderungsanforderungsdaten synchronisieren. Das Schema für den Protokolldatensatz dieser Daten finden Sie im [nächsten Abschnitt](#additional-information).

Die Incident- und Änderungsanforderungsdaten können über das Dashboard des ITSM-Connectors in der Lösung visuell dargestellt werden.

![Screenshot des Dashboards des ITSM-Connectors.](media/itsmc-overview/itsmc-overview-sample-log-analytics.png)

Das Dashboard bietet darüber hinaus Informationen zum Connectorstatus, die als Ausgangspunkt bei der Analyse von Verbindungsproblemen verwendet werden können.

Zudem lassen sich die mit den betroffenen Computern synchronisierten Incidents innerhalb der Dienstzuordnungslösung visuell darstellen.

Service Map ermittelt automatisch die Anwendungskomponenten auf Windows- und Linux-Systemen und stellt die Kommunikation zwischen Diensten dar. In dieser Lösung können Sie die Server ihrer Funktion gemäß anzeigen – als verbundene Systeme, die wichtige Dienste bereitstellen. Service Map zeigt Verbindungen zwischen Servern, Prozessen und Ports über die gesamte TCP-Verbindungsarchitektur an. Außer der Installation eines Agents ist keine weitere Konfiguration erforderlich. [Weitere Informationen](../insights/service-map.md)

Wenn Sie die Dienstzuordnungslösung verwenden, können Sie die in den ITSM-Lösungen erstellten Service Desk-Elemente anzeigen, wie im folgenden Beispiel gezeigt:

![Log Analytics-Bildschirm](media/itsmc-overview/itsmc-overview-integrated-solutions.png)

Weitere Informationen: [Dienstzuordnung](../insights/service-map.md)


## <a name="additional-information"></a>Zusätzliche Informationen

### <a name="data-synced-from-itsm-product"></a>Synchronisierte Daten aus dem ITSM-Produkt
Incidents und Änderungsanforderungen aus dem ITSM-Produkt werden mit Ihrem Log Analytics-Arbeitsbereich basierend auf der Verbindungskonfiguration synchronisiert.

Die folgenden Informationen sind Beispiele für Daten, die vom ITSMC gesammelt werden:

> [!NOTE]
>
> Abhängig vom in Log Analytics importierten Arbeitselementtyp enthält **ServiceDesk_CL** die folgenden Felder:

**Arbeitselement** : **Incidents**  
ServiceDeskWorkItemType_s="Incident"

**Fields**

- ServiceDeskConnectionName
- Service Desk-ID
- State
- Dringlichkeit
- Auswirkung
- Priority
- Eskalation
- Erstellt von
- Gelöst von
- Geschlossen von
- `Source`
- Zugewiesen zu
- Category
- Titel
- BESCHREIBUNG
- Erstellt am
- Geschlossen am
- Gelöst am
- Datum der letzten Änderung
- Computer


**Arbeitselement** : **Änderungsanforderungen**

ServiceDeskWorkItemType_s="ChangeRequest"

**Fields**
- ServiceDeskConnectionName
- Service Desk-ID
- Erstellt von
- Geschlossen von
- `Source`
- Zugewiesen zu
- Titel
- type
- Category
- State
- Eskalation
- Konfliktstatus
- Dringlichkeit
- Priority
- Risiko
- Auswirkung
- Zugewiesen zu
- Erstellt am
- Geschlossen am
- Datum der letzten Änderung
- Anforderungsdatum
- Geplantes Startdatum
- Geplantes Enddatum
- Startdatum der Arbeit
- Enddatum der Arbeit
- BESCHREIBUNG
- Computer

## <a name="output-data-for-a-servicenow-incident"></a>Ausgabedaten für einen ServiceNow-Incident

| Log Analytics-Feld | ServiceNow-Feld |
|:--- |:--- |
| ServiceDeskId_s| Number |
| IncidentState_s | State |
| Urgency_s |Dringlichkeit |
| Impact_s |Auswirkung|
| Priority_s | Priority |
| CreatedBy_s | Geöffnet von |
| ResolvedBy_s | Gelöst von|
| ClosedBy_s  | Geschlossen von |
| Source_s| Kontakttyp |
| AssignedTo_s | Zugewiesen zu  |
| Category_s | Category |
| Title_s|  Kurze Beschreibung |
| Description_s|  Notizen |
| CreatedDate_t|  Geöffnet |
| ClosedDate_t| closed|
| ResolvedDate_t|Gelöst|
| Computer  | Konfigurationselement |

## <a name="output-data-for-a-servicenow-change-request"></a>Ausgabedaten für eine ServiceNow-Änderungsanforderung

| Log Analytics | ServiceNow-Feld |
|:--- |:--- |
| ServiceDeskId_s| Number |
| CreatedBy_s | Angefordert von |
| ClosedBy_s | Geschlossen von |
| AssignedTo_s | Zugewiesen zu  |
| Title_s|  Kurze Beschreibung |
| Type_s|  type |
| Category_s|  Category |
| CRState_s|  State|
| Urgency_s|  Dringlichkeit |
| Priority_s| Priority|
| Risk_s| Risiko|
| Impact_s| Auswirkung|
| RequestedDate_t  | Datum der Anforderung |
| ClosedDate_t | Geschlossen am |
| PlannedStartDate_t  |     Geplantes Startdatum |
| PlannedEndDate_t  |   Geplantes Enddatum |
| WorkStartDate_t  | Tatsächliches Startdatum |
| WorkEndDate_t | Tatsächliches Enddatum|
| Description_s | BESCHREIBUNG |
| Computer  | Konfigurationselement |


## <a name="troubleshoot-itsm-connections"></a>Problembehandlung bei ITSM-Verbindungen
1. Wenn für die Verbindung auf der Benutzeroberfläche der verbundenen Quelle ein Fehler auftritt, und die Fehlermeldung **Fehler beim Speichern der Verbindung** angezeigt wird, gehen Sie folgendermaßen vor:
   - Stellen Sie für ServiceNow-, Cherwell- und Provance-Verbindungen sicher,  
   - dass Sie Benutzername, Kennwort, Client-ID  und geheimen Clientschlüssel für jede der Verbindungen richtig eingegeben haben.  
   - Überprüfen Sie, ob Sie über ausreichende Berechtigungen für das entsprechende ITSM-Produkt verfügen, um die Verbindung herzustellen.  
   - Stellen Sie für Service Manager sicher,  
   - dass die Web-App erfolgreich bereitgestellt und die Hybridverbindung erstellt wird. Um zu überprüfen, ob die Verbindung mit dem lokalen Service Manager-Computer erfolgreich hergestellt wird, besuchen Sie die Web-App-URL, wie in der Dokumentation zum Herstellen der [Hybridverbindung](./itsmc-connections.md#configure-the-hybrid-connection) erläutert.  

2. Wenn Daten von ServiceNow nicht in Log Analytics synchronisiert werden, stellen Sie sicher, dass sich die ServiceNow-Instanz nicht im Energiesparmodus befindet. ServiceNow-Entwicklungsinstanzen wechseln manchmal nach längerem Leerlauf in den Energiesparmodus. Melden Sie das Problem andernfalls.
3. Wenn Log Analytics-Warnungen ausgelöst werden, aber keine Arbeitselemente im ITSM-Produkt oder keine Konfigurationselemente erstellt bzw. diese nicht mit Arbeitselementen verknüpft werden, nutzen Sie folgende Quellen. Dort finden Sie darüber hinaus allgemeine Informationen:
   -  ITSMC: Die Lösung zeigt eine Zusammenfassung der Verbindungen/Arbeitselemente/Computer usw. Klicken Sie auf die Kachel **Connectorstatus**. Sie werden zur **Protokollsuche** mit der relevanten Abfrage umgeleitet. Untersuchen Sie die Protokolldatensätze, für die „LogType_S“ den Wert „ERROR“ enthält, auf weitere Informationen.
   - Seite **Protokollsuche** : Sie können die Fehler und die zugehörigen Informationen mithilfe der Abfrage `*`ServiceDeskLog_CL`*` anzeigen.

## <a name="troubleshoot-service-manager-web-app-deployment"></a>Problembehandlung bei der Service Manager-Web-App-Bereitstellung
1.  Stellen Sie bei Problemen mit der Web-App-Bereitstellung sicher, dass Sie für das angegebene Abonnement über ausreichende Berechtigungen zum Erstellen/Bereitstellen von Ressourcen verfügen.
2.  Wenn die Fehlermeldung **Objektverweis ist nicht auf eine Instanz eines Objekts festgelegt** angezeigt wird, während Sie das [Skript](itsmc-service-manager-script.md) ausführen, stellen Sie sicher, dass Sie im Abschnitt **Benutzerkonfiguration** gültige Werte eingegeben haben.
3.  Wenn Sie den Service Bus Relay-Namespace nicht erstellen, stellen Sie sicher, dass der erforderliche Ressourcenanbieter im Abonnement registriert ist. Wenn er nicht registriert ist, erstellen Sie den Service Bus Relay-Namespace manuell über das Azure-Portal. Sie können ihn auch beim [Erstellen der Hybridverbindung](./itsmc-connections.md#configure-the-hybrid-connection) über das Azure-Portal erstellen.


## <a name="contact-us"></a>Kontakt

Kontaktieren Sie uns bei Fragen oder Feedback zum IT Service Management Connector über [omsitsmfeedback@microsoft.com](mailto:omsitsmfeedback@microsoft.com).

## <a name="next-steps"></a>Nächste Schritte
[Hinzufügen von ITSM-Produkten/-Diensten zum IT Service Management Connector](./itsmc-connections.md).

