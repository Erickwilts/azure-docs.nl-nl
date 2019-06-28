---
title: Dynamics 365 voor Customer Engagement bieden vereisten | Azure Marketplace
description: De vereisten voor het publiceren van een Azure-toepassing aanbieden op Azure Marketplace.
services: Dynamics 365 for Customer Engagement offer, Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: pabutler
ms.openlocfilehash: 9cfec65843012e781c8a8dd06cbc4d1225b0dd36
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67338893"
---
# <a name="dynamics-365-for-customer-engagement-prerequisites"></a>Dynamics 365 voor Customer Engagement-vereisten

Dit artikel beschrijft de technische en zakelijke vereisten voor het publiceren van een Dynamics 365 voor Customer Engagement toepassing aanbieding op de AppSource-Marketplace.  Als u hebt nog niet gedaan, raadpleegt u de [Office 365, Dynamics 365, PowerApps en Power BI bieden Publicatiehandleiding voor](../../appsource-offer-publishing-guide.md).


## <a name="technical-requirements"></a>Technische vereisten

Uw Dynamics 365 voor Customer Engagement-toepassing moet voldoen aan de [beoordelingsrichtlijnen voor app van Microsoft AppSource](https://smp-cdn-prod.azureedge.net/documents/AppsourceGuidelines/Microsoft%20AppSource%20app%20review%20guidelines_v5.pdf), waaronder de volgende vereisten:


|              Vereiste             |        Description           |
|            ---------------           |      ---------------         |
| Integratie van Azure Active Directory   | Uw app in Azure Active Directory voor federatieve eenmalige aanmelding moet toestaan (AAD federatieve SSO) met toestemming ingeschakeld. Zie voor meer informatie, [over het verkrijgen van AppSource-gecertificeerd voor Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/howto-get-appsource-certified). |
| Integratie met Microsoft Cloud services (optioneel) | Wanneer deze functionaliteit vereist is, moet uw app integreren met andere Microsoft-Cloud-services zoals Microsoft Power BI, Microsoft Flow of Microsoft Azure-services zoals machine learning en cognitive services. |
| Line-of-business gericht            |  Uw app moet zich richten op een goed gedefinieerde bedrijfsproces of uitgeven, voornamelijk gericht op zakelijke klanten en kunnen gebruikers zich aanmelden met hun werkreferenties (gebruikersnaam en wachtwoord).  |
| Gratis proefperiode is afgelopen en evaluatiesoftware |  Een klant moet mogelijk uw app gedurende een beperkte periode gratis te gebruiken: een 'nu downloaden"voor gratis apps, een 'gratis proefversie' voor een opgegeven periode, een demonstrator 'Test drive' of een 'Contact opnemen' optie aanvragen.  |
| Geen/laag configureren                 | Uw app moet zijn eenvoudig en snel te configureren en instellen (geen ontwikkeling of aanpassing vereist).  |
| Klantondersteuning                     | Ondersteuning voor uw app moet bevatten een koppeling voor ondersteuning waar klanten hulp kunnen vinden.  |
| Beschikbaarheid/uptime                  | Uw app moet een uptime van ten minste 99,9% hebben. |
|  |  |


## <a name="business-requirements"></a>Zakelijke vereisten

De zakelijke vereisten zijn onder andere de volgende procedure, contractuele en wettelijke verplichtingen:

* U moet worden geregistreerd op de [Microsoft Partner Network (MPN)](https://partners.microsoft.com/PartnerProgram/simplifiedenrollment.aspx) of een geregistreerde Cloud-Marketplace-uitgever. Als u niet bent geregistreerd, volg de stappen in [geworden van een Cloud-Marketplace-uitgever](https://docs.microsoft.com/azure/marketplace/become-publisher).  (U kunt ook verbinding maken met de [Dynamics 365 voor Customer Engagement-voorbereidingsteam](https://experience.dynamics.com/isvengage/)).

    >[!NOTE]
    >Het account voor registratie van dezelfde Microsoft Developer Center moet u zich aanmeldt bij de Cloud Partner-Portal. U hebt slechts één Microsoft-account voor uw aanbiedingen op Azure Marketplace. Dit account mag niet zijn specifiek voor afzonderlijke services of aanbiedingen.

* Omdat AppSource niet een publicatieoptie commerce-functionaliteit bieden, moet u uw huidige bestellen en facturering infrastructuur zonder extra worden geïnvesteerd of wijzigingen.
* U bent verantwoordelijk voor het maken van technische ondersteuning beschikbaar voor klanten in een commercieel redelijke manier. Deze ondersteuning is gratis, betaald, of via de community-benaderingen.
* U bent zelf verantwoordelijk voor licentiëring van uw software en eventuele afhankelijkheden voor software van derden.
* U moet de bijbehorende marketingmateriaal, zoals een officiële app-naam, beschrijving (in HTML-indeling), logo's in PNG-indeling (40, 40, 90 x 90, 115 x 115 en 255 x 115 pixels) en de gebruiksrechtovereenkomst en privacybeleid hebt gemaakt.  


## <a name="next-steps"></a>Volgende stappen

Nadat u deze vereisten wordt voldaan, kunt u [een Dynamics 365 Customer Engagement-aanbieding maken](./cpp-create-offer.md) 
