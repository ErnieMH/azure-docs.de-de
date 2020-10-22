---
title: Azure IoT-Geräte-SDK für C – Serialisierungsprogramm | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Serializer-Bibliothek im Azure IoT-Geräte-SDK für C zum Erstellen von Geräte-Apps, die mit einer IoT Hub-Instanz kommunizieren, verwenden.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: conceptual
ms.date: 09/06/2016
ms.author: robinsh
ms.custom: amqp
ms.openlocfilehash: f52d1d1c5f264550076688d5e25e110de230eff4
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2020
ms.locfileid: "92152226"
---
# <a name="azure-iot-device-sdk-for-c--more-about-serializer"></a>Azure IoT-Geräte-SDK für C – weitere Informationen zum Serialisierungsprogramm

Im ersten Artikel dieser Serie wurde das [Azure IoT-Geräte-SDK für C](iot-hub-device-sdk-c-intro.md) vorgestellt. Im nächsten Artikel erhalten Sie eine ausführlichere Beschreibung zum [Azure IoT-Geräte-SDK für C – IoTHubClient](iot-hub-device-sdk-c-iothubclient.md). Der vorliegende Artikel bietet eine detaillierte Beschreibung der letzten Komponente, der Bibliothek des **Serialisierungsprogramms** , und schließt die Artikelreihe zum SDK ab.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Im Einführungsartikel wurde die Verwendung der Bibliothek des **Serialisierungsprogramms** zum Senden von Ereignissen an und zum Empfangen von Nachrichten von IoT Hub beschrieben. In diesem Artikel wird genauer erläutert, wie Sie Ihre Daten mit der Makrosprache des **Serialisierungsprogramms** modellieren. Im Artikel wird ebenfalls im Detail beschrieben, wie die Serialisierung von Nachrichten durch die Bibliothek funktioniert (und wie Sie das Serialisierungsverhalten in einigen Fällen steuern können). Darüber hinaus werden einige Parameter beschrieben, die Sie ändern können und die die Größe der von Ihnen erstellten Modelle bestimmen.

Zum Abschluss werden einige in vorherigen Artikeln bereits erläuterte Themen erneut aufgegriffen, z.B. die Behandlung von Nachrichten und Eigenschaften. Sie werden feststellen, dass diese Features bei Verwendung der Bibliothek des **Serialisierungsprogramms** genauso funktionieren wie mit der **IoTHubClient**-Bibliothek.

Sämtliche in diesem Artikel beschriebenen Elemente basieren auf den Beispielen für das SDK des **Serialisierungsprogramms** . Wenn Sie die Beschreibungen in diesem Artikel nachvollziehen möchten, sehen Sie sich die Anwendungen **simplesample\_amqp** und **simplesample\_http** an, die im Azure IoT-Geräte-SDK für C enthalten sind.

