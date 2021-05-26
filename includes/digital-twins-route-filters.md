---
author: baanders
description: Includedatei für Routingfilteroptionen für Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 12/04/2020
ms.author: baanders
ms.openlocfilehash: 11037d48a3023ec5b63c87902c4cbfcd9d764d85
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110473704"
---
| Filtername | BESCHREIBUNG | Filtertextschema | Unterstützte Werte | 
| --- | --- | --- | --- |
| True/False | Ermöglicht das Erstellen einer Route ohne Filterung oder das Deaktivieren einer Route, sodass keine Ereignisse gesendet werden. | `<true/false>` | `true` = Route ist ohne Filterung aktiviert. <br> `false` = Route ist deaktiviert. |
| type | Der [Ereignistyp](../articles/digital-twins/concepts-route-events.md#types-of-event-messages), der durch Ihre digitale Zwillingsinstanz fließt. | `type = '<event-type>'` | Folgende Werte für Ereignistypen sind möglich: <br>`Microsoft.DigitalTwins.Twin.Create` <br> `Microsoft.DigitalTwins.Twin.Delete` <br> `Microsoft.DigitalTwins.Twin.Update`<br>`Microsoft.DigitalTwins.Relationship.Create`<br>`Microsoft.DigitalTwins.Relationship.Update`<br> `Microsoft.DigitalTwins.Relationship.Delete` <br> `microsoft.iot.telemetry`  |
| `Source` | Der Name der Azure Digital Twins-Instanz. | `source = '<host-name>'`| Dies sind die möglichen Werte für Hostnamen: <br> **Für Benachrichtigungen**: `<your-Digital-Twins-instance>.api.<your-region>.digitaltwins.azure.net` <br> **Für Telemetrie**: `<your-Digital-Twins-instance>.api.<your-region>.digitaltwins.azure.net/<twin-ID>`|
| Subject | Eine Beschreibung des Ereignisses im Zusammenhang mit der oben genannten Ereignisquelle. | `subject = '<subject>'` | Folgende Werte für den Betreff sind möglich: <br>**Für Benachrichtigungen**: Der Betreff ist `<twin-ID>` <br> oder ein URI-Format für Betreffelemente, die durch mehrere Teile oder IDs eindeutig identifiziert werden:<br>`<twin-ID>/relationships/<relationship-ID>`<br> **Für Telemetrie**: Der Betreff ist der Komponentenpfad (wenn die Telemetrie von einer Zwillingskomponente ausgegeben wird). Beispiel: `comp1.comp2`. Wenn die Telemetrie nicht von einer Komponente ausgegeben wird, dann ist ihr Betrefffeld leer. |
| Datenschema | DTDL-Modell-ID | `dataschema = '<model-dtmi-ID>'` | **Für Telemetrie**: Das Datenschema ist die Modell-ID des Zwillings oder der Komponente, der bzw. die die Telemetrie ausgibt. Zum Beispiel, `dtmi:example:com:floor4;2` <br>**Für Benachrichtigungen (Erstellen/Löschen)** : Der Zugriff auf das Datenschema kann im Benachrichtigungstext unter `$body.$metadata.$model` erfolgen. <br>**Für Benachrichtigungen (Aktualisieren)** : Der Zugriff auf das Datenschema kann im Benachrichtigungstext unter `$body.modelId` erfolgen.|
| Inhaltstyp | Der Inhaltstyp des Datenwerts. | `datacontenttype = '<content-type>'` | Der Inhaltstyp lautet `application/json`. |
| Spezifikationsversion | Die Version des von Ihnen verwendeten Ereignisschemas. | `specversion = '<version>'` | Die Version muss `1.0` sein. Dies gibt das CloudEvents-Schema, Version 1.0, an. |
| Benachrichtigungstext | Verweisen auf eine beliebige Eigenschaft im Feld `data` einer Benachrichtigung. | `$body.<property>` | Beispiele für Benachrichtigungen finden Sie unter [Konzepte: Ereignisbenachrichtigungen](../articles/digital-twins/concepts-event-notifications.md). Auf jede Eigenschaft im Feld `data` kann mit `$body` verwiesen werden.

Die folgenden Datentypen werden als Werte unterstützt, die von Verweisen auf die oben aufgeführten Daten zurückgegeben werden:

| Datentyp | Beispiel |
|-|-|-|
|**String**| `STARTS_WITH($body.$metadata.$model, 'dtmi:example:com:floor')` <br> `CONTAINS(subject, '<twin-ID>')`|
|**Integer**|`$body.errorCode > 200`|
|**Double**|`$body.temperature <= 5.5`|
|**Bool**|`$body.poweredOn = true`|
|**NULL**|`$body.prop != null`|

Die folgenden Operatoren werden beim Definieren von Routenfiltern unterstützt:

|Familie|Operatoren|Beispiel|
|-|-|-|
|Logisch|AND, OR, ( )|`(type != 'microsoft.iot.telemetry' OR datacontenttype = 'application/json') OR (specversion != '1.0')`|
|Vergleich|<, <=, >, >=, =, !=|`$body.temperature <= 5.5`

Die folgenden Funktionen werden beim Definieren von Routenfiltern unterstützt:

|Funktion|Beschreibung|Beispiel|
|--|--|--|
|STARTS_WITH(x,y)|Gibt TRUE zurück, wenn der Wert `x` mit der Zeichenfolge `y` beginnt.|`STARTS_WITH($body.$metadata.$model, 'dtmi:example:com:floor')`|
|ENDS_WITH(x,y) | Gibt TRUE zurück, wenn der Wert `x` mit der Zeichenfolge `y` endet.|`ENDS_WITH($body.$metadata.$model, 'floor;1')`|
|CONTAINS(x,y)| Gibt TRUE zurück, wenn der Wert `x` die Zeichenfolge `y` enthält.|`CONTAINS(subject, '<twin-ID>')`|

Wenn Sie einen Filter implementieren oder aktualisieren, kann es einige Minuten dauern, bis sich die Änderung in der Datenpipeline widerspiegelt.