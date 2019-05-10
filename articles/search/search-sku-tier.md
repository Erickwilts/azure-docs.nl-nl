---
title: Kies een prijscategorie laag of SKU voor Azure Search-service - Azure Search
description: "Azure Search kan worden ingericht op deze SKU's: Gratis, Basic en Standard, waar Standard is beschikbaar in verschillende resourceconfiguraties en capaciteit niveaus."
services: search
author: HeidiSteen
manager: cgronlun
tags: azure-portal
ms.service: search
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 5b9e0dfb88c979618ce4cc5ed56e372cb0f65608
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65472725"
---
# <a name="choose-a-pricing-tier-for-azure-search"></a>Kies een prijscategorie voor Azure Search

In Azure Search, een [resource is gemaakt](search-create-service-portal.md) op een prijzen laag of SKU die voor de levensduur van de service is opgelost. -Laag zijn **gratis**, **Basic**, **Standard**, of **met geoptimaliseerde opslag**.  **Standard** en **met geoptimaliseerde opslag** zijn beschikbaar in verschillende configuraties en capaciteit. 

De meeste klanten beginnen met de **gratis** -laag voor evaluatie en vervolgens omgezet naar een van de hogere betaalde lagen voor ontwikkeling en productie-implementaties. U kunt alle snelstartgidsen en zelfstudies uitvoeren op de **gratis** laag, zoals die voor de resource-intensieve cognitief zoeken.

> [!NOTE]
> De Servicelagen met geoptimaliseerde opslag zijn momenteel beschikbaar als preview op kortingsprijzen voor testen en experimenteren met het doel van het verzamelen van feedback. Laatste prijzen worden later aangekondigd wanneer deze lagen algemeen beschikbaar zijn. We raden u aan die deze lagen gebruiken voor productie-Apps.

Lagen weerspiegelen de eigenschappen van de hardware die als host fungeert voor de service (in plaats functies) en door elkaar worden onderscheiden:

+ Aantal indexen die u kunt maken
+ Grootte en snelheid van de partities (fysieke opslag)

Hoewel alle lagen, inclusief de **gratis** laag, in het algemeen bieden functiepariteit, grotere workloads kunnen dicteren vereisten voor hogere lagen. Bijvoorbeeld, [AI indexeren met cognitieve Services](cognitive-search-concept-intro.md) heeft langlopende vaardigheden die time-out voor een gratis service, tenzij de gegevensset te klein gebeurt.

> [!NOTE] 
> De uitzondering functiepariteit is [indexeerfuncties](search-indexer-overview.md), die zijn niet beschikbaar op S3HD.
>

Binnen een laag, kunt u [replica en partitie resources aanpassen](search-capacity-planning.md) vergroten of verkleinen van de schaal. U kunt beginnen met een of twee van elk en de rekenkracht voor de workload van een intensief indexering tijdelijk verhogen. De mogelijkheid om af te stemmen resource niveaus binnen een laag voegt u flexibiliteit toe, maar ook iets ingewikkelder, uw analyse. Mogelijk hebt om te experimenteren om te zien of een lagere laag met hogere resources/replica's betere waarde en prestaties dan een hogere laag met lagere te biedt. Zie voor meer informatie over wanneer en waarom zou u capaciteit aanpassen, [aandachtspunten voor prestaties en optimalisatie](search-performance-optimization.md).

## <a name="tiers-for-azure-search"></a>Lagen voor Azure Search