Das [**Azure IoT-Geräte-SDK für C**](https://github.com/Azure/azure-iot-sdk-c) finden Sie im GitHub-Repository und ausführliche Informationen zur API in der [C-API-Referenz](/azure/iot-hub/iot-c-sdk-ref/).

## <a name="the-modeling-language"></a>Die Modelliersprache

Im Artikel zum [Azure IoT-Geräte-SDK für C](iot-hub-device-sdk-c-intro.md) dieser Serie wurde anhand des in der Anwendung **simplesample\_amqp** bereitgestellten Beispiels die Modelliersprache des **Azure IoT-Geräte-SDK für C** vorgestellt:

```C
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(double, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

Wie Sie sehen, basiert die Modelliersprache auf in C geschriebenen Makros. Die Definition beginnt immer mit **BEGIN\_NAMESPACE** und endet immer mit **END\_NAMESPACE**. Es ist üblich, den Namespace nach Ihrem Unternehmen bzw. wie in diesem Fall nach dem Projekt zu benennen, an dem Sie gerade arbeiten.

Im Namespace werden Modelldefinitionen gespeichert. In diesem Fall handelt es sich um ein einzelnes Modell für einen Windgeschwindigkeitsmesser. Dem Modell kann ein beliebiger Name zugewiesen werden, üblicherweise erfolgt die Benennung jedoch gemäß dem Gerät oder der Art von Daten, die mit IoT Hub ausgetauscht werden sollen.  

Modelle umfassen eine Definition der Ereignisse, die Sie als Eingangsereignisse an IoT Hub senden (die *Daten*), sowie die Nachrichten, die Sie von IoT Hub empfangen können (die *Aktionen*). Wie Sie in diesem Beispiel sehen, verfügen Ereignisse über einen Typ und einen Namen. Aktionen besitzen einen Namen sowie optionale Parameter (von denen jeder einen bestimmten Typ aufweist).

In diesem Beispiel werden keine weiteren Datentypen gezeigt, die vom SDK unterstützt werden. Darum wird es im Anschluss gehen.

> [!NOTE]
> Bei IoT Hub werden die Daten, die ein Gerät an den Hub sendet, als *Ereignisse* bezeichnet. In der Modelliersprache heißen sie dagegen *Daten* und werden mithilfe von **WITH_DATA** definiert. Analog dazu gilt: An Geräte gesendete Daten werden bei IoT Hub als *Nachrichten* bezeichnet. In der Modelliersprache heißen sie dagegen *Aktionen* und werden mithilfe von **WITH_ACTION** definiert. Denken Sie daran, dass diese Begriffe in diesem Artikel ggf. synonym verwendet werden.
> 
> 

## <a name="supported-data-types"></a>Unterstützte Datentypen

Die folgenden Datentypen werden in Modellen unterstützt, die mit der Bibliothek des **Serialisierungsprogramms** erstellt wurden:

| type | BESCHREIBUNG |
| --- | --- |
| double |Gleitkommazahl mit doppelter Genauigkeit |
| INT |32-bit integer |
| float |Gleitkommazahl mit einfacher Genauigkeit |
| long |Lange ganze Zahl |
| int8\_t |8-Bit-Ganzzahl |
| int16\_t |16-Bit-Ganzzahl |
| int32\_t |32-bit integer |
| int64\_t |64-bit integer |
| bool |boolean |
| ascii\_char\_ptr |ASCII-Zeichenfolge |
| EDM\_DATE\_TIME\_OFFSET |Datums-/Uhrzeit-Abweichung |
| EDM\_GUID |GUID |
| EDM\_BINARY |BINARY |
| DECLARE\_STRUCT |Komplexer Datentyp |

Beginnen wir mit dem letzten Datentyp. Mit **DECLARE\_STRUCT** können Sie komplexe Datentypen definieren, die Gruppierungen der anderen einfachen Datentypen darstellen. Mit diesen Gruppierungen können Sie ein Modell definieren, das folgendermaßen aussieht:

```C
DECLARE_STRUCT(TestType,
double, aDouble,
int, aInt,
float, aFloat,
long, aLong,
int8_t, aInt8,
uint8_t, auInt8,
int16_t, aInt16,
int32_t, aInt32,
int64_t, aInt64,
bool, aBool,
ascii_char_ptr, aAsciiCharPtr,
EDM_DATE_TIME_OFFSET, aDateTimeOffset,
EDM_GUID, aGuid,
EDM_BINARY, aBinary
);

DECLARE_MODEL(TestModel,
WITH_DATA(TestType, Test)
);
```

Unser Modell enthält ein einzelnes Datenereignis vom Typ **TestType**. Bei **TestType** handelt es sich um einen komplexen Typ, der mehrere Elemente enthält, die zusammengenommen die einfachen Datentypen darstellen, die von der Modelliersprache des **Serialisierungsprogramms** unterstützt werden.

Mit einem solchen Modell können wir Code schreiben, um Daten an IoT Hub zu senden, der folgendermaßen aussieht:

```C
TestModel* testModel = CREATE_MODEL_INSTANCE(MyThermostat, TestModel);

testModel->Test.aDouble = 1.1;
testModel->Test.aInt = 2;
testModel->Test.aFloat = 3.0f;
testModel->Test.aLong = 4;
testModel->Test.aInt8 = 5;
testModel->Test.auInt8 = 6;
testModel->Test.aInt16 = 7;
testModel->Test.aInt32 = 8;
testModel->Test.aInt64 = 9;
testModel->Test.aBool = true;
testModel->Test.aAsciiCharPtr = "ascii string 1";

time_t now;
time(&now);
testModel->Test.aDateTimeOffset = GetDateTimeOffset(now);

EDM_GUID guid = { { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x0F } };
testModel->Test.aGuid = guid;

unsigned char binaryArray[3] = { 0x01, 0x02, 0x03 };
EDM_BINARY binaryData = { sizeof(binaryArray), &binaryArray };
testModel->Test.aBinary = binaryData;

SendAsync(iotHubClientHandle, (const void*)&(testModel->Test));
```

Im Wesentlichen weisen wir jedem Element der **Test**-Struktur einen Wert zu und rufen **SendAsync** auf, um das **Test**-Datenereignis an die Cloud zu senden. **SendAsync** ist eine Hilfsfunktion, die ein einzelnes Datenereignis an IoT Hub sendet:

```C
void SendAsync(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const void *dataEvent)
{
    unsigned char* destination;
    size_t destinationSize;
    if (SERIALIZE(&destination, &destinationSize, *(const unsigned char*)dataEvent) ==
    {
        // null terminate the string
        char* destinationAsString = (char*)malloc(destinationSize + 1);
        if (destinationAsString != NULL)
        {
            memcpy(destinationAsString, destination, destinationSize);
            destinationAsString[destinationSize] = '\0';
            IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromString(destinationAsString);
            if (messageHandle != NULL)
            {
                IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)0);

                IoTHubMessage_Destroy(messageHandle);
            }
            free(destinationAsString);
        }
        free(destination);
    }
}
```

Die Funktion serialisiert das angegebene Datenereignis und sendet es unter Verwendung von **IoTHubClient\_SendEventAsync** an IoT Hub. Hierbei handelt es sich um den gleichen Code, der auch schon in vorherigen Artikeln erläutert wurde. (**SendAsync** kapselt die Logik in eine praktische Funktion.)

Eine weitere im vorherigen Codebeispiel verwendete Hilfsfunktion ist **GetDateTimeOffset**. Diese Funktion wandelt die angegebene Uhrzeit in einen Wert vom Typ **EDM\_DATE\_TIME\_OFFSET** um:

```C
EDM_DATE_TIME_OFFSET GetDateTimeOffset(time_t time)
{
    struct tm newTime;
    gmtime_s(&newTime, &time);
    EDM_DATE_TIME_OFFSET dateTimeOffset;
    dateTimeOffset.dateTime = newTime;
    dateTimeOffset.fractionalSecond = 0;
    dateTimeOffset.hasFractionalSecond = 0;
    dateTimeOffset.hasTimeZone = 0;
    dateTimeOffset.timeZoneHour = 0;
    dateTimeOffset.timeZoneMinute = 0;
    return dateTimeOffset;
}
```

Bei Ausführung dieses Codes wird folgende Nachricht an IoT Hub gesendet:

```C
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

