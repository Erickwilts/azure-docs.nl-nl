---
title: Meer informatie over uw Azure Cosmos DB factuur
description: In dit artikel wordt uitgelegd hoe u uw Azure Cosmos DB factuur begrijpt met enkele voor beelden.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: f800560aa4ddef52c2274fa8b3f5c98d68189f0f
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68717525"
---
# <a name="understand-your-azure-cosmos-db-bill"></a>Meer informatie over uw Azure Cosmos DB factuur

Als volledig beheerde Cloud-systeem eigen database service, Azure Cosmos DB vereenvoudigt de facturering door alleen voor ingerichte door Voer en verbruikte opslag. Er zijn geen aanvullende licentie kosten, hardware, kosten voor hulp programma of faciliteiten in vergelijking met on-premises of door IaaS gehoste alternatieven. Wanneer u rekening houdt met de mogelijkheden van meerdere regio's van Azure Cosmos DB, biedt de database service een aanzienlijke verlaging van de kosten in vergelijking met bestaande on-premises of IaaSe oplossingen.

Met Azure Cosmos DB wordt per uur gefactureerd op basis van de ingerichte door Voer en de verbruikte opslag. Voor de ingerichte door Voer is de facturerings eenheid 100 RU/sec per uur, in rekening gebracht tegen $0,008 per uur, uitgaande van de standaard open bare prijzen, zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/). Voor de verbruikte opslag wordt gefactureerd van $0,25 per 1 GB opslag per maand. Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/)voor meer informatie. 

In dit artikel worden enkele voor beelden gebruikt om inzicht te krijgen in de details die u op de maandelijkse factuur ziet. De getallen die in de voor beelden worden weer gegeven, kunnen verschillen als uw Azure Cosmos-containers een andere hoeveelheid door Voer inrichten hebben, als deze over meerdere regio's vallen of gedurende een maand een andere periode hebben.

>! Eraan Facturering is voor elk deel van een uur in de periode, geen 60 minuut.

## <a name="billing-examples"></a>Voorbeelden van facturering

### <a name="billing-example---throughput-on-a-container-full-month"></a>Voor beeld van facturering: door Voer voor een container (volledige maand)

* We gaan ervan uit dat u een door Voer van 1.000 RU/SEC op een container configureert en er 24 uur * 30 dagen voor de maand = 720 uur totaal bestaat.  

* 1\.000 RU/SEC 10 eenheden van 100 RU/sec per uur voor elk uur dat de containers bestaan (1000/100 = 10). 

* 10 eenheden per uur vermenigvuldigen met de kosten van $0,008 (per 100 RU/sec per uur) = $0,08 per uur. 

* Vermenigvuldiging van $0,08 per uur met het aantal uren in de maand is gelijk aan $0,08 * 24 uur * 30 dagen = $57,60 voor de maand.  

* De totale maandelijkse factuur toont 7.200 eenheden (van 100 RUs), wat de kosten $57,60 kost.

### <a name="billing-example---throughput-on-a-container-partial-month"></a>Voor beeld van facturering: door Voer voor een container (gedeeltelijke maand)

* We gaan ervan uit dat we een container maken met een ingerichte door Voer van 2.500 RU/sec. De container bevindt zich 24 uur per maand (het is bijvoorbeeld 24 uur nadat u deze hebt gemaakt).  

* Vervolgens zien we 600 eenheden op de factuur (2.500 RU/SEC/100 RU/SEC/eenheid * 24 uur). De kosten zijn $4,80 (600 eenheden * $0.008/Unit).

* De totale factuur voor de maand is $4,80.

### <a name="billing-rate-if-storage-size-changes"></a>Facturerings tarief als de opslag grootte verandert

Opslagcapaciteit wordt in rekening gebracht in eenheden van de maximale hoeveelheid opgeslagen gegevens per uur (in GB) gedurende een maand. Als u bijvoorbeeld 100 GB aan opslag hebt gebruikt tijdens de eerste helft van de maand en 50 GB in de tweede helft, krijgt u een factuur voor het equivalent van 75 GB aan opslag gedurende die maand.

