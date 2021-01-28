---
title: Azure Enterprise-REST-APIs
description: In diesem Artikel werden die REST-APIs beschrieben, die Sie mit Ihrer Azure Enterprise-Registrierung verwenden können.
author: bandersmsft
ms.author: banders
ms.date: 01/21/2021
ms.topic: conceptual
ms.service: cost-management-billing
ms.subservice: enterprise
ms.reviewer: boalcsva
ms.openlocfilehash: 1fdf64053a55eb33d80ed461c231e8c6dd84d63b
ms.sourcegitcommit: b39cf769ce8e2eb7ea74cfdac6759a17a048b331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98677730"
---
# <a name="azure-enterprise-rest-apis"></a>Azure Enterprise-REST-APIs

In diesem Artikel werden die REST-APIs beschrieben, die Sie mit Ihrer Azure Enterprise-Registrierung verwenden können. Außerdem wird erläutert, wie allgemeine Probleme mit REST-APIs gelöst werden.

## <a name="consumption-and-usage-apis"></a>Verbrauchs- und Verwendungs-APIs

Microsoft Enterprise Azure-Kunden erhalten über REST-APIs Verwendungs- und Abrechnungsinformationen. Der Rollenbesitzer (Unternehmensadministrator, Abteilungsadministrator, Kontobesitzer) muss den Zugriff auf die API aktivieren, indem er im Azure EA-Portal einen Schlüssel generiert. Anschließend kann jeder, der über die Registrierungsnummer und den Schlüssel verfügt, über die API auf die Daten zugreifen.

### <a name="available-apis"></a>Verfügbare APIs

**Saldo und Zusammenfassung**: Die [API für Saldo und Zusammenfassung](/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) bietet eine monatliche Übersicht über Informationen zu Salden, neuen Käufen, Azure Marketplace-Gebühren, Korrekturen und Überschreitungsgebühren. Weitere Informationen finden Sie unter [Berichterstellungs-APIs für Unternehmenskunden – Saldo und Zusammenfassung](/rest/api/billing/enterprise/billing-enterprise-api-balance-summary).

**Verwendungsdetails**: Die [API für Verwendungsdetails](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) bietet eine tägliche Aufschlüsselung der verbrauchten Mengen und durch eine Registrierung anfallenden geschätzten Kosten. Das Ergebnis umfasst auch Informationen zu Instanzen, Verbrauchseinheiten und Abteilungen. Sie können die API nach Abrechnungszeitraum oder nach einem angegebenen Start- und Enddatum abfragen. Weitere Informationen finden Sie unter [Berichterstellungs-APIs für Unternehmenskunden – Verwendungsdetails](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail).

**Marketplace-Gebühren**: Die [API für Marketplace-Gebühren](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) gibt eine Aufschlüsselung der nutzungsbasierten Marketplace-Gebühren pro Tag für den angegebenen Abrechnungszeitraum oder für festgelegte Start- und Enddaten zurück. Weitere Informationen finden Sie unter [Berichterstellungs-APIs für Unternehmenskunden – Marketplace-Gebühren](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge).

**Preisblatt**: Die [Preisblatt-API](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) stellt den zutreffenden Tarif pro Verbrauchseinheit für einen Registrierungs- und Abrechnungszeitraum bereit. Weitere Informationen finden Sie unter [Berichterstellungs-APIs für Unternehmenskunden – Preisblatt](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet).

**Abrechnungszeiträume**: Die [API für Abrechnungszeiträume](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) gibt eine Liste von Abrechnungszeiträumen zurück, die Verbrauchsdaten für eine Registrierung in umgekehrter chronologischer Reihenfolge enthalten. Jeder Zeitraum enthält eine Eigenschaft, die auf die API-Route für die vier Sätze von Daten verweist: BalanceSummary, UsageDetails, Marketplace Charges und PriceSheet. Weitere Informationen finden Sie unter [Berichterstellungs-APIs für Unternehmenskunden – Abrechnungszeiträume](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods).

### <a name="enable-api-data-access"></a>Aktivieren des API-Datenzugriffs

Rollenbesitzer können die folgenden Schritte im Azure EA-Portal ausführen. Dafür navigieren sie zu **Berichte** > **Nutzung herunterladen** > **API-Zugriffsschlüssel**. Anschließend können sie folgende Aktionen ausführen:

- Primäre und sekundäre Zugriffsschlüssel generieren
- Zugriffsschlüssel deaktivieren
- Start- und Enddaten von Zugriffsschlüsseln anzeigen

