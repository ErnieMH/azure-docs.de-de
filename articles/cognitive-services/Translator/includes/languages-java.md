---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.custom: devx-track-java
ms.author: erhopf
ms.openlocfilehash: 34f5aed7de32fc50587e4d291e56a464f8739183
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87374796"
---
[!INCLUDE [Prerequisites](prerequisites-java.md)]

[!INCLUDE [Set up and use environment variables](setup-env-variables.md)]

## <a name="initialize-a-project-with-gradle"></a>Initialisieren eines Projekts mit Gradle

Erstellen Sie zunächst ein Arbeitsverzeichnis für dieses Projekt. Führen Sie den folgenden Befehl über die Befehlszeile (oder das Terminal) aus:

```console
mkdir get-languages-sample
cd get-languages-sample
```

Als Nächstes initialisieren Sie ein Gradle-Projekt. Mit diesem Befehl werden grundlegende Builddateien für Gradle und insbesondere die Datei `build.gradle.kts` erstellt. Diese Datei wird zur Laufzeit zum Erstellen und Konfigurieren Ihrer Anwendung verwendet. Führen Sie den folgenden Befehl in Ihrem Arbeitsverzeichnis aus:

```console
gradle init --type basic
```

Wenn Sie zur Auswahl einer **DSL** aufgefordert werden, wählen Sie **Kotlin** aus.

## <a name="configure-the-build-file"></a>Konfigurieren der Builddatei

Navigieren Sie zu `build.gradle.kts`, und öffnen Sie die Datei in einer IDE oder einem Text-Editor Ihrer Wahl. Kopieren Sie anschließend diese Buildkonfiguration:

```java
plugins {
    java
    application
}
application {
    mainClassName = "GetLanguages"
}
repositories {
    mavenCentral()
}
dependencies {
    compile("com.squareup.okhttp:okhttp:2.5.0")
    compile("com.google.code.gson:gson:2.8.5")
}
```