### <a name="billing-rate-when-container-or-a-set-of-containers-are-active-for-less-than-an-hour"></a>Het facturerings tarief wanneer een container of een set containers minder dan een uur actief is

Er wordt een vast bedrag in rekening gebracht voor elk uur dat de container of data base bestaat, ongeacht het gebruik of als de container of data base minder dan een uur actief is. Als u bijvoorbeeld een container of Data Base maakt en deze vijf minuten later verwijdert, zal uw factuur één uur bevatten.

### <a name="billing-rate-when-throughput-on-a-container-or-database-scales-updown"></a>Facturerings tarief wanneer de door Voer voor een container of data base omhoog/omlaag wordt geschaald

Als u de ingerichte door Voer op 9:30 uur verhoogt van 400 RU/sec naar 1.000 RU/SEC en vervolgens een lagere ingerichte door Voer op 10:45 A.M. terugbrengt naar 400 RU/sec., worden er twee uur aan 1.000 RU/sec in rekening gebracht. 

Als u de ingerichte door Voer voor een container of een set containers op 9:30 uur verhoogt van 100-K RU/sec tot 200-K RU/sec. en vervolgens de ingerichte door Voer op 10:45 uur terug naar 100-K RU/sec., worden er twee uur aan 200 K RU/sec in rekening gebracht.

### <a name="billing-example-multiple-containers-each-with-dedicated-provisioned-throughput-mode"></a>Voor beeld van facturering: meerdere containers, elk met een speciale ingerichte doorvoer modus

* Als u een Azure Cosmos-account in VS-Oost 2 maakt met twee containers met ingerichte door Voer van 500 RU/SEC en 700 RU per seconde, hebt u een totale ingerichte door Voer van 1.200 RU/sec.  

* Er worden 1200/100 * $0,008 = $0.096/uur in rekening gebracht. 

* Als uw door Voer is gewijzigd en u de capaciteit van elke container hebt verhoogd met 500 RU/sec, terwijl u ook een nieuwe onbeperkte container maakt met 20.000 RU/sec., is uw totale ingerichte capaciteit 22.200 RU/SEC (1.000 RU/sec. + 1.200 RU/sec. + 20, verbruik/seconde).  

* Uw factuur wordt dan gewijzigd in: $0,008 x 222 = $1.776/uur. 

* Als er in een maand van 720 uur (24 uur * 30 dagen) een ingerichte door Voer van 500 uur 1.200 RU/SEC is en voor de resterende 220 uur door Voer is 22.200 RU/sec, wordt in uw maandelijkse factuur de volgende gegevens weer gegeven: 500 x $0.096/uur + 220 x $1.776/Hour = $438.72/maand.

![Voor beeld van toegewezen doorvoer facturering](./media/understand-your-bill/bill-example1.png)

### <a name="billing-example-containers-with-shared-throughput-mode"></a>Facturerings voorbeeld: containers met gedeelde doorvoer modus

* Als u een Azure Cosmos-account in VS-Oost 2 maakt met twee Azure Cosmos-data bases (met een set containers die de door Voer op database niveau delen) met de ingerichte door Voer van 50-K RU/SEC en 70-K RU/sec, hebt u een totaal ingericht door Voer van 120 K RU/sec.  

* Er worden 1200 x $0,008 = $9.60/uur in rekening gebracht. 

* Als uw door Voer is gewijzigd en u de ingerichte door Voer van elke Data Base hebt verhoogd met 10.000 RU/SEC voor elke Data Base, en u een nieuwe container toevoegt aan de eerste data base met de toegewezen doorvoer modus van 15 K RU/seconde naar uw gedeelde doorvoer database, wordt uw de totale ingerichte capaciteit is 155-K RU/sec. (60 K RU/sec. + 80 K RU/SEC + 15 K RU/sec.).  

