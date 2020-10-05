---
title: Übersicht über Service Health | Microsoft-Dokumentation
description: Erfahren Sie, wie Service Health Ihnen ein anpassbares Dashboard bietet, das die Integrität Ihrer Azure-Dienste in den Regionen nachverfolgt, in denen Sie sie verwenden.
ms.topic: conceptual
ms.date: 05/10/2019
ms.openlocfilehash: 8246b0ab93b95c13858e4ff96d0f24b255d05e55
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90967789"
---
# <a name="service-health-overview"></a>Service Health-Übersicht

Service Health bietet Ihnen ein anpassbares Dashboard, das die Integrität Ihrer Azure-Dienste in den Regionen nachverfolgt, wo Sie sie verwenden. In diesem Dashboard können Sie aktive Ereignisse wie laufende Dienstprobleme, bevorstehende geplante Wartung oder relevante Integritätsempfehlungen nachverfolgen. Wenn Ereignisse inaktiv werden, werden sie bis zu 90 Tage lang in Ihrem Integritätsverlauf festgehalten. Schließlich können Sie mit dem Service Health-Dashboard Service Health-Warnungen erstellen und verwalten, die Sie proaktiv benachrichtigen, wenn Sie von Dienstproblemen betroffen sind.

## <a name="service-health-events"></a>Service Health-Ereignisse

Service Health verfolgt vier Typen von Integritätsereignissen nach, die sich auf Ihre Ressourcen auswirken könnten:

1. **Dienstprobleme**: Probleme in den Azure-Diensten, die Sie momentan betreffen. 
2. **Geplante Wartung**: Anstehende Wartung, die sich in der Zukunft auf die Verfügbarkeit Ihrer Dienste auswirken kann.  
3. **Integritätsempfehlungen**: Änderungen in Azure-Diensten, die Ihre Aufmerksamkeit erfordern. Beispiele hierfür sind das Einstellen von Azure-Features oder Upgradeanforderungen (z. B. ein Upgrade auf ein unterstütztes PHP-Framework).
4. **Sicherheitsempfehlungen**: Sicherheitsbezogene Benachrichtigungen oder Verstöße, die sich auf die Verfügbarkeit Ihrer Azure-Dienste auswirken können.

> [!NOTE]
> Um Service Health-Ereignisse anzuzeigen, [müssen Benutzer über die Rolle „Leser“](../role-based-access-control/role-assignments-portal.md) für ein Abonnement verfügen.

## <a name="get-started-with-service-health"></a>Erste Schritte mit Service Health

Um Ihr Service Health-Dashboard zu starten, wählen Sie auf dem Dashboard Ihres Portals die Kachel „Service Health“ aus. Wenn Sie die Kachel zuvor entfernt haben oder ein benutzerdefiniertes Dashboard verwenden, suchen Sie den Service Health-Dienst unter „Weitere Dienste“ (unten links auf Ihrem Dashboard).

![Erste Schritte mit Service Health](./media/service-health-overview/azure-service-health-overview-1.png)

## <a name="see-current-issues-which-impact-your-services"></a>Anzeigen aktueller Probleme, die sich auf Ihre Dienste auswirken

Die Ansicht **Dienstprobleme** zeigt laufende Probleme in Azure-Diensten an, die sich auf Ihre Ressourcen auswirken. Sie können nachvollziehen, wann das Problem aufgetreten ist und welche Dienste und Regionen betroffen sind. Sie können auch das aktuelle Update lesen, um zu verstehen, was Azure unternimmt, um das Problem zu lösen. 

