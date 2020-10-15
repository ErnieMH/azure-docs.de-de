---
title: Senden von Azure Service Health-Warnungen mit OpsGenie mit Webhooks
description: Erhalten Sie personalisierte Benachrichtigungen zu Service Health-Ereignissen an Ihre OpsGenie-Instanz.
ms.topic: conceptual
ms.date: 06/10/2019
ms.openlocfilehash: 112774cb1f9e16b08225471e8dbc1bb79b1bd37d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "86529096"
---
# <a name="send-azure-service-health-alerts-with-opsgenie-using-webhooks"></a>Senden von Azure Service Health-Warnungen mit OpsGenie mit Webhooks

In diesem Artikel erfahren Sie, wie Sie Azure Service Health-Warnungen mit Webhook in OpsGenie einrichten. Mithilfe der Azure Service Health-Integration von [OpsGenie](https://www.opsgenie.com/) können Sie Azure Service Health-Warnungen an OpsGenie weiterleiten. OpsGenie kann die zuständigen Personen basierend auf Bereitschaftszeitplänen bestimmen und mithilfe von E-Mails, Textnachrichten (SMS), Telefonanrufen, iOS- und Android-Pushbenachrichtigungen benachrichtigen sowie Warnungen eskalieren, bis diese bestätigt oder geschlossen werden.

## <a name="creating-a-service-health-integration-url-in-opsgenie"></a>Erstellen einer URL für die Service Health-Integration in OpsGenie
1.  Stellen Sie sicher, dass Sie Ihr [OpsGenie](https://www.opsgenie.com/)-Konto registriert haben und angemeldet sind.

1.  Navigieren Sie zum Abschnitt **Integrations** (Integrationen) in OpsGenie.

    ![Abschnitt „Integrations“ in OpsGenie](./media/webhook-alerts/opsgenie-integrations-section.png)

1.  Wählen Sie die Schaltfläche für die **Azure Service Health**-Integration aus.

    ![Schaltfläche „Azure Service Health“ in OpsGenie](./media/webhook-alerts/opsgenie-azureservicehealth-button.png)

1.  **Benennen** Sie die Warnung, und füllen Sie das Feld **Assigned to Team** (Dem Team zugewiesen) aus.

1.  Füllen Sie die anderen Felder aus: **Recipients** (Empfänger), **Enabled** (Aktiviert) und **Suppress Notifications** (Benachrichtigungen unterdrücken).

1.  Kopieren und speichern Sie die **Integrations-URL**, der am Ende bereits Ihr `apiKey` angefügt sein sollte.

    ![„Integration URL“ in OpsGenie](./media/webhook-alerts/opsgenie-integration-url.png)

1.  Wählen Sie **Save Integration** (Integration speichern) aus.

## <a name="create-an-alert-using-opsgenie-in-the-azure-portal"></a>Erstellen einer Warnung mithilfe von OpsGenie im Azure-Portal
### <a name="for-a-new-action-group"></a>Für eine neue Aktionsgruppe:
1. Befolgen Sie die Schritte 1 bis 8 in [Erstellen einer Warnung zu einer Dienstintegritätsbenachrichtigung für eine neue Aktionsgruppe mit dem Azure-Portal](./alerts-activity-log-service-notifications-portal.md).

1. Definieren Sie in der Liste der **Aktionen** Folgendes:

    a. **Aktionstyp:** *Webhook*

    b. **Details:** Die zuvor gespeicherte OpsGenie-**Integrations-URL**.

    c. **Name:** Name, Alias oder Bezeichner des Webhook.

1. Wählen Sie **Save** (Speichern) aus, wenn das Erstellen der Warnung abgeschlossen ist.

### <a name="for-an-existing-action-group"></a>Für eine vorhandene Aktionsgruppe:
1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) die Option **Überwachen** aus.

1. Wählen Sie im Abschnitt **Einstellungen** die Option **Aktionsgruppen** aus.

1. Suchen und markieren Sie die Aktionsgruppe, die Sie bearbeiten möchten.

1. Fügen Sie Folgendes zur Liste der **Aktionen** hinzu:

    a. **Aktionstyp:** *Webhook*

    b. **Details:** Die zuvor gespeicherte OpsGenie-**Integrations-URL**.

    c. **Name:** Name, Alias oder Bezeichner des Webhook.

1. Wählen Sie **Save** (Speichern) aus, wenn Sie mit dem Aktualisieren der Aktionsgruppe fertig sind.

## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Testen der Webhookintegration über eine HTTP POST-Anforderung
1. Erstellen Sie die Service Health-Nutzlast, die Sie senden möchten. Eine Service Health-Beispielwebhook-Nutzlast finden Sie unter [Webhooks für Azure-Aktivitätsprotokollwarnungen](../azure-monitor/platform/activity-log-alerts-webhook.md).

1. Erstellen Sie eine HTTP POST-Anforderung, indem Sie wie folgt vorgehen:

    ```
    POST        https://api.opsgenie.com/v1/json/azureservicehealth?apiKey=<APIKEY>

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
1. Sie sollten eine `200 OK`-Antwort mit der Meldung „successful“ (Erfolgreich) erhalten.

1. Wechseln Sie zu [OpsGenie](https://www.opsgenie.com/), um zu überprüfen, ob Ihre Integration erfolgreich eingerichtet wurde.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie [Webhookbenachrichtigungen für vorhandene Problemverwaltungssysteme konfigurieren](service-health-alert-webhook-guide.md).
- Weitere Informationen zum [Webhookschema für Aktivitätsprotokollwarnungen](../azure-monitor/platform/activity-log-alerts-webhook.md). 
- Weitere Informationen zu [Dienstintegritätsbenachrichtigungen](./service-notifications.md).
- Weitere Informationen zu [Aktionsgruppen](../azure-monitor/platform/action-groups.md).
