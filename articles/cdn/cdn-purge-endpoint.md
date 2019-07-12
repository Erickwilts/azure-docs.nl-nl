---
title: Een Azure CDN-eindpunt leegmaken | Microsoft Docs
description: Informatie over het verwijderen van alle inhoud in cache uit een Azure CDN-eindpunt.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: sohamnchatterjee
ms.assetid: 0b50230b-fe82-4740-90aa-95d4dde8bd4f
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2019
ms.author: magattus
ms.openlocfilehash: ff877810cb32d22cffd2af79880b6223c41d7d73
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593530"
---
# <a name="purge-an-azure-cdn-endpoint"></a>Een Azure CDN-eindpunt leegmaken
## <a name="overview"></a>Overzicht
Azure CDN edge-knooppunten cache activa totdat van de asset time-to-live (TTL) verloopt.  Nadat de TTL van de asset is verlopen, wanneer een client vraagt om de asset van het edge-knooppunt, het edge-knooppunt wordt een nieuwe bijgewerkte kopie van de activa voor het bieden van de clientaanvraag ophalen en store de cache wordt vernieuwd.

De beste manier om ervoor te zorgen dat uw gebruikers altijd de meest recente versie van uw activa ophalen is versie van de activa voor elke update en deze publiceren als nieuwe URL's.  CDN haalt onmiddellijk de nieuwe activa voor de volgende aanvragen van clients.  Soms wilt u mogelijk in de cache opgeslagen inhoud verwijderen uit alle edge-knooppunten en zorgen dat ze alle nieuwe bijgewerkte activa ophalen.  Dit kan komen door updates aan uw webtoepassing of aan snel update-activa met onjuiste gegevens bevatten.

> [!TIP]
> Houd er rekening mee dat alleen inhoud op het CDN edge-servers in de cache wissen wist.  Alle downstream caches, zoals de WAP-servers en de lokale caches van, kunnen nog steeds een kopie van het bestand bevatten.  Het is belangrijk om te onthouden dit als u van een bestand time-to-live.  U kunt afdwingen dat een downstream-client om aan te vragen van de meest recente versie van het bestand door een unieke naam geven telkens wanneer u deze bijwerken, of door te profiteren van [queryreeksen opslaan in cache](cdn-query-string.md).  
> 
> 

In deze zelfstudie helpt u bij het opschonen van de activa van alle edge-knooppunten van een eindpunt.

## <a name="walkthrough"></a>Walkthrough
1. In de [Azure Portal](https://portal.azure.com), blader naar het CDN-profiel met het eindpunt dat u wilt leegmaken.
2. Klik op de knop verwijderen uit de blade CDN-profiel.
   
    ![Blade CDN-profiel](./media/cdn-purge-endpoint/cdn-profile-blade.png)
   
    Het opschonen van Logboeken-blade wordt geopend.
   
    ![CDN leegmaken blade](./media/cdn-purge-endpoint/cdn-purge-blade.png)
3. Selecteer op de blade opschonen van het serviceadres dat u wilt leegmaken in de vervolgkeuzelijst URL.
   
    ![Formulier verwijderen](./media/cdn-purge-endpoint/cdn-purge-form.png)
   
   > [!NOTE]
   > U kunt ook naar de blade opschonen ophalen door te klikken op de **opschonen** knop op de blade CDN-eindpunt.  In dat geval de **URL** veld wordt ingevuld met de serviceadres van dat specifieke eindpunt zijn.
   > 
   > 
4. Selecteer welke elementen die u wilt leegmaken van het edge-knooppunten.  Als u wissen van alle activa wilt, klikt u op de **Alles leegmaken** selectievakje.  Anders typt u het pad van de activa die u leegmaken wilt de **pad** tekstvak. Hieronder indelingen worden ondersteund in het pad.
    1. **Opschonen van één URL**: Individuele asset opschonen door de volledige URL op te geven met of zonder de bestandsextensie, bijvoorbeeld`/pictures/strasbourg.png`; `/pictures/strasbourg`
    2. **Leegmaken met jokertekens**: Sterretje (\*) kan worden gebruikt als een jokerteken. Opschonen van alle mappen, submappen en bestanden in een eindpunt met `/*` in het pad of opschonen van alle submappen en bestanden in een specifieke map door te geven van de map gevolgd door `/*`, bijvoorbeeld`/pictures/*`.  Houd er rekening mee dat leegmaken met jokertekens wordt momenteel niet ondersteund door Azure CDN van Akamai. 
    3. **Opschonen van domein hoofdmap**: Opschonen van de hoofdmap van het eindpunt met '/' in het pad.
   
   > [!TIP]
   > Paden moeten worden opgegeven voor het opschonen van Logboeken en moet een relatieve URL die geschikt zijn voor de volgende [reguliere expressie](/dotnet/standard/base-types/regular-expression-language-quick-reference). **Alles leegmaken** en **leegmaken met jokertekens** niet wordt ondersteund door **Azure CDN van Akamai** op dit moment.
   > > Opschonen van één URL `@"^\/(?>(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/?)*)$";`  
   > > Query-tekenreeks `@"^(?:\?[-\@_a-zA-Z0-9\/%:;=!,.\+'&\(\)\u0020]*)?$";`  
   > > Leegmaken met jokertekens `@"^\/(?:[a-zA-Z0-9-_.%=\(\)\u0020]+\/)*\*$";`. 
   > 
   > Meer **pad** tekstvakken worden weergegeven nadat voert u tekst kunt u een lijst met meerdere elementen te maken.  U kunt assets kan verwijderen uit de lijst door te klikken op de knop met het weglatingsteken (...).
   > 
5. Klik op de **opschonen** knop.
   
    ![Knop verwijderen](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [!IMPORTANT]
> Opschonen van aanvragen nodig ongeveer twee minuten om het proces met **Azure CDN van Microsoft** en **Azure CDN van Verizon** (standaard en premium), en ongeveer 10 seconden met **Azure CDN van Akamai**.  Azure CDN heeft een limiet van 50 gelijktijdige aanvragen voor wissen op een bepaald moment op het niveau van het profiel. 
> 
> 

## <a name="see-also"></a>Zie ook
* [Vooraf assets op een Azure CDN-eindpunt laden](cdn-preload-endpoint.md)
* [Naslaginformatie over Azure CDN REST-API - opschonen of een eindpunt vooraf laden](/rest/api/cdn/endpoints)