[![Verwalten von Dienstproblemen](./media/service-health-overview/azure-service-health-overview-2.png)](./media/service-health-overview/azure-service-health-overview-2.png#lightbox)

Wählen Sie die Registerkarte **Potenzielle Auswirkung** aus, um die spezifische Liste der Ressourcen in Ihrem Besitz anzuzeigen, die von dem Problem betroffen sein könnten. Sie können eine CSV-Liste dieser Ressourcen herunterladen, um diese für Ihr Team freizugeben.

[![Verwalten von Dienstproblemen – Auswirkung](./media/service-health-overview/azure-service-health-overview-4.png)](./media/service-health-overview/azure-service-health-overview-4.png#lightbox)

## <a name="see-emerging-issues-which-may-impact-your-services"></a>Anzeigen von neu auftretenden Problemen, die sich möglicherweise auf Ihre Dienste auswirken

Es gibt Situationen, in denen weitverbreitete Dienstprobleme möglicherweise auf der [Azure-Statusseite](https://status.azure.com) gepostet werden, bevor eine gezielte Kommunikation an betroffene Kunden gesendet werden kann. Aktive Probleme auf der Azure-Statusseite werden in Service Health als *Neu auftretende Probleme* angezeigt, um sicherzustellen, dass Azure Service Health einen umfassenden Überblick über Probleme bietet, von denen Sie möglicherweise betroffen sind. Wenn ein Ereignis auf der Azure-Statusseite aktiv ist, wird in Service Health ein Banner mit neu auftretenden Problemen angezeigt. Klicken Sie auf das Banner, um die vollständigen Details für das Problem anzuzeigen.

![Neu auftretendes Dienstproblem](./media/service-health-overview/azure-service-health-emerging-issue.png)

## <a name="get-links-and-downloadable-explanations"></a>Abrufen von Links und herunterladbaren Erläuterungen 

Sie können einen Link für das Problem abrufen, den Sie in Ihrem Problemverwaltungssystem verwenden können. Sie können PDF- und manchmal CSV-Dateien herunterladen, die Sie für Personen freigeben können, die keinen Zugriff auf das Azure-Portal besitzen.   

[![Verwalten von Dienstproblemen – Problemverwaltung](./media/service-health-overview/azure-service-health-overview-3.png)](./media/service-health-overview/azure-service-health-overview-3.png#lightbox)

## <a name="get-support-from-microsoft"></a>Hilfe von Microsoft erhalten

Kontaktieren Sie den Support, wenn Ihre Ressource immer noch in schlechtem Zustand ist, obwohl das Problem gelöst wurde.  Verwenden Sie die Supportlinks rechts auf der Seite.  

## <a name="pin-a-personalized-health-map-to-your-dashboard"></a>Anheften einer personalisierten Integritätskarte an Ihr Dashboard

Filtern Sie Service Health, um Ihre geschäftskritischen Abonnements, Regionen und Ressourcentypen anzuzeigen. Speichern Sie den Filter, und heften Sie eine personalisierte Integritätsweltkarte an Ihr Dashboard im Portal an. 

[![Filtern der personalisierten Integritätskarte](./media/service-health-overview/azure-service-health-overview-6a.png)](./media/service-health-overview/azure-service-health-overview-6a.png#lightbox)

![Anheften einer personalisierten Integritätskarte](./media/service-health-overview/azure-service-health-overview-6b.png)

## <a name="configure-service-health-alerts"></a>Konfigurieren von Service Health-Warnungen

Service Health lässt sich in Azure Monitor integrieren und warnt Sie per E-Mail, SMS und Webhookbenachrichtigungen, wenn Ihre geschäftskritischen Ressourcen beeinträchtigt sind. Richten Sie eine Aktivitätsprotokollwarnung für das entsprechende Service Health-Ereignis ein. Leiten Sie diese Warnung mithilfe von Aktionsgruppen an die entsprechenden Personen in Ihrer Organisation weiter. Weitere Informationen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen zu Dienstbenachrichtigungen](./alerts-activity-log-service-notifications-portal.md).

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2OaXt]

## <a name="next-steps"></a>Nächste Schritte

Richten Sie Warnungen ein, damit Sie über Integritätsprobleme benachrichtigt werden. Weitere Informationen finden Sie unter [Bewährte Methoden zum Einrichten von Azure Service Health-Warnungen](https://www.youtube.com/watch?v=k5d5ca8K6tc&list=PLLasX02E8BPBBSqygdRvlTnHfp1POwE8K&index=6&t=0s). 
