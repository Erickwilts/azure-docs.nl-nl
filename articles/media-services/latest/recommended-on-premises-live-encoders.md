---
title: Live streaming encoders die worden aanbevolen door Media Services-Azure | Microsoft Docs
description: Meer informatie over on-premises coderings Programma's voor live streamen aanbevolen door Media Services
services: media-services
keywords: code ring; encoders; media
author: johndeu
manager: johndeu
ms.author: johndeu
ms.date: 08/16/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 1f152f7f94c12beecf015d9389ed85cb310573bd
ms.sourcegitcommit: a8b638322d494739f7463db4f0ea465496c689c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2019
ms.locfileid: "68297730"
---
# <a name="recommended-live-streaming-encoders"></a>Aanbevolen code ring voor live streamen

In Azure Media Services vertegenwoordigt een [live gebeurtenis](https://docs.microsoft.com/rest/api/media/liveevents) (kanaal) een pijp lijn voor het verwerken van live-streaming-inhoud. De live-gebeurtenis ontvangt Live invoer stromen op een van de volgende twee manieren.

* Een on-premises Live Encoder verzendt een gegevensgestuurde multi-bitrate RTMP-of Smooth Streaming (gefragmenteerde MP4)-stream naar de live gebeurtenis die niet is ingeschakeld voor het uitvoeren van Live code ring met Media Services. De opgenomen streams passeren Live gebeurtenissen zonder verdere verwerking. Deze methode wordt **Pass-Through**genoemd. Een live coderings programma kan een single-bitrate stream verzenden naar een Pass-Through-kanaal. Deze configuratie wordt niet aanbevolen omdat de client niet is toegestaan voor Adaptive Bitrate Streaming.

  > [!NOTE]
  > Het gebruik van een Pass-Through-methode is de voordeligste manier om live streamen uit te voeren.
 
* Een on-premises Live Encoder verzendt een stream met één bitsnelheid naar de live gebeurtenis die is ingeschakeld voor het uitvoeren van Live code ring met Media Services in een van de volgende indelingen: RTMP of Smooth Streaming (gefragmenteerde MP4). De live-gebeurtenis voert vervolgens Live encoding uit van de inkomende single-bitrate stream naar een multi-bitrate-video stroom (adaptief).

Zie voor gedetailleerde informatie over Live encoding met Media Services [live streamen met Media Services v3](live-streaming-overview.md).

## <a name="live-encoders-that-output-rtmp"></a>Live coderings Programma's die RTMP uitvoeren

Media Services raadt het gebruik aan van een van de volgende live-encoders met RTMP als uitvoer. De ondersteunde URL-schema's `rtmp://` zijn `rtmps://`of.

> [!NOTE]
> Bij het streamen via RTMP controleert u de instellingen voor de firewall en/of proxy om te zien of de uitgaande TCP-poorten 1935 en 1936 open zijn.

- Adobe Flash Media Live Encoder 3.2
- Haivision KB
- Haivision Makito X HEVC
- OBS Studio
- Switcher Studio (iOS)
- Telestream Wirecast 8.1+
- Telestream Wirecast S
- Teradek Slice 756
- TriCaster 8000
- Tricaster Mini HD-4
- VMIX
- xStream

## <a name="live-encoders-that-output-fragmented-mp4"></a>Live coderings Programma's die gefragmenteerde MP4 uitvoeren

Media Services raadt u aan om een van de volgende Live coderings Programma's te gebruiken met multi-bitrate Smooth Streaming (gefragmenteerde MP4) als uitvoer. De ondersteunde URL-schema's `http://` zijn `https://`of.

- Ateme TITAN Live
- Cisco Digital Media Encoder 2200
- Elemental Live
- Envivio 4Caster C4 Gen III
- Denk aan de communicatie selenio MCP3
- Media Excel Hero Live en Hero 4K (UHD/HEVC)

> [!TIP]
>  Als u live-gebeurtenissen in meerdere talen wilt streamen (bijvoorbeeld een Engels audio nummer en één Spaans audio spoor), kunt u dit doen met de media Excel live encoder die is geconfigureerd voor het verzenden van live feeds naar een Pass-through live-gebeurtenis.

## <a name="configuring-on-premises-live-encoder-settings"></a>Instellingen voor on-premises Live coderings Programma's configureren

Zie voor meer informatie over welke instellingen geldig zijn voor uw live-gebeurtenis type [vergelijking van live gebeurtenis typen](live-event-types-comparison.md).

### <a name="playback-requirements"></a>Vereisten voor afspelen

Als u de inhoud wilt afspelen, moet zowel een audio-als een video stroom aanwezig zijn. Afspelen van de stream alleen video wordt niet ondersteund.

### <a name="configuration-tips"></a>Configuratie tips

- Gebruik indien mogelijk een internetverbinding ' hardwired '.
- Wanneer u de vereisten voor de band breedte bepaalt, verdubbelt u de streaming bitrates. Hoewel het niet verplicht is, kunt u met deze eenvoudige regel de impact van de netwerk congestie verminderen.
- Wanneer u coderingsprogramma's op basis van software, sluit u alle onnodige programma's.
- Het wijzigen van de encoder configuratie nadat deze is gestart, heeft negatieve gevolgen voor de gebeurtenis. Configuratie wijzigingen kunnen ertoe leiden dat de gebeurtenis Insta Biel wordt. 
- Zorg ervoor dat u uw gebeurtenis in ruime tijd kunt instellen. Voor grootschalige gebeurtenissen raden we u aan om de installatie een uur voor uw gebeurtenis te starten.

## <a name="becoming-an-on-premises-encoder-partner"></a>Een on-premises coderings partner worden

Als Azure Media Services on-premises encoder-partner Media Services propageert u uw product door uw Codeer aan te bevelen voor zakelijke klanten. Als u een on-premises encoder partner wilt worden, moet u de compatibiliteit van uw on-premises encoder controleren met Media Services. Voer hiervoor de volgende verificaties uit.

### <a name="pass-through-live-event-verification"></a>Geslaagde live gebeurtenis verificatie

1. Controleer of het **streaming-eind punt** wordt uitgevoerd In uw Media Services-account. 
2. Maak en start de gebeurtenis **Pass-Through** Live. <br/> Zie [Live Event states and billing](live-event-states-billing.md) (Statussen en facturering voor livegebeurtenissen) voor meer informatie.
3. De opname-Url's ophalen en uw on-premises Encoder configureren om de URL te gebruiken voor het verzenden van een multi-bitrate live stream naar Media Services.
4. Haal de Preview-URL op en gebruik deze om te controleren of de invoer van het coderings programma daad werkelijk wordt ontvangen.
5. Maak een nieuw **Asset** -object.
6. Een **Live uitvoer** maken en de Asset-naam gebruiken die u hebt gemaakt.
7. Maak een **streaming-Locator** met de ingebouwde **streaming-beleids** typen.
8. Geef een lijst van de paden op de **streaming-Locator** op om terug te gaan naar de url's die u wilt gebruiken.
9. Haal de hostnaam op voor het **streaming-eind punt** van waaruit u gegevens wilt streamen.
10. Combi neer de URL uit stap 8 met de hostnaam in stap 9 om de volledige URL op te halen.
11. Voer uw Live coderings programma uit voor ongeveer 10 minuten.
12. Stop de livegebeurtenis. 
13. Gebruik een speler als [Azure Media Player](https://aka.ms/azuremediaplayer) om de gearchiveerde Asset te bekijken om ervoor te zorgen dat het afspelen geen zicht bare storingen op alle kwaliteits niveaus heeft. U kunt ook controleren en valideren via de Preview-URL tijdens de Live sessie.
14. Registreer de Asset-ID, de gepubliceerde streaming-URL voor het Live Archief en de instellingen en versie die worden gebruikt in uw Live coderings programma.
15. Stel de status van de live-gebeurtenis na het maken van elk voor beeld opnieuw in.
16. Herhaal stap 5 tot en met 15 voor alle configuraties die worden ondersteund door uw coderings programma (met en zonder AD-Signa lering, bijschriften of verschillende coderings snelheden).

### <a name="live-encoding-live-event-verification"></a>Live-gebeurtenis verificatie voor Live-code ring

1. Controleer of het **streaming-eind punt** wordt uitgevoerd In uw Media Services-account. 
2. De live-gebeurtenis voor het **coderen** van Live maken en starten. <br/> Zie [Live Event states and billing](live-event-states-billing.md) (Statussen en facturering voor livegebeurtenissen) voor meer informatie.
3. Haal de opname-Url's op en configureer uw encoder om een live stream met één bitsnelheid te pushen naar Media Services.
4. Haal de Preview-URL op en gebruik deze om te controleren of de invoer van het coderings programma daad werkelijk wordt ontvangen.
5. Maak een nieuw **Asset** -object.
6. Een **Live uitvoer** maken en de Asset-naam gebruiken die u hebt gemaakt.
7. Maak een **streaming-Locator** met de ingebouwde **streaming-beleids** typen.
8. Geef een lijst van de paden op de **streaming-Locator** op om terug te gaan naar de url's die u wilt gebruiken.
9. Haal de hostnaam op voor het **streaming-eind punt** van waaruit u gegevens wilt streamen.
10. Combi neer de URL uit stap 8 met de hostnaam in stap 9 om de volledige URL op te halen.
11. Voer uw Live coderings programma uit voor ongeveer 10 minuten.
12. Stop de livegebeurtenis.
13. Gebruik een speler als [Azure Media Player](https://aka.ms/azuremediaplayer) om de gearchiveerde Asset te bekijken om ervoor te zorgen dat het afspelen geen zicht bare storingen voor alle kwaliteits niveaus heeft. U kunt ook controleren en valideren via de Preview-URL tijdens de Live sessie.
14. Registreer de Asset-ID, de gepubliceerde streaming-URL voor het Live Archief en de instellingen en versie die worden gebruikt in uw Live coderings programma.
15. Stel de status van de live-gebeurtenis na het maken van elk voor beeld opnieuw in.
16. Herhaal stap 5 tot en met 15 voor alle configuraties die worden ondersteund door uw coderings programma (met en zonder AD-Signa lering, bijschriften of verschillende coderings snelheden).

### <a name="longevity-verification"></a>Duurzaamheids verificatie

Volg dezelfde stappen als in [Pass-through live-gebeurtenis verificatie](#pass-through-live-event-verification) , met uitzonde ring van stap 11. <br/>Voer in plaats van tien minuten uw Live coderings programma uit gedurende een week of langer. Gebruik een speler zoals [Azure Media Player](https://aka.ms/azuremediaplayer) om de live-streaming van tijd tot tijd (of een gearchiveerde Asset) te bekijken om ervoor te zorgen dat het afspelen geen zicht bare storingen heeft.

### <a name="email-your-recorded-settings"></a>Uw vastgelegde instellingen per e-mail verzenden

Als laatste moet u de vastgelegde instellingen en de para meters amshelp@microsoft.com van uw Live-Archief per e-mail verzenden naar Azure Media Services als een melding dat alle verificatie controles zelf zijn geslaagd. Neem ook uw contact gegevens op voor elke follow-up. U kunt contact opnemen met het Azure Media Services team met vragen over dit proces.

## <a name="next-steps"></a>Volgende stappen

[Live streamen met Media Services v3](live-streaming-overview.md)
