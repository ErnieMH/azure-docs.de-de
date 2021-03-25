---
title: Verwalten von Daten, Arbeitsblättern und Tabellen in Excel Online
description: Verwalten von Daten in Arbeitsblättern und Tabellen in Excel Online for Business oder Excel Online für OneDrive mithilfe von Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 08/23/2018
tags: connectors
ms.openlocfilehash: 097db6683127b410e713be53e6de838cf7734ddc
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91400722"
---
# <a name="manage-excel-online-data-with-azure-logic-apps"></a>Verwalten von Excel Online-Daten mit Azure Logic Apps

Mit [Azure Logic Apps](../logic-apps/logic-apps-overview.md) und dem [Excel Online for Business](/connectors/excelonlinebusiness/)-Connector oder dem [Excel Online für OneDrive](/connectors/excelonline/)-Connector können Sie automatisierte Aufgaben und Workflows basierend auf Ihren Daten in Excel Online for Business oder OneDrive erstellen. Dieser Connector bietet Aktionen, die Sie beim Verwalten Ihrer Daten und Tabellen unterstützen. Beispiele:

* Sie können neue Arbeitsblätter und Tabellen erstellen.
* Sie können Arbeitsblätter, Tabellen und Zeilen abrufen und verwalten.
* Sie können einzelne Zeilen und Schlüsselspalten hinzufügen.

Sie können dann die Ausgaben dieser Aktionen mit Aktionen für andere Dienste verwenden. Wenn Sie beispielsweise eine Aktion verwenden, die jede Woche Arbeitsblätter erstellt, können Sie eine weitere Aktion verwenden, die über den Office 365 Outlook-Connector eine Bestätigungs-E-Mail sendet.

Falls Sie noch nicht mit Logik-Apps vertraut sind, finden Sie weitere Informationen unter [Was ist Azure Logic Apps?](../logic-apps/logic-apps-overview.md).

> [!NOTE]
> Die Connectors für [Excel Online for Business](/connectors/excelonlinebusiness/) und [Excel Online for OneDrive](/connectors/excelonline/) funktionieren mit Azure Logic Apps und unterscheiden sich vom [Excel-Connector für PowerApps](/connectors/excel/).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Wenn Sie nicht über ein Azure-Abonnement verfügen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/).

* Ein [Geschäfts-, Schul- oder Unikonto](https://www.office.com/) für Ihr Geschäftskonto oder Ihr persönliches Microsoft-Konto.

  Ihre Excel-Daten können sich in einer durch Trennzeichen getrennten Datei (CSV-Datei) in einem Speicherordner befinden, beispielsweise in OneDrive. 
  Sie können diese CSV-Datei auch mit dem [Flatfileconnector](../logic-apps/logic-apps-enterprise-integration-flatfile.md)verwenden.

* Grundlegende Kenntnisse über die [Erstellung von Logik-Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Die Logik-App, in der Sie auf Ihre Excel Online-Daten zugreifen möchten. Dieser Connector stellt nur Aktionen bereit, daher müssen Sie zum Starten Ihrer Logik-App einen separaten Trigger auswählen, z.B. einen **Wiederholungstrigger**.

## <a name="add-excel-action"></a>Hinzufügen eine Excel-Aktion

1. Öffnen Sie Ihre Logik-App über das [Azure-Portal](https://portal.azure.com) im Logik-App-Designer, wenn sie nicht bereits geöffnet ist.

1. Wählen Sie unter dem Trigger die Option **Neuer Schritt** aus.

1. Geben Sie im Suchfeld den Begriff „excel“ als Filter ein. Wählen Sie in der Liste mit den Aktionen die gewünschte Aktion aus.

   > [!NOTE]
   > Der Logik-App-Designer kann keine Tabellen laden, die über 100 oder mehr Spalten verfügen. Verringern Sie nach Möglichkeit die Anzahl der Spalten in der ausgewählten Tabelle, damit der Designer die Tabelle laden kann.

1. Melden Sie sich bei Aufforderung dazu bei Ihrem Geschäfts-, Schul- oder Unikonto an.

   Ihre Anmeldeinformationen autorisieren Ihre Logik-App zur Erstellung einer Verbindung mit Excel Online sowie zum Zugriff auf Ihre Daten.

1. Fahren Sie damit fort, die erforderlichen Informationen für die ausgewählte Aktion einzugeben und Ihren Logik-App-Workflow zu erstellen.

## <a name="connector-reference"></a>Connector-Referenz

Technische Details wie Trigger, Aktionen und Limits, wie sie in der OpenAPI-Datei (ehemals Swagger) des Connectors beschrieben werden, finden Sie auf diesen Referenzseite des Connectors:

* [Excel Online for Business](/connectors/excelonlinebusiness/)
* [Excel Online for OneDrive](/connectors/excelonline/)

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu anderen [Logic Apps-Connectors](../connectors/apis-list.md)
