---
title: Wat is Video Indexer?
titlesuffix: Azure Media Services
description: Dit onderwerp geeft een overzicht van de service Video Indexer.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 2c72c7c493c0a887adab147054c725a2e1c0659f
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65799127"
---
# <a name="what-is-video-indexer"></a>Wat is Video Indexer?

Azure Video Indexer is een cloudtoepassing die is gebouwd met behulp van Azure Media Analytics, Azure Search, Cognitive Services (zoals de Face-API, Microsoft Translator, de Computer Vision-API en Custom Speech Service). Hiermee kunt u de inzichten ophalen uit uw video's met Video Indexer-video en audio modellen die hieronder worden beschreven:
  
## <a name="video-insights"></a>Inzichten in video 's

- **Gezichtsdetectie**: Detecteert en groepen gezichten worden weergegeven in de video.
- **Beroemdheden identificatie**: Video Indexer identificeert automatisch meer dan 1 miljoen beroemdheden – zoals world-leiders, actors, actresses, atleten, onderzoekers, zakelijke en technische leiders overal ter wereld. De gegevens van deze beroemdheden zijn ook beschikbaar op verschillende beroemde websites, zoals IMDB en Wikipedia.
- **Op basis van een account gezichts-id**: Video Indexer traint een model voor een specifiek account. Gezichten in de video die op basis van het getrainde model vervolgens herkent. Zie voor meer informatie, [aanpassen van een persoon-model van de website Video Indexer](customize-person-model-with-website.md) en [aanpassen van een persoon-model met de Video Indexer-API](customize-person-model-with-api.md).
- **Miniaturen extractie voor gezichten** ('beste face"): Automatisch de beste vastgelegde gezicht in elke groep van gezichten wordt uitgevoerd (gebaseerd op de kwaliteit, de grootte en positie van de voorzijde) identificeert en pak het uit als een afbeelding asset.
- **Visual tekstherkenning** (OCR): Retourneert de tekst die visueel wordt weergegeven in de video.
- **Visual inhoudstoezicht**: Detecteert volwassen en/of ongepaste visuele elementen.
- **Identificatie van labels**: Hiermee geeft u visuele objecten en acties die worden weergegeven.
- **Scène segmentering**: Hiermee bepaalt u wanneer een scène wijzigingen in de video op basis van visuele aanwijzingen. Een scène ziet u één gebeurtenis en het bestaat uit een reeks opeenvolgende slagen, die semantisch zijn gerelateerd. 
- **Detectie van agenda**: Hiermee bepaalt u wanneer een opname in de video op basis van visuele aanwijzingen verandert. Een schermopname is een reeks frames die zijn overgenomen uit de dezelfde filmstudio camera. Zie voor meer informatie, [scènes, foto's en hoofdframes](scenes-shots-keyframes.md).
- **Detectie van een zwarte**: Hiermee geeft u zwarte frames die zijn gepresenteerd in de video.
- **Sleutelframes extractie**: Detecteert stabiel hoofdframes in een video.
- **Rolling tegoed**: het begin en einde van de rolling-credits in het einde van tv-programma's en films identificeren.

## <a name="audio-insights"></a>Audio insights

- **Automatische taaldetectie**: Identificeert automatisch de dominante gesproken taal. Ondersteunde talen zijn Engels, Spaans, Frans, Duits, Italiaans, Chinees (Vereenvoudigd), Japans, Russisch en Braziliaans Portugees wordt fallback naar Engels als de taal kan niet worden gedetecteerd.
- **Audiotranscriptie**: Converteert van spraak naar tekst in 12 talen waardoor nu extensies. Ondersteunde talen zijn Engels, Spaans, Frans, Duits, Italiaans, Chinees (Vereenvoudigd), Japans, Arabisch, Russisch, Portugees (Brazilië), Hindi en Koreaans.
- **Ondertiteling**: Hiermee maakt u ondertiteling in drie indelingen hebben: VTT, TTML, SRT.
- **Twee verwerking channel**: Automatisch detecteert, afzonderlijke transcript- en samengevoegd in één tijdlijn.
- **Reductie van ruis**: Hiermee schakelt u van de telephony-audio- of ruis opnamen (gebaseerd op Skype filters).
- **Transcript customization** (CRIS): Aangepaste spraak-naar-tekst-modellen te maken van branchespecifieke Transcripten traint. Zie voor meer informatie, [een taalmodel van de Video Indexer-website aanpassen](customize-language-model-with-website.md) en [een taalmodel met Video Indexer-API's aanpassen](customize-language-model-with-api.md).
- **Sprekerherkenning-opsomming**: Kaarten en begrijpt welke spreker spokenetwerktopologie welke woorden en wanneer.
- **Statistieken van de spreker**: Voorziet in statistieken voor sprekers spraak ratio's.
- **Tekstuele inhoudstoezicht**: Expliciete tekst in de audiotranscript gedetecteerd.
- **Audio-effecten**: Hiermee geeft u audio-effecten, zoals hand hiep, spraak en stilte.
- **Detectie van emoties in**: Identificeert willekeurige emoties op basis van spraak (wat wordt genoemd) en stem tonen (hoe deze wordt genoemd).  De emotie kan vreugde, verdriet, boosheid of angst zijn.
- **Vertaling**: Hiermee maakt u vertalingen van de audiotranscript aan 54 verschillende talen.

## <a name="audio-and-video-insights-multi-channels"></a>Audio en video insights (meerdere kanalen)

Tijdens het indexeren van een kanaal gedeeltelijke is resultaten voor deze modellen beschikbaar

- **Trefwoorden extraheren**: Extraheert trefwoorden uit de visual tekst en spraak.
- **Extractie merken**: Extraheert merken uit visual tekst en spraak.
- **Onderwerp Deductie**: Deductie van de belangrijkste onderwerpen uit Transcripten maakt. Het niveau van de 1e IPTC taxonomie is opgenomen.
- **Artefacten**: Extraheert de grote verscheidenheid aan 'volgende niveau van details' artefacten voor elk van de modellen.
- **Sentimentanalyse**: Hiermee geeft u positieve, negatieve en neutrale sentimenten uit visual tekst en spraak.
 
Wanneer Video Indexer klaar is met de verwerking en analyse, kunt u de video-inzichten bekijken, cureren, doorzoeken en publiceren.

De Video Indexer-service is geschikt voor een groot aantal toepassingen, ongeacht of u inhoudsbeheerder of ontwikkelaar bent. Inhoudsbeheerders kunnen de webportal van Video Indexer gebruiken om de voordelen van de service te benutten zonder één regel code te schrijven. Zie [deze zelfstudie over Video Indexer](video-indexer-get-started.md) om aan de slag te gaan met de website. Ontwikkelaars kunnen API's gebruiken om inhoud op schaal te verwerken. Zie [REST-API van Video Indexer gebruiken](video-indexer-use-apis.md) voor meer informatie. De service stelt klanten ook in staat om met behulp van widgets videostreams en geëxtraheerde inzichten te publiceren in hun eigen toepassingen. Zie [Video Indexer-widgets insluiten in uw toepassingen](video-indexer-embed-widgets.md) voor meer informatie.

U kunt zich registreren voor de service met behulp van een bestaand account van AAD, LinkedIn, Facebook, Google of MSA. Zie [Aan de slag](video-indexer-get-started.md) voor meer informatie.

## <a name="scenarios"></a>Scenario's

Hieronder vindt u enkele scenario's waarin Video Indexer van pas kan komen:

- Zoeken: inzichten die zijn geëxtraheerd uit de video kunnen worden gebruikt voor het verbeteren van de zoekervaring in een videobibliotheek. Zo is het bijvoorbeeld mogelijk om door het indexeren van gesproken woorden en gezichten momenten in een video te vinden waarop een bepaalde persoon bepaalde woorden heeft uitgesproken of waarop twee personen samen zijn te zien. Zoekopdrachten op basis van dergelijke inzichten uit video's zijn handig voor persbureaus, onderwijsinstellingen, omroepen, eigenaren van inhoud voor de entertainmentindustrie, LOB-apps voor ondernemingen, kortom voor elke bedrijfstak waarin videobibliotheken worden onderhouden die door gebruikers worden doorzocht.
- Inhoud maken – inzichten uit video's geëxtraheerde en effectief maken van inhoud, zoals aanhangwagen, sociale media-inhoud, nieuws clips enzovoort van de bestaande inhoud in het archief van de organisatie 
- Inkomsten genereren: Video Indexer kan helpen om de waarde van video's te verbeteren. Branches die bijvoorbeeld afhankelijk zijn van advertentie-inkomsten (via nieuwsmedia, social media, enzovoort), kunnen meer relevante advertenties aanbieden door de geëxtraheerde inzichten te gebruiken als aanvullende signalen voor de advertentieserver (het is relevanter om een advertentie voor sportschoenen weer te geven tijdens een voetbalwedstrijd dan tijdens een zwemwedstrijd).
- Betrokkenheid van gebruikers: video-inzichten kunnen worden gebruikt om de betrokkenheid van gebruikers te verbeteren door het positioneren van de relevante videomomenten aan gebruikers. Laten we als voorbeeld een educatieve video nemen waarin de eerste 30 minuten gaan over bollen en de 30 minuten daarna over piramiden. Een leerling die informatie zoekt over piramiden, zal het dan prettig vinden als de video start vanaf de markering voor 30 minuten en niet vanaf het begin.

## <a name="next-steps"></a>Volgende stappen

U kunt aan de slag met Video Indexer. Raadpleeg voor meer informatie de volgende artikelen:

- [Aan de slag met de Video Indexer-website](video-indexer-get-started.md)
- [REST-API van Video Indexer gebruiken](video-indexer-use-apis.md)
- [Video Indexer-widgets insluiten in uw toepassingen](video-indexer-embed-widgets.md)
