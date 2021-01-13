---
title: Verwendungsanalyse mit Azure Application Insights | Microsoft Docs
description: Verstehen Sie Ihre Benutzer und wie sie Ihre App verwenden.
ms.topic: conceptual
ms.date: 03/25/2019
ms.openlocfilehash: 4f4954451bfa195b07c580ffa451b8cb333eb32c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90532102"
---
# <a name="usage-analysis-with-application-insights"></a>Verwendungsanalyse mit Application Insights

Welche Funktionen Ihrer Web- oder mobilen App sind die beliebtesten? Erreichen die Benutzer mit Ihrer App ihre Ziele? Brechen sie an bestimmten Stellen ab, und kehren sie später zurück?  Mit [Azure Application Insights](./app-insights-overview.md) erhalten Sie wertvolle Einblicke in die Nutzung Ihrer App durch die Benutzer. Bei jeder Aktualisierung Ihrer App können Sie beurteilen, wie gut sie für Benutzer funktioniert. Mit diesem Wissen können Sie datengesteuerte Entscheidungen über die nächsten Entwicklungszyklen treffen.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4Cijb]

## <a name="send-telemetry-from-your-app"></a>Senden von Telemetriedaten aus der App

Sie erzielen optimale Ergebnisse, wenn Sie Application Insights im Servercode der App und in den Webseiten installieren. Die Client- und die Serverkomponente der App senden Telemetriedaten zur Analyse an das Azure-Portal.

1. **Servercode:** Installieren Sie das entsprechende Modul für die [ASP.NET](./asp-net.md)-, [Azure](./app-insights-overview.md)-, [Java](./java-get-started.md)-, [Node.js](./nodejs.md)-App oder einen [anderen Typ](./platforms.md) von App.

    * *Sie möchten keinen Servercode installieren? [Erstellen Sie einfach eine Azure Application Insights-Ressource](./create-new-resource.md).*

2. **Webseitencode:** Fügen Sie Ihrer Webseite vor dem schließenden ``</head>``-Tag das folgende Skript hinzu. Ersetzen Sie den Instrumentierungsschlüssel durch den geeigneten Wert für Ihre Application Insights-Ressource:
    
    ```html
    <script type="text/javascript">
    var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){function n(e){t[e]=function(){var n=arguments;t.queue.push(function(){t[e].apply(t,n)})}}var t={config:e};t.initialize=!0;var i=document,a=window;setTimeout(function(){var n=i.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/scripts/b/ai.2.min.js",i.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{t.cookie=i.cookie}catch(e){}t.queue=[],t.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var s="Track"+r[0];if(n("start"+s),n("stop"+s),n("setAuthenticatedUserContext"),n("clearAuthenticatedUserContext"),n("flush"),!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var o=a[r];a[r]=function(e,n,i,a,s){var c=o&&o(e,n,i,a,s);return!0!==c&&t["_"+r]({message:e,url:n,lineNumber:i,columnNumber:a,error:s}),c},e.autoExceptionInstrumented=!0}return t}(
    {
      instrumentationKey:"INSTRUMENTATION_KEY"
    }
    );window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
    </script>
    ```

    Informationen zu komplexeren Konfigurationen für die Websiteüberwachung finden Sie im [Referenzartikel zum JavaScript SDK](./javascript.md).

3. **Code der mobilen App:** Verwenden Sie das App Center SDK, um Ereignisse aus Ihrer App zu erfassen, und senden Sie Kopien dieser Ereignisse zur Analyse an Application Insights. Eine entsprechende Anleitung finden Sie [hier](../learn/mobile-center-quickstart.md).

4. **Abrufen von Telemetriedaten:** Führen Sie das Projekt einige Minuten lang im Debugmodus aus, und suchen Sie dann im Blatt „Übersicht“ von Application Insights nach Ergebnissen.

    Veröffentlichen Sie die App, um die Leistung der App zu überwachen und zu ermitteln, was die Benutzer mit Ihrer App tun.

## <a name="include-user-and-session-id-in-your-telemetry"></a>Einschließen der Benutzer- und Sitzungs-ID in der Telemetrie
Um im Laufe der Zeit Benutzeraktionen nachzuverfolgen, erfordert Application Insights die Möglichkeit, um diese zu kennzeichnen. Das Ereignistool ist das einzige Nutzungstool, das keine Benutzer- oder Sitzungs-ID erfordert.

Beginnen Sie mit dem Senden von Benutzer- und Sitzungs-IDs mithilfe [dieses Prozesses](./usage-send-user-context.md).

## <a name="explore-usage-demographics-and-statistics"></a>Untersuchen von demografische Daten und Statistiken zur Nutzung
Ermitteln Sie, wann Personen Ihre App verwenden, für welche Seiten sie sich am meisten interessieren, wo sich die Benutzer befinden, welche Browser und Betriebssysteme sie verwenden. 

