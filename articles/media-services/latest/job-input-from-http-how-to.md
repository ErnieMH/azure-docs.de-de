---
title: Erstellen einer Auftragseingabe aus einer HTTPS-URL
description: In diesem Thema wird gezeigt, wie Sie eine Azure Media Services-Auftragseingabe aus einer HTTPS-URL erstellen.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: how-to
ms.date: 08/31/2020
ms.author: inhenkel
ms.openlocfilehash: 1073a13f477894e1b35d732fd1cd6191747a62a2
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98955986"
---
# <a name="create-a-job-input-from-an-https-url"></a>Erstellen einer Auftragseingabe aus einer HTTPS-URL

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Wenn Sie in Media Services v3 Aufträge zur Verarbeitung Ihrer Videos übermitteln, müssen Sie Media Services mitteilen, wo sich das Eingabevideo befindet. Eine der Optionen ist die Angabe einer HTTPS-URL als Auftragseingabe (wie im folgenden Beispiel gezeigt). Beachten Sie, dass AMS v3 derzeit keine segmentierte Transfercodierung über HTTPS-URLs unterstützt. Ein vollständiges Beispiel finden Sie in diesem [GitHub-Beispiel](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

> [!TIP]
> Bevor Sie mit der Entwicklung beginnen, lesen Sie [Entwickeln mit Media Services v3-APIs](media-services-apis-overview.md) (Informationen zum Zugreifen auf APIs, Namenskonventionen usw.).

## <a name="net-sample"></a>.NET-Beispiel

Der folgende Code zeigt, wie Sie einen Auftrag mit einer HTTPS-URL-Eingabe erstellen.

[!code-csharp[Main](../../../media-services-v3-dotnet-quickstarts/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="job-error-codes"></a>Auftragsfehlercodes

Weitere Informationen finden Sie unter [Fehlercodes](/rest/api/media/jobs/get#joberrorcode).

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer Auftragseingabe aus einer lokalen Datei](job-input-from-local-file-how-to.md)
