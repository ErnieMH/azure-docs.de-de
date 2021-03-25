---
title: Aufgabenvoreinstellung für Azure Media Indexer
description: Dieses Thema enthält einen Überblick über die Aufgabenvoreinstellung für Azure Media Services Media Indexer.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/10/2021
ms.author: inhenkel
ms.openlocfilehash: c054c0aa8c58894f63f8ce8432e8d7a9e1275639
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "103012223"
---
# <a name="task-preset-for-azure-media-indexer"></a>Aufgabenvoreinstellung für Azure Media Indexer

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

Azure Media Indexer ist ein Medienprozessor, mit dem Sie die folgenden Aufgaben ausführen: Mediendateien und Inhalte durchsuchbar machen, Untertitelspuren und Schlüsselwörter generieren, Medienobjektdateien indizieren, die Bestandteil Ihres Medienobjekts sind.

Dieses Thema beschreibt die Voreinstellung, die Sie an Ihren Indizierungsauftrag übergeben müssen. Ein vollständiges Beispiel finden Sie unter [Indizieren von Mediendateien mit Azure Media Indexer](media-services-index-content.md).

## <a name="azure-media-indexer-configuration-xml"></a>Azure Media Indexer-Konfigurations-XML

Die folgende Tabelle erläutert Elemente und Attribute der Konfigurations-XML.

|Name|Anforderung|BESCHREIBUNG|
|---|---|---|
|Eingabe|true|Medienobjekte, die Sie indizieren möchten.<br/>Der Azure Media Indexer unterstützt die folgenden Mediendateiformate: MP4, MOV, WMV, MP3, M4A, WMA, AAC, WAV. <br/><br/>Sie können den/die Dateinamen in Attribut **name** oder **list** des Elements **input** angeben (wie unten gezeigt). Wenn Sie nicht angeben, welche Medienobjektdatei indiziert werden soll, wird die primäre Datei ausgewählt. Wenn keine primäre Medienobjektdatei festgelegt ist, wird die erste Datei im Eingabemedienobjekt indiziert.<br/><br/>So legen Sie den Namen der Medienobjektdatei explizit fest:<br/>```<input name="TestFile.wmv" />```<br/><br/>Sie können auch in einem Arbeitsschritt mehrere Medienobjektdateien indizieren (bis zu 10 Dateien). Gehen Sie dazu folgendermaßen vor:<br/>– Erstellen Sie eine Textdatei (Manifestdatei), und geben Sie ihr die Erweiterung .LST.<br/>– Fügen Sie dieser Manifestdatei eine Liste aller Medienobjektdateinamen im Eingabemedienobjekt hinzu.<br/>– Fügen Sie die Manifestdatei zum Medienobjekt hinzu (laden Sie sie hoch).<br/>– Geben Sie den Namen der Manifestdatei im list-Attribut des input-Elements an.<br/>```<input list="input.lst">```<br/><br/>**Hinweis:** Wenn Sie mehr als 10 Dateien zur Manifestdatei hinzufügen, tritt bei der Indizierung ein Fehler auf (Fehlercode 2006).|
|metadata|false|Metadaten für die angegebene(n) Medienobjektdatei(en).<br/>```<metadata key="..." value="..." />```<br/><br/>Sie können Werte für vordefinierte Schlüssel angeben. <br/><br/>Derzeit werden die folgenden Schlüssel unterstützt:<br/><br/>**title** und **description** – zum Aktualisieren des Sprachmodells zum Verbessern der Spracherkennungsgenauigkeit verwendet.<br/>```<metadata key="title" value="[Title of the media file]" /><metadata key="description" value="[Description of the media file]" />```<br/><br/>**username** und **password** – wird für die Authentifizierung beim Herunterladen von Internetdateien über http oder https verwendet.<br/>```<metadata key="username" value="[UserName]" /><metadata key="password" value="[Password]" />```<br/>Die Werte „username“ und „password“ gelten für alle Medien-URLs im Eingabemanifest.|
|Features<br/><br/>Wurde in Version 1.2 hinzugefügt. Das einzige unterstützte Feature ist derzeit die Spracherkennung („ASR“).|false|Die Spracherkennungsfunktion umfasst die folgenden Einstellungsschlüssel:<br/><br/>Sprache:<br/>– Die natürliche Sprache, die in der Multimediadatei erkannt werden soll.<br/>– English, Spanish<br/><br/>CaptionFormats:<br/>– Eine mit Semikolons getrennte Liste der gewünschten Ausgabeuntertitel-Formate (sofern vorhanden)<br/>– ttml;webvtt<br/><br/><br/>GenerateKeywords:<br/>– Ein boolesches Flag, das angibt, ob eine XML-Datei mit Schlüsselwörtern erforderlich ist oder nicht.<br/>– True; False.|

## <a name="azure-media-indexer-configuration-xml-example"></a>Azure Media Indexer-Konfigurations-XML – Beispiel

``` 
<?xml version="1.0" encoding="utf-8"?>  
<configuration version="2.0">  
  <input>  
    <metadata key="title" value="[Title of the media file]" />  
    <metadata key="description" value="[Description of the media file]" />  
  </input>  
  <settings>  
  </settings>  
  
  <features>  
    <feature name="ASR">    
      <settings>  
        <add key="Language" value="English"/>  
        <add key="GenerateKeywords" value ="true" />  
      </settings>  
    </feature>  
  </features>  
  
</configuration>  
```
  
## <a name="next-steps"></a>Nächste Schritte

Siehe [Indizieren von Mediendateien mit Azure Media Indexer](media-services-index-content.md).