In Benutzer- und Sitzungsberichte werden Ihre Daten nach Seiten oder benutzerdefinierten Ereignissen gefiltert und nach Eigenschaften wie Speicherort, Umgebung und Seite segmentiert. Sie können auch eigene Filter hinzufügen.

![Bildschirmaufnahme der Übersichtsseite „Benutzer“ für ein fiktives Unternehmen](./media/usage-overview/users.png)  

Aus den Informationen auf der rechten Seite gehen interessante Muster im Datensatz hervor.  

* Der Bericht **Benutzer** zählt die eindeutigen Benutzer, die innerhalb der ausgewählten Zeiträume auf Ihre Seiten zugreifen. Für Web-Apps werden Benutzer mithilfe von Cookies gezählt. Wenn eine Person mit verschiedenen Browsern oder Clientcomputern auf Ihre Website zugreift oder ihre Cookies löscht, wird sie mehrfach gezählt.
* Der Bericht **Sitzungen** zählt die Benutzersitzungen, die auf Ihre Website zugreifen. Eine Sitzung ist ein Zeitraum der Aktivität eines Benutzers, der von einem Zeitraum der Inaktivität von mehr als eine halben Stunde beendet wird.

[Weitere Informationen zu den Tools für Benutzer, Sitzungen und Ereignisse](usage-segmentation.md)  

## <a name="retention---how-many-users-come-back"></a>Vermerkdauer – wie viele Benutzer kehren zurück?

Anhand der Vermerkdauer können Sie basierend auf Kohorten von Benutzern, die während eines bestimmten Zeitrahmens eine Businessaktion durchgeführt haben, nachvollziehen, wie oft Ihre Benutzer die App erneut verwenden. 

- Ermitteln, welche bestimmten Funktionen dazu führen, dass einige Benutzer häufiger zurückkehren als andere 
- Bilden von Hypothesen basierend auf echten Benutzerdaten 
- Bestimmen, ob die Vermerkdauer ein Problem in Ihrem Produkt darstellt 

![Bildschirmaufnahme der Übersichtsseite „Aufbewahrung“, auf der Informationen darüber angezeigt werden, wie oft Benutzer zur Verwendung ihrer App zurückkehren](./media/usage-overview/retention.png) 

Die Vermerkdauer-Steuerelemente oben ermöglichen Ihnen das Definieren bestimmter Ereignisse und des Zeitbereichs für die Berechnen der Vermerkdauer. Das Diagramm in der Mitte bietet eine visuelle Darstellung des Prozentsatzes der gesamten Vermerkdauer nach dem angegebenen Zeitbereichs. Im Diagramm unten wird eine einzelne Vermerkdauer in einem bestimmten Zeitraum dargestellt. Dank dieses Detaillierungsgrads können Sie mit höherer Granularität verstehen, was Ihre Benutzer tun und was sich auf zurückkehrende Benutzer auswirkt.  

[Weitere Informationen zum Vermerkdauer-Tool](usage-retention.md)

## <a name="custom-business-events"></a>Benutzerdefinierte Geschäftsereignisse

Um ein genaues Verständnis davon zu erhalten, was Benutzer mit Ihrer App machen, ist es hilfreich, Codezeilen einzufügen, um benutzerdefinierte Ereignisse zu protokollieren. Diese Ereignisse können alles nachverfolgen – von detaillierten Benutzeraktionen wie das Klicken auf bestimmte Schaltflächen bis hin zu aussagekräftigeren Geschäftsereignissen wie das Tätigen eines Kaufs oder das Gewinnen eines Spiels. 

Seitenaufrufe können zwar in einigen Fällen nützliche Ereignisse darstellen, im Allgemeinen ist dies jedoch nicht der Fall. Ein Benutzer kann eine Produktseite öffnen, ohne das Produkt zu kaufen. 

Mit bestimmten Geschäftsereignisse können Sie den Aufenthalt von Benutzern auf Ihrer Website in einem Diagramm darstellen. Sie können deren Einstellungen für unterschiedliche Optionen ermitteln und herausfinden, wo sie abbrechen oder Schwierigkeiten haben. Mit diesem Wissen können Sie fundierte Entscheidungen über die Prioritäten in Ihrem Entwicklungs-Backlog treffen.

Ereignisse können von der Clientseite der App aus protokolliert werden:

```JavaScript

    appInsights.trackEvent("ExpandDetailTab", {DetailTab: tabName});
```

Oder von der Serverseite:

```csharp
    var tc = new Microsoft.ApplicationInsights.TelemetryClient();
    tc.TrackEvent("CreatedAccount", new Dictionary<string,string> {"AccountType":account.Type}, null);
    ...
    tc.TrackEvent("AddedItemToCart", new Dictionary<string,string> {"Item":item.Name}, null);
    ...
    tc.TrackEvent("CompletedPurchase");
```

