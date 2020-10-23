---
title: 'Zelfstudie: Migreren van Google Maps naar Azure Maps | Microsoft Azure Maps'
description: Zelfstudie over hoe u van Google Maps migreert naar Microsoft Azure Maps. U wordt begeleid bij het overschakelen naar Azure Maps-API's en SDK's.
author: rbrundritt
ms.author: richbrun
ms.date: 09/23/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: ee7eda58c211ca570b052d55c813999e62b95fde
ms.sourcegitcommit: fbb620e0c47f49a8cf0a568ba704edefd0e30f81
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91876308"
---
# <a name="tutorial---migrate-from-google-maps-to-azure-maps"></a>Zelfstudie: Migreren van Google Maps naar Azure Maps

In dit artikel vindt u informatie over het migreren van web-, mobiele en servertoepassingen van Google Maps naar het Microsoft Azure Maps-platform. Deze zelfstudie bevat vergelijkende codevoorbeelden, suggesties voor migratie en best practices voor migratie naar Azure Maps.

## <a name="azure-maps-platform-overview"></a>Overzicht van het Azure Maps-platform

Azure Maps biedt ontwikkelaars uit alle sectoren krachtige georuimtelijke mogelijkheden. De mogelijkheden zijn verpakt met regelmatige bijgewerkte kaartgegevens om geografische context te bieden voor web- en mobiele toepassingen. Azure Maps heeft een met Azure One API-compatibele set REST API's. De REST API's bieden mogelijkheden voor rendering van kaarten, zoekfuncties, navigatie-instructies, verkeer, tijdzones, geolocatie, geofencing, kaartgegevens, weer, mobiliteit en Spatial Operations. Bewerkingen worden verstrekt met zowel web- als Android-SDK's om ontwikkeling op meerdere platforms eenvoudig, flexibel en draagbaar te maken.

## <a name="high-level-platform-comparison"></a>Algemene platformvergelijking

De tabel bevat een globale lijst van Azure Maps-functies die overeenkomen met de functies van Google Maps. In deze lijst worden niet alle functies van Azure Maps weergegeven. Aanvullende functies van Azure Maps zijn onder andere: toegankelijkheid, geofencing, isochronen, ruimtelijke bewerkingen, directe toegang tot kaarttegels, batchservices en vergelijking van gegevensdekking (dat wil zeggen fotodekking).

| Google Maps-functie         | Azure Maps-ondersteuning                     |
|-----------------------------|:--------------------------------------:|
| Web SDK                     | ✓                                      |
| Android SDK                 | ✓                                      |
| iOS SDK                     | Gepland                                |
| REST Service-API's           | ✓                                      |
| Aanwijzingen (navigatie)        | ✓                                      |
| Afstandsmatrix             | ✓                                      |
| Terrein                   | Gepland                                |
| Geocodering (vooruit/achteruit) | ✓                                      |
| Geolocatie                 | N.v.t.                                    |
| Dichtstbijzijnde wegen               | ✓                                      |
| Locaties zoeken               | ✓                                      |
| Details over plaatsen              | N.v.t.: website en telefoonnummer beschikbaar |
| Foto's van plaatsen               | N.v.t.                                    |
| Locatie automatisch aanvullen          | ✓                                      |
| Uitlijnen op weg                | ✓                                      |
| Snelheidslimieten                | ✓                                      |
| Statische kaarten                 | ✓                                      |
| Statische straatweergave          | N.v.t.                                    |
| Tijdzone                   | ✓                                      |
| In Maps ingesloten API           | N.v.t.                                    |
| Kaart-URL's                    | N.v.t.                                    |

Google Maps biedt basisverificatie op basis van een sleutel. Azure Maps biedt zowel basisverificatie op basis van sleutels als Azure Active Directory-verificatie. Azure Active Directory-verificatie biedt meer beveiligingsfuncties, vergeleken met de basisverificatie op basis van sleutels.

## <a name="licensing-considerations"></a>Licentieoverwegingen

Houd wat licenties betreft rekening met de volgende punten wanneer u van Google Maps overstapt op Azure Maps.

