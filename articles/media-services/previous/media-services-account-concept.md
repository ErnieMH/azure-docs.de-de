---
title: Verwalten von Azure Media Services v2-Konten | Microsoft-Dokumentation
description: Um mit dem Verwalten, Verschlüsseln, Codieren, Analysieren und Streamen von Medieninhalten in Azure beginnen zu können, müssen Sie ein Media Services-Konto erstellen. In diesem Artikel erfahren Sie, wie Sie Azure Media Services v2-Konten verwalten.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/10/2021
ms.author: inhenkel
ms.openlocfilehash: a097b186a2287ec13866c8a5ee9420641b44d131
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "103015946"
---
# <a name="manage-azure-media-services-v2-accounts"></a>Verwalten von Azure Media Services v2-Konten

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

Um mit dem Verwalten, Verschlüsseln, Codieren, Analysieren und Streamen von Medieninhalten in Azure beginnen zu können, müssen Sie ein Media Services-Konto erstellen. Wenn Sie ein Media Services-Konto erstellen, müssen Sie den Namen einer Azure Storage-Kontoressource angeben. Das angegebene Speicherkonto wird an Ihr Media Services-Konto angefügt. Das Media Services-Konto und alle zugeordneten Speicherkonten müssen im gleichen Azure-Abonnement enthalten sein.  

## <a name="moving-a-media-services-account-between-subscriptions"></a>Verschieben eines Media Services-Kontos zwischen Abonnements

Wenn Sie ein Media Services-Konto in ein neues Abonnement verschieben möchten, müssen Sie zunächst die gesamte Ressourcengruppe, die das Media Services-Konto enthält, in das neue Abonnement verschieben. Dabei müssen alle angefügten Ressourcen verschoben werden: Azure Storage-Konten, Azure CDN-Profile usw. Weitere Informationen finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../../azure-resource-manager/management/move-resource-group-and-subscription.md). Wie bei allen Ressourcen in Azure kann das Verschieben einer Ressourcengruppe einige Zeit in Anspruch nehmen.

Das mehrinstanzenfähige Modell wird von Media Services v2 nicht unterstützt. Wenn Sie ein Media Services-Konto in ein Abonnement in einem neuen Mandanten verschieben müssen, erstellen Sie in dem neuen Mandanten zunächst eine neue Azure AD-Anwendung (Active Directory). Verschieben Sie dann Ihr Konto in das Abonnement in dem neuen Mandanten. Nach Abschluss der Mandantenverschiebung können Sie eine Azure AD-Anwendung aus dem neuen Mandanten verwenden, um über die v2-APIs auf das Media Services-Konto zuzugreifen.

> [!IMPORTANT]
> Für den Zugriff auf die Media Services v2-API müssen die Informationen der [Azure AD-Authentifizierung](media-services-portal-get-started-with-aad.md) zurückgesetzt werden.
  
### <a name="considerations"></a>Überlegungen

* Erstellen Sie vor der Migration zu einem anderen Abonnement Sicherungen aller Daten in Ihrem Konto.
* Beenden Sie alle Streamingendpunkte und Livestreamingressourcen. Während der Verschiebung der Ressourcengruppe können die Benutzer nicht auf Ihre Inhalte zugreifen.

> [!IMPORTANT]
> Starten Sie den Streamingendpunkt erst, nachdem der Verschiebevorgang erfolgreich abgeschlossen wurde.

### <a name="troubleshoot"></a>Problembehandlung

Wenn ein Media Services-Konto oder ein zugehöriges Azure Storage-Konto nach dem Verschieben der Ressourcengruppe „getrennt“ wird, versuchen Sie, die Speicherkontoschlüssel zu rotieren. Sollte das Rotieren der Speicherkontoschlüssel das Problem mit dem getrennten Media Services-Kontos nicht beheben, stellen Sie im Media Services-Konto über das Menü „Support und Problembehandlung“ eine neue Supportanfrage.  

## <a name="next-steps"></a>Nächste Schritte

[Erstellen eines Kontos](media-services-portal-create-account.md)
