---
title: Microsoft Threat Modeling Tool release 07/29/2020-Azure
description: Documenteer de release opmerkingen voor de 7.3.00729.1 van het hulp programma voor het maken van bedreigings modellen.
author: jegeib
ms.author: jegeib
ms.service: security
ms.topic: article
ms.date: 07/29/2020
ms.openlocfilehash: fc343f292fa32ed2db67dd74aba7a66dbc00d6ab
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91317834"
---
# <a name="threat-modeling-tool-update-release-73007291---07292020"></a>Threat Modeling Tool Update release 7.3.00729.1-07/29/2020

Versie 7.3.00729.1 van de Microsoft Threat Modeling Tool (TMT) is uitgebracht op 29 2020 juli en bevat de volgende wijzigingen:

- Opgeloste fouten
 
## <a name="known-issues"></a>Bekende problemen

### <a name="errors-related-to-tmt7application-file-deserialization"></a>Fouten met betrekking tot het deserialiseren van TMT7. Application file

#### <a name="issue"></a>Probleem

Sommige klanten hebben het volgende fout bericht gerapporteerd tijdens het downloaden van de Threat Modeling Tool:

```
The threat model file '$PATH\TMT7.application' could not be deserialized. File is not an actual threat model or the threat model may be corrupted.
```

Deze fout treedt op omdat sommige browsers geen systeem eigen ondersteuning bieden voor de installatie van ClickOnce. In deze gevallen wordt het bestand van de ClickOnce-toepassing gedownload naar de harde schijf van de gebruiker.

#### <a name="workaround"></a>Tijdelijke oplossing

Deze fout wordt weer gegeven als de Threat Modeling Tool wordt gestart door te dubbel klikken op het bestand TMT7. Application. Het hulp programma werkt echter normaal na het overs laan van de fout. In plaats van de Threat Modeling Tool te starten door te dubbel klikken op het bestand TMT7. Application, moeten gebruikers tijdens de installatie snelkoppelingen gebruiken die zijn gemaakt in het Windows-menu om de Threat Modeling Tool te starten.

## <a name="system-requirements"></a>Systeemvereisten

- Ondersteunde besturingssystemen
  - [Micro soft Windows 10 jubileum update](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) of hoger
- .NET-versie vereist
  - [.Net 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262) of hoger
- Aanvullende vereisten
  - Er is een Internet verbinding vereist voor het ontvangen van updates voor het hulp programma en voor sjablonen.

## <a name="documentation-and-feedback"></a>Documentatie en feedback

- Documentatie voor de Threat Modeling Tool bevindt zich op [docs.Microsoft.com](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool)en bevat informatie [over het gebruik van het hulp programma](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-getting-started).

## <a name="next-steps"></a>Volgende stappen

Down load de nieuwste versie van de [Microsoft Threat Modeling tool](https://aka.ms/threatmodelingtool).
