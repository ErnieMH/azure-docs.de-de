---
title: Übersicht über die Umleitung in Azure Application Gateway
description: Erhalten Sie Informationen über die Umleitungsfunktion in Azure Application Gateway zum Umleiten von an einem Listener empfangenem Datenverkehr an einen anderen Listener oder an eine externe Site.
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.topic: conceptual
ms.date: 11/16/2019
ms.author: amsriva
ms.openlocfilehash: fb01d5a4923410f693b682d66be8d5d09f9019d0
ms.sourcegitcommit: 6e1124fc25c3ddb3053b482b0ed33900f46464b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2020
ms.locfileid: "90561577"
---
# <a name="application-gateway-redirect-overview"></a>Übersicht über die Umleitung in Application Gateway

Mithilfe von Application Gateway können Sie Datenverkehr umleiten.  Es verfügt über einen allgemeinen Umleitungsmechanismus, mit dem Datenverkehr, der an einem Listener empfangen wird, an einen anderen Listener oder eine externe Website umgeleitet werden kann. Dies vereinfacht die Anwendungskonfiguration, optimiert die Ressourcennutzung und ermöglicht neue Umleitungsszenarien (einschließlich globale und pfadbasierte Umleitung).

Ein typisches Umleitungsszenario vieler Webanwendungen ist die Unterstützung der automatischen Umleitung von HTTP zu HTTPS, um sicherzustellen, dass die gesamte Kommunikation zwischen einer Anwendung und ihren Benutzern über einen verschlüsselten Pfad stattfindet. In der Vergangenheit haben Kunden unter anderem dedizierte Back-End-Pools erstellt, die einzig dazu dienten, eingehende HTTP-Anforderungen zu HTTPS umzuleiten. Mit der Umleitungsunterstützung in Application Gateway können Sie dies einfach dadurch erreichen, dass Sie einer Routingregel eine neue Umleitungskonfiguration hinzufügen und einen anderen Listener mit HTTPS-Protokoll als Ziellistener angeben.

Die folgenden Umleitungstypen werden unterstützt:

- 301 Dauerhafte Umleitung
- 302 Found (302 Gefunden)
- 303 Verweis
- 307 Temporäre Umleitung

Die Application Gateway-Umleitung bietet Folgendes:

-  **Globale Umleitung**

   Umleitungen von einem Listener zu einem anderen Listener des Gateways. Dies ermöglicht HTTP-zu-HTTPS-Umleitungen auf einer Website.
- **Pfadbasierte Umleitung**

   Bei dieser Art von Umleitung kann HTTP nur für einen bestimmten Websitebereich zu HTTPS umgeleitet werden – etwa in einem durch /cart/* gekennzeichneten Einkaufswagenbereich.
- **Umleitung an eine externe Website**

![Diagramm mit Benutzern und einer App Gateway-Instanz sowie Verbindungen zwischen den beiden, einschließlich eines roten HTTP-Pfeils (entsperrt), eines direkten roten Pfeils (301, nicht zulässig) und eines grünen HTTPS-Pfeils (gesperrt).](./media/redirect-overview/redirect.png)

Aufgrund dieser Änderung müssen Kunden ein neues Umleitungskonfigurationsobjekt erstellen, das den Ziellistener oder die externe Website angibt, an den bzw. an die die Umleitung erfolgen soll. Das Konfigurationselement unterstützt auch Optionen zum Anfügen von URI-Pfad und Abfragezeichenfolge an die umgeleitete URL. Sie können auch den Typ der Umleitung auswählen. Die erstellte Umleitungskonfiguration wird mittels einer neuen Regel an den Quelllistener angefügt. Bei Verwendung einer einfachen Regel wird die Umleitungskonfiguration einem Quelllistener zugeordnet und fungiert als globale Umleitung. Wenn eine pfadbasierte Regel verwendet wird, wird die Umleitungskonfiguration in der URL-Pfadzuordnung definiert. Daher gilt sie nur für den bestimmten Pfadbereich einer Site.

### <a name="next-steps"></a>Nächste Schritte

[Konfigurieren der Umleitung für Application Gateway mit PowerShell](tutorial-url-redirect-powershell.md)
