---
title: 'Azure Security Center-Vorfälle: Korrelation von intelligenten Warnungen'
description: In diesem Thema erfahren Sie, wie Fusion durch die Korrelation von intelligenten Cloudwarnungen Sicherheitsincidents in Azure Security Center generiert.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: e9d5a771-bfbe-458c-9a9b-a10ece895ec1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/02/2019
ms.author: memildin
ms.openlocfilehash: 742d7122c60b383e8ab7aa73f73b11b47843a9d5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91613820"
---
# <a name="cloud-smart-alert-correlation-in-azure-security-center-incidents"></a>Korrelation von intelligenten Cloudwarnungen in Azure Security Center (Incidents)

Azure Security Center analysiert kontinuierlich die Hybrid Cloud-Workloads unter Verwendung von erweiterten Analysen und Threat Intelligence, um Sie auf schädliche Aktivitäten aufmerksam zu machen.

Die Bedrohungsabdeckung wird ständig erweitert. Es ist wichtig, selbst kleinste Kompromittierungen zu erkennen, und für Sicherheitsanalysten kann es schwierig sein, die verschiedenen Warnungen zu sichten und einen tatsächlichen Angriff zu erkennen. Security Center unterstützt Analysten bei der Bewältigung dieser Warnungen. Die Lösung ermöglicht eine zeitnahe Angriffsdiagnose und fasst zu diesem Zweck verschiedene Warnungen und Signale mit geringer Genauigkeit zu Sicherheitsincidents zusammen.

Fusion-Analysen sind die Technologie und das Analyse-Back-End, die Security Center-Incidents zugrunde liegen, und ermöglichen die Korrelation verschiedener Warnungen und kontextbezogener Signale. Fusion analysiert ressourcenübergreifend die verschiedenen, für ein Abonnement gemeldeten Signale. Fusion ermittelt Muster, die Aufschluss über den Status eines Angriffs geben, oder Signale mit gemeinsamen kontextbezogen Informationen, die darauf hindeuten, dass eine einheitliche Gegenmaßnahme ergriffen werden sollte.

Bei Fusion-Analysen werden Sicherheitsdomänenkenntnisse mit künstlicher Intelligenz kombiniert, um Warnungen zu analysieren und neue Angriffsmuster zu erkennen, sobald diese auftreten. 

Security Center nutzt die Angriffsmatrix von Mitre, um einen Zusammenhang zwischen Warnungen und deren vermeintlicher Absicht herzustellen und so zur Formalisierung von Sicherheitsdomänenkenntnissen beizutragen. Auf der Grundlage der Informationen, die für die einzelnen Schritte eines Angriffs gesammelt wurden, kann Security Center außerdem Aktivitäten ausschließen, die lediglich wie Angriffsschritte aussehen, aber gar keine sind.

Da Angriffe häufig mandantenübergreifend auftreten, kann Security Center KI-Algorithmen für die Analyse von Angriffssequenzen kombinieren, die für die einzelnen Abonnements gemeldet werden. Dadurch lässt sich erkennen, ob es sich um verbreitete Warnungsmuster handelt (und nicht nur um Warnungen, die zufällig zusammenhängen).

Bei der Untersuchung eines Incidents benötigen Analysten häufig zusätzlichen Kontext, um die Art der Bedrohung und mögliche Gegenmaßnahmen einschätzen zu können. Selbst wenn also beispielsweise eine Netzwerkanomalie erkannt wurde, ist es schwer, angemessen darauf zu reagieren, wenn die weiteren Vorgänge in Bezug auf das Netzwerk oder die betroffenen Ressourcen nicht klar sind. Ein Sicherheitsincident kann hier mit Artefakten, verwandten Ereignissen und Informationen weiterhelfen. Welche zusätzlichen Informationen für Sicherheitsincidents verfügbar sind, hängt von der Art der erkannten Bedrohung und der Konfiguration Ihrer Umgebung ab. 

> [!TIP]
> Eine Liste der Warnungen für Sicherheitsvorfälle, die von Fusion-Analysen erzeugt werden, finden Sie in der [Verweistabelle zu Warnungen](alerts-reference.md#alerts-fusion).

:::image type="content" source="./media/security-center-alerts-cloud-smart/security-incident.png" alt-text="Screenshot des Berichts zu erkanntem Sicherheitsvorfall":::


Weitere Informationen zu Sicherheitsvorfällen finden Sie unter [Verwalten von Sicherheitsvorfällen in Azure Security Center](security-center-incident.md).

