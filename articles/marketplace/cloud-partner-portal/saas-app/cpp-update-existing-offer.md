---
title: Bijwerken van bestaande aanbieding van Azure SaaS-toepassing | Azure Marketplace
description: Het bijwerken van een bestaande SaaS-toepassing-aanbieding op Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: pbutlerm
ms.openlocfilehash: 2195c9a5e1f0d3683ea8cf6564d97cbabd072f81
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65834203"
---
# <a name="update-an-existing-saas-application-offer"></a>Een bestaande aanbieding van de SaaS-toepassing bijwerken

Er zijn verschillende soorten updates die u doen naar uw aanbieding wilt mogelijk nadat deze wordt gepubliceerd en actief is. Eventuele wijzigingen die u in uw nieuwe versie van uw aanbieding aanbrengt moet worden opgeslagen en opnieuw om deze weer in de Marketplace worden gepubliceerd. In dit artikel wordt stapsgewijs uitgelegd de verschillende aspecten van het bijwerken van uw SaaS-aanbod in de [Cloud Partner-Portal](https://cloudpartner.azure.com/).

> [!IMPORTANT] 
> SaaS biedt functionaliteit wordt gemigreerd naar de [Microsoft Partner Center](https://partner.microsoft.com/dashboard/directory).  Alle nieuwe uitgevers moeten Partner Center gebruiken voor het maken van nieuwe SaaS-aanbiedingen en bestaande aanbiedingen beheren.  Huidige uitgevers met SaaS-aanbiedingen zijn batchwise worden gemigreerd van de Cloud Partner-Portal naar het Partnercentrum.  De Cloud Partner-Portal wordt weergegeven statusberichten om aan te geven wanneer specifieke bestaande aanbiedingen zijn gemigreerd.

Er zijn diverse redenen waarom u wilt mogelijk bijwerken van uw aanbieding, zoals:

- Een nieuwe versie toevoegen aan een bestaande app.
- Een app wordt bijgewerkt.
- Nieuwe functies toe te voegen aan een app.
- De metagegevens van de marketplace voor de aanbieding wordt bijgewerkt.

Om te helpen u bij deze wijzigingen, de portal biedt de **vergelijken** en **geschiedenis** functies.

## <a name="unpermitted-changes-to-a-saas-offer"></a>Ongeoorloofde wijzigingen in een SaaS-aanbieding

Er zijn kenmerken van een SaaS-aanbieding die niet kan worden gewijzigd nadat de aanbieding gepubliceerd op Azure Marketplace is. U kunt de volgende instellingen niet wijzigen:

- Aanbiedings-ID en uitgevers-ID van de aanbieding
- Versie-tags, bijvoorbeeld: `1.0.1`
- Licentie/facturering model wijzigingen in bestaande aanbiedingen.

## <a name="common-update-operations"></a>Algemene updatebewerkingen
 
De volgende updatebewerkingen komen veel voor.

### <a name="update-offer-contacts"></a>Aanbieding contactpersonen bijwerken

Gebruik de volgende stappen uit om bij te werken van de contactpersonen voor ondersteuning van uw aanbieding.

1. Meld u aan bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).
2. Onder **alle aanbiedingen**, vinden de aanbieding die u wilt bijwerken.
3. Ga naar de **contactpersonen** tabblad. Uw contactpersonen bijwerken.
4. Selecteer **publiceren** om de werkstroom voor het publiceren van uw wijzigingen te starten.


### <a name="update-offer-marketplace-metadata"></a>Metagegevens van marketplace-aanbieding update

Gebruik de volgende stappen uit om bij te werken van de marketplace-metagegevens die zijn gekoppeld aan uw aanbieding. (Bijvoorbeeld: bedrijf naam, logo's, enz.)

1. Meld u aan bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/).
2. Onder **alle aanbiedingen**, vinden de aanbieding die u wilt bijwerken.
3. Ga naar de **Storefront Details** tabblad. Volg de instructies in de [publiceren SaaS bieden](./cpp-publish-offer.md) artikel om wijzigingen in metagegevens.
4. Selecteer **publiceren** om de werkstroom voor het publiceren van uw wijzigingen te starten.

## <a name="compare-feature"></a>Functie vergelijken

Wanneer u wijzigingen in een gepubliceerde aanbieding aanbrengt, kunt u de functie vergelijken om te controleren van de wijzigingen die u hebt aangebracht. De volgende schermopname ziet u de optie vergelijken voor een gepubliceerde aanbieding.

![Gebruik vergelijken om te zien bieden wijzigingen in de Cloud Partner-Portal](./media/saas-offer-compare.png)

### <a name="to-use-the-compare-feature"></a>De functie vergelijken gebruiken:

1. Op elk gewenst moment in het bewerkingsproces vergelijken voor uw aanbieding te selecteren.
2. Side-by-side-versies van marketing activa en metagegevens bekijken.

## <a name="publishing-history"></a>Geschiedenis van publiceren

Historische publicatie-activiteit, selecteer de **geschiedenis** tabblad op de navigatiebalk aan de linkerkant menu balk van Cloud Partner-Portal. Hier ziet u de voorzien van een tijdstempel acties tijdens de levensduur van uw Azure Marketplace-aanbiedingen.

![Zie bieden geschiedenis in de Cloud Partner-Portal](./media/saas-offer-history.png)

U kunt de pagina van de geschiedenis Audit gebruiken om te zoeken naar een specifieke Aanbiedings en toepassen van filters zoals uitgever, aanbieding en Type van de gebeurtenis (bijvoorbeeld OfferGoLiveRequested.) U kunt ook de controlegeschiedenis downloaden als csv-bestand.


## <a name="next-steps"></a>Volgende stappen

[Aanbieding voor SaaS-toepassing](./cpp-saas-offer.md)