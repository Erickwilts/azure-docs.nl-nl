---
title: Videotranscriptie beoordelingen met behulp van .NET - Content Moderator maken
titlesuffix: Azure Cognitive Services
description: Maken van videotranscriptie beoordelingen met de Content Moderator-SDK voor .NET
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 03/19/2019
ms.author: sajagtap
ms.openlocfilehash: fa782f687979f1d32cdf1c18bd08f6672e39adfe
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64868596"
---
# <a name="create-video-transcript-reviews-using-net"></a>Maken van videotranscriptie beoordelingen met behulp van .NET

In dit artikel vindt u informatie en voorbeelden van code om u te helpen snel aan de slag met de [Content Moderator-SDK met C#](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) aan:

- Maken van een video bekijken voor menselijke moderators
- Een gecontroleerde transcript toevoegen aan de beoordeling
- Publiceren van de beoordeling

## <a name="prerequisites"></a>Vereisten

- Meld u aan of maak een account op de Content Moderator [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com/) site als u nog niet hebt gedaan.
- In dit artikel wordt ervan uitgegaan dat u hebt [onder toezicht van de video](video-moderation-api.md) en [gemaakt van de video beoordeling](video-reviews-quickstart-dotnet.md) in het controlehulpprogramma menselijke beslissingen te nemen. Nu u met toezicht transcripts van video in het beoordelingsprogramma toevoegen.

## <a name="ensure-your-api-key-can-call-the-review-api-job-creation"></a>Zorg ervoor dat uw API-sleutel (project maken) van de beoordeling-API kunt aanroepen

Nadat u de vorige stappen hebt uitgevoerd, zou u twee Content Moderator-sleutels kunnen hebben als u vanuit de Azure-portal bent gestart.

Als u van plan bent de door Azure verstrekte API-sleutel in uw SDK-voorbeeld te gebruiken, volgt u de stappen in de sectie [De Azure-sleutel met de beoordelings-API gebruiken](./review-tool-user-guide/configure.md#use-your-azure-account-with-the-review-apis) zodat uw app de beoordelings-API kan aanroepen en beoordelingen kan maken.

Als u de gratis proefversie van de sleutel gebruikt die wordt gegenereerd door het beoordelingsprogramma, is uw account van het beoordelingsprogramma al op de hoogte van de sleutel, zodat er geen extra stappen zijn vereist.

## <a name="prepare-your-video-for-review"></a>Uw video voorbereiden voor controle

Het transcript toevoegen aan een video beoordeling. De video moet online zijn gepubliceerd. U moet het streaming-eindpunt. Het streaming-eindpunt kunt de video speler van revisie hulpprogramma de video afspelen.

![Videodemo miniatuur](images/ams-video-demo-view.PNG)

- Kopieer de **URL** op deze [demo van Azure Media Services](https://aka.ms/azuremediaplayer?url=https%3A%2F%2Famssamples.streaming.mediaservices.windows.net%2F91492735-c523-432b-ba01-faba6c2206a2%2FAzureMediaServicesPromo.ism%2Fmanifest) -pagina voor de manifest-URL.

## <a name="create-your-visual-studio-project"></a>Het Visual Studio-project maken

1. Voeg een nieuw project van het type **Console app (.NET Framework)** toe aan uw oplossing.

1. Noem het project **VideoTranscriptReviews**.

1. Selecteer dit project als het enige opstartproject voor de oplossing.

### <a name="install-required-packages"></a>De vereiste pakketten installeren

Installeer de volgende NuGet-pakketten voor het project TermLists.

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Microsoft.Rest.ClientRuntime.Azure
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>De using-instructies van het programma bijwerken

Wijzig het programma de using-instructies toe als volgt.


```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Threading;
using Microsoft.Azure.CognitiveServices.ContentModerator;
using Microsoft.CognitiveServices.ContentModerator;
using Microsoft.CognitiveServices.ContentModerator.Models;
using Newtonsoft.Json;
```

### <a name="add-private-properties"></a>Private-eigenschappen toevoegen

De volgende persoonlijke eigenschappen toevoegen aan de naamruimte VideoTranscriptReviews, klasse Program.

Indien vermeld, vervang de voorbeeldwaarden voor deze eigenschappen.

```csharp
namespace VideoReviews
{
    class Program
    {
        // NOTE: Replace this example location with the location for your Content Moderator account.
        /// <summary>
        /// The region/location for your Content Moderator account, 
        /// for example, westus.
        /// </summary>
        private static readonly string AzureRegion = "YOUR CONTENT MODERATOR REGION";

        // NOTE: Replace this example key with a valid subscription key.
        /// <summary>
        /// Your Content Moderator subscription key.
        /// </summary>
        private static readonly string CMSubscriptionKey = "YOUR CONTENT MODERATOR KEY";

        // NOTE: Replace this example team name with your Content Moderator team name.
        /// <summary>
        /// The name of the team to assign the job to.
        /// </summary>
        /// <remarks>This must be the team name you used to create your 
        /// Content Moderator account. You can retrieve your team name from
        /// the Content Moderator web site. Your team name is the Id associated 
        /// with your subscription.</remarks>
        private const string TeamName = "YOUR CONTENT MODERATOR TEAM ID";

        /// <summary>
        /// The base URL fragment for Content Moderator calls.
        /// </summary>
        private static readonly string AzureBaseURL =
            $"{AzureRegion}.api.cognitive.microsoft.com";

        /// <summary>
        /// The minimum amount of time, in milliseconds, to wait between calls
        /// to the Content Moderator APIs.
        /// </summary>
        private const int throttleRate = 2000;
```

### <a name="create-content-moderator-client-object"></a>Content Moderator-clientobject maken

De methodedefinitie van de volgende aan naamruimte VideoTranscriptReviews, klasse programma toevoegen.

```csharp
/// <summary>
/// Returns a new Content Moderator client for your subscription.
/// </summary>
/// <returns>The new client.</returns>
/// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
/// When you have finished using the client,
/// you should dispose of it either directly or indirectly. </remarks>
public static ContentModeratorClient NewClient()
{
    return new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey))
    {
        Endpoint = AzureBaseURL
    };
}
```

## <a name="create-a-video-review"></a>Maken van een video beoordeling

Maken van een video bekijken met **ContentModeratorClient.Reviews.CreateVideoReviews**. Zie voor meer informatie de [API-naslaghandleiding](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4).

**CreateVideoReviews** heeft de volgende vereiste parameters:
1. Een tekenreeks met een MIME-type moet ' application/json'. 
1. De naam van uw Content Moderator-team.
1. Een **IList\<CreateVideoReviewsBodyItem >** object. Elke **CreateVideoReviewsBodyItem** object vertegenwoordigt een video beoordeling. In deze Quick Start maakt een beoordeling op een tijdstip.

**CreateVideoReviewsBodyItem** heeft een aantal eigenschappen. Ten minste, moet u de volgende eigenschappen instellen:
- **Inhoud**. De URL van de video moet worden gecontroleerd.
- **ContentId**. Een ID om toe te wijzen aan de video beoordeling.
- **Status**. Stel de waarde "Unpublished." Als u deze niet instelt, wordt de standaardwaarde op 'In behandeling', wat betekent dat de video beoordeling is gepubliceerd en in afwachting van menselijke beoordeling. Zodra een beoordeling van de video is gepubliceerd, kunt u niet langer videoframes, een transcript of een transcript toezicht resultaat aan toevoegen.

> [!NOTE]
> **CreateVideoReviews** retourneert een IList\<string >. Deze tekenreeksen bevat een ID voor een video beoordeling. Deze id's zijn GUID's en zijn niet hetzelfde als de waarde van de **ContentId** eigenschap.

De methodedefinitie van de volgende aan naamruimte VideoReviews, klasse programma toevoegen.

```csharp
/// <summary>
/// Create a video review. For more information, see the API reference:
/// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4 
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="id">The ID to assign to the video review.</param>
/// <param name="content">The URL of the video to review.</param>
/// <returns>The ID of the video review.</returns>
private static string CreateReview(ContentModeratorClient client, string id, string content)
{
    Console.WriteLine("Creating a video review.");

    List<CreateVideoReviewsBodyItem> body = new List<CreateVideoReviewsBodyItem>() {
        new CreateVideoReviewsBodyItem
        {
            Content = content,
            ContentId = id,
            /* Note: to create a published review, set the Status to "Pending".
            However, you cannot add video frames or a transcript to a published review. */
            Status = "Unpublished",
        }
    };

    var result = client.Reviews.CreateVideoReviews("application/json", TeamName, body);

    Thread.Sleep(throttleRate);

    // We created only one review.
    return result[0];
}
```

> [!NOTE]
> Er geldt een limiet voor het aantal aanvragen per seconde (RPS) voor de sleutel van de Content Moderator-service. Als u de limiet overschrijdt, veroorzaakt dit een uitzondering met foutcode 429 in de SDK.
>
> Een sleutel voor de gratis laag heeft een limiet van één RPS.

## <a name="add-transcript-to-video-review"></a>Transcript aan video revisie toevoegen

U een transcript toevoegen aan een video bekijken met **ContentModeratorClient.Reviews.AddVideoTranscript**. **AddVideoTranscript** heeft de volgende vereiste parameters:
1. Uw Content Moderator-team-id.
1. De video revisie-ID die wordt geretourneerd door **CreateVideoReviews**.
1. Een **Stream** -object dat het transcript bevat.

Het transcript moet zich in de WebVTT-indeling. Zie voor meer informatie, [WebVTT: De tekst van Web-Video wordt bijgehouden indeling](https://www.w3.org/TR/webvtt1/).

> [!NOTE]
> Het programma gebruikt een transcript voorbeeld in de VTT-indeling. In een echte oplossing, gebruikt u de service Azure Media Indexer [genereren een transcript](https://docs.microsoft.com/azure/media-services/media-services-index-content) van een video.

De methodedefinitie van de volgende aan naamruimte VideotranscriptReviews, klasse programma toevoegen.

```csharp
/// <summary>
/// Add a transcript to the indicated video review.
/// The transcript must be in the WebVTT format.
/// For more information, see the API reference:
/// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b8b2e7151f0b10d451fe
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="review_id">The video review ID.</param>
/// <param name="transcript">The video transcript.</param>
static void AddTranscript(ContentModeratorClient client, string review_id, string transcript)
{
    Console.WriteLine("Adding a transcript to the review with ID {0}.", review_id);
    client.Reviews.AddVideoTranscript(TeamName, review_id, new MemoryStream(System.Text.Encoding.UTF8.GetBytes(transcript)));
    Thread.Sleep(throttleRate);
}
```

## <a name="add-a-transcript-moderation-result-to-video-review"></a>Toevoegen van een transcript toezicht resultaat naar video-overzicht

Naast het toevoegen van een transcript naar een video-overzicht, moet u ook het resultaat van toezicht houden op dit transcript toevoegen. U doet met **ContentModeratorClient.Reviews.AddVideoTranscriptModerationResult**. Zie voor meer informatie de [API-naslaghandleiding](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b93ce7151f0b10d451ff).

**AddVideoTranscriptModerationResult** heeft de volgende vereiste parameters:
1. Een tekenreeks met een MIME-type moet ' application/json'. 
1. De naam van uw Content Moderator-team.
1. De video revisie-ID die wordt geretourneerd door **CreateVideoReviews**.
1. An IList\<TranscriptModerationBodyItem>. Een **TranscriptModerationBodyItem** heeft de volgende eigenschappen:
1. **Voorwaarden**. An IList\<TranscriptModerationBodyItemTermsItem>. Een **TranscriptModerationBodyItemTermsItem** heeft de volgende eigenschappen:
1. **Index**. De op nul gebaseerde index van de termijn.
1. **Term**. Een tekenreeks is die de term bevat.
1. **Tijdstempel**. Een tekenreeks is die bevat, in seconden, de tijd in het transcript waarop de voorwaarden worden gevonden.

Het transcript moet zich in de WebVTT-indeling. Zie voor meer informatie, [WebVTT: De tekst van Web-Video wordt bijgehouden indeling](https://www.w3.org/TR/webvtt1/).

De methodedefinitie van de volgende aan naamruimte VideoTranscriptReviews, klasse programma toevoegen. Deze methode verzendt een transcript naar de **ContentModeratorClient.TextModeration.ScreenText** methode. Ook wordt het resultaat omgezet in een IList\<TranscriptModerationBodyItem >, en verzendt naar **AddVideoTranscriptModerationResult**.

```csharp
/// <summary>
/// Add the results of moderating a video transcript to the indicated video review.
/// For more information, see the API reference:
/// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b93ce7151f0b10d451ff
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="review_id">The video review ID.</param>
/// <param name="transcript">The video transcript.</param>
static void AddTranscriptModerationResult(ContentModeratorClient client, string review_id, string transcript)
{
    Console.WriteLine("Adding a transcript moderation result to the review with ID {0}.", review_id);

    // Screen the transcript using the Text Moderation API. For more information, see:
    // https://westus2.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f
    Screen screen = client.TextModeration.ScreenText("eng", "text/plain", transcript);

    // Map the term list returned by ScreenText into a term list we can pass to AddVideoTranscriptModerationResult.
    List<TranscriptModerationBodyItemTermsItem> terms = new List<TranscriptModerationBodyItemTermsItem>();
    if (null != screen.Terms)
    {
        foreach (var term in screen.Terms)
        {
            if (term.Index.HasValue)
            {
                terms.Add(new TranscriptModerationBodyItemTermsItem(term.Index.Value, term.Term));
            }
        }
    }

    List<TranscriptModerationBodyItem> body = new List<TranscriptModerationBodyItem>()
    {
        new TranscriptModerationBodyItem()
        {
            Timestamp = "0",
            Terms = terms
        }
    };

    client.Reviews.AddVideoTranscriptModerationResult("application/json", TeamName, review_id, body);

    Thread.Sleep(throttleRate);
}
```

## <a name="publish-video-review"></a>Bekijk video publiceren

Publiceren van een video bekijken met **ContentModeratorClient.Reviews.PublishVideoReview**. **PublishVideoReview** heeft de volgende vereiste parameters:
1. De naam van uw Content Moderator-team.
1. De video revisie-ID die wordt geretourneerd door **CreateVideoReviews**.

De methodedefinitie van de volgende aan naamruimte VideoReviews, klasse programma toevoegen.

```csharp
/// <summary>
/// Publish the indicated video review. For more information, see the reference API:
/// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7bb29e7151f0b10d45201
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="review_id">The video review ID.</param>
private static void PublishReview(ContentModeratorClient client, string review_id)
{
    Console.WriteLine("Publishing the review with ID {0}.", review_id);
    client.Reviews.PublishVideoReview(TeamName, review_id);
    Thread.Sleep(throttleRate);
}
```

## <a name="putting-it-all-together"></a>Alles samenvoegen

Voeg de **Main** methodedefinitie naamruimte VideoTranscriptReviews, klasse programma. Tot slot sluit de klasse Program en de VideoTranscriptReviews-naamruimte.

> [!NOTE]
> Het programma gebruikt een transcript voorbeeld in de VTT-indeling. In een echte oplossing, gebruikt u de service Azure Media Indexer [genereren een transcript](https://docs.microsoft.com/azure/media-services/media-services-index-content) van een video.

```csharp
static void Main(string[] args)
{
    using (ContentModeratorClient client = NewClient())
    {
        // Create a review with the content pointing to a streaming endpoint (manifest)
        var streamingcontent = "https://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest";
        string review_id = CreateReview(client, "review1", streamingcontent);

        var transcript = @"WEBVTT

        01:01.000 --> 02:02.000
        First line with a negative word in a transcript.

        02:03.000 --> 02:25.000
        This is another line in the transcript.
        ";

        AddTranscript(client, review_id, transcript);

        AddTranscriptModerationResult(client, review_id, transcript);

        // Publish the review
        PublishReview(client, review_id);

        Console.WriteLine("Open your Content Moderator Dashboard and select Review > Video to see the review.");
        Console.WriteLine("Press any key to close the application.");
        Console.ReadKey();
    }
}
```

## <a name="run-the-program-and-review-the-output"></a>Het programma uitvoeren en de uitvoer controleren

Als u de toepassing uitvoert, ziet u uitvoer op de volgende regels:

```console
Creating a video review.
Adding a transcript to the review with ID 201801v5b08eefa0d2d4d64a1942aec7f5cacc3.
Adding a transcript moderation result to the review with ID 201801v5b08eefa0d2d4d64a1942aec7f5cacc3.
Publishing the review with ID 201801v5b08eefa0d2d4d64a1942aec7f5cacc3.
Open your Content Moderator Dashboard and select Review > Video to see the review.
Press any key to close the application.
```

## <a name="navigate-to-your-video-transcript-review"></a>Navigeer naar de videotranscriptie controleren

Ga naar de videotranscriptie controle in uw Content Moderator-controlehulpprogramma op de **bekijken**>**Video**>**Transcript** scherm.

Ziet u de volgende functies:
- De twee regels van de tekst die u hebt toegevoegd
- De term grof taalgebruik gevonden en gemarkeerd door de service van de toezicht op tekst
- De video begint selecteren van een tekst transcriptie met tijdstempel

![Videotranscriptie beoordeling van menselijke moderators](images/ams-video-transcript-review.PNG)

## <a name="next-steps"></a>Volgende stappen

Krijgen de [Content Moderator .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) en de [Visual Studio-oplossing](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) voor deze en andere Content Moderator-snelstartgidsen voor .NET.

Meer informatie over het genereren van [video bekijkt](video-reviews-quickstart-dotnet.md) in het controlehulpprogramma.

Bekijk de gedetailleerde zelfstudie over het ontwikkelen van een [videotoezicht oplossing voltooien](video-transcript-moderation-review-tutorial-dotnet.md).
