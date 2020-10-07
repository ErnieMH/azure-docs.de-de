---
title: Erstellen und Bearbeiten von Azure Policy-Sicherheitsrichtlinien über die REST-API
description: Erfahren Sie mehr über die Azure Policy-Richtlinienverwaltung über eine REST-API.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 6fcfed585aece4fd57c085e0c9f6fdcd6cbeae74
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2020
ms.locfileid: "91448360"
---
# <a name="configure-a-security-policy-in-azure-policy-using-the-rest-api"></a>Konfigurieren einer Sicherheitsrichtlinie in Azure Policy über die REST-API

Als Teil der nativen Integration in Azure Policy ermöglicht Ihnen Azure Security Center die Nutzung der REST-API von Azure Policy, um Richtlinienzuweisungen zu erstellen. Die folgenden Anweisungen führen Sie durch das Erstellen von Richtlinienzuweisungen sowie durch das Anpassen vorhandener Zuweisungen. 

Wichtige Konzepte in Azure Policy: 

- Eine **Richtliniendefinition** ist eine Regel. 

- Eine **Initiative** ist eine Sammlung von Richtliniendefinitionen (Regeln). 

- Eine **Zuweisung** ist eine Anwendung einer Initiative oder Richtlinie für einen bestimmten Bereich (Verwaltungsgruppe, Abonnement usw.). 

Security Center hat eine integrierte Initiative, die alle seiner Sicherheitsrichtlinien enthält. Um die Richtlinien von Security Center für Ihre Azure-Ressourcen zu bewerten, sollten Sie eine Zuweisung zur Verwaltungsgruppe oder zum Abonnement erstellen, die oder das Sie bewerten möchten.

In der integrierten Initiative sind alle Richtlinien von Security Center standardmäßig aktiviert. Sie können bestimmte Richtlinien aus der integrierten Initiative deaktivieren. Wenn Sie z.B. alle Security Center-Richtlinien außer der **Webanwendungsfirewall** anwenden möchten, ändern Sie den Wert des effect-Parameters der Richtlinie in **Deaktiviert**. 

## <a name="api-examples"></a>API-Beispiele

Ersetzen Sie diese Variablen in den folgenden Beispielen:

- Geben Sie für **{scope}**  den Namen der Verwaltungsgruppe oder des Abonnements ein, auf die oder das Sie die Richtlinie anwenden.
- Geben Sie für **{policyAssignmentName}**  den [Namen der relevanten Richtlinienzuweisung ](#policy-names) ein.
- Geben Sie für **{name}**  Ihren Namen oder den Namen des Administrators ein, der die Richtlinienänderung genehmigt hat.

In diesem Beispiel wird gezeigt, wie Sie die integrierte Security Center-Initiative einem Abonnement oder einer Verwaltungsgruppe zuweisen:
 
 ```
    PUT  
    https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 

    Request Body (JSON) 

    { 

      "properties":{ 

    "displayName":"Enable Monitoring in Azure Security Center", 

    "metadata":{ 

    "assignedBy":"{Name}" 

    }, 

    "policyDefinitionId":"/providers/Microsoft.Authorization/policySetDefinitions/1f3afdf9-d0c9-4c3d-847f-89da613e70a8", 

    "parameters":{}, 

    } 

    } 
 ```

In diesem Beispiel wird gezeigt, wie Sie die integrierte Security Center-Initiative einem Abonnement zuweisen, wobei die folgenden Richtlinien deaktiviert sind: 

- Systemupdates („systemUpdatesMonitoringEffect“) 

- Sicherheitskonfigurationen („systemConfigurationsMonitoringEffect“) 

- Endpunktschutz (Endpoint Protection) („endpointProtectionMonitoringEffect“) 

 ```
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 
    
    Request Body (JSON) 
    
    { 
    
      "properties":{ 
    
    "displayName":"Enable Monitoring in Azure Security Center", 
    
    "metadata":{ 
    
    "assignedBy":"{Name}" 
    
    }, 
    
    "policyDefinitionId":"/providers/Microsoft.Authorization/policySetDefinitions/1f3afdf9-d0c9-4c3d-847f-89da613e70a8", 
    
    "parameters":{ 
    
    "systemUpdatesMonitoringEffect":{"value":"Disabled"}, 
    
    "systemConfigurationsMonitoringEffect":{"value":"Disabled"}, 
    
    "endpointProtectionMonitoringEffect":{"value":"Disabled"}, 
    
    }, 
    
     } 
    
    } 
 ```
In diesem Beispiel wird gezeigt, wie Sie eine Zuweisung entfernt wird:
 ```
    DELETE   
    https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 
 ```

## <a name="policy-names-reference"></a>Referenz der Richtliniennamen <a name="policy-names"></a>

|Richtlinienname in Security Center|Richtlinienname, der in Azure Policy angezeigt wird |Name des Effect-Parameters der Richtlinie|
|----|----|----|
|SQL-Verschlüsselung |Nicht verschlüsselte SQL-Datenbank in Azure Security Center überwachen |sqlEncryptionMonitoringEffect| 
|SQL-Überwachung |Nicht überwachte SQL-Datenbank in Azure Security Center überwachen |sqlAuditingMonitoringEffect|
|Systemupdates |Fehlende Systemupdates in Azure Security Center überwachen |systemUpdatesMonitoringEffect|
|Speicherverschlüsselung |Fehlende Blobverschlüsselung für Speicherkonten überwachen |storageEncryptionMonitoringEffect|
|JIT-Netzwerkzugriff |Möglichen Just-In-Time-Netzwerkzugriff in Azure Security Center überwachen |jitNetworkAccessMonitoringEffect |
|Adaptive Anwendungssteuerungen |Mögliche App-Positivlisten in Azure Security Center überwachen |adaptiveApplicationControlsMonitoringEffect|
|Netzwerksicherheitsgruppen |Zu wenig einschränkenden Netzwerkzugriff in Azure Security Center überwachen |networkSecurityGroupsMonitoringEffect| 
|Sicherheitskonfigurationen |Betriebssystem-Sicherheitsrisiken in Azure Security Center überwachen |systemConfigurationsMonitoringEffect| 
|Endpoint Protection |Fehlenden Endpoint Protection-Schutz in Azure Security Center überwachen |endpointProtectionMonitoringEffect |
|Datenträgerverschlüsselung |Nicht verschlüsselte VM-Datenträger in Azure Security Center überwachen |diskEncryptionMonitoringEffect|
|Sicherheitsrisikobewertung |VM-Sicherheitsrisiken in Azure Security Center überwachen |vulnerabilityAssessmentMonitoringEffect|
|Web Application Firewall |Nicht geschützte Webanwendung in Azure Security Center überwachen |webApplicationFirewallMonitoringEffect |
|Firewall der nächsten Generation |Nicht geschützte Netzwerkendpunkte in Azure Security Center überwachen| |


## <a name="next-steps"></a>Nächste Schritte

Weitere verwandte Informationen finden Sie in den folgenden Artikeln: 

- [Benutzerdefinierte Sicherheitsrichtlinien](custom-security-policies.md)
- [Übersicht über Sicherheitsrichtlinien](tutorial-security-policy.md)