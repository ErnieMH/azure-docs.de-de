---
title: Abrufen weiterer Elemente oder Datensätze mit Paginierung
description: Einrichten der Paginierung zur Überschreitung der Standardseitengröße für Connectoraktionen in Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 04/11/2019
ms.openlocfilehash: d46bf711a46e27b81a1284b5fc55cf403b7da048
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87090262"
---
# <a name="get-more-data-items-or-records-by-using-pagination-in-azure-logic-apps"></a>Abrufen weiterer Daten, Elemente oder Datensätze mithilfe der Paginierung in Azure Logic Apps

Wenn Sie Daten, Elemente oder Datensätze mithilfe einer Connectoraktion in [Azure Logic Apps](../logic-apps/logic-apps-overview.md) abrufen, erhalten Sie möglicherweise so große Resultsets, dass die Aktion nicht alle Ergebnisse gleichzeitig zurückgibt. Bei einigen Aktionen kann die Anzahl der Ergebnisse die Standardseitengröße des Connectors überschreiten. In diesem Fall gibt die Aktion nur die erste Seite mit Ergebnissen zurück. Die Standardseitengröße für die Aktion **Zeilen abrufen** des SQL Server-Connectors ist z. B. 2048, dieser Wert kann aber basierend auf anderen Einstellungen abweichen.

Bei einigen Aktionen können Sie eine *Paginierung*-Einstellung aktivieren, sodass Ihre Logik-App mehr Ergebnisse bis zur Paginierungsgrenze abrufen kann, diese aber nach Abschluss der Aktion als einzelne Nachricht zurückgibt. Wenn Sie die Paginierung verwenden, müssen Sie einen *Schwellenwert* angeben, der die Zielanzahl der Ergebnisse angibt, die von der Aktion zurückgegeben werden sollen. Die Aktion ruft Ergebnisse ab, bis der angegebene Schwellenwert erreicht ist. Wenn Ihre Gesamtzahl der Elemente unter dem angegebenen Schwellenwert liegt, ruft die Aktion alle Ergebnisse ab.

Durch Aktivieren der Paginierungseinstellung werden Ergebnisseiten abgerufen, die auf der Seitengröße eines Connectors basieren. Dieses Verhalten bedeutet, dass Sie unter Umständen mehr Ergebnisse erzielen können, als durch den Schwellenwert angegeben. Wenn Sie z. B. die SQL Server-Aktion **Zeilen abrufen** verwenden, die die Paginierungseinstellung unterstützt:

* Die Standardseitengröße der Aktion beträgt 2048 Datensätze pro Seite.
* Angenommen, Sie verfügen über 10.000 Datensätze und geben als Mindestwert 5000 Datensätze an.
* Die Paginierung ruft Seiten von Datensätzen ab, daher gibt die Aktion 6144 (3 Seiten x 2048 Datensätze) und nicht 5000 Datensätze zurück, um mindestens den angegebenen Mindestwert zu erreichen.

Hier folgt eine Liste mit nur einigen der Connectors, bei denen Sie die Standardseitengröße für bestimmte Aktionen überschreiten können:

* [Azure Blob Storage](/connectors/azureblob/)
* [Dynamics 365](/connectors/dynamicscrmonline/)
* [Excel](/connectors/excel/)
* [HTTP](../connectors/connectors-native-http.md)
* [IBM DB2](/connectors/db2/)
* [Microsoft Teams](/connectors/teams/)
* [Oracle Database](/connectors/oracle/)
* [Salesforce](/connectors/salesforce/)
* [SharePoint](/connectors/sharepointonline/)
* [SQL Server](/connectors/sql/)

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie noch kein Azure-Abonnement haben, [melden Sie sich für ein kostenloses Azure-Konto an](https://azure.microsoft.com/free/).

* Die Logik-App und die Aktion, bei der Sie die Paginierung aktivieren möchten. Wenn Sie über keine Logik-App verfügen, lesen Sie den Artikel [Schnellstart: Erstellen Ihres ersten automatisierten Workflows mit Azure Logic Apps – Azure-Portal](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="turn-on-pagination"></a>Aktivieren der Paginierung

Um festzustellen, ob eine Aktion die Paginierung im Logic App Designer unterstützt, überprüfen Sie die Einstellungen der Aktion für die Einstellung **Paginierung**. In diesem Beispiel wird veranschaulicht, wie die Paginierung in der SQL Server-Aktion **Zeilen abrufen** aktiviert wird.

1. Wählen Sie in der Aktion oben rechts zuerst die Auslassungspunkte ( **...** ) und dann **Einstellungen** aus.

   ![Öffnen der Aktionseinstellungen](./media/logic-apps-exceed-default-page-size-with-pagination/sql-action-settings.png)

   Wenn die Aktion die Paginierung unterstützt, zeigt sie die Einstellung **Paginierung** an.

1. Ändern Sie die Einstellung **Paginierung** von **Aus** in **Ein**. Geben Sie in der Eigenschaft **Schwellenwert** einen ganzzahligen Wert für die Zielanzahl der Ergebnisse an, die die Aktion zurückgeben soll.

   ![Angeben der minimalen Anzahl der zurückzugebenden Ergebnisse](./media/logic-apps-exceed-default-page-size-with-pagination/sql-action-settings-pagination.png)

1. Wählen Sie abschließend **Fertig** aus.

## <a name="workflow-definition---pagination"></a>Workflowdefinition – Paginierung

Wenn Sie die Paginierung für eine Aktion aktivieren, die diese Funktion unterstützt, enthält die Workflowdefinition Ihrer Logik-App die Eigenschaft `"paginationPolicy"` zusammen mit der Eigenschaft `"minimumItemCount"` in der Eigenschaft `"runtimeConfiguration"` dieser Aktion. Beispiel:

```json
"actions": {
   "HTTP": {
      "inputs": {
         "method": "GET",
         "uri": "https://www.testuri.com"
      },
      "runAfter": {},
      "runtimeConfiguration": {
         "paginationPolicy": {
            "minimumItemCount": 1000
         }
      },
      "type": "Http"
   }
},
```

## <a name="get-support"></a>Support

Weitere Informationen finden Sie auf der [Frageseite von Microsoft Q&A für Azure Logic Apps](/answers/topics/azure-logic-apps.html).
