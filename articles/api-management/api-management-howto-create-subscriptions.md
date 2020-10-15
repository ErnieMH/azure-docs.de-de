---
title: Erstellen von Abonnements in Azure API Management | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Abonnements in Azure API Management erstellen. Ein Abonnement ist erforderlich, um Abonnementschlüssel abzurufen, die den Zugriff auf APIs ermöglichen.
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/14/2018
ms.author: apimpm
ms.openlocfilehash: 191323a4c150c00c93245be35c9c8af381e26b42
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87904857"
---
# <a name="create-subscriptions-in-azure-api-management"></a>Erstellen von Abonnements in Azure API Management (APIM)

Beim Veröffentlichen von APIs über Azure API Management ist eine einfache und gängige Methode zum Schutz des Zugriffs auf diese APIs die Verwendung von Abonnementschlüsseln. Clientanwendungen, die die veröffentlichten APIs nutzen möchten, müssen einen gültigen Abonnementschlüssel in HTTP-Anforderungen einschließen, wenn Aufrufe an diese APIs gesendet werden. Um einen Abonnementschlüssel für den API-Zugriff zu erhalten, ist ein Abonnement erforderlich. Weitere Informationen zu Abonnements finden Sie unter [Abonnements in Azure API Management](api-management-subscriptions.md).

In diesem Artikel werden Sie durch die Schritte zum Erstellen von Abonnements im Azure-Portal geleitet.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen der Schritte in diesem Artikel ist Folgendes erforderlich:

+ [Erstellen einer API Management-Instanz](get-started-create-service-instance.md)
+ Grundkenntnisse zu [Abonnements in API Management](api-management-subscriptions.md)

## <a name="create-a-new-subscription"></a>Erstellen eines neuen Abonnements

1. Wählen Sie im Menü links die Option **Abonnements** aus.
2. Wählen Sie **Abonnement hinzufügen** aus.
3. Geben Sie einen Namen für das Abonnement an, und wählen Sie den Bereich aus.
4. Wählen Sie optional aus, ob das Abonnement einem Benutzer zugeordnet werden soll.
5. Wählen Sie **Speichern** aus.

![Flexible Abonnements](./media/api-management-subscriptions/flexible-subscription.png)

Nach der Erstellung des Abonnements werden zwei API-Schlüssel für den Zugriff auf die APIs bereitgestellt: ein primärer Schlüssel und ein sekundärer Schlüssel. 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu API Management:

+ Informationen zu weiteren [Konzepten](api-management-terminology.md) in API Management
+ [Tutorials](import-and-publish.md) zum weiteren Kennenlernen von API Management
+ [FAQ-Seite](api-management-faq.md) für häufig gestellte Fragen