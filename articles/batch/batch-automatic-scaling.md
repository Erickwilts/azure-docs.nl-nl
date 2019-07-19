---
title: Reken knooppunten automatisch schalen in een Azure Batch groep | Microsoft Docs
description: Schakel automatisch schalen in een Cloud groep in om het aantal reken knooppunten in de pool dynamisch aan te passen.
services: batch
documentationcenter: ''
author: laurenhughes
manager: gwallace
editor: ''
ms.assetid: c624cdfc-c5f2-4d13-a7d7-ae080833b779
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: multiple
ms.date: 06/20/2017
ms.author: lahugh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 489a3935605432b485f7b0866668f6dbfaac686b
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68323758"
---
# <a name="create-an-automatic-scaling-formula-for-scaling-compute-nodes-in-a-batch-pool"></a>Een automatisch Schalende formule voor het schalen van reken knooppunten in een batch-pool maken

Azure Batch kunnen Pools automatisch schalen op basis van de para meters die u definieert. Met automatisch schalen voegt batch knoop punten dynamisch toe aan een groep als taak vereisten toenemen en worden reken knooppunten verwijderd naarmate ze afnemen. U kunt zowel tijd als geld besparen door het aantal reken knooppunten dat wordt gebruikt door uw batch-toepassing automatisch aan te passen. 

U kunt automatisch schalen inschakelen voor een groep reken knooppunten door deze te koppelen aan een auto *Scale-formule* die u definieert. De batch-service gebruikt de formule voor automatisch schalen om het aantal reken knooppunten te bepalen die nodig zijn om de werk belasting uit te voeren. Reken knooppunten kunnen specifieke knoop punten of [knoop punten met lage prioriteit](batch-low-pri-vms.md)zijn. Batch reageert op gegevens over service-metrieken die periodiek worden verzameld. Met deze metrische gegevens kunt u het aantal reken knooppunten in de pool aanpassen op basis van de formule en een configureerbaar interval.

U kunt automatisch schalen inschakelen wanneer u een pool maakt of op een bestaande groep. U kunt ook een bestaande formule wijzigen voor een groep die is geconfigureerd voor automatisch schalen. Met Batch kunt u uw formules evalueren voordat u deze aan groepen toewijst en de status van automatisch schalen uitvoert.

In dit artikel worden de verschillende entiteiten beschreven waaruit uw formules voor automatisch schalen, waaronder variabelen, Opera Tors, bewerkingen en functies, worden besproken. We bespreken hoe u verschillende reken resources en taak gegevens in batch kunt ophalen. U kunt deze metrische gegevens gebruiken om het aantal knoop punten van uw groep aan te passen op basis van het resource gebruik en de taak status. Vervolgens wordt beschreven hoe u een formule maakt en automatisch schalen op een groep inschakelt met behulp van de batch REST-en .NET-Api's. Ten slotte volt ooien we samen met enkele voor beelden van formules.

