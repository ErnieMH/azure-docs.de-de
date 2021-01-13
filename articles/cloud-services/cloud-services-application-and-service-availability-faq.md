---
title: Häufig gestellte Fragen zur Problemen bei der Anwendungs- und Dienstverfügbarkeit
titleSuffix: Azure Cloud Services
description: Dieser Artikel behandelt häufig gestellte Fragen zur Anwendung und Dienstverfügbarkeit von Microsoft Azure Cloud Services.
services: cloud-services
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 2707c5a6cb110d30b85b765f6c8b144e8cca7abe
ms.sourcegitcommit: a92fbc09b859941ed64128db6ff72b7a7bcec6ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2020
ms.locfileid: "92074718"
---
# <a name="application-and-service-availability-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Probleme mit der Anwendung und Dienstverfügbarkeit von Azure Cloud Services: Häufig gestellte Fragen (FAQs)

Dieser Artikel enthält häufig gestellte Fragen zu Problemen mit der Anwendung und Dienstverfügbarkeit von [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Informationen zur Größe finden sie auch unter [Größen für Clouddienste](cloud-services-sizes-specs.md) .

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-role-got-recycled-was-there-any-update-rolled-out-for-my-cloud-service"></a>Meine Rolle wurde wiederverwendet. Wurde für meinen Clouddienst ein Update eingeführt?
Ungefähr einmal im Monat gibt Microsoft eine neue Version des Gastbetriebssystems für Windows Azure PaaS-VMs heraus. Das Gastbetriebssystem ist nur ein solches Update in der Pipeline. Eine Version kann durch viele andere Faktoren beeinflusst werden. Darüber hinaus wird Azure auf Hunderttausenden von Computern ausgeführt. Daher ist es unmöglich, ein genaues Datum und eine Uhrzeit vorherzusagen, zu der Ihre Rollen neu gestartet werden. Wir aktualisieren den RSS-Feed zu Gastbetriebssystemupdates mit den neuesten Informationen, die uns vorliegen. Dabei handelt es sich jedoch um ungefähre Zeitangaben. Uns ist bekannt, dass dies für Kunden problematisch ist, und wir arbeiten an einem Plan zur Begrenzung oder genauen zeitlichen Planung von Neustarts.

Ausführliche Informationen zu den neuesten Updates für Gastbetriebssysteme finden Sie unter [Azure-Gastbetriebssystemversionen und SDK-Kompatibilitätsmatrix](cloud-services-guestos-update-matrix.md).

Hilfreiche Informationen zu Neustarts und Verweise auf technische Details zu Gast- und Hostbetriebssystemupdates finden Sie im MSDN-Blogbeitrag [Role Instance Restarts Due to OS Upgrades](/archive/blogs/kwill/role-instance-restarts-due-to-os-upgrades) (Neustarts von Rolleninstanzen aufgrund von Betriebssystemupgrades).

## <a name="why-does-the-first-request-to-my-cloud-service-after-the-service-has-been-idle-for-some-time-take-longer-than-usual"></a>Warum dauert die erste Anforderung an meinen Clouddienst länger als üblich, nachdem der Dienst für einige Zeit im Leerlauf war?
Wenn der Webserver die erste Anforderung erhält, kompiliert er zuerst den Code neu und verarbeitet anschließend die Anforderung. Deshalb dauert die erste Anforderung länger als die anderen. Standardmäßig wird der Anwendungspool bei Benutzerinaktivität heruntergefahren. Zudem wird der Anwendungspool standardmäßig nach 1740 Minuten (29 Stunden) wiederverwendet.

Anwendungspools für Internetinformationsdienste (IIS) können in regelmäßigen Abständen wiederverwendet werden, um instabile Zustände zu vermeiden, die dazu führen können, dass die Anwendung abstürzt, nicht mehr reagiert oder Arbeitsspeicher verloren geht.

Weitere Informationen zu diesem Problem finden Sie in den folgenden Dokumenten:
* [Fixing slow initial load for IIS (Beheben des ersten langsamen Ladevorgangs bei IIS)](https://stackoverflow.com/questions/13386471/fixing-slow-initial-load-for-iis)
* [IIS 7.5 web application first request after app-pool recycle very slow (Erste Anforderung bei IIS 7.5-Webanwendung nach Wiederverwendung des Anwendungspools sehr langsam)](https://stackoverflow.com/questions/13917205/iis-7-5-web-application-first-request-after-app-pool-recycle-very-slow)

Wenn Sie das Standardverhalten von IIS ändern möchten, müssen Sie Startaufgaben verwenden. Manuelle Änderungen an den Webrolleninstanzen gehen nämlich letztendlich verloren.

Weitere Informationen finden Sie unter [Konfigurieren und Ausführen von Startaufgaben für einen Clouddienst](cloud-services-startup-tasks.md).