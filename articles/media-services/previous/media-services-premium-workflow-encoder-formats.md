---
title: Media Encoder Premium Workflow indelingen en codecs | Microsoft Docs
description: Dit onderwerp bevat een overzicht van Media Encoder Premium Workflow indelingen en codecs
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.reviewer: anilmur
ms.openlocfilehash: 9ec8e2df8befa28fc023edb30889f177b60939f9
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "69015042"
---
# <a name="media-encoder-premium-workflow-formats-and-codecs"></a>Media Encoder Premium Workflow indelingen en codecs

> [!NOTE]
> Voor Premium encoder-vragen kunt mepd@microsoft.comu e-mail verzenden.
> 
> Media Encoder Premium Workflow media processor die in dit onderwerp wordt besproken, is niet beschikbaar in China. 

Dit document bevat een lijst met invoer-en uitvoer bestands indelingen en-codecs die worden ondersteund door de open bare preview-versie van het coderings programma **Media Encoder Premium workflow** .

[Invoer indelingen en codecs Media Encoder Premium Workflow](#input_formats)

Uitvoer indelingen en codecs Media Encoder Premium Workflow

**Media Encoder Premium workflow** ondersteunt closed captioning die in [deze](#closed_captioning) sectie wordt beschreven. 

## <a id="input_formats"></a>Invoer indelingen en codecs Media Encoder Premium Workflow

In de volgende sectie worden de codecs en bestands indelingen vermeld die door deze media processor als invoer worden ondersteund.

### <a name="input-containerfile-formats"></a>Invoer van container/bestands indelingen

* Adobe® Flash® F4V
* MXF/SMPTE 377M
* GXF
* MPEG-2-transport stromen
* MPEG-2-programma-streams
* MPEG-4/MP4
* Windows Media/ASF
* AVI (niet-gecomprimeerd 8bit/10bit)

### <a name="input-video-codecs"></a>Video-codecs invoeren

* AVC 8-bits/10-bits, tot 4:2:2, inclusief AVCIntra
* Avid DNxHD (in MXF)
* DVCPro/DVCProHD (in MXF)
* HEVC/H. 265, hoofd-en hoofd-10-profiel
* JPEG2000
* MPEG-2 (Maxi maal 422 profiel en hoog niveau; inclusief varianten zoals XDCAM, XDCAM HD, XDCAM IMX, CableLabs® en D10)
* MPEG-1
* Windows Media Video/VC-1

### <a name="input-audio-codecs"></a>Audio-codecs invoeren

* AES (SMPTE 331M en 302M, AES3-2003)
* Dolby® E
* Dolby® Digital (AC3)
* AAC (AAC-LC, AAC-HE en AAC-HEv2; tot 5,1)
* MPEG Layer 2
* MP3 (MPEG-1 Audio Layer 3)
* Windows Media Audio
* WAV/PCM

## <a id="output_format"></a>Uitvoer indelingen en codecs Media Encoder Premium Workflow

In de volgende sectie worden de codecs en bestands indelingen vermeld die worden ondersteund als uitvoer van deze media processor.

### <a name="output-containerfile-formats"></a>Uitvoer container/bestands indelingen

* Adobe® Flash® F4V
* MXF (OP1a, XDCAM en AS02)
* DPP (inclusief AS11)
* GXF
* MPEG-4/MP4
* Windows Media/ASF
* AVI (niet-gecomprimeerd 8bit/10bit)
* Smooth Streaming bestands indeling (PIFF 1,3)
* MPEG-TS 

### <a name="output-video-codecs"></a>Video-codecs voor uitvoer

* AVC (H. 264; 8-bits; tot hoog profiel, niveau 5,2; 4 KB Ultra HD; AVC-Intra)
* Avid DNxHD (in MXF)
* DVCPro/DVCProHD (in MXF)
* MPEG-2 (Maxi maal 422 profiel en hoog niveau; inclusief varianten zoals XDCAM, XDCAM HD, XDCAM IMX, CableLabs® en D10)
* MPEG-1
* Windows Media Video/VC-1
* JPEG-miniatuur maken
* HEVC (H. 265; 8-bits en 10-bits, hoofd-en hoofd-10-profiel)

  Ondersteuning voor HDR 10 is beschikbaar in bepaalde scenario's. Neem contact mepd@microsoft.com op met voor meer informatie


### <a name="output-audio-codecs"></a>Audio-codecs voor uitvoer

* AES (SMPTE 331M en 302M, AES3-2003)
* Dolby® Digital (AC3)
* Dolby® Digital Plus (E-AC3) tot 7,1
* AAC (AAC-LC, AAC-HE en AAC-HEv2; tot 5,1)
* MPEG Layer 2
* MP3 (MPEG-1 Audio Layer 3)
* Windows Media Audio

>[!NOTE]
>Als u codeert naar Dolby® Digital (AC3), kan de uitvoer alleen naar een ISO MP4-bestand worden geschreven.

## <a id="closed_captioning"></a>Ondersteuning voor ondertiteling

**Media Encoder Premium workflow** ondersteunt in opname:

1. SCC-bestanden
2. SMPTE-TT-bestanden
3. CEA-608/CEA-708: wordt als gebruikers gegevens (SEI berichten van H. 264-elementaire stromen, ATSC/53, SCTE20) of vervoerd als bijkomende gegevens in MXF/GXF-bestanden
4. STL-ondertitelings bestanden

Bij uitvoer zijn de volgende opties beschikbaar:

1. CEA-608 naar CEA-708-vertaling
2. CEA-608/CEA-708 Pass Through (Inge sloten in SEI berichten van H. 264 elementaire stromen of vervoerd als bijkomende gegevens in MXF-bestanden)
3. SCC
4. Getimede tekst van SMPTE (van bron CEA-608 per SMPTE RP2052, inclusief het maken van DFXP-bestanden)
5. Ondertitelings bestand SRT
6. DVB-ondertitel stromen

> [!NOTE]
> Niet alle hierboven genoemde uitvoer indelingen worden ondersteund voor levering via streaming in Azure Media Services.

## <a name="known-issues"></a>Bekende problemen

Als uw invoer video geen ondertiteling bevat, bevat het uitvoer activum nog een leeg TTML-bestand. 

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

