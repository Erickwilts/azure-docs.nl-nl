---
title: Subclip een video als codering met Azure Media Services
description: In dit onderwerp wordt beschreven hoe u een video subclip wanneer codering met Azure Media Services met .NET SDK
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/09/2019
ms.author: juliako
ms.openlocfilehash: 3d584ee742aa93cdecf4b04d942afb2ed83a7357
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67305096"
---
# <a name="subclip-a-video-when-encoding-with-media-services---net"></a>Subclip een video als codering met Media Services - .NET

U kunt trim of een video subclip wanneer codering met behulp van een [taak](https://docs.microsoft.com/rest/api/media/jobs). Deze functionaliteit werkt met een [transformeren](https://docs.microsoft.com/rest/api/media/transforms) die is gebouwd met behulp van de [BuiltInStandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) presets, of de [StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) voorinstellingen.

De volgende C# voorbeeld maakt u een taak die een video in een actief worden verwijderd omdat deze een coderingstaak indient. 

## <a name="prerequisites"></a>Vereisten

Als u wilt de in dit onderwerp beschreven stappen hebt voltooid, hebt u naar:

- [Een Azure Media Services-account maken](create-account-cli-how-to.md)
- Maken van een transformatie- en invoer en uitvoer van activa. U kunt zien over het maken van een transformatie- en invoer- en Assets in de [uploaden, coderen en streamen van video's met behulp van .NET](stream-files-tutorial-with-api.md) zelfstudie.
- Controleer de [Encoding concept](encoding-concept.md) onderwerp.

## <a name="example"></a>Voorbeeld

```csharp
/// <summary>
/// Submits a request to Media Services to apply the specified Transform to a given input video.
/// </summary>
/// <param name="client">The Media Services client.</param>
/// <param name="resourceGroupName">The name of the resource group within the Azure subscription.</param>
/// <param name="accountName"> The Media Services account name.</param>
/// <param name="transformName">The name of the transform.</param>
/// <param name="jobName">The (unique) name of the job.</param>
/// <param name="inputAssetName">The name of the input asset.</param>
/// <param name="outputAssetName">The (unique) name of the  output asset that will store the result of the encoding job. </param>
// <SubmitJob>
private static async Task<Job> JobWithBuiltInStandardEncoderWithSingleClipAsync(
    IAzureMediaServicesClient client,
    string resourceGroupName,
    string accountName,
    string transformName,
    string jobName,
    string inputAssetName,
    string outputAssetName)
{
    var jobOutputs = new List<JobOutputAsset>
    {
        new JobOutputAsset(state: JobState.Queued, progress: 0, assetName: outputAssetName)
    };

    var clipStart = new AbsoluteClipTime()
    {
        Time = new TimeSpan(0, 0, 20)
    };

    var clipEnd = new AbsoluteClipTime()
    {
        Time = new TimeSpan(0, 0, 30)
    };

    var jobInput = new JobInputAsset(assetName: inputAssetName, start: clipStart, end: clipEnd);

    Job job = await client.Jobs.CreateAsync(
        resourceGroupName,
        accountName,
        transformName,
        jobName,
        new Job(input: jobInput, outputs: jobOutputs.ToArray(), name: jobName)
        {
            Description = $"A Job with transform {transformName} and single clip.",
            Priority = Priority.Normal,
        });

    return job;
}
```

## <a name="next-steps"></a>Volgende stappen

[Coderen met een aangepaste transformatie](customize-encoder-presets-how-to.md) 