Beachten Sie, dass die Serialisierung in JSON erfolgt. Hierbei handelt es sich um das Format, das von der Bibliothek des **Serialisierungsmoduls** generiert wird. Beachten Sie auch, dass jedes Element des serialisierten JSON-Objekts den Elementen des in unserem Modell definierten **TestType**-Ereignisses entspricht. Die Werte entsprechen ebenfalls exakt den im Code verwendeten Werten. Beachten Sie jedoch, dass die Binärdaten base64-codiert sind: „AQID“ ist die Base64-Codierung von {0x01, 0x02, 0x03}.

Dieses Beispiel veranschaulicht den Vorteil der Bibliothek des **Serialisierungsprogramms** : Mit dieser Bibliothek können wir JSON-Daten an die Cloud senden, ohne uns explizit mit der Serialisierung in unserer Anwendung beschäftigen zu müssen. Wir müssen nur die Werte der Datenereignisse in unserem Modell festlegen und dann einfach APIs aufrufen, um diese Ereignisse an die Cloud zu senden.

Mit diesen Informationen können wir Modelle definieren, die alle unterstützten Datentypen umfassen, einschließlich der komplexen Typen (wir könnten auch komplexe Typen innerhalb anderer komplexer Typen verwenden). Das durch das Beispiel oben generierte serialisierte JSON-Format zeigt jedoch einen wichtigen Aspekt. *wie* wir Daten über die Bibliothek des **Serialisierungsprogramms** senden, bestimmt genau, wie die JSON-Daten geformt sind. Um diesen Aspekt geht es im nächsten Abschnitt.

## <a name="more-about-serialization"></a>Weitere Informationen zur Serialisierung

Der vorherige Abschnitt zeigte ein Beispiel der Ausgabe, die durch die Bibliothek des **Serialisierungsprogramms** generiert wurde. In diesem Abschnitt wird erläutert, wie die Bibliothek Daten serialisiert und wie Sie dieses Verhalten mithilfe der Serialisierungs-APIs steuern können.

Um die Serialisierung besser erläutern können, arbeiten wir mit einem neuen Modell basierend auf einem Thermostat. Zunächst erhalten Sie einige Hintergrundinformationen zum gewünschten Szenario.

Wir möchten ein Thermostat modellieren, das Temperatur und Luftfeuchtigkeit misst. Jedes Datenelement soll auf unterschiedliche Weise an IoT Hub gesendet werden. Standardmäßig geht alle 2 Minuten ein Temperaturereignis und alle 15 Minuten ein Luftfeuchtigkeitsereignis beim Thermostat ein. Beim Eingang jedes Ereignisses muss dieses einen Zeitstempel erhalten, der die Uhrzeit angibt, zu der die entsprechende Temperatur oder Luftfeuchtigkeit gemessen wurde.

Anhand dieses Szenarios zeigen wir Ihnen zwei verschiedene Möglichkeiten, die Daten zu modellieren. Zudem wird erläutert, wie sich die Modellierung auf die serialisierte Ausgabe auswirkt.

### <a name="model-1"></a>Modell 1

Hier sehen Sie die erste Version eines Modells, das das vorherige Szenario unterstützt:

```C
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(TemperatureEvent,
int, Temperature,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_STRUCT(HumidityEvent,
int, Humidity,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureEvent, Temperature),
WITH_DATA(HumidityEvent, Humidity)
);

END_NAMESPACE(Contoso);
```

