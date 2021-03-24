---
title: Anpassen von Regeln über das Portal – Azure Web Application Firewall
description: Dieser Artikel enthält Informationen zum Anpassen von Web Application Firewall-Regeln (WAF) in Application Gateway mit dem Azure-Portal.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.date: 11/14/2019
ms.author: victorh
ms.topic: article
ms.openlocfilehash: c4635333614ee1c0fd0322c29a659380fb4315c9
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "74048378"
---
# <a name="customize-web-application-firewall-rules-using-the-azure-portal"></a>Anpassen von Web Application Firewall-Regeln mit dem Azure-Portal

Die Web Application Firewall (WAF) von Azure Application Gateway bietet Schutz für Webanwendungen. Diese Schutzmaßnahmen werden durch die Kernregeln (Core Rule Set, CRS) des Open Web Application Security-Projekts (OWASP) bereitgestellt. Einige Regeln können falsche positive Ergebnisse ausgeben und den realen Datenverkehr blockieren. Aus diesem Grund bietet Application Gateway die Möglichkeit, Regelgruppen und Regeln anzupassen. Weitere Informationen zu den jeweiligen Regelgruppen und Regeln finden Sie in der [Liste der CRS-Regelgruppen und -Regeln von Web Application Firewall](application-gateway-crs-rulegroups-rules.md).

>[!NOTE]
> Wenn Ihr Anwendungsgateway nicht auf die WAF-Ebene zurückgreift, wird die Option zum Aktualisieren des Anwendungsgateways auf die WAF-Ebene im rechten Bereich angezeigt. 

![Aktivieren der WAF][fig1]

## <a name="view-rule-groups-and-rules"></a>Anzeigen von Regelgruppen und Regeln

**So zeigen Sie Regelgruppen und Regeln an**
1. Browsen Sie zum Anwendungsgateway, und wählen Sie **Web Application Firewall** aus.  
2. Wählen Sie die **WAF-Richtlinie** aus.
2. Wählen Sie **Verwaltete Regeln** aus.

   Diese Ansicht zeigt auf der Seite eine Tabelle aller Regelgruppen an, die mit dem ausgewählten Regelsatz bereitgestellt werden. Alle Kontrollkästchen der Regel sind aktiviert.

## <a name="disable-rule-groups-and-rules"></a>Deaktivieren von Regelgruppen und Regeln

> [!IMPORTANT]
> Gehen Sie vorsichtig vor, wenn Sie Regelgruppen oder Regeln deaktivieren. Dies könnte Sie erhöhten Sicherheitsrisiken aussetzen.

Wenn Sie Regeln deaktivieren, können Sie eine gesamte Regelgruppe oder bestimmte Regeln in einer oder mehreren Regelgruppen deaktivieren. 

**So deaktivieren Sie Regelgruppen oder bestimmte Regeln**

   1. Suchen Sie nach den Regeln oder Regelgruppen, die Sie deaktivieren möchten.
   2. Aktivieren Sie die Kontrollkästchen für die Regeln, die Sie deaktivieren möchten. 
   3. Wählen Sie die Aktion am oberen Rand der Seite (Aktivieren/Deaktivieren) für die ausgewählten Regeln aus.
   2. Wählen Sie **Speichern** aus. 

![Speichern der Änderungen][3]

## <a name="mandatory-rules"></a>Obligatorische Regeln

Die folgende Liste enthält die Bedingungen, die dazu führen, dass WAF die Anforderung im Präventionsmodus blockiert. Im Erkennungsmodus werden sie als Ausnahmen protokolliert.

Diese können nicht konfiguriert oder deaktiviert werden:

* Fehler beim Analysieren des Anforderungstexts führen dazu, dass die Anforderung blockiert wird, sofern die Textüberprüfung nicht deaktiviert ist (XML, JSON, Formulardaten).
* Die Datenlänge des Anforderungstexts (ohne Dateien) überschreitet das konfigurierte Limit.
* Der Anforderungstext (einschließlich Dateien) überschreitet den Grenzwert.
* Ein interner Fehler ist in der WAF-Engine aufgetreten.

CRS 3.x-spezifisch:

* Anomaliebewertung für Eingang überschritt den Schwellenwert

## <a name="next-steps"></a>Nächste Schritte

Nach dem Konfigurieren Ihrer deaktivierten Regeln können Sie sich darüber informieren, wie Sie WAF-Protokolle anzeigen. Weitere Informationen finden Sie unter [Application Gateway-Diagnose](../../application-gateway/application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ../media/application-gateway-customize-waf-rules-portal/1.png
[3]: ../media/application-gateway-customize-waf-rules-portal/figure3.png
