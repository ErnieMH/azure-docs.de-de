---
title: include file
description: include file
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/16/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: b8a61586acd888301350277d924f3d4fe176b4c8
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2020
ms.locfileid: "92148201"
---
### <a name="step-1-navigate-to-the-virtual-network-gateway"></a>Schritt 1: Navigieren zum Gateway für virtuelle Netzwerke

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu **Alle Ressourcen**. 
2. Um die Seite mit dem Gateway für virtuelle Netzwerke zu öffnen, navigieren Sie zu dem Gateway für virtuelle Netzwerke, das Sie löschen möchten, und klicken Sie darauf.

### <a name="step-2-delete-connections"></a>Schritt 2: Löschen von Verbindungen

1. Klicken Sie auf der Seite für das Gateway für virtuelle Netzwerke auf **Verbindungen**, um alle Verbindungen mit dem Gateway anzuzeigen.
2. Klicken Sie auf **...** in der Zeile mit dem Namen der Verbindung, und wählen Sie in der Dropdownliste die Option **Löschen** aus.
3. Klicken Sie auf **Ja**, um zu bestätigen, dass Sie die Verbindung löschen möchten. Wenn Sie über mehrere Verbindungen verfügen, löschen Sie jede Verbindung.

### <a name="step-3-delete-the-virtual-network-gateway"></a>Schritt 3: Löschen des Gateways für virtuelle Netzwerke

Hinweis: Wenn Sie neben der S2S-Konfiguration noch über eine P2S-Konfiguration für dieses VNET verfügen, werden automatisch alle P2S-Clients ohne Warnung getrennt, wenn Sie das Gateway des virtuellen Netzwerks löschen.

1. Klicken Sie auf der Seite für das Gateway für virtuelle Netzwerke auf **Übersicht**.
2. Klicken Sie auf der Seite **Übersicht** auf **Löschen**, um das Gateway zu löschen.