Sie können diesen Ereignissen Eigenschaftswerte anfügen, damit Sie die Ereignisse beim Überprüfen im Portal filtern oder teilen können. Außerdem wird jedem Ereignis ein Standardsatz von Eigenschaften angefügt, z.B. eine anonyme Benutzer-ID, die Ihnen die Ablaufverfolgung der Sequenz von Aktivitäten eines einzelnen Benutzers ermöglicht.

Erfahren Sie mehr über [benutzerdefinierte Ereignisse](./api-custom-events-metrics.md#trackevent) und [Eigenschaften](./api-custom-events-metrics.md#properties).

### <a name="slice-and-dice-events"></a>Aufteilen von Ereignissen

In den Tools für Benutzer, Sitzungen und Ereignisse können Sie benutzerdefinierte Ereignisse nach Benutzer, Ereignisname und Eigenschaften aufteilen.
![Bildschirmaufnahme der Übersichtsseite „Benutzer“ für ein fiktives Unternehmen](./media/usage-overview/users.png)  
  
## <a name="design-the-telemetry-with-the-app"></a>Entwerfen der Telemetrie mit der App

Berücksichtigen Sie beim Entwerfen der einzelnen Funktionen Ihrer App, wie sie deren Erfolg bei den Benutzern messen werden. Entscheiden Sie, welche Geschäftsereignisse aufgezeichnet werden müssen, und codieren Sie die Aufrufnachverfolgung für diese Ereignisse von Anfang an in die App.

## <a name="a--b-testing"></a>A-B-Tests
Wenn Sie nicht wissen, welche Variante eines Features erfolgreicher sein wird, veröffentlichten Sie beide, und ermöglichen Sie verschiedenen Benutzern den Zugriff darauf. Messen Sie den jeweiligen Erfolg, und erstellen Sie anschließend eine vereinheitlichte Version.

Für dieses Verfahren fügen Sie unterschiedliche Eigenschaftswerte an alle Telemetriedaten an, die von jeder Version Ihrer App gesendet wird. Dazu definieren Sie Eigenschaften im aktiven "TelemetryContext"-Element. Diese Standardeigenschaften werden jeder Telemetrienachricht hinzugefügt, die die Anwendung sendet, nicht nur Ihren benutzerdefinierten Nachrichten, sondern auch den Standardtelemetriedaten.

Filtern und teilen Sie im Application Insights-Portal Ihre Daten anhand der Eigenschaftswerte, um die verschiedenen Versionen zu vergleichen.

Dazu [richten Sie einen Telemetrieinitialisierer ein](./api-filtering-sampling.md#addmodify-properties-itelemetryinitializer):

**ASP.NET-Apps**

```csharp
    // Telemetry initializer class
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize(ITelemetry item)
            {
                var itemProperties = item as ISupportProperties;
                if (itemProperties != null && !itemProperties.Properties.ContainsKey("AppVersion"))
                {
                    itemProperties.Properties["AppVersion"] = "v2.1";
                }
            }
    }
```

Im Web-App-Initialisierer wie „Global.asax.cs“:

```csharp

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
         .Add(new MyTelemetryInitializer());
    }
```

**ASP.NET Core Apps**

> [!NOTE]
> Das Hinzufügen eines Initialisierers mit `ApplicationInsights.config` oder `TelemetryConfiguration.Active` ist für ASP.NET Core-Anwendungen nicht zulässig. 

Zum Hinzufügen eines neuen `TelemetryInitializer` für [ASP.NET Core](asp-net-core.md#adding-telemetryinitializers)-Anwendungen wird dieser wie unten gezeigt dem Abhängigkeitsinjektionscontainer hinzugefügt. Dies erfolgt in der `ConfigureServices`-Methode Ihrer `Startup.cs`-Klasse.

```csharp
 using Microsoft.ApplicationInsights.Extensibility;
 using CustomInitializer.Telemetry;
 public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<ITelemetryInitializer, MyTelemetryInitializer>();
}
```

Alle neuen TelemetryClients-Elemente fügen automatisch den von Ihnen angegebenen Eigenschaftswert hinzu. Einzelne Telemetrieereignisse können die Standardwerte außer Kraft setzen.

## <a name="next-steps"></a>Nächste Schritte
   - [Benutzer, Sitzungen, Ereignisse](usage-segmentation.md)
   - [Trichter](usage-funnels.md)
   - [Vermerkdauer](usage-retention.md)
   - [Benutzerabläufe](usage-flows.md)
   - [Arbeitsmappen](../platform/workbooks-overview.md)
   - [Hinzufügen von Benutzerkontext](usage-send-user-context.md)