> [!IMPORTANT]
> Wanneer u een batch-account maakt, kunt u de [account configuratie](batch-api-basics.md#account)opgeven, waarmee wordt bepaald of groepen worden toegewezen in een batch service-abonnement (de standaard instelling) of in uw gebruikers abonnement. Als u uw batch-account hebt gemaakt met de standaard configuratie van de batch-service, is uw account beperkt tot een maximum aantal kernen dat kan worden gebruikt voor de verwerking. De batch-service schaalt alleen reken knooppunten tot die kern limiet. Daarom kan de batch-service niet het doel aantal reken knooppunten bereiken dat is opgegeven door een formule voor automatisch schalen. Zie [quota's en limieten voor de Azure batch-service](batch-quota-limit.md) voor meer informatie over het weer geven en uitbreiden van uw account quota's.
>
>Als u uw account hebt gemaakt met de configuratie voor gebruikers abonnementen, worden uw account shares in het kern quotum voor het abonnement. Zie [Virtual Machines limits](../azure-subscription-service-limits.md#virtual-machines-limits) (Limieten voor Virtuele Machines) in [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md) (Azure-abonnement en servicelimieten, -quota en -beperkingen) voor meer informatie.
>
>

## <a name="automatic-scaling-formulas"></a>Formules voor automatisch schalen
Een formule voor automatisch schalen is een teken reeks waarde die u definieert die een of meer instructies bevat. De formule voor automatisch schalen wordt toegewezen aan de eigenschap [autoScaleFormula][rest_autoscaleformula] element (Batch REST) or [CloudPool.AutoScaleFormula][net_cloudpool_autoscaleformula] van een groep (batch .net). De batch-service gebruikt de formule om het doel aantal reken knooppunten in de pool te bepalen voor het volgende interval voor de verwerking. De formule teken reeks mag niet langer zijn dan 8 KB, kan bestaan uit Maxi maal 100 instructies, gescheiden door punt komma's, en regel einden en opmerkingen kunnen bevatten.

U kunt formules voor automatisch schalen beschouwen als een batch-automatische schaal aanpassing. Formule-instructies zijn vrije expressies die zowel door de service gedefinieerde variabelen (variabelen die zijn gedefinieerd door de batch-service) als door de gebruiker gedefinieerde variabelen (variabelen die u definieert) kunnen bevatten. Ze kunnen verschillende bewerkingen uitvoeren op deze waarden met behulp van ingebouwde typen, Opera tors en functies. Een instructie kan bijvoorbeeld de volgende vorm hebben:

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

Formules bevatten doorgaans meerdere instructies voor het uitvoeren van bewerkingen op waarden die zijn verkregen in eerdere instructies. U krijgt bijvoorbeeld eerst een waarde voor en geeft `variable1`deze door aan een functie voor het vullen `variable2`van het volgende:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

Neem deze instructies op in uw formule voor automatisch schalen om te komen op het doel aantal reken knooppunten. Specifieke knoop punten en knoop punten met een lage prioriteit hebben elk hun eigen doel instellingen, zodat u een doel voor elk type knoop punt kunt definiëren. Een formule voor automatisch schalen kan een doel waarde bevatten voor toegewezen knoop punten, een doel waarde voor knoop punten met een lage prioriteit of beide.

Het doel aantal knoop punten kan hoger of lager of gelijk zijn aan het huidige aantal knoop punten van het type in de groep. Batch evalueert de formule voor automatisch schalen van een pool met een bepaald interval (Zie [Automatische schaal intervallen](#automatic-scaling-interval)). Met batch wordt het doel nummer van elk type knoop punt in de pool aangepast aan het getal dat uw formule voor automatisch schalen op het moment van de evaluatie opgeeft.

### <a name="sample-autoscale-formulas"></a>Voor beeld van formules voor automatisch schalen

Hieronder vindt u voor beelden van twee automatisch schalen formules, die kunnen worden aangepast aan de meeste scenario's. De variabelen `startingNumberOfVMs` en `maxNumberofVMs` in de voorbeeld formules kunnen worden aangepast aan uw behoeften.

#### <a name="pending-tasks"></a>Taken in behandeling
```
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicatedNodes=min(maxNumberofVMs, pendingTaskSamples);
```

Met deze formule voor automatisch schalen wordt de pool in eerste instantie gemaakt met één virtuele machine. De `$PendingTasks` metrische gegevens bepalen het aantal taken dat wordt uitgevoerd of in de wachtrij wordt geplaatst. Met de formule vindt u het gemiddelde aantal taken in de afgelopen 180 seconden dat in behandeling `$TargetDedicatedNodes` is en stelt u de variabele dienovereenkomstig in. De formule zorgt ervoor dat het doel aantal toegewezen knoop punten nooit meer dan 25 Vm's overschrijdt. Wanneer er nieuwe taken worden verzonden, wordt de groep automatisch verg root. Zodra de taken zijn voltooid, worden de virtuele machines één voor één vrijgegeven en wordt de groep met de formule voor automatisch schalen kleiner.

Met deze formule worden toegewezen knoop punten geschaald, maar kunnen ook worden aangepast om knoop punten met een lage prioriteit te schalen.

#### <a name="preempted-nodes"></a>Knoop punten die zijn afgebroken 
```
maxNumberofVMs = 25;
$TargetDedicatedNodes = min(maxNumberofVMs, $PreemptedNodeCount.GetSample(180 * TimeInterval_Second));
$TargetLowPriorityNodes = min(maxNumberofVMs , maxNumberofVMs - $TargetDedicatedNodes);
```

In dit voor beeld wordt een pool gemaakt die begint met 25 knoop punten met lage prioriteit. Telkens wanneer een knoop punt met een lage prioriteit is afgebroken, wordt het vervangen door een toegewezen knoop punt. Net als bij het eerste voor beeld `maxNumberofVMs` wordt voor komen dat de groep meer dan 25 vm's overschrijdt. Dit voor beeld is handig voor het gebruik van Vm's met lage prioriteit, terwijl u er ook voor zorgt dat alleen een vast aantal preemptions plaatsvindt voor de levens duur van de groep.

## <a name="variables"></a>Variabelen
U kunt zowel door de **service gedefinieerde** als door de **gebruiker gedefinieerde** variabelen gebruiken in uw formules voor automatisch schalen. De door de service gedefinieerde variabelen zijn ingebouwd in de batch-service. Sommige door de service gedefinieerde variabelen zijn lezen/schrijven, en sommige zijn alleen-lezen. Door de gebruiker gedefinieerde variabelen zijn variabelen die u definieert. In de voorbeeld formule die wordt weer gegeven in de `$TargetDedicatedNodes` vorige `$PendingTasks` sectie en zijn door de service gedefinieerde variabelen. Variabelen `startingNumberOfVMs` en`maxNumberofVMs` zijn door de gebruiker gedefinieerde variabelen.

> [!NOTE]
> Door de service gedefinieerde variabelen worden altijd voorafgegaan door een dollar teken ($). Het dollar teken is optioneel voor door de gebruiker gedefinieerde variabelen.
>
>

De volgende tabellen bevatten zowel lees-/schrijftoegang als alleen-lezen variabelen die zijn gedefinieerd door de batch-service.

U kunt de waarden van deze door de service gedefinieerde variabelen ophalen en instellen om het aantal reken knooppunten in een pool te beheren:

| Door service gedefinieerde variabelen lezen/schrijven | Description |
| --- | --- |
| $TargetDedicatedNodes |Het doel aantal toegewezen reken knooppunten voor de pool. Het aantal toegewezen knoop punten is als doel opgegeven, omdat een pool mogelijk niet altijd het gewenste aantal knoop punten bereikt. Als het doel aantal toegewezen knoop punten bijvoorbeeld wordt gewijzigd door een evaluatie van automatisch schalen voordat de groep het eerste doel heeft bereikt, kan de groep het doel niet bereiken. <br /><br /> Een pool in een account dat is gemaakt met de batch-service configuratie heeft mogelijk niet het doel bereikt als het doel een knoop punt van een batch-account of een kern quotum overschrijdt. Een pool in een account dat is gemaakt met de configuratie van gebruikers abonnementen, bereikt mogelijk niet het doel als het doel het gedeelde kern quotum voor het abonnement overschrijdt.|
| $TargetLowPriorityNodes |Het doel aantal reken knooppunten met lage prioriteit voor de groep. Het aantal knoop punten met een lage prioriteit wordt als doel opgegeven, omdat een pool mogelijk niet altijd het gewenste aantal knoop punten bereikt. Als het doel aantal knoop punten met een lage prioriteit bijvoorbeeld wordt gewijzigd door een evaluatie van automatisch schalen voordat de groep het eerste doel heeft bereikt, kan de groep het doel niet bereiken. Het doel van een pool kan ook niet worden bereikt als het doel een knoop punt van een batch-account of een kern quotum overschrijdt. <br /><br /> Zie [virtuele machines met lage prioriteit gebruiken met batch (preview)](batch-low-pri-vms.md)voor meer informatie over reken knooppunten met lage prioriteit. |
| $NodeDeallocationOption |De actie die wordt uitgevoerd wanneer reken knooppunten uit een pool worden verwijderd. Mogelijke waarden zijn:<ul><li>opnieuw **in de wachtrij plaatsen**: Hiermee worden taken onmiddellijk beëindigd en weer in de taak wachtrij geplaatst, zodat deze opnieuw worden gepland.<li>**beëindigen**: Hiermee worden taken onmiddellijk beëindigd en uit de taak wachtrij verwijderd.<li>**taskcompletion**: wacht tot actieve taken zijn voltooid en verwijder vervolgens het knoop punt uit de groep.<li>**retaineddata**: er wordt gewacht tot alle lokale taak gegevens die op het knoop punt zijn bewaard, moeten worden opgeruimd voordat het knoop punt uit de groep wordt verwijderd.</ul> |

U kunt de waarde van deze door de service gedefinieerde variabelen ophalen om aanpassingen te maken die zijn gebaseerd op metrische gegevens van de batch-service:

| Alleen-lezen service gedefinieerde variabelen | Description |
| --- | --- |
| $CPUPercent |Het gemiddelde percentage van CPU-gebruik. |
| $WallClockSeconds |Het aantal seconden dat is verbruikt. |
| $MemoryBytes |Het gemiddelde aantal mega bytes dat wordt gebruikt. |
| $DiskBytes |Het gemiddelde aantal gigabytes dat is gebruikt op de lokale schijven. |
| $DiskReadBytes |Het aantal gelezen bytes. |
| $DiskWriteBytes |Het aantal geschreven bytes. |
| $DiskReadOps |Het aantal uitgevoerde Lees schijf bewerkingen. |
| $DiskWriteOps |Het aantal bewerkingen voor schrijf schijven dat is uitgevoerd. |
| $NetworkInBytes |Het aantal binnenkomende bytes. |
| $NetworkOutBytes |Het aantal uitgaande bytes. |
| $SampleNodeCount |Het aantal reken knooppunten. |
| $ActiveTasks |Het aantal taken dat kan worden uitgevoerd, maar nog niet is uitgevoerd. Het aantal $ActiveTasks omvat alle taken in de actieve status en waarvan is voldaan aan de afhankelijkheden. Taken die de actieve status hebben maar waarvan niet is voldaan aan de afhankelijkheden, worden uitgesloten van het $ActiveTasks aantal.|
| $RunningTasks |Het aantal taken in een uitvoerings status. |
| $PendingTasks |De som van $ActiveTasks en $RunningTasks. |
| $SucceededTasks |Het aantal taken dat is voltooid. |
| $FailedTasks |Het aantal mislukte taken. |
| $CurrentDedicatedNodes |Het huidige aantal toegewezen reken knooppunten. |
| $CurrentLowPriorityNodes |Het huidige aantal reken knooppunten met lage prioriteit, met inbegrip van knoop punten met een vooraf-bezet. |
| $PreemptedNodeCount | Het aantal knoop punten in de pool dat zich in een pre-bezet bevindt. |

> [!TIP]
> De alleen-lezen, service gedefinieerde variabelen die in de vorige tabel worden weer gegeven, zijn *objecten* die verschillende methoden bieden om toegang te krijgen tot gegevens die zijn gekoppeld aan elk. Zie [voorbeeld gegevens verkrijgen](#getsampledata) verderop in dit artikel voor meer informatie.
>
>

## <a name="types"></a>Dergelijke
Deze typen worden ondersteund in een formule:

* double
* doubleVec
* doubleVecList
* string
* tijds tempel: tijds tempel is een samengestelde structuur die de volgende leden bevat:

  * jaar
  * maand (1-12)
  * dag (1-31)
  * weekdag (in de notatie van nummer; bijvoorbeeld 1 voor maandag)
  * uur (in 24-uurs getalnotatie; bijvoorbeeld 13 betekent 1 uur)
  * minuut (00-59)
  * seconde (00-59)
* timeinterval

  * TimeInterval_Zero
  * TimeInterval_100ns
  * TimeInterval_Microsecond
  * TimeInterval_Millisecond
  * TimeInterval_Second
  * TimeInterval_Minute
  * TimeInterval_Hour
  * TimeInterval_Day
  * TimeInterval_Week
  * TimeInterval_Year

## <a name="operations"></a>Bewerkingen
Deze bewerkingen zijn toegestaan voor de typen die worden vermeld in de vorige sectie.

| Bewerking | Ondersteunde Opera tors | Resultaat type |
| --- | --- | --- |
| dubbele *operator* dubbele |+, -, *, / |double |
| dubbele *operator* TimeInterval |* |timeinterval |
| dubbele *operator* doubleVec |+, -, *, / |doubleVec |
| doubleVec- *operator* doubleVec |+, -, *, / |doubleVec |
| dubbele *operator* TimeInterval |*, / |timeinterval |
| TimeInterval- *operator* TimeInterval |+, - |timeinterval |
| tijds tempel TimeInterval *operator* |+ |timestamp |
| tijds tempel *operator* TimeInterval |+ |timestamp |
| tijds tempel *operator* time stamp |- |timeinterval |
| dubbele *operator* |-, ! |double |
| *operator*timeinterval |- |timeinterval |
| dubbele *operator* dubbele |<, <=, ==, >=, >, != |double |
| teken reeks *operator* |<, <=, ==, >=, >, != |double |
| tijds tempel *operator* time stamp |<, <=, ==, >=, >, != |double |
| TimeInterval- *operator* TimeInterval |<, <=, ==, >=, >, != |double |
| dubbele *operator* dubbele |&&, &#124;&#124; |double |

Bij het testen van een dubbele met een ternaire`double ? statement1 : statement2`operator (), is niet nul **waar**en is nul **False**.

## <a name="functions"></a>Functions
Deze vooraf gedefinieerde **functies** zijn beschikbaar voor gebruik bij het definiëren van een formule voor automatisch schalen.

| Function | Retour type | Description |
| --- | --- | --- |
| avg(doubleVecList) |double |Retourneert de gemiddelde waarde voor alle waarden in de doubleVecList. |
| len (doubleVecList) |double |Retourneert de lengte van de vector die is gemaakt op basis van de doubleVecList. |
| LG (double) |double |Retourneert de logaritme van het logboek grondtal 2 van de dubbele waarde. |
| lg(doubleVecList) |doubleVec |Hiermee wordt het onderdeel-and-log-basis 2 van de doubleVecList geretourneerd. Een VEC (double) moet expliciet worden door gegeven voor de para meter. Anders wordt ervan uitgegaan dat de dubbele LG (double) versie wordt gebruikt. |
| LN (double) |double |Retourneert het natuurlijke logboek van de dubbele waarde. |
| LN (doubleVecList) |doubleVec |Hiermee wordt het onderdeel-and-log-basis 2 van de doubleVecList geretourneerd. Een VEC (double) moet expliciet worden door gegeven voor de para meter. Anders wordt ervan uitgegaan dat de dubbele LG (double) versie wordt gebruikt. |
| logboek (dubbel) |double |Retourneert de logaritme met grondtal 10 van de dubbele waarde. |
| log(doubleVecList) |doubleVec |Hiermee wordt het onderdeel weerstands logboek van de doubleVecList geretourneerd. Een VEC (double) moet expliciet worden door gegeven voor de enkele dubbele para meter. Anders wordt de dubbele logboek versie (double) gebruikt. |
| Max (doubleVecList) |double |Retourneert de maximum waarde in de doubleVecList. |
| min (doubleVecList) |double |Retourneert de minimum waarde in de doubleVecList. |
| norm (doubleVecList) |double |Retourneert de twee-norm van de vector die is gemaakt op basis van de doubleVecList. |
| percentiel (doubleVec v, dubbele p) |double |Retourneert het percentiel element van de vector v. |
| ASELECT () |double |Retourneert een wille keurige waarde tussen 0,0 en 1,0. |
| range(doubleVecList) |double |Retourneert het verschil tussen de minimum-en maximum waarden in de doubleVecList. |
| STD (doubleVecList) |double |Retourneert de standaard deviatie van de steek proef van de waarden in de doubleVecList. |
| stoppen () | |Stopt de evaluatie van de expressie voor automatisch schalen. |
| Sum (doubleVecList) |double |Retourneert de som van alle onderdelen van de doubleVecList. |
| time(string dateTime="") |timestamp |Retourneert het tijds tempel van de huidige tijd als er geen para meters zijn door gegeven, of het tijds tempel van de dateTime-teken reeks als deze wordt door gegeven. Ondersteunde dateTime-indelingen zijn W3C-DTF en RFC 1123. |
| Val (doubleVec v, Double i) |double |Retourneert de waarde van het element dat zich op locatie i in vector v bevindt, met een begin index van nul. |

Sommige functies die in de vorige tabel worden beschreven, kunnen een lijst als argument accepteren. De lijst met door komma's gescheiden waarden is een combi natie van *dubbele* en *doubleVec*. Bijvoorbeeld:

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

De *doubleVecList* -waarde wordt voor de evaluatie geconverteerd naar één *doubleVec* . Bijvoorbeeld, als `v = [1,2,3]`, wordt aangeroepen `avg(v)` , is gelijk aan aanroepen `avg(1,2,3)`. Aanroepen `avg(v, 7)` is gelijk aan `avg(1,2,3,7)`aanroepen.

## <a name="getsampledata"></a>Voorbeeld gegevens verkrijgen
Formules voor automatisch schalen voeren de metrische gegevens (voor beelden) uit die door de batch-service worden verschaft. Een formule verg root of verkleint de pool grootte op basis van de waarden die worden opgehaald uit de service. De door de service gedefinieerde variabelen die eerder zijn beschreven, zijn objecten die verschillende methoden bieden om toegang te krijgen tot gegevens die aan dat object zijn gekoppeld. De volgende expressie toont bijvoorbeeld een aanvraag om de laatste vijf minuten van het CPU-gebruik op te halen:

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| Methode | Description |
| --- | --- |
| GetSample() |De `GetSample()` methode retourneert een vector van gegevens voorbeelden.<br/><br/>Een voor beeld is 30 seconden voor metrische gegevens. Met andere woorden: alle voor beelden worden elke 30 seconden opgehaald. Maar zoals hieronder vermeld, is er een vertraging tussen het moment waarop een voor beeld wordt verzameld en wanneer het beschikbaar is voor een formule. Daarom kunnen niet alle voor beelden voor een bepaalde periode beschikbaar zijn voor evaluatie met een formule.<ul><li>`doubleVec GetSample(double count)`<br/>Hiermee geeft u het aantal steek proeven op dat moet worden opgehaald uit de meest recente voor beelden die zijn verzameld.<br/><br/>`GetSample(1)`retourneert het laatst beschik bare voor beeld. Voor metrische gegevens, `$CPUPercent`zoals, moet dit echter niet worden gebruikt, omdat het onmogelijk is om te weten *Wanneer* het voor beeld is verzameld. Het kan recent zijn of, vanwege systeem problemen, mogelijk veel oudere items zijn. In dergelijke gevallen is het beter om een tijds interval te gebruiken zoals hieronder wordt weer gegeven.<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>Hiermee geeft u een tijds bestek voor het verzamelen van voorbeeld gegevens. U kunt ook het percentage steek proeven opgeven dat beschikbaar moet zijn in het aangevraagde tijds bestek.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)`retourneert 20 voor beelden als alle voor beelden voor de laatste tien minuten aanwezig zijn in de CPUPercent-geschiedenis. Als de laatste minuut van de geschiedenis niet beschikbaar was, worden er echter slechts 18 steek proeven geretourneerd. In dit geval:<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)`Dit kan mislukken omdat er slechts 90 procent van de voor beelden beschikbaar zijn.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)`slagen.<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>Hiermee geeft u een tijds bestek op voor het verzamelen van gegevens, met een begin tijd en een eind tijd.<br/><br/>Zoals hierboven vermeld, is er sprake van een vertraging tussen het moment waarop een voor beeld wordt verzameld en wanneer het beschikbaar is voor een formule. Houd rekening met deze vertraging wanneer u `GetSample` de-methode gebruikt. Zie `GetSamplePercent` hieronder. |
| GetSamplePeriod() |Retourneert de periode van de voor beelden die zijn gemaakt in een historische voorbeeld gegevensset. |
| Aantal () |Retourneert het totale aantal voor beelden in de metrische geschiedenis. |
| HistoryBeginTime() |Retourneert het tijds tempel van het oudste beschik bare gegevens voorbeeld voor de metriek. |
| GetSamplePercent() |Retourneert het percentage steek proeven dat beschikbaar is voor een opgegeven tijds interval. Bijvoorbeeld:<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>Omdat de `samplePercent` `GetSamplePercent` methode mislukt als het percentage geretourneerde steek proeven kleiner is dan het opgegeven aantal, kunt u de methode gebruiken om eerst te controleren. `GetSample` U kunt vervolgens een alternatieve actie uitvoeren als er onvoldoende steek proeven aanwezig zijn, zonder dat de evaluatie van automatisch schalen wordt onderbroken. |

### <a name="samples-sample-percentage-and-the-getsample-method"></a>Voor beelden, steek proef percentage en de methode *GetSample ()*
De kern bewerking van een formule voor automatisch schalen is het verkrijgen van metrische gegevens voor taken en resources en het aanpassen van de grootte van de pool op basis van die gegevens. Daarom is het belang rijk om een duidelijk beeld te hebben van de manier waarop automatisch schalen van formules communiceert met metrische gegevens (voor beelden).

**Voorbeelden**

De batch-service neemt periodiek voor beelden van metrische gegevens voor taken en resources en maakt deze beschikbaar voor uw formules voor automatisch schalen. Deze voor beelden worden elke 30 seconden door de batch-service vastgelegd. Er is echter meestal een vertraging tussen het moment waarop deze voor beelden werden vastgelegd en wanneer ze beschikbaar worden gesteld aan (en kunnen worden gelezen door) uw formules voor automatisch schalen. Vanwege verschillende factoren, zoals netwerk-of andere infrastructuur problemen, kunnen voor een bepaald interval geen voor beelden worden vastgelegd.

**Voorbeeld percentage**

Wanneer `samplePercent` wordt door gegeven aan `GetSample()` de methode of `GetSamplePercent()` de methode wordt aangeroepen, verwijst _percent_ naar een vergelijking tussen het totale aantal voor beelden dat is vastgelegd door de batch-service en het aantal voor beelden dat beschikbaar voor uw formule voor automatisch schalen.

Laten we eens kijken naar een time span van tien minuten als voor beeld. Omdat voor beelden om de 30 seconden worden geregistreerd binnen een time span van tien minuten, zou het maximum aantal voor beelden dat door batch worden vastgelegd, 20 voor beelden (2 per minuut) zijn. Vanwege de inherente latentie van het rapportage mechanisme en andere problemen in azure, kunnen er echter maar 15 voor beelden zijn die beschikbaar zijn voor uw formule voor automatisch schalen om te lezen. Voor die periode van tien minuten is er bijvoorbeeld slechts 75% van het totale aantal geregistreerde steek proeven beschikbaar voor uw formule.

**GetSample () en voorbeeld bereik**

Uw formules voor automatisch schalen worden verg root of verkleind uw Pools &mdash; met het toevoegen van knoop punten of het verwijderen van knoop punten. Omdat knoop punten u geld kosten, wilt u er zeker van zijn dat uw formules gebruikmaken van een intelligente analyse methode op basis van voldoende gegevens. Daarom wordt u aangeraden een trending-type analyse in uw formules te gebruiken. Dit type verg root of verkleint uw Pools op basis van een reeks verzamelde voor beelden.

Als u dit wilt doen `GetSample(interval look-back start, interval look-back end)` , gebruikt u om een vector van voor beelden te retour neren:

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

Wanneer de bovenstaande regel door batch wordt geëvalueerd, wordt een aantal steek proeven geretourneerd als een vector met waarden. Bijvoorbeeld:

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

Wanneer u de vector van voor beelden hebt verzameld, kunt u vervolgens functies als `min()`, `max()`gebruiken en `avg()` zinvolle waarden afleiden van het verzamelde bereik.

Voor extra beveiliging kunt u ervoor zorgen dat een formule-evaluatie mislukt als er minder dan een bepaald steekproef percentage beschikbaar is voor een bepaalde periode. Wanneer u een formule-evaluatie afdwingt, geeft u Batch de opdracht om de formule verder te evalueren als het opgegeven percentage voor beelden niet beschikbaar is. In dit geval wordt er geen wijziging aangebracht in de pool grootte. Als u een vereiste percentage voor steek proeven wilt opgeven voordat de evaluatie slaagt, geeft u dit op `GetSample()`als de derde para meter voor. Hier is een vereiste van 75 procent van de voor beelden opgegeven:

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

Omdat er een vertraging kan optreden in de beschik baarheid van voor beelden, is het belang rijk om altijd een tijds bereik op te geven met een begin tijd van meer dan één minuut. Het duurt ongeveer een minuut voordat er voor beelden worden door gegeven door het systeem, waardoor de steek `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` proeven in het bereik mogelijk niet beschikbaar zijn. U kunt opnieuw de percentage para meter van `GetSample()` gebruiken om een bepaald steekproef percentage af te dwingen.

> [!IMPORTANT]
> We **raden u ten zeerste aan om** **te voor komen dat u *alleen* op uw formules voor `GetSample(1)` automatisch schalen moet vertrouwen**. Dit komt doordat `GetSample(1)` de batch-service, "geef ik de laatste steek proef die u hebt ontvangen, niet meer op hoe lang geleden u deze hebt opgehaald." Omdat het slechts één voor beeld is en een ouder voor beeld is, is het mogelijk niet representatief voor de grotere afbeelding van de status van de recente taak of de resource. Als u dat wel `GetSample(1)`doet, moet u ervoor zorgen dat het deel uitmaakt van een grotere instructie en niet het enige gegevens punt waarop uw formule van toepassing is.
>
>

## <a name="metrics"></a>Metrische gegevens
U kunt zowel de metrische gegevens van de resource als de taak gebruiken wanneer u een formule definieert. U past het doel aantal toegewezen knoop punten in de pool aan op basis van de metrische gegevens die u ophaalt en evalueert. Zie de sectie [variabelen](#variables) hierboven voor meer informatie over elke metriek.

<table>
  <tr>
    <th>Gegevens</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><b>Resource</b></td>
    <td><p>Metrische gegevens van resources zijn gebaseerd op de CPU, de band breedte, het geheugen gebruik van reken knooppunten en het aantal knoop punten.</p>
        <p> Deze door de service gedefinieerde variabelen zijn handig voor het maken van aanpassingen op basis van het aantal knoop punten:</p>
    <p><ul>
            <li>$TargetDedicatedNodes</li>
            <li>$TargetLowPriorityNodes</li>
            <li>$CurrentDedicatedNodes</li>
            <li>$CurrentLowPriorityNodes</li>
            <li>$PreemptedNodeCount</li>
            <li>$SampleNodeCount</li>
    </ul></p>
    <p>Deze door de service gedefinieerde variabelen zijn handig voor het maken van aanpassingen op basis van het resource gebruik van het knoop punt:</p>
    <p><ul>
      <li>$CPUPercent</li>
      <li>$WallClockSeconds</li>
      <li>$MemoryBytes</li>
      <li>$DiskBytes</li>
      <li>$DiskReadBytes</li>
      <li>$DiskWriteBytes</li>
      <li>$DiskReadOps</li>
      <li>$DiskWriteOps</li>
      <li>$NetworkInBytes</li>
      <li>$NetworkOutBytes</li></ul></p>
  </tr>
  <tr>
    <td><b>Taak</b></td>
    <td><p>De metrische gegevens van de taak zijn gebaseerd op de status van taken, zoals actief, in behandeling en voltooid. De volgende door de service gedefinieerde variabelen zijn handig voor het aanpassen van de grootte van een groep op basis van de metrische gegevens van de taak:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a>Een formule voor automatisch schalen schrijven
U maakt een formule voor automatisch schalen met behulp van instructies die gebruikmaken van de bovenstaande onderdelen en combi neren deze instructies vervolgens in een volledige formule. In deze sectie maken we een voor beeld van een formule voor automatisch schalen die een aantal beslissingen over de schaal baarheid kan uitvoeren.

Eerst gaan we de vereisten voor de nieuwe formule voor automatisch schalen definiëren. De formule moet:

1. Verhoog het doel aantal toegewezen reken knooppunten in een pool als het CPU-gebruik hoog is.
2. Verminder het doel aantal toegewezen reken knooppunten in een pool wanneer het CPU-gebruik laag is.
3. Beperk altijd het maximum aantal toegewezen knoop punten tot 400.

Als u het aantal knoop punten tijdens een hoog CPU-gebruik wilt verhogen, definieert u de instructie waarmee een door de`$totalDedicatedNodes`gebruiker gedefinieerde variabele () wordt gevuld met een waarde van 110 procent van het huidige doel aantal toegewezen knoop punten, maar alleen als het minimale gemiddelde CPU-gebruik tijdens de de afgelopen 10 minuten was meer dan 70 procent. Gebruik anders de waarde voor het huidige aantal toegewezen knoop punten.

```
$totalDedicatedNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicatedNodes * 1.1) : $CurrentDedicatedNodes;
```

Als u het aantal toegewezen knoop punten tijdens een laag CPU-gebruik wilt *verlagen* , wordt met de volgende instructie `$totalDedicatedNodes` in de formule dezelfde variabele ingesteld op 90 procent van het huidige doel aantal toegewezen knoop punten als het gemiddelde CPU-gebruik in de afgelopen 60 minuten minder was dan 20 procent. Gebruik anders de huidige waarde van `$totalDedicatedNodes` die we in de bovenstaande instructie hebben ingevuld.

```
$totalDedicatedNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicatedNodes * 0.9) : $totalDedicatedNodes;
```

Beperk nu het doel aantal toegewezen reken knooppunten tot een maximum van 400:

```
$TargetDedicatedNodes = min(400, $totalDedicatedNodes)
```

Dit is de volledige formule:

```
$totalDedicatedNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicatedNodes * 1.1) : $CurrentDedicatedNodes;
$totalDedicatedNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicatedNodes * 0.9) : $totalDedicatedNodes;
$TargetDedicatedNodes = min(400, $totalDedicatedNodes)
```

## <a name="create-an-autoscale-enabled-pool-with-net"></a>Een groep met automatisch schalen maken met .NET

Voer de volgende stappen uit om een pool te maken met automatisch schalen in .NET.

1. Maak de pool met [BatchClient. pool Operations. CreatePool](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.createpool).
2. Stel de eigenschap [CloudPool. AutoScaleEnabled](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleenabled) in `true`op.
3. Stel de eigenschap [CloudPool. AutoScaleFormula](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleformula) in op uw formule voor automatisch schalen.
4. Beschrijving Stel de eigenschap [CloudPool. AutoScaleEvaluationInterval](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval) in (de standaard waarde is 15 minuten).
5. Voer de pool door met [CloudPool. commit](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commit) of [CommitAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commitasync).

Met het volgende code fragment maakt u een groep met automatische schaal functionaliteit in .NET. Met de formule automatisch schalen van de pool wordt het doel aantal toegewezen knoop punten ingesteld op 5 op maandag en 1 op elke andere dag van de week. Het [interval voor automatisch schalen](#automatic-scaling-interval) is ingesteld op 30 minuten. In deze en de andere C# fragmenten in dit artikel is een `myBatchClient` correct geïnitialiseerd exemplaar van de [BatchClient][net_batchclient] -klasse.

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool(
                    poolId: "mypool",
                    virtualMachineSize: "standard_d1_v2",
                    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));    
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicatedNodes = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
await pool.CommitAsync();
```

