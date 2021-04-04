---
title: Anzeigen relevanter Azure Advisor-Empfehlungen
description: Informationen zum Anzeigen und Filtern von Azure Advisor-Empfehlungen für mehr Übersichtlichkeit.
ms.topic: article
ms.date: 04/03/2019
ms.openlocfilehash: 43bdaba7d774bba8857a6eb3ef296d7ab8e1c264
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "90986868"
---
# <a name="view-azure-advisor-recommendations-that-matter-to-you"></a>Anzeigen relevanter Azure Advisor-Empfehlungen

Azure Advisor bietet Empfehlungen, die Ihnen helfen, Ihre Azure-Bereitstellungen zu optimieren. Innerhalb von Advisor haben Sie Zugriff auf verschiedene Features, die Ihnen helfen, Ihre Empfehlungen auf diejenigen zu beschränken, die für Sie relevant sind.

## <a name="configure-subscriptions-and-resource-groups"></a>Konfigurieren von Abonnements und Ressourcengruppen

Advisor gibt Ihnen die Möglichkeit, Abonnements und Ressourcengruppen auszuwählen, die für Sie und Ihr Unternehmen von Belang sind. Sie sehen nur Empfehlungen für die Abonnements und Ressourcengruppen, die Sie auswählen. Standardmäßig sind alle ausgewählt. Konfigurationseinstellungen gelten für das Abonnement oder die Ressourcengruppe, sodass die gleichen Einstellungen für alle Benutzer gelten, die Zugriff auf dieses Abonnement oder diese Ressourcengruppe haben. Konfigurationseinstellungen können im Azure-Portal oder programmgesteuert geändert werden.

So nehmen Sie Änderungen im Azure-Portal vor

