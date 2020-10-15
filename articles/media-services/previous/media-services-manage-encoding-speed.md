---
title: Verwalten von Geschwindigkeit und Parallelität der Codierung mit Azure Media Services | Microsoft-Dokumentation
description: Dieser Artikel bietet einen kurzen Überblick darüber, wie Sie die Geschwindigkeit und Parallelität Ihrer Codierungsaufträge/-aufgaben mit Azure Media Services verwalten können.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 676313f8-a158-4e3a-a99b-2c29a341ecc9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 4b6f843678d64bddd276f6123a432699efc89ad9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89269283"
---
#  <a name="manage-speed-and-concurrency-of-your-encoding"></a>Verwalten von Geschwindigkeit und Parallelität der Codierung

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)] 

Dieser Artikel bietet einen kurzen Überblick darüber, wie Sie die Geschwindigkeit und Parallelität Ihrer Codierungsaufträge/-aufgaben verwalten können.

## <a name="overview"></a>Übersicht

In Media Services bestimmt der **Typ einer reservierten Einheit** die Geschwindigkeit, mit der Medienverarbeitungsaufgaben erfolgen. Sie können zwischen den folgenden reservierten Einheitentypen wählen: **S1**, **S2** oder **S3**. Derselbe Codierungsauftrag wird bei Verwendung der reservierten Einheit **S2** beispielsweise schneller ausgeführt als mit dem Typ **S1**. Im Thema [Skalieren der Codierung](media-services-scale-media-processing-overview.md) finden Sie eine Tabelle, die Ihnen bei der Entscheidung hilft, wenn Sie zwischen verschiedenen Codierungsgeschwindigkeiten wählen müssen.

Zusätzlich zum Typ reservierter Einheiten können Sie angeben, dass für Ihr Konto **reservierte Einheiten** bereitgestellt werden sollen. Die Anzahl der bereitgestellten reservierten Einheiten bestimmt die Anzahl der Medienaufgaben, die gleichzeitig unter einem angegebenen Konto verarbeitet werden können. Wenn Ihr Konto beispielsweise über fünf reservierte Einheiten verfügt, werden fünf Medienaufgaben gleichzeitig ausgeführt, solange es auszuführende Aufgaben gibt. Die restlichen Aufgaben bleiben in der Warteschlange und werden nacheinander für die Verarbeitung aufgerufen, wenn eine aktive Aufgabe abgeschlossen wird. Wenn für ein Konto keine reservierten Einheiten bereitgestellt wurden, werden die Aufgaben nacheinander aufgerufen. In diesem Fall hängt die Wartezeit zwischen dem Abschließen einer Aufgabe und dem Start der nächsten Aufgabe von der Verfügbarkeit von Ressourcen im System ab.

Ausführliche Informationen und Beispiele für das Skalieren von Codierungseinheiten finden Sie in [diesem](media-services-scale-media-processing-overview.md) Thema.

## <a name="next-step"></a>Nächster Schritt

[Skalieren der Codierung](media-services-scale-media-processing-overview.md)

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