> [!IMPORTANT]
> Wanneer u een groep met automatisch schalen maakt, moet u de para meter _targetDedicatedNodes_ of de para meter _targetLowPriorityNodes_ niet opgeven voor de aanroep van **CreatePool**. Geef in plaats daarvan de eigenschappen **AutoScaleEnabled** en **AutoScaleFormula** op voor de groep. De waarden voor deze eigenschappen bepalen het doel nummer van elk type knoop punt. Als u de grootte van een groep met automatisch schalen hand matig wilt wijzigen (bijvoorbeeld met [BatchClient. pool Operations. ResizePoolAsync][net_poolops_resizepoolasync]), **schakelt** u eerst automatische schaling op de groep uit en vervolgens wijzigt u de grootte.
>
>

Naast batch .NET kunt u gebruikmaken van een van de andere batch- [sdk's](batch-apis-tools.md#azure-accounts-for-batch-development), [batch rest](https://docs.microsoft.com/rest/api/batchservice/), [batch Power shell](batch-powershell-cmdlets-get-started.md)-cmdlets en de [batch-cli](batch-cli-get-started.md) voor het configureren van automatisch schalen.


### <a name="automatic-scaling-interval"></a>Interval voor automatisch schalen
De batch-service past standaard elke 15 minuten de grootte van een pool aan op basis van de automatisch geschaalde formule. Dit interval kan worden geconfigureerd met behulp van de volgende groeps eigenschappen:

* [CloudPool. AutoScaleEvaluationInterval][net_cloudpool_autoscaleevalinterval] (batch .net)
* [autoScaleEvaluationInterval][rest_autoscaleinterval] (REST API)

Het minimale interval is vijf minuten en de maximum waarde is 168 uur. Als er een interval buiten dit bereik is opgegeven, retourneert de batch-service een ongeldige aanvraag (400).

> [!NOTE]
> Automatisch schalen is momenteel niet bedoeld om te reageren op wijzigingen in minder dan een minuut, maar is bedoeld om de grootte van de pool geleidelijk aan te passen wanneer u een werk belasting uitvoert.
>
>

## <a name="enable-autoscaling-on-an-existing-pool"></a>Automatisch schalen inschakelen voor een bestaande groep

Elke batch-SDK biedt een manier om automatisch schalen in te scha kelen. Bijvoorbeeld:

* [BatchClient. pool Operations. EnableAutoScaleAsync][net_enableautoscaleasync] (batch .net)
* [Automatisch schalen inschakelen voor een groep][rest_enableautoscale] (REST API)

Houd bij het inschakelen van automatisch schalen op een bestaande groep de volgende punten in acht:

* Als automatisch schalen op dit moment is uitgeschakeld voor de groep wanneer u de aanvraag verzendt om automatisch schalen in te scha kelen, moet u een geldige formule voor automatisch schalen opgeven wanneer u de aanvraag verzendt. U kunt optioneel een evaluatie-interval voor automatisch schalen opgeven. Als u geen interval opgeeft, wordt de standaard waarde van 15 minuten gebruikt.
* Als automatisch schalen momenteel is ingeschakeld voor de groep, kunt u een formule voor automatisch schalen, een evaluatie-interval of beide opgeven. U moet ten minste één van deze eigenschappen opgeven.

  * Als u een nieuw evaluatie-interval voor automatisch schalen opgeeft, wordt het bestaande evaluatie schema gestopt en wordt er een nieuw schema gestart. De begin tijd van de nieuwe planning is het tijdstip waarop de aanvraag om automatisch schalen in te scha kelen is uitgegeven.
  * Als u de formule voor automatisch schalen of het evaluatie-interval weglaat, blijft de batch-service de huidige waarde van deze instelling gebruiken.

> [!NOTE]
> Als u waarden voor de para meters *targetDedicatedNodes* of *targetLowPriorityNodes* van de methode **CreatePool** hebt opgegeven bij het maken van de groep in .net, of voor de vergelijk bare para meters in een andere taal, worden deze waarden wordt genegeerd wanneer de formule voor automatisch schalen wordt geëvalueerd.
>
>

Dit C# code fragment maakt gebruik van de [batch .net][net_api] -bibliotheek om automatisch schalen in te scha kelen op een bestaande groep:

```csharp
// Define the autoscaling formula. This formula sets the target number of nodes
// to 5 on Mondays, and 1 on every other day of the week
string myAutoScaleFormula = "$TargetDedicatedNodes = (time().weekday == 1 ? 5:1);";

// Set the autoscale formula on the existing pool
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a>Een formule voor automatisch schalen bijwerken

Als u de formule wilt bijwerken op basis van een bestaande groep die automatisch kan worden geschaald, roept u de bewerking aan om automatisch schalen weer in te scha kelen met de nieuwe formule. Als automatisch schalen bijvoorbeeld al is ingeschakeld op `myexistingpool` wanneer de volgende .net-code wordt uitgevoerd, wordt de formule voor automatisch schalen vervangen door de inhoud van. `myNewFormula`

```csharp
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-the-autoscale-interval"></a>Het interval voor automatisch schalen bijwerken

Als u het interval voor automatisch schalen wilt bijwerken van een bestaande pool met automatisch schalen, roept u de bewerking aan zodat automatisch schalen opnieuw met het nieuwe interval kan worden ingeschakeld. Als u het evaluatie-interval voor automatisch schalen bijvoorbeeld wilt instellen op 60 minuten voor een pool die al automatisch wordt geschaald in .NET, doet u het volgende:

```csharp
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a>Een formule voor automatisch schalen evalueren

U kunt een Formule evalueren voordat u deze toepast op een groep. Op deze manier kunt u de formule testen om te zien hoe de instructies worden geëvalueerd voordat u de formule in productie neemt.

Als u een formule voor automatisch schalen wilt evalueren, moet u eerst automatische schaling op de groep met een geldige formule inschakelen. Als u een formule wilt testen voor een pool waarvoor automatisch schalen nog niet is ingeschakeld, gebruikt u de formule `$TargetDedicatedNodes = 0` met één regel wanneer u automatische schaling voor het eerst inschakelt. Gebruik vervolgens een van de volgende opties om de formule te evalueren die u wilt testen:

* [BatchClient. pool Operations. EvaluateAutoScale](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscale) of [EvaluateAutoScaleAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscaleasync)

    Deze batch .NET-methoden vereisen de ID van een bestaande groep en een teken reeks met de formule voor automatisch schalen die moet worden geëvalueerd.

* [Een formule voor automatisch schalen evalueren](https://docs.microsoft.com/rest/api/batchservice/evaluate-an-automatic-scaling-formula)

    Geef in deze REST API aanvraag de groeps-ID op in de URI en de formule voor automatisch schalen in het element *autoScaleFormula* van de hoofd tekst van de aanvraag. Het antwoord van de bewerking bevat de fout informatie die kan worden gerelateerd aan de formule.

In dit [batch][net_api] code fragment van .net wordt een formule voor automatisch schalen geëvalueerd. Als voor de pool niet automatisch schalen is ingeschakeld, worden deze eerst geactiveerd.

```csharp
// First obtain a reference to an existing pool
CloudPool pool = await batchClient.PoolOperations.GetPoolAsync("myExistingPool");

// If autoscaling isn't already enabled on the pool, enable it.
// You can't evaluate an autoscale formula on non-autoscale-enabled pool.
if (pool.AutoScaleEnabled == false)
{
    // We need a valid autoscale formula to enable autoscaling on the
    // pool. This formula is valid, but won't resize the pool:
    await pool.EnableAutoScaleAsync(
        autoscaleFormula: "$TargetDedicatedNodes = $CurrentDedicatedNodes;",
        autoscaleEvaluationInterval: TimeSpan.FromMinutes(5));

    // Batch limits EnableAutoScaleAsync calls to once every 30 seconds.
    // Because we want to apply our new autoscale formula below if it
    // evaluates successfully, and we *just* enabled autoscaling on
    // this pool, we pause here to ensure we pass that threshold.
    Thread.Sleep(TimeSpan.FromSeconds(31));

    // Refresh the properties of the pool so that we've got the
    // latest value for AutoScaleEnabled
    await pool.RefreshAsync();
}

// We must ensure that autoscaling is enabled on the pool prior to
// evaluating a formula
if (pool.AutoScaleEnabled == true)
{
    // The formula to evaluate - adjusts target number of nodes based on
    // day of week and time of day
    string myFormula = @"
        $curTime = time();
        $workHours = $curTime.hour >= 8 && $curTime.hour < 18;
        $isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
        $isWorkingWeekdayHour = $workHours && $isWeekday;
        $TargetDedicatedNodes = $isWorkingWeekdayHour ? 20:10;
    ";

    // Perform the autoscale formula evaluation. Note that this code does not
    // actually apply the formula to the pool.
    AutoScaleRun eval =
        await batchClient.PoolOperations.EvaluateAutoScaleAsync(pool.Id, myFormula);

    if (eval.Error == null)
    {
        // Evaluation success - print the results of the AutoScaleRun.
        // This will display the values of each variable as evaluated by the
        // autoscale formula.
        Console.WriteLine("AutoScaleRun.Results: " +
            eval.Results.Replace("$", "\n    $"));

        // Apply the formula to the pool since it evaluated successfully
        await batchClient.PoolOperations.EnableAutoScaleAsync(pool.Id, myFormula);
    }
    else
    {
        // Evaluation failed, output the message associated with the error
        Console.WriteLine("AutoScaleRun.Error.Message: " +
            eval.Error.Message);
    }
}
```

Een geslaagde evaluatie van de formule die in dit code fragment wordt weer gegeven, produceert de resultaten die vergelijkbaar zijn met:

```
AutoScaleRun.Results:
    $TargetDedicatedNodes=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a>Informatie over automatische schaal uitvoeringen ophalen

Om ervoor te zorgen dat uw formule op de verwachte manier wordt uitgevoerd, raden we u aan om regel matig de resultaten van de automatisch schalen uit te voeren die door batch worden uitgevoerd op uw pool. Als u dit wilt doen, kunt u een verwijzing naar de groep ophalen (of vernieuwen) en de eigenschappen van de laatste automatische schaal uitvoering bekijken.

In batch .NET heeft de eigenschap [CloudPool. AutoScaleRun](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscalerun) diverse eigenschappen die informatie geven over de meest recente automatische schaal aanpassing die voor de groep wordt uitgevoerd:

* [AutoScaleRun.Timestamp](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.timestamp)
* [AutoScaleRun.Results](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.results)
* [AutoScaleRun.Error](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.error)

In de REST API wordt met de aanvraag [informatie over een groep ophalen](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool) informatie over de groep geretourneerd, waaronder de meest recente informatie over automatisch schalen in de eigenschap [autoScaleRun](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool) .

In het C# volgende code fragment wordt de batch .net-bibliotheek gebruikt om informatie af te drukken over de laatste automatische schaal aanpassing van de groep _myPool_:

```csharp
await Cloud pool = myBatchClient.PoolOperations.GetPoolAsync("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

Voorbeeld uitvoer van het voor gaande fragment:

```
Last execution: 10/14/2016 18:36:43
Result:
  $TargetDedicatedNodes=10;
  $NodeDeallocationOption=requeue;
  $curTime=2016-10-14T18:36:43.282Z;
  $isWeekday=1;
  $isWorkingWeekdayHour=0;
  $workHours=0
Error:
```

## <a name="example-autoscale-formulas"></a>Voor beeld van formules voor automatisch schalen
Laten we eens kijken naar een paar formules waarin verschillende manieren worden weer gegeven om de hoeveelheid reken resources in een pool aan te passen.

### <a name="example-1-time-based-adjustment"></a>Voorbeeld 1: Op tijd gebaseerde aanpassing
Stel dat u de pool grootte wilt aanpassen op basis van de dag van de week en het tijdstip van de dag. In dit voor beeld ziet u hoe u het aantal knoop punten in de pool dienovereenkomstig verhoogt of verlaagt.

De formule verkrijgt eerst de huidige tijd. Als het een weekdag is (1-5) en binnen werk uren (8 uur tot 6 uur), wordt de grootte van de doel groep ingesteld op 20 knoop punten. Anders wordt deze ingesteld op 10 knoop punten.

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicatedNodes = $isWorkingWeekdayHour ? 20:10;
```

### <a name="example-2-task-based-adjustment"></a>Voorbeeld 2: Aanpassing op basis van een taak
In dit voor beeld wordt de pool grootte aangepast op basis van het aantal taken in de wachtrij. Zowel opmerkingen als regel einden zijn acceptabel in formule teken reeksen.

```csharp
// Get pending tasks for the past 15 minutes.
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have fewer than 70 percent data points, we use the last sample point,
// otherwise we use the maximum of last sample point and the history average.
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1), avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM to pending tasks, otherwise
// half of current dedicated.
$targetVMs = $tasks > 0? $tasks:max(0, $TargetDedicatedNodes/2);
// The pool size is capped at 20, if target VM value is more than that, set it
// to 20. This value should be adjusted according to your use case.
$TargetDedicatedNodes = max(0, min($targetVMs, 20));
// Set node deallocation mode - keep nodes active only until tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-3-accounting-for-parallel-tasks"></a>Voorbeeld 3: Accounting voor parallelle taken
In dit voor beeld wordt de pool grootte aangepast op basis van het aantal taken. Deze formule houdt ook rekening met de [MaxTasksPerComputeNode][net_maxtasks] -waarde die is ingesteld voor de groep. Deze aanpak is nuttig in situaties waarin het [uitvoeren van parallelle taken](batch-parallel-node-tasks.md) is ingeschakeld voor uw pool.

```csharp
// Determine whether 70 percent of the samples have been recorded in the past
// 15 minutes; if not, use last sample
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set the number of nodes to add to one-fourth the number of active tasks (the
// MaxTasksPerComputeNode property on this pool is set to 4, adjust this number
// for your use case)
$cores = $TargetDedicatedNodes * 4;
$extraVMs = (($tasks - $cores) + 3) / 4;
$targetVMs = ($TargetDedicatedNodes + $extraVMs);
// Attempt to grow the number of compute nodes to match the number of active
// tasks, with a maximum of 3
$TargetDedicatedNodes = max(0,min($targetVMs,3));
// Keep the nodes active until the tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-4-setting-an-initial-pool-size"></a>Voor beeld 4: Een initiële pool grootte instellen
In dit voor beeld C# ziet u een code fragment met een formule voor automatisch schalen waarmee de pool grootte wordt ingesteld op een opgegeven aantal knoop punten voor een eerste periode. Vervolgens wordt de pool grootte aangepast op basis van het aantal actieve en actieve taken nadat de eerste tijds periode is verstreken.

De formule in het volgende code fragment:

* Hiermee stelt u de aanvankelijke pool grootte in op vier knoop punten.
* Past de pool grootte niet aan binnen de eerste tien minuten van de levens cyclus van de pool.
* Na 10 minuten wordt de maximum waarde opgehaald van het aantal actieve en actieve taken in de afgelopen 60 minuten.
  * Als beide waarden 0 zijn (geeft aan dat er in de afgelopen 60 minuten geen taken werden uitgevoerd of actief zijn), wordt de groeps grootte ingesteld op 0.
  * Als een van beide waarden groter is dan nul, wordt er geen wijziging aangebracht.

```csharp
string now = DateTime.UtcNow.ToString("r");
string formula = string.Format(@"
    $TargetDedicatedNodes = {1};
    lifespan         = time() - time(""{0}"");
    span             = TimeInterval_Minute * 60;
    startup          = TimeInterval_Minute * 10;
    ratio            = 50;

    $TargetDedicatedNodes = (lifespan > startup ? (max($RunningTasks.GetSample(span, ratio), $ActiveTasks.GetSample(span, ratio)) == 0 ? 0 : $TargetDedicatedNodes) : {1});
    ", now, 4);
```

## <a name="next-steps"></a>Volgende stappen
* [Maximaliseer Azure batch reken resource gebruik met gelijktijdige knooppunt taken](batch-parallel-node-tasks.md) bevat details over hoe u meerdere taken tegelijkertijd kunt uitvoeren op de reken knooppunten in uw pool. Naast automatisch schalen kunt u met deze functie de duur van taken voor sommige workloads verlagen, waardoor u geld bespaart.
* Voor een andere efficiëntie Booster kunt u ervoor zorgen dat uw batch-toepassing op de meest optimale manier een query uitvoert op de batch-service. Zie [een efficiënte query uitvoeren op de Azure batch-service](batch-efficient-list-queries.md) voor meer informatie over het beperken van de hoeveelheid gegevens die de kabel kruist wanneer u de status van mogelijk duizenden reken knooppunten of taken doorzoekt.

[net_api]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch
[net_batchclient]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.batchclient
[net_cloudpool_autoscaleformula]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleformula
[net_cloudpool_autoscaleevalinterval]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval
[net_enableautoscaleasync]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.enableautoscaleasync
[net_maxtasks]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.maxtaskspercomputenode
[net_poolops_resizepoolasync]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.resizepoolasync

[rest_api]: https://docs.microsoft.com/rest/api/batchservice/
[rest_autoscaleformula]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
[rest_autoscaleinterval]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
[rest_enableautoscale]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