* Uw factuur zou dan worden: 1.550 * $0,008 = $12.40/uur.  

* Als er in een maand van 720 uur een ingerichte door Voer van 300 uur 120-K RU/SEC is en voor de resterende 420 uur door Voer is 155-K RU/sec. uw maandelijkse factuur wordt weer gegeven: 300 x $9.60/uur + 420 x $12.40/Hour = $2.880 + $5.208 = $8088/maand. 

![Voor beeld van een facturering van gedeelde door Voer](./media/understand-your-bill/bill-example2.png)

## <a name="billing-examples-with-geo-replication-and-multi-master"></a>Facturerings voorbeelden met geo-replicatie en multi-master  

U kunt op elk gewenst moment Azure-regio's aan uw Azure Cosmos DB-database account toevoegen of eruit verwijderen. De door u geconfigureerde door Voer voor diverse Azure Cosmos DB-data bases en containers wordt gereserveerd in elk van de Azure-regio's die zijn gekoppeld aan uw Azure Cosmos-database account. Als de som van de ingerichte door Voer (RU/sec) die is geconfigureerd voor alle data bases en containers in uw Azure Cosmos-database account (ingericht per uur), T is en het aantal Azure-regio's dat aan uw database account is gekoppeld, N is, wordt het totaal de ingerichte door Voer voor een bepaald uur, voor uw Azure Cosmos-database account, (a) die is geconfigureerd met één schrijf regio, is gelijk aan T x N RU/sec. en (b) geconfigureerd met alle regio's die kunnen worden verwerkt, is gelijk aan T x (N + 1) RU/sec. respectievelijk. Ingerichte door Voer (enkele schrijf regio) kosten $0.008/uur per 100 RU/SEC en ingerichte door Voer met meerdere Beschrijf bare regio's (configuratie van meerdere masters) $0,016/per uur per 100 RU/sec. (Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/)). Met Azure Cosmos DB kunt u de gegevens uit een wille keurige regio lezen, ongeacht of er een enkele schrijf regio of meerdere schrijf regio's zijn.

### <a name="billing-example-multi-region-azure-cosmos-account-single-region-writes"></a>Voor beeld van facturering: meerdere regio's Azure Cosmos-account, schrijf bewerkingen in één regio

We gaan ervan uit dat u een Azure Cosmos-container hebt in VS West. De container wordt gemaakt met een door Voer van 10.000 RU/SEC en u hebt 1 TB aan gegevens per maand opgeslagen. We gaan ervan uit dat u drie regio's (VS-Oost, Europa-noord en Azië-oost) toevoegt aan uw Azure Cosmos-account, elk met dezelfde opslag en door voer. Uw totale maandelijkse factuur wordt (ervan uitgaande in 30 dagen per maand). Uw factuur zou er als volgt uitzien: 

|**Item** |**Gebruik (maand)** |**Malen** |**Maandelijkse kosten** |
|---------|---------|---------|-------|
|Doorvoerfactuur voor container in US - west      | 10.000 RU/SEC * 24 * 30    |$0,008 per 100 RU/sec per uur   |$576|
|Doorvoerfactuur voor drie extra regio's: US - oost, Europa - noord en Azië - oost       | 3 * 10.000 RU/SEC * 24 * 30    |$0,008 per 100 RU/sec per uur  |$1.728|
|Opslagfactuur voor container in US - west      | 250 GB    |$0,25/GB  |$62,50|
|Opslagfactuur voor drie extra regio's: US - oost, Europa - noord en Azië - oost      | 3 * 250 GB    |$0,25/GB  |$187,50|
|**Eind**     |     |  |**$2.554**|

*Er wordt ook van uitgegaan dat u elke maand 100 GB aan gegevens van de container in West-US uitmaakt om gegevens te repliceren naar de VS-Oost, Europa-noord en Azië-oost. U wordt gefactureerd voor uitgaand verkeer volgens tarieven per gegevens overdracht.*