- Azure Maps brengt kosten in rekening voor het gebruik van interactieve kaarten; deze kosten zijn gebaseerd op het aantal geladen kaarttegels. Aan de andere kant brengt Google Maps kosten in rekening voor het laden van het kaartbesturingselement. In de interactieve SDK's van Azure Maps worden kaarttegels automatisch in de cache geplaatst om de ontwikkelingskosten te beperken. Er wordt één Azure Maps-transactie gegenereerd voor elke 15 kaarttegels die worden geladen. De interactieve SDK's van Azure Maps gebruiken tegels van 512 pixels en er wordt gemiddeld een of minder transacties per paginaweergave gegenereerd.
- Vaak is het kosteneffectiever om statische kaartafbeeldingen van Google Maps-webservices te vervangen door de SDK van de Azure Maps-webservice. De SDK van de Azure Maps-webservice maakt gebruik van kaarttegels. De service genereert vaak slechts een fractie van een transactie per geladen kaart, tenzij de gebruiker de kaart pant en erop in-/uitzoomt. De SDK van de Azure Maps-webservice bevat opties voor het uitschakelen van pannen en zoomen, indien gewenst. Daarnaast biedt de SDK van de Azure Maps-webservice veel meer visualisatieopties dan de statischekaartservice.
- Met Azure Maps kunnen gegevens van het platform worden opgeslagen in Azure. De gegevens kunnen gedurende een periode van maximaal zes maanden volgens de [gebruiksvoorwaarden](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=46) ook ergens anders in de cache worden opgeslagen.

Hier volgen enkele verwante informatiebronnen voor Azure Maps:

- [Pagina met Azure Maps-prijzen](https://azure.microsoft.com/pricing/details/azure-maps/)
- [Azure-prijscalculator](https://azure.microsoft.com/pricing/calculator/?service=azure-maps)
- [Azure Maps-gebruiksvoorwaarden](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=46) (opgenomen in de Voorwaarden voor Online Diensten van Microsoft)
- [De juiste prijscategorie kiezen in Azure Maps](https://docs.microsoft.com/azure/azure-maps/choose-pricing-tier)

## <a name="suggested-migration-plan"></a>Voorgesteld migratieplan

Het volgende is een algemeen migratieplan.

1. Maak inventarisatie van de Google Maps-SDK's en -services die door uw toepassing worden gebruikt. Controleer of Azure Maps alternatieve SDK's en services biedt.
2. Maak een Azure-abonnement via [https://azure.com](https://azure.com) indien u nog geen abonnement hebt.
3. Maak een Azure Maps-account ([documentatie](https://docs.microsoft.com/azure/azure-maps/how-to-manage-account-keys)) en de verificatiesleutel of Azure Active Directory ([documentatie](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication)).
4. Migreer uw toepassingscode.
5. Test uw gemigreerde toepassing.
6. Implementeer uw gemigreerde toepassing naar productie.

## <a name="create-an-azure-maps-account"></a>Een Azure Maps-account maken

Ga als volgt te werk om een Azure Maps-account te maken en toegang te krijgen tot het Azure Maps-platform:

1. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.
2. Meld u aan bij de [Azure-portal](https://portal.azure.com/).
3. Maak een [Azure Maps-account](https://docs.microsoft.com/azure/azure-maps/how-to-manage-account-keys). 
4. [Download uw Azure Maps-abonnementssleutel](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication#view-authentication-details) of configureer Azure Active Directory-verificatie voor verbeterde beveiliging.

## <a name="azure-maps-technical-resources"></a>Technische informatiebronnen voor Azure Maps

Hier volgt een lijst met nuttige technische informatiebronnen voor Azure Maps.

- Overzicht: [https://azure.com/maps](https://azure.com/maps)
- Documentatie: [https://aka.ms/AzureMapsDocs](https://aka.ms/AzureMapsDocs)
- Voorbeelden van Web-SDK-code: [https://aka.ms/AzureMapsSamples](https://aka.ms/AzureMapsSamples)
- Ontwikkelaarsforums: [https://aka.ms/AzureMapsForums](https://aka.ms/AzureMapsForums)
- Video's: [https://aka.ms/AzureMapsVideos](https://aka.ms/AzureMapsVideos)
- Blog: [https://aka.ms/AzureMapsBlog](https://aka.ms/AzureMapsBlog)
- Tech Blog: [https://aka.ms/AzureMapsTechBlog](https://aka.ms/AzureMapsTechBlog)
- Azure Maps-feedback (UserVoice): [https://aka.ms/AzureMapsFeedback](https://aka.ms/AzureMapsFeedback)
- [Azure Maps Jupyter Notebook](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook)

## <a name="migration-support"></a>Ondersteuning voor migratie

Ontwikkelaars kunnen migratieondersteuning zoeken via de [forums](https://aka.ms/AzureMapsForums) of via een van de vele opties voor ondersteuning voor Azure: [https://azure.microsoft.com/support/options](https://azure.microsoft.com/support/options)

U leert hoe u uw Google Maps-toepassing kunt migreren met behulp van deze artikelen: 

[Een Android-app migreren](migrate-from-google-maps-android-app.md) 

[Een webservice migreren](migrate-from-google-maps-web-services.md) 

[Een web-app migreren](migrate-from-google-maps-web-app.md)