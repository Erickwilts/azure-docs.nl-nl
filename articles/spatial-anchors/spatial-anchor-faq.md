---
title: Veelgestelde vragen over Azure Spatial Anchors | Microsoft Docs
description: Azure Spatial Anchors is een beheerd cloudservice- en ontwikkelplatform voor mixedreality-ervaringen met meerdere apparaten en meerdere gebruikers op HoloLens-, iOS- en Android-apparaten. Deze veelgestelde vragen over de service worden beantwoord vanuit een technisch oogpunt.
author: ramonarguelles
manager: vicenterivera
services: azure-spatial-anchors
ms.author: ramonarguelles
ms.date: 02/24/2019
ms.topic: overview
ms.service: azure-spatial-anchors
ms.openlocfilehash: d38f7a22d90c62b245e49e6bdb0a3041c4f1023f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60303295"
---
# <a name="frequently-asked-questions-about-azure-spatial-anchors"></a>Veelgestelde vragen over Azure Spatial Anchors

Azure Spatial Anchors is een beheerd cloudservice- en ontwikkelplatform dat mixedreality-ervaringen met ruimtelijk inzicht voor meerdere gebruikers mogelijk maakt op HoloLens-, iOS- en Android-apparaten.

Zie het [overzicht van Azure Spatial Anchors](overview.md) voor meer informatie.

## <a name="azure-spatial-anchors-product-faqs"></a>Veelgestelde vragen over het product Azure Spatial Anchors

**V: Voor welke apparaten biedt Azure Spatial Anchors ondersteuning?**

**A:** Azure Spatial Anchors biedt ontwikkelaars de mogelijkheid om apps te bouwen op HoloLens-apparaten, op iOS-apparaten met ondersteuning voor ARKit en op Android-apparaten met ondersteuning voor ARCore. iOS- en Android-apparaten kunnen ook telefoons en tablets zijn.

**V: Heb ik een verbinding met de cloud nodig om Azure Spatial Anchors te kunnen gebruiken?**

