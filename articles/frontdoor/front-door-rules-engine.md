---
title: 'Regel-Engine für Azure Front Door: Architektur und Terminologie'
description: Dieser Artikel enthält eine Übersicht zum Azure Front Door-Regelmodul-Feature.
services: frontdoor
documentationcenter: ''
author: duongau
editor: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 9/29/2020
ms.author: duau
ms.openlocfilehash: 8e478cebcf8c5c9365100ade23c3d610c24930ba
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "91569752"
---
# <a name="what-is-rules-engine-for-azure-front-door"></a>Was ist das Regelmodul für Azure Front Door? 

Mithilfe des Regelmoduls können Sie anpassen, wie HTTP-Anforderungen im Edge-Bereich gehandhabt werden, und ein besser gesteuertes Verhalten für Ihre Webanwendung erzielen. Das Regelmodul für Azure Front Door weist verschiedene wichtige Funktionen auf, zu denen folgende gehören:

* Erzwingen von HTTPS, um sicherzustellen, dass alle Endbenutzer über eine sichere Verbindung mit Ihren Inhalten interagieren.
* Implementieren von Sicherheitsheadern wie HTTP Strict-Transport-Security (HSTS), X-XSS-Protection, Content-Security-Policy, X-Frame-Options und Access-Control-Allow-Origin für Cross-Origin Resource Sharing (CORS)-Szenarien, um browserbasierte Sicherheitsrisiken zu verhindern. Sicherheitsbasierte Attribute können auch mit Cookies definiert werden.
* Weiterleiten von Anforderungen an mobile oder Desktopversionen Ihrer Anwendung, basierend auf den Mustern von Inhalten in Anforderungsheadern, Cookies oder Abfragezeichenfolgen.
* Verwenden von Umleitungsfunktionen, um die Umleitungen 301, 302, 307 und 308 an den Client zurückzugeben und so an neue Hostnamen, Pfade oder Protokolle zu leiten.
- Dynamisches Ändern der Zwischenspeicherkonfiguration Ihrer Route, basierend auf den eingehenden Anforderungen.
- Umschreiben des Pfads der URL-Anforderung und Weiterleiten der Anforderung an das entsprechende Back-End in Ihrem konfigurierten Back-End-Pool.

## <a name="architecture"></a>Aufbau 

Das Regelmodul behandelt Anforderungen im Edge-Bereich. Wenn eine Anforderung auf den Front Door-Endpunkt trifft, wird zuerst WAF ausgeführt, gefolgt von der Regelmodulkonfiguration, die Ihrem Front-End/Ihrer Domäne zugeordnet ist. Wenn eine Regelmodulkonfiguration ausgeführt wird, bedeutet dies, dass die übergeordnete Routingregel bereits übereinstimmt. Damit alle Aktionen in jeder Regel ausgeführt werden, müssen alle Übereinstimmungsbedingungen innerhalb einer Regel erfüllt sein. Wenn eine Anforderung mit keiner der Bedingungen in der Regelmodulkonfiguration übereinstimmt, wird die Standardroutingregel ausgeführt. 

Im folgenden Diagramm wird beispielsweise ein Regelmodul so konfiguriert, dass ein Antwortheader angefügt wird. Der Header ändert das maximale Alter (Max-age) der Cachesteuerung, wenn die Übereinstimmungsbedingung erfüllt wird. 

![Antwortheaderaktion](./media/front-door-rules-engine/rules-engine-architecture-3.png)

In einem anderen Beispiel sehen wir, dass das Regelmodul so konfiguriert ist, dass ein Benutzer zur mobilen Version der Website gesendet wird, wenn die Übereinstimmungsbedingung „Gerätetyp“ den Wert „true“ hat. 

![Außerkraftsetzung der Routenkonfiguration](./media/front-door-rules-engine/rules-engine-architecture-1.png)

In diesen beiden Beispielen wird die angegebene Routenregel ausgeführt, wenn keine der Übereinstimmungsbedingungen erfüllt ist. 

## <a name="terminology"></a>Begriff 

Mit dem Azure Front Door-Regelmodul können Sie eine Kombination von Regelmodulkonfigurationen erstellen, die jeweils aus einer Reihe von Regeln bestehen. Im Folgenden finden Sie einige hilfreiche Begriffe, auf die Sie bei der Konfiguration Ihres Regelmoduls stoßen werden. 

- *Regelmodulkonfiguration*: Ein Satz von Regeln, die auf eine einzelne Routenregel angewendet werden. Jede Konfiguration ist auf 25 Regeln beschränkt. Sie können bis zu 10 Konfigurationen erstellen. 
- *Regelmodulregel*: Eine Regel, die bis zu 10 Übereinstimmungsbedingungen und 5 Aktionen umfasst.
- *Übereinstimmungsbedingung*: Es gibt eine Vielzahl von Übereinstimmungsbedingungen, anhand derer Ihre eingehenden Anforderungen analysiert werden können. Eine Regel kann bis zu 10 Übereinstimmungsbedingungen enthalten. Übereinstimmungsbedingungen werden mit einem **UND**-Operator ausgewertet. Eine vollständige Liste der Übereinstimmungsbedingungen finden Sie [hier](front-door-rules-engine-match-conditions.md). 
- *Aktion:* Aktionen geben vor, was mit Ihren eingehenden Anforderungen geschieht: Derzeit sind Anforderungs-/Antwortheaderaktionen, Weiterleitungen, Umleitungen und Umschreibungen verfügbar. Eine Regel kann bis zu fünf Aktionen enthalten, aber nur eine Außerkraftsetzung einer Routenkonfiguration.  Eine vollständige Liste der Aktionen finden Sie [hier](front-door-rules-engine-actions.md).


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie Ihre erste [Regelmodulkonfiguration](front-door-tutorial-rules-engine.md) erstellen. 
- Erfahren Sie mehr über das [Erstellen einer Front Door-Instanz](quickstart-create-front-door.md).
- Informieren Sie sich über die [Funktionsweise von Azure Front Door Service](front-door-routing-architecture.md).
