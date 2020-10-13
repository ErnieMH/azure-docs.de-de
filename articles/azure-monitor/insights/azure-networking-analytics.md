---
title: Azure Networking Analytics-Lösung in Azure Monitor | Microsoft-Dokumentation
description: Mit der Azure Networking Analytics-Lösung in Azure Monitor können Sie Protokolle für Azure-Netzwerksicherheitsgruppen und Azure Application Gateway-Protokolle überprüfen.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/21/2018
ms.openlocfilehash: 4dc5b84ff127aef173deecfd2be705004d92ee0c
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2020
ms.locfileid: "91449924"
---
# <a name="azure-networking-monitoring-solutions-in-azure-monitor"></a>Azure-Netzwerküberwachungslösungen in Azure Monitor

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Monitor umfasst die folgenden Lösungen zur Überwachung Ihrer Netzwerke:
* Netzwerkleistungsmonitor (NPM) zum
    * Überwachen der Integrität Ihres Netzwerks
* Azure Application Gateway-Analyse zur Überprüfung
    * Azure Application Gateway-Protokolle
    * Azure Application Gateway-Metriken
* Lösungen zum Überwachen und Überprüfen der Netzwerkaktivität in Ihrem Cloudnetzwerk
    * [Traffic Analytics](../../networking/network-monitoring-overview.md#traffic-analytics) 
    * Azure-Netzwerksicherheitsgruppen-Analyse

## <a name="network-performance-monitor-npm"></a>Netzwerkleistungsmonitor (NPM)

Die [Netzwerkleistungsmonitor](../../networking/network-monitoring-overview.md)-Verwaltungslösung ist eine Lösung zur Netzwerküberwachung, die die Integrität, Verfügbarkeit und Erreichbarkeit von Netzwerken überwacht.  Sie dient zur Überwachung der Konnektivität zwischen:

* Public Cloud und lokaler Umgebung
* Rechenzentren und Benutzerstandorten (Zweigstellen)
* Subnetzen, die verschiedene Ebenen einer Multi-Tier-Anwendung hosten.

Weitere Informationen finden Sie unter [Netzwerkleistungsmonitor](../../networking/network-monitoring-overview.md).

## <a name="azure-application-gateway-and-network-security-group-analytics"></a>Azure Application Gateway- und Netzwerksicherheitsgruppen-Analysen
Verwendung der Lösungen:
1. Fügen Sie die Verwaltungslösung in Azure Monitor hinzu, und
2. Aktivieren Sie die Diagnose, um die Diagnose an einen Log Analytics-Arbeitsbereich in Azure Monitor weiterzuleiten. Die Protokolle müssen nicht in Azure Blob Storage geschrieben werden.

Sie können die Diagnose und die entsprechende Lösung für Application Gateway und/oder Netzwerksicherheitsgruppen aktivieren.

Wenn Sie die Ressourcenprotokollierung für einen bestimmten Ressourcentyp nicht aktivieren, aber die Lösung installieren, sind die Dashboardblätter für diese Ressource leer und enthalten eine Fehlermeldung.

> [!NOTE]
> Im Januar 2017 wurde die unterstützte Vorgehensweise zum Senden von Protokollen von Application Gateways und Netzwerksicherheitsgruppen an einen Log Analytics-Arbeitsbereich geändert. Wenn Sie die **Azure Networking Analytics**-Lösung (veraltet) sehen, finden Sie unter [Migrieren von der alten Networking Analytics-Lösung](#migrating-from-the-old-networking-analytics-solution) Schritte, die Sie befolgen müssen.
>
>

## <a name="review-azure-networking-data-collection-details"></a>Überprüfen der Details zur Datensammlung für Azure Networking
Die Azure Application Gateway-Analysen und die Analyseverwaltungslösungen für Netzwerksicherheitsgruppen sammeln Diagnoseprotokolle direkt aus Azure Application Gateways und Netzwerksicherheitsgruppen. Die Protokolle müssen nicht in Azure Blob Storage geschrieben werden, und es wird kein Agent für die Datensammlung benötigt.

Die folgende Tabelle zeigt Datenerfassungsmethoden und andere Details zur Art der Erfassung von Daten für Azure Application Gateway- und Netzwerksicherheitsgruppen-Analysen.

| Plattform | Direkt-Agent | System Center Operations Manager-Agent | Azure | Operations Manager erforderlich? | Daten vom Operations Manager-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |  |  |&#8226; |  |  |Bei der Anmeldung |


## <a name="azure-application-gateway-analytics-solution-in-azure-monitor"></a>Azure Application Gateway-Analyselösung in Azure Monitor

![Symbol für Azure Application Gateway-Analyse](media/azure-networking-analytics/azure-analytics-symbol.png)

Die folgenden Protokolle werden für Anwendungsgateways unterstützt:

* ApplicationGatewayAccessLog
* ApplicationGatewayPerformanceLog
* ApplicationGatewayFirewallLog

Für Anwendungsgateways werden folgende Metriken unterstützt:


* Durchsatz innerhalb von fünf Minuten

### <a name="install-and-configure-the-solution"></a>Installieren und Konfigurieren der Lösung
Gehen Sie folgendermaßen vor, um die Azure Application Gateway-Analyselösung zu installieren und zu konfigurieren:

1. Aktivieren Sie die Azure Application Gateway-Analyselösung in [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureAppGatewayAnalyticsOMS?tab=Overview) oder anhand des unter [Hinzufügen von Azure Monitor-Lösungen aus dem Lösungskatalog](./solutions.md) beschriebenen Prozesses.
2. Aktivieren Sie die Diagnoseprotokollierung für die [Application Gateways](../../application-gateway/application-gateway-diagnostics.md), die Sie überwachen möchten.

#### <a name="enable-azure-application-gateway-diagnostics-in-the-portal"></a>Aktivieren der Azure Application Gateway-Diagnose im Portal

1. Navigieren Sie im Azure-Portal zur Application Gateway-Ressource, die überwacht werden soll.
2. Wählen Sie *Diagnoseprotokolle* aus, um die folgende Seite zu öffnen.

   ![Screenshot: Seite „Diagnoseprotokolle“ für eine Application Gateway-Ressource mit der Option zum Aktivieren der Diagnose](media/azure-networking-analytics/log-analytics-appgateway-enable-diagnostics01.png)
3. Klicken Sie auf *Diagnose aktivieren*, um die folgende Seite zu öffnen.

   ![Screenshot: Seite zum Konfigurieren der Diagnoseeinstellungen. Die Option zum Senden an Log Analytics sowie drei Protokolltypen und eine Metrik sind ausgewählt.](media/azure-networking-analytics/log-analytics-appgateway-enable-diagnostics02.png)
4. Um die Diagnose zu aktivieren, klicken Sie unter *Status* auf *Ein*.
5. Klicken Sie auf das Kontrollkästchen *An Log Analytics senden*.
6. Wählen Sie einen vorhandenen Log Analytics-Arbeitsbereich aus, oder erstellen Sie einen Arbeitsbereich.
7. Klicken Sie für jeden der Protokolltypen, die erfasst werden sollen, auf das Kontrollkästchen unter **Protokoll**.
8. Klicken Sie auf *Speichern*, um die Protokollierung der Diagnose in Azure Monitor zu aktivieren.

#### <a name="enable-azure-network-diagnostics-using-powershell"></a>Aktivieren der Azure-Netzwerkdiagnose mit PowerShell

Das folgende PowerShell-Skript zeigt ein Beispiel für die Aktivierung der Ressourcenprotokollierung für Application Gateways.

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$gateway = Get-AzApplicationGateway -Name 'ContosoGateway'

Set-AzDiagnosticSetting -ResourceId $gateway.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-application-gateway-analytics"></a>Verwenden der Azure Application Gateway-Analyse
![Abbildung der Azure Application Gateway-Analyse](media/azure-networking-analytics/log-analytics-appgateway-tile.png)

Nach dem Klicken auf die Kachel **Azure Application Gateway analytics** (Azure Application Gateway-Analyse) in der Übersicht können Sie Zusammenfassungen der Protokolle anzeigen und dann die Details für die folgenden Kategorien anzeigen:

* Application Gateway-Zugriffsprotokolle
  * Client- und Serverfehler für Application Gateway-Zugriffsprotokolle
  * Anforderungen pro Stunde für jedes Anwendungsgateway
  * Anforderungen mit Fehlern pro Stunde für jedes Anwendungsgateway
  * Fehler nach Benutzer-Agent für Anwendungsgateways
* Application Gateway-Leistung
  * Integrität von Hosts für Application Gateway
  * Maximum und 95. Perzentil für Application Gateway-Anforderungen mit einem Fehler

![Screenshot: Dashboard für Application Gateway-Zugriffsprotokolle, das Kacheln mit Daten für Gatewayfehler, für Anforderungen und für Anforderungen mit Fehlern zeigt](media/azure-networking-analytics/log-analytics-appgateway01.png)

![Screenshot: Dashboard für Application Gateway-Zugriffsprotokolle, das Kacheln mit Daten für Fehler nach Benutzer-Agent, für die Hostintegrität und für Anforderungen mit Fehlern zeigt](media/azure-networking-analytics/log-analytics-appgateway02.png)

Prüfen Sie auf dem **Azure Application Gateway**-Analysedashboard die Zusammenfassungsinformationen auf einem der Blätter, und klicken Sie dann auf einen Eintrag, um ausführliche Informationen auf der Seite „Protokollsuche“ anzuzeigen.

Sie können auf jeder Seite für die Protokollsuche die Ergebnisse nach Zeit, detaillierte Ergebnisse und Ihren Protokollsuchverlauf anzeigen. Außerdem können Sie nach Facets filtern, um die Ergebnisse einzugrenzen.


## <a name="azure-network-security-group-analytics-solution-in-azure-monitor"></a>Analyselösung der Azure-Netzwerksicherheitsgruppe in Azure Monitor

![Symbol für Azure-Netzwerksicherheitsgruppen-Analyse](media/azure-networking-analytics/azure-analytics-symbol.png)

> [!NOTE]
> Die Netzwerksicherheitsgruppen-Analyselösung wird in den Community-Support verlagert, da ihre Funktionalität durch [Traffic Analytics](../../network-watcher/traffic-analytics.md) ersetzt wurde.
> - Die Lösung ist jetzt in den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/oms-azurensg-solution/) verfügbar und schon bald nicht mehr über den Azure Marketplace zu beziehen.
> - Für Bestandskunden, die die Lösung bereits ihrem Arbeitsbereich hinzugefügt haben, bleibt die Funktion ohne Änderungen erhalten.
> - Microsoft unterstützt das Senden von NSG-Ressourcenprotokollen an Ihren Arbeitsbereich mithilfe von Diagnostics Settings auch weiterhin.

Die folgenden Protokolle werden für Netzwerksicherheitsgruppen unterstützt:

* NetworkSecurityGroupEvent
* NetworkSecurityGroupRuleCounter

### <a name="install-and-configure-the-solution"></a>Installieren und Konfigurieren der Lösung
Gehen Sie folgendermaßen vor, um die Azure Networking Analytics-Lösung zu installieren und zu konfigurieren:

1. Aktivieren Sie die Azure-Analyselösung für Netzwerksicherheitsgruppen mithilfe des unter [Hinzufügen von Azure Monitor-Lösungen aus dem Lösungskatalog](./solutions.md) beschriebenen Verfahrens.
2. Aktivieren Sie die Diagnoseprotokollierung für die [Netzwerksicherheitsgruppen](../../virtual-network/virtual-network-nsg-manage-log.md)-Ressourcen, die Sie überwachen möchten.

### <a name="enable-azure-network-security-group-diagnostics-in-the-portal"></a>Aktivieren der Diagnose für die Azure-Netzwerksicherheitsgruppe im Portal

1. Navigieren Sie im Azure-Portal zur Netzwerksicherheitsgruppen-Ressource, die überwacht werden soll
2. Wählen Sie *Diagnoseprotokolle* aus, um die folgende Seite zu öffnen.

   ![Screenshot: Seite „Diagnoseprotokolle“ für eine Netzwerksicherheitsgruppen-Ressource mit der Option zum Aktivieren der Diagnose](media/azure-networking-analytics/log-analytics-nsg-enable-diagnostics01.png)
3. Klicken Sie auf *Diagnose aktivieren*, um die folgende Seite zu öffnen.

   ![Screenshot: Seite zum Konfigurieren der Diagnoseeinstellungen. Der Status ist auf „Ein“ festgelegt, „An Log Analytics senden“ ist ausgewählt, und zwei Protokolltypen sind ausgewählt.](media/azure-networking-analytics/log-analytics-nsg-enable-diagnostics02.png)
4. Um die Diagnose zu aktivieren, klicken Sie unter *Status* auf *Ein*.
5. Klicken Sie auf das Kontrollkästchen *An Log Analytics senden*.
6. Wählen Sie einen vorhandenen Log Analytics-Arbeitsbereich, oder erstellen Sie einen Arbeitsbereich.
7. Klicken Sie für jeden der Protokolltypen, die erfasst werden sollen, auf das Kontrollkästchen unter **Protokoll**.
8. Klicken Sie auf *Speichern*, um die Protokollierung der Diagnose in Log Analytics zu aktivieren.

### <a name="enable-azure-network-diagnostics-using-powershell"></a>Aktivieren der Azure-Netzwerkdiagnose mit PowerShell

Das folgende PowerShell-Skript zeigt ein Beispiel für die Aktivierung der Ressourcenprotokollierung für die Netzwerksicherheitsgruppen:
```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$nsg = Get-AzNetworkSecurityGroup -Name 'ContosoNSG'

Set-AzDiagnosticSetting -ResourceId $nsg.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="use-azure-network-security-group-analytics"></a>Verwenden von Azure-Netzwerksicherheitsgruppen-Analysen
Nach dem Klicken auf die Kachel **Azure Network Security Group analytics** (Azure-Netzwerksicherheitsgruppen-Analyse) in der Übersicht können Sie Zusammenfassungen der Protokolle anzeigen und dann die Details für die folgenden Kategorien anzeigen:

* Blockierte Flows der Netzwerksicherheitsgruppe
  * Regeln der Netzwerksicherheitsgruppe mit blockierten Flows
  * MAC-Adressen mit blockierten Flows
* Zugelassene Flows der Netzwerksicherheitsgruppe
  * Regeln der Netzwerksicherheitsgruppe mit zugelassenen Flows
  * MAC-Adressen mit zugelassenen Flows

![Screenshot: Kacheln mit Daten für blockierte Flows der Netzwerksicherheitsgruppe, einschließlich Regeln mit blockierten Flows und MAC-Adressen mit blockierten Flows](media/azure-networking-analytics/log-analytics-nsg01.png)

![Screenshot: Kacheln mit Daten für zugelassene Flows der Netzwerksicherheitsgruppe, einschließlich Regeln mit zugelassenen Flows und MAC-Adressen mit zugelassenen Flows](media/azure-networking-analytics/log-analytics-nsg02.png)

Prüfen Sie im **Azure Network Security Group analytics** (Azure-Netzwerksicherheitsgruppen-Analyse)-Dashboard die Zusammenfassungsinformationen auf einem der Blätter, und klicken Sie dann auf einen Eintrag, um ausführliche Informationen auf der Seite „Protokollsuche“ anzuzeigen.

Sie können auf jeder Seite für die Protokollsuche die Ergebnisse nach Zeit, detaillierte Ergebnisse und Ihren Protokollsuchverlauf anzeigen. Außerdem können Sie nach Facets filtern, um die Ergebnisse einzugrenzen.

## <a name="migrating-from-the-old-networking-analytics-solution"></a>Migrieren von der alten Networking Analytics-Lösung
Im Januar 2017 wurde die unterstützte Vorgehensweise zum Senden von Protokollen von Azure Application Gateways und Azure-Netzwerksicherheitsgruppen an einen Log Analytics-Arbeitsbereich geändert. Diese Änderungen bieten folgende Vorteile:
+ Protokolle werden direkt in Azure Monitor geschrieben, sodass kein Speicherkonto mehr notwendig ist
+ Geringere Latenz ab dem Zeitpunkt, zu dem Protokolle generiert werden, bis zu ihrer Verfügbarkeit in Azure Monitor
+ Weniger Konfigurationsschritte
+ Ein allgemeines Format für alle Typen der Azure-Diagnose

So verwenden Sie die aktualisierte Lösung:

1. [Konfigurieren der direkt an Azure Monitor von Azure Application Gateways gesendeten Diagnosedaten](#enable-azure-application-gateway-diagnostics-in-the-portal)
2. [Konfigurieren der direkt an Azure Monitor von Azure-Netzwerksicherheitsgruppen gesendeten Diagnosedaten](#enable-azure-network-security-group-diagnostics-in-the-portal)
2. Aktivieren Sie die *Azure Application Gateway*- und die *Azure-Netzwerksicherheitsgruppen*-Analyselösung mithilfe des unter [Hinzufügen von Azure Monitor -Lösungen aus dem Lösungskatalog](solutions.md) beschriebenen Prozesses.
3. Aktualisieren Sie alle gespeicherten Abfragen, Dashboards oder Warnungen zur Verwendung des neuen Datentyps.
   + Der Typ lautet AzureDiagnostics. Sie können „ResourceType“ verwenden, um nach Azure-Netzwerkprotokollen zu filtern.

     | Verwenden Sie anstelle von | Verwendung: |
     | --- | --- |
     | NetworkApplicationgateways &#124; dabei gilt: OperationName=="ApplicationGatewayAccess" | AzureDiagnostics &#124; dabei gilt: ResourceType=="APPLICATIONGATEWAYS" und OperationName=="ApplicationGatewayAccess" |
     | NetworkApplicationgateways &#124; dabei gilt: OperationName=="ApplicationGatewayPerformance" | AzureDiagnostics &#124; dabei gilt: ResourceType=="APPLICATIONGATEWAYS" und OperationName=="ApplicationGatewayPerformance" |
     | NetworkSecuritygroups | AzureDiagnostics &#124; dabei gilt: ResourceType=="NETWORKSECURITYGROUPS" |

   + Ändern Sie für jedes Feld, dessen Name das Suffix \_s, \_d oder \_g enthält, das erste Zeichen in einen Kleinbuchstaben.
   + Für jedes Feld, dessen Name das Suffix \_o enthält, werden die Daten basierend auf den geschachtelten Feldnamen in einzelne Felder aufgeteilt.
4. Entfernen Sie die *Azure Networking Analytics*-Lösung (veraltet).
   + Wenn Sie PowerShell einsetzen, verwenden Sie `Set-AzureOperationalInsightsIntelligencePack -ResourceGroupName <resource group that the workspace is in> -WorkspaceName <name of the log analytics workspace> -IntelligencePackName "AzureNetwork" -Enabled $false`.

Daten, die vor der Änderung gesammelt wurden, werden in der neuen Lösung nicht angezeigt. Sie können die Abfrage dieser Daten mithilfe des alten Typs und der alten Feldnamen fortsetzen.

## <a name="troubleshooting"></a>Problembehandlung
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie [Protokollabfragen in Azure Monitor](../log-query/log-query-overview.md), um detaillierte Azure-Diagnosedaten anzuzeigen.