### <a name="billing-example-multi-region-azure-cosmos-account-multi-region-writes"></a>Facturerings voorbeeld: meerdere regio's Azure Cosmos-account, meerdere regio's schrijven

We gaan ervan uit dat u een Azure Cosmos-container maakt in VS West. De container wordt gemaakt met een door Voer van 10.000 RU/SEC en u hebt 1 TB aan gegevens per maand opgeslagen. We gaan ervan uit dat u drie regio's toevoegt (VS-Oost, Europa-noord en Azië-oost), elk met dezelfde opslag en door Voer en u de mogelijkheid wilt geven om te schrijven naar de containers in alle regio's die zijn gekoppeld aan uw Azure Cosmos-account. De totale maandelijkse factuur wordt als volgt (indien 30 dagen per maand):

|**Item** |**Gebruik (maand)**|**Malen** |**Maandelijkse kosten** |
|---------|---------|---------|-------|
|Doorvoer factuur voor container in West-VS (alle regio's zijn schrijfbaar)       | 10.000 RU/SEC * 24 * 30    |$0,016 per 100 RU/sec per uur    |$1.152 |
|Doorvoer factuur voor drie extra regio's: VS-Oost, Europa-noord en Azië-oost (alle regio's zijn schrijfbaar)        | (3 + 1) * 10.000 RU/SEC * 24 * 30    |$0,016 per 100 RU/sec per uur   |$4.608 |
|Opslagfactuur voor container in US - west      | 250 GB    |$0,25/GB  |$62,50|
|Opslagfactuur voor drie extra regio's: US - oost, Europa - noord en Azië - oost      | 3 * 250 GB    |$0,25/GB  |$187,50|
|**Eind**     |     |  |**$6.010**|

*Er wordt ook van uitgegaan dat u elke maand 100 GB aan gegevens van de container in West-US uitmaakt om gegevens te repliceren naar de VS-Oost, Europa-noord en Azië-oost. U wordt gefactureerd voor uitgaand verkeer volgens tarieven per gegevens overdracht.*

### <a name="billing-example-azure-cosmos-account-with-multi-master-database-level-throughput-including-dedicated-throughput-mode-for-some-containers"></a>Facturering-voorbeeld: Azure Cosmos-account met multi-master, data base-niveau doorvoer, inclusief toegewezen doorvoer modus voor sommige containers

We gaan het volgende voor beeld overwegen, waarbij we een Azure Cosmos-account met meerdere regio's bevatten waarin alle regio's schrijfbaar zijn (configuratie van meerdere masters). Ter vereenvoudiging gaan we ervan uit dat de opslag ruimte constant blijft en niet wordt gewijzigd en deze hier weglaat om het voor beeld eenvoudiger te houden. De ingerichte door Voer in de maand varieerde als volgt (ervan uitgaande dat er 30 dagen of 720 uur is): 

[0-100 uur]:  

* We hebben een Azure Cosmos-account gemaakt (VS-West, VS-Oost, Europa-noord), waarbij alle regio's beschrijfbaar zijn 

* We hebben een Data Base (D1) gemaakt met gedeelde door Voer 10.000 RU/SEC 

* We hebben een Data Base (D2) gemaakt met gedeelde door Voer 30-K RU/SEC en  

* Er is een container (C1) gemaakt met een speciale door Voer 20 K RU/sec. 

[101-200 uur]:  

* We hebben de data base (D1) omhoog geschaald tot 50 K RU/sec. 

* We hebben de data base (D2) omhoog geschaald tot 70 K RU/sec.  

* De container (C1) is verwijderd  

[201-300 uur]:  

* We hebben de container (C1) opnieuw gemaakt met de toegewezen door Voer 20 K RU/sec. 

[301-400 uur]:  

* Er is een van de regio's uit het Azure Cosmos-account verwijderd (aantal Beschrijf bare regio's is nu 2) 

* We hebben de data base (D1) geschaald naar 10.000 RU/sec. 

* We hebben de data base (D2) omhoog geschaald tot 80 K RU/sec.  

