---
title: 'Schnellstart: Verwalten von Telefonnummern mithilfe von Azure Communication Services'
description: Hier erfahren Sie, wie Sie Telefonnummern mithilfe von Azure Communication Services verwalten.
author: prakulka
manager: nmurav
services: azure-communication-services
ms.author: prakulka
ms.date: 03/10/2021
ms.topic: quickstart
ms.service: azure-communication-services
ms.custom: references_regions
zone_pivot_groups: acs-azp-java-net-python-csharp-js
ms.openlocfilehash: 269967db4daed2bb3c42e668e2b36d74f8b0f45c
ms.sourcegitcommit: b4032c9266effb0bf7eb87379f011c36d7340c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2021
ms.locfileid: "107904584"
---
# <a name="quickstart-manage-phone-numbers"></a>Schnellstart: Verwalten von Telefonnummern

[!INCLUDE [Regional Availability Notice](../../includes/regional-availability-include.md)]

[!INCLUDE [Bulk Acquisition Instructions](../../includes/phone-number-special-order.md)]

::: zone pivot="platform-azp"
[!INCLUDE [Azure portal](./includes/phone-numbers-portal.md)]
::: zone-end

::: zone pivot="programming-language-csharp"
[!INCLUDE [Azure portal](./includes/phone-numbers-net.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Java](./includes/phone-numbers-java.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Python](./includes/phone-numbers-python.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [JavaScript](./includes/phone-numbers-js.md)]
::: zone-end

## <a name="troubleshooting"></a>Problembehandlung

Allgemeine Fragen und Probleme:

- Der Erwerb von Telefonnummern wird nur in den USA unterstützt. Stellen Sie zum Erwerben von Telefonnummern Folgendes sicher:
  - Die zugehörige Abrechnungsadresse für das Azure-Abonnement liegt in den USA. Derzeit ist es nicht möglich, eine Ressource in ein anderes Abonnement zu verschieben.
  - Ihre Communication Services-Ressource wird am Datenstandort „USA“ bereitgestellt. Derzeit ist es nicht möglich, eine Ressource an einen anderen Datenstandort zu verschieben.

- Wenn eine Telefonnummer freigegeben wurde, kann sie bis zum Ende des Abrechnungszeitraums nicht freigegeben oder erneut erworben werden.

- Wenn eine Communication Services-Ressource gelöscht wird, werden die Telefonnummern, die dieser Ressource zugeordnet sind, gleichzeitig automatisch freigegeben.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erwerben einer Telefonnummer
> * Verwalten Ihrer Telefonnummer
> * Freigeben einer Telefonnummer

> [!div class="nextstepaction"]
> [Senden einer SMS](../telephony-sms/send.md)
> [Erste Schritte für das Tätigen von Anrufen](../voice-video-calling/getting-started-with-calling.md)
