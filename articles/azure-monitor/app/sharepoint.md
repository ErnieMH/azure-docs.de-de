---
title: Überwachen einer SharePoint-Website mit Application Insights
description: Beginnen Sie mit der Überwachung einer neuen Anwendung mit einem neuen Instrumentationsschlüssel.
ms.topic: conceptual
ms.date: 09/08/2020
ms.openlocfilehash: afd7a5fc8ccf7546078fa8a4ffa7a076cb05be6a
ms.sourcegitcommit: 6a4687b86b7aabaeb6aacdfa6c2a1229073254de
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91757247"
---
# <a name="monitor-a-sharepoint-site-with-application-insights"></a>Überwachen einer SharePoint-Website mit Application Insights

Azure Application Insights überwacht die Verfügbarkeit, Leistung und Nutzung Ihrer Apps. Hier erfahren Sie, wie Sie dieses Tool für eine SharePoint-Website einrichten.

> [!NOTE]
> Aufgrund von Sicherheitsbedenken können Sie das in diesem Artikel beschriebene Skript auf der modernen SharePoint-Benutzeroberfläche nicht direkt zu Ihren Webseiten hinzufügen. Als Alternative können Sie das [SharePoint Framework (SPFx)](/sharepoint/dev/spfx/extensions/overview-extensions) verwenden, um eine benutzerdefinierte Erweiterung zu erstellen, über die Sie Application Insights in Ihren SharePoint-Websites installieren können.