* De container (C1) wordt opnieuw verwijderd 

[401-500 uur]:  

* We hebben de data base (D2) geschaald naar 10.000 RU/SEC  

* We hebben de container (C1) opnieuw gemaakt met de toegewezen door Voer 20 K RU/sec. 

[501-700 uur]:  

* We hebben de data base (D1) omhoog geschaald tot 20 K RU/sec.  

* We hebben de data base (D2) omhoog geschaald tot 100 K RU/sec.  

* De container (C1) wordt opnieuw verwijderd  

[701-720 uur]:  

* We hebben de data base (D2) omlaag geschaald tot 50 K RU/sec.  

In de onderstaande afbeelding ziet u de wijzigingen in de totale ingerichte door Voer gedurende 720 uur voor de maand. 

![Voor beeld van Real-Life](./media/understand-your-bill/bill-example3.png)

De totale maandelijkse factuur (ervan uitgaande tot 30 dagen/720 uur per maand) wordt als volgt berekend:

|**Loopt**  |**RU/s** |**Item** |**Gebruik (elk uur)** |**Kosten** |
|---------|---------|---------|-------|-------|
|[0-100] |D1:10K <br/>D2:30K <br/>C1:20.000 |Doorvoer factuur voor container in West-VS (alle regio's zijn schrijfbaar)  | `D1: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br/>`D2: 30 K RU/sec/100 * $0.016 * 100 hours = $480` <br/>`C1: 20 K RU/sec/100 *$0.016 * 100 hours = $320` |$960  |
| | |Doorvoer factuur voor twee extra regio's: VS-Oost, Europa-noord (alle regio's zijn schrijfbaar)  |`(2 + 1) * (60 K RU/sec /100 * $0.016) * 100 hours = $2,880`  |$2.880  |
|[101-200] |D1:50.000 <br/>D2:70K <br/>C1:-- |Doorvoer factuur voor container in West-VS (alle regio's zijn schrijfbaar)  |`D1: 50 K RU/sec/100 * $0.016 * 100 hours = $800` <br/>`D2: 70 K RU/sec/100 * $0.016 * 100 hours = $1,120` |$1920  |
| | |Doorvoer factuur voor twee extra regio's: VS-Oost, Europa-noord (alle regio's zijn schrijfbaar)  |`(2 + 1) * (120 K RU/sec /100 * $0.016) * 100 hours = $5,760`  |$5.760  |
|[201-300]  |D1:50.000 <br/>D2:70K <br/>C1:20.000 |Doorvoer factuur voor container in West-VS (alle regio's zijn schrijfbaar)  |`D1: 50 K RU/sec/100 * $0.016 * 100 hours = $800` <br/>`D2: 70 K RU/sec/100 * $0.016 * 100 hours = $1,120` <br/>`C1: 20 K RU/sec/100 *$0.016 * 100 hours = $320` |$2.240  |
| | |Doorvoer factuur voor twee extra regio's: VS-Oost, Europa-noord (alle regio's zijn schrijfbaar)  |`(2 + 1) * (140 K RU/sec /100 * $0.016-) * 100 hours = $6,720` |$6.720 |
|[301-400] |D1:10K <br/>D2:80K <br/>C1:-- |Doorvoer factuur voor container in West-VS (alle regio's zijn schrijfbaar)  |`D1: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br/>`D2: 80 K RU/sec/100 * $0.016 * 100 hours = $1,280`  |$1.440   |
| | |Doorvoer factuur voor twee extra regio's: VS-Oost, Europa-noord (alle regio's zijn schrijfbaar)  |`(1 + 1) * (90 K RU/sec /100 * $0.016) * 100 hours = $2,880`  |$2.880  |
|[401-500] |D1:10K <br>D2:10K <br>C1:20.000 |Doorvoer factuur voor container in West-VS (alle regio's zijn schrijfbaar)  |`D1: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br>`D2: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br>`C1: 20 K RU/sec/100 *$0.016 * 100 hours = $320` |$640  |
| | |Doorvoer factuur voor twee extra regio's: VS-Oost, Europa-noord (alle regio's zijn schrijfbaar)  |`(1 + 1) * (40 K RU/sec /100 * $0.016) * 100 hours = $1,280`  |$1.280  |
|[501-700] |D1:20.000 <br>D2:100.000 <br>C1:-- |Doorvoer factuur voor container in West-VS (alle regio's zijn schrijfbaar)  |`D1: 20 K RU/sec/100 * $0.016 * 200 hours = $640` <br>`D2: 100 K RU/sec/100 * $0.016 * 200 hours = $3,200` |$3.840  |
| | |Doorvoer factuur voor twee extra regio's: VS-Oost, Europa-noord (alle regio's zijn schrijfbaar)  |`(1 + 1) * (120 K RU/sec /100 * $0.016) * 200 hours = $1,280`  |$7.680  |
|[701-720] |D1:20.000 <br/>D2:50.000 <br/>C1:-- |Doorvoer factuur voor container in West-VS (alle regio's zijn schrijfbaar)  |`D1: 20 K RU/sec/100 *$0.016 * 20 hours = $64` <br/>`D2: 50 K RU/sec/100 *$0.016 * 20 hours = $160` |$224  |
| | |Doorvoer factuur voor twee extra regio's: VS-Oost, Europa-noord (alle regio's zijn schrijfbaar)  |`(1 + 1) * (70 K RU/sec /100 * $0.016) * 20 hours = $448`  |$224  |
|| |**Totale maandelijkse kosten**  | |**$38.688**   |