1. Öffnen Sie [Azure Advisor](https://aka.ms/azureadvisordashboard) im Azure-Portal.

1. Wählen Sie im Menü die Option **Konfiguration** aus.

   ![Konfigurationsmenü von Advisor](./media/view-recommendations/configuration.png)

1. Aktivieren Sie das Kontrollkästchen **Einschließen** für alle Abonnements oder Ressourcengruppen, für die Sie Advisor-Empfehlungen erhalten möchten. Wenn das Kontrollkästchen deaktiviert ist, haben Sie möglicherweise keine Berechtigung, eine Konfigurationsänderung für dieses Abonnement oder diese Ressourcengruppe vorzunehmen. Erfahren Sie mehr zu [Berechtigungen in Azure Advisor](permissions.md).

1. Klicken Sie nach einer Änderung unten auf **Übernehmen**.

## <a name="filtering-your-view-in-the-azure-portal"></a>Filtern der Ansicht im Azure-Portal

Konfigurationseinstellungen bleiben aktiv, bis Sie geändert werden. Wenn Sie die Empfehlungen für eine einzelne Ansicht einschränken möchten, können Sie dazu die Dropdownfelder oben im Advisor-Bereich verwenden. In den Bereichen „Übersicht“, „Hochverfügbarkeit“, „Sicherheit“, „Leistung“, „Kosten“ und „Alle Empfehlungen“ können Sie die Abonnements, Ressourcentypen und den gewünschten Empfehlungsstatus auswählen.

   :::image type="content" source="./media/view-recommendations/filtering.png" alt-text="Screenshot: Azure Advisor zeigt Filteroptionen an.":::

## <a name="dismissing-and-postponing-recommendations"></a>Ablehnen und Zurückstellen von Empfehlungen

Azure Advisor ermöglicht Ihnen, Empfehlungen für eine einzelne Ressource abzulehnen oder zurückzustellen. Wenn Sie eine Empfehlung ablehnen, sehen Sie sie erst wieder, sobald Sie sie manuell aktivieren. Beim Zurückstellen einer Empfehlung können Sie jedoch eine Dauer festlegen, nach der die Empfehlung automatisch wieder aktiviert wird. Das Zurückstellen ist im Azure-Portal oder programmgesteuert möglich.

### <a name="postpone-a-single-recommendation-in-the-azure-portal"></a>Zurückstellen einer einzelnen Empfehlung im Azure-Portal 

1. Öffnen Sie [Azure Advisor](https://aka.ms/azureadvisordashboard) im Azure-Portal.
1. Wählen Sie eine Empfehlungskategorie aus, um Ihre Empfehlungen anzuzeigen.
1. Wählen Sie in der Liste „Empfehlungen“ eine Empfehlung aus.
1. Wählen Sie für eine Empfehlung entweder „Zurückstellen“ oder „Schließen“ aus, um sie zurückzustellen oder abzulehnen.

     :::image type="content" source="./media/view-recommendations/postpone-dismiss.png" alt-text="Screenshot: Fenster „Verwaltete Datenträger verwenden“ mit den für eine einzelne Empfehlung hervorgehoben Aktionen „Verschieben“ und „Schließen“.":::

### <a name="postpone-or-dismiss-a-multiple-recommendations-in-the-azure-portal"></a>Zurückstellen oder Ablehnen mehrerer Empfehlungen im Azure-Portal

1. Öffnen Sie [Azure Advisor](https://aka.ms/azureadvisordashboard) im Azure-Portal.
1. Wählen Sie eine Empfehlungskategorie aus, um Ihre Empfehlungen anzuzeigen.
1. Wählen Sie in der Liste „Empfehlungen“ eine Empfehlung aus.
1. Aktivieren Sie das Kontrollkästchen links neben der Zeile für alle Ressourcen, für die Sie die Empfehlung zurückstellen oder ablehnen möchten.
1. Klicken Sie links oben in der Tabelle auf **Verschieben** oder **Schließen**.

     :::image type="content" source="./media/view-recommendations/postpone-dismiss-multiple.png" alt-text="Screenshot: Fenster „Verwaltete Datenträger verwenden“ mit der ausgewählten Spalte und den oben links in der Tabelle hervorgehobenen Aktionen „Verschieben“ und „Schließen“.":::

> [!NOTE]
> Sie benötigen die Berechtigung „Mitwirkender“ oder „Besitzer“, um eine Empfehlung abzulehnen oder zurückzustellen. Erfahren Sie mehr zu Berechtigungen in Azure Advisor.

> [!NOTE]
> Wenn die Auswahlkontrollkästchen deaktiviert sind, werden ggf. noch Empfehlungen geladen. Warten Sie, bis alle Empfehlungen geladen sind, bevor Sie versuchen, sie zurückzustellen oder abzulehnen.

### <a name="reactivate-a-postponed-or-dismissed-recommendation"></a>Reaktivieren einer zurückgestellten oder abgelehnten Empfehlung

Sie können eine Empfehlung reaktivieren, die zurückgestellt oder abgelehnt wurde. Dieser Vorgang ist im Azure-Portal oder programmgesteuert möglich. Führen Sie im Azure-Portal die folgenden Schritte aus:

1. Öffnen Sie [Azure Advisor](https://aka.ms/azureadvisordashboard) im Azure-Portal.

1. Ändern Sie den Filter im Bereich „Übersicht“ in **Verschoben**. Advisor zeigt zurückgestellte oder abgelehnte Empfehlungen.

    :::image type="content" source="./media/view-recommendations/activate-postponed.png" alt-text="Screenshot: Azure Advisor-Fenster mit dem ausgewählten Dropdownmenü „Verschoben“.":::

1. Wählen Sie eine Kategorie aus, um **zurückgestellte** und **abgelehnte** Empfehlungen einzublenden.

1. Wählen Sie in der Liste „Empfehlungen“ eine Empfehlung aus. Dadurch werden Empfehlungen auf der Registerkarte **Verschoben und verworfen** geöffnet, die bereits ausgewählt wurden, um die Ressourcen anzuzeigen, für die diese Empfehlung zurückgestellt oder abgelehnt wurde.

1. Klicken Sie am Ende der Zeile auf **Aktivieren**. Nach dem Klicken ist die Empfehlung für diese Ressource aktiv und wird daher aus dieser Tabelle entfernt. Die Empfehlung wird nun auf der Registerkarte **Aktiv** angezeigt.
 
     :::image type="content" source="./media/view-recommendations/activate-postponed-2.png" alt-text="Screenshot: Fenster „Vorläufiges Löschen aktivieren“, das die Registerkarte „Verschoben und verworfen“ zeigt, wobei die Aktion „Aktivieren“ hervorgehoben ist.":::

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel wird erläutert, wie Sie Empfehlungen anzeigen können, die in Azure Advisor für Sie relevant sind. Weitere Informationen zum Advisor: 

- [Was ist Azure Advisor?](advisor-overview.md)
- [Erste Schritte mit Advisor](advisor-get-started.md)
- [Berechtigungen in Azure Advisor](permissions.md)



