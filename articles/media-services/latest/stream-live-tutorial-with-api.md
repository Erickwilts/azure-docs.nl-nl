---
title: .NET gebruiken voor live streamen met Azure Media Services v3 | Microsoft Docs
description: In deze zelfstudie doorloopt u de stappen voor live streamen met Media Services v3 met behulp van .NET Core.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 06/13/2019
ms.author: juliako
ms.openlocfilehash: 8bac9b178aef1ddee396d94a193d9b9262cd6fce
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67123074"
---
# <a name="tutorial-stream-live-with-media-services-v3-using-net"></a>Zelfstudie: Live streamen met Azure Media Services v3 met behulp van .NET

In Azure Media Services zijn [livegebeurtenissen](https://docs.microsoft.com/rest/api/media/liveevents) verantwoordelijk voor het verwerken inhoud voor live streamen. Een livegebeurtenis biedt een invoereindpunt (de URL voor opnemen) dat u vervolgens doorgeeft aan een live-encoder. De livegebeurtenis ontvangt live-invoerstromen van de live-encoder en maakt deze beschikbaar voor streaming via een of meer [streaming-eindpunten](https://docs.microsoft.com/rest/api/media/streamingendpoints). Livegebeurtenissen bieden ook een preview-eindpunt (voorbeeld-URL) dat u kunt gebruiken om een voorbeeld van de stream te bekijken en deze te valideren voordat deze verder wordt verwerkt en geleverd. In deze zelfstudie ziet u hoe u .NET Core gebruikt om een **pass-through**-type van een live-gebeurtenis te maken. 

> [!NOTE]
> Zorg dat u [Live streamen met Media Services v3](live-streaming-overview.md) hebt gelezen voordat u verder gaat. 

In deze zelfstudie leert u het volgende:    

> [!div class="checklist"]
> * Downloaden van de voorbeeld-app zoals beschreven in het onderwerp
> * De code bestuderen die live streamen uitvoert
> * Bekijk de gebeurtenis met [Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/index.html) op https://ampdemo.azureedge.net
> * Resources opschonen

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Hieronder wordt aangegeven wat de vereisten zijn om de zelfstudie te voltooien.

- Installeer Visual Studio Code of Visual Studio.
- [Een Azure Media Services-account maken](create-account-cli-how-to.md).<br/>Vergeet niet de waarden die u hebt gebruikt voor de namen van de resourcegroep en het Media Services-account.
- Volg de stappen in [Access Azure Media Services API with the Azure CLI](access-api-cli-how-to.md) (Toegang tot de Azure Media Services-API met de Azure CLI) en sla de referenties op. U hebt deze nodig voor toegang tot de API.
- Een camera of een apparaat (zoals een laptop) die wordt gebruikt om een gebeurtenis uit te zenden.
- Een on-premises live-encoder die signalen van de camera converteert naar stromen die naar een Media Service-service voor live streamen worden verzonden. De stroom moet de **RTMP**- of **Smooth Streaming**-indeling hebben.

## <a name="download-and-configure-the-sample"></a>Het voorbeeld downloaden en configureren

Gebruik de volgende opdracht om een GitHub-opslagplaats te klonen op uw computer die het .NET-voorbeeld voor het streamen van video bevat:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```

Het voorbeeld voor live streamen staat in de map [Live](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/Live/MediaV3LiveApp).

Open [appsettings.json](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/appsettings.json) in het project dat u hebt gedownload. Vervang de waarden door referenties die u hebt verkregen via [toegang tot API's](access-api-cli-how-to.md).

> [!IMPORTANT]
> In dit voorbeeld wordt een uniek achtervoegsel voor elke resource gebruikt. Als u de foutopsporing annuleert of de app beëindigt zonder deze helemaal door te lopen, blijven er meerdere livegebeurtenissen in uw account staan. <br/>Zorg dat u de actieve livegebeurtenissen stopt. Anders worden deze **in rekening gebracht**!

## <a name="examine-the-code-that-performs-live-streaming"></a>De code bestuderen die live streamen uitvoert

In dit gedeelte worden de functies bekeken die zijn gedefinieerd in het bestand [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs) van het project *MediaV3LiveApp*.

In het voorbeeld wordt een uniek achtervoegsel voor elke resource gemaakt, zodat er geen naamconflicten optreden als u het voorbeeld meerdere keren uitvoert zonder dat er wordt opgeschoond.

> [!IMPORTANT]
> In dit voorbeeld wordt een uniek achtervoegsel voor elke resource gebruikt. Als u de foutopsporing annuleert of de app beëindigt zonder deze helemaal door te lopen, blijven er meerdere livegebeurtenissen in uw account staan. <br/>
> Zorg dat u de actieve livegebeurtenissen stopt. Anders worden deze **in rekening gebracht**!
 
### <a name="start-using-media-services-apis-with-net-sdk"></a>Starten met het gebruik van Media Services API's met .NET SDK

Als u wilt starten met Media Services API's met .NET, moet u een **AzureMediaServicesClient**-object maken. Als u het object wilt maken, moet u referenties opgeven die de client nodig heeft om verbinding te maken met Azure met behulp van Microsoft Azure Active Directory. In de code die u aan het begin van het artikel hebt gekloond, wordt met de functie **GetCredentialsAsync** het object ServiceClientCredentials gemaakt op basis van de referenties die zijn opgegeven in het lokale configuratiebestand. 

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateMediaServicesClient)]

### <a name="create-a-live-event"></a>Een livegebeurtenis maken

In deze sectie wordt beschreven hoe u een livegebeurtenis van het type **pass-through** maakt (LiveEventEncodingType ingesteld op None). Zie voor meer informatie over de beschikbare typen Live gebeurtenissen [Live gebeurtenistypen](live-events-outputs-concept.md#live-event-types). 
 
Er zijn enkele dingen die u opgeven wilt misschien bij het maken van de live-gebeurtenis:

* Media Services-locatie 
* Het streaming-protocol voor de livegebeurtenis (momenteel worden de protocollen RTMP en Smooth Streaming ondersteund).<br/>U kunt de protocoloptie niet wijzigen terwijl de livegebeurtenis of de daaraan gekoppelde live-uitvoer worden uitgevoerd. Als u verschillende protocollen nodig hebt, maakt u voor elk streaming-protocol een afzonderlijke livegebeurtenis.  
* IP-beperkingen voor de opname en voorbeeldweergave. U kunt de IP-adressen definiëren die zijn toegestaan om een video van deze livegebeurtenis op te nemen. Toegestane IP-adressen kunnen worden opgegeven als één IP-adres (bijvoorbeeld 10.0.0.1), een IP-adresbereik met een IP-adres en een CIDR-subnetmasker (bijvoorbeeld 10.0.0.1/22) of een IP-adresbereik met een IP-adres en een decimaal subnetmasker met punten (bijvoorbeeld , ' 10.0.0.1(255.255.252.0)').<br/>Als geen IP-adressen zijn opgegeven en er geen regeldefinitie bestaat, zijn er geen IP-adressen toegestaan. Als u IP-adres(sen) wilt toestaan, maakt u een regel en stelt u 0.0.0.0/0 in.<br/>De IP-adressen moeten een van de volgende indelingen hebben: IpV4-adres met 4 cijfers, CIDR-adresbereik.
* Bij het maken van de gebeurtenis kunt u opgeven dat deze automatisch wordt gestart. <br/>Wanneer autostart is ingesteld op True, wordt de Live gebeurtenis gestart na het maken ervan. Dit betekent dat facturering begint zodra de livegebeurtenis begint. U moet expliciet Stop aanroepen in de resource van de livegebeurtenis om verdere facturering stop te zetten. Zie [Live Event states and billing](live-event-states-billing.md) (Statussen en facturering voor livegebeurtenissen) voor meer informatie.
* Voor een URL voor opnemen worden voorspeld, stelt u de modus 'aangepaste'. Zie voor gedetailleerde informatie [URL's voor Live gebeurtenis opnemen](live-events-outputs-concept.md#live-event-ingest-urls).

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveEvent)]

### <a name="get-ingest-urls"></a>URL’s voor opnemen ophalen

Wanneer de livegebeurtenis is gemaakt, kunt u URL's voor opnemen ophalen die u aan de live-encoder levert. Het coderingsprogramma gebruikt deze URL's voor het invoeren van een live stream.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetIngestURL)]

### <a name="get-the-preview-url"></a>De voorbeeld-URL ophalen

Gebruik het previewEndpoint als u een voorbeeld wilt bekijken en wilt controleren of de invoer van de encoder daadwerkelijk wordt ontvangen.

> [!IMPORTANT]
> Controleer voordat u verder gaat of video naar de voorbeeld-URL stroomt.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#GetPreviewURLs)]

### <a name="create-and-manage-live-events-and-live-outputs"></a>Livegebeurtenissen en live-uitvoer maken en beheren

Wanneer de stream naar de livegebeurtenis stroomt, kunt u de streaming-gebeurtenis starten door een Asset, live-uitvoer en streaming-locator te maken. Hiermee wordt de stream gearchiveerd en beschikbaar gesteld aan kijkers via het streaming-eindpunt. 

#### <a name="create-an-asset"></a>Een Asset maken

Maak een Asset die u met de live-uitvoer wilt gebruiken.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateAsset)]

#### <a name="create-a-live-output"></a>Een live-uitvoer maken

Live-uitvoer starten zodra ze zijn gemaakt en stoppen wanneer ze worden verwijderd. Wanneer u de live-uitvoer verwijdert, verwijdert u de onderliggende Asset en de inhoud van de Asset niet.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateLiveOutput)]

#### <a name="create-a-streaming-locator"></a>Een streaming-locator maken

> [!NOTE]
> Wanneer uw Media Services-account is gemaakt, wordt er een **standaard** streaming-eindpunt met de status **Gestopt** aan uw account toegevoegd. Als u inhoud wilt streamen en gebruik wilt maken van [dynamische pakketten](dynamic-packaging-overview.md) en dynamische versleuteling, moet het streaming-eindpunt van waar u inhoud wilt streamen, de status **Wordt uitgevoerd** hebben. 

Als u de Asset van de live-uitvoer publiceert met een streaming-locator, blijft de livegebeurtenis (tot de maximale DVR-duur) zichtbaar tot de streaming-locator verloopt of wordt verwijderd (welke het eerst van toepassing is).

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CreateStreamingLocator)]

```csharp

// Get the url to stream the output
ListPathsResponse paths = await client.StreamingLocators.ListPathsAsync(resourceGroupName, accountName, locatorName);

foreach (StreamingPath path in paths.StreamingPaths)
{
    UriBuilder uriBuilder = new UriBuilder();
    uriBuilder.Scheme = "https";
    uriBuilder.Host = streamingEndpoint.HostName;

    uriBuilder.Path = path.Paths[0];
    // Get the URL from the uriBuilder: uriBuilder.ToString()
}
```

### <a name="cleaning-up-resources-in-your-media-services-account"></a>Resources in uw Media Services-account opschonen

Als u klaar bent met het streamen van gebeurtenissen en de resources wilt opruimen die eerder zijn ingericht, volgt u de volgende procedure.

* Stop het pushen van de stream vanuit het coderingsprogramma.
* Stop de livegebeurtenis. Zodra de livegebeurtenis is gestopt, worden hiervoor geen kosten meer in rekening gebracht. Als u het kanaal opnieuw wilt starten, wordt dezelfde URL voor opnemen gebruikt, zodat u het coderingsprogramma niet opnieuw hoeft te configureren.
* U kunt uw streaming-eindpunt stoppen, tenzij u het archief van uw live gebeurtenis wilt blijven leveren als stream op aanvraag. Voor een livegebeurtenis die is gestopt, worden er geen kosten meer in rekening gebracht.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLiveEventAndOutput)]

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupLocatorAssetAndStreamingEndpoint)]

In de volgende code ziet u hoe u alle livegebeurtenissen uit uw account verwijdert:

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/Live/MediaV3LiveApp/Program.cs#CleanupAccount)]   

## <a name="watch-the-event"></a>De gebeurtenis bekijken

Als u de gebeurtenis wilt bekijken, kopieert u de streaming-URL die u hebt verkregen toen u de code uitvoerde, beschreven in Een streaming-locator maken, en gebruikt u een speler van uw keuze. U kunt [Azure Media Player](https://amp.azure.net/libs/amp/latest/docs/index.html) gebruiken om uw stream te testen op https://ampdemo.azureedge.net. 

De livegebeurtenis wordt automatisch geconverteerd naar inhoud op aanvraag wanneer deze wordt gestopt. Zelfs na het stoppen en verwijderen van de gebeurtenis kunnen gebruikers de gearchiveerde inhoud als video op aanvraag streamen, mits u de asset niet hebt verwijderd. Een asset kan niet worden verwijderd als deze wordt gebruikt door een gebeurtenis. U moet eerst de gebeurtenis verwijderen. 

## <a name="clean-up-resources"></a>Resources opschonen

Als u de resources van de resourcegroep niet meer nodig hebt, met inbegrip van de Media Services en opslagaccounts die u hebt gemaakt voor deze zelfstudie, verwijdert u de resourcegroep die u eerder hebt gemaakt.

Voer de volgende CLI-opdracht uit:

```azurecli-interactive
az group delete --name amsResourceGroup
```

> [!IMPORTANT]
> Zolang de livegebeurtenis loopt, worden er kosten in rekening gebracht. Als het project/programma vastloopt of om welke reden dan ook wordt afgesloten, is het mogelijk dat de livegebeurtenis in een factureringsstaat staat.

## <a name="ask-questions-give-feedback-get-updates"></a>Stel vragen, feedback geven, updates ophalen

Bekijk de [Azure Media Services-community](media-services-community.md) artikel om te zien van verschillende manieren kunt u vragen stellen, feedback te geven en updates over Media Services ophalen.

## <a name="next-steps"></a>Volgende stappen

[Bestanden streamen](stream-files-tutorial-with-api.md)
 
