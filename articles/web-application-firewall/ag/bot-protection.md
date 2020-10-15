---
title: Konfigurieren von Bot-Schutz für Azure Web Application Firewall (WAF)
description: Erfahren Sie, wie der Bot-Schutz für Web Application Firewall (WAF) in Azure Application Gateway konfiguriert wird.
services: web-application-firewall
ms.topic: article
author: vhorne
ms.service: web-application-firewall
ms.date: 10/31/2019
ms.author: victorh
ms.openlocfilehash: 89c863e85d9eab27a47bc1bf7b98cd1c8d89e900
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "73511523"
---
# <a name="configure-bot-protection-for-web-application-firewall-on-azure-application-gateway-preview"></a>Konfigurieren von Bot-Schutz für Web Application Firewall (WAF) in Azure Application Gateway (Vorschau)

In diesem Artikel erfahren Sie, wie Sie eine Bot-Schutzregel in Azure Web Application Firewall (WAF) für Application Gateway über das Azure-Portal konfigurieren. 

Sie können einen verwalteten Bot-Schutzregelsatz für Ihre WAF aktivieren, um Anforderungen von IP-Adressen, die als schädlich bekannt sind, zu blockieren oder zu protokollieren. Die IP-Adressen stammen aus dem Microsoft Threat Intelligence-Feed. Microsoft Threat Intelligence basiert auf Intelligent Security Graph und wird von mehreren Diensten verwendet (einschließlich Azure Security Center).

> [!NOTE]
> Der Bot-Schutzregelsatz befindet sich derzeit in der öffentlichen Vorschau und wird mit einer Vorschau-SLA (Vereinbarung zum Servicelevel) bereitgestellt. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter  [Ergänzende Nutzungsbedingungen für Microsoft Azure-Vorschauversionen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) .

## <a name="prerequisites"></a>Voraussetzungen

Erstellen Sie eine WAF-Basisrichtlinie für Application Gateway, indem Sie die unter [Erstellen von Web Application Firewall-Richtlinien für Application Gateway](create-waf-policy-ag.md) beschriebenen Anleitungen befolgen.

## <a name="enable-bot-protection-rule-set"></a>Aktivieren des Bot-Schutzregelsatzes

1. Wählen Sie auf der Seite mit der **Basisrichtlinie**, die Sie zuvor erstellt haben, unter **Einstellungen** die Option **Regeln**aus.  

2. Aktivieren Sie auf der Detailseite unter dem Abschnitt **Regeln verwalten**  aus dem Dropdownmenü das Kontrollkästchen für die Bot-Schutzregel, und wählen Sie dann **Speichern** aus.

> [!div class="mx-imgBorder"]
> ![Bot-Schutz](../media/bot-protection/bot-protection.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu benutzerdefinierten Regeln finden Sie unter [Benutzerdefinierte Regeln für Web Application Firewall v2 in Azure Application Gateway](custom-waf-rules-overview.md).