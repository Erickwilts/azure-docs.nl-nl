---
title: Hiermee worden dynamische manifesten gefilterd en Azure Media Services | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe u filters maken, zodat de client voor het specifieke secties van de stroom van een stroom gebruiken kunt. Media Services wordt gemaakt dynamische manifesten voor het bereiken van deze selectief streaming.
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
ms.date: 07/11/2019
ms.author: juliako
ms.openlocfilehash: bbbb570cc042d5faa16b66c42aef9792b24fdb12
ms.sourcegitcommit: 470041c681719df2d4ee9b81c9be6104befffcea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67854057"
---
# <a name="pre-filtering-manifests-by-using-dynamic-packager"></a>Manifesten vooraf filteren met behulp van dynamische pakket

Wanneer u Adaptive Bitrate Streaming inhoud levert aan apparaten, moet u vaak meerdere versies van een manifest publiceren om specifieke apparaatfuncties of beschik bare netwerk bandbreedte te bereiken. Met de [Dynamic packager](dynamic-packaging-overview.md) kunt u filters opgeven waarmee u specifieke codecs, resoluties, bitsnelheden en combi Naties van audio tracks kunt filteren om meerdere kopieën te maken. U hoeft alleen maar een nieuwe URL te publiceren met een specifieke set filters die zijn geconfigureerd voor uw doel apparaten (iOS, Android, SmartTV of browsers) en de netwerk mogelijkheden (scenario's met een hoge band breedte, mobiele of lage band breedte). In dit geval kunnen clients de streaming van uw inhoud manipuleren via de query reeks (door beschik bare [Asset filters of account filters](filters-concept.md)op te geven) en filters gebruiken om specifieke secties van een stroom te streamen.

Bij sommige bezorgings scenario's moet u ervoor zorgen dat een klant geen toegang kan krijgen tot specifieke sporen. Het is bijvoorbeeld mogelijk dat u een manifest dat HD-tracks bevat niet wilt publiceren naar een specifieke Subscriber-laag. Het is ook mogelijk dat u specifieke ABR-tracks (Adaptive bitrate) wilt verwijderen om de leverings kosten te verlagen voor een specifiek apparaat dat niet voor de extra sporen in aanmerking komt. In dit geval kunt u een lijst met vooraf gemaakte filters koppelen aan uw [streaming-Locator](streaming-locators-concept.md) bij het maken. In dit geval kunnen clients niet bewerken hoe de inhoud wordt gestreamd. deze wordt gedefinieerd door de **streaming-Locator**.

U kunt filteren combi neren door [filters op te geven voor streaming-Locator](filters-concept.md#associating-filters-with-streaming-locator) + extra apparaatspecifieke filters die uw client opgeeft in de URL. Dit kan handig zijn om extra sporen te beperken, zoals meta gegevens of gebeurtenis stromen, audio talen of beschrijvende audio sporen. 

Deze mogelijkheid om verschillende filters op te geven voor uw stroom, biedt een krachtige oplossing voor **dynamische manifest** manipulatie voor het richten op meerdere gebruiks scenario's voor uw doel apparaten. In dit onderwerp worden de concepten van **dynamische manifesten** beschreven en vindt u voor beelden van scenario's waarin u deze functie kunt gebruiken.

> [!NOTE]
> Dynamische manifesten wijzigen de Asset en het standaard manifest voor die Asset niet. 
> 

##  <a name="overview-of-manifests"></a>Overzicht van manifesten

Azure Media Services ondersteunt HLS-, MPEG-DASH-en Smooth Streaming-protocollen. Als onderdeel van [dynamische pakketten](dynamic-packaging-overview.md)worden de streaming-client manifesten (HLS Master-afspeel lijst, Dash-media Presentation Description [MPD] en Smooth streaming) dynamisch gegenereerd op basis van de indelings kiezer in de URL. Zie de bezorgings protocollen in de [algemene werk stroom op aanvraag](dynamic-packaging-overview.md#delivery-protocols). 

### <a name="get-and-examine-manifest-files"></a>Manifest bestanden ophalen en onderzoeken

U geeft een lijst op met de eigenschaps voorwaarden voor het filteren van eigenschappen op basis van welke sporen van uw stroom (Live of video op aanvraag [VOD]) moeten worden opgenomen in een dynamisch gemaakt manifest. Als u de eigenschappen van de tracks wilt ophalen en bekijken, moet u eerst het Smooth Streaming-manifest laden.

In de zelf studie [bestanden uploaden, coderen en streamen met .net](stream-files-tutorial-with-api.md#get-streaming-urls) wordt uitgelegd hoe u de streaming-url's met .net kunt bouwen. Als u de app uitvoert, wijst een van de Url's naar het Smooth Streaming-manifest `https://amsaccount-usw22.streaming.media.azure.net/00000000-0000-0000-0000-0000000000000/ignite.ism/manifest`:.<br/> Kopieer en plak de URL in de adres balk van een browser. Het bestand wordt gedownload. U kunt deze openen in een tekst editor van uw keuze.

Zie voor een REST-voor beeld [bestanden uploaden, coderen en streamen met rest](stream-files-tutorial-with-rest.md#list-paths-and-build-streaming-urls).

### <a name="monitor-the-bitrate-of-a-video-stream"></a>De bitrate van een video stroom bewaken

U kunt de [Azure Media Player-demo pagina](https://aka.ms/azuremediaplayer) gebruiken om de bitsnelheid van een video stroom te controleren. Op de pagina demo worden diagnostische gegevens weer gegeven op het tabblad **diagnostiek** :

![Diagnostische gegevens Azure Media Player][amp_diagnostics]
 
### <a name="examples-urls-with-filters-in-query-string"></a>Voorbeelden: Url's met filters in de query reeks

U kunt filters toep assen op protocollen voor het streamen van ABR: HLS, MPEG-DASH en Smooth Streaming. In de volgende tabel ziet u enkele voor beelden van Url's met filters:

|Protocol|Voorbeeld|
|---|---|
|HLS|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|MPEG DASH|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Smooth Streaming|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(filter=myAssetFilter)`|

## <a name="rendition-filtering"></a>Vertoning filteren

U kunt ervoor kiezen om uw asset te coderen naar meerdere coderings profielen (H. 264 baseline, H. 264 High, AACL, AACH, Dolby Digital Plus) en meerdere kwaliteits bitrates. Niet alle client apparaten ondersteunen echter wel de profielen en bitrates van uw asset. Oudere Android-apparaten ondersteunen bijvoorbeeld alleen H. 264 Baseline + AACL. Het verzenden van hogere bitrates naar een apparaat dat de voor delen niet kan verkrijgen, verwerkt de band breedte en de berekening van het apparaat. Een dergelijk apparaat moet alle opgegeven informatie decoderen, zodat het alleen kan worden geschaald voor weer gave.

Met het dynamische manifest kunt u apparaatprofielen maken (zoals Mobile, console of HD/SD) en de sporen en kwaliteiten opnemen die u van elk profiel wilt maken. Dit heet weergave filters. In het volgende diagram ziet u een voor beeld hiervan.

![Voor beeld van weergave filters][renditions2]

In het volgende voor beeld werd een encoder gebruikt voor het coderen van een mezzanine-asset in zeven ISO Mp4's-video weergaven (van 180p naar 1080p). De versleutelde Asset kan [dynamisch worden verpakt](dynamic-packaging-overview.md) in een van de volgende streaming-protocollen: HLS, MPEG DASH en Smooth. 

Boven in het volgende diagram ziet u het HLS-manifest voor de Asset zonder filters. (Deze bevat alle zeven weer gaven.)  In de linkerbenedenhoek ziet u een HLS-manifest waarop een filter met de naam ' Ott ' is toegepast. Met het filter "Ott" wordt het verwijderen van alle bitsnelheden onder 1 Mbps opgegeven, dus de laagste twee kwaliteits niveaus zijn verwijderd uit het antwoord. In de rechter benedenhoek toont het diagram het HLS-manifest waarop een filter met de naam ' Mobile ' is toegepast. Met het filter ' Mobiel ' geeft u het verwijderen van vertoningen aan waarvan de resolutie groter is dan 720p, waardoor de twee 1080p-vertoningen zijn verwijderd.

![Vertoning filteren][renditions1]

## <a name="removing-language-tracks"></a>Taal sporen verwijderen
Uw assets kunnen meerdere audio talen bevatten, zoals Engels, Spaans, Frans, enzovoort. Normaal gesp roken worden met de Player-beheer Programma's standaard audio bijgehouden selectie en beschik bare audio sporen per gebruiker geselecteerd.

Het ontwikkelen van dergelijke Player Sdk's is lastig, omdat er verschillende implementaties nodig zijn voor apparaatspecifieke Windows-frameworks. Op sommige platforms zijn de Api's van de speler beperkt en bevatten ze geen functie voor audio selectie, waarbij gebruikers de standaard audio track niet kunnen selecteren of wijzigen. Met Asset filters kunt u het gedrag beheren door filters te maken die alleen gewenste audio talen bevatten.

![Filteren van taal sporen][language_filter]

## <a name="trimming-the-start-of-an-asset"></a>Het begin van een Asset bijsnijden

In de meest live streaming-gebeurtenissen voeren Opera tors enkele tests uit vóór de werkelijke gebeurtenis. Ze kunnen bijvoorbeeld een pastel bevatten zoals deze voor het begin van de gebeurtenis: ' Het programma wordt tijdelijk gestart. ' 

Als het programma wordt gearchiveerd, worden de test-en pastel gegevens ook gearchiveerd en opgenomen in de presentatie. Deze informatie mag echter niet worden weer gegeven aan de clients. Met het dynamische manifest kunt u een begin tijd filter maken en de ongewenste gegevens verwijderen uit het manifest.

![Bijsnijden starten][trim_filter]

## <a name="creating-subclips-views-from-a-live-archive"></a>Subfragmenten (weer gaven) maken op basis van een Live-Archief

Veel live-gebeurtenissen zijn langdurig actief en live archief bevat mogelijk meerdere gebeurtenissen. Nadat de live-gebeurtenis is beëindigd, willen broadcasters mogelijk het Live Archief opsplitsen in de start-en stop sequenties van het logische programma. 

U kunt deze virtuele Program ma's afzonderlijk publiceren zonder het live archief te verwerken en geen afzonderlijke assets te maken (dit biedt geen voor deel van de bestaande fragmenten in de cache in Cdn's). Voor beelden van dergelijke virtuele Program ma's zijn de kwart alen van een voetbal-of basketbal spel, innings in honkbal of afzonderlijke evenementen van elk sport programma.

Met het dynamische manifest kunt u filters maken met behulp van begin-en eind tijden en virtuele weer gaven maken boven aan uw live-archief. 

![Filter subfragment][subclip_filter]

Hier ziet u de gefilterde activa:

![Ski][skiing]

## <a name="adjusting-the-presentation-window-dvr"></a>Het presentatie venster (DVR) aanpassen

Azure Media Services biedt momenteel een circulair archief waarbij de duur tussen 5 minuten en 25 uur kan worden geconfigureerd. Het filteren van manifesten kan worden gebruikt voor het maken van een voortschrijdend DVR-venster boven aan het archief, zonder dat er media worden verwijderd. Er zijn veel scenario's waarbij broadcasters een beperkt DVR-venster willen aanbieden om met de Live Edge te verplaatsen en tegelijkertijd een groter archiverings venster te gebruiken. Een omroep gebruiker kan de gegevens uit het DVR-venster gebruiken om clips te markeren, of ze willen mogelijk verschillende DVR-Vensters voor verschillende apparaten opgeven. De meeste mobiele apparaten verwerken bijvoorbeeld geen grote DVR-Windows (u kunt een DVR-venster van 2 minuten hebben voor mobiele apparaten en één uur voor desktop-clients).

![DVR-venster][dvr_filter]

## <a name="adjusting-livebackoff-live-position"></a>LiveBackoff aanpassen (Live positie)

Manifest filtering kan worden gebruikt om enkele seconden van de Live rand van een actief programma te verwijderen. Door te filteren kunt u uitzenders de presentatie bekijken op het publicatie punt van de preview-versie en aankondigings punten maken voordat de kijkers de stroom ontvangen (een back-up van 30 seconden). Broadcasters kunnen vervolgens deze advertenties naar hun client raamwerks pushen, zodat ze de informatie ontvangen en verwerken vóór de aankondigings mogelijkheid.

Naast de reclame-ondersteuning kan de Live-afmeldings instelling worden gebruikt om de positie van de kijkers aan te passen, zodat de gebruiker nog steeds fragmenten kan ophalen van de server. Op die manier krijgen clients geen HTTP 404-of 412-fout.

![Filteren op Live-uitgaand][livebackoff_filter]

## <a name="combining-multiple-rules-in-a-single-filter"></a>Meerdere regels in één filter combi neren

U kunt meerdere filter regels combi neren in één filter. U kunt bijvoorbeeld een ' bereik regel ' definiëren om pastels uit een live archief te verwijderen en ook beschik bare bitrates te filteren. Wanneer u meerdere filter regels toepast, is het eind resultaat het snij punt van alle regels.

![Meerdere filter regels][multiple-rules]

## <a name="combining-multiple-filters-filter-composition"></a>Meerdere filters combi neren (filter samenstelling)

U kunt ook meerdere filters combi neren in één URL. In het volgende scenario ziet u waarom u mogelijk filters wilt combi neren:

1. U moet uw video kwaliteiten filteren voor mobiele apparaten, zoals Android of iPad (om de video kwaliteiten te beperken). Als u de ongewenste kwaliteiten wilt verwijderen, maakt u een account filter dat geschikt is voor de apparaatprofielen. U kunt account filters gebruiken voor al uw assets onder hetzelfde Media Services-account zonder verdere koppeling.
1. U wilt ook de begin-en eind tijd van een activum bijsnijden. Hiervoor maakt u een activa filter en stelt u de begin-en eind tijd in. 
1. U wilt beide filters combi neren. Zonder combi natie moet u kwaliteits filters aan het filter Bijsnijden toevoegen, waardoor het filter gebruik moeilijker te maken is.


Als u filters wilt combi neren, moet u de filter namen instellen op de manifest/afspeel lijst-URL in een door punt komma's gescheiden indeling. Stel dat u een filter hebt met de naam *MyMobileDevice* dat de kwaliteiten filtert en dat u een andere benoemde *MyStartTime* hebt om een specifieke begin tijd in te stellen. U kunt Maxi maal drie filters combi neren. 

Zie [dit blog bericht](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/)voor meer informatie.

## <a name="considerations-and-limitations"></a>Overwegingen en beperkingen

- De waarden voor **forceEndTimestamp**, **presentationWindowDuration**en **liveBackoffDuration** moeten niet worden ingesteld voor een VOD-filter. Ze worden alleen gebruikt voor Live-filter scenario's. 
-  Een dynamisch manifest werkt in GOP terug-grenzen (keyframes), dus het verkleinen van GOP terug nauw keurigheid.
-  U kunt dezelfde filter naam gebruiken voor account-en Asset-filters. Asset filters hebben een hogere prioriteit en vervangen account filters.
- Als u een filter bijwerkt, kan het tot twee minuten duren voordat het streaming-eind punt de regels vernieuwt. Als u filters hebt gebruikt om de inhoud te leveren (en u hebt de inhoud in de caches van proxy's en CDN-caches opgeslagen), kunnen deze filters worden bijgewerkt als gevolg van fouten in de speler. U wordt aangeraden de cache te wissen na het bijwerken van het filter. Als deze optie niet mogelijk is, kunt u overwegen een ander filter te gebruiken.
- Klanten moeten het manifest hand matig downloaden en het exacte tijds tempel en de tijd schaal van de start parseren.
    
    - [Het manifest bestand ophalen en controleren om de](#get-and-examine-manifest-files)eigenschappen van de sporen in een Asset te bepalen.
    - De formule voor het instellen van de tijds tempel eigenschappen van de Asset-filter is: <br/>startTimestamp = &lt;begin tijd in het manifest&gt; +  &lt;verwachte filter start tijd in seconden&gt; * tijd schaal

## <a name="next-steps"></a>Volgende stappen

In de volgende artikelen ziet u hoe u via een programma filters maakt:  

- [Filters maken met REST-Api's](filters-dynamic-manifest-rest-howto.md)
- [Filters maken met .NET](filters-dynamic-manifest-dotnet-howto.md)
- [Filters maken met CLI](filters-dynamic-manifest-cli-howto.md)

[renditions1]: ./media/filters-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/filters-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/filters-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/filters-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/filters-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/filters-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/filters-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/filters-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/filters-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/filters-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/filters-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/filters-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/filters-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/filters-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/filters-dynamic-manifest-overview/media-services-skiing.png
[amp_diagnostics]: ./media/filters-dynamic-manifest-overview/amp_diagnostics.png