Beachten Sie, dass das Modell zwei Datenereignisse enthält: **Temperature** (Temperatur) und **Humidity** (Luftfeuchtigkeit). Im Gegensatz zu den vorherigen Beispielen ist der Typ jedes Ereignisses eine Struktur, die mithilfe von **DECLARE\_STRUCT** definiert wird. **TemperatureEvent** enthält eine Temperaturmessung und einen Zeitstempel, **HumidityEven**t enthält eine Luftfeuchtigkeitsmessung und einen Zeitstempel. Dieses Modell stellt eine natürliche Möglichkeit bereit, die Daten für das oben beschriebene Szenario zu modellieren. Beim Senden eines Ereignisses an die Cloud wird entweder ein Temperatur/Zeitstempel-Paar oder ein Luftfeuchtigkeit/Zeitstempel-Paar gesendet.

Wir können ein Temperaturereignis mithilfe von folgendem Code an die Cloud senden:

```C
time_t now;
time(&now);
thermostat->Temperature.Temperature = 75;
thermostat->Temperature.Time = GetDateTimeOffset(now);

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Im Beispielcode wurden hartcodierte Werte für Temperatur und Luftfeuchtigkeit verwendet. In der Praxis werden diese Werte durch Abtasten der entsprechenden Sensoren im Thermostat abgerufen.

In diesem Code wird die weiter oben vorgestellte Hilfsfunktion **GetDateTimeOffset** verwendet. Aus Gründen, die weiter unten im vorliegenden Artikel klar werden, trennt dieser Code explizit die Serialisierung und das Senden des Ereignisses. Der vorherige Code serialisiert das Temperaturereignis in einen Puffer. Anschließend sendet die Hilfsfunktion **sendMessage** (enthalten in **simplesample\_amqp**) das Ereignis an IoT Hub:

```C
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId);

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
}
```

Dieser Code ist eine Teilmenge der Hilfsfunktion **SendAsync**, die im vorherigen Abschnitt beschrieben wurde, daher werden wir an dieser Stelle nicht erneut darauf eingehen.

Bei Ausführung des oben stehenden Codes zum Senden des Temperaturereignisses wird die folgende serialisierte Form des Ereignisses an IoT Hub gesendet:

```C
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Wir senden eine Temperatur vom Typ **TemperatureEvent**, und diese Struktur enthält ein Element vom Typ **Temperature** und ein Element vom Typ **Time**. Diese Tatsache wird direkt in den serialisierten Daten reflektiert.

Ebenso können wir mit folgendem Code ein Luftfeuchtigkeitsereignis senden:

```C
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Die serialisierte Form, die an IoT Hub gesendet wird, sieht folgendermaßen aus:

```C
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Auch hier keine Überraschungen.

Bei diesem Modell können Sie sich vorstellen, dass weitere Ereignisse problemlos hinzugefügt werden können. Sie definieren weitere Strukturen mit **DECLARE\_STRUCT** und nehmen das entsprechende Ereignis mit **WITH\_DATA** in das Modell auf.

Nun soll das Modell so verändert werden, dass es die gleichen Daten enthält, jedoch mit einer anderen Struktur.

### <a name="model-2"></a>Modell 2

Betrachten Sie das folgende Modell als Alternative zum oben gezeigten:

```C
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

In diesem Fall haben wir die **DECLARE\_STRUCT**-Makros entfernt und definieren ganz einfach die Datenelemente aus unserem Szenario mithilfe von einfachen Datentypen der Modelliersprache.

Ignorieren Sie für den Moment das **Time**-Ereignis. Hier finden Sie den Code für eingehende **Temperature**-Ereignisse:

```C
time_t now;
time(&now);
thermostat->Temperature = 75;

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Dieser Code sendet das folgende serialisierte Ereignis an IoT Hub:

```C
{"Temperature":75}
```

Der Code zum Senden des Luftfeuchtigkeitsereignisses sieht folgendermaßen aus:

```C
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Dieser Code sendet Folgendes an IoT Hub:

```C
{"Humidity":45}
```

Bis hierher verläuft noch immer alles wie erwartet. Jetzt ändern wir die Verwendung des SERIALIZE-Makros.

Das **SERIALIZE**-Makro kann mehrere Datenereignisse als Argumente aufnehmen. So können wir die Ereignisse **Temperature** und **Humidity** gemeinsam serialisieren und in einem einzelnen Aufruf an IoT Hub senden:

```C
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Nun könnte man denken, dass mit diesem Code zwei Datenereignisse an IoT Hub gesendet werden:

[ {"Temperature":75}, {"Humidity":45} ]

Anders ausgedrückt: Sie könnten erwarten, dass dieser Code dem getrennten Senden von **Temperature** und **Humidity** entspricht. Es ist lediglich eine Vereinfachung, beide Ereignisse im selben Aufruf an **SERIALIZE** zu übergeben. Dies ist jedoch nicht der Fall. Stattdessen sendet der obige Code das folgende einzelne Datenereignis an IoT Hub:

{"Temperature":75, "Humidity":45}

