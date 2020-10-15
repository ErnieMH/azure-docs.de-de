---
title: Leitfaden zur Lizenzierung von B2B-Kollaboration – Azure Active Directory | Microsoft-Dokumentation
description: Für die Azure Active Directory B2B-Kollaboration sind keine kostenpflichtigen Azure AD-Lizenzen erforderlich, aber es sind auch kostenpflichtige Features für B2B-Gastbenutzer erhältlich.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 28d02160163da4081ad8adbe233b27fee970a0df
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87907443"
---
# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Leitfaden zur Lizenzierung von Azure Active Directory B2B-Kollaboration

Mit der Azure Active Directory Business-to-Business-Kollaboration (Azure AD B2B-Kollaboration) können Sie externe Benutzer (oder „Gastbenutzer“) zur Verwendung Ihrer kostenpflichtigen Azure AD-Dienste einladen. Einige Features sind kostenlos, aber für alle kostenpflichtigen Azure AD-Features können Sie bis zu fünf Gastbenutzer für jede Lizenz der Azure AD-Edition einladen, die Sie für einen Mitarbeiter oder einen Nicht-Gastbenutzer in Ihrem Mandanten besitzen.

> [!NOTE]
> Weitere Informationen zu den Preisen von Azure AD und den Features für die B2B-Zusammenarbeit finden Sie unter [Azure Active Directory – Preise](https://azure.microsoft.com/pricing/details/active-directory/).

Die Lizenzierung von B2B-Gastbenutzern wird basierend auf dem Verhältnis von 1:5 automatisch berechnet und gemeldet. 

Darüber hinaus können Gastbenutzer kostenlose Azure AD-Features ohne zusätzliche Lizenzanforderungen nutzen. Gastbenutzer haben Zugriff auf kostenlose Azure AD-Features, auch wenn sie über keine kostenpflichtigen Azure AD-Lizenzen verfügen. 

## <a name="examples-calculating-guest-user-licenses"></a>Beispiele: Berechnen der Lizenzen für Gastbenutzer
Nachdem Sie bestimmt haben, wie viele Gastbenutzer Zugriff auf Ihre kostenpflichtigen Azure AD-Dienste benötigen, müssen Sie sicherstellen, dass Sie über genügend kostenpflichtige Azure AD-Lizenzen verfügen, um Gastbenutzer im erforderlichen Verhältnis von 1:5 abzudecken. Im Folgenden finden Sie einige Beispiele:

- Sie möchten 100 Gastbenutzer für Ihre Azure AD-Apps oder Azure AD-Dienste einladen und die Zugriffsverwaltung und Bereitstellung ermöglichen. Für 50 dieser Gastbenutzer möchten Sie darüber hinaus MFA und bedingten Zugriff festlegen. Für diese Features benötigen Sie daher zehn Azure AD Premium P1-Lizenzen. Wenn Sie Identity Protection-Features für Ihre Gastbenutzer verwenden möchten, benötigen Sie Azure AD Premium P2-Lizenzen im gleichen Verhältnis von 1:5 für die Gastbenutzer.
- Sie möchten 60 Gastbenutzer einladen, für die alle MFA erforderlich ist. Daher müssen Sie über mindestens 12 Azure AD Premium P1-Lizenzen verfügen. Sie haben 10 Mitarbeiter mit Azure AD Premium P1-Lizenzen, sodass entsprechend dem Lizenzierungsverhältnis von 1:5 bis zu 50 Gastbenutzer zulässig sind. Sie müssen zwei zusätzliche Premium P1-Lizenzen erwerben, um 10 zusätzliche Gastbenutzer abzudecken.

## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Zusammenarbeit:

* [Azure Active Directory-Preise](https://azure.microsoft.com/pricing/details/active-directory/)
* [Was ist die Azure AD B2B-Zusammenarbeit?](what-is-b2b.md)
* [Häufig gestellte Fragen zur Azure Active Directory B2B-Zusammenarbeit](faq.md)
