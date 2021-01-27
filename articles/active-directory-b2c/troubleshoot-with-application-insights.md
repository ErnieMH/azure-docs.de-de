---
title: Beheben von Problemen bei benutzerdefinierten Richtlinien mit Application Insights
titleSuffix: Azure AD B2C
description: Einrichten von Application Insights zum Nachverfolgen der Ausführung Ihrer benutzerdefinierten Richtlinien.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: troubleshooting
ms.date: 10/16/2020
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 33504487b6175023e18893812c533950305cb1d3
ms.sourcegitcommit: 4d48a54d0a3f772c01171719a9b80ee9c41c0c5d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2021
ms.locfileid: "98746001"
---
# <a name="collect-azure-active-directory-b2c-logs-with-application-insights"></a>Sammeln von Azure Active Directory B2C-Protokollen mit Application Insights

In diesem Artikel werden die Schritte zum Sammeln von Protokollen von Active Directory B2C (Azure AD B2C) beschrieben, mit denen Sie Probleme bei Ihren benutzerdefinierten Richtlinien diagnostizieren können. Application Insights bietet eine Möglichkeit zum Diagnostizieren von Ausnahmen und Visualisieren von Leistungsproblemen der Anwendung. Azure AD B2C enthält eine Funktion zum Senden von Daten an Application Insights.

Die hier detailliert beschriebenen Aktivitätsprotokolle sollten **NUR** bei der Entwicklung Ihrer benutzerdefinierten Richtlinien aktiviert werden.

> [!WARNING]
> Legen Sie `DeploymentMode` in Produktionsumgebungen nicht auf `Development` fest. Protokolle erfassen alle Ansprüche, die an Identitätsanbieter gesendet bzw. von ihnen empfangen werden. Sie als Entwickler sind für alle personenbezogenen Daten verantwortlich, die in Ihren Application Insights-Protokollen gesammelt werden. Diese detaillierten Protokolle werden nur gesammelt, wenn sich die Richtlinie im **ENTWICKLUNGSMODUS** befindet.

## <a name="set-up-application-insights"></a>Einrichten von Application Insights

Erstellen Sie eine Instanz von Application Insights in Ihrem Abonnement, falls Sie noch keine haben.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie auf der oberen Menüleiste den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihr Azure-Abonnement (nicht Ihr Azure AD B2C-Verzeichnis) enthält.
1. Wählen Sie **Ressource erstellen** im linken Navigationsbereich aus.
1. Suchen Sie nach **Application Insights**, wählen Sie die Option aus, und wählen Sie dann **Erstellen** aus.
1. Füllen Sie das Formular aus, wählen Sie **Überprüfen und erstellen** und dann **Erstellen** aus.
1. Wählen Sie nach Abschluss der Bereitstellung **Zu Ressource wechseln** aus.
1. Wählen Sie im Application Insights-Menü unter **Konfigurieren** die Option **Eigenschaften** aus.
1. Notieren Sie den **Instrumentierungsschlüssel** zur Verwendung in einem späteren Schritt.

## <a name="configure-the-custom-policy"></a>Konfigurieren der benutzerdefinierten Richtlinie

1. Öffnen Sie die RP-Datei (Relying Party, vertrauende Seite), z.B. *SignUpOrSignin.xml*.
1. Fügen Sie dem `<TrustFrameworkPolicy>`-Element die folgenden Attribute hinzu:

   ```xml
   DeploymentMode="Development"
   UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
   ```

