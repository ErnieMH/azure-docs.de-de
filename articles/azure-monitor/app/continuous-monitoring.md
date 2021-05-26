---
title: Kontinuierliches Überwachen der DevOps-Releasepipeline mit Azure Pipelines und Azure Application Insights | Microsoft-Dokumentation
description: Dieser Artikel enthält Anweisungen zum schnellen Einrichten der kontinuierlichen Überwachung mit Application Insights.
ms.topic: conceptual
ms.date: 05/01/2020
ms.openlocfilehash: faa2dc0918756587ccf9093e8c9cc5d4379c6883
ms.sourcegitcommit: 42ac9d148cc3e9a1c0d771bc5eea632d8c70b92a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/13/2021
ms.locfileid: "109846786"
---
# <a name="add-continuous-monitoring-to-your-release-pipeline"></a>Hinzufügen der kontinuierlichen Überwachung zur Releasepipeline

Azure Pipelines kann in Azure Application Insights integriert werden, um die kontinuierliche Überwachung Ihrer DevOps-Releasepipeline während des gesamten Lebenszyklus der Softwareentwicklung zu ermöglichen. 

Bei der kontinuierlichen Überwachung können Releasepipelines Überwachungsdaten von Application Insights und anderen Azure-Ressourcen integrieren. Wenn die Releasepipeline eine Application Insights-Warnung erkennt, kann sie die Bereitstellung steuern oder einen Rollback dafür ausführen, bis die Warnung gelöst wurde. Wenn alle Überprüfungen erfolgreich durchgeführt wurden, können Bereitstellungen vom Testen bis hin zur Produktion ohne manuellen Eingriff automatisch fortgesetzt werden. 

## <a name="configure-continuous-monitoring"></a>Konfigurieren der kontinuierlichen Überwachung