De volgende tabel bevat de beschikbare categorieën. Andere bronnen van laaggegevens omvatten de [pagina met prijzen](https://azure.microsoft.com/pricing/details/search/), [service en limieten](search-limits-quotas-capacity.md), en de portal-pagina bij het inrichten van een service.

|Laag | Capaciteit |
|-----|-------------|
|Gratis | Gedeeld met andere abonnees. Niet-schaalbare, beperkt tot 3 indexen en 50 MB opslag. |
|Basis | Toegewezen computerbronnen voor productieworkloads op kleinere schaal. Een partitie van 2 GB en maximaal drie replica's. |
|Standard 1 (S1) | Van S1 op van toegewezen machines met meer capaciteit voor de opslag en verwerking op ieder niveau. Partitiegrootte is 25 GB per partitie (maximaal 300 GB per service) voor S1. |
|Standard 2 (S2) | Lijkt naar S1 uitvoert, maar met 100 GB/partities (max 1,2 TB per service) |
|Standard 3 (S3) | 200 GB per partitie (max 2,4 TB per service) |
|Standard 3 met hoge dichtheid (S3-HD) | High-densitysampling is een *hostmodus* voor S3. De onderliggende hardware is geoptimaliseerd voor een groot aantal kleinere indexen, bedoeld voor scenario's voor multitenancy. S3 HD heeft de dezelfde per eenheid kosten in rekening gebracht zoals S3, maar de hardware is geoptimaliseerd voor het snel bestanden lezen op een groot aantal kleinere indexen.|
|1 (L1) met geoptimaliseerde opslag | 1 TB per partitie (maximaal 12 TB per service) |
|2 (L2) met geoptimaliseerde opslag | 2 TB per partitie (maximaal 24 TB per service) |

> [!NOTE] 
> De lagen met geoptimaliseerde opslag bieden grotere opslagcapaciteit tegen een lagere prijs per TB dan de standaard-laag. Primaire afweging is hogere latentie van query, die u voor uw specifieke toepassingsvereisten controleren moet.  Zie voor meer informatie over Prestatieoverwegingen van deze laag [aandachtspunten voor prestaties en optimalisatie](search-performance-optimization.md).
>

## <a name="how-billing-works"></a>Werking van facturering

Er zijn drie manieren u kosten maken in Azure Search in Azure Search en zijn er vaste en variabele onderdelen. Deze sectie komen op zijn beurt elk onderdeel van de facturering.

### <a name="1-core-service-costs-fixed-and-variable"></a>1. Servicekosten voor Core (vaste en variabele)

De minimale kosten is de eerste zoekeenheid (1 replica x 1 partitie) voor de service zelf, en dit bedrag wordt vastgesteld voor de levensduur van de service omdat de service op iets minder dan deze configuratie kan niet worden uitgevoerd. 

Meer dan het minimum, kunt u toevoegen replica's en partities onafhankelijk van elkaar. U kunt bijvoorbeeld alleen replica's of alleen de partities toevoegen. Incrementele verhogingen in capaciteit via de replica's en partities die deel uitmaken van de variabele kosten-component. 

Facturering is gebaseerd op een [formule (replica's x partities x-tarief)](#search-units). Het tarief is afhankelijk van de prijscategorie die u selecteert.

In de volgende schermafbeelding per eenheid prijzen wordt aangegeven voor gratis, basis en S1 (S2, S3, L1 en L2 worden niet weergegeven). Als u hebt gemaakt een **Basic**, **Standard**, of **met geoptimaliseerde opslag** -service, uw maandelijkse kosten zou de waarde die wordt weergegeven voor de gemiddelde *prijs-1*en *prijs 2* respectievelijk. Kosten per eenheid terecht voor elke laag omdat de rekenkundige power- en opslagcapaciteit op elke opeenvolgende laag valt. De tarieven voor Azure Search worden gepubliceerd op de [Azure Search-pagina met prijzen](https://azure.microsoft.com/pricing/details/search/).

![Per eenheid prijzen](./media/search-sku-tier/per-unit-pricing.png "Per eenheid prijzen")

Wanneer kosten van een zoekoplossing, ziet u de prijzen en -capaciteit niet lineair zijn (verdubbeling van capaciteit van meer dan verdubbeld de kosten). Zie voor een voorbeeld van hoe u van de formule werkt ["Het toewijzen van replica's en partities"](search-capacity-planning.md#how-to-allocate-replicas-and-partitions).


### <a name="2-data-egress-charges-during-indexing"></a>2. Kosten voor uitgaande gegevens tijdens het indexeren

Het gebruik van [Azure Search-indexeerfuncties](search-indexer-overview.md) kan leiden tot facturering impact, afhankelijk van waar de services die zich bevinden. Volledig als u de Azure Search-service in dezelfde regio als uw gegevens maakt, kunt u kosten voor uitgaande gegevens voorkomen. De volgende punten zijn van de [bandbreedte pagina met prijzen](https://azure.microsoft.com/pricing/details/bandwidth/).

+ Microsoft niet in rekening voor alle binnenkomende gegevens voor elke service op Azure, of voor alle uitgaande gegevens van Azure Search.

+ In oplossingen voor meerdere services zijn er geen kosten voor gegevens die via de kabel wanneer alle services zich in dezelfde regio bevinden.

In rekening gebracht voor uitgaande gegevens als services zich in verschillende regio's. Deze kosten zijn per se geen onderdeel van uw factuur voor Azure Search, maar ze hier worden genoemd omdat als u van gegevens of AI verrijkt indexeerfuncties gegevens wilt halen uit verschillende regio's gebruikmaakt, u de kosten die in uw algehele factuur weergegeven ziet. 

### <a name="3-ai-enriched-indexing-using-cognitive-services"></a>3. AI-verrijkt indexeren met cognitieve Services

Voor [AI indexeren met cognitieve Services](cognitive-search-concept-intro.md), moet u van plan bent voor het koppelen van een factureerbare Cognitive Services-resource, in dezelfde regio als de Azure Search, op de S0-prijscategorie voor de verwerking van betalen per gebruik. Er is geen 'vaste prijs' die is gekoppeld aan het koppelen van Cognitive Services. U betaalt alleen voor de verwerking die u nodig hebt.

Afbeelding extractie tijdens documenten kraken is een Azure Search-kosten in rekening gebracht op basis van het aantal afbeeldingen uit uw documenten hebt uitgepakt. Extractie van tekst is momenteel gratis. 

Andere enrichments, zoals de verwerking van natuurlijke taal, zijn gebaseerd op [ingebouwde cognitieve vaardigheden](cognitive-search-predefined-skills.md) worden in rekening gebracht op basis van een resource voor Cognitive Services, hetzelfde tarief als u de taak met Cognitive Services rechtstreeks heeft uitgevoerd. Zie voor meer informatie, [koppelen van een Cognitive Services-resource met een set vaardigheden](cognitive-search-attach-cognitive-services.md).

<a name="search-units"></a>

### <a name="billing-based-on-search-units"></a>Facturering op basis van de search-eenheden

Voor Azure Search-bewerkingen, de belangrijkste facturering concept om te begrijpen is een *zoekeenheid* (SU). Omdat Azure Search, hangt af van de replica's en partities om te indexeren en query's, verstandig niet het om aan te brengen door slechts in één van de andere kosten in rekening. In plaats daarvan is facturering gebaseerd op een samenstelling van beide. 

SU is het product van *replica* en *partities* die worden gebruikt door een service: **`(R X P = SU)`**

Elke service wordt gestart met één SU (één replica wordt vermenigvuldigd met één partitie) als het minimum. De maximale waarde voor elke service is 36 su's die op verschillende manieren kunnen worden bereikt: 6 partities x 6 replica's, of 3 partities x 12 replica's, een paar te noemen. Het is gebruikelijk is minder dan de totale capaciteit gebruiken. Bijvoorbeeld, een 3-replica, 3-partitie service, kosten in rekening gebracht als 9 su's. U kunt bekijken [in deze grafiek](search-capacity-planning.md#chart) geldige combinaties in één oogopslag zien.

Het tarief is **per uur per SU**, waarbij elke laag met een steeds hogere snelheid. Hogere lagen geleverd met grotere en sneller partities, die bijdragen aan een totale hogere uurtarief voor die laag. Tarieven voor elke laag kan worden gevonden op [prijsinformatie](https://azure.microsoft.com/pricing/details/search/). 

De meeste klanten Breng slechts een deel van de totale capaciteit online, met de rest in reserveren. Wat betreft facturering is het aantal partities en replica's die u levert online, berekend met behulp van de formule SU, waarmee wordt bepaald wat u daadwerkelijk betalen op uurbasis.

### <a name="billing-for-image-extraction-in-cognitive-search"></a>Facturering voor het ophalen van de afbeelding in cognitief zoeken

Als u installatiekopieën van de bestanden in een cognitief zoeken pijplijn indexeren uitpakken wilt, in rekening gebracht voor de bewerking op uw factuur voor Azure Search. De parameter die wordt geactiveerd extractie van de installatiekopie is **imageAction** in een [configuratie van de indexeerfunctie](https://docs.microsoft.com/rest/api/searchservice/create-indexer#indexer-parameters). Als **imageAction** is ingesteld op none (standaard), er zijn geen kosten voor het ophalen van de installatiekopie.

Prijs is onderhevig aan wijzigingen, maar wordt altijd beschreven op de [prijsinformatie](https://azure.microsoft.com/pricing/details/search/) pagina voor Azure Search. 

### <a name="billing-for-built-in-skills-in-cognitive-search"></a>Facturering voor ingebouwde vaardigheden in cognitief zoeken

Bij het instellen van een pijplijn verrijking eventuele [ingebouwde vaardigheden](cognitive-search-predefined-skills.md) gebruikt in de pijplijn zijn gebaseerd op machine learning-modellen. Deze modellen worden geleverd door de Cognitive Services. Gebruik van deze modellen tijdens het indexeren wordt hetzelfde tarief gefactureerd als wanneer u de resource direct heeft aangevraagd.

Stel bijvoorbeeld een pijplijn die bestaat uit een optische tekenherkenning (OCR) voor de gescande afbeelding JPEG-bestanden, waar de resulterende tekst in een Azure Search-index wordt gepusht voor vrije zoekquery's. Uw pijplijn voor indexering omvat een indexeerfunctie met de [OCR-kwalificatie](cognitive-search-skill-ocr.md), en dat de kwalificatie zou zijn [die is gekoppeld aan een resource voor Cognitive Services](cognitive-search-attach-cognitive-services.md). Wanneer u de indexeerfunctie uitvoert, worden kosten in rekening gebracht op uw factuur cognitieve Resources voor de uitvoering van OCR weergegeven.

## <a name="tips-for-reducing-costs"></a>Tips voor het verminderen van kosten

U kunt de service op de factuur verlagen niet afsluiten. Toegewezen resources zijn operationele 24-7, voor exclusief gebruik gedurende de levensduur van uw service is toegewezen. De enige manier om een factuur verlagen door te verminderen van replica's en partities op een laag niveau waarmee u nog steeds acceptabel is en [SLA-naleving](https://azure.microsoft.com/support/legal/sla/search/v1_0/). 

Een hendel voor het verminderen van kosten is een laag met een lagere uurtarief kiezen. S1 uurtarieven zijn lager dan S2 of S3-tarieven. Ervan uitgaande dat u een service die is gericht op de laagste waarde van de load-projecties, als u de service langzamerhand inricht, kan u een tweede grotere lagen service maken, uw indexen op de tweede service opnieuw en verwijder vervolgens het eerste item. 

Als u hebt plannen van capaciteit voor gedaan op de lokale servers, weet u dat het gebruikelijk is dat 'kopen van' zodat u de verwachte groei kunt verwerken. Maar met een cloudservice, u kunt een meer kosten te besparen doelbewust omdat u niet worden gekoppeld aan een specifieke aankoop. U kunt altijd overschakelen naar een hogere lagen service als het huidige abonnement ontoereikend is.

### <a name="capacity-drill-down"></a>Inzoom capaciteit

In Azure Search capaciteit is gestructureerd als *replica's* en *partities*. 

+ Replica's zijn exemplaren van de search service, waarbij elke replica als host fungeert voor een gelijke kopie van een index. Een service met 6 replica's heeft bijvoorbeeld 6 kopieën van elke index geladen in de service. 

+ Partities indexen opslaan en doorzoekbare gegevens automatisch splitsen: twee partities uw index in de helft, drie partities splitsen in derde, enzovoort. Wat betreft de capaciteit, *partitie grootte* is van de primaire functie van de onderscheidende voor lagen.

> [!NOTE]
> Alle **Standard** en **met geoptimaliseerde opslag** lagen ondersteuning [flexibele combinaties replica en partities](search-capacity-planning.md#chart) zodat u kunt [gewicht van uw systeem voor snelheid of opslag](search-performance-optimization.md) door het veranderen van de balans. **Basic** biedt u drie replica's voor hoge beschikbaarheid, maar slechts één partitie heeft. **Gratis** lagen bieden geen toegewezen resources: computing resources worden gedeeld door meerdere abonnees.

### <a name="more-about-service-limits"></a>Meer informatie over Servicelimieten

Hostresources, zoals indexen en indexeerfuncties-Services. Elke laag legt [Servicelimieten](search-limits-quotas-capacity.md) op de hoeveelheid resources die u kunt maken. Hierdoor is een bovengrens voor het aantal indexen (en andere objecten) de tweede onderscheidende functie voor lagen. Houd er rekening mee de beperkingen voor aantal indexen tijdens het bekijken van elke optie in de portal. Andere resources, zoals Indexeerfuncties en gegevensbronnen kennis en vaardigheden, wordt intensief gebruikt met index limieten.

## <a name="consumption-patterns"></a>Gebruikspatronen voor databasebronnen

De meeste klanten beginnen met de **gratis** service, die ze voor onbepaalde tijd houden, en kies vervolgens een van de **Standard** of **met geoptimaliseerde opslag** lagen voor het ontwikkelen van ernstige of werkbelastingen voor productie. 

![Azure search-lagen](./media/search-sku-tier/tiers.png "Azure search Prijscategorieën")

Op beide uiteinden **Basic** en **S3 HD** bestaan voor belangrijke, maar ongewone verbruik patronen. **Basic** is bedoeld voor kleine productieworkloads: SLA, biedt bronnen, hoge beschikbaarheid, maar matig opslag, toegenomen van 2 GB totaal toegewezen. Deze laag is ontworpen voor klanten die consistent onder gebruikte beschikbare capaciteit. Aan het einde, **S3 HD** is voor werkbelastingen karakteristiek voor ISV's, partners, [multitenant oplossingen](search-modeling-multitenant-saas-applications.md), of geen configuratie voor een groot aantal kleine indexen. Het is vaak voor de hand liggende wanneer **Basic** of **S3 HD** laag is geschikt, maar als u wilt bevestigen dat u kunt boeken [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) of [Neem contact op met Azure Ondersteuning voor](https://azure.microsoft.com/support/options/) voor verdere richtlijnen.

Focus verplaatsen naar de meest gangbare standaardlagen, **S1-S3** voortgang met het toenemende aantal niveaus van capaciteit, met belangrijk punten op de grootte van partitie- en maximumwaarden voor aantallen indexen, Indexeerfuncties en corollary resources zijn:

|  | S1 | S2 | S3 |  |  |  |  |
|--|----|----|----|--|--|--|--|
| Partitiegrootte|  25 GB | 100 GB | 200 GB |  |  |  |  |
| limieten voor index en indexeerfunctie| 50 | 200 | 200 |  |  |  |  |

**S1** is een algemene keuze wanneer toegewezen resources en meerdere partities noodzakelijk worden. Met partities van 25 GB voor maximaal 12 partities, de per-service beperken op **S1** is 300 GB totaal als u partities via de replica's maximaliseren (Zie [partities en replica's toewijzen](search-capacity-planning.md#chart) voor meer met gelijke taakverdeling samenstelling.)

Portal- en prijsmodel pagina's plaatsen de focus op de grootte van partitie en opslag, maar voor elke laag alle mogelijkheden (schijfcapaciteit, de snelheid van CPU's) compute lineair groeien met de prijs. Een **S2** replica is sneller dan **S1**, en **S3** sneller dan **S2**. **S3** lagen opsplitsen in het algemeen lineaire compute-prijzen-patroon met onevenredig sneller i/o's. Als u verwacht de i/o knelpunt dat, een **S3** biedt u veel meer IOPS dan lagere niveaus.

**S3** en **S3 HD** worden ondersteund door identieke hoge capaciteit infrastructuur, maar elk een de maximumlimiet is bereikt op verschillende manieren. **S3** is gericht op een kleiner aantal zeer grote indexen. Hierdoor de maximumlimiet is afhankelijk van de resource (2,4 TB voor elke service). **S3 HD** is gericht op een groot aantal kleine indexen. 1000 indexen **S3 HD** uiterste te belasten in de vorm van de indexbeperkingen is bereikt. Als u een **S3 HD** klant die is vereist meer dan 1000 indexen, neem contact op met Microsoft Support voor meer informatie over om door te gaan.

> [!NOTE]
> Voorheen, document limieten overweging zijn, maar zijn niet langer van toepassing op nieuwe services. Zie voor meer informatie over de voorwaarden waaronder document limieten nog steeds van toepassing, [Servicelimieten: document limieten](search-limits-quotas-capacity.md#document-limits).
>

Opslaglagen geoptimaliseerd, **L1 L2**, zijn ideaal voor toepassingen met vereisten voor grote hoeveelheden gegevens, maar een relatief klein aantal eindgebruikers waar Minimalisatie van het aantal Querylatentie niet de hoogste prioriteit is.  

|  | L1 | L2 |  |  |  |  |  |
|--|----|----|--|--|--|--|--|
| Partitiegrootte|  1 TB | 2 TB |  |  |  |  |  |
| limieten voor index en indexeerfunctie| 10 | 10 |  |  |  |  |  |

*L2* biedt tweemaal de totale opslagcapaciteit aan een *L1*.  De laag op basis van de maximale hoeveelheid gegevens die u vindt dat de index moet kiezen.  De *L1* laag schaal van partities in stappen van 1 TB tot een maximum van 12 TB, terwijl de *L2* verhogen door 2 TB per partitie tot een maximum van 24 TB.

## <a name="evaluate-capacity"></a>Capaciteit evalueren

Capaciteit en kosten van het uitvoeren van de service gaan hand in hand. Lagen leggen beperkingen met betrekking tot twee niveaus (opslag en bronnen), dus u met beide doen moet omdat het een eerst is bereikt de effectieve limiet is. 

Bedrijfsvereisten voorschrijven doorgaans het aantal indexen, u moet. Bijvoorbeeld, globale index voor een grote opslagruimte voor documenten, of misschien meerdere indexen op basis van regio, toepassing of nichemarkten business.

Om te bepalen van de grootte van een index, die u hebt tot [bouwen een](search-create-index-portal.md). De gegevensstructuur van de in Azure Search is voornamelijk een [omgekeerde index](https://en.wikipedia.org/wiki/Inverted_index), heeft andere eigenschappen dan de brongegevens. Grootte en complexiteit voor een omgekeerde index worden bepaald door de inhoud, niet per se de hoeveelheid gegevens die u in het kanaal. Een bron van grote hoeveelheden gegevens met zeer grote redundantie kan leiden tot een kleinere-index dan een kleinere gegevensset die zeer variabel inhoud bevat. Het is daarom zelden mogelijk afleiden van de indexgrootte van de is gebaseerd op de grootte van de oorspronkelijke gegevensset.

> [!NOTE] 
> Hoewel het schatten van de toekomstige behoeften voor indexen en opslag kunt lijkt giswerk, is het waard is om dit. Als de capaciteit van een laag blijkt te laag zijn, moet u voor het inrichten van een nieuwe service van het hogere niveau en vervolgens [uw indexen opnieuw laden](search-howto-reindex.md). Er is geen in-place upgrade van de dezelfde service van een SKU naar een andere.
>

### <a name="step-1-develop-rough-estimates-using-the-free-tier"></a>Stap 1: Maakt een ruwe schatting met behulp van de gratis laag ontwikkelen

Een mogelijke aanpak voor het schatten van de capaciteit is te beginnen met de **gratis** laag. Intrekken die de **gratis** service biedt maximaal 3 indexen, 50 MB opslag, en 2 minuten tijd worden geïndexeerd. Het kan lastig zijn om in te schatten van de indexgrootte van een verwachte met deze beperkingen, maar het volgende voorbeeld wordt een benadering:

+ [Maak een gratis service](search-create-service-portal.md)
+ Een kleine, representatieve gegevensset voorbereiden (uitgaan van 5000 documenten en de grootte van de steekproef tien procent)
+ [Bouw een eerste index](search-create-index-portal.md) en noteer de grootte in de portal (wordt ervan uitgegaan dat 30 MB)

Ervan uitgaande dat het voorbeeld is zowel vertegenwoordiger en tien procent van de gehele gegevensbron, wordt een index 30 MB ongeveer 300 MB als alle documenten die zijn geïndexeerd. Met dit voorlopige nummer hebt, u mogelijk dubbele bedrag voor het budget voor twee indexen (ontwikkeling en productie), voor een totaal van 600 MB in de opslagvereisten voor. Dit gemakkelijk door wordt voldaan aan de **Basic** tier, zodat u er rekening gebracht.

### <a name="step-2-develop-refined-estimates-using-a-billable-tier"></a>Stap 2: Ontwikkelen van verfijnde schattingen met behulp van een factureerbare laag

Sommige klanten de voorkeur geeft aan om te beginnen met toegewezen resources die kunnen grotere steekproeven en verwerkingstijden en realistische maakt een schatting van de index aantal, de grootte en de query volumes tijdens het ontwikkelen van ontwikkelen. In eerste instantie een service is ingericht op basis van een schatting inschatting en vervolgens als het project voor de ontwikkeling zich verder ontwikkelt, teams meestal weten of de bestaande service boven of onder de capaciteit voor geplande productieworkloads. 

1. [Bekijk Servicelimieten op elke laag](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#index-limits) om te bepalen of het aantal indexen, u moet kunnen ondersteuning voor lagere niveaus. In de **Basic**-**S1**-**S2** index limieten voor de lagen zijn 15-50-200, respectievelijk.  De **met geoptimaliseerde opslag** laag geldt een limiet van 10 indexen omdat het designer voor de ondersteuning van een klein aantal grote indexen.

1. [Een service maken op een factureerbare laag](search-create-service-portal.md):

    + Start op laag **Basic** of **S1** als u aan het begin van de leercurve.
    + Start op hoog **S2** of zelfs **S3**, als voor de hand liggende grootschalige indexeren en query-belastingen.
    + Opslag geoptimaliseerd, op **L1** of **L2**, als u een grote hoeveelheid gegevens indexeert en querybelasting relatief klein, zoals een interne bedrijfstoepassing is.

1. [Bouw een eerste index](search-create-index-portal.md) om te bepalen hoe gegevens worden omgezet naar een index. Dit is de enige manier om te schatten van de indexgrootte van de.

1. [Opslag, Servicelimieten, queryvolume en latentie controleren](search-monitor-usage.md) in de portal. In de portal ziet u query's per seconde, beperkte query's en zoeklatentie; die allemaal kunt u bepalen als u de juiste laag hebt geselecteerd. Afgezien van portal metrische gegevens, kunt u configureren diepe bewaking, zoals clickthrough-analyses door in te schakelen [zoekverkeer](search-traffic-analytics.md). 

Index aantal en de grootte zijn net zo relevant voor uw analyse, omdat de maximale limieten zijn bereikt via volledig benut opslag (partities) of door de maximale limieten voor resources (indexen, indexeerfuncties, enzovoort), afhankelijk van wat het eerste komt. De portal kunt u bijhouden van beide, van huidige gebruiks- en maximumlimieten naast elkaar op de pagina overzicht.

> [!NOTE]
> Vereisten voor opslag kunnen worden te veel hoge als documenten overbodige gegevens bevatten. In het ideale geval bevatten documenten alleen gegevens die u nodig hebt voor de zoekfunctie. Binaire gegevens is niet doorzoekbaar en moet worden opgeslagen afzonderlijk (bijvoorbeeld in een Azure-tabel of blob-opslag) met een veld in de index voor het opslaan van een URL-verwijzing naar de externe gegevens. De maximale grootte van een afzonderlijke document is 16 MB (of minder als u bulksgewijs uploaden van meerdere documenten in één aanvraag zijn). [Servicelimieten in Azure Search](search-limits-quotas-capacity.md) bevat meer informatie.
>

**Overwegingen voor het volume van query**

Query's-per-seconde (QPS) is een meetwaarde die verf laat komen krijgt bij het afstemmen van prestaties, maar is doorgaans niet aanmerking laag, tenzij u verwacht hoge queryvolume in het begin dat.

De standaard-laag kunnen u een balans tussen de replica's leveren naar partities, sneller query kunt worden geholpen door extra replica's ondersteunen voor het laden van netwerktaakverdeling en extra partities voor parallelle verwerking. U kunt afstemmen van prestaties nadat de service is ingericht.

Klanten die verwachten sterke langdurige query volumes vanaf het begin dat moeten rekening houden met hogere **Standard** lagen, ondersteund door krachtigere hardware. U kunt vervolgens partities en replica's offline te halen, of zelfs overschakelen naar een lagere laag-service als de query-volumes niet om te realiseren. Zie voor meer informatie over het berekenen van de query-doorvoer [Azure Search-prestaties en optimalisatie](search-performance-optimization.md).

De opslag geoptimaliseerd voor workloads met veel gegevens, ondersteuning van meer algemene index opslagruimte beschikbaar is, waar query latentievereisten enigszins soepele zijn lean lagen.  Extra replica's moeten nog steeds worden gebruikt voor het laden van netwerktaakverdeling en extra partities voor parallelle verwerking. U kunt afstemmen van prestaties nadat de service is ingericht.

**Serviceovereenkomsten**

De **gratis** laag en preview-functies worden niet geleverd met [service level agreements (Sla's)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). Voor alle factureerbare lagen, sla's van kracht wanneer u voldoende redundantie voor uw service inricht. Twee of meer replica's zijn vereist voor de SLA van de query (lezen). Drie of meer replica's zijn vereist voor query's en indexering SLA (lezen-schrijven). Het aantal partities is niet een aandachtspunt SLA. 

## <a name="tips-for-tier-evaluation"></a>Tips voor evaluatie van de laag

+ Meer informatie over het bouwen van efficiënte indexen en welke methoden vernieuwing zijn de minimale impactful. Het is raadzaam [zoekverkeer](search-traffic-analytics.md) voor de inzichten die op query-activiteiten.

+ Metrische gegevens om te bouwen om query's en verzamelen van gegevens over gebruikspatronen (query's tijdens kantooruren, indexering tijdens daluren) toestaan, en deze gegevens gebruiken om te informeren over toekomstige beslissingen voor de provisioning-service. Tijdens het niet praktisch op een per uur of dagelijks uitgebracht, kunt u partities en resources voor geplande wijzigingen in query-volumes, of niet-geplande maar langdurige wijzigingen als niveaus lang genoeg is bevatten voor het ondernemen van actie garandeert dynamisch aanpassen.

+ Houd er rekening mee dat het enige nadeel van onder inrichting is dat u wellicht te moeten verbreken van een service als de daadwerkelijke vereisten groter dan u geschat zijn. Als u wilt voorkomen dat de service wordt onderbroken, zou u een nieuwe service maken in hetzelfde abonnement op een hogere laag en deze naast elkaar uitvoeren totdat alle apps en aanvragen gericht op het nieuwe eindpunt.

## <a name="next-steps"></a>Volgende stappen

Beginnen met een **gratis** -laag en het bouwen van een eerste index met behulp van een subset van uw gegevens om te begrijpen van de kenmerken ervan. De gegevensstructuur in Azure Search is een omgekeerde index, waarbij de grootte en complexiteit van een omgekeerde index wordt bepaald door de inhoud. Houd er rekening mee dat maximaal redundante inhoud leidt doorgaans tot een kleinere-index dan maximaal onregelmatige inhoud. Het is daarom inhoud kenmerken in plaats van de grootte van de gegevensset waarmee wordt bepaald index opslagvereisten.

Zodra u een eerste idee van de indexgrootte hebt, [een factureerbare service inrichten](search-create-service-portal.md) op een van de lagen die in dit artikel worden besproken, ofwel **Basic**, **Standard**, of **Met geoptimaliseerde opslag** laag. Een kunstmatig opgelegde beperkingen gelden voor het formaat van de gegevens te versoepelen en [opnieuw opbouwen van uw index](search-howto-reindex.md) om op te nemen alle gegevens die u daadwerkelijk wilt worden doorzocht.

[Partities en replica's toewijzen](search-capacity-planning.md) indien nodig om op te halen van de prestaties en schaal die u nodig hebt.

Als prestaties en capaciteit fijn, bent u klaar. Anders, een search-service op een andere laag die meer nauw aansluit bij de behoeften van uw opnieuw maken.

> [!NOTE]
> Voor meer hulp bij uw vragen naar boeken [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) of [Neem contact op met ondersteuning voor Azure](https://azure.microsoft.com/support/options/).