Beachten Sie, dass dieses Beispiel Abhängigkeiten von OkHttp für HTTP-Anforderungen und von Gson für das Bearbeiten und Analysieren von JSON aufweist. Weitere Informationen zu Buildkonfigurationen finden Sie unter [Creating New Gradle Builds](https://guides.gradle.org/creating-new-gradle-builds/) (Erstellen neuer Gradle-Builds).

## <a name="create-a-java-file"></a>Erstellen einer Java-Datei

Erstellen Sie einen Ordner für Ihre Beispiel-App. Führen Sie in Ihrem Arbeitsverzeichnis den folgenden Befehl aus:

```console
mkdir -p src/main/java
```

Erstellen Sie als Nächstes in diesem Ordner eine Datei namens `GetLanguages.java`.

## <a name="import-required-libraries"></a>Importieren der erforderlichen Bibliotheken

Öffnen Sie `GetLanguages.java`, und fügen Sie die folgenden Importanweisungen hinzu:

```java
import java.io.*;
import java.net.*;
import java.util.*;
import com.google.gson.*;
import com.squareup.okhttp.*;
```

## <a name="define-variables"></a>Definieren von Variablen

Sie müssen zunächst eine öffentliche Klasse für Ihr Projekt erstellen:

```java
public class GetLanguages {
  // All project code goes here...
}
```

Fügen Sie der Klasse `GetLanguages` die folgenden Zeilen hinzu. Sie werden feststellen, dass der Abonnementschlüssel und der Endpunkt aus Umgebungsvariablen gelesen werden:

```java
private static String subscriptionKey = System.getenv("TRANSLATOR_TEXT_SUBSCRIPTION_KEY");
private static String endpoint = System.getenv("TRANSLATOR_TEXT_ENDPOINT");
String url = endpoint + "/languages?api-version=3.0";
```

Wenn Sie ein Cognitive Services-Abonnement mehrerer Dienste verwenden, müssen Sie auch `Ocp-Apim-Subscription-Region` in Ihre Anforderungsparameter aufnehmen. [Erfahren Sie mehr über die Authentifizierung mit dem Abonnement für mehrere Dienste](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="create-a-client-and-build-a-request"></a>Erstellen eines Clients und einer Anforderung

Fügen Sie der Klasse `GetLanguages` die folgende Zeile hinzu, um `OkHttpClient` zu instanziieren:

```java
// Instantiates the OkHttpClient.
OkHttpClient client = new OkHttpClient();
```

Erstellen Sie als Nächstes die `GET`-Anforderung.

```java
// This function performs a GET request.
public String Get() throws IOException {
    Request request = new Request.Builder()
            .url(url).get()
            .addHeader("Content-type", "application/json").build();
    Response response = client.newCall(request).execute();
    return response.body().string();
}
```

## <a name="create-a-function-to-parse-the-response"></a>Erstellen einer Funktion zum Analysieren der Antwort

Mit dieser einfachen Funktion wird die JSON-Antwort vom Translator-Dienst analysiert und übersichtlicher gemacht.

```java
// This function prettifies the json response.
public static String prettify(String json_text) {
    JsonParser parser = new JsonParser();
    JsonElement json = parser.parse(json_text);
    Gson gson = new GsonBuilder().setPrettyPrinting().create();
    return gson.toJson(json);
}
```

## <a name="put-it-all-together"></a>Korrektes Zusammenfügen

Im letzten Schritt wird eine Anforderung gesendet und eine Antwort empfangen. Fügen Sie Ihrem Projekt die folgenden Zeilen hinzu:

```java
public static void main(String[] args) {
    try {
        GetLanguages getLanguagesRequest = new GetLanguages();
        String response = getLanguagesRequest.Get();
        System.out.println(prettify(response));
    } catch (Exception e) {
        System.out.println(e);
    }
}
```

## <a name="run-the-sample-app"></a>Ausführen der Beispiel-App

Sie können Ihre Beispiel-App jetzt ausführen. Navigieren Sie über die Befehlszeile (oder Terminalsitzung) zum Stamm Ihres Projektverzeichnisses, und führen Sie Folgendes aus:

```console
gradle build
```

Führen Sie nach Abschluss des Buildvorgangs Folgendes aus:

```console
gradle run
```

## <a name="sample-response"></a>Beispiel für eine Antwort

Die Abkürzung für das Land bzw. die Region finden Sie in [dieser Liste der Sprachen](https://docs.microsoft.com/azure/cognitive-services/translator/language-support).

Es wird eine erfolgreiche Antwort im JSON-Format zurückgegeben, wie im folgenden Beispiel gezeigt:

```json
{
  "translation": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr"
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl"
    },
...
  },
  "transliteration": {
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "scripts": [
        {
          "code": "Arab",
          "name": "Arabic",
          "nativeName": "العربية",
          "dir": "rtl",
          "toScripts": [
            {
              "code": "Latn",
              "name": "Latin",
              "nativeName": "اللاتينية",
              "dir": "ltr"
            }
          ]
        },
        {
          "code": "Latn",
          "name": "Latin",
          "nativeName": "اللاتينية",
          "dir": "ltr",
          "toScripts": [
            {
              "code": "Arab",
              "name": "Arabic",
              "nativeName": "العربية",
              "dir": "rtl"
            }
          ]
        }
      ]
    },
...
  },
  "dictionary": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr",
      "translations": [
        {
          "name": "English",
          "nativeName": "English",
          "dir": "ltr",
          "code": "en"
        }
      ]
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl",
      "translations": [
        {
          "name": "English",
          "nativeName": "English",
          "dir": "ltr",
          "code": "en"
        }
      ]
    },
...
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich anhand der API-Referenz mit den Möglichkeiten von Translator vertraut.

> [!div class="nextstepaction"]
> [API-Referenz](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
