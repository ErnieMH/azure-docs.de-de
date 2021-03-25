---
title: Konfigurieren von Apple Push Notification Service in Azure Notification Hubs | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie einen Azure Notification Hub mit Apple Push Notification Service-Einstellungen (APNS) konfigurieren.
services: notification-hubs
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.topic: article
ms.date: 06/22/2020
ms.author: sethm
ms.reviewer: thsomasu
ms.lastreviewed: 03/25/2019
ms.openlocfilehash: 63c7e0c9569428b55420911f253deee52ce440cb
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "85255397"
---
# <a name="configure-apple-push-notification-service-settings-for-a-notification-hub-in-the-azure-portal"></a>Konfigurieren von Apple Push Notification Service-Einstellungen für einen Notification Hub im Azure-Portal

In diesem Artikel wird gezeigt, wie Sie Apple Push Notification Service-Einstellungen (APNS) für einen Azure Notification Hub über das Azure-Portal konfigurieren.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie noch keinen Notification Hub erstellt haben, erstellen Sie ihn jetzt. Weitere Informationen finden Sie unter [Erstellen einer Azure Notification Hub-Instanz über das Azure-Portal](create-notification-hub-portal.md).

## <a name="configure-apple-push-notification-service"></a>Konfigurieren von Apple Push Notification Service

Die folgende Vorgehensweise beschreibt die Schritte zum Konfigurieren der Apple Push Notification Service-Einstellungen (APNS) für einen Notification Hub:

1. Wählen Sie im Azure-Portal auf der Seite **Notification Hub** im Menü auf der linken Seite die Option **Apple (APNS)** aus.

1. Wählen Sie für **Authentifizierungsmodus** entweder **Zertifikat** oder **Token** aus.

   - Wenn Sie **Zertifikat** auswählen:
      - Wählen Sie das Dateisymbol und anschließend die hochzuladende *P12*-Datei aus.
      - Geben Sie ein Kennwort ein.
      - Wählen Sie den Modus **Sandbox** aus. Alternativ können Sie den Modus **Produktion** auswählen, um Pushbenachrichtigungen an Benutzer zu senden, die Ihre App im Store erworben haben.

     ![Screenshot einer APNS-Zertifikatkonfiguration im Azure-Portal](./media/configure-apple-push-notification-service/notification-hubs-apple-config-cert.png)

   - Wenn Sie **Token** auswählen:
      - Geben Sie die Werte für **Schlüssel-ID**, **Paket-ID**, **Team-ID** und **Token** ein.
      - Wählen Sie den Modus **Sandbox** aus. Alternativ können Sie den Modus **Produktion** auswählen, um Pushbenachrichtigungen an Benutzer zu senden, die Ihre App im Store erworben haben.

     ![Screenshot einer APNS-Tokenkonfiguration im Azure-Portal](./media/configure-apple-push-notification-service/notification-hubs-apple-config-token.png)

## <a name="next-steps"></a>Nächste Schritte

Ein Tutorial mit Schrittanleitungen zum Senden von Benachrichtigungen an iOS-Geräte finden Sie im folgenden Artikel: [Senden von Pushbenachrichtigungen an iOS-Apps mit Azure Notification Hubs](ios-sdk-get-started.md).