## <a name="create-an-application-insights-resource"></a>Erstellen einer Application Insights-Ressource
Erstellen Sie im [Azure-Portal](https://portal.azure.com)eine neue Application Insights-Ressource. Wählen Sie als Anwendungstyp "ASP.NET" aus.

![Klicken Sie auf "Eigenschaften", wählen Sie den Schlüssel aus, und drücken Sie STRG+C](./media/sharepoint/001.png)

Im daraufhin geöffneten Fenster werden die Leistungs- und Nutzungsdaten für Ihre App angezeigt. Um bei der nächsten Anmeldung bei Azure dorthin zu gelangen, sollten Sie eine Kachel auf dem Startbildschirm anlegen. Klicken Sie alternativ auf "Durchsuchen", um das Blatt zu finden.

## <a name="add-the-script-to-your-web-pages"></a>Hinzufügen des Skripts zu Ihren Webseiten

```HTML
<!-- 
To collect user behavior analytics tools about your application, 
insert the following script into each page you want to track.
Place this code immediately before the closing </head> tag,
and before any other scripts. Your first data will appear 
automatically in just a few seconds.
-->
<script type="text/javascript">
var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(n){var o={config:n,initialize:!0},t=document,e=window,i="script";setTimeout(function(){var e=t.createElement(i);e.src=n.url||"https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js",t.getElementsByTagName(i)[0].parentNode.appendChild(e)});try{o.cookie=t.cookie}catch(e){}function a(n){o[n]=function(){var e=arguments;o.queue.push(function(){o[n].apply(o,e)})}}o.queue=[],o.version=2;for(var s=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];s.length;)a("track"+s.pop());var r="Track",c=r+"Page";a("start"+c),a("stop"+c);var u=r+"Event";if(a("start"+u),a("stop"+u),a("addTelemetryInitializer"),a("setAuthenticatedUserContext"),a("clearAuthenticatedUserContext"),a("flush"),o.SeverityLevel={Verbose:0,Information:1,Warning:2,Error:3,Critical:4},!(!0===n.disableExceptionTracking||n.extensionConfig&&n.extensionConfig.ApplicationInsightsAnalytics&&!0===n.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){a("_"+(s="onerror"));var p=e[s];e[s]=function(e,n,t,i,a){var r=p&&p(e,n,t,i,a);return!0!==r&&o["_"+s]({message:e,url:n,lineNumber:t,columnNumber:i,error:a}),r},n.autoExceptionInstrumented=!0}return o}(
{
  instrumentationKey:"INSTRUMENTATION_KEY"
}
);(window[aiName]=aisdk).queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
</script>
```

Fügen Sie das Skript direkt vor dem &lt;/head&gt;-Tag jeder Seite ein, die Sie nachverfolgen möchten. Wenn Ihre Website über eine Masterseite verfügt, können Sie das Skript dort ablegen. Beispielsweise würden Sie es in einem ASP.NET MVC-Projekt unter „View\Shared\_Layout.cshtml“ speichern.

Das Skript enthält den Instrumentationsschlüssel, der die Telemetriedaten an Ihre Application Insights-Ressource leitet.

### <a name="add-the-code-to-your-site-pages"></a>Hinzufügen des Codes zu den Seiten Ihrer Website
#### <a name="on-the-master-page"></a>Auf der Masterseite
Wenn Sie die Masterseite der Website bearbeiten können, kann dadurch die Überwachung jeder Seite der Website bereitgestellt werden.

Checken Sie die Masterseite aus, und bearbeiten Sie sie mit SharePoint Designer oder einem anderen Editor.

![Screenshot, der zeigt, wie die Masterseite mithilfe von SharePoint Designer oder einem anderen Editor bearbeitet wird](./media/sharepoint/03-master.png)

Fügen Sie den Code direkt vor dem Tag </head> ein. 

![Screenshot, der zeigt, wo Sie den Code zur Seite Ihrer Website hinzufügen können](./media/sharepoint/04-code.png)

#### <a name="or-on-individual-pages"></a>Auf einzelnen Seiten
Um eine begrenzte Anzahl von Seiten zu überwachen, fügen Sie das Skript separat auf jeder Seite hinzu. 

Fügen Sie ein Webpart ein, und betten Sie den Codeausschnitt darin ein.

![Screenshot, der das Hinzufügen des Skripts zum Überwachen einer begrenzten Gruppe von Seiten zeigt](./media/sharepoint/05-page.png)

## <a name="view-data-about-your-app"></a>Anzeigen von Daten über Ihre App
Stellen Sie Ihre App erneut bereit.

Kehren Sie zum Blatt Ihrer Anwendung im [Azure-Portal](https://portal.azure.com)zurück.

Die ersten Ereignisse werden in der Suche angezeigt. 

![Screenshot, der die neuen Daten zeigt, die Sie in der App anzeigen können](./media/sharepoint/09-search.png)

Klicken Sie nach einigen Sekunden auf "Aktualisieren", wenn Sie mehr Daten erwarten.

## <a name="capturing-user-id"></a>Erfassen der Benutzer-ID
Der Codeausschnitt der Standardwebseite erfasst nicht die Benutzer-ID aus SharePoint. Mit einer kleinen Änderung ist dies allerdings möglich.

1. Kopieren Sie den Instrumentationsschlüssel Ihrer App aus der Essentials-Dropdownliste in Application Insights. 

    ![Screenshot, der das Kopieren der App-Instrumentierung vom Dropdownmenü „Grundlagen“ in Application Insights zeigt](./media/sharepoint/02-props.png)

1. Ersetzen Sie „XXXX“ durch den Instrumentationsschlüssel im Codeausschnitt unten. 
2. Betten Sie anstelle des Ausschnitts, den Sie aus dem Portal erhalten, das Skript in die SharePoint-App ein.

```


<SharePoint:ScriptLink ID="ScriptLink1" name="SP.js" runat="server" localizable="false" loadafterui="true" /> 
<SharePoint:ScriptLink ID="ScriptLink2" name="SP.UserProfiles.js" runat="server" localizable="false" loadafterui="true" /> 

<script type="text/javascript"> 
var personProperties; 

// Ensure that the SP.UserProfiles.js file is loaded before the custom code runs. 
SP.SOD.executeOrDelayUntilScriptLoaded(getUserProperties, 'SP.UserProfiles.js'); 

function getUserProperties() { 
    // Get the current client context and PeopleManager instance. 
    var clientContext = new SP.ClientContext.get_current(); 
    var peopleManager = new SP.UserProfiles.PeopleManager(clientContext); 

    // Get user properties for the target user. 
    // To get the PersonProperties object for the current user, use the 
    // getMyProperties method. 

    personProperties = peopleManager.getMyProperties(); 

    // Load the PersonProperties object and send the request. 
    clientContext.load(personProperties); 
    clientContext.executeQueryAsync(onRequestSuccess, onRequestFail); 
} 

// This function runs if the executeQueryAsync call succeeds. 
function onRequestSuccess() { 
var appInsights=window.appInsights||function(config){
function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
    }({
        instrumentationKey:"XXXX"
    });
    window.appInsights=appInsights;
    appInsights.trackPageView(document.title,window.location.href, {User: personProperties.get_displayName()});
} 

// This function runs if the executeQueryAsync call fails. 
function onRequestFail(sender, args) { 
} 
</script> 


```



## <a name="next-steps"></a>Nächste Schritte
* [Webtests](./monitor-web-app-availability.md) zur Überwachung der Verfügbarkeit Ihrer Website.
* [Application Insights](./app-insights-overview.md) für andere App-Typen.

<!--Link references-->