Das mag seltsam erscheinen, da unser Modell **Temperature** und **Humidity** als zwei *separate* Ereignisse definiert:

```C
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Genauer gesagt: wir haben nicht diese Ereignisse modelliert, in denen sich **Temperature** und **Humidity** in der gleichen Struktur befinden:

```C
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

Hätten wir dieses Modell verwendet, wäre es leichter zu verstehen, wie **Temperature** und **Humidity** in der gleichen serialisierten Nachricht gesendet werden. Möglicherweise ist nicht klar, warum das so funktioniert, wenn Sie beide Datenereignisse mithilfe von Modell 2 an **SERIALIZE** übergeben.

Dieses Verhalten ist einfacher zu verstehen, wenn Sie die Annahmen kennen, von denen die Bibliothek des **Serialisierungsprogramms** ausgeht. Um dies zu verstehen, kehren wir zunächst zu unserem Modell zurück:

```C
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Betrachten Sie dieses Modell aus der objektorientierten Perspektive. In diesem Fall modellieren wir ein physisches Gerät (Thermostat), das Attribute wie **Temperature** und **Humidity** enthält.

Wir können mit folgendem Code den gesamten Status des Modells senden:

```C
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Vorausgesetzt, dass die Werte für Temperatur, Luftfeuchtigkeit und Uhrzeit festgelegt wurden, wird ein Ereignis ähnlich dem folgenden an IoT Hub gesendet:

```C
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Mitunter sollen jedoch nur *einige* Eigenschaften des Modells an die Cloud gesendet werden. (Dies gilt insbesondere, wenn das Modell eine große Anzahl von Datenereignissen umfasst.) Es ist nützlich, nur eine Teilmenge der Datenereignisse zu senden, so wie im Beispiel weiter oben:

```C
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Damit wird exakt das gleiche serialisierte Ereignis generiert wie beim Definieren von **TemperatureEven**t mit **Temperature** und **Time** (siehe Modell 1). In diesem Fall konnten wir mithilfe eines anderen Modells (Modell 2) exakt dasselbe serialisierte Ereignis generieren, weil wir **SERIALIZE** auf andere Weise aufgerufen haben.

Hierbei ist Folgendes entscheidend: Wenn Sie mehrere Datenereignisse an **SERIALIZE** übergeben, wird davon ausgegangen, dass jedes Ereignis eine Eigenschaft in einem einzelnen JSON-Objekt ist.

Die beste Methode hängt von Ihnen und Ihrer Meinung über Ihr Modell ab. Wenn Sie „Ereignisse“ an die Cloud senden und jedes Ereignis einen definierten Satz von Eigenschaften umfasst, ist die erste Methode für Sie sinnvoll. In diesem Fall verwenden Sie **DECLARE\_STRUCT**, um die Struktur jedes Ereignisses zu definieren, und schließen die Ereignisse dann mit dem **WITH\_DATA**-Makro in Ihr Modell ein. Anschließend senden Sie jedes Ereignis auf die gleiche Weise wie im ersten oben gezeigten Beispiel. Bei dieser Methode würden Sie nur ein einzelnes Datenereignis an **SERIALIZER** übergeben.

Wenn Sie Ihr Modell eher aus objektorientierter Perspektive betrachten, eignet sich die zweite Methode wahrscheinlich besser für Sie. In diesem Fall stellen die Elemente, die Sie mithilfe von **WITH\_DATA** definieren, die Eigenschaften Ihres Objekts dar. Sie können eine beliebige Teilmenge von Ereignissen an **SERIALIZE** übergeben – je nachdem, welche Informationen zum Objektzustand Sie an die Cloud senden möchten.

Keine dieser Methoden ist richtig oder falsch. Machen Sie sich bewusst, wie die Bibliothek des **Serialisierungsprogramms** funktioniert, und wählen Sie die Modellierungsmethode aus, die Ihren Anforderungen am besten entspricht.

## <a name="message-handling"></a>Behandlung von Nachrichten

Bisher ging es in diesem Artikel nur um das Senden von Ereignissen an IoT Hub, nicht um das Empfangen von Nachrichten. Das liegt daran, dass der Nachrichtenempfang bereits sehr ausführlich im Artikel [Azure IoT-Geräte-SDK für C](iot-hub-device-sdk-c-intro.md) erläutert wurde. Sie erinnern sich aus diesem Artikel sicher, dass Sie Nachrichten verarbeiten, indem Sie eine Nachrichtenrückruffunktion registrieren:

```C
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Anschließend schreiben Sie die Rückruffunktion, die beim Empfang einer Nachricht aufgerufen wird:

```C
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
            result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
            memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

Durch diese Implementierung von **IoTHubMessage** wird für jede Aktion in Ihrem Modell die dafür bestimmte Funktion aufgerufen. Ein Beispiel – Ihr Modell definiert folgende Aktion:

```C
WITH_ACTION(SetAirResistance, int, Position)
```

