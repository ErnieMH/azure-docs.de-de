---
title: 'Problembehandlung: Fehlende Daten in den heruntergeladenen Aktivitätsprotokollen | Microsoft-Dokumentation'
description: Bietet Ihnen eine Lösung für fehlende Daten in heruntergeladenen Azure Active Directory-Aktivitätsprotokollen.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5003d7b644a0c75401a17ed6a37f31acd8180aa9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "85608074"
---
# <a name="i-cant-find-all-the-data-in-the-azure-active-directory-activity-logs-i-downloaded"></a>Ich finde nicht alle Daten in den Azure Active Directory-Aktivitätsprotokollen, die ich heruntergeladen habe.

## <a name="symptoms"></a>Symptome

Ich habe die Aktivitätsprotokolle (Überwachung oder Anmeldungen) heruntergeladen, und für den ausgewählten Zeitraum werden nicht alle Datensätze angezeigt. Warum? 

 ![Berichterstellung](./media/troubleshoot-missing-data-download/01.png)
 
## <a name="cause"></a>Ursache

Beim Herunterladen von Aktivitätsprotokollen im Azure-Portal gilt eine Obergrenze von 250.000 Datensätzen, die nach Aktualität sortiert werden. 

## <a name="resolution"></a>Lösung

Mithilfe von [Azure AD-Berichterstellungs-APIs](concept-reporting-api.md) können jederzeit bis zu einer Millionen Datensätze abgerufen werden.

## <a name="next-steps"></a>Nächste Schritte

* [Azure Active Directory-Berichte: häufig gestellte Fragen](reports-faq.md)

