---
title: Konfigurieren von API-Protokollen – Azure Event Grid IoT Edge | Microsoft-Dokumentation
description: Erfahren Sie mehr über mögliche Protokollkonfigurationen für ein Event Grid-Modul.
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.date: 07/08/2020
ms.topic: article
ms.openlocfilehash: a9cf6088201ffeaed76d99a9b211e5bcd1ea139a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91322579"
---
# <a name="configure-event-grid-api-protocols"></a>Konfigurieren von Event Grid API-Protokollen

Dieser Leitfaden enthält Beispiele zu möglichen Protokollkonfigurationen für ein Event Grid-Modul. Das Event Grid-Modul macht die API für ihre Verwaltungs- und Laufzeitvorgänge verfügbar. Die folgende Tabelle enthält die Protokolle und Ports.

| Protocol | Port | BESCHREIBUNG |
| ---------------- | ------------ | ------------ |
| HTTP | 5888 | Standardmäßig deaktiviert. Nur während Tests nützlich. Nicht geeignet für Produktionsworkloads.
| HTTPS | 4438 | Standard

Informationen zu allen möglichen Konfigurationen finden Sie im Leitfaden zu [Sicherheit und Authentifizierung](security-authentication.md).

## <a name="expose-https-to-iot-modules-on-the-same-edge-network"></a>Verfügbarmachen von HTTPS in IoT-Modulen im selben Edge-Netzwerk

```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=strict",
    "inbound__serverAuth__serverCert__source=IoTEdge"
  ]
}
 ```

## <a name="enable-https-to-other-iot-modules-and-non-iot-workloads"></a>Aktivieren von HTTPS für andere IoT-Module und Nicht-IoT-Workloads

```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=strict",
    "inbound__serverAuth__serverCert__source=IoTEdge"
  ],
  "HostConfig": {
    "PortBindings": {
      "4438/tcp": [
        {
          "HostPort": "4438"
        }
      ]
    }
  }
}
 ```

>[!NOTE]
> Im Abschnitt **PortBindings** können Sie interne Ports den Ports des Containerhosts zuordnen. Diese Funktion ermöglicht es, das Event Grid-Modul von außerhalb des IoT Edge-Containernetzwerks zu erreichen, wenn das IoT Edge-Gerät öffentlich erreichbar ist.

## <a name="expose-http-and-https-to-iot-modules-on-the-same-edge-network"></a>Verfügbarmachen von HTTP- und HTTPS-Modulen für IoT-Module im selben Edge-Netzwerk

```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=enabled",
    "inbound__serverAuth__serverCert__source=IoTEdge"
  ]
}
 ```

## <a name="enable-http-and-https-to-other-iot-modules-and-non-iot-workloads"></a>Aktivieren von HTTP und HTTPS für andere IoT-Module und Nicht-IoT-Workloads

```json
 {
  "Env": [
    "inbound__serverAuth__tlsPolicy=enabled",
    "inbound__serverAuth__serverCert__source=IoTEdge"
  ],
  "HostConfig": {
    "PortBindings": {
      "4438/tcp": [
        {
          "HostPort": "4438"
        }
      ],
      "5888/tcp": [
        {
          "HostPort": "5888"
        }
      ]
    }
  }
}
 ```

>[!NOTE]
> Standardmäßig ist jedes IoT-Modul Teil der IoT Edge-Runtime, die vom Bridgenetzwerk erstellt wird. Sie ermöglicht verschiedenen IoT-Modulen im selben Netzwerk, miteinander zu kommunizieren. **PortBindings** gestattet es Ihnen, einen internen Containerport dem Hostcomputer zuzuordnen, wodurch jeder in die Lage versetzt wird, von außerhalb auf den Port des Event Grid-Moduls zugreifen zu können.

>[!IMPORTANT]
> Zwar können die Ports von außerhalb des IoT Edge-Netzwerks zugänglich gemacht werden, doch die Clientauthentifizierung erzwingt, wer tatsächlich Aufrufe an das Modul durchführen darf.
