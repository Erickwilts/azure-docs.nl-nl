---
title: Bestaande spelers gebruiken om uw inhoud af te spelen-Azure | Microsoft Docs
description: In dit artikel vindt u een lijst met bestaande spelers die u kunt gebruiken om uw inhoud af te spelen.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 7e9fcf89-0fb6-4fa4-96cb-666320684d69
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: bd09b734f1ac5ac3c98b6c0c717a48de19b0106f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "89261067"
---
# <a name="playing-your-content-with-existing-players"></a>Uw inhoud afspelen op bestaande spelers

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

Azure Media Services ondersteunt veel populaire streaming-indelingen, zoals Smooth Streaming, HTTP Live Streaming en MPEG-Dash. In dit onderwerp vindt u een overzicht van de bestaande spelers die u kunt gebruiken om uw stromen te testen.

### <a name="the-azure-portal-media-services-content-player"></a>De Azure Portal Media Services inhouds speler
De **Azure** -Portal biedt een inhouds speler die u kunt gebruiken om uw video te testen.

Klik op de gewenste video (Controleer of deze is [gepubliceerd](media-services-portal-publish.md)) en klik op de knop **afspelen** onder aan de portal.

Hierbij geldt het volgende:

* **MEDIA SERVICES CONTENT PLAYER** speelt af vanaf het standaard streaming-eindpunt. Als u wilt afspelen vanaf een ander streaming-eindpunt, gebruikt u een andere speler. Bijvoorbeeld [Azure Media Player](https://aka.ms/azuremediaplayer).

![AMSPlayer][AMSPlayer]

### <a name="azure-media-player"></a>Azure Media Player

Gebruik [Azure Media Player](https://aka.ms/azuremediaplayer) om uw inhoud (helder of beveiligd) in een van de volgende indelingen af te spelen:

* Smooth Streaming
* MPEG DASH
* HLS
* Progressieve MP4

### <a name="flash-player"></a>Flash-speler

#### <a name="playready-with-token"></a>PlayReady met token

[https://sltoken.azurewebsites.net](https://sltoken.azurewebsites.net)

### <a name="dash-players"></a>STREEPJE-spelers

[https://dashplayer.azurewebsites.net](https://dashplayer.azurewebsites.net)

[https://dashif.org](https://dashif.org)

### <a name="other"></a>Anders
Als u HLS-Url's wilt testen, kunt u ook het volgende gebruiken:

* **Safari** op een IOS-apparaat of
* **3IVX HLS Player** in Windows.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
