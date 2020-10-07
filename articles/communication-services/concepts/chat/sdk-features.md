---
title: Overzicht van de chat-clientbibliotheek voor Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Meer informatie over de chat-client bibliotheek voor Azure Communication Services.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 09/30/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 3b66c962758afc28da3c6be938c13d768c4f2572
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2020
ms.locfileid: "91666140"
---
# <a name="chat-client-library-overview"></a>Overzicht van de chat-clientbibliotheek

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

Chat-clientbibliotheken van Azure Communication Services kunnen worden gebruikt om uitgebreide realtime chatmogelijkheden aan uw toepassingen toe te voegen.

## <a name="chat-client-library-capabilities"></a>Mogelijkheden van chat-clientbibliotheken

De volgende lijst bevat de set van functies die momenteel beschikbaar zijn in de Communication Services die chat-clientbibliotheken aanroepen.

| Groep van functies | Mogelijkheid                                                                                                          | JS  | Java | .NET | Python |
| ----------------- | ------------------------------------------------------------------------------------------------------------------- | --- | ----- | ---- | -----  |
| Belangrijkste mogelijkheden | Een chatgesprek maken tussen 2 of meer gebruikers (max. 250 gebruikers)                                                       | ✔️   | ✔️  | ✔️    | ✔️   |
|                   | Het onderwerp van een chatgesprek bijwerken                                                                              | ✔️   | ✔️ | ✔️    | ✔️   |
|                   | Leden toevoegen aan of verwijderen uit een chatgesprek                                                                           | ✔️   | ✔️  | ✔️    | ✔️  |
|                   | Kies of u de chatberichtgeschiedenis wilt delen met nieuw toegevoegde leden - *alle/geen/tot een bepaald tijdstip* | ✔️   | ✔️   | ✔️    | ✔️  |
|                   | Een lijst met alle gesprek van chatleden ophalen                                                                          | ✔️   | ✔️  | ✔️ | ✔️ |
|                   | Een chatgesprek verwijderen                                                                                              | ✔️   | ✔️  | ✔️    | ✔️  |
|                   | Een lijst ophalen van de lidmaatschappen van chatgesprekken van gebruikers                                                                  | ✔️   | ✔️  | ✔️    | ✔️  |
|                   | Informatie ophalen voor een bepaald chatgesprek                                                                              | ✔️   | ✔️  | ✔️ | ✔️ |
|                   | Berichten verzenden en ontvangen in een chatgesprek                                                                            | ✔️   | ✔️   | ✔️    | ✔️  |
|                   | De inhoud van een bericht bewerken nadat het is verzonden                                                                   | ✔️   | ✔️  | ✔️ | ✔️ |
|                   | Een bericht verwijderen                                                                                                       | ✔️   | ✔️  | ✔️ | ✔️ |
|                   | Een bericht met prioriteit Normaal of Hoog labelen op het moment van verzenden                                               | ✔️   | ✔️  | ✔️    | ✔️   |
|                   | Leesbevestigingen verzenden en ontvangen voor berichten die door leden zijn gelezen <br/> *Niet beschikbaar wanneer een chatgesprek meer dan 20 leden bevat*    | ✔️   | ✔️  | ✔️    | ✔️   |
|                   | Meldingen verzenden en ontvangen wanneer een lid actief een bericht in een chatgesprek typt <br/> *Niet beschikbaar wanneer een chatgesprek meer dan 20 leden bevat*      | ✔️   | ✔️   | ✔️    | ✔️    |
|                   | Alle berichten in een chatgesprek ophalen <br/> *Ondersteuning voor Unicode-emoji's*                                                  | ✔️   | ✔️  | ✔️    | ✔️  |
|                   | Emoji's verzenden als onderdeel van de berichtinhoud                                                                              | ✔️   | ✔️  | ✔️    | ✔️  |
|Realtime-signalering (mogelijk door een bedrijfseigen signaleringspakket)| Melding ontvangen wanneer een gebruiker een nieuw bericht ontvangt in een chatgesprek waarvan hij lid is                                     | ✔️   | ❌    | ❌  | ❌  |
|                    | Melding ontvangen wanneer een bericht is bewerkt door een ander lid in een chatgesprek waarvan hij lid is                | ✔️   | ❌    | ❌    | ❌  |
|                    | Melding ontvangen wanneer een bericht is verwijderd door een ander lid in een chatgesprek waarvan hij lid is                | ✔️   | ❌    | ❌    | ❌  |
|                    | Melding ontvangen wanneer een ander lid van het chatgesprek typt                                                             | ✔️   | ❌    | ❌    | ❌  |
|                    | Melding ontvangen wanneer een ander lid een bericht heeft gelezen (leesbevestiging) in het chatgesprek                               | ✔️   | ❌    | ❌    | ❌  |
| Gebeurtenissen             | Event Grid gebruiken om u te abonneren op gebruikersactiviteiten die zich voordoen in chatgesprekken en om aangepaste meldingsservices of bedrijfslogica te integreren     | ✔️   | ✔️  | ✔️    | ✔️  |
| Bewaking        | Gebruik (aantal verzonden berichten) bewaken                                                                               | ✔️   | ✔️  | ✔️    | ✔️  |
|                    | De kwaliteit en status van API-aanvragend or uw app bewaken en waarschuwingen configureren via de portal                                                          | ✔️   | ✔️  | ✔️    | ✔️  |
|Aanvullende functies | [Cognitive Services-API's](https://docs.microsoft.com/azure/cognitive-services/) gebruiken in combinatie met de chat-clientbibliotheek om intelligente functies in te schakelen: *vertaling en sentimentanalyse van het binnenkomende bericht op een client, spraak-naar-tekst-conversie om een bericht op te stellen wanneer het lid spreekt, enzovoort.*                                                                                         | ✔️   | ✔️  | ✔️    | ✔️  |

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aan de slag met chat](../../quickstarts/chat/get-started.md)

De volgende documenten zijn mogelijk interessant voor u:

- Uzelf bekend maken met de [chatconcepten](../chat/concepts.md)
