---
title: Erstellen eines Auftrags mit mehreren Transformationsausgaben
description: In diesem Thema wird gezeigt, wie Sie einen Azure Media Services-Auftrag mit mehreren Transformationsausgaben erstellen.
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
ms.custom: devx-track-csharp
ms.openlocfilehash: a40ce1fd7b3d9d946641c91f5876d8c73755b005
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2021
ms.locfileid: "101092266"
---
# <a name="create-a-job-with-multiple-transform-outputs"></a>Erstellen eines Auftrags mit mehreren Transformationsausgaben

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

In diesem Thema wird gezeigt, wie Sie eine Transformation mit zwei Transformationsausgaben erstellen können. Die erste fordert die Codierung der Eingabe für Adaptive Bitrate Streaming mit einer integrierten [AdaptiveStreaming](encoding-concept.md#builtinstandardencoderpreset)-Voreinstellung. Die zweite fordert, dass das Audiosignal im Eingangsvideo mit [AudioAnalyzerPreset](analyzing-video-audio-files-concept.md#built-in-presets) verarbeitet wird. Nachdem die Transformation erstellt wurde, können Sie einen Auftrag übermitteln, der Ihr Video entsprechend verarbeitet. Da in diesem Beispiel zwei Transformationsausgaben angegeben werden, müssen zwei Auftragsausgaben angegeben werden. Sie können wählen, ob Sie beide Auftragsausgaben an dasselbe Objekt leiten (wie unten gezeigt) oder die Ergebnisse in separate Objekte schreiben lassen möchten.
 

> [!TIP]
> Bevor Sie mit der Entwicklung beginnen, lesen Sie [Entwickeln mit Media Services v3-APIs](media-services-apis-overview.md) (Informationen zum Zugreifen auf APIs, Namenskonventionen usw.).

## <a name="create-a-transform"></a>Erstellen einer Transformation

Der folgende Code zeigt, wie Sie eine Transformation erstellen, die zwei Ausgaben erzeugt.

```csharp
private static async Task<Transform> GetOrCreateTransformAsync(
    IAzureMediaServicesClient client,
    string resourceGroupName,
    string accountName,
    string transformName)
{
    // Does a Transform already exist with the desired name? Assume that an existing Transform with the desired name
    // also uses the same recipe or Preset for processing content.
    Transform transform = await client.Transforms.GetAsync(resourceGroupName, accountName, transformName);

    if (transform == null)
    {
        // You need to specify what you want it to produce as an output
        TransformOutput[] output = new TransformOutput[]
        {
            new TransformOutput
            {
                Preset = new BuiltInStandardEncoderPreset()
                {
                    // This sample uses the built-in encoding preset for Adaptive Bitrate Streaming.
                    PresetName = EncoderNamedPreset.AdaptiveStreaming
                }
            },
            // Create an analyzer preset with video insights.
            new TransformOutput(new AudioAnalyzerPreset("en-US"))
        };

        // Create the Transform with the output defined above
        transform = await client.Transforms.CreateOrUpdateAsync(resourceGroupName, accountName, transformName, output);
    }

    return transform;
}
```

## <a name="submit-a-job"></a>Übermitteln eines Auftrags

Erstellen Sie einen Auftrag mit einer HTTPS-URL-Eingabe und mit zwei Auftragsausgaben.

```csharp
private static async Task<Job> SubmitJobAsync(IAzureMediaServicesClient client,
    string resourceGroup,
    string accountName,
    string transformName)
{
    // Output from the encoding Job must be written to an Asset, so let's create one
    string outputAssetName1 = $"output-" + Guid.NewGuid().ToString("N");
    Asset outputAsset = await client.Assets.CreateOrUpdateAsync(resourceGroup, accountName, outputAssetName1, new Asset());

    // This example shows how to encode from any HTTPs source URL - a new feature of the v3 API.  
    // Change the URL to any accessible HTTPs URL or SAS URL from Azure.
    JobInputHttp jobInput =
        new JobInputHttp(files: new[] { "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/Ignite-short.mp4" });

    JobOutput[] jobOutputs =
    {
        // Since we are specifying two Transform Outputs, two Job Outputs are needed.
        // In this example, the first Job Output is for the results from adaptive bitrate encoding,
        // and the second is for the results from audio analysis. In this example, both are written to the
        // same output Asset. Or, you can specify different Assets.
        
        new JobOutputAsset(outputAsset.Name),
        new JobOutputAsset(outputAsset.Name)

    };

    // In this example, we are using a unique job name.
    //
    // If you already have a job with the desired name, use the Jobs.Get method
    // to get the existing job. In Media Services v3, Get methods on entities returns null 
    // if the entity doesn't exist (a case-insensitive check on the name).
    Job job;
    try
    {
        string jobName = $"job-" + Guid.NewGuid().ToString("N");
        job = await client.Jobs.CreateAsync(
                    resourceGroup,
                    accountName,
                    transformName,
                    jobName,
                    new Job
                    {
                        Input = jobInput,
                        Outputs = jobOutputs,
                    });
    }
    catch (Exception exception)
    {
        if (exception.GetBaseException() is ApiErrorException apiException)
        {
            Console.Error.WriteLine(
                $"ERROR: API call failed with error code '{apiException.Body.Error.Code}' and message '{apiException.Body.Error.Message}'.");
        }
        throw exception;
    }

    return job;
}
```
## <a name="job-error-codes"></a>Auftragsfehlercodes

Weitere Informationen finden Sie unter [Fehlercodes](/rest/api/media/jobs/get#joberrorcode).

## <a name="next-steps"></a>Nächste Schritte

[Azure Media Services v3-Beispiele mit .NET](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/master/) 
