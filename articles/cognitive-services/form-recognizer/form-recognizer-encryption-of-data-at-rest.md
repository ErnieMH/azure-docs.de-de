---
title: Verschlüsselung für ruhende Daten der Formularerkennung
titleSuffix: Azure Cognitive Services
description: Microsoft bietet von Microsoft verwaltete Verschlüsselungsschlüssel an und ermöglicht Ihnen auch die Verwaltung Ihrer Cognitive Services-Abonnements mit Ihren eigenen Schlüsseln, den so genannten kundenseitig verwalteten Schlüsseln (Customer Managed Keys, CMK). In diesem Artikel erfahren Sie mehr über die Datenverschlüsselung im Ruhezustand für die Formularerkennung und wie Sie CMK aktivieren und verwalten können.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 08/28/2020
ms.author: egeaney
ms.openlocfilehash: c959231826125349ecd6a62afe529248f7ac2eec
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91326795"
---
# <a name="form-recognizer-encryption-of-data-at-rest"></a>Verschlüsselung für ruhende Daten der Formularerkennung

Die Azure-Formularerkennung verschlüsselt Ihre Daten beim Speichern in der Cloud automatisch. Durch die Verschlüsselung der Formularerkennung werden Ihre Daten ausreichend geschützt, um den Sicherheits- und Complianceanforderungen Ihrer Organisation gerecht zu werden.

[!INCLUDE [cognitive-services-about-encryption](../includes/cognitive-services-about-encryption.md)]

> [!IMPORTANT]
> Vom Kunden verwaltete Schlüssel sind nur für Ressourcen erhältlich, die nach dem 11. Mai 2020 erstellt wurden. Um CMK mit der Formularerkennung zu verwenden, müssen Sie eine neue Formularerkennungsressource erstellen. Nachdem die Ressource erstellt wurde, können Sie mit Azure Key Vault Ihre verwaltete Identität einrichten.

[!INCLUDE [cognitive-services-cmk](../includes/configure-customer-managed-keys.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Formular zum Anfordern von kundenseitig verwalteten Schlüsseln für die Formularerkennung](https://aka.ms/cogsvc-cmk)
* [Weitere Informationen zu Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)