---
title: Azure Media Services-termen en begrippen - Azure | Microsoft Docs
description: In dit onderwerp biedt een kort overzicht van Azure Media Services-termen en begrippen en koppelingen voor meer informatie.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/13/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 1e76569c7f5157dce681d15ec8d499b90e080102
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65762310"
---
# <a name="media-services-concepts"></a>Media Services-concepten

Dit onderwerp bevat een kort overzicht van Azure Media Services-termen en begrippen. Dit artikel bevat ook koppelingen naar artikelen met gedetailleerde uitleg van Media Services v3-concepten en functies. 

Bekijk de basisconcepten die worden beschreven in de volgende onderwerpen voordat u start met de ontwikkeling.

> [!NOTE]
> U kunt momenteel geen gebruik maken van de Azure-portal om v3-resources te beheren. Gebruik de [REST API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref) of een van de ondersteunde [SDK's](media-services-apis-overview.md#sdks).

## <a name="terminology"></a>Terminologie

Deze sectie wordt beschreven hoe sommige algemene termen in de branche toegewezen aan de API van Media Services v3.

### <a name="live-event"></a>Live-gebeurtenis

Een **Live gebeurtenis** vertegenwoordigt een pijplijn voor het opnemen, transcodering (optioneel) en live streams van metagegevens van video, audio en realtime verpakking.

Voor klanten die migreren van Media Services v2 API's, de **Live gebeurtenis** vervangt de **kanaal** entiteit in v2. Zie voor meer informatie, [migreren van v2 naar v3](migrate-from-v2-to-v3.md).

### <a name="streaming-endpoint-packaging-and-origin"></a>Streaming-eindpunt (verpakking en oorsprong)

Een **Streaming-eindpunt** vertegenwoordigt een dynamische (just-in-time)-verpakking en oorsprong service die uw live en on-demand inhoud rechtstreeks aan een client-afspeeltoepassing, met een van de algemene mediaprotocollen voor streaming (HLS kunt leveren (of streepje). Bovendien de **Streaming-eindpunt** biedt dynamische (just-in-time)-versleuteling voor de bedrijfstak toonaangevende DRM's.

In de branche streaming media, deze service wordt vaak aangeduid als een **Packager** of **oorsprong**.  Andere algemene voorwaarden in de branche voor deze mogelijkheid bevatten JITP (Just-in-time-packager) of JITE (Just-in-time-versleuteling). 
 
## <a name="cloud-upload-and-storage"></a>Uploaden naar en opslaan in de cloud

Als u wilt beheren, coderen, codering, analyseren en streaming van media-inhoud in Azure starten, moet u een Media Services-account maken en uploaden van uw digitale bestanden naar **activa**.

- [Uploaden naar en opslaan in de cloud](storage-account-concept.md)
- [Activa-concept](assets-concept.md)

## <a name="encoding"></a>Encoding

Nadat u uw hoogwaardige digitale media-bestanden naar Assets uploaden, kunt u ze coderen in indelingen die kunnen worden afgespeeld op een groot aantal browsers en apparaten. 

Als u wilt coderen met Media Services v3, moet u maken **transformeert** en **taken**.

![Transformaties](./media/encoding/transforms-jobs.png)

- [Transformaties en taken](transforms-jobs-concept.md)
- [Codering met mediaservices](encoding-concept.md)

## <a name="media-analytics"></a>Media-analyses

Voor het analyseren van uw video en audio-bestanden, moet u ook maken **transformeert** en **taken**.

- [Video-en audiobestanden analyseren](analyzing-video-audio-files-concept.md)

## <a name="packaging-delivery-protection"></a>Verpakking, levering, beveiliging

Zodra uw inhoud wordt gecodeerd, kunt u profiteren van **dynamische verpakking**. In Media Services, een **Streaming-eindpunt**  /oorsprong wordt de dynamische verpakking-service gebruikt voor het leveren van media-inhoud naar client spelers. Video's in de uitvoer als beschikbaar wilt maken voor clients om te worden afgespeeld, u moet maken een **Streaming-Locator gemaakt** en bouw vervolgens de streaming-URL's. 

Bij het maken van de **Streaming-Locator gemaakt**, naast de assetnaam, moet u om op te geven **Streaming beleid**. **Beleid voor streaming** kunt u voor het definiëren van protocollen voor streaming en versleuteling opties (indien aanwezig) voor uw **Streaming-Locators**.

Dynamische pakketten wordt gebruikt of u uw inhoud live of on-demand streamen. Het volgende diagram toont de streaming on demand met dynamische verpakking werkstroom.

![Dynamische verpakking](./media/dynamic-packaging-overview/media-services-dynamic-packaging.png)

Met Media Services van uw live en on-demand inhoud dynamisch wordt versleuteld met Advanced Encryption Standard (AES-128) kan worden geleverd of / en een van de drie belangrijkste digital rights management (DRM)-systemen: Microsoft PlayReady, Google Widevine en FairPlay van Apple. Media Services biedt ook een service voor het leveren van AES-sleutels en DRM (PlayReady, Widevine en FairPlay) licenties voor geautoriseerde clients.

Als versleutelingsopties voor uw stroom op te geven, maakt u de **inhoud sleutel beleid** en koppel deze aan uw **Streaming-Locator gemaakt**. De **inhoud sleutel beleid** kunt u configureren hoe de inhoudssleutel wordt geleverd als u wilt beëindigen van clients.

De volgende afbeelding ziet u de Media Services content protection-werkstroom: 

![Inhoud beveiligen](./media/content-protection/content-protection.svg)

&#42;dynamische versleuteling ondersteunt AES-128 "clear key', CBCS en CENC. 

U kunt Media Services gebruiken **dynamische manifesten** alleen een specifieke weergave of subclips van uw video te streamen. In het volgende voorbeeld is een coderingsprogramma gebruikt een tussentijds asset coderen in zeven ISO MP4s video voorinstelling (van 180p 1080p). De gecodeerde asset kan dynamisch worden verpakt in een van de volgende protocollen voor streaming: HLS, MPEG DASH en Smooth.  Aan de bovenkant van het diagram, het HLS-manifest voor de asset met geen filters wordt weergegeven (bevat alle zeven voorinstelling).  In de linksonder, wordt het HLS-manifest waarop een filter met de naam "ott" is toegepast weergegeven. Het filter "ott" Hiermee geeft u als u wilt verwijderen van alle bitsnelheden hieronder 1 Mbps, wat leidde tot de twee quality-niveaus van onder, worden verwijderd uit in het antwoord. In de rechts onderaan wordt het HLS-manifest waarop een filter met de naam 'mobiel' is toegepast weergegeven. Het filter 'mobiel' Hiermee geeft u het verwijderen van vertoningen waar de oplossing is groter dan 720p, wat leidde tot de twee 1080p voorinstelling wordt overblijft.

![Weergavefiltering](./media/filters-dynamic-manifest-overview/media-services-rendition-filter.png)

- [Dynamische pakketten](dynamic-packaging-overview.md)
- [Streaming-eindpunten](streaming-endpoint-concept.md)
- [Streaming-locators](streaming-locators-concept.md)
- [Beleid voor streaming](streaming-policy-concept.md)
- [Beleid voor inhoudssleutels](content-key-policy-concept.md)
- [Beveiliging van inhoud](content-protection-overview.md)
- [Dynamische manifesten](filters-dynamic-manifest-overview.md)
- [Filters](filters-concept.md)

## <a name="live-streaming"></a>Live streamen

Azure Media Services kunt u live-evenementen Bied uw klanten op de Azure-cloud. **Livegebeurtenissen** zijn verantwoordelijk voor het opnemen en verwerken van de live videofeeds. Bij het maken van een **Live gebeurtenis**, een invoereindpunt waarmee u kunt een live signaal verzenden vanaf een externe coderingsprogramma wordt gemaakt. Zodra u de stroom doorgestuurd hebt naar de **Live gebeurtenis**, kunt u de streaminggebeurtenis starten door het maken van een **Asset**, **uitvoer Live**, en **Streaming-Locator gemaakt** . **Live uitvoer** worden gearchiveerd de stroom in de **Asset** en het beschikbaar maken voor gebruikers via de **Streaming-eindpunt**. Een **Live gebeurtenis** kan bestaan uit een van de twee typen: **pass-through** en **van realtime codering**.

De volgende afbeelding illustreert de werkstroom van Pass Through-type:

![passthrough](./media/live-streaming/pass-through.svg)

- [Overzicht van live streaming](live-streaming-overview.md)
- [Livegebeurtenissen en live-uitvoer](live-events-outputs-concept.md)

## <a name="monitoring"></a>Bewaking

### <a name="event-grid"></a>Event Grid

Als u wilt zien van de voortgang van de taak, moet u **Event Grid**. Media Services verzendt ook de Live-gebeurtenis-typen. Met Event Grid kunnen uw apps luisteren naar en reageren op gebeurtenissen uit vrijwel alle Azure-services, evenals aangepaste bronnen. 

- [Event Grid-gebeurtenissen verwerken](reacting-to-media-services-events.md)
- [Schema 's](media-services-event-schemas.md)

### <a name="azure-monitor"></a>Azure Monitor

Monitor voor metrische gegevens en logboeken met diagnostische gegevens die u helpen begrijpen hoe uw toepassingen worden uitgevoerd met Azure Monitor.

- [Diagnostische logboeken en metrische gegevens](media-services-metrics-diagnostic-logs.md)
- [Schema's voor diagnostische logboeken](media-services-diagnostic-logs-schema.md)

## <a name="player-clients"></a>Afspeel-clients

U kunt Azure Media Player gebruiken voor het afspelen van media-inhoud gestreamd door Media Services op een groot aantal browsers en apparaten. Azure Media Player maakt gebruik van industriële standaarden, zoals HTML5, Media bron-extensies (MSE) en Encrypted Media Extensions (EME) voor een geavanceerde adaptieve streamingervaring. 

- [Overzicht van Azure Media Player](use-azure-media-player.md)

## <a name="ask-questions-give-feedback-get-updates"></a>Stel vragen, feedback geven, updates ophalen

Bekijk de [Azure Media Services-community](media-services-community.md) artikel om te zien van verschillende manieren kunt u vragen stellen, feedback te geven en updates over Media Services ophalen.

## <a name="next-steps"></a>Volgende stappen

* [Codeer externe bestands- en stream-video-REST](stream-files-tutorial-with-rest.md)
* [Codeer het geüploade bestand en stream-video - .NET](stream-files-tutorial-with-api.md)
* [Stream live - .NET](stream-live-tutorial-with-api.md)
* [Analyseren van uw video - .NET](analyze-videos-tutorial-with-api.md)
* [AES-128 dynamische versleuteling - .NET](protect-with-aes128.md)
* [Dynamisch versleutelen met multi-DRM - .NET](protect-with-drm.md) 