### <a name="generate-or-retrieve-the-api-key"></a>Generieren oder Abrufen des API-Schlüssels

1. Melden Sie sich als Unternehmensadministrator an.
2. Klicken Sie im linken Navigationsfenster auf **Berichte** und anschließend auf die Registerkarte **Nutzung herunterladen**.
3. Klicken Sie auf **API-Zugriffsschlüssel**.
4. Wählen Sie unter **Registrierungs-Zugriffsschlüssel** das Symbol zum Generieren eines Schlüssels aus, um entweder einen primären oder einen sekundären Schlüssel zu generieren.
5. Wählen Sie **Schlüssel erweitern** aus, um den gesamten generierten API-Zugriffsschlüssel anzuzeigen.
6. Wählen Sie **Kopieren** aus, um den API-Zugriffsschlüssel für die sofortige Verwendung abzurufen.

![Beispiel für die Seite „API-Zugriffsschlüssel“](./media/ea-portal-rest-apis/ea-create-generate-or-retrieve-the-api-key.png)

Führen Sie die folgenden Schritte aus, wenn Sie den Benutzern, die keine Unternehmensadministratoren sind, die API-Zugriffsschlüssel für Ihre Registrierung geben möchten:

1. Klicken Sie im linken Navigationsfenster auf **Verwalten**.
2. Klicken Sie auf das Stiftsymbol neben **DA-Ansichtsgebühren** (Department Administrator, Abteilungsadministrator).
3. Wählen Sie **Aktivieren** und anschließend **Speichern** aus.
4. Klicken Sie auf das Stiftsymbol neben **AO-Ansichtsgebühren** (Account Owner, Kontobesitzer).
5. Wählen Sie **Aktivieren** und anschließend **Speichern** aus.

![Beispiel für aktivierte DA- und AO-Ansichtsgebühren](./media/ea-portal-rest-apis/create-ea-generate-or-retrieve-api-key-enable-ao-do-view.png) Die oben beschriebenen Schritte ermöglichen Inhabern von API-Zugriffsschlüsseln den Zugriff auf Kosten- und Preisinformationen in Nutzungsberichten.

### <a name="pass-keys-in-the-api"></a>Übergeben von Schlüsseln in der API

Übergeben Sie den API-Schlüssel zur Authentifizierung und Autorisierung. Übergeben Sie die folgende Eigenschaft an HTTP-Header:

| Anforderungsheaderschlüssel | Wert |
| --- | --- |
| Authorization | Geben Sie den Wert im folgenden Format an: **bearer {API\_KEY}** .
Beispiel: bearer \&lt;APIKey\&gt; |

### <a name="swagger"></a>Swagger

