---
title: Registrieren und Überprüfen eines Power BI-Mandanten (Vorschau)
description: Erfahren Sie, wie Sie über das Azure Purview-Portal einen Power BI-Mandanten registrieren und überprüfen können.
author: chanuengg
ms.author: csugunan
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 11/19/2020
ms.openlocfilehash: 8fb4c797df7961726ca785a56a6ab25807999842
ms.sourcegitcommit: 950e98d5b3e9984b884673e59e0d2c9aaeabb5bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2021
ms.locfileid: "107600861"
---
# <a name="register-and-scan-a-power-bi-tenant-preview"></a>Registrieren und Überprüfen eines Power BI-Mandanten (Vorschau)

In diesem Artikel wird gezeigt, wie Sie das Azure Purview-Portal zum Registrieren und Überprüfen eines Power BI-Mandanten verwenden.

> [!Note]
> Wenn sich die Purview-Instanz und der Power BI-Mandant im selben Azure-Mandanten befinden, können Sie nur die Authentifizierung mit verwalteten Identitäten (MSI) verwenden, um eine Überprüfung eines Power BI-Mandanten einzurichten. 

## <a name="create-a-security-group-for-permissions"></a>Erstellen einer Sicherheitsgruppe für Berechtigungen

Erstellen Sie zum Einrichten der Authentifizierung eine Sicherheitsgruppe, und fügen Sie ihr die verwaltete Purview-Identität hinzu.

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach **Azure Active Directory**.
1. Erstellen Sie eine neue Sicherheitsgruppe in Ihrem Azure Active Directory-System, indem Sie gemäß [Erstellen einer Basisgruppe und Hinzufügen von Mitgliedern mit Azure Active Directory](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md) vorgehen.

    > [!Tip]
    > Sie können diesen Schritt überspringen, wenn Sie bereits über eine Sicherheitsgruppe verfügen, die Sie verwenden möchten.

1. Wählen Sie als **Gruppentyp** die Option **Sicherheit** aus.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/security-group.png" alt-text="Gruppentyp „Sicherheit“":::

1. Fügen Sie dieser Sicherheitsgruppe die verwaltete Purview-Identität hinzu. Wählen Sie **Mitglieder** und dann **+ Mitglieder hinzufügen** aus.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/add-group-member.png" alt-text="Der Gruppe die verwaltete Instanz des Katalogs hinzufügen.":::

1. Suchen Sie nach Ihrer verwalteten Purview-Identität, und wählen Sie sie aus.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/add-catalog-to-group-by-search.png" alt-text="Katalog hinzufügen, indem nach ihm gesucht wird":::

    Es sollte eine Erfolgsmeldung angezeigt werden, die Ihnen mitteilt, dass der Katalog hinzugefügt wurde.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/success-add-catalog-msi.png" alt-text="Katalog hinzufügen, MSI-Erfolg":::

## <a name="associate-the-security-group-with-the-tenant"></a>Verknüpfen der Sicherheitsgruppe mit dem Mandanten