## <a name="proactively-estimating-your-monthly-bill"></a>Proactief schatten van uw maandelijkse factuur  

Laten we eens kijken naar een ander voor beeld, waar u proactief uw factuur kunt inschatten vóór het einde van de maand. U kunt uw factuur als volgt schatten:

|**Opslag kosten** | |
|----|----|
|Gem. record grootte (KB) |1 |
|Aantal records  |100.000.000  |
|Totale opslag (GB)  |100 |
|Maandelijkse kosten per GB  |$0,25  |
|Verwachte maandelijkse kosten voor opslag   |$25,00  |

<br>

|**Doorvoer kosten** | | | |
|----|----|----|----|
|Bewerkingstype| Aanvragen per seconde| Gem. RU/aanvraag| RUs vereist|
|Schrijven| 100 | 5 | 500|
|Lezen| 400| 1| 400|

Totaal aantal RU/SEC: 500 + 400 = 900 uur kosten: 900/100 * $0,008 = $0,072 verwachte maandelijkse kosten voor door Voer (aangenomen 31 dagen): $0,072 * 24 * 31 = $53,57

**Totale maandelijkse kosten**

Totale maandelijkse kosten = maandelijkse kosten voor opslag en maandelijkse kosten voor het totale aantal maandelijkse kosten voor door Voer = $25,00 + $53,57 = $78,57

*Prijzen kunnen per regio verschillen. Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/)voor actuele prijzen.*

## <a name="billing-with-azure-cosmos-db-reserved-capacity"></a>Facturering met Azure Cosmos DB gereserveerde capaciteit

Azure Cosmos DB gereserveerde capaciteit biedt u de mogelijkheid om vooraf ingerichte door voer te kopen (een gereserveerde capaciteit of een reserve ring) die kan worden toegepast op alle Azure Cosmos DB data bases en containers (voor elke API of elk gegevens model) in alle Azure-regio's. Omdat de ingerichte doorvoer prijs per regio verschilt, is het handiger om gereserveerde capaciteit te beschouwen als een monetair tegoed dat u bij een korting hebt aangeschaft en dat kan worden opgehaald uit de ingerichte door Voer voor de desbetreffende prijs in elke regio. Stel bijvoorbeeld dat u een Azure Cosmos-account hebt met één container die is ingericht met 50-K RU/SEC en wereld wijd gerepliceerde twee regio's, VS-Oost en Japan-Oost. Als u kiest voor de optie betalen naar gebruik, betaalt u het volgende:  