Ein Swagger-Endpunkt ist unter [Enterprise Reporting V3 APIs](https://consumption.azure.com/swagger/ui/index) für die folgenden APIs verfügbar. Swagger hilft Ihnen beim Überprüfen der API. Verwenden Sie Swagger, um Client-SDKs mithilfe von [AutoRest](https://github.com/Azure/AutoRest) oder [Swagger CodeGen](https://swagger.io/swagger-codegen/) zu generieren. Ab dem 1. Mai 2014 verfügbare Daten sind über die API verfügbar.

### <a name="api-response-codes"></a>API-Antwortcodes

Wenn Sie eine API verwenden, werden die Antwortstatuscodes angezeigt. Sie sind in der folgenden Tabelle beschrieben.

| Antwortstatuscode | `Message` | BESCHREIBUNG |
| --- | --- | --- |
| 200 | OK | Kein Fehler |
| 401 | Nicht autorisiert | API-Schlüssel nicht gefunden, ungültig, abgelaufen usw. |
| 404 | Nicht verfügbar | Berichtsendpunkt nicht gefunden |
| 400 | Ungültige Anforderung | Ungültige Parameter – Datumsbereiche, EA-Nummern usw. |
| 500 | Serverfehler | Unerwarteter Fehler beim Verarbeiten der Anforderung |

### <a name="usage-and-billing-data-update-frequency"></a>Aktualisierungshäufigkeit für Nutzungs- und Abrechnungsdaten

Nutzungs- und Abrechnungsdatendateien werden für den aktuellen Abrechnungsmonat alle 24 Stunden aktualisiert. Die Datenlatenz kann jedoch bis zu drei Tage betragen. Beispiel: Bei einer Nutzung am Montag erscheinen die Daten in der Datendatei unter Umständen erst am Donnerstag.

### <a name="azure-service-catalog"></a>Azure-Dienstkatalog

Alle Azure-Dienste werden im CSV-Format in einem Azure Storage-Blob in einem Katalog veröffentlicht. Dieser Katalog ist hilfreich, wenn Sie einen Katalog mit allen Azure-Diensten für Ihr System zusammenstellen müssen. Der aktuelle Katalog befindet sich unter [https://azurecatalog.blob.core.windows.net/catalog/AzureCatalog.csv](https://azurecatalog.blob.core.windows.net/catalog/AzureCatalog.csv).

### <a name="csv-data-file-details"></a>Details zur CSV-Datendatei

Die folgenden Informationen beschreiben die Eigenschaften von API-Berichten.

#### <a name="usage-summary"></a>Nutzungszusammenfassung

Das JSON-Format wird aus dem CSV-Bericht generiert. Somit entspricht das Format dem CSV-Format der Zusammenfassung. Der Spaltenname wird bereitgestellt, daher erfolgt die Deserialisierung beim Versenden der JSON-Zusammenfassungsdaten in eine Datentabelle.

| CSV-Spaltenname | JSON-Spaltenname | Neue JSON-Spalte | Comment |
| --- | --- | --- | --- |
| AccountOwnerId | AccountOwnerLiveId | AccountOwnerLiveId |   |
| Kontoname | AccountName | AccountName |   |
| ServiceAdministratorId | ServiceAdministratorLiveId | ServiceAdministratorLiveId |   |
| SubscriptionId | SubscriptionId | SubscriptionId |   |
| SubscriptionGuid | MOCPSubscriptionGuid | SubscriptionGuid |   |
| Subscription Name | SubscriptionName | SubscriptionName |   |
| Date | Date | Date | Zeigt das Datum an, an dem der Dienstkatalogbericht ausgeführt wurde. Das Format ist eine Datumszeichenfolge ohne Zeitstempel. |
| Month | Month | Month (Monat) |   |
| Tag | Tag | Tag |   |
| Jahr | Jahr | Jahr |   |
| Product | BillableItemName | Product |   |
| Messungs-ID | ResourceGUID | MeterId |   |
| Meter Category | Dienst | MeterCategory | Dient zum Finden von Diensten. Relevant für Dienste mit mehr als einem ServiceType. Beispiel: Virtual Machines. |
| Unterkategorie für Messung | ServiceType | MeterSubCategory | Stellt eine zweite Detailebene für einen Dienst bereit. Beispiel: A1 VM (außer Windows).  |
| Messbereich | ServiceRegion | MeterRegion | Die dritte Detailebene, die für einen Dienst erforderlich ist. Hilfreich, um den Regionskontext von ResourceGUID zu finden. |
| Meter Name | ServiceResource | MeterName | Der Name des Diensts. |
| Verbrauchte Menge | ResourceQtyConsumed | ConsumedQuantity |   |
| ResourceRate | ResourceRate | ResourceRate |   |
| ExtendedCost | ExtendedCost | ExtendedCost |   |
| Ressourcenspeicherort | ServiceSubRegion | ResourceLocation |   |
| Genutzter Dienst | ServiceInfo | ConsumedService |   |
| Instanzen-ID | Komponente | InstanceId |   |
| ServiceInfo1 | ServiceInfo1 | ServiceInfo1 |   |
| ServiceInfo2 | ServiceInfo2 | ServiceInfo2 |   |
| AdditionalInfo | AdditionalInfo | AdditionalInfo |   |
| `Tags` | `Tags` | `Tags` |   |
| Dienstbezeichner speichern   | OrderNumber | StoreServiceIdentifier   |   |
| Department Name | DepartmentName | DepartmentName |   |
| Kostenstelle | CostCenter | CostCenter |   |
| Berechnungseinheit | UnitOfMeasure | UnitOfMeasure | Beispielwerte: Stunden, GB, Ereignisse, Pushvorgänge, Einheiten, Einheit Stunden, MB, tägliche Einheiten |
| ResourceGroup | ResourceGroup | ResourceGroup |   |

#### <a name="azure-marketplace-report"></a>Azure Marketplace-Bericht

| CSV-Spaltenname | JSON-Spaltenname | Neue JSON-Spalte |
| --- | --- | --- |
| AccountOwnerId | AccountOwnerId | AccountOwnerId |
| Kontoname | AccountName | AccountName |
| SubscriptionId | SubscriptionId | SubscriptionId |
| SubscriptionGuid | SubscriptionGuid | SubscriptionGuid |
| Subscription Name | SubscriptionName |  SubscriptionName |
| Date | BillingCycle |  Datum (nur Datumszeichenfolge. Kein Zeitstempel)
| Month | Month |  Month (Monat) |
| Tag | Tag |  Tag |
| Jahr | Jahr |  Jahr |
| Messungs-ID | MeterResourceId |  MeterId |
| Name des Herausgebers | PublisherFriendlyName |  PublisherName |
| Angebotsname | OfferFriendlyName |  OfferName |
| Plan Name | PlanFriendlyName |  PlanName |
| Verbrauchte Menge | BilledQty |  ConsumedQuantity |
| ResourceRate | ResourceRate | ResourceRate |
| ExtendedCost | ExtendedCost | ExtendedCost |
| Berechnungseinheit | UnitOfMeasure | UnitOfMeasure |
| Instanzen-ID | InstanceId | InstanceId |
| Zusätzliche Informationen | AdditionalInfo | AdditionalInfo |
| `Tags` | `Tags` | `Tags` |
| Order Number | OrderNumber | OrderNumber |
| Department Name | DepartmentNames | DepartmentName |
| Kostenstelle | CostCenters |  CostCenter |
| Ressourcengruppe | ResourceGroup |  ResourceGroup |

#### <a name="price-sheet"></a>Preisblatt

| CSV-Spaltenname | JSON-Spaltenname | Comment |
| --- | --- | --- |
| Dienst | Dienst |  Keine Änderung am Preis |
| Berechnungseinheit | UnitOfMeasure |   |
| Overage Part Number | ConsumptionPartNumber |   |
| Overage Unit Price | ConsumptionPrice |   |
| Währungscode | CurrencyCode |     |

### <a name="common-api-issues"></a>Häufige API-Probleme

Wenn Sie Azure Enterprise-REST-APIs verwenden, treten unter Umständen die folgenden allgemeinen Probleme auf:

Möglicherweise versuchen Sie, einen API-Schlüssel zu verwenden, der nicht über den richtigen Autorisierungstyp verfügt. API-Schlüssel werden generiert von:

- Unternehmensadministrator
- Abteilungsadministrator (Department Administrator, DA)
- Kontobesitzer (Account Owner, AO)

Ein Schlüssel, der vom EA-Administrator generiert wird, bietet Zugriff auf alle Informationen für diese Registrierung. Ein Unternehmensadministrator ohne Schreibberechtigungen kann keinen API-Schlüssel generieren.

Ein Schlüssel, der von einem Abteilungsadministrator oder einem Kontobesitzer generiert wird, bietet keinen Zugriff auf Bilanzen, Gebühren und Preisblattinformationen.

Ein API-Schlüssel läuft alle sechs Monate ab. Wenn der Schlüssel abgelaufen ist, müssen Sie ihn neu generieren.

Wenn Sie einen Zeitlimitfehler erhalten, können Sie diesen beheben, indem Sie den Schwellenwert für das Zeitlimit erhöhen.

Unter Umständen erhalten Sie den Ablauffehler 401 (nicht autorisiert). Der Fehler wird in der Regel durch einen abgelaufenen Schlüssel verursacht. Wenn der Schlüssel abgelaufen ist, können Sie ihn erneut generieren.

Unter Umständen werden bei einem API-Aufruf die Fehler 400 und 404 (nicht verfügbar) zurückgegeben, wenn für den ausgewählten Datumsbereich keine aktuellen Daten verfügbar sind. Dieser Fehler kann beispielsweise auftreten, wenn vor Kurzem eine Registrierungsübertragung initiiert wurde. Daten ab einem bestimmten Datum befinden sich nun in einer neuen Registrierung. Andernfalls kann der Fehler auftreten, wenn Sie eine neue Registrierungsnummer zum Abrufen von Informationen verwenden, die in einer alten Registrierung gespeichert sind.

## <a name="next-steps"></a>Nächste Schritte

- Azure EA-Portaladministratoren finden unter [Azure EA-Portalverwaltung](ea-portal-administration.md) Informationen zu allgemeinen Verwaltungsaufgaben.
- Hilfe zu Problemen im Azure EA-Portal finden Sie unter [Beheben von Zugriffsproblemen beim Azure EA-Portal](ea-portal-troubleshoot.md).