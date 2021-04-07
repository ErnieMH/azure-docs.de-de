---
title: JobConfig.json-Felder von Azure Stream Analytics
description: In diesem Artikel werden die unterstützten Felder für die Datei JobConfig.json von Azure Stream Analytics aufgelistet, die zum Erstellen von Aufträgen in Visual Studio Code verwendet wird.
author: su-jie
ms.author: sujie
ms.service: stream-analytics
ms.topic: how-to
ms.date: 02/14/2020
ms.openlocfilehash: 0eebd0b62e973572a40b7b141ae908046700ba3c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98020484"
---
# <a name="azure-stream-analytics-jobconfigjson-fields"></a>JobConfig.json-Felder von Azure Stream Analytics

Die folgenden Felder werden in der Datei *JobConfig.json* unterstützt, die zum [Erstellen eines Azure Stream Analytics-Auftrags mit Visual Studio Code](quick-create-visual-studio-code.md) verwendet wird.

```json
{
    "DataLocale": "string",
    "OutputErrorPolicy": "string",
    "EventsLateArrivalMaxDelayInSeconds": "integer",
    "EventsOutOfOrderMaxDelayInSeconds": "integer",
    "EventsOutOfOrderPolicy": "string",
    "StreamingUnits": "integer",
    "CompatibilityLevel": "string",
    "UseSystemAssignedIdentity": "boolean",
    "GlobalStorage": {
        "AccountName": "string",
        "AccountKey": "string",
    },
    "DataSourceCredentialDomain": "string",
    "ScriptType": "string",
    "Tags": {}
}
```

|Name|type|Erforderlich|Wert|
|----|----|--------|-----|
|DataLocale|Zeichenfolge|Nein|Das Datengebietsschema des Stream Analytics-Auftrags. Wert sollte der Name eines unterstützten Auftrags sein. Standardeinstellung ist „en-US“, wenn keine Angabe gemacht wurde.|
|OutputErrorPolicy|Zeichenfolge|Nein|Gibt die Richtlinie an, die auf Ereignisse angewendet werden soll, die bei der Ausgabe eintreffen und aufgrund einer fehlerhaften Formatierung (fehlende Spaltenwerte, Spaltenwerte falschen Typs oder falscher Größe) nicht in den externen Speicher geschrieben werden können. - Beenden oder Ablegen|
|EventsLateArrivalMaxDelayInSeconds|integer|Nein|Die maximal akzeptable Verzögerung in Sekunden, bei der verspätet eintreffende Ereignisse mit einbezogen werden könnten. Der unterstützte Bereich ist -1 bis 1814399 (20,23:59:59 Tage) und -1 wird verwendet, um die Wartezeit auf unbestimmte Zeit festzulegen. Wenn die Eigenschaft nicht vorhanden ist, wird sie so interpretiert, dass sie einen Wert von -1 hat.|
|EventsOutOfOrderMaxDelayInSeconds|integer|Nein|Die maximal akzeptable Verzögerung in Sekunden, bei der Störereignisse so angepasst werden können, dass sie wieder normal sind.|
|EventsOutOfOrderPolicy|Zeichenfolge|Nein|Gibt die Richtlinie an, die auf Ereignisse angewendet werden soll, die im Eingangsereignisdatenstrom als Störereignisse ankommen. - Anpassen oder Ablegen|
|StreamingUnits|integer|Ja|Gibt die Anzahl der Streamingeinheiten an, die der Streamingauftrag verwendet.|
|CompatibilityLevel|Zeichenfolge|Nein|Steuert bestimmte Laufzeitverhalten des Streamingauftrags. - Die folgenden Werte sind zulässig: „1.0“, „1.1“, „1.2“|
|UseSystemAssignedIdentity|boolean|Nein|Legen Sie „true“ fest, damit dieser Auftrag unter Verwendung einer verwalteten Azure Active Directory-Identität als er selbst mit anderen Azure-Diensten kommunizieren kann.|
|GlobalStorage.AccountName|Zeichenfolge|Nein|Das globale Speicherkonto wird zum Speichern von Inhalten zu Ihrem Stream Analytics-Auftrag verwendet, z. B. Momentaufnahmen für die SQL-Verweisdaten.|
|GlobalStorage.AccountKey|Zeichenfolge|Nein|Entsprechender Schlüssel für das globale Speicherkonto.|
|DataSourceCredentialDomain|Zeichenfolge|Nein|Reservierte Eigenschaft für die lokale Speicherung von Anmeldeinformationen.|
|ScriptType|Zeichenfolge|Ja|Reservierte Eigenschaft zur Angabe des Typs dieser Quelldatei. Akzeptabler Wert ist „JobConfig“ für „JobConfig.json“.|
|Tags|JSON-Schlüssel-Wert-Paare|Nein|Tags sind Name/Wert-Paare, die Ihnen das Kategorisieren von Ressourcen und die Anzeige einer konsolidierten Abrechnung ermöglichen, indem Sie dasselbe Tag auf mehrere Ressourcen und Ressourcengruppen anwenden. Bei Tagnamen wird nicht zwischen Groß- und Kleinschreibung unterschieden und bei Tagwerten wird die Groß- und Kleinschreibung beachtet.|

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen eines Azure Stream Analytics-Auftrags in Visual Studio Code](quick-create-visual-studio-code.md)
* [Lokales Testen von Stream Analytics-Abfragen mit Beispieldaten mithilfe von Visual Studio Code](visual-studio-code-local-run.md)
* [Lokales Testen von Azure Stream Analytics-Abfragen unter Verwendung einer Livestreameingabe mithilfe von Visual Studio Code](visual-studio-code-local-run-live-input.md)
*[Bereitstellen eines Azure Stream Analytics-Auftrags mit einem CI/CD npm-Paket](./cicd-overview.md)