1. Wählen Sie in [Azure DevOps](https://dev.azure.com) eine Organisation und ein Projekt aus.
   
1. Wählen Sie auf der Projektseite im Menü auf der linken Seite die Option **Pipelines** > **Releases**. 
   
1. Klicken Sie auf den Pfeil neben **Neu**, und wählen Sie **Neue Releasepipeline**. Falls Sie noch nicht über eine Pipeline verfügen, wählen Sie auf der angezeigten Seite die Option **Neue Pipeline**.
   
1. Suchen Sie im Bereich **Wählen Sie eine Vorlage aus** nach **Azure App Service-Bereitstellung mit fortlaufender Überwachung**. Wählen Sie diese Option und anschließend **Übernehmen**. 

   ![Neue Azure Pipelines-Releasepipeline](media/continuous-monitoring/001.png)

1. Wählen Sie im Feld **Stufe 1** den Hyperlink für die Option **Stufenaufgaben anzeigen**.

   ![Stufenaufgaben anzeigen](media/continuous-monitoring/002.png)

1. Füllen Sie im Konfigurationsbereich **Stufe 1** die folgenden Felder aus: 

    | Parameter        | Wert |
   | ------------- |:-----|
   | **Name der Stufe**      | Geben Sie einen Stufennamen an, oder übernehmen Sie **Stufe 1**. |
   | **Azure-Abonnement** | Öffnen Sie die Dropdownliste, und wählen Sie den Link zum gewünschten Azure-Abonnement aus.|
   | **App-Typ** | Öffnen Sie die Dropdownliste, und wählen Sie Ihren App-Typ aus. |
   | **App Service-Name** | Geben Sie den Namen Ihrer Azure App Service-Instanz ein. |
   | **Name der Ressourcengruppe für Application Insights**    | Öffnen Sie die Dropdownliste, und wählen Sie die gewünschte Ressourcengruppe aus. |
   | **Application Insights-Ressourcenname** | Öffnen Sie die Dropdownliste, und wählen Sie die Application Insights-Ressource für die zuvor gewählte Ressourcengruppe aus.

1. Wählen Sie zum Speichern der Pipeline mit den Standardeinstellungen für Warnungsregeln oben rechts im Azure DevOps-Fenster die Option **Speichern**. Geben Sie einen beschreibenden Kommentar ein, und wählen Sie anschließend **OK**.

## <a name="modify-alert-rules"></a>Ändern von Warnungsregeln

Die Vorlage **Azure App Service-Bereitstellung mit kontinuierlicher Überwachung** verfügt standardmäßig über vier Warnungsregeln: **Verfügbarkeit**, **Anforderungsfehler**, **Serverantwortzeit** und **Serverausnahmen**. Sie können weitere Regeln hinzufügen oder die Regeleinstellungen ändern, um Ihre Servicelevelanforderungen zu erfüllen. 

Ändern Sie die Einstellungen für Warnungsregeln wie folgt:

Wählen Sie auf der Seite der Releasepipeline im linken Bereich die Option **Configure Application Insights Alerts** (Application Insights-Warnungen konfigurieren).

Die vier Standardwarnungsregeln werden über ein Inlineskript erstellt:

```azurecli
$subscription = az account show --query "id";$subscription.Trim("`"");$resource="/subscriptions/$subscription/resourcegroups/"+"$(Parameters.AppInsightsResourceGroupName)"+"/providers/microsoft.insights/components/" + "$(Parameters.ApplicationInsightsResourceName)";
az monitor metrics alert create -n 'Availability_$(Release.DefinitionName)' -g $(Parameters.AppInsightsResourceGroupName) --scopes $resource --condition 'avg availabilityResults/availabilityPercentage < 99' --description "created from Azure DevOps";
az monitor metrics alert create -n 'FailedRequests_$(Release.DefinitionName)' -g $(Parameters.AppInsightsResourceGroupName) --scopes $resource --condition 'count requests/failed > 5' --description "created from Azure DevOps";
az monitor metrics alert create -n 'ServerResponseTime_$(Release.DefinitionName)' -g $(Parameters.AppInsightsResourceGroupName) --scopes $resource --condition 'avg requests/duration > 5' --description "created from Azure DevOps";
az monitor metrics alert create -n 'ServerExceptions_$(Release.DefinitionName)' -g $(Parameters.AppInsightsResourceGroupName) --scopes $resource --condition 'count exceptions/server > 5' --description "created from Azure DevOps";
```

Sie können das Skript ändern und zusätzliche Warnungsregeln hinzufügen, die Warnungsbedingungen ändern oder Warnungsregeln entfernen, die für Ihre Bereitstellungszwecke nicht sinnvoll sind.

## <a name="add-deployment-conditions"></a>Hinzufügen von Bereitstellungsbedingungen

Beim Hinzufügen von Bereitstellungsgates zu Ihrer Releasepipeline verhindert eine Warnung, die die von Ihnen festgelegten Schwellenwerte überschreitet, die unerwünschte Releasehöherstufung. Nach Auflösung der Warnung kann die Bereitstellung automatisch fortgesetzt werden.

Fügen Sie Bereitstellungsgates wie folgt hinzu:

1. Wählen Sie auf der Hauptseite der Pipeline unter **Stufen** das Symbol **Bedingungen vor der Bereitstellung** oder **Bedingungen nach der Bereitstellung**. Dies hängt davon ab, welche Stufe ein Gate mit kontinuierlicher Überwachung benötigt.
   
   ![Bedingungen vor der Bereitstellung](media/continuous-monitoring/004.png)
   
1. Legen Sie im Konfigurationsbereich **Bedingungen vor der Bereitstellung** die Option **Gates** auf **Aktiviert** fest.
   
1. Wählen Sie neben **Bereitstellungsgates** die Option **Hinzufügen**.
   
1. Wählen Sie im Dropdownmenü die Option **Azure Monitor-Warnungen abfragen**. Mit dieser Option können Sie sowohl auf Azure Monitor- als auch auf Application Insights-Warnungen zugreifen.
   
   ![Abfragen von Azure Monitor-Warnungen](media/continuous-monitoring/005.png)
   
1. Geben Sie unter **Auswertungsoptionen** die gewünschten Werte für Einstellungen wie **Der Zeitraum bis zur erneuten Auswertung von Gates** und **Das Zeitlimit, nach dem Gates als fehlerhaft gelten** ein. 

## <a name="view-release-logs"></a>Anzeigen von Releaseprotokollen

Sie können das Verhalten von Bereitstellungsgates und andere Releaseschritte den Releaseprotokollen entnehmen. Öffnen Sie die Protokolle wie folgt:

1. Wählen Sie auf der Pipelineseite im Menü links die Option **Releases**. 
   
1. Wählen Sie ein beliebiges Release aus. 
   
1. Wählen Sie unter **Stufen** eine beliebige Stufe aus, um eine Zusammenfassung zum Release anzuzeigen. 
   
1. Wählen Sie in der Releasezusammenfassung zum Anzeigen von Protokollen die Option **Protokolle anzeigen**, wählen Sie für eine beliebige Stufe den Hyperlink **Erfolgreich** oder **Fehler**, oder zeigen Sie auf eine Stufe, und wählen Sie **Protokolle**. 
   
   ![Anzeigen von Releaseprotokollen](media/continuous-monitoring/006.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Pipelines finden Sie in der [Azure Pipelines-Dokumentation](/azure/devops/pipelines).
