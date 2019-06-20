---
title: Functies van de expressie in de gegevensstroom Mapping-functie van Azure Data Factory
description: Meer informatie over functies in de gegevensstroom toewijzen.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/15/2019
ms.openlocfilehash: 12a97e9f355ca1c71e926433e5eafd8b960a87da
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203220"
---
# <a name="data-transformation-expressions-in-mapping-data-flow"></a>Data transformation expressies in de gegevensstroom toewijzen 

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

## <a name="expression-functions"></a>Expressie functies

In Data Factory, gebruikt u de expressietaal van de functie voor toewijzing gegevensstroom gegevenstransformaties configureren.

---
### <code>abs</code>
<code><b>abs(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Positieve absolute waarde van de combinatie van twee getallen.
* ``abs(-20) -> 20``
* ``abs(10) -> 10``
---
### <code>acos</code>
<code><b>acos(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Berekent de omgekeerde waarde van een cosinus * ``acos(1) -> 0.0``
---
### <code>add</code>
<code><b>add(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Voegt twee tekenreeksen of getallen. Voegt een datum op een aantal dagen. Voegt één matrix voor hetzelfde type naar een andere. Hetzelfde als de operator + * ``add(10, 20) -> 30``
* ``10 + 20 -> 30``
* ``add('ice', 'cream') -> 'icecream'``
* ``'ice' + 'cream' + ' cone' -> 'icecream cone'``
* ``add(toDate('2012-12-12'), 3) -> 2012-12-15 (date value)``
* ``toDate('2012-12-12') + 3 -> 2012-12-15 (date value)``
* ``[10, 20] + [30, 40] => [10, 20, 30, 40]``
---
### <code>addDays</code>
<code><b>addDays(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;days to add&gt;</i> : integral) => datetime</b></code><br/><br/>
Dagen toevoegen aan een datum of tijdstempel. Hetzelfde als de operator voor datum + * ``addDays(toDate('2016-08-08'), 1) -> 2016-08-09``
---
### <code>addMonths</code>
<code><b>addMonths(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;months to add&gt;</i> : integral) => datetime</b></code><br/><br/>
Maanden toevoegen aan een datum of tijdstempel * ``addMonths(toDate('2016-08-31'), 1) -> 2016-09-30``
* ``addMonths(toTimestamp('2016-09-30 10:10:10'), -1) -> 2016-08-31 10:10:10``
---
### <code>and</code>
<code><b>and(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Logische AND-operator. Hetzelfde als & & * ``and(true, false) -> false``
* ``true && false -> false``
---
### <code>asin</code>
<code><b>asin(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Berekent een waarde inverse sinus * ``asin(0) -> 0.0``
---
### <code>atan</code>
<code><b>atan(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Een inverse tangens waarde wordt berekend * ``atan(0) -> 0.0``
---
### <code>atan2</code>
<code><b>atan2(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Retourneert de hoek in radialen tussen de positieve x-as van een beheerlaag en de opgegeven door de coördinaten * ``atan2(0, 0) -> 0.0``
---
### <code>avg</code>
<code><b>avg(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Met deze eigenschap wordt het gemiddelde van waarden van een kolom * ``avg(sales) -> 7523420.234``
---
### <code>avgIf</code>
<code><b>avgIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Op basis van een criteria wordt het gemiddelde van waarden van een kolom * ``avgIf(region == 'West', sales) -> 7523420.234``
---
### <code>byName</code>
<code><b>byName(<i>&lt;column name&gt;</i> : string) => any</b></code><br/><br/>
Hiermee selecteert een waarde in de kolom met de naam in de stroom. Als er meerdere overeenkomsten, wordt de eerste overeenkomst die wordt geretourneerd. Als er geen overeenkomst wordt er een NULL-waarde. De geretourneerde waarde bevat type omgezet door een van de functies voor typeconversie (TO_DATE, TO_STRING...).  Kolomnamen bekend tijdens de ontwerpfase moeten worden opgelost door hun naam. Berekende invoer worden niet ondersteund, maar kunt u vervangingen voor parameter * ``toString(byName('parent')) -> appa``
* ``toLong(byName('income')) -> 9000000000009``
* ``toBoolean(byName('foster')) -> false``
* ``toLong(byName($debtCol)) -> 123456890``
* ``birthDate -> 12/31/2050``
* ``toString(byName('Bogus Column')) -> NULL``
---
### <code>byPosition</code>
<code><b>byPosition(<i>&lt;position&gt;</i> : integer) => any</b></code><br/><br/>
Een waarde in de kolom met de relatieve positie (1 op basis van) selecteert in de stroom. Als de positie buiten het bereik is wordt een nullwaarde geretourneerd. De geretourneerde waarde is moet type omgezet door een van de functies voor typeconversie (TO_DATE, TO_STRING...) Berekende invoer worden niet ondersteund, maar kunt u vervangingen voor parameter * ``toString(byPosition(1)) -> amma``
* ``toDecimal(byPosition(2), 10, 2) -> 199990.99``
* ``toBoolean(byName(4)) -> false``
* ``toString(byName($colName)) -> family``
* ``toString(byPosition(1234)) -> NULL``
---
### <code>case</code>
<code><b>case(<i>&lt;condition&gt;</i> : boolean, <i>&lt;true_expression&gt;</i> : any, <i>&lt;false_expression&gt;</i> : any, ...) => any</b></code><br/><br/>
Op basis van voorwaarden afwisselende geldt één waarde of de andere. Als het aantal invoerbewerkingen zelfs, is de andere NULL voor de laatste voorwaarde * ``case(custType == 'Premium', 10, 4.5)``
* ``case(custType == 'Premium', price*0.95, custType == 'Elite',   price*0.9, price*2)``
* ``case(dayOfWeek(saleDate) == 1, 'Sunday', dayOfWeek(saleDate) == 6, 'Saturday')``
---
### <code>cbrt</code>
<code><b>cbrt(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
De hoofdmap van de kubus van een getal berekenen * ``cbrt(8) -> 2.0``
---
### <code>ceil</code>
<code><b>ceil(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Retourneert de kleinste integer die niet kleiner is dan het aantal * ``ceil(-0.1) -> 0``
---
### <code>concat</code>
<code><b>concat(<i>&lt;this&gt;</i> : string, <i>&lt;that&gt;</i> : string, ...) => string</b></code><br/><br/>
Worden samengevoegd in een variabele aantal tekenreeksen samen. Hetzelfde als de operator met tekenreeksen + * ``concat('Awesome', 'Cool', 'Product') -> 'AwesomeCoolProduct'``
* ``'Awesome' + 'Cool' + 'Product' -> 'AwesomeCoolProduct'``
* ``concat(addrLine1, ' ', addrLine2, ' ', city, ' ', state, ' ', zip)``
* ``addrLine1 + ' ' + addrLine2 + ' ' + city + ' ' + state + ' ' + zip``
---
### <code>concatWS</code>
<code><b>concatWS(<i>&lt;separator&gt;</i> : string, <i>&lt;this&gt;</i> : string, <i>&lt;that&gt;</i> : string, ...) => string</b></code><br/><br/>
Worden samengevoegd in een variabele aantal tekenreeksen samen met een scheidingsteken. De eerste parameter is het scheidingsteken * ``concatWS(' ', 'Awesome', 'Cool', 'Product') -> 'Awesome Cool Product'``
* ``concatWS(' ' , addrLine1, addrLine2, city, state, zip) ->``
* ``concatWS(',' , toString(order_total), toString(order_discount))``
---
### <code>cos</code>
<code><b>cos(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Berekent een cosinuswaarde * ``cos(10) -> -0.83907152907``
---
### <code>cosh</code>
<code><b>cosh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Een hyperbolische cosinus van een waarde berekend * ``cosh(0) -> 1.0``
---
### <code>count</code>
<code><b>count([<i>&lt;value1&gt;</i> : any]) => long</b></code><br/><br/>
Hiermee haalt u het totale aantal waarden. Als de optionele kolommen is opgegeven, NULL-waarden in de telling worden genegeerd * ``count(custId) -> 100``
* ``count(custId, custName) -> 50``
* ``count() -> 125``
* ``count(iif(isNull(custId), 1, NULL)) -> 5``
---
### <code>countDistinct</code>
<code><b>countDistinct(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : any], ...) => long</b></code><br/><br/>
Het totale aantal afzonderlijke waarden van een set kolommen opgehaald * ``countDistinct(custId, custName) -> 60``
---
### <code>countIf</code>
<code><b>countIf(<i>&lt;value1&gt;</i> : boolean, [<i>&lt;value2&gt;</i> : any]) => long</b></code><br/><br/>
Op basis van een criteria wordt het totale aantal waarden. Als de optionele kolom is opgegeven, NULL-waarden in de telling worden genegeerd * ``countIf(state == 'CA' && commission < 10000, name) -> 100``
---
### <code>covariancePopulation</code>
<code><b>covariancePopulation(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
De populatiecovariantie tussen de twee kolommen opgehaald * ``covariancePopulation(sales, profit) -> 122.12``
---
### <code>covariancePopulationIf</code>
<code><b>covariancePopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number, <i>&lt;value3&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criteria, de populatiecovariantie van twee kolommen opgehaald * ``covariancePopulationIf(region == 'West', sales) -> 122.12``
---
### <code>covarianceSample</code>
<code><b>covarianceSample(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
De voorbeeld-covariantie van twee kolommen opgehaald * ``covarianceSample(sales, profit) -> 122.12``
---
### <code>covarianceSampleIf</code>
<code><b>covarianceSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number, <i>&lt;value3&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criteria, wordt de voorbeeld-covariantie van twee kolommen * ``covarianceSampleIf(region == 'West', sales, profit) -> 122.12``
---
### <code>crc32</code>
<code><b>crc32(<i>&lt;value1&gt;</i> : any, ...) => long</b></code><br/><br/>
Berekent de hash CRC32 van set kolommen met verschillende primitieve gegevenstypen gegeven een bitlengte die mag alleen bestaan uit van de waarden 0(256), 224, 256, 384, 512. Het kan worden gebruikt voor het berekenen van een vingerafdruk voor een rij * ``crc32(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 3630253689``
---
### <code>cumeDist</code>
<code><b>cumeDist() => integer</b></code><br/><br/>
De functie CumeDist berekent de positie van een waarde ten opzichte van alle waarden in de partitie. Het resultaat is het aantal rijen is voorgaande of gelijk zijn aan de huidige rij in de volgorde van de partitie die is gedeeld door het totale aantal rijen in de partitie venster. Eventuele tie-waarden in de volgorde worden geëvalueerd op dezelfde positie.
* ``cumeDist() -> 1``
---
### <code>currentDate</code>
<code><b>currentDate([<i>&lt;value1&gt;</i> : string]) => date</b></code><br/><br/>
Hiermee haalt u de huidige datum wanneer deze taak wordt gestart om uit te voeren. U kunt een optionele tijdzone in de vorm van 'GMT', 'PST', 'UTC', '-Amerika/Cayman' doorgeven. De lokale tijdzone wordt als standaardwaarde gebruikt.
* ``currentDate() -> 12-12-2030``
* ``currentDate('PST') -> 12-31-2050``
---
### <code>currentTimestamp</code>
<code><b>currentTimestamp() => timestamp</b></code><br/><br/>
De huidige tijdstempel wordt als de taak wordt gestart om uit te voeren met de lokale tijdzone * ``currentTimestamp() -> 12-12-2030T12:12:12``
---
### <code>currentUTC</code>
<code><b>currentUTC([<i>&lt;value1&gt;</i> : string]) => timestamp</b></code><br/><br/>
Hiermee haalt u de huidige de tijdstempel als UTC. U kunt een optionele tijdzone in de vorm van 'GMT', 'PST', 'UTC', '-Amerika/Cayman' doorgeven. Dit is standaard ingesteld op de huidige tijdzone * ``currentUTC() -> 12-12-2030T19:18:12``
* ``currentUTC('Asia/Seoul') -> 12-13-2030T11:18:12``
---
### <code>dayOfMonth</code>
<code><b>dayOfMonth(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Haalt de dag van de maand uitgaande van een datum * ``dayOfMonth(toDate('2018-06-08')) -> 08``
---
### <code>dayOfWeek</code>
<code><b>dayOfWeek(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Hiermee haalt u de dag van de week uitgaande van een datum. 1 - zondag, 2 - maandag..., 7: zaterdag * ``dayOfWeek(toDate('2018-06-08')) -> 7``
---
### <code>dayOfYear</code>
<code><b>dayOfYear(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Haalt de dag van het jaar uitgaande van een datum * ``dayOfYear(toDate('2016-04-09')) -> 100``
---
### <code>degrees</code>
<code><b>degrees(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Converteert radialen naar graden * ``degrees(3.141592653589793) -> 180``
---
### <code>denseRank</code>
<code><b>denseRank(<i>&lt;value1&gt;</i> : any, ...) => integer</b></code><br/><br/>
Berekent de positie van een waarde in een groep met waarden. Het resultaat is een plus het aantal rijen is voorgaande of gelijk zijn aan de huidige rij in de volgorde van de partitie. De waarden wordt niet produceren hiaten in de reeks. Compacte positie werkt zelfs wanneer de gegevens niet is gesorteerd en wijzigen in waarden zoekt * ``denseRank(salesQtr, salesAmt) -> 1``
---
### <code>divide</code>
<code><b>divide(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Deelt twee getallen. Hetzelfde als de / operator * ``divide(20, 10) -> 2``
* ``20 / 10 -> 2``
---
### <code>endsWith</code>
<code><b>endsWith(<i>&lt;string&gt;</i> : string, <i>&lt;substring to check&gt;</i> : string) => boolean</b></code><br/><br/>
Controleert of als de tekenreeks met de opgegeven tekenreeks eindigt * ``endsWith('great', 'eat') -> true``
---
### <code>equals</code>
<code><b>equals(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Vergelijking is gelijk aan operator. Hetzelfde als operator == * ``equals(12, 24) -> false``
* ``12==24 -> false``
* ``'bad'=='bad' -> true``
* ``'good'== NULL -> false``
* ``NULL===NULL -> false``
---
### <code>equalsIgnoreCase</code>
<code><b>equalsIgnoreCase(<i>&lt;value1&gt;</i> : string, <i>&lt;value2&gt;</i> : string) => boolean</b></code><br/><br/>
Vergelijking is gelijk aan de operator is niet hoofdlettergevoelig. Hetzelfde als <> = operator * ``'abc'<==>'abc' -> true``
* ``equalsIgnoreCase('abc', 'Abc') -> true``
---
### <code>factorial</code>
<code><b>factorial(<i>&lt;value1&gt;</i> : number) => long</b></code><br/><br/>
De faculteit van een getal berekenen * ``factorial(5) -> 120``
---
### <code>false</code>
<code><b>false() => boolean</b></code><br/><br/>
Retourneert altijd een waarde false. De functie syntax(false()) gebruiken als er een kolom met de naam 'false' * ``isDiscounted == false()``
* ``isDiscounted() == false``
---
### <code>first</code>
<code><b>first(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : boolean]) => any</b></code><br/><br/>
Hiermee haalt u de eerste waarde van de kolomgroep van een. Als de tweede parameter null-waarden is negeren wordt weggelaten, wordt ervan uitgegaan false * ``first(sales) -> 12233.23``
* ``first(sales, false) -> NULL``
---
### <code>floor</code>
<code><b>floor(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Retourneert de grootste integer die niet groter zijn dan het aantal * ``floor(-0.1) -> -1``
---
### <code>fromUTC</code>
<code><b>fromUTC(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => timestamp</b></code><br/><br/>
Zet het tijdstempel van UTC. U kunt eventueel de tijdzone in de vorm van 'GMT', 'PST', 'UTC', '-Amerika/Cayman' doorgeven. Dit is standaard ingesteld op de huidige tijdzone * ``fromUTC(currentTimeStamp()) -> 12-12-2030T19:18:12``
* ``fromUTC(currentTimeStamp(), 'Asia/Seoul') -> 12-13-2030T11:18:12``
---
### <code>greater</code>
<code><b>greater(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Vergelijking van meer operator. Hetzelfde als > operator * ``greater(12, 24) -> false``
* ``'abcd' > 'abc' -> true``
---
### <code>greaterOrEqual</code>
<code><b>greaterOrEqual(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Vergelijking van de groter is dan of gelijk zijn aan operator. Hetzelfde als > operator = * ``greaterOrEqual(12, 12) -> false``
* ``'abcd' >= 'abc' -> true``
---
### <code>greatest</code>
<code><b>greatest(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Retourneert de grootste waarde uit de lijst met waarden als invoer. Wordt null geretourneerd als alle invoer null is * ``greatest(10, 30, 15, 20) -> 30``
* ``greatest(toDate('12/12/2010'), toDate('12/12/2011'), toDate('12/12/2000')) -> '12/12/2011'``
---
### <code>hour</code>
<code><b>hour(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Hiermee wordt de uurwaarde van een tijdstempel. U kunt een optionele tijdzone in de vorm van 'GMT', 'PST', 'UTC', '-Amerika/Cayman' doorgeven. De lokale tijdzone wordt als standaardwaarde gebruikt.
* ``hour(toTimestamp('2009-07-30T12:58:59')) -> 12``
* ``hour(toTimestamp('2009-07-30T12:58:59'), 'PST') -> 12``
---
### <code>iif</code>
<code><b>iif(<i>&lt;condition&gt;</i> : boolean, <i>&lt;true_expression&gt;</i> : any, [<i>&lt;false_expression&gt;</i> : any]) => any</b></code><br/><br/>
Op basis van een voorwaarde van toepassing is één waarde of de andere. Andere is een niet nader omschreven worden beschouwd als NULL. Zowel de waarden moeten compatibel zijn (numerieke, tekenreeks...) * ``iif(custType == 'Premium', 10, 4.5)``
* ``iif(amount > 100, 'High')``
* ``iif(dayOfWeek(saleDate) == 6, 'Weekend', 'Weekday')``
---
### <code>in</code>
<code><b>in(<i>&lt;array of items&gt;</i> : array, <i>&lt;item to find&gt;</i> : any) => boolean</b></code><br/><br/>
Controleert of een item in de matrix * ``in([10, 20, 30], 10) -> true``
* ``in(['good', 'kid'], 'bad') -> false``
---
### <code>initCap</code>
<code><b>initCap(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Converteert de eerste letter van elk woord naar hoofdletters. Woorden die zijn geïdentificeerd als gescheiden door spaties * ``initCap('cool iceCREAM') -> 'Cool IceCREAM'``
---
### <code>instr</code>
<code><b>instr(<i>&lt;string&gt;</i> : string, <i>&lt;substring to find&gt;</i> : string) => integer</b></code><br/><br/>
Zoeken naar de positie (1 op basis van) van de subtekenreeks in een tekenreeks. 0 wordt geretourneerd als niet is gevonden * ``instr('great', 'eat') -> 3``
* ``instr('microsoft', 'o') -> 7``
* ``instr('good', 'bad') -> 0``
---
### <code>isDelete</code>
<code><b>isDelete([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Controleert of als de rij is gemarkeerd voor verwijderen. U kunt de index (1-indeling) van de stroom doorgeven voor transformaties nemen meer dan één invoerstroom. Standaardwaarde voor de stream-index is 1 * ``isDelete() -> true``
* ``isDelete(1) -> false``
---
### <code>isError</code>
<code><b>isError([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Hiermee wordt gecontroleerd of de rij is gemarkeerd als fout. U kunt de index (1-indeling) van de stroom doorgeven voor transformaties nemen meer dan één invoerstroom. Standaardwaarde voor de stream-index is 1 * ``isError() -> true``
* ``isError(1) -> false``
---
### <code>isIgnore</code>
<code><b>isIgnore([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Hiermee wordt gecontroleerd of de rij is gemarkeerd om te worden genegeerd. U kunt de index (1-indeling) van de stroom doorgeven voor transformaties nemen meer dan één invoerstroom. Standaardwaarde voor de stream-index is 1 * ``isIgnore() -> true``
* ``isIgnore(1) -> false``
---
### <code>isInsert</code>
<code><b>isInsert([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Controleert of als de rij is gemarkeerd voor invoegen. U kunt de index (1-indeling) van de stroom doorgeven voor transformaties nemen meer dan één invoerstroom. Standaardwaarde voor de stream-index is 1 * ``isInsert() -> true``
* ``isInsert(1) -> false``
---
### <code>isMatch</code>
<code><b>isMatch([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Controleert of als de rij is afgestemd op opzoeken. U kunt de index (1-indeling) van de stroom doorgeven voor transformaties nemen meer dan één invoerstroom. Standaardwaarde voor de stream-index is 1 * ``isMatch() -> true``
* ``isMatch(1) -> false``
---
### <code>isNull</code>
<code><b>isNull(<i>&lt;value1&gt;</i> : any) => boolean</b></code><br/><br/>
Controleert of de waarde NULL is * ``isNull(NULL()) -> true``
* ``isNull('') -> false'``
---
### <code>isUpdate</code>
<code><b>isUpdate([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Controleert of als de rij is gemarkeerd voor update. U kunt de index (1-indeling) van de stroom doorgeven voor transformaties nemen meer dan één invoerstroom. Standaardwaarde voor de stream-index is 1 * ``isUpdate() -> true``
* ``isUpdate(1) -> false``
---
### <code>kurtosis</code>
<code><b>kurtosis(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Haalt de kurtosis van een kolom * ``kurtosis(sales) -> 122.12``
---
### <code>kurtosisIf</code>
<code><b>kurtosisIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criteria, haalt de kurtosis van een kolom * ``kurtosisIf(region == 'West', sales) -> 122.12``
---
### <code>lag</code>
<code><b>lag(<i>&lt;value&gt;</i> : any, [<i>&lt;number of rows to look before&gt;</i> : number], [<i>&lt;default value&gt;</i> : any]) => any</b></code><br/><br/>
Hiermee haalt u de waarde van de eerste parameter geëvalueerd n rijen voor de huidige rij. De tweede parameter is het aantal rijen om terug te kijken en de standaardwaarde is 1. Als er niet zo veel rijen wordt waarde null geretourneerd als er een standaardwaarde is opgegeven * ``lag(amount, 2) -> 60``
* ``lag(amount, 2000, 100) -> 100``
---
### <code>last</code>
<code><b>last(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : boolean]) => any</b></code><br/><br/>
Hiermee haalt u de laatste waarde van de kolomgroep van een. Als de tweede parameter null-waarden is negeren wordt weggelaten, wordt ervan uitgegaan false * ``last(sales) -> 523.12``
* ``last(sales, false) -> NULL``
---
### <code>lastDayOfMonth</code>
<code><b>lastDayOfMonth(<i>&lt;value1&gt;</i> : datetime) => date</b></code><br/><br/>
Haalt de laatste datum van de maand uitgaande van een datum * ``lastDayOfMonth(toDate('2009-01-12')) -> 2009-01-31``
---
### <code>lead</code>
<code><b>lead(<i>&lt;value&gt;</i> : any, [<i>&lt;number of rows to look after&gt;</i> : number], [<i>&lt;default value&gt;</i> : any]) => any</b></code><br/><br/>
Hiermee haalt de waarde van de eerste parameter geëvalueerd n rijen na de huidige rij. De tweede parameter is het aantal rijen dat u wilt kijken ernaar uit en de standaardwaarde is 1. Als er niet zo veel rijen wordt waarde null geretourneerd als er een standaardwaarde is opgegeven * ``lead(amount, 2) -> 60``
* ``lead(amount, 2000, 100) -> 100``
---
### <code>least</code>
<code><b>least(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Vergelijking kleiner is dan of gelijk zijn operator. Hetzelfde als < = operator * ``least(10, 30, 15, 20) -> 10``
* ``least(toDate('12/12/2010'), toDate('12/12/2011'), toDate('12/12/2000')) -> '12/12/2000'``
---
### <code>left</code>
<code><b>left(<i>&lt;string to subset&gt;</i> : string, <i>&lt;number of characters&gt;</i> : integral) => string</b></code><br/><br/>
Haalt een subtekenreeks beginnen bij index 1 met het aantal tekens. Hetzelfde als de SUBTEKENREEKS (str, 1, n) * ``left('bojjus', 2) -> 'bo'``
* ``left('bojjus', 20) -> 'bojjus'``
---
### <code>length</code>
<code><b>length(<i>&lt;value1&gt;</i> : string) => integer</b></code><br/><br/>
Retourneert de lengte van de tekenreeks * ``length('kiddo') -> 5``
---
### <code>lesser</code>
<code><b>lesser(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
De vergelijking kleiner operator. Hetzelfde als < operator * ``lesser(12 < 24) -> true``
* ``'abcd' < 'abc' -> false``
---
### <code>lesserOrEqual</code>
<code><b>lesserOrEqual(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Vergelijking kleiner is dan of gelijk zijn operator. Hetzelfde als < = operator * ``lesserOrEqual(12, 12) -> true``
* ``'abcd' <= 'abc' -> false``
---
### <code>levenshtein</code>
<code><b>levenshtein(<i>&lt;from string&gt;</i> : string, <i>&lt;to string&gt;</i> : string) => integer</b></code><br/><br/>
Haalt de levenshtein afstand tussen de twee tekenreeksen * ``levenshtein('boys', 'girls') -> 4``
---
### <code>like</code>
<code><b>like(<i>&lt;string&gt;</i> : string, <i>&lt;pattern match&gt;</i> : string) => boolean</b></code><br/><br/>
Het patroon is een tekenreeks is die is letterlijk. De uitzonderingen zijn de volgende speciale tekens: _ komt overeen met Eén willekeurig teken in de invoer (vergelijkbaar met. in reguliere expressies posix) % komt overeen met nul of meer tekens in de invoer (vergelijkbaar met. * in posix reguliere expressies).
Het escape-teken ''. Als een escape-teken vóór een speciale symbool of een ander escapeteken, het volgende teken letterlijk. Het is ongeldig om een ander escapeteken.
* ``like('icecream', 'ice%') -> true``
---
### <code>locate</code>
<code><b>locate(<i>&lt;substring to find&gt;</i> : string, <i>&lt;string&gt;</i> : string, [<i>&lt;from index - 1-based&gt;</i> : integral]) => integer</b></code><br/><br/>
Zoeken naar de positie (1 op basis van) van de subtekenreeks in een tekenreeks die vanaf een bepaalde positie. Als u weglaat de positie vanaf het begin van de tekenreeks wordt geacht. 0 wordt geretourneerd als niet is gevonden * ``locate('eat', 'great') -> 3``
* ``locate('o', 'microsoft', 6) -> 7``
* ``locate('bad', 'good') -> 0``
---
### <code>log</code>
<code><b>log(<i>&lt;value1&gt;</i> : number, [<i>&lt;value2&gt;</i> : number]) => double</b></code><br/><br/>
Berekent de logaritmische waarde. Een optionele base kan worden opgegeven anders een getal euler als gebruikt * ``log(100, 10) -> 2``
---
### <code>log10</code>
<code><b>log10(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Berekent de logaritmische waarde op basis van base 10 * ``log10(100) -> 2``
---
### <code>lower</code>
<code><b>lower(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Een tekenreeks lowercases * ``lower('GunChus') -> 'gunchus'``
---
### <code>lpad</code>
<code><b>lpad(<i>&lt;string to pad&gt;</i> : string, <i>&lt;final padded length&gt;</i> : integral, <i>&lt;padding&gt;</i> : string) => string</b></code><br/><br/>
Links vult de tekenreeks die door de opgegeven opvulling totdat deze van een bepaalde periode is. Als de tekenreeks gelijk aan of groter is dan de lengte is, wordt dit wordt beschouwd als een niet-op * ``lpad('great', 10, '-') -> '-----great'`` 
* ``lpad('great', 4, '-') -> 'great'`` 
* '' lpad ('geweldige', 8, ' <>') -> ' <><great'``
---
### <code>ltrim</code>
<code><b>ltrim(<i>&lt;string to trim&gt;</i> : string, <i>&lt;trim characters&gt;</i> : string) => string</b></code><br/><br/>
Links verwijdert een reeks van toonaangevende tekens. Als tweede parameter opgegeven is, worden deze verwijderd uit witruimte. Anders verwijdert deze een teken dat is opgegeven in de tweede parameter * ``ltrim('!--!wor!ld!', '-!') -> 'wor!ld!'``
---
### <code>max</code>
<code><b>max(<i>&lt;value1&gt;</i> : any) => any</b></code><br/><br/>
Hiermee haalt u de maximale waarde van een kolom * ``MAX(sales) -> 12312131.12``
---
### <code>maxIf</code>
<code><b>maxIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Op basis van een criteria, wordt de maximale waarde van een kolom * ``maxIf(region == 'West', sales) -> 99999.56``
---
### <code>md5</code>
<code><b>md5(<i>&lt;value1&gt;</i> : any, ...) => string</b></code><br/><br/>
De MD5-samenvatting van de set kolommen van verschillende primitieve gegevenstypen berekent en retourneert een 32 hexadecimale tekenreeks. Het kan worden gebruikt voor het berekenen van een vingerafdruk voor een rij * ``md5(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 'c1527622a922c83665e49835e46350fe'``
---
### <code>mean</code>
<code><b>mean(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Hiermee haalt u het gemiddelde van waarden van een kolom. Hetzelfde als Gem. * ``mean(sales) -> 7523420.234``
---
### <code>meanIf</code>
<code><b>meanIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Op basis van een criteria wordt het gemiddelde van waarden van een kolom. Hetzelfde als avgIf * ``meanIf(region == 'West', sales) -> 7523420.234``
---
### <code>min</code>
<code><b>min(<i>&lt;value1&gt;</i> : any) => any</b></code><br/><br/>
Hiermee haalt u de minimale waarde van een kolom * ``min(sales) -> 00.01``
* ``min(orderDate) -> 12/12/2000``
---
### <code>minIf</code>
<code><b>minIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Op basis van een criteria, wordt de minimale waarde van een kolom * ``minIf(region == 'West', sales) -> 00.01``
---
### <code>minus</code>
<code><b>minus(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Trekt getallen. Afgetrokken van een datum aantal dagen. Hetzelfde als de - operator * ``minus(20, 10) -> 10``
* ``20 - 10 -> 10``
* ``minus(toDate('2012-12-15'), 3) -> 2012-12-12 (date value)``
* ``toDate('2012-12-15') - 3 -> 2012-12-13 (date value)``
---
### <code>minute</code>
<code><b>minute(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Hiermee wordt de minuut waarde van een tijdstempel wordt opgehaald. U kunt een optionele tijdzone in de vorm van 'GMT', 'PST', 'UTC', '-Amerika/Cayman' doorgeven. De lokale tijdzone wordt als standaardwaarde gebruikt.
* ``minute(toTimestamp('2009-07-30T12:58:59')) -> 58``
* ``minute(toTimestamp('2009-07-30T12:58:59', 'PST')) -> 58``
---
### <code>mod</code>
<code><b>mod(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Modulus van twee cijfers. Hetzelfde als de operator % * ``mod(20, 8) -> 4``
* ``20 % 8 -> 4``
---
### <code>month</code>
<code><b>month(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Hiermee haalt u de maandwaarde van een datum of tijdstempel * ``month(toDate('2012-8-8')) -> 8``
---
### <code>monthsBetween</code>
<code><b>monthsBetween(<i>&lt;from date/timestamp&gt;</i> : datetime, <i>&lt;to date/timestamp&gt;</i> : datetime, [<i>&lt;time zone&gt;</i> : boolean], [<i>&lt;value4&gt;</i> : string]) => double</b></code><br/><br/>
Haalt het aantal maanden tussen twee datesYou kunt doorgeven aan een optionele tijdzone in de vorm van 'GMT', 'PST', 'UTC', '-Amerika/Cayman'. De lokale tijdzone wordt als standaardwaarde gebruikt.
* ``monthsBetween(toDate('1997-02-28 10:30:00'), toDate('1996-10-30')) -> 3.94959677``
---
### <code>multiply</code>
<code><b>multiply(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Vermenigvuldigt twee getallen. Hetzelfde als de * operator * ``multiply(20, 10) -> 200``
* ``20 * 10 -> 200``
---
### <code>nTile</code>
<code><b>nTile([<i>&lt;value1&gt;</i> : integer]) => integer</b></code><br/><br/>
De functie NTile verdeelt de rijen voor elke partitie venster in `n` buckets, variërend van 1 tot en met maximaal `n`. Bucketwaarden kunnen variëren van maximaal 1. Als het aantal rijen in de partitie niet gelijkmatig in het aantal buckets verdelen is, zijn gedistribueerde één bucket, beginnend met de eerste bucket met de rest-waarden. De functie NTile is handig voor de berekening van tertiles, kwartielen deciles en andere algemene samenvattende statistieken. De functie berekent twee variabelen die tijdens de initialisatie: De grootte van een reguliere bucket heeft één extra rij toegevoegd. Beide variabelen zijn gebaseerd op de grootte van de huidige partitie. Tijdens de berekening van de functie houdt van het nummer van de huidige rij, het nummer van de huidige bucket en het nummer van de rij die de bucket (bucketThreshold) verandert. Wanneer de huidige rij nummer bereikt drempelwaarde bucket, de bucket-waarde wordt verhoogd met één en de drempelwaarde wordt verhoogd door de Bucketgrootte (plus één extra als de huidige bucket wordt aangevuld).
* ``nTile() -> 1``
* ``nTile(numOfBuckets) -> 1``
---
### <code>negate</code>
<code><b>negate(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Een getal wordt genegeerd. Hiermee schakelt u positieve getallen naar negatief en omgekeerd * ``negate(13) -> -13``
---
### <code>nextSequence</code>
<code><b>nextSequence() => long</b></code><br/><br/>
Retourneert de volgende unieke reeks. Het aantal opeenvolgende alleen binnen een partitie is en wordt voorafgegaan door de partitionId * ``nextSequence() -> 12313112``
---
### <code>normalize</code>
<code><b>normalize(<i>&lt;String to normalize&gt;</i> : string) => string</b></code><br/><br/>
De tekenreekswaarde naar een afzonderlijke accenten zijn niet toegestaan unicode-tekens normaliseren * ``normalize('boys') -> 'boys'``
---
### <code>not</code>
<code><b>not(<i>&lt;value1&gt;</i> : boolean) => boolean</b></code><br/><br/>
Logische negatie operator * ``not(true) -> false``
* ``not(premium)``
---
### <code>notEquals</code>
<code><b>notEquals(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Vergelijking is niet gelijk aan de operator. Hetzelfde als! operator = * ``12!=24 -> true``
* ``'abc'!='abc' -> false``
---
### <code>null</code>
<code><b>null() => null</b></code><br/><br/>
Retourneert een NULL-waarde. Gebruik de functie syntax(null()) als er een kolom met de naam 'null'. Elke bewerking die gebruikmaakt van leidt tot een null-waarde * ``custId = NULL (for derived field)``
* ``custId == NULL -> NULL``
* ``'nothing' + NULL -> NULL``
* ``10 * NULL -> NULL'``
* ``NULL == '' -> NULL'``
---
### <code>or</code>
<code><b>or(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Logische OR-operator. Hetzelfde als || * ``or(true, false) -> true``
* '' ' True ' || waar onwaar -> '' ---
### <code>pMod</code>
<code><b>pMod(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Positieve absolute waarde van de combinatie van twee getallen.
* ``pmod(-20, 8) -> 4``
---
### <code>power</code>
<code><b>power(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt een getal tot de macht van een andere * ``power(10, 2) -> 100``
---
### <code>rank</code>
<code><b>rank(<i>&lt;value1&gt;</i> : any, ...) => integer</b></code><br/><br/>
Berekent de positie van een waarde in een groep met waarden. Het resultaat is een plus het aantal rijen is voorgaande of gelijk zijn aan de huidige rij in de volgorde van de partitie. De waarden levert hiaten in de reeks. Rang werkt, zelfs wanneer gegevens worden niet gesorteerd en wijzigen in waarden zoekt * ``rank(salesQtr, salesAmt) -> 1``
---
### <code>regexExtract</code>
<code><b>regexExtract(<i>&lt;string&gt;</i> : string, <i>&lt;regex to find&gt;</i> : string, [<i>&lt;match group 1-based index&gt;</i> : integral]) => string</b></code><br/><br/>
Pak een overeenkomende subtekenreeks voor een bepaalde regex-patroon. De laatste parameter identificeert de match-groep en is standaard ingesteld op 1, als u dit weglaat. Gebruik '<regex>'(back quote) zodat deze overeenkomt met een tekenreeks zonder aanhalingstekens * ``regexExtract('Cost is between 600 and 800 dollars', '(\\d+) and (\\d+)', 2) -> '800'``
* ``regexExtract('Cost is between 600 and 800 dollars', `(\d+) and (\d+)`, 2) -> '800'``
---
### <code>regexMatch</code>
<code><b>regexMatch(<i>&lt;string&gt;</i> : string, <i>&lt;regex to match&gt;</i> : string) => boolean</b></code><br/><br/>
Hiermee wordt gecontroleerd of de tekenreeks die overeenkomt met de opgegeven regex-patroon. Gebruik '<regex>'(back quote) zodat deze overeenkomt met een tekenreeks zonder aanhalingstekens * ``regexMatch('200.50', '(\\d+).(\\d+)') -> true``
* ``regexMatch('200.50', `(\d+).(\d+)`) -> true``
---
### <code>regexReplace</code>
<code><b>regexReplace(<i>&lt;string&gt;</i> : string, <i>&lt;regex to find&gt;</i> : string, <i>&lt;substring to replace&gt;</i> : string) => string</b></code><br/><br/>
Vervang alle instanties van een regex-patroon met een andere subtekenreeks in de opgegeven tekenreeks gebruik '<regex>'(back quote) zodat deze overeenkomt met een tekenreeks zonder aanhalingstekens * ``regexReplace('100 and 200', '(\\d+)', 'bojjus') -> 'bojjus and bojjus'``
* ``regexReplace('100 and 200', `(\d+)`, 'gunchus') -> 'gunchus and gunchus'``
---
### <code>regexSplit</code>
<code><b>regexSplit(<i>&lt;string to split&gt;</i> : string, <i>&lt;regex expression&gt;</i> : string) => array</b></code><br/><br/>
Hiermee wordt een tekenreeks op basis van een scheidingsteken op basis van de reguliere expressie en retourneert een matrix met tekenreeksen * ``regexSplit('oneAtwoBthreeC', '[CAB]') -> ['one', 'two', 'three']``
* ``regexSplit('oneAtwoBthreeC', '[CAB]')[1] -> 'one'``
* ``regexSplit('oneAtwoBthreeC', '[CAB]')[0] -> NULL``
* ``regexSplit('oneAtwoBthreeC', '[CAB]')[20] -> NULL``
---
### <code>replace</code>
<code><b>replace(<i>&lt;string&gt;</i> : string, <i>&lt;substring to find&gt;</i> : string, <i>&lt;substring to replace&gt;</i> : string) => string</b></code><br/><br/>
Vervang alle instanties van een subtekenreeks met een andere subtekenreeks in de opgegeven tekenreeks * ``replace('doggie dog', 'dog', 'cat') -> 'catgie cat'``
* ``replace('doggie dog', 'dog', '') -> 'gie'``
---
### <code>reverse</code>
<code><b>reverse(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Een tekenreeks is omgekeerd * ``reverse('gunchus') -> 'suhcnug'``
---
### <code>right</code>
<code><b>right(<i>&lt;string to subset&gt;</i> : string, <i>&lt;number of characters&gt;</i> : integral) => string</b></code><br/><br/>
Een subtekenreeks met het aantal tekens vanaf de rechterkant. Hetzelfde als de SUBTEKENREEKS (str LENGTH(str) - n, n) * ``right('bojjus', 2) -> 'us'``
* ``right('bojjus', 20) -> 'bojjus'``
---
### <code>rlike</code>
<code><b>rlike(<i>&lt;string&gt;</i> : string, <i>&lt;pattern match&gt;</i> : string) => boolean</b></code><br/><br/>
Controleert of de tekenreeks die overeenkomt met de opgegeven regex-patroon * ``rlike('200.50', '(\d+).(\d+)') -> true``
---
### <code>round</code>
<code><b>round(<i>&lt;number&gt;</i> : number, [<i>&lt;scale to round&gt;</i> : number], [<i>&lt;rounding option&gt;</i> : integral]) => double</b></code><br/><br/>
Rondt een getal dat is een optionele schalen en een optionele afronding modus. Als de schaal wordt weggelaten, wordt deze standaard ingesteld op 0.  Als de modus wordt weggelaten, wordt deze standaard ingesteld op ROUND_HALF_UP(5). De waarden voor de afronding van bevatten 1 - 2 ROUND_UP - ROUND_DOWN 3 - 4 ROUND_CEILING - ROUND_FLOOR 5 - ROUND_HALF_UP 6 - 7 ROUND_HALF_DOWN - ROUND_HALF_EVEN 8 - ROUND_UNNECESSARY * ``round(100.123) -> 100.0``
* ``round(2.5, 0) -> 3.0``
* ``round(5.3999999999999995, 2, 7) -> 5.40``
---
### <code>rowNumber</code>
<code><b>rowNumber() => integer</b></code><br/><br/>
Een sequentiële rij nummeren voor rijen in een venster te beginnen met 1 toegewezen * ``rowNumber() -> 1``
---
### <code>rpad</code>
<code><b>rpad(<i>&lt;string to pad&gt;</i> : string, <i>&lt;final padded length&gt;</i> : integral, <i>&lt;padding&gt;</i> : string) => string</b></code><br/><br/>
Rechts vult de tekenreeks die door de opgegeven opvulling totdat deze van een bepaalde periode is. Als de tekenreeks gelijk aan of groter is dan de lengte is, wordt dit wordt beschouwd als een niet-op * ``rpad('great', 10, '-') -> 'great-----'`` 
* ``rpad('great', 4, '-') -> 'great'`` 
* ``rpad('great', 8, '<>') -> 'great<><'`` 
---
### <code>rtrim</code>rtrim</code>
<code><b>rtrim(<i>&lt;string to trim&gt;</i> : string, <i>&lt;trim characters&gt;</i> : string) => string</b></code><br/><br/>
Rechts verwijdert een reeks van toonaangevende tekens. Als tweede parameter opgegeven is, worden deze verwijderd uit witruimte. Anders verwijdert deze een teken dat is opgegeven in de tweede parameter * ``rtrim('!--!wor!ld!', '-!') -> '!--!wor!ld'``
---
### <code>second</code>
<code><b>second(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Hiermee haalt u de tweede waarde van een datum. U kunt een optionele tijdzone in de vorm van 'GMT', 'PST', 'UTC', '-Amerika/Cayman' doorgeven. De lokale tijdzone wordt als standaardwaarde gebruikt.
* ``second(toTimestamp('2009-07-30T12:58:59')) -> 59``
---
### <code>sha1</code>
<code><b>sha1(<i>&lt;value1&gt;</i> : any, ...) => string</b></code><br/><br/>
De SHA-1-samenvatting van de set kolommen van verschillende primitieve gegevenstypen berekent en retourneert een 40 hexadecimale tekenreeks. Het kan worden gebruikt voor het berekenen van een vingerafdruk voor een rij * ``sha1(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> '63849fd2abb65fbc626c60b1f827bd05573f0cea'``
---
### <code>sha2</code>
<code><b>sha2(<i>&lt;value1&gt;</i> : integer, <i>&lt;value2&gt;</i> : any, ...) => string</b></code><br/><br/>
Berekent de SHA-2-samenvatting van de set kolommen met verschillende primitieve gegevenstypen gegeven een bitlengte die mag alleen bestaan uit van de waarden 0(256), 224, 256, 384, 512. Het kan worden gebruikt voor het berekenen van een vingerafdruk voor een rij * ``sha2(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 'd3b2bff62c3a00e9370b1ac85e428e661a7df73959fa1a96ae136599e9ee20fd'``
---
### <code>sin</code>
<code><b>sin(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Berekent een sinuswaarde * ``sin(2) -> 0.90929742682``
---
### <code>sinh</code>
<code><b>sinh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
De waarde van een hyperbolische sinus berekend * ``sinh(0) -> 0.0``
---
### <code>skewness</code>
<code><b>skewness(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Haalt de asymmetrie van een kolom * ``skewness(sales) -> 122.12``
---
### <code>skewnessIf</code>
<code><b>skewnessIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criteria, haalt de asymmetrie van een kolom * ``skewnessIf(region == 'West', sales) -> 122.12``
---
### <code>slice</code>
<code><b>slice(<i>&lt;array to slice&gt;</i> : array, <i>&lt;from 1-based index&gt;</i> : integral, [<i>&lt;number of items&gt;</i> : integral]) => array</b></code><br/><br/>
Een subset van een matrix van een positie worden uitgepakt. Positie is 1 op basis van. Als de lengte wordt weggelaten, is dit standaard ingesteld op het einde van de tekenreeks * ``slice([10, 20, 30, 40], 1, 2) -> [10, 20]``
* ``slice([10, 20, 30, 40], 2) -> [20, 30, 40]``
* ``slice([10, 20, 30, 40], 2)[1] -> 20``
* ``slice([10, 20, 30, 40], 2)[0] -> NULL``
* ``slice([10, 20, 30, 40], 2)[20] -> NULL``
* ``slice([10, 20, 30, 40], 8) -> []``
---
### <code>soundex</code>
<code><b>soundex(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Hiermee haalt u de code geluid voor de tekenreeks * ``soundex('genius') -> 'G520'``
---
### <code>split</code>
<code><b>split(<i>&lt;string to split&gt;</i> : string, <i>&lt;split characters&gt;</i> : string) => array</b></code><br/><br/>
Hiermee wordt een tekenreeks op basis van een scheidingsteken en retourneert een matrix met tekenreeksen * ``split('100,200,300', ',') -> ['100', '200', '300']``
* ``split('100,200,300', '|') -> ['100,200,300']``
* ``split('100, 200, 300', ', ') -> ['100', '200', '300']``
* ``split('100, 200, 300', ', ')[1] -> '100'``
* ``split('100, 200, 300', ', ')[0] -> NULL``
* ``split('100, 200, 300', ', ')[20] -> NULL``
* ``split('100200300', ',') -> ['100200300']``
---
### <code>sqrt</code>
<code><b>sqrt(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Berekent de vierkantswortel van een getal * ``sqrt(9) -> 3``
---
### <code>startsWith</code>
<code><b>startsWith(<i>&lt;string&gt;</i> : string, <i>&lt;substring to check&gt;</i> : string) => boolean</b></code><br/><br/>
Controleert of de tekenreeks met de opgegeven tekenreeks begint * ``startsWith('great', 'gr') -> true``
---
### <code>stddev</code>
<code><b>stddev(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Haalt de standaardafwijking van een kolom * ``stdDev(sales) -> 122.12``
---
### <code>stddevIf</code>
<code><b>stddevIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criteria, haalt de standaardafwijking van een kolom * ``stddevIf(region == 'West', sales) -> 122.12``
---
### <code>stddevPopulation</code>
<code><b>stddevPopulation(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Haalt de standaardafwijking van een kolom * ``stddevPopulation(sales) -> 122.12``
---
### <code>stddevPopulationIf</code>
<code><b>stddevPopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criteria, haalt de standaardafwijking van een kolom * ``stddevPopulationIf(region == 'West', sales) -> 122.12``
---
### <code>stddevSample</code>
<code><b>stddevSample(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Haalt de standaardafwijking van voorbeeld van een kolom * ``stddevSample(sales) -> 122.12``
---
### <code>stddevSampleIf</code>
<code><b>stddevSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criteria, wordt de standaarddeviatie van voorbeeld van een kolom * ``stddevSampleIf(region == 'West', sales) -> 122.12``
---
### <code>subDays</code>
<code><b>subDays(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;days to subtract&gt;</i> : integral) => datetime</b></code><br/><br/>
Aftrekken maanden vanaf een datum. Hetzelfde als de - operator voor datum * ``subDays(toDate('2016-08-08'), 1) -> 2016-08-09``
---
### <code>subMonths</code>
<code><b>subMonths(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;months to subtract&gt;</i> : integral) => datetime</b></code><br/><br/>
Maanden vanaf de datum- of timestamp aftrekken * ``subMonths(toDate('2016-09-30'), 1) -> 2016-08-31``
---
### <code>substring</code>
<code><b>substring(<i>&lt;string to subset&gt;</i> : string, <i>&lt;from 1-based index&gt;</i> : integral, [<i>&lt;number of characters&gt;</i> : integral]) => string</b></code><br/><br/>
Een subtekenreeks van een bepaalde lengte van een positie. Positie is 1 op basis van. Als de lengte wordt weggelaten, is dit standaard ingesteld op het einde van de tekenreeks * ``substring('Cat in the hat', 5, 2) -> 'in'``
* ``substring('Cat in the hat', 5, 100) -> 'in the hat'``
* ``substring('Cat in the hat', 5) -> 'in the hat'``
* ``substring('Cat in the hat', 100, 100) -> ''``
---
### <code>sum</code>
<code><b>sum(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
De totale som van een numerieke kolom opgehaald * ``sum(col) -> value``
---
### <code>sumDistinct</code>
<code><b>sumDistinct(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Het totaal van afzonderlijke waarden van een numerieke kolom opgehaald * ``sumDistinct(col) -> value``
---
### <code>sumDistinctIf</code>
<code><b>sumDistinctIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Op basis van criteria haalt de totale som van een numerieke kolom. De voorwaarde kan worden gebaseerd op een willekeurige kolom * ``sumDistinctIf(state == 'CA' && commission < 10000, sales) -> value``
* ``sumDistinctIf(true, sales) -> SUM(sales)``
*********************************
### <code>sumIf</code>
<code><b>sumIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Op basis van criteria haalt de totale som van een numerieke kolom. De voorwaarde kan worden gebaseerd op een willekeurige kolom * ``sumIf(state == 'CA' && commission < 10000, sales) -> value``
* ``sumIf(true, sales) -> SUM(sales)``
*********************************
### <code>tan</code>
<code><b>tan(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Berekent een tangenswaarde * ``tan(0) -> 0.0``
---
### <code>tanh</code>
<code><b>tanh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Een hyperbolische tangens waarde wordt berekend * ``tanh(0) -> 0.0``
---
### <code>toBoolean</code>
<code><b>toBoolean(<i>&lt;value1&gt;</i> : string) => boolean</b></code><br/><br/>
Zet een waarde van ('t ', 'true', 'y', 'Ja', '1') op ' True ' en ('f', 'false',... ', 'Nee', '0') op onwaar en NULL voor een andere waarde * ``toBoolean('true') -> true``
* ``toBoolean('n') -> false``
* ``toBoolean('truthy') -> NULL``
---
### <code>toDate</code>
<code><b>toDate(<i>&lt;string&gt;</i> : any, [<i>&lt;date format&gt;</i> : string]) => date</b></code><br/><br/>
Converteert een tekenreeks naar een datum een optionele datumnotatie gegeven. Raadpleeg Java SimpleDateFormat voor alle mogelijke notaties. Als de datumnotatie wordt weggelaten, worden de combinaties van de volgende geaccepteerd. [ yyyy, yyyy-[M]M, yyyy-[M]M-[d]d, yyyy-[M]M-[d]d, yyyy-[M]M-[d]d, yyyy-[M]M-[d]dT* ] * ``toDate('2012-8-8') -> 2012-8-8``
* ``toDate('12/12/2012', 'MM/dd/yyyy') -> 2012-12-12``
---
### <code>toDecimal</code>
<code><b>toDecimal(<i>&lt;value&gt;</i> : any, [<i>&lt;precision&gt;</i> : integral], [<i>&lt;scale&gt;</i> : integral], [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => decimal(10,0)</b></code><br/><br/>
Een numerieke of tekenreeks wordt geconverteerd naar een decimale waarde. Als precision en scale niet opgegeven zijn, is dit standaard ingesteld op (10,2). Een optionele Java decimale notatie voor de conversie kan worden gebruikt. Een optionele landinstelling-indeling in de vorm van BCP47 taal, zoals en-US, de, zh-CN * ``toDecimal(123.45) -> 123.45``
* ``toDecimal('123.45', 8, 4) -> 123.4500``
* ``toDecimal('$123.45', 8, 4,'$###.00') -> 123.4500``
* ``toDecimal('Ç123,45', 10, 2, 'Ç###,##', 'de') -> 123.45``
---
### <code>toDouble</code>
<code><b>toDouble(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => double</b></code><br/><br/>
Een numerieke waarde of de tekenreeks wordt geconverteerd naar een double-waarde. Een optionele Java decimale notatie voor de conversie kan worden gebruikt. Een optionele landinstelling-indeling in de vorm van BCP47 taal, zoals en-US, de, zh-CN * ``toDouble(123.45) -> 123.45``
* ``toDouble('123.45') -> 123.45``
* ``toDouble('$123.45', '$###.00') -> 123.45``
* ``toDouble('Ç123,45', 'Ç###,##', 'de') -> 123.45``
---
### <code>toFloat</code>
<code><b>toFloat(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => float</b></code><br/><br/>
Een numerieke of tekenreeks wordt geconverteerd naar een drijvende-kommawaarde. Een optionele Java decimale notatie voor de conversie kan worden gebruikt. Kapt een getal met dubbele precisie * ``toFloat(123.45) -> 123.45``
* ``toFloat('123.45') -> 123.45``
* ``toFloat('$123.45', '$###.00') -> 123.45``
---
### <code>toInteger</code>
<code><b>toInteger(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => integer</b></code><br/><br/>
Een numerieke of tekenreeks wordt geconverteerd naar een geheel getal. Een optionele Java decimale notatie voor de conversie kan worden gebruikt. Kapt een lang, float dubbele * ``toInteger(123) -> 123``
* ``toInteger('123') -> 123``
* ``toInteger('$123', '$###') -> 123``
---
### <code>toLong</code>
<code><b>toLong(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => long</b></code><br/><br/>
Een numerieke of tekenreeks wordt geconverteerd naar een long-waarde. Een optionele Java decimale notatie voor de conversie kan worden gebruikt. Kapt een gegevenstype met drijvende komma, double * ``toLong(123) -> 123``
* ``toLong('123') -> 123``
* ``toLong('$123', '$###') -> 123``
---
### <code>toShort</code>
<code><b>toShort(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => short</b></code><br/><br/>
Converteert een numerieke waarde of een tekenreeks naar een korte waarde. Een optionele Java decimale notatie voor de conversie kan worden gebruikt. Kapt een geheel getal, lang, float, double * ``toShort(123) -> 123``
* ``toShort('123') -> 123``
* ``toShort('$123', '$###') -> 123``
---
### <code>toString</code>
<code><b>toString(<i>&lt;value&gt;</i> : any, [<i>&lt;number format/date format&gt;</i> : string]) => string</b></code><br/><br/>
Converteert een primitief gegevenstype naar een tekenreeks. Voor de getallen en datum kan een indeling die worden opgegeven. Als u niets opgeeft de systeemstandaard wordt opgehaald. Java decimale indeling wordt gebruikt voor getallen. Raadpleeg de Java-SimpleDateFormat voor alle mogelijke datumnotaties; de standaardwaarde is jjjj-MM-dd * ``toString(10) -> '10'``
* ``toString('engineer') -> 'engineer'``
* ``toString(123456.789, '##,###.##') -> '123,456.79'``
* ``toString(123.78, '000000.000') -> '000123.780'``
* ``toString(12345, '##0.#####E0') -> '12.345E3'``
* ``toString(toDate('2018-12-31')) -> '2018-12-31'``
* ``toString(toDate('2018-12-31'), 'MM/dd/yy') -> '12/31/18'``
* ``toString(4 == 20) -> 'false'``
---
### <code>toTimestamp</code>
<code><b>toTimestamp(<i>&lt;string&gt;</i> : any, [<i>&lt;timestamp format&gt;</i> : string], [<i>&lt;time zone&gt;</i> : string]) => timestamp</b></code><br/><br/>
Converteert een tekenreeks naar een datum een optionele tijdstempelnotatie gegeven. Raadpleeg Java SimpleDateFormat voor alle mogelijke notaties. Als de tijdstempel wordt weggelaten de standaard-patroon jjjj-[M] M-[d] d uu: mm: [. f...] wordt gebruikt * ``toTimestamp('2016-12-31 00:12:00') -> 2012-8-8T00:12:00``
* ``toTimestamp('2016/12/31T00:12:00', 'MM/dd/yyyyThh:mm:ss') -> 2012-12-12T00:12:00``
---
### <code>toUTC</code>
<code><b>toUTC(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => timestamp</b></code><br/><br/>
De tijdstempel converteert naar UTC. U kunt een optionele tijdzone in de vorm van 'GMT', 'PST', 'UTC', '-Amerika/Cayman' doorgeven. Dit is standaard ingesteld op de huidige tijdzone * ``toUTC(currentTimeStamp()) -> 12-12-2030T19:18:12``
* ``toUTC(currentTimeStamp(), 'Asia/Seoul') -> 12-13-2030T11:18:12``
---
### <code>translate</code>
<code><b>translate(<i>&lt;string to translate&gt;</i> : string, <i>&lt;lookup characters&gt;</i> : string, <i>&lt;replace characters&gt;</i> : string) => string</b></code><br/><br/>
Vervangen door een reeks tekens met een andere reeks tekens in de tekenreeks. Tekens zijn 1 tot en met 1-vervanging * ``translate('(Hello)', '()', '[]') -> '[Hello]'``
* ``translate('(Hello)', '()', '[') -> '[Hello'``
---
### <code>trim</code>
<code><b>trim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></code><br/><br/>
Verwijdert een tekenreeks voorloop- en volgspaties. Als tweede parameter opgegeven is, worden deze verwijderd uit witruimte. Anders verwijdert deze een teken dat is opgegeven in de tweede parameter * ``trim('!--!wor!ld!', '-!') -> 'wor!ld'``
---
### <code>true</code>
<code><b>true() => boolean</b></code><br/><br/>
Retourneert altijd een waarde true. De functie syntax(true()) gebruiken als er een kolom met de naam 'true' * ``isDiscounted == true()``
* ``isDiscounted() == true``
---
### <code>typeMatch</code>
<code><b>typeMatch(<i>&lt;type&gt;</i> : string, <i>&lt;base type&gt;</i> : string) => boolean</b></code><br/><br/>
Komt overeen met het type van de kolom. Kan alleen worden gebruikt in patroon expressions.number overeenkomt met korte, geheel getal, lang, double, drijvende komma of een decimaal, integraal komt overeen met een korte, geheel getal, lang, decimale komt overeen met dubbele, float, decimaal getal en datum/tijd komt overeen met datum- of timestamp type * ``typeMatch(type, 'number') -> true``
* ``typeMatch('date', 'number') -> false``
---
### <code>upper</code>
<code><b>upper(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Een tekenreeks uppercases * ``upper('bojjus') -> 'BOJJUS'``
---
### <code>variance</code>
<code><b>variance(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Haalt de variantie van een kolom * ``variance(sales) -> 122.12``
---
### <code>varianceIf</code>
<code><b>varianceIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criteria, haalt de variantie van een kolom * ``varianceIf(region == 'West', sales) -> 122.12``
---
### <code>variancePopulation</code>
<code><b>variancePopulation(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Haalt de populatieverschillen van een kolom * ``variancePopulation(sales) -> 122.12``
---
### <code>variancePopulationIf</code>
<code><b>variancePopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criteria, haalt de populatieverschillen van een kolom * ``variancePopulationIf(region == 'West', sales) -> 122.12``
---
### <code>varianceSample</code>
<code><b>varianceSample(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Haalt de zuivere variantie van een kolom * ``varianceSample(sales) -> 122.12``
---
### <code>varianceSampleIf</code>
<code><b>varianceSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criteria, haalt de zuivere variantie van een kolom * ``varianceSampleIf(region == 'West', sales) -> 122.12``
---
### <code>weekOfYear</code>
<code><b>weekOfYear(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Hiermee wordt opgehaald van de week van het jaar uitgaande van een datum * ``weekOfYear(toDate('2008-02-20')) -> 8``
---
### <code>xor</code>
<code><b>xor(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Logische XOR-operator. Hetzelfde als ^ operator * ``xor(true, false) -> true``
* ``xor(true, true) -> false``
* ``true ^ false -> true``
---
### <code>year</code>
<code><b>year(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Hiermee haalt u de jaarwaarde van een datum * ``year(toDate('2012-8-8')) -> 2012``

## <a name="next-steps"></a>Volgende stappen

[Informatie over het gebruik van de opbouwfunctie voor expressies](concepts-data-flow-expression-builder.md).
