---
author: baanders
description: 'Include-Datei für Azure Digital Twins-Tutorials: Konfigurieren des Beispielprojekts'
ms.service: digital-twins
ms.topic: include
ms.date: 5/25/2020
ms.author: baanders
ms.openlocfilehash: 4ac748c606d8ec3c8ba754c34d9c9e7512344a83
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91292678"
---
## <a name="configure-the-sample-project"></a>Konfigurieren des Beispielprojekts

Richten Sie als Nächstes eine Beispielclient-Anwendung ein, die mit Ihrer Azure Digital Twins-Instanz interagiert.

Navigieren Sie auf Ihrem Computer zu der Datei, die Sie zuvor von [*Azure Digital Twins-Beispiele*](https://docs.microsoft.com/samples/azure-samples/digital-twins-samples/digital-twins-samples) heruntergeladen haben (und entpacken Sie sie, sofern noch nicht geschehen).

Navigieren Sie dann im Ordner zu _AdtSampleApp_. Öffnen Sie _**AdtE2ESample.sln**_ in Visual Studio 2019. 

Erstellen Sie in Visual Studio über den Bereich *Projektmappen-Explorer* eine Kopie der Datei _SampleClientApp > **serviceConfig.json.TEMPLATE**_. (Zum Kopieren und Einfügen können Sie das Kontextmenü verwenden.) Benennen Sie die Kopie in *serviceConfig.json* um. Sie dient als voreingestellte JSON-Datei mit den erforderlichen Konfigurationsvariablen zum Ausführen des Projekts.

Wählen Sie die Datei *serviceConfig.json* aus, um sie im Bearbeitungsfenster zu öffnen. Ändern Sie `tenantId` in Ihre *Verzeichnis-ID*, `clientId` in die *Anwendungs-ID* und `instanceUrl` in die *hostName*-URL der Azure Digital Twins-Instanz (stellen Sie *https://* voran, wie nachfolgend gezeigt):

```json
{
  "tenantId": "<your-directory-ID>",
  "clientId": "<your-application-ID>",
  "instanceUrl": "https://<your-Azure-Digital-Twins-instance-hostName>"
}
```



Speichern und schließen Sie die Datei. 

Konfigurieren Sie als Nächstes die Datei *serviceConfig.json*, die in das Ausgabeverzeichnis kopiert werden soll, wenn Sie *SampleClientApp* erstellen. Klicken Sie dazu mit der rechten Maustaste auf die Datei *serviceConfig.json*, und wählen Sie *Eigenschaften* aus. Ändern Sie in der Überprüfung der *Eigenschaften* den Wert der Eigenschaft *In Ausgabeverzeichnis kopieren* in *Kopieren, falls aktueller*.

:::image type="content" source="../articles/digital-twins/media/includes/copy-config.png" alt-text="Auszug aus dem Visual Studio-Fenster mit dem Projektmappen-Explorer-Bereich, in dem „serviceConfig.json“ hervorgehoben ist, und mit dem Bereich „Eigenschaften“, in dem die Eigenschaft „In Ausgabeverzeichnis kopieren“ auf „Kopieren, falls aktueller“ festgelegt ist" border="false":::

Lassen Sie das Projekt _**AdtE2ESample**_ in Visual Studio geöffnet, um es weiterhin im Tutorial zu verwenden.

