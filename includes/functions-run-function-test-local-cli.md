---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/09/2020
ms.author: glenga
ms.openlocfilehash: 55c64048e0604987c5a4c26961e5617106358e76
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "84436148"
---
## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

Führen Sie Ihre Funktion aus, indem Sie den lokalen Azure Functions-Runtimehost im Ordner *LocalFunctionProj* starten:

::: zone pivot="programming-language-csharp,programming-language-powershell,programming-language-javascript,programming-language-python"
```
func start
```
::: zone-end

::: zone pivot="programming-language-typescript"
```
npm install
npm start
```
::: zone-end

::: zone pivot="programming-language-java"
```
mvn clean package 
mvn azure-functions:run
```
::: zone-end

Gegen Ende der Ausgabe sollten die folgenden Zeilen angezeigt werden: 

<pre>
...

Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

Http Functions:

        HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
...

</pre>

>[!NOTE]  
> Sollte „HttpExample“ nicht wie unten dargestellt angezeigt werden, haben Sie den Host wahrscheinlich außerhalb des Stammordners des Projekts gestartet. Drücken Sie in diesem Fall **STRG**+**C**, um den Host zu beenden. Navigieren Sie anschließend zum Stammordner des Projekts, und führen Sie den vorherigen Befehl erneut aus.

Kopieren Sie die URL Ihrer `HttpExample`-Funktion aus dieser Ausgabe in einen Browser, und fügen Sie die Abfragezeichenfolge `?name=<your-name>` an. Die vollständige URL lautet dann wie folgt: `http://localhost:7071/api/HttpExample?name=Functions`. Im Browser sollte eine Meldung wie `Hello Functions` angezeigt werden:

![Ergebnis der lokal im Browser ausgeführten Funktion](./media/functions-run-function-test-local-cli/function-test-local-browser.png)

Im Terminal, in dem Sie Ihr Projekt gestartet haben, wird beim Senden von Anforderungen auch die Protokollausgabe angezeigt.

Wenn Sie so weit sind, drücken Sie **STRG**+**C**, und wählen Sie `y` aus, um den Funktionshost zu beenden.
