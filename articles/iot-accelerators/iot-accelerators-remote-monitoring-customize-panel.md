---
title: Hinzufügen eines Panels zur Benutzeroberfläche der Remoteüberwachungslösung – Azure | Microsoft-Dokumentation
description: Dieser Artikel zeigt Ihnen, wie Sie auf der Webbenutzeroberfläche des Solution Accelerators für die Remoteüberwachung ein neues Panel zum Dashboard hinzufügen.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/05/2018
ms.topic: conceptual
ms.custom: devx-track-js
ms.openlocfilehash: 1dcca8409022ba4cf1f988b7c777e3a1fa511060
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91318363"
---
# <a name="add-a-custom-panel-to-the-dashboard-in-the-remote-monitoring-solution-accelerator-web-ui"></a>Hinzufügen eines benutzerdefinierten Panels zum Dashboard zur Webbenutzeroberfläche des Solution Accelerators für die Remoteüberwachung

Dieser Artikel zeigt Ihnen, wie Sie auf der Webbenutzeroberfläche des Solution Accelerators für die Remoteüberwachung ein neues Panel zu einer Dashboardseite hinzufügen. Der Artikel beschreibt Folgendes:

- Vorbereiten einer lokalen Entwicklungsumgebung.
- Hinzufügen eines neuen Panels zu einer Dashboardseite auf der Webbenutzeroberfläche.

Das Beispielpanel in diesem Artikel wird auf der vorhandenen Dashboardseite angezeigt.

## <a name="prerequisites"></a>Voraussetzungen

Um die Schritte in diesem Leitfaden abzuschließen, muss die folgende Software auf dem lokalen Entwicklungscomputer installiert sein:

- [Git-Client](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Vorbereitung

Sie sollten die Schritte im Artikel [Add a custom page to the Remote Monitoring solution accelerator web UI](iot-accelerators-remote-monitoring-customize-page.md) (Hinzufügen einer benutzerdefinierten Seite zur Webbenutzeroberfläche des Solution Accelerators für die Remoteüberwachung) ausführen, bevor Sie fortfahren.

## <a name="add-a-panel"></a>Hinzufügen eines Bereichs

Um der Webbenutzeroberfläche ein Panel hinzuzufügen, müssen Sie die Quelldateien hinzufügen, in denen das Panel definiert ist, und dann das Dashboard so ändern, dass das Panel angezeigt wird.

### <a name="add-the-new-files-that-define-the-panel"></a>Hinzufügen der neuen Dateien, in denen das Panel definiert ist

Für den Einstieg enthält der Ordner **src/walkthrough/components/pages/dashboard/panels/examplePanel** die Dateien, die zur Definition eines Panels benötigt werden, darunter:

**examplePanel.js**

[!code-javascript[Example panel](~/remote-monitoring-webui/src/walkthrough/components/pages/dashboard/panels/examplePanel/examplePanel.js?name=panel "Example panel")]

Kopieren Sie den Ordner **src/walkthrough/components/pages/dashboard/panels/examplePanel** in den Ordner **src/components/pages/dashboard/panels**.

Fügen Sie den folgenden Export zur Datei **src/walkthrough/components/pages/dashboard/panels/index.js** hinzu:

```js
export * from './examplePanel';
```

### <a name="add-the-panel-to-the-dashboard"></a>Hinzufügen des Panels zum Dashboard

Ändern Sie **src/components/pages/dashboard/dashboard.js**, um das Panel hinzuzufügen.

Fügen Sie der Liste der Importe aus Panels das Beispielpanel hinzu:

```js
import {
  OverviewPanel,
  AlertsPanel,
  TelemetryPanel,
  AnalyticsPanel,
  MapPanel,
  transformTelemetryResponse,
  chartColorObjects,
  ExamplePanel
} from './panels';
```

Fügen Sie dem Raster im Seiteninhalt die folgende Zellendefinition hinzu:

```js
          <Cell className="col-2">
            <ExamplePanel t={t} />
          </Cell>
```

## <a name="test-the-flyout"></a>Testen des Flyouts

Wenn die Webbenutzeroberfläche nicht lokal ausgeführt wird, führen Sie den folgenden Befehl im Stammverzeichnis Ihrer lokalen Kopie im Repository durch:

```cmd/sh
npm start
```

Der vorherige Befehl führt die Benutzeroberfläche lokal unter `http://localhost:3000/dashboard` aus. Navigieren Sie zur Seite **Dashboard**, um das neue Panel anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Informationen zu den verfügbaren Ressourcen erhalten, mit denen Sie der Webbenutzeroberfläche im Solution Accelerator für die Remoteüberwachung Dashboards hinzufügen oder diese anpassen können.

Weitere konzeptuelle Informationen zum Solution Accelerator für die Remoteüberwachung finden Sie unter [Remoteüberwachungsarchitektur](iot-accelerators-remote-monitoring-sample-walkthrough.md).