1. Fügen Sie dem Knoten `<RelyingParty>` einen untergeordneten Knoten `<UserJourneyBehaviors>` hinzu, falls dieser noch nicht vorhanden ist. Er muss direkt nach `<DefaultUserJourney ReferenceId="UserJourney Id" from your extensions policy, or equivalent (for example:SignUpOrSigninWithAAD" />` angeordnet werden.
1. Fügen Sie den folgenden Knoten als untergeordnetes Element des `<UserJourneyBehaviors>`-Elements hinzu. Achten Sie darauf, `{Your Application Insights Key}` durch den Application Insights-**Instrumentierungsschlüssel** zu ersetzen, den Sie zuvor notiert haben.

    ```xml
    <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
    ```

    * `DeveloperMode="true"` weist Application Insights an, die Telemetriedaten über die Verarbeitungspipeline zu beschleunigen. Dies ist gut für die Entwicklung, bei hohem Datenvolumen jedoch mit Einschränkungen versehen. Legen Sie `DeveloperMode` in der Produktion auf `false` fest.
    * `ClientEnabled="true"` sendet das clientseitige Application Insights-Skript zum Nachverfolgen der Seitenansicht und von clientseitigen Fehlern. Diese können Sie in der **browserTimings**-Tabelle im Application Insights-Portal anzeigen. Durch die Einstellung von `ClientEnabled= "true"` fügen Sie Application Insights Ihrem Seitenskript hinzu und erhalten Zeitangaben zu Seitenladevorgängen und AJAX-Aufrufen, Anzahl und Details von Browserausnahmen und AJAX-Fehlern sowie die Anzahl von Benutzern und Sitzungen. Dieses Feld ist **optional** und ist standardmäßig auf `false` eingestellt.
    * `ServerEnabled="true"` sendet die vorhandene Datei „UserJourneyRecorder.json“ als benutzerdefiniertes Ereignis an Application Insights.

    Beispiel:

    ```xml
    <TrustFrameworkPolicy
      ...
      TenantId="fabrikamb2c.onmicrosoft.com"
      PolicyId="SignUpOrSignInWithAAD"
      DeploymentMode="Development"
      UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
    >
    ...
    <RelyingParty>
      <DefaultUserJourney ReferenceId="UserJourney ID from your extensions policy, or equivalent (for example: SignUpOrSigninWithAzureAD)" />
      <UserJourneyBehaviors>
        <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
      </UserJourneyBehaviors>
      ...
    </TrustFrameworkPolicy>
    ```

1. Laden Sie die Richtlinie hoch.

## <a name="see-the-logs-in-application-insights"></a>Anzeigen von Protokollen in Application Insights

Es gibt eine kurze Verzögerung (in der Regel weniger als fünf Minuten), bevor Sie neue Protokolle in Application Insights anzeigen können.

1. Öffnen Sie die zuvor erstellte Application Insights-Ressource im [Azure-Portal](https://portal.azure.com).
1. Wählen Sie auf der Seite **Übersicht** die Option **Protokolle** aus.
1. Öffnen Sie eine neue Registerkarte in Application Insights.

Es folgt eine Liste der Abfragen, die Sie zum Anzeigen der Protokolle verwenden können:

| Abfrage | BESCHREIBUNG |
|---------------------|--------------------|
`traces` | Anzeigen aller von Azure AD B2C generierten Protokolle |
`traces | where timestamp > ago(1d)` | Anzeigen aller am letzten Tag von Azure AD B2C generierten Protokolle

Diese Einträge können ggf. lang sein. Führen Sie einen Export in eine CSV-Datei durch, um sich dies genauer anzusehen.

Weitere Informationen zu Abfragen finden Sie unter [Übersicht über Protokollabfragen in Azure Monitor](../azure-monitor/log-query/log-query-overview.md).

## <a name="configure-application-insights-in-production"></a>Konfigurieren von Application Insights in der Produktionsumgebung

Um die Leistung der Produktionsumgebung und die Benutzerfreundlichkeit zu verbessern, müssen Sie Ihre Richtlinie so konfigurieren, dass unwichtige Nachrichten ignoriert werden. Verwenden Sie die folgende Konfiguration, um nur kritische Fehlermeldungen an Application Insights zu senden. 

1. Legen Sie das `DeploymentMode`-Attribut von [TrustFrameworkPolicy](trustframeworkpolicy.md) auf `Production` fest. 

   ```xml
   <TrustFrameworkPolicy xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06" PolicySchemaVersion="0.3.0.0"
   TenantId="yourtenant.onmicrosoft.com"
   PolicyId="B2C_1A_signup_signin"
   PublicPolicyUri="http://yourtenant.onmicrosoft.com/B2C_1A_signup_signin"
   DeploymentMode="Production"
   UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights">
   ```

1. Legen Sie `DeveloperMode` von [JourneyInsights](relyingparty.md#journeyinsights) auf `false` fest.

   ```xml
   <UserJourneyBehaviors>
     <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="false" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
   </UserJourneyBehaviors>
   ```
   
1. Laden Sie die Richtlinie hoch, und testen Sie sie.

## <a name="next-steps"></a>Nächste Schritte

In der Community wurde ein User Journey-Viewer entwickelt, der als Hilfe für Identitätsentwickler dient. Mit dem Viewer werden Daten aus Ihrer Application Insights-Instanz gelesen, und es wird eine gut strukturierte Ansicht der User Journey-Ereignisse bereitgestellt. Sie können den Quellcode abrufen und in Ihrer eigenen Lösung bereitstellen.

Der User Journey-Player wird von Microsoft nicht unterstützt und unverändert zur Verfügung gestellt.

Die Version des Viewers, der Ereignisse aus Application Insights liest, befindet sich auf GitHub unter

[Azure-Samples/active-directory-b2c-advanced-policies](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/wingtipgamesb2c/src/WingTipUserJourneyPlayerWebApplication).
