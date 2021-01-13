---
title: 'Schnellstart: Java-Clientbibliothek für die Bing-Videosuche'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/19/2020
ms.custom: devx-track-java
ms.author: aahi
ms.openlocfilehash: 2b3d4993406f150b2983d4d820f7d070b5de1e96
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "87375759"
---
Verwenden Sie diese Schnellstartanleitung, um unter Verwendung der Bing-Videosuche-Clientbibliothek für Java mit der Suche nach Nachrichten zu beginnen. Die Bing-Videosuche verfügt zwar über eine REST-API, die mit den meisten Programmiersprachen kompatibel ist, die Clientbibliothek ist jedoch eine einfache Möglichkeit, den Dienst in Ihre Anwendungen zu integrieren. Der Quellcode für dieses Beispiel steht mit zusätzlichen Anmerkungen und Features auf [GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingVideoSearch) bereit.

## <a name="prerequisites"></a>Voraussetzungen

* Das [Java Development Kit (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/jdk11-downloads-5066655.html)

* Die [Gson-Bibliothek](https://github.com/google/gson)

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](~/includes/cognitive-services-bing-video-search-signup-requirements.md)]

Installieren Sie die Abhängigkeit für die Bing-Videosuche-Clientbibliothek mithilfe von Maven, Gradle oder einem anderen Abhängigkeitsverwaltungssystem. Die Maven-POM-Datei erfordert die folgende Deklaration:

```xml
  <dependencies>
    <dependency>
      <groupId>com.microsoft.azure.cognitiveservices</groupId>
      <artifactId>azure-cognitiveservices-videosearch</artifactId>
      <version>0.0.1-beta-SNAPSHOT</version>
    </dependency>
  </dependencies> 
```

## <a name="create-and-initialize-a-project"></a>Erstellen und Initialisieren eines Projekts


Erstellen Sie in Ihrer bevorzugten IDE oder in Ihrem bevorzugten Editor ein neues Java-Projekt, und importieren Sie die folgenden Bibliotheken.

```java
    import com.microsoft.azure.cognitiveservices.videosearch.*;
    import com.microsoft.azure.cognitiveservices.videosearch.VideoObject;
    import com.microsoft.rest.credentials.ServiceClientCredentials;
    import okhttp3.Interceptor;
    import okhttp3.OkHttpClient;
    import okhttp3.Request;
    import okhttp3.Response;
    import java.io.IOException;
    import java.util.ArrayList;
    import java.util.List; 
```

## <a name="create-a-search-client"></a>Erstellen eines Suchclients

1. Implementieren Sie den `VideoSearchAPIImpl`-Client, der Ihren API-Endpunkt und eine Instanz der `ServiceClientCredentials`-Klasse erfordert.

    ```java
    public static VideoSearchAPIImpl getClient(final String subscriptionKey) {
        return new VideoSearchAPIImpl("https://api.cognitive.microsoft.com/bing/v7.0/",
                new ServiceClientCredentials() {
                //...
                }
    )};
    ```

    Führen Sie zum Implementieren von `ServiceClientCredentials` die folgenden Schritte aus:

    1. Überschreiben Sie die Funktion `applyCredentialsFilter()` mit einem `OkHttpClient.Builder`-Objekt als Parameter. 
        
        ```java
        //...
        new ServiceClientCredentials() {
                @Override
                public void applyCredentialsFilter(OkHttpClient.Builder builder) {
                //...
                }
        //...
        ```
    
    2. Rufen Sie `builder.addNetworkInterceptor()` in `applyCredentialsFilter()` auf. Erstellen Sie ein neues `Interceptor`-Objekt, und überschreiben Sie seine `intercept()`-Methode, um ein Interceptor-Objekt vom Typ `Chain` zu übernehmen.

        ```java
        //...
        builder.addNetworkInterceptor(
            new Interceptor() {
                @Override
                public Response intercept(Chain chain) throws IOException {
                //...    
                }
            });
        ///...
        ```

    3. Erstellen Sie in der Funktion `intercept` Variablen für Ihre Anforderung. Verwenden Sie `Request.Builder()` zum Erstellen Ihrer Anforderung. Fügen Sie dem Header `Ocp-Apim-Subscription-Key` Ihren Abonnementschlüssel hinzu, und geben Sie `chain.proceed()` für das Anforderungsobjekt zurück.
            
        ```java
        //...
        public Response intercept(Chain chain) throws IOException {
            Request request = null;
            Request original = chain.request();
            Request.Builder requestBuilder = original.newBuilder()
                    .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
            request = requestBuilder.build();
            return chain.proceed(request);
        }
        //...
        ```

## <a name="send-a-search-request-and-receive-the-response"></a>Senden einer Suchanforderung und Empfangen der Antwort 

1. Erstellen Sie eine Funktion namens `VideoSearch()`, die Ihren Abonnementschlüssel als Zeichenfolge akzeptiert. Instanziieren Sie den zuvor erstellten Suchclient.
    
    ```java
    public static void VideoSearch(String subscriptionKey){
        VideoSearchAPIImpl client = VideoSDK.getClient(subscriptionKey);
        //...
    }
    ```
2. Senden Sie innerhalb von `VideoSearch()` unter Verwendung des Clients eine Videosuchanforderung mit `SwiftKey` als Suchbegriff. Wenn die API für die Videosuche ein Ergebnis zurückgegeben hat, rufen Sie das erste Ergebnis ab, und geben Sie die ID, den Namen und die URL sowie die Gesamtanzahl der zurückgegebenen Videos aus. 
    
    ```java
    VideosInner videoResults = client.searchs().list("SwiftKey");

    if (videoResults == null){
        System.out.println("Didn't see any video result data..");
    }
    else{
        if (videoResults.value().size() > 0){
            VideoObject firstVideoResult = videoResults.value().get(0);

            System.out.println(String.format("Video result count: %d", videoResults.value().size()));
            System.out.println(String.format("First video id: %s", firstVideoResult.videoId()));
            System.out.println(String.format("First video name: %s", firstVideoResult.name()));
            System.out.println(String.format("First video url: %s", firstVideoResult.contentUrl()));
        }
        else{
            System.out.println("Couldn't find video results!");
        }
    }
    ```

3. Rufen Sie die Suchmethode über Ihre main-Methode auf.

    ```java
    public static void main(String[] args) {
        VideoSDK.VideoSearch("YOUR-SUBSCRIPTION-KEY");
    }
    ```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Single-Page-Webanwendung](../../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Weitere Informationen 

* [Worum handelt es sich bei der Bing-Videosuche-API?](../../overview.md)
* [Cognitive Services SDK-Beispiele für .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