* in VS-Oost: voor 50-K RU/seconde tegen $0,008 per 100 RU/sec in die regio 

* in Japan-Oost: voor 50-K RU/sec. met een snelheid van $0,009 per 100 RU/sec in die regio

Uw totale factuur (zonder gereserveerde capaciteit) zou worden (waarbij 30 dagen of 720 uur wordt uitgegaan): 

|**Regio**| **Uurtarief per uur op basis van 100 RU/s**|**Eenheden (RU/s)**|**Gefactureerd bedrag (per uur)**| **Gefactureerd bedrag (maandelijks)**|
|----|----|----|----|----|
|East US|$0,008 |50 K|$4|$2.880 |
|Japan - oost|$0,009 |50 K| $4,50 |$3.240 |
|Totaal|||$8,50|$6.120 |

U kunt in plaats daarvan een gereserveerde capaciteit hebben gekocht. U kunt gereserveerde capaciteit voor 100-K RU/seconde kopen tegen de prijs van $56.064 gedurende één jaar (bij 20% korting) of $6,40 per uur. Bekijk de prijzen voor gereserveerde capaciteit op de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/).  

* Kosten van door Voer (betalen naar gebruik): 100.000 RU/SEC/100 * $0.008/uur * 8760 uur in een jaar = $70.080 

* Kosten van door Voer (met gereserveerde capaciteit) $70.080 gedisconteerd op 20% = $56.064 

Wat u effectief hebt aangeschaft, is een tegoed van $8 per uur, voor 100 K RU per seconde, met behulp van de lijst prijs in VS-Oost, tegen de prijs van $6,40 per uur. U kunt vervolgens op elk uur aftrekken van deze vooraf betaalde door Voer reserve ring voor de ingerichte doorvoer capaciteit in een wereld wijde Azure-regio op basis van de regionale lijst prijzen die zijn ingesteld voor uw abonnement. In dit voor beeld, waarin u 50 K RU/sec. hebt ingericht in VS-Oost en Japan-Oost, kunt u $8,00 waard van ingerichte door Voer per uur opstellen en wordt de overschrijding van $0,50 per uur (of $360/maand) gefactureerd. 

|**Regio**| **Uurtarief per uur op basis van 100 RU/s**|**Eenheden (RU/s)**| **Gefactureerd bedrag (per uur)**| **Gefactureerd bedrag (maandelijks)**|
|----|----|----|----|----|
|East US|$0,008 |50 K|$4|$2.880 |
|Japan - oost|$0,009 |50 K| $4,50 |$3.240 |
|||Betalen per gebruik|$8,50|$6120|
|Aangeschafte gereserveerde capaciteit|$0,0064 (20% korting) |100 RU/sec of $8 capaciteit vooraf aangeschaft |-$8|-$5.760 |
|Nettofactuur|||$0,50 |$360 |

## <a name="next-steps"></a>Volgende stappen

U kunt nu door gaan met de volgende artikelen voor meer informatie over cost Optimization in Azure Cosmos DB:

* Meer informatie over [hoe Cosmos DB prijs model rendabel is voor klanten](total-cost-ownership.md)
* Meer informatie over het [optimaliseren voor ontwikkeling en testen](optimize-dev-test.md)
* Meer informatie over het [optimaliseren van doorvoer kosten](optimize-cost-throughput.md)
* Meer informatie over het [optimaliseren van opslag kosten](optimize-cost-storage.md)
* Meer informatie over [het optimaliseren van de kosten van lees-en schrijf bewerkingen](optimize-cost-reads-writes.md)
* Meer informatie over [het optimaliseren van de kosten van query's](optimize-cost-queries.md)
* Meer informatie over [het optimaliseren van de kosten voor Azure Cosmos-accounts met meerdere regio's](optimize-cost-regions.md)
