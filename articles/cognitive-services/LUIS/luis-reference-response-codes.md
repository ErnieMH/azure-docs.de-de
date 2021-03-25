---
title: API – HTTP-Antwortcodes – LUIS
titleSuffix: Azure Cognitive Services
description: Es wird beschrieben, welche HTTP-Antwortcodes über die Erstellungs- und Endpunkt-APIs von LUIS zurückgegeben werden.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 03/04/2019
ms.openlocfilehash: 46a7fff86d3a8c4539b77a0f271179d68e7a26d2
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91541678"
---
# <a name="common-api-response-codes-and-their-meaning"></a>Allgemeine API-Antwortcodes und ihre Bedeutung

Die APIs für die Erstellung ([authoring](https://go.microsoft.com/fwlink/?linkid=2092087)) und für Endpunkte ([endpoint](https://go.microsoft.com/fwlink/?linkid=2092356)) geben HTTP-Antwortcodes zurück. Antwortnachrichten enthalten spezifische Informationen zu einer Anforderung, aber es wird ein allgemeiner HTTP-Antwortstatuscode verwendet.

## <a name="common-status-codes"></a>Häufige Statuscodes
In der folgenden Tabelle sind einige der häufigsten HTTP-Antwortstatuscodes für die [Erstellungs-](https://go.microsoft.com/fwlink/?linkid=2092087) und [Endpunkt](https://go.microsoft.com/fwlink/?linkid=2092356)-APIs aufgeführt:

|Code|API|Erklärung|
|:--|--|--|
|400|Erstellung, Endpunkt|Die Parameter der Anforderung sind fehlerhaft, da die erforderlichen Parameter nicht vorhanden, falsch formatiert oder zu groß sind|
|400|Erstellung, Endpunkt|Der Text der Anforderung ist fehlerhaft, da der JSON-Code nicht vorhanden, falsch formatiert oder zu groß ist|
|401|Erstellen|Endpunkt-Abonnementschlüssel wurde anstelle des Erstellungsschlüssels verwendet|
|401|Erstellung, Endpunkt|Ungültig, falsch formatiert oder leerer Schlüssel|
|401|Erstellung, Endpunkt| Schlüssel stimmen nicht mit Region überein|
|401|Erstellen|Sie sind nicht der Besitzer oder Projektmitarbeiter|
|401|Erstellen|Ungültige Reihenfolge der API-Aufrufe|
|403|Erstellung, Endpunkt|Monatliche Kontingentgrenze für Schlüssel überschritten|
|409|Endpunkt|Anwendung wird noch geladen|
|410|Endpunkt|Anwendung muss neu trainiert und erneut veröffentlicht werden|
|414|Endpunkt|Abfrage überschreitet maximale Zeichenzahl|
|429|Erstellung, Endpunkt|Ratenlimit wurde überschritten (Anforderungen/Sekunde)|

## <a name="next-steps"></a>Nächste Schritte

* REST-API-Dokumentation für [Erstellung](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) und [Endpunkt](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78)
