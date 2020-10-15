---
title: Abrufen der aktuellen POP-IP-Liste für Azure CDN | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe der Rest-API POP-Server abrufen können. POP-Server nehmen Anforderungen an Ursprungsserver vor, die Azure Content Delivery Network-Endpunkten zugeordnet sind.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2019
ms.author: allensu
ms.custom: ''
ms.openlocfilehash: 13d4587a80cbbe66e18eb1bec8f3154221ac62b5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88192557"
---
# <a name="retrieve-the-current-pop-ip-list-for-azure-cdn"></a>Abrufen der aktuellen POP-IP-Liste für Azure CDN

## <a name="retrieve-the-current-verizon-pop-ip-list-for-azure-cdn"></a>Abrufen der aktuellen POP-IP-Liste von Verizon für Azure CDN

Mit der REST-API können Sie den Satz von IP-Adressen für Point of Presence (POP)-Server von Verizon abrufen. Diese POP-Server führen Anforderung an Ursprungsserver durch, die Azure Content Delivery Network (CDN)-Endpunkten in einem Verizon-Profil (**Azure CDN Standard von Verizon** oder **Azure CDN Premium von Verizon**) zugeordnet sind. Beachten Sie, dass sich dieser Satz von IP-Adressen von den IP-Adressen unterscheidet, die dem Client bei Anforderungen an die POPs angezeigt werden würden. 

Informationen zur Syntax des REST-API-Vorgangs zum Abrufen der POP-Liste finden Sie unter [Edgeknoten – Liste](https://docs.microsoft.com/rest/api/cdn/edgenodes/list).

## <a name="retrieve-the-current-microsoft-pop-ip-list-for-azure-cdn"></a>Abrufen der aktuellen POP-IP-Liste von Microsoft für Azure CDN

Wenn Sie Ihre Anwendung so sperren möchten, dass nur Datenverkehr aus Azure CDN von Microsoft akzeptiert wird, müssen Sie IP-ACLs für Ihr Back-End einrichten. Sie können auch den Satz akzeptierter Werte für den Header „X-Forwarded-Host“ einschränken, der von Azure CDN von Microsoft gesendet wird. Diese Schritte werden wie folgt beschrieben:

Konfigurieren Sie IP-ACLs für Ihre Back-Ends so, dass sie nur Datenverkehr aus dem Back-End-IP-Adressraum von Azure CDN von Microsoft und den Infrastrukturdiensten von Azure akzeptieren. 

* IPv4-Back-End-IP-Adressraum von Azure CDN von Microsoft: 147.243.0.0/16
* IPv6-Back-End-IP-Adressraum von Azure CDN von Microsoft: 2a01:111:2050::/44

Um Diensttags mit Azure CDN von Microsoft zu verwenden, verwenden Sie das Azure Front Door-Tag. IP-Bereiche und Diensttags für Microsoft-Dienste finden Sie [hier](https://www.microsoft.com/download/details.aspx?id=56519).


## <a name="typical-use-case"></a>Typischer Anwendungsfall

Aus Sicherheitsgründen können Sie diese IP-Liste verwenden, um zu erzwingen, dass Anfragen an Ihren Ursprungsserver nur von einem gültigen Verizon-POP gestellt werden. Wenn ein Benutzer zum Beispiel den Hostnamen oder die IP-Adresse für den Ursprungsserver eines CDN-Endpunkts ermittelt hat, können Anfragen direkt an den Ursprungsserver gerichtet und so die Skalierungs- und Sicherheitsfunktionen von Azure CDN umgangen werden. Dieses Szenario kann verhindert werden, indem Sie die IPs in der zurückgegebenen Liste als die einzigen erlaubten IPs auf einem Ursprungsserver festlegen. Um sicherzustellen, dass Sie mit der aktuellen POP-Liste arbeiten, rufen Sie diese mindestens einmal täglich ab. 

## <a name="next-steps"></a>Nächste Schritte

Informationen zur REST-API finden Sie unter [Azure CDN-REST-APIs](https://docs.microsoft.com/rest/api/cdn/).