In diesem Fall müssen Sie eine Funktion mit folgender Signatur definieren:

```C
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

**SetAirResistance** wird dann aufgerufen, wenn diese Nachricht an Ihr Gerät gesendet wird.

Bisher wurde noch nicht erläutert, wie die serialisierte Version einer Nachricht aussieht. Anders gesagt: wenn Sie eine **SetAirResistance** -Nachricht an Ihr Gerät senden, wie sieht diese aus?

Zum Senden einer Nachricht an ein Gerät verwenden Sie das Azure IoT-Dienst-SDK. Sie müssen aber wissen, welche Zeichenfolge gesendet werden muss, um eine bestimmte Aktion aufzurufen. Das allgemeine Format zum Senden einer Nachricht sieht folgendermaßen aus:

```C
{"Name" : "", "Parameters" : "" }
```

Sie senden ein serialisiertes JSON-Objekt mit zwei Eigenschaften: **Name** ist der Name der Aktion (Nachricht) und **Parameters** enthält die Parameter dieser Aktion.

Um beispielsweise **SetAirResistance** aufzurufen, können Sie folgende Nachricht an Ihr Gerät senden:

```C
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

Der Name der Aktion muss genau mit einer in Ihrem Modell definierten Aktion übereinstimmen. Die Parameternamen müssen ebenfalls übereinstimmen. Beachten Sie auch die Groß-/Kleinschreibung. **Name** und **Parameters** werden immer mit Großbuchstaben geschrieben. Achten Sie auf die richtige Groß-/Kleinschreibung beim Namen der Aktion und bei den Parametern in Ihrem Modell. In diesem Beispiel lautet der Name der Aktion „SetAirResistance“, nicht „setairresistance“.

Die beiden anderen Aktionen, **TurnFanOn** und **TurnFanOff**, können durch Senden dieser Nachrichten an ein Gerät aufgerufen werden:

```C
{"Name" : "TurnFanOn", "Parameters" : {}}
{"Name" : "TurnFanOff", "Parameters" : {}}
```

In diesem Abschnitt wurde alles beschrieben, was Sie über das Senden von Ereignissen und das Empfangen von Nachrichten mit der Bibliothek des **Serialisierungsprogramms** wissen müssen. Bevor wir fortfahren, möchten wir Ihnen einige Parameter erläutern, die Sie konfigurieren können, um die Größe Ihres Modells zu steuern.

## <a name="macro-configuration"></a>Konfiguration von Makros

Wenn Sie die Bibliothek des **Serialisierungsprogramms** verwenden, finden Sie einen wichtigen Teil des SDKs in der Bibliothek „azure-c-shared-utility“.

Wenn Sie das Repository „Azure-iot-sdk-c“ auf GitHub geklont und den Befehl `git submodule update --init` ausgegeben haben, finden Sie diese freigegebene Hilfsprogrammbibliothek hier:

```C
.\\c-utility
```