**A:** Voor Azure Spatial Anchors is momenteel een netwerkverbinding met internet vereist. We horen graag uw opmerkingen op onze [feedbacksite](https://feedback.azure.com/forums/919252-azure-spatial-anchors).

**V: Wat zijn de connectiviteitsvereisten voor Azure Spatial Anchors?**

**A:** Azure Spatial Anchors werkt met Wi-Fi- en mobiele breedbandverbindingen.

**V: Hoe nauwkeurig kan Azure Spatial Anchors de locatie van ankers bepalen?**

**A:** De nauwkeurigheid van het zoeken naar ankers is afhankelijk van allerlei factoren, zoals de belichting, de objecten in de omgeving en zelfs het oppervlak waarop het anker is geplaatst. Probeer de ankers uit in een omgeving die representatief is voor de omgeving waarin u ze wilt gebruiken om te bepalen of de nauwkeurigheid voldoet aan uw behoeften. Als u merkt dat de nauwkeurigheid in bepaalde omgevingen te wensen overlaat, raadpleegt u [Logboekregistratie en diagnostische gegevens in Azure Spatial Anchors](./concepts/logging-diagnostics.md).

**V: Hoe lang duurt het maken en zoeken van ankers?**

**A:** De benodigde tijd voor het maken en zoeken van ankers is afhankelijk van veel factoren, zoals de netwerkverbinding, de verwerkingscapaciteit en belasting van het apparaat, en de specifieke omgeving. We hebben klanten die toepassingen bouwen voor allerlei verschillende bedrijfstakken, zoals productie, detailhandel en gaming. Dit geeft wel aan dat de service een goede gebruikerservaring mogelijk maakt voor hun scenario's.

## <a name="privacy-faq"></a>Veelgestelde vragen over privacy

**V: Wanneer mijn toepassing ergens een ruimtelijk anker plaatst, hebben alle apps dan toegang tot dat anker?**

**A:** Ankers zijn geïsoleerd per Azure-account. Alleen apps die u toegang tot uw account verleent, hebben toegang tot ankers binnen het account.

**V: Welke informatie over een omgeving wordt verzonden en opgeslagen in de service bij het gebruik van Azure Spatial Anchors? Worden foto's van de omgeving verzonden en opgeslagen?**

**A**: Bij het maken of zoeken van ankers, worden foto's van de omgeving in een afgeleide indeling op het apparaat verwerkt. Deze afgeleide indeling wordt verzonden naar en opgeslagen in de service.

Om transparantie te bieden, wordt hieronder een afbeelding van een omgeving en de afgeleide sparse puntcloud weergegeven. De puntcloud toont de geometrische weergave van de omgeving die wordt verzonden naar en opgeslagen in de service. Voor elk punt in de sparse puntcloud wordt een hash van de visuele kenmerken van dat punt verzonden en opgeslagen. De hash is afgeleid van pixelgegevens, maar bevat deze gegevens zelf niet.

Azure Spatial Anchors voldoet aan de [voorwaarden van de serviceovereenkomst van Azure](https://go.microsoft.com/fwLink/?LinkID=522330&amp;amp;clcid=0x9) en de [privacyverklaring van Microsoft](https://go.microsoft.com/fwlink/?LinkId=521839&amp;clcid=0x409).

![Een omgeving en de afgeleide sparse puntcloud](./media/sparce-point-cloud.png)
*Afbeelding 1: een omgeving en de afgeleide sparse puntcloud*


**V: Is er een manier waarop ik diagnostische gegevens kan verzenden naar Microsoft?**

**A**: Ja. Azure Spatial Anchors heeft een diagnostische modus die ontwikkelaars kunnen inschakelen via de Azure Spatial Anchors-API. Dit is bijvoorbeeld handig als u een omgeving treft waarin u geen ankers kunt maken en zoeken zoals verwacht. Mogelijk vragen wij u een diagnostisch rapport in te dienen met informatie die ons kan helpen bij het opsporen van fouten. Zie voor meer informatie [Logboekregistratie en diagnostische gegevens in Azure Spatial Anchors](./concepts/logging-diagnostics.md).

## <a name="availability-and-pricing-faqs"></a>Veelgestelde vragen over beschikbaarheid en prijzen

**V: Is er een SLA (Service Level Agreement)?**

**A:** Zoals gebruikelijk voor Azure-services, streven we naar een beschikbaarheid van meer dan 99,9%. Let op: Azure Spatial Anchors is momenteel alleen beschikbaar als preview-versie. Hiervoor gelden de [aanvullende voorwaarden voor preview-versies](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

**V: Kan ik mijn apps met behulp van Azure Spatial Anchors publiceren naar app-stores? Kan ik Azure Spatial Anchors gebruiken voor bedrijfskritische productiescenario's?**

**A:** Azure Spatial Anchors is momenteel alleen beschikbaar als preview-versie en gedurende deze tijd kunt u apps ontwikkelen, [feedback geven](https://feedback.azure.com/forums/919252-azure-spatial-anchors) over het product en uw productie-implementaties plannen.

Algemene beschikbaarheidsdatums worden binnenkort bekendgemaakt.

**V: Zijn er beperkingslimieten?**
 
**A**: Ja, er zijn beperkingslimieten.  We verwachten niet dat u hier tegenaan zult lopen bij het ontwikkelen en testen van toepassingen. We zijn gereed om te voldoen aan de grootschalige behoeften voor productie-implementaties van onze klanten. [Neem contact met ons op](mailto:azuremrs@microsoft.com) om uw wensen te bespreken. Omdat Spatial Anchors nog in de preview-fase is, hebben we nog geen laag- en prijsstructuur gepubliceerd, maar we verwachten dit binnenkort te doen.

**V: In welke regio's is Azure Spatial Anchors beschikbaar?**

**A:** U kunt vandaag nog een Azure Spatial Anchors-account maken in de Azure-regio US - oost 2. Dit betekent dat er zowel rekenkracht als opslagcapaciteit voor deze service zijn in deze regio. Echter, er zijn geen beperkingen aan waar uw klanten zich bevinden. In de toekomst breiden we de beschikbaarheid van de service uit naar alle primaire Azure-regio's.

**V: Worden er kosten in rekening gebracht voor Azure Spatial Anchors? Nu of in de toekomst?**

**A:** Meer informatie over de prijzen tijdens de preview-fase vindt u op onze [pagina met prijzen](https://azure.microsoft.com/pricing/details/spatial-anchors/).

## <a name="technical-faqs"></a>Veelgestelde technische vragen

**V: Hoe werkt Azure Spatial Anchors?**

**A:** Azure Spatial Anchors is afhankelijk van mixed/augmented reality-trackers. Deze trackers nemen de omgeving waar met camera's en tracken het apparaat in 6 vrijheidsgraden (6DoF) terwijl het zich verplaatst door de ruimte.

Met behulp van Azure Spatial Anchors en een gegeven 6DoF-tracker als bouwsteen kunt u bepaalde nuttige plaatsen in uw werkelijke omgeving aanduiden als ankerpunten. U kunt bijvoorbeeld een anker gebruiken om inhoud weer te geven op een specifieke locatie in de echte wereld.

Wanneer u een anker maakt, wordt in de client-SDK informatie over de omgeving rond dat punt vastgelegd en naar de service verzonden. Als een ander apparaat zoekt naar het anker in die dezelfde ruimte, worden soortgelijke gegevens naar de service verzonden. Deze gegevens wordt vergeleken met de eerder opgeslagen omgevingsgegevens. De positie van het anker ten opzichte van het apparaat wordt geretourneerd voor gebruik in de toepassing.

**V: Hoe is Azure Spatial Anchors geïntegreerd met ARKit en ARCore op iOS en Android?**

**A:** Azure Spatial Anchors maakt gebruik van de systeemeigen trackingmogelijkheden van ARKit en ARCore. Onze SDK's voor iOS en Android bieden bovendien mogelijkheden voor het persistent maken van ankers in een beheerde cloudservice zodat uw apps die ankers eenvoudig opnieuw kunnen vinden door verbinding met de service te maken.

**V: Hoe is Azure Spatial Anchors geïntegreerd met HoloLens?**

**A:** Azure Spatial Anchors maakt gebruik van de systeemeigen trackingmogelijkheden van HoloLens. We bieden een Azure Spatial Anchors-SDK voor het bouwen van apps op HoloLens. De SDK is geïntegreerd met de systeemeigen mogelijkheden van HoloLens en biedt extra mogelijkheden. Hiermee kunnen app-ontwikkelaars ankers persistent maken in een beheerde cloudservice zodat uw apps die ankers opnieuw kunnen vinden door verbinding te maken met de service.

**V: Welke platforms en talen worden ondersteund door Azure Spatial Anchors?**

**A:** Ontwikkelaars kunnen apps bouwen met Azure Spatial Anchors met behulp van vertrouwde hulpprogramma's en frameworks voor hun apparaat:

- Unity op HoloLens, iOS en Android
- Swift of Objective-C op iOS
- Java of de Android-NDK op Android
- C++/WinRT op HoloLens

Klik [hier](index.yml) om aan de slag te gaan met ontwikkeling.

**V: Werkt het met Unreal?**

**A:** We verwachten snel ondersteuning voor Unreal te kunnen bieden.

**V: Werkt het met Xamarin?**

**A:** Ja. Hoewel we geen SDK voor Xamarin bieden, verwachten we dat ontwikkelaars Azure Spatial Anchors in hun Xamarin-apps kunnen gebruiken via integratie met de Azure Spatial Anchors-API.