1. Melden Sie sich beim [Power BI-Verwaltungsportal](https://app.powerbi.com/admin-portal/tenantSettings) an.
1. Wählen Sie die Seite **Mandanteneinstellungen** aus.

    > [!Important]
    > Sie müssen Power BI-Administrator sein, um die Seite mit den Mandanteneinstellungen anzeigen zu können.

1. Wählen Sie **Administrator-API-Einstellungen** > **Dienstprinzipalen die Verwendung schreibgeschützter Power BI-Administrator-APIs gestatten (Vorschau)** aus.
1. Wählen Sie **Sicherheitsgruppen angeben** aus.

    :::image type="content" source="./media/setup-power-bi-scan-PowerShell/allow-service-principals-power-bi-admin.png" alt-text="Bild, das zeigt, wie Dienstprinzipale schreibgeschützte Power BI-Administrator-API-Berechtigungen erhalten können":::

    > [!Caution]
    > Wenn Sie der von Ihnen erstellten Sicherheitsgruppe (die Ihre verwaltete Purview-Identität als Mitglied hat) gestatten, schreibgeschützte Power BI-Administrator-APIs zu verwenden, gestatten Sie ihr auch, auf die Metadaten (z. B. Dashboard- und Berichtsnamen, Besitzer, Beschreibungen usw.) für sämtliche Ihrer Power BI-Artefakte in diesem Mandanten zugreifen zu können. Sobald die Metadaten in Azure Purview eingelesen wurden, wird anhand der Pruview-Berechtigungen, nicht anhand der Power BI-Berechtigungen, festgelegt, wer diese Metadaten anzeigen kann.

    > [!Note]
    > Sie können die Sicherheitsgruppe aus Ihren Entwicklereinstellungen entfernen, die zuvor extrahierten Metadaten werden jedoch nicht aus dem Purview-Konto entfernt. Bei Bedarf können Sie sie separat löschen.

## <a name="register-your-power-bi-and-set-up-a-scan"></a>Registrieren von Power BI und Einrichten einer Überprüfung

Nachdem Sie nun der verwalteten Purview-Identität die Berechtigung erteilt haben, eine Verbindung mit der Administrator-API Ihres Power BI-Mandanten herzustellen, können Sie Ihre Überprüfung über Azure Purview Studio einrichten.

1. Wählen Sie im linken Navigationsbereich die Option **Quellen** aus.

1. Klicken Sie anschließend auf **Registrieren**.

    Wählen Sie **Power BI** als Ihre Datenquelle aus.

    :::image type="content" source="media/setup-power-bi-scan-catalog-portal/select-power-bi-data-source.png" alt-text="Bild, das die Liste der verfügbaren Datenquellen anzeigt":::

3. Geben Sie Ihrer Power BI-Instanz einen Anzeigenamen.

    :::image type="content" source="media/setup-power-bi-scan-catalog-portal/power-bi-friendly-name.png" alt-text="Bild, das den Anzeigenamen der Power BI-Datenquelle anzeigt":::

    Der Name muss zwischen 3 und 63 Zeichen lang sein und darf nur Buchstaben, Ziffern, Unterstriche und Bindestriche enthalten.  Leerzeichen sind nicht zulässig.

    Standardmäßig findet das System den Power BI-Mandanten, der in demselben Azure-Abonnement vorhanden ist.

    :::image type="content" source="media/setup-power-bi-scan-catalog-portal/power-bi-datasource-registered.png" alt-text="Registrierte Power BI-Datenquelle":::

    > [!Note]
    > Für Power BI ist die Registrierung und Überprüfung von Datenquellen nur für eine Instanz zulässig.


4. Geben Sie Ihrer Überprüfung einen Namen. Wählen Sie dann die Option zum Einschließen oder Ausschließen der persönlichen Arbeitsbereiche aus. Beachten Sie, dass die einzige unterstützte Authentifizierungsmethode **Verwaltete Identität** ist.

    :::image type="content" source="media/setup-power-bi-scan-catalog-portal/power-bi-scan-setup.png" alt-text="Bild, das die Einrichtung der Power BI-Überprüfung zeigt":::

    > [!Note]
    > * Wenn Sie die Konfiguration einer Überprüfung so wechseln, dass ein persönlicher Arbeitsbereich ein- oder ausgeschlossen wird, wird eine vollständige Überprüfung der Power BI-Quelle ausgelöst.
    > * Der Überprüfungsname muss zwischen 3 und 63 Zeichen lang sein und darf nur Buchstaben, Ziffern, Unterstriche und Bindestriche enthalten. Leerzeichen sind nicht zulässig.

5. Richten Sie einen Auslöser für die Überprüfung ein. Ihre Optionen sind **Einmal**, **Alle 7 Tage** und **Alle 30 Tage**.

    :::image type="content" source="media/setup-power-bi-scan-catalog-portal/scan-trigger.png" alt-text="Bild, das den Auslöser der Überprüfung zeigt":::

6. Wählen Sie für **Neue Überprüfung überprüfen** die Option **Speichern und ausführen** aus, um Ihre Überprüfung zu starten.

    :::image type="content" source="media/setup-power-bi-scan-catalog-portal/save-run-power-bi-scan.png" alt-text="Bild des Power BI-Bildschirms zu „Speichern und ausführen“":::

## <a name="next-steps"></a>Nächste Schritte

- [Browsen im Azure Purview-Datenkatalog](how-to-browse-catalog.md)
- [Suchen im Azure Purview-Datenkatalog](how-to-search-catalog.md)