Wenn Sie die Bibliothek nicht geklont haben, finden Sie sie [hier](https://github.com/Azure/azure-c-shared-utility).

In der freigegebenen Hilfsprogrammbibliothek finden Sie den folgenden Ordner:

```C
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

Dieser Ordner enthält eine Visual Studio-Projektmappe mit der Bezeichnung **macro\_utils\_h\_generator.sln**:

  ![Screenshot der Visual Studio-Projektmappe maco_utils_h_generator](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.png)

Das Programm in dieser Projektmappe generiert die Datei **macro\_utils.h**. Das SDK enthält standardmäßig die Datei „macro\_utils.h“. In dieser Projektmappe können Sie einige Parameter bearbeiten und dann die Headerdatei basierend auf diesen Parametern erneut erstellen.

Die beiden wichtigsten Parameter sind **nArithmetic** und **nMacroParameters**, die in den folgenden beiden Zeilen in „macro\_utils.tt“ definiert werden:

```C
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>
```

Bei diesen Werten handelt es sich um die Standardparameter, die im SDK enthalten sind. Die Parameter haben folgende Bedeutung:

* nMacroParameters: Steuert, wie viele Parameter in einer einzigen DECLARE\_MODEL-Makrodefinition vorhanden sein dürfen.
* nArithmetic: Steuert die Gesamtanzahl von Elementen, die in einem Modell zulässig sind.

Diese Parameter sind deshalb so wichtig, weil sie steuern, wie groß Ihr Modell sein darf. Sehen Sie sich z. B. folgende Modelldefinition an:

```C
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

Wie bereits erwähnt, handelt es sich bei **DECLARE\_MODEL** nur um ein C-Makro. Die Namen des Modells und die **WITH\_DATA**-Anweisung (derzeit noch ein anderes Makro) sind Parameter von **DECLARE\_MODEL**. **nMacroParameters** bestimmt, wie viele Parameter in **DECLARE\_MODEL** einbezogen werden können. Dies definiert, wie viele Datenereignis- und Aktionsdeklarationen Sie verwenden können. Der Standardgrenzwert liegt bei 124. Sie können also ein Modell mit einer Kombination aus etwa 60 Aktionen und Datenereignissen definieren. Wenn dieser Grenzwert überschritten wird, erhalten Sie Compilerfehler, die wie folgt aussehen:

  ![Screenshot von Compilerfehlern durch die Makroparameter](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.png)

Der **nArithmetic** -Parameter ist mehr für die interne Funktionsweise der Makrosprache und weniger für Ihre Anwendung relevant.  Er steuert die Gesamtanzahl von Elementen, die in Ihrem Modell vorhanden sein können (einschließlich **DECLARE_STRUCT**-Makros). Wenn Sie Compilerfehler wie den folgenden erhalten, versuchen Sie, **nArithmetic**zu erhöhen:

   ![Screenshot von arithmetischen Compilerfehlern](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.png)

Wenn Sie diese Parameter ändern möchten, bearbeiten Sie die Werte in der Datei „macro\_utils.tt“, kompilieren Sie die Projektmappe „macro\_utils\_h\_generator.sln“ neu, und führen Sie das kompilierte Programm aus. In diesem Fall wird eine neue Datei „macro\_utils.h“ generiert und im Verzeichnis „.\\common\\inc“ abgelegt.

Um die neue Version von „macro\_utils.h“ zu verwenden, entfernen Sie das NuGet-Paket des **Serialisierungsprogramms** aus Ihrer Projektmappe, und binden Sie stattdessen das Visual Studio-Projekt des **Serialisierungsprogramms** ein. Dadurch kann der Code mit dem Quellcode der Bibliothek des Serialisierungsprogramms kompiliert werden. Dies umfasst die aktualisierte Datei „macro\_utils.h“. Wenn Sie dies für **simplesample\_amqp** durchführen möchten, entfernen Sie zuerst das NuGet-Paket für die Bibliothek des Serialisierungsprogramms aus der Projektmappe:

   ![Screenshot des Entfernens des NuGet-Pakets für die Serialisierungsmodulbibliothek](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.png)

Fügen Sie dann dieses Projekt Ihrer Visual Studio-Projektmappe hinzu:

> .\\c\\serializer\\build\\windows\\serializer.vcxproj
> 
> 

Wenn Sie hiermit fertig sind, sollte Ihre Projektmappe wie folgt aussehen:

   ![Screenshot der Visual Studio-Projektmappe simplesample_amqp](media/iot-hub-device-sdk-c-serializer/05-serializer-project.png)

Wenn Sie die Projektmappe jetzt kompilieren, wird die aktualisierte Datei „macro\_utils.h“ in Ihre Binärdaten eingefügt.

Beachten Sie, dass durch eine ausreichende Erhöhung dieses Werts die Grenzwerte des Compilers überschritten werden können. An diesem Punkt ist **nMacroParameters** der entscheidende Parameter. Die Spezifikation C99 gibt an, dass in einer Makrodefinition mindestens 127 Parameter zulässig sind. Da der Microsoft-Compiler diese Spezifikation exakt einhält (und einen Grenzwert von 127 besitzt), können Sie **nMacroParameters** nicht auf einen höheren Grenzwert festlegen. Bei anderen Compilern ist dies möglicherweise zulässig (der GNU-Compiler beispielsweise unterstützt einen höheren Grenzwert).

In diesem Artikel haben wir so ziemlich alle Informationen zusammengestellt, die Sie benötigen, um Code mit der Bibliothek des **Serialisierungsprogramms** zu schreiben. Bevor wir zum Abschluss kommen, möchten wir noch einige Themen aus vorherigen Artikeln aufgreifen, bei denen möglicherweise Fragen offen geblieben sind.

## <a name="the-lower-level-apis"></a>Die Low-Level-APIs
In diesem Artikel stand die Beispielanwendung **simplesample\_amqp** im Mittelpunkt. In diesem Beispiel werden die APIs höherer Ebene (ohne **LL**) verwendet, um Ereignisse zu senden und Nachrichten zu empfangen. Wenn Sie diese APIs verwenden, wird ein Hintergrundthread ausgeführt, der sowohl Ereignisse sendet als auch Nachrichten empfängt. Bei Bedarf können die Low-Level-APIs (LL) verwendet werden, um das Senden von Ereignissen an die Cloud und das Empfangen von Nachrichten aus der Cloud explizit und ohne diesen Hintergrundthread zu steuern.

Wie bereits in einem [vorherigen Artikel](iot-hub-device-sdk-c-iothubclient.md)beschrieben, gibt es eine Gruppe von Funktionen, die aus den APIs auf höherer Ebene bestehen:

* IoTHubClient\_CreateFromConnectionString
* IoTHubClient\_SendEventAsync
* IoTHubClient\_SetMessageCallback
* IoTHubClient\_Destroy

Diese APIs werden in **simplesample\_amqp** gezeigt.

Es gibt auch eine analoge Gruppe von Low-Level-APIs.

* IoTHubClient\_LL\_CreateFromConnectionString
* IoTHubClient\_LL\_SendEventAsync
* IoTHubClient\_LL\_SetMessageCallback
* IoTHubClient\_LL\_Destroy

Beachten Sie, dass die Low-Level-APIs genauso funktionieren, wie in den vorherigen Artikeln beschrieben. Sie können die erste Gruppe von APIs verwenden, wenn Sie einen Hintergrundthread zum Senden von Ereignissen und Empfangen von Nachrichten einrichten möchten. Wenn Sie genau steuern möchten, wie Daten an IoT Hub gesendet und von IoT Hub empfangen werden, verwenden Sie die zweite API-Gruppe. Beide API-Gruppen funktionieren mit der Bibliothek des **Serialisierungsprogramms** gleichermaßen gut.

Ein Beispiel für die Verwendung der APIs auf niedrigerer Ebene mit der Bibliothek des **Serialisierungsprogramms** finden Sie in der Anwendung **simplesample\_http**.

## <a name="additional-topics"></a>Weitere Themen
Einige weitere erwähnenswerte Themen sind die Behandlung von Eigenschaften, die Verwendung von alternativen Geräteanmeldedaten sowie Konfigurationsoptionen. All diese Themen werden in einem [vorherigen Artikel](iot-hub-device-sdk-c-iothubclient.md)erläutert. Das Entscheidende ist, dass diese Features mit der Bibliothek des **Serialisierungsprogramms** genauso funktionieren wie mit der **IoTHubClient**-Bibliothek. Wenn Sie beispielsweise Eigenschaften aus dem Modell an ein Ereignis anfügen möchten, verwenden Sie dafür **IoTHubMessage\_Properties** und **Map**\_**AddorUpdate** wie zuvor beschrieben:

```C
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Dabei spielt es keine Rolle, ob das Ereignis über die Bibliothek des **Serialisierungsprogramm**s generiert oder manuell unter Verwendung der **IoTHubClient**-Bibliothek erstellt wurde.

Was die Verwendung alternativer Geräteanmeldeinformationen angeht: Ein **IOTHUB\_CLIENT\_HANDLE** lässt sich mit **IoTHubClient\_LL\_Create** ebenso gut zuordnen wie mit **IoTHubClient\_CreateFromConnectionString**.

Und schließlich können Sie Konfigurationsoptionen bei Verwendung der Bibliothek des **Serialisierungsprogramms** mit **IoTHubClient\_LL\_SetOption** auf die gleiche Weise festlegen wie bei Verwendung der **IoTHubClient**-Bibliothek.

Ein Feature, das nur in der Bibliothek des **Serialisierungsprogramms** zur Verfügung steht, sind die Initialisierungs-APIs. Um die Bibliothek verwenden zu können, müssen Sie zuerst **serializer\_init** aufrufen:

```C
serializer_init(NULL);
```

Dieser Aufruf erfolgt direkt vor dem Aufruf von **IoTHubClient\_CreateFromConnectionString**.

Wenn Sie die Arbeit in der Bibliothek abgeschlossen haben, müssen Sie als Letztes **serializer\_deinit** aufrufen:

```C
serializer_deinit();
```

Abgesehen davon funktionieren alle oben aufgeführten Features in der Bibliothek des **Serialisierungsprogramms** genauso wie in der **IoTHubClient**-Bibliothek. Weitere Informationen zu diesen Themen finden Sie im [vorherigen Artikel](iot-hub-device-sdk-c-iothubclient.md) dieser Serie.

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel beschreibt detailliert die besonderen Aspekte der im **Azure IoT-Geräte-SDK für C** enthaltenen Bibliothek des **Serialisierungsprogramms**. Der Artikel stellt alle Informationen bereit, die Sie benötigen, um mithilfe von Modellen Ereignisse an IoT Hub senden und Nachrichten an IoT Hub empfangen zu können.

Dies ist zugleich der Abschluss der dreiteiligen Artikelreihe zur Entwicklung von Anwendungen mit dem **Azure IoT-Geräte-SDK für C**. Diese Informationen sollten nicht nur für den Einstieg genügen, sondern vermitteln auch tiefgreifende Kenntnisse zur Funktionsweise der APIs. Für weitere Informationen stehen einige Beispiele im SDK zur Verfügung, die hier nicht behandelt werden. Darüber hinaus bietet sich die [Azure IoT SDK-Dokumentation](https://github.com/Azure/azure-iot-sdk-c) als weitere Informationsquelle an.

Weitere Informationen zum Entwickeln für IoT Hub finden Sie im Artikel über die [Azure IoT SDKs](iot-hub-devguide-sdks.md).

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter [Bereitstellen von KI auf Edgegeräten mit Azure IoT Edge](../iot-edge/quickstart-linux.md).