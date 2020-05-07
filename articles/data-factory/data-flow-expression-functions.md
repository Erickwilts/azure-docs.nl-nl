---
title: Expressie functies in de stroom toewijzings gegevens
description: Meer informatie over expressie functies in het toewijzen van gegevens stroom.
author: kromerm
ms.author: makromer
manager: anandsub
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 02/15/2019
ms.openlocfilehash: 52f389e00d63f3659dfe79487b31ec9c3fab1ced
ms.sourcegitcommit: b9d4b8ace55818fcb8e3aa58d193c03c7f6aa4f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "82580695"
---
# <a name="data-transformation-expressions-in-mapping-data-flow"></a>Gegevens transformatie expressies in gegevens stroom toewijzen

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

## <a name="expression-functions"></a>Expressiefuncties

In Data Factory gebruikt u de expressie taal van de functie gegevens stroom toewijzen om gegevens transformaties te configureren.
___
### <code>abs</code>
<code><b>abs(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Absolute waarde van een getal.  
* ``abs(-20) -> 20``  
* ``abs(10) -> 10``  
___   
### <code>acos</code>
<code><b>acos(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt een inverse cosinus waarde berekend* ``acos(1) -> 0.0``  
___
### <code>add</code>
<code><b>add(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Voegt een paar teken reeksen of getallen toe. Voegt een datum toe aan een aantal dagen. Voegt een duur toe aan een tijds tempel. Voegt een matrix van hetzelfde type toe aan een andere. Hetzelfde als de operator +* ``add(10, 20) -> 30``  
* ``10 + 20 -> 30``  
* ``add('ice', 'cream') -> 'icecream'``  
* ``'ice' + 'cream' + ' cone' -> 'icecream cone'``  
* ``add(toDate('2012-12-12'), 3) -> toDate('2012-12-15')``  
* ``toDate('2012-12-12') + 3 -> toDate('2012-12-15')``  
* ``[10, 20] + [30, 40] -> [10, 20, 30, 40]``  
* ``toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS') + (days(1) + hours(2) - seconds(10)) -> toTimestamp('2019-02-04 07:19:18.871', 'yyyy-MM-dd HH:mm:ss.SSS')``  
___
### <code>addDays</code>
<code><b>addDays(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;days to add&gt;</i> : integral) => datetime</b></code><br/><br/>
Voeg dagen toe aan een datum of tijds tempel. Hetzelfde als de operator + voor datum* ``addDays(toDate('2016-08-08'), 1) -> toDate('2016-08-09')``  
___
### <code>addMonths</code>
<code><b>addMonths(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;months to add&gt;</i> : integral, [<i>&lt;value3&gt;</i> : string]) => datetime</b></code><br/><br/>
Voeg maanden toe aan een datum of tijds tempel. U kunt desgewenst een tijd zone door geven* ``addMonths(toDate('2016-08-31'), 1) -> toDate('2016-09-30')``  
* ``addMonths(toTimestamp('2016-09-30 10:10:10'), -1) -> toTimestamp('2016-08-31 10:10:10')``  
___
### <code>and</code>
<code><b>and(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Logische AND-operator. Hetzelfde als && * ``and(true, false) -> false``  
* ``true && false -> false``  
___
### <code>asin</code>
<code><b>asin(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt een inverse sinus waarde berekend* ``asin(0) -> 0.0``  
___
### <code>atan</code>
<code><b>atan(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt een inverse raaklijn waarde berekend* ``atan(0) -> 0.0``  
___
### <code>atan2</code>
<code><b>atan2(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Retourneert de hoek in radialen tussen de positieve x-as van een vlak en het punt dat door de coördinaten wordt gegeven* ``atan2(0, 0) -> 0.0``  
___
### <code>byName</code>
<code><b>byName(<i>&lt;column name&gt;</i> : string, [<i>&lt;stream name&gt;</i> : string]) => any</b></code><br/><br/>
Selecteert een kolom waarde op naam in de stroom. U kunt een optionele naam van een stream door geven als het tweede argument. Als er meerdere overeenkomsten zijn, wordt de eerste overeenkomst geretourneerd. Als er geen overeenkomst wordt gevonden, wordt een NULL-waarde geretourneerd. De geretourneerde waarde moet van het type worden geconverteerd door een van de Type Conversion Functions (TO_DATE, TO_STRING...).  Kolom namen die bekend zijn tijdens de ontwerp fase moeten worden geadresseerd op basis van de naam. Berekende invoer waarden worden niet ondersteund, maar u kunt parameter vervangingen gebruiken* ``toString(byName('parent'))``  
* ``toLong(byName('income'))``  
* ``toBoolean(byName('foster'))``  
* ``toLong(byName($debtCol))``  
* ``toString(byName('Bogus Column'))``  
* ``toString(byName('Bogus Column', 'DeriveStream'))``  
___
### <code>byPosition</code>
<code><b>byPosition(<i>&lt;position&gt;</i> : integer) => any</b></code><br/><br/>
Selecteert een kolom waarde op basis van de relatieve positie (1 gebaseerd) in de stroom. Als de positie buiten het bereik valt, wordt een NULL-waarde geretourneerd. De geretourneerde waarde moet van het type worden geconverteerd door een van de Type Conversion Functions (TO_DATE, TO_STRING...) Berekende invoer waarden worden niet ondersteund, maar u kunt parameter vervangingen gebruiken* ``toString(byPosition(1))``  
* ``toDecimal(byPosition(2), 10, 2)``  
* ``toBoolean(byName(4))``  
* ``toString(byName($colName))``  
* ``toString(byPosition(1234))``  
___
### <code>case</code>
<code><b>case(<i>&lt;condition&gt;</i> : boolean, <i>&lt;true_expression&gt;</i> : any, <i>&lt;false_expression&gt;</i> : any, ...) => any</b></code><br/><br/>
Op basis van afwisselende voor waarden geldt een waarde of de andere. Als het aantal invoer waarden even is, wordt de andere standaard ingesteld op NULL voor de laatste voor waarde* ``case(10 + 20 == 30, 'dumbo', 'gumbo') -> 'dumbo'``  
* ``case(10 + 20 == 25, 'bojjus', 'do' < 'go', 'gunchus') -> 'gunchus'``  
* ``isNull(case(10 + 20 == 25, 'bojjus', 'do' > 'go', 'gunchus')) -> true``  
* ``case(10 + 20 == 25, 'bojjus', 'do' > 'go', 'gunchus', 'dumbo') -> 'dumbo'``  
___
### <code>cbrt</code>
<code><b>cbrt(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
De derdemachts wortel van een getal berekenen* ``cbrt(8) -> 2.0``  
___
### <code>ceil</code>
<code><b>ceil(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Retourneert het kleinste gehele getal dat niet kleiner is dan het getal* ``ceil(-0.1) -> 0``  
___
### <code>coalesce</code>
<code><b>coalesce(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Retourneert de eerste not null-waarde uit een set invoer waarden. Alle invoer waarden moeten van hetzelfde type zijn* ``coalesce(10, 20) -> 10``  
* ``coalesce(toString(null), toString(null), 'dumbo', 'bo', 'go') -> 'dumbo'``  
___
### <code>collect</code>
<code><b>collect(<i>&lt;value1&gt;</i> : any) => array</b></code><br/><br/>
Verzamelt alle waarden van de expressie in de geaggregeerde groep naar een matrix. Structuren kunnen tijdens dit proces worden verzameld en getransformeerd naar alternatieve structuren. Het aantal items is gelijk aan het aantal rijen in die groep en kan Null-waarden bevatten. Het aantal verzamelde items moet klein zijn* ``collect(salesPerson)``
* ``collect(firstName + lastName))``
* ``collect(@(name = salesPerson, sales = salesAmount) )``
___
### <code>columnNames</code>
<code><b>columnNames(<i>&lt;value1&gt;</i> : string) => array</b></code><br/><br/>
Hiermee worden alle uitvoer kolommen voor een stroom opgehaald. U kunt een optionele naam van een stream door geven als het tweede argument.  
* ``columnNames()``
* ``columnNames('DeriveStream')``
___
### <code>compare</code>
<code><b>compare(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => integer</b></code><br/><br/>
Vergelijkt twee waarden van hetzelfde type. Retourneert een negatief geheel getal als waarde1 < Value2, 0 als waarde1 = = waarde2, positieve waarde als waarde1 > Value2* ``(compare(12, 24) < 1) -> true``  
* ``(compare('dumbo', 'dum') > 0) -> true``  
___
### <code>concat</code>
<code><b>concat(<i>&lt;this&gt;</i> : string, <i>&lt;that&gt;</i> : string, ...) => string</b></code><br/><br/>
Voegt een variabele aantal teken reeksen samen. Hetzelfde als de operator + met teken reeksen* ``concat('dataflow', 'is', 'awesome') -> 'dataflowisawesome'``  
* ``'dataflow' + 'is' + 'awesome' -> 'dataflowisawesome'``  
* ``isNull('sql' + null) -> true``  
___
### <code>concatWS</code>
<code><b>concatWS(<i>&lt;separator&gt;</i> : string, <i>&lt;this&gt;</i> : string, <i>&lt;that&gt;</i> : string, ...) => string</b></code><br/><br/>
Voegt een variabele aantal teken reeksen samen met een scheidings teken. De eerste para meter is het scheidings teken* ``concatWS(' ', 'dataflow', 'is', 'awesome') -> 'dataflow is awesome'``  
* ``isNull(concatWS(null, 'dataflow', 'is', 'awesome')) -> true``  
* ``concatWS(' is ', 'dataflow', 'awesome') -> 'dataflow is awesome'``  
___
### <code>contains</code>
<code><b>contains(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : unaryfunction) => boolean</b></code><br/><br/>
Retourneert waar als een element in de geleverde matrix als waar wordt geëvalueerd in het geleverde predicaat. Contains verwacht een verwijzing naar één element in de predicaat functie als #item* ``contains([1, 2, 3, 4], #item == 3) -> true``  
* ``contains([1, 2, 3, 4], #item > 5) -> false``  
___
### <code>cos</code>
<code><b>cos(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt een cosinus waarde berekend* ``cos(10) -> -0.8390715290764524``  
___
### <code>cosh</code>
<code><b>cosh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt een hyperbolische cosinus van een waarde berekend* ``cosh(0) -> 1.0``  
___
### <code>crc32</code>
<code><b>crc32(<i>&lt;value1&gt;</i> : any, ...) => long</b></code><br/><br/>
Berekent de CRC32-hash van een set kolom van verschillende primitieve gegevens typen met een bitlengte die alleen uit waarden 0 (256), 224, 256, 384, 512 kan zijn. Het kan worden gebruikt voor het berekenen van een vinger afdruk voor een rij* ``crc32(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 3630253689L``  
___
### <code>currentDate</code>
<code><b>currentDate([<i>&lt;value1&gt;</i> : string]) => date</b></code><br/><br/>
Hiermee wordt de huidige datum opgehaald waarop deze taak wordt uitgevoerd. U kunt een optionele tijd zone door geven in de vorm ' GMT ', ' PST ', ' UTC ', ' America/Caymaneilanden '. De lokale tijd zone wordt als standaard waarde gebruikt. Raadpleeg de SimpleDateFormat van Java voor beschik bare indelingen. ["https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html) * ``currentDate() == toDate('2250-12-31') -> false``  
* ``currentDate('PST')  == toDate('2250-12-31') -> false``  
* ``currentDate('America/New_York')  == toDate('2250-12-31') -> false``  
___
### <code>currentTimestamp</code>
<code><b>currentTimestamp() => timestamp</b></code><br/><br/>
Hiermee wordt het huidige tijds tempel opgehaald wanneer de taak wordt uitgevoerd met de lokale tijd zone* ``currentTimestamp() == toTimestamp('2250-12-31 12:12:12') -> false``  
___
### <code>currentUTC</code>
<code><b>currentUTC([<i>&lt;value1&gt;</i> : string]) => timestamp</b></code><br/><br/>
Hiermee wordt de huidige tijds tempel opgehaald als UTC. Als u wilt dat uw huidige tijd wordt geïnterpreteerd in een andere tijd zone dan de tijd zone van uw cluster, kunt u een optionele tijd zone door geven in de vorm ' GMT ', ' PST ', ' UTC ', ' America/Caymaneilanden '. De waarde wordt standaard ingesteld op de huidige tijd zone. Raadpleeg de SimpleDateFormat van Java voor beschik bare indelingen. [https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html). Gebruik fromUTC () om de UTC-tijd om te zetten in een andere tijd zone.* ``currentUTC() == toTimestamp('2050-12-12 19:18:12') -> false``  
* ``currentUTC() != toTimestamp('2050-12-12 19:18:12') -> true``  
* ``fromUTC(currentUTC(), 'Asia/Seoul') != toTimestamp('2050-12-12 19:18:12') -> true``  
___
### <code>dayOfMonth</code>
<code><b>dayOfMonth(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Hiermee wordt de dag van de maand opgehaald op basis van een datum* ``dayOfMonth(toDate('2018-06-08')) -> 8``  
___
### <code>dayOfWeek</code>
<code><b>dayOfWeek(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Hiermee wordt de dag van de week opgehaald op basis van een datum. 1: zondag, 2-maandag..., 7-zaterdag* ``dayOfWeek(toDate('2018-06-08')) -> 6``  
___
### <code>dayOfYear</code>
<code><b>dayOfYear(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Hiermee wordt de dag van het jaar opgehaald op basis van een datum* ``dayOfYear(toDate('2016-04-09')) -> 100``  
___
### <code>days</code>
<code><b>days(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
De duur in milliseconden voor het aantal dagen* ``days(2) -> 172800000L``  
___
### <code>degrees</code>
<code><b>degrees(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Converteert radialen naar graden* ``degrees(3.141592653589793) -> 180``  
___
### <code>divide</code>
<code><b>divide(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Deelt een combi natie van getallen. Hetzelfde als de operator/* ``divide(20, 10) -> 2``  
* ``20 / 10 -> 2``  
___
### <code>endsWith</code>
<code><b>endsWith(<i>&lt;string&gt;</i> : string, <i>&lt;substring to check&gt;</i> : string) => boolean</b></code><br/><br/>
Controleert of de teken reeks eindigt met de opgegeven teken reeks* ``endsWith('dumbo', 'mbo') -> true``  
___
### <code>equals</code>
<code><b>equals(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Operator voor vergelijking gelijk aan. Hetzelfde als = = operator* ``equals(12, 24) -> false``  
* ``12 == 24 -> false``  
* ``'bad' == 'bad' -> true``  
* ``isNull('good' == toString(null)) -> true``  
* ``isNull(null == null) -> true``  
___
### <code>equalsIgnoreCase</code>
<code><b>equalsIgnoreCase(<i>&lt;value1&gt;</i> : string, <i>&lt;value2&gt;</i> : string) => boolean</b></code><br/><br/>
De vergelijkings operator is gelijk aan het hoofdletter gebruik negeren. Hetzelfde als <=>-operator* ``'abc'<=>'Abc' -> true``  
* ``equalsIgnoreCase('abc', 'Abc') -> true``  
___
### <code>factorial</code>
<code><b>factorial(<i>&lt;value1&gt;</i> : number) => long</b></code><br/><br/>
De faculteit van een getal berekenen* ``factorial(5) -> 120``  
___
### <code>false</code>
<code><b>false() => boolean</b></code><br/><br/>
Retourneert altijd een waarde ONWAAR. Gebruik de functie syntaxis (false ()) als er een kolom is met de naam ' false '* ``(10 + 20 > 30) -> false``  
* ``(10 + 20 > 30) -> false()``  
___
### <code>filter</code>
<code><b>filter(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : unaryfunction) => array</b></code><br/><br/>
Hiermee worden elementen van de matrix gefilterd die niet voldoen aan het gegeven predicaat. Het filter verwacht een verwijzing naar een element in de predicaat functie als #item* ``filter([1, 2, 3, 4], #item > 2) -> [3, 4]``  
* ``filter(['a', 'b', 'c', 'd'], #item == 'a' || #item == 'b') -> ['a', 'b']``  
___
### <code>floor</code>
<code><b>floor(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Retourneert het grootste gehele getal dat niet groter is dan het aantal* ``floor(-0.1) -> -1``  
___
### <code>fromBase64</code>
<code><b>fromBase64(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Codeert de gegeven teken reeks in base64* ``fromBase64('Z3VuY2h1cw==') -> 'gunchus'``  
___
### <code>fromUTC</code>
<code><b>fromUTC(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => timestamp</b></code><br/><br/>
Wordt geconverteerd naar de tijds tempel van UTC. U kunt eventueel de tijd zone in de vorm ' GMT ', ' PST ', ' UTC ', ' America/Caymaneilanden ' door geven. Het wordt standaard ingesteld op de huidige SimpleDateFormat van timezoneRefer Java voor beschik bare indelingen. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``fromUTC(currentTimeStamp()) == toTimestamp('2050-12-12 19:18:12') -> false``  
* ``fromUTC(currentTimeStamp(), 'Asia/Seoul') != toTimestamp('2050-12-12 19:18:12') -> true``  
___
### <code>greater</code>
<code><b>greater(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Hogere operator voor vergelijking. Hetzelfde als >-operator* ``greater(12, 24) -> false``  
* ``('dumbo' > 'dum') -> true``  
* ``(toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS') > toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS')) -> true``  
___
### <code>greaterOrEqual</code>
<code><b>greaterOrEqual(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
De vergelijking is groter dan of gelijk aan de operator. Hetzelfde als >= operator* ``greaterOrEqual(12, 12) -> true``  
* ``('dumbo' >= 'dum') -> true``  
___
### <code>greatest</code>
<code><b>greatest(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Retourneert de grootste waarde tussen de lijst met waarden als invoer waarbij Null-waarden worden overgeslagen. Retourneert null als alle invoer waarden null zijn* ``greatest(10, 30, 15, 20) -> 30``  
* ``greatest(10, toInteger(null), 20) -> 20``  
* ``greatest(toDate('2010-12-12'), toDate('2011-12-12'), toDate('2000-12-12')) -> toDate('2011-12-12')``  
* ``greatest(toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS'), toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS')) -> toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS')``  
___
### <code>hasColumn</code>
<code><b>hasColumn(<i>&lt;column name&gt;</i> : string, [<i>&lt;stream name&gt;</i> : string]) => boolean</b></code><br/><br/>
Hiermee wordt in de stroom op naam naar een kolom waarde gecontroleerd. U kunt een optionele naam van een stream door geven als het tweede argument.  Kolom namen die bekend zijn tijdens de ontwerp fase moeten worden geadresseerd op basis van de naam. Berekende invoer waarden worden niet ondersteund, maar u kunt parameter vervangingen gebruiken* ``hasColumn('parent')``  
___
### <code>hour</code>
<code><b>hour(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Hiermee wordt de uur waarde van een tijds tempel opgehaald. U kunt een optionele tijd zone door geven in de vorm ' GMT ', ' PST ', ' UTC ', ' America/Caymaneilanden '. De lokale tijd zone wordt als standaard waarde gebruikt. Raadpleeg de SimpleDateFormat van Java voor beschik bare indelingen. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``hour(toTimestamp('2009-07-30 12:58:59')) -> 12``  
* ``hour(toTimestamp('2009-07-30 12:58:59'), 'PST') -> 12``  
___
### <code>hours</code>
<code><b>hours(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
De duur in milliseconden voor het aantal uren* ``hours(2) -> 7200000L``  
___
### <code>iif</code>
<code><b>iif(<i>&lt;condition&gt;</i> : boolean, <i>&lt;true_expression&gt;</i> : any, [<i>&lt;false_expression&gt;</i> : any]) => any</b></code><br/><br/>
Op basis van een voor waarde geldt één waarde of de andere. Als de andere waarde niet wordt opgegeven, wordt deze als NULL beschouwd. Beide waarden moeten compatibel zijn (numeriek, teken reeks...)* ``iif(10 + 20 == 30, 'dumbo', 'gumbo') -> 'dumbo'``  
* ``iif(10 > 30, 'dumbo', 'gumbo') -> 'gumbo'``  
* ``iif(month(toDate('2018-12-01')) == 12, 345.12, 102.67) -> 345.12``  
___
### <code>iifNull</code>
<code><b>iifNull(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : any], ...) => any</b></code><br/><br/>
Controleert of de eerste para meter null is. Als dit niet het geval is, wordt de eerste para meter geretourneerd. Als de waarde Null is, wordt de tweede para meter geretourneerd. Als er drie para meters zijn opgegeven, is het gedrag hetzelfde als IIf (isNull (waarde1), Value2, value3) en de derde para meter wordt geretourneerd als de eerste waarde niet null is.  
* ``iifNull(10, 20) -> 10``  
* ``iifNull(null, 20, 40) -> 20``  
* ``iifNull('azure', 'data', 'factory') -> 'factory'``  
* ``iifNull(null, 'data', 'factory') -> 'data'``  
___
### <code>in</code>
<code><b>in(<i>&lt;array of items&gt;</i> : array, <i>&lt;item to find&gt;</i> : any) => boolean</b></code><br/><br/>
Hiermee wordt gecontroleerd of een item zich in de matrix bevindt* ``in([10, 20, 30], 10) -> true``  
* ``in(['good', 'kid'], 'bad') -> false``  
___
### <code>initCap</code>
<code><b>initCap(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Converteert de eerste letter van elk woord naar hoofd letters. Woorden worden geïdentificeerd als gescheiden door witruimte* ``initCap('cool iceCREAM') -> 'Cool Icecream'``  
___
### <code>instr</code>
<code><b>instr(<i>&lt;string&gt;</i> : string, <i>&lt;substring to find&gt;</i> : string) => integer</b></code><br/><br/>
Hiermee wordt de positie (1 op basis van de subtekenreeks in een teken reeks) gezocht. 0 wordt geretourneerd als het niet is gevonden* ``instr('dumbo', 'mbo') -> 3``  
* ``instr('microsoft', 'o') -> 5``  
* ``instr('good', 'bad') -> 0``  
___
### <code>isDelete</code>
<code><b>isDelete([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Hiermee wordt gecontroleerd of de rij is gemarkeerd voor verwijderen. Voor trans formaties waarbij meer dan één invoer stroom wordt gemaakt, kunt u de (1-op-basis) index van de stroom door geven. De stroom index moet 1 of 2 zijn en de standaard waarde is 1* ``isDelete()``  
* ``isDelete(1)``  
___
### <code>isError</code>
<code><b>isError([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Hiermee wordt gecontroleerd of de rij als fout is gemarkeerd. Voor trans formaties waarbij meer dan één invoer stroom wordt gemaakt, kunt u de (1-op-basis) index van de stroom door geven. De stroom index moet 1 of 2 zijn en de standaard waarde is 1* ``isError()``  
* ``isError(1)``  
___
### <code>isIgnore</code>
<code><b>isIgnore([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Hiermee wordt gecontroleerd of de rij is gemarkeerd om te worden genegeerd. Voor trans formaties waarbij meer dan één invoer stroom wordt gemaakt, kunt u de (1-op-basis) index van de stroom door geven. De stroom index moet 1 of 2 zijn en de standaard waarde is 1* ``isIgnore()``  
* ``isIgnore(1)``  
___
### <code>isInsert</code>
<code><b>isInsert([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Hiermee wordt gecontroleerd of de rij is gemarkeerd voor invoegen. Voor trans formaties waarbij meer dan één invoer stroom wordt gemaakt, kunt u de (1-op-basis) index van de stroom door geven. De stroom index moet 1 of 2 zijn en de standaard waarde is 1* ``isInsert()``  
* ``isInsert(1)``  
___
### <code>isMatch</code>
<code><b>isMatch([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Hiermee wordt gecontroleerd of de rij overeenkomt bij het opzoeken. Voor trans formaties waarbij meer dan één invoer stroom wordt gemaakt, kunt u de (1-op-basis) index van de stroom door geven. De stroom index moet 1 of 2 zijn en de standaard waarde is 1* ``isMatch()``  
* ``isMatch(1)``  
___
### <code>isNull</code>
<code><b>isNull(<i>&lt;value1&gt;</i> : any) => boolean</b></code><br/><br/>
Controleert of de waarde NULL is* ``isNull(NULL()) -> true``  
* ``isNull('') -> false``  
___
### <code>isUpdate</code>
<code><b>isUpdate([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Hiermee wordt gecontroleerd of de rij is gemarkeerd voor bijwerken. Voor trans formaties waarbij meer dan één invoer stroom wordt gemaakt, kunt u de (1-op-basis) index van de stroom door geven. De stroom index moet 1 of 2 zijn en de standaard waarde is 1* ``isUpdate()``  
* ``isUpdate(1)``  
___
### <code>isUpsert</code>
<code><b>isUpsert([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Hiermee wordt gecontroleerd of de rij is gemarkeerd voor invoegen. Voor trans formaties waarbij meer dan één invoer stroom wordt gemaakt, kunt u de (1-op-basis) index van de stroom door geven. De stroom index moet 1 of 2 zijn en de standaard waarde is 1* ``isUpsert()``  
* ``isUpsert(1)``  
___
### <code>lastDayOfMonth</code>
<code><b>lastDayOfMonth(<i>&lt;value1&gt;</i> : datetime) => date</b></code><br/><br/>
Hiermee wordt de laatste datum van de maand opgehaald op basis van een datum* ``lastDayOfMonth(toDate('2009-01-12')) -> toDate('2009-01-31')``  
___
### <code>least</code>
<code><b>least(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Vergelijkings operator kleiner dan of gelijk aan. Hetzelfde als <= operator* ``least(10, 30, 15, 20) -> 10``  
* ``least(toDate('2010-12-12'), toDate('2011-12-12'), toDate('2000-12-12')) -> toDate('2000-12-12')``  
___
### <code>left</code>
<code><b>left(<i>&lt;string to subset&gt;</i> : string, <i>&lt;number of characters&gt;</i> : integral) => string</b></code><br/><br/>
Hiermee wordt een subtekenreeks geëxtraheerd die begint bij index 1 met het aantal tekens. Hetzelfde als subtekenreeks (str, 1, n)* ``left('bojjus', 2) -> 'bo'``  
* ``left('bojjus', 20) -> 'bojjus'``  
___
### <code>length</code>
<code><b>length(<i>&lt;value1&gt;</i> : string) => integer</b></code><br/><br/>
Retourneert de lengte van de teken reeks* ``length('dumbo') -> 5``  
___
### <code>lesser</code>
<code><b>lesser(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Operator voor vergelijkings minus. Hetzelfde als <-operator* ``lesser(12, 24) -> true``  
* ``('abcd' < 'abc') -> false``  
* ``(toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS') < toTimestamp('2019-02-05 08:21:34.890', 'yyyy-MM-dd HH:mm:ss.SSS')) -> true``  
___
### <code>lesserOrEqual</code>
<code><b>lesserOrEqual(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Vergelijkings operator kleiner dan of gelijk aan. Hetzelfde als <= operator* ``lesserOrEqual(12, 12) -> true``  
* ``('dumbo' <= 'dum') -> false``  
___
### <code>levenshtein</code>
<code><b>levenshtein(<i>&lt;from string&gt;</i> : string, <i>&lt;to string&gt;</i> : string) => integer</b></code><br/><br/>
Hiermee wordt de Levenshtein-afstand tussen twee teken reeksen opgehaald* ``levenshtein('boys', 'girls') -> 4``  
___
### <code>like</code>
<code><b>like(<i>&lt;string&gt;</i> : string, <i>&lt;pattern match&gt;</i> : string) => boolean</b></code><br/><br/>
Het patroon is een teken reeks die letterlijk overeenkomt. De uitzonde ringen zijn de volgende speciale symbolen: _ overeenkomen met een wille keurig teken in de invoer (vergelijkbaar met. in POSIX-reguliere expressies)% komt overeen met nul of meer tekens in de invoer (vergelijkbaar met. * in reguliere expressies van POSIX).
Het escape-teken is ' '. Als een escape-teken voorafgaat aan een speciaal symbool of een ander escape teken, wordt het volgende teken letterlijk vergeleken. Het is niet toegestaan om een ander teken te escapepen.  
* ``like('icecream', 'ice%') -> true``  
___
### <code>locate</code>
<code><b>locate(<i>&lt;substring to find&gt;</i> : string, <i>&lt;string&gt;</i> : string, [<i>&lt;from index - 1-based&gt;</i> : integral]) => integer</b></code><br/><br/>
Hiermee wordt de positie (1 op basis van de subtekenreeks) binnen een teken reeks met een bepaalde positie gevonden. Als de positie wordt wegge laten, wordt deze in overweging genomen vanaf het begin van de teken reeks. 0 wordt geretourneerd als het niet is gevonden* ``locate('mbo', 'dumbo') -> 3``  
* ``locate('o', 'microsoft', 6) -> 7``  
* ``locate('bad', 'good') -> 0``  
___
### <code>log</code>
<code><b>log(<i>&lt;value1&gt;</i> : number, [<i>&lt;value2&gt;</i> : number]) => double</b></code><br/><br/>
De logboek waarde wordt berekend. Een optionele basis kan worden opgegeven als een Euler-nummer bij gebruik* ``log(100, 10) -> 2``  
___
### <code>log10</code>
<code><b>log10(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt de logboek waarde berekend op basis van 10 basis* ``log10(100) -> 2``  
___
### <code>lower</code>
<code><b>lower(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Kleine letters een teken reeks* ``lower('GunChus') -> 'gunchus'``  
___
### <code>lpad</code>
<code><b>lpad(<i>&lt;string to pad&gt;</i> : string, <i>&lt;final padded length&gt;</i> : integral, <i>&lt;padding&gt;</i> : string) => string</b></code><br/><br/>
Links wordt de teken reeks met de opgegeven opvulling gematteerd tot deze een bepaalde lengte heeft. Als de teken reeks gelijk is aan of groter is dan de lengte, wordt deze bijgesneden tot de lengte* ``lpad('dumbo', 10, '-') -> '-----dumbo'``  
* ``lpad('dumbo', 4, '-') -> 'dumb'``  
*' ' lpad (' Dumbo ', 8, ' <> ')-> ' <><dumbo'``  
___
### <code>LTrim</code>
<code><b>ltrim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></code><br/><br/>
Hiermee wordt een teken reeks met voorloop tekens bijgesneden. Als de tweede para meter niet is opgegeven, wordt witruimte verkleind. Anders wordt het teken dat in de tweede para meter is opgegeven, verkleind* ``ltrim('  dumbo  ') -> 'dumbo  '``  
* ``ltrim('!--!du!mbo!', '-!') -> 'du!mbo!'``  
___
### <code>map</code>
<code><b>map(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : unaryfunction) => any</b></code><br/><br/>
Wijst elk element van de matrix toe aan een nieuw element met behulp van de gegeven expressie. Toewijzing verwacht een verwijzing naar één element in de expressie functie als #item* ``map([1, 2, 3, 4], #item + 2) -> [3, 4, 5, 6]``  
* ``map(['a', 'b', 'c', 'd'], #item + '_processed') -> ['a_processed', 'b_processed', 'c_processed', 'd_processed']``  
___
### <code>mapIndex</code>
<code><b>mapIndex(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : binaryfunction) => any</b></code><br/><br/>
Wijst elk element van de matrix toe aan een nieuw element met behulp van de gegeven expressie. Toewijzing verwacht een verwijzing naar één element in de expressie functie als #item en een verwijzing naar de element index als #index* ``mapIndex([1, 2, 3, 4], #item + 2 + #index) -> [4, 6, 8, 10]``  
___
### <code>md5</code>
<code><b>md5(<i>&lt;value1&gt;</i> : any, ...) => string</b></code><br/><br/>
Hiermee wordt de MD5-Digest van een set kolom van verschillende primitieve gegevens typen berekend en wordt een hexadecimale teken reeks van 32 tekens geretourneerd. Het kan worden gebruikt voor het berekenen van een vinger afdruk voor een rij* ``md5(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> '4ce8a880bd621a1ffad0bca905e1bc5a'``  
___
### <code>millisecond</code>
<code><b>millisecond(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Hiermee wordt de milliseconde-waarde van een datum opgehaald. U kunt een optionele tijd zone door geven in de vorm ' GMT ', ' PST ', ' UTC ', ' America/Caymaneilanden '. De lokale tijd zone wordt als standaard waarde gebruikt. Raadpleeg de SimpleDateFormat van Java voor beschik bare indelingen. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``millisecond(toTimestamp('2009-07-30 12:58:59.871', 'yyyy-MM-dd HH:mm:ss.SSS')) -> 871``  
___
### <code>milliseconds</code>
<code><b>milliseconds(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
De duur in milliseconden voor het aantal milliseconden* ``milliseconds(2) -> 2L``  
___
### <code>minus</code>
<code><b>minus(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Trekt getallen af. Aftrekken van het datum aantal dagen. De duur aftrekken van een tijds tempel. Trek twee time Stamps af om het verschil in milliseconden te verkrijgen. Hetzelfde als de-operator* ``minus(20, 10) -> 10``  
* ``20 - 10 -> 10``  
* ``minus(toDate('2012-12-15'), 3) -> toDate('2012-12-12')``  
* ``toDate('2012-12-15') - 3 -> toDate('2012-12-12')``  
* ``toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS') + (days(1) + hours(2) - seconds(10)) -> toTimestamp('2019-02-04 07:19:18.871', 'yyyy-MM-dd HH:mm:ss.SSS')``  
* ``toTimestamp('2019-02-03 05:21:34.851', 'yyyy-MM-dd HH:mm:ss.SSS') - toTimestamp('2019-02-03 05:21:36.923', 'yyyy-MM-dd HH:mm:ss.SSS') -> -2072``  
___
### <code>minute</code>
<code><b>minute(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Hiermee wordt de minuut waarde van een tijds tempel opgehaald. U kunt een optionele tijd zone door geven in de vorm ' GMT ', ' PST ', ' UTC ', ' America/Caymaneilanden '. De lokale tijd zone wordt als standaard waarde gebruikt. Raadpleeg de SimpleDateFormat van Java voor beschik bare indelingen. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``minute(toTimestamp('2009-07-30 12:58:59')) -> 58``  
* ``minute(toTimestamp('2009-07-30 12:58:59'), 'PST') -> 58``  
___
### <code>minutes</code>
<code><b>minutes(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
De duur in milliseconden voor het aantal minuten* ``minutes(2) -> 120000L``  
___
### <code>mod</code>
<code><b>mod(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Modulus van een combi natie van getallen. Hetzelfde als de operator%* ``mod(20, 8) -> 4``  
* ``20 % 8 -> 4``  
___
### <code>month</code>
<code><b>month(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Hiermee wordt de maand waarde van een datum of tijds tempel opgehaald* ``month(toDate('2012-8-8')) -> 8``  
___
### <code>monthsBetween</code>
<code><b>monthsBetween(<i>&lt;from date/timestamp&gt;</i> : datetime, <i>&lt;to date/timestamp&gt;</i> : datetime, [<i>&lt;roundoff&gt;</i> : boolean], [<i>&lt;time zone&gt;</i> : string]) => double</b></code><br/><br/>
Hiermee wordt het aantal maanden tussen twee datums opgehaald. U kunt de berekening afronden. U kunt een optionele tijd zone door geven in de vorm ' GMT ', ' PST ', ' UTC ', ' America/Caymaneilanden '. De lokale tijd zone wordt als standaard waarde gebruikt. Raadpleeg de SimpleDateFormat van Java voor beschik bare indelingen. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``monthsBetween(toTimestamp('1997-02-28 10:30:00'), toDate('1996-10-30')) -> 3.94959677``  
___
### <code>multiply</code>
<code><b>multiply(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Vermenigvuldigt het paar getallen. Hetzelfde als de operator ** ``multiply(20, 10) -> 200``  
* ``20 * 10 -> 200``  
___
### <code>negate</code>
<code><b>negate(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Een getal wordt genegeerd. Hiermee worden positieve getallen negatief en omgekeerd* ``negate(13) -> -13``  
___
### <code>nextSequence</code>
<code><b>nextSequence() => long</b></code><br/><br/>
Retourneert de volgende unieke sequentie. Het getal is opeenvolgend alleen binnen een partitie en wordt voorafgegaan door de partitionId* ``nextSequence() == 12313112 -> false``  
___
### <code>normalize</code>
<code><b>normalize(<i>&lt;String to normalize&gt;</i> : string) => string</b></code><br/><br/>
De teken reeks waarde normaliseren om Unicode-tekens met accenten te scheiden* ``regexReplace(normalize('bo²s'), `\p{M}`, '') -> 'boys'``  
___
### <code>not</code>
<code><b>not(<i>&lt;value1&gt;</i> : boolean) => boolean</b></code><br/><br/>
Logische negatie-operator* ``not(true) -> false``  
* ``not(10 == 20) -> true``  
___
### <code>notEquals</code>
<code><b>notEquals(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
De vergelijkings operator is niet gelijk aan. Hetzelfde als! =-operator* ``12 != 24 -> true``  
* ``'bojjus' != 'bo' + 'jjus' -> false``  
___
### <code>notNull</code>
<code><b>notNull(<i>&lt;value1&gt;</i> : any) => boolean</b></code><br/><br/>
Controleert of de waarde niet NULL is* ``notNull(NULL()) -> false``  
* ``notNull('') -> true``  
___
### <code>null</code>
<code><b>null() => null</b></code><br/><br/>
Retourneert een NULL-waarde. Gebruik de functie syntaxis (Null ()) als er een kolom is met de naam null. Elke bewerking die gebruikmaakt, resulteert in een NULL-waarde* ``isNull('dumbo' + null) -> true``  
* ``isNull(10 * null) -> true``  
* ``isNull('') -> false``  
* ``isNull(10 + 20) -> false``  
* ``isNull(10/0) -> true``  
___
### <code>or</code>
<code><b>or(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Logische OR-operator. Hetzelfde als | |* ``or(true, false) -> true``  
* ``true || false -> true``  
___
### <code>pMod</code>
<code><b>pMod(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Positieve modulus van een paar getallen.  
* ``pmod(-20, 8) -> 4``  
___
### <code>partitionId</code>
<code><b>partitionId() => integer</b></code><br/><br/>
Retourneert de huidige partitie-id waarin de invoer rij zich bevindt* ``partitionId()``  
___
### <code>power</code>
<code><b>power(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt één getal verhoogd naar de macht van een andere.* ``power(10, 2) -> 100``  
___
### <code>reduce</code>
<code><b>reduce(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : any, <i>&lt;value3&gt;</i> : binaryfunction, <i>&lt;value4&gt;</i> : unaryfunction) => any</b></code><br/><br/>
Verzamelt elementen in een matrix. Reductie verwacht een verwijzing naar een accumulator en één element in de eerste expressie functie als #acc en #item en verwacht de resulterende waarde als #result voor gebruik in de tweede expressie functie* ``toString(reduce(['1', '2', '3', '4'], '0', #acc + #item, #result)) -> '01234'``  
___
### <code>regexExtract</code>
<code><b>regexExtract(<i>&lt;string&gt;</i> : string, <i>&lt;regex to find&gt;</i> : string, [<i>&lt;match group 1-based index&gt;</i> : integral]) => string</b></code><br/><br/>
Een overeenkomende subtekenreeks ophalen voor een opgegeven regex-patroon. De laatste para meter identificeert de overeenkomende groep en wordt standaard ingesteld op 1 als dit wordt wegge laten. Gebruik '<regex>' (back-quote) om een teken reeks zonder aanhalings tekens te zoeken* ``regexExtract('Cost is between 600 and 800 dollars', '(\\d+) and (\\d+)', 2) -> '800'``  
* ``regexExtract('Cost is between 600 and 800 dollars', `(\d+) and (\d+)`, 2) -> '800'``  
___
### <code>regexMatch</code>
<code><b>regexMatch(<i>&lt;string&gt;</i> : string, <i>&lt;regex to match&gt;</i> : string) => boolean</b></code><br/><br/>
Controleert of de teken reeks overeenkomt met het opgegeven regex-patroon. Gebruik '<regex>' (back-quote) om een teken reeks zonder aanhalings tekens te zoeken* ``regexMatch('200.50', '(\\d+).(\\d+)') -> true``  
* ``regexMatch('200.50', `(\d+).(\d+)`) -> true``  
___
### <code>regexReplace</code>
<code><b>regexReplace(<i>&lt;string&gt;</i> : string, <i>&lt;regex to find&gt;</i> : string, <i>&lt;substring to replace&gt;</i> : string) => string</b></code><br/><br/>
Alle exemplaren van een regex-patroon vervangen door een andere subtekenreeks in de opgegeven teken reeks<regex>gebruik ' ' (back quote) om een teken reeks zonder aanhalings tekens te zoeken.* ``regexReplace('100 and 200', '(\\d+)', 'bojjus') -> 'bojjus and bojjus'``  
* ``regexReplace('100 and 200', `(\d+)`, 'gunchus') -> 'gunchus and gunchus'``  
___
### <code>regexSplit</code>
<code><b>regexSplit(<i>&lt;string to split&gt;</i> : string, <i>&lt;regex expression&gt;</i> : string) => array</b></code><br/><br/>
Splitst een teken reeks op basis van een scheidings teken op basis van een reguliere expressie en retourneert een matrix met teken reeksen* ``regexSplit('bojjusAgunchusBdumbo', `[CAB]`) -> ['bojjus', 'gunchus', 'dumbo']``  
* ``regexSplit('bojjusAgunchusBdumboC', `[CAB]`) -> ['bojjus', 'gunchus', 'dumbo', '']``  
* ``(regexSplit('bojjusAgunchusBdumboC', `[CAB]`)[1]) -> 'bojjus'``  
* ``isNull(regexSplit('bojjusAgunchusBdumboC', `[CAB]`)[20]) -> true``  
___
### <code>replace</code>
<code><b>replace(<i>&lt;string&gt;</i> : string, <i>&lt;substring to find&gt;</i> : string, [<i>&lt;substring to replace&gt;</i> : string]) => string</b></code><br/><br/>
Alle exemplaren van een subtekenreeks vervangen door een andere subtekenreeks in de opgegeven teken reeks. Als de laatste para meter wordt wegge laten, wordt standaard een lege teken reeks geretourneerd* ``replace('doggie dog', 'dog', 'cat') -> 'catgie cat'``  
* ``replace('doggie dog', 'dog', '') -> 'gie '``  
* ``replace('doggie dog', 'dog') -> 'gie '``  
___
### <code>reverse</code>
<code><b>reverse(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Hiermee wordt een teken reeks omgekeerd* ``reverse('gunchus') -> 'suhcnug'``  
___
### <code>right</code>
<code><b>right(<i>&lt;string to subset&gt;</i> : string, <i>&lt;number of characters&gt;</i> : integral) => string</b></code><br/><br/>
Hiermee wordt een subtekenreeks geëxtraheerd met het aantal tekens vanaf de rechter kant. Gelijk aan subtekenreeks (str, LENGTH (str)-n, n)* ``right('bojjus', 2) -> 'us'``  
* ``right('bojjus', 20) -> 'bojjus'``  
___
### <code>rlike</code>
<code><b>rlike(<i>&lt;string&gt;</i> : string, <i>&lt;pattern match&gt;</i> : string) => boolean</b></code><br/><br/>
Controleert of de teken reeks overeenkomt met het opgegeven regex-patroon* ``rlike('200.50', `(\d+).(\d+)`) -> true``  
* ``rlike('bogus', `M[0-9]+.*`) -> false``  
___
### <code>round</code>
<code><b>round(<i>&lt;number&gt;</i> : number, [<i>&lt;scale to round&gt;</i> : number], [<i>&lt;rounding option&gt;</i> : integral]) => double</b></code><br/><br/>
Rondt een getal af op basis van een optionele schaal en een optionele Afrondings modus. Als de schaal wordt wegge laten, wordt deze standaard ingesteld op 0.  Als de modus wordt wegge laten, wordt de standaard waarde ROUND_HALF_UP (5). De waarden voor afronding bevatten 1 ROUND_UP 2 ROUND_DOWN 3 ROUND_CEILING 4-ROUND_FLOOR 5-ROUND_HALF_UP 6-ROUND_HALF_DOWN 7-ROUND_HALF_EVEN 8-ROUND_UNNECESSARY* ``round(100.123) -> 100.0``  
* ``round(2.5, 0) -> 3.0``  
* ``round(5.3999999999999995, 2, 7) -> 5.40``  
___
### <code>rpad</code>
<code><b>rpad(<i>&lt;string to pad&gt;</i> : string, <i>&lt;final padded length&gt;</i> : integral, <i>&lt;padding&gt;</i> : string) => string</b></code><br/><br/>
Met rechts wordt de teken reeks met de opgegeven opvulling gematteerd tot deze een bepaalde lengte heeft. Als de teken reeks gelijk is aan of groter is dan de lengte, wordt deze bijgesneden tot de lengte* ``rpad('dumbo', 10, '-') -> 'dumbo-----'``  
* ``rpad('dumbo', 4, '-') -> 'dumb'``  
* ``rpad('dumbo', 8, '<>') -> 'dumbo<><'``  
___
### <code>rtrim</code>RTrim</code>
<code><b>rtrim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></code><br/><br/>
Rechts knipt een teken reeks van voorloop tekens. Als de tweede para meter niet is opgegeven, wordt witruimte verkleind. Anders wordt het teken dat in de tweede para meter is opgegeven, verkleind* ``rtrim('  dumbo  ') -> '  dumbo'``  
* ``rtrim('!--!du!mbo!', '-!') -> '!--!du!mbo'``  
___
### <code>second</code>
<code><b>second(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Hiermee wordt de tweede waarde van een datum opgehaald. U kunt een optionele tijd zone door geven in de vorm ' GMT ', ' PST ', ' UTC ', ' America/Caymaneilanden '. De lokale tijd zone wordt als standaard waarde gebruikt. Raadpleeg de SimpleDateFormat van Java voor beschik bare indelingen. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``second(toTimestamp('2009-07-30 12:58:59')) -> 59``  
___
### <code>seconds</code>
<code><b>seconds(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
De duur in milliseconden voor het aantal seconden* ``seconds(2) -> 2000L``  
___
### <code>sha1</code>
<code><b>sha1(<i>&lt;value1&gt;</i> : any, ...) => string</b></code><br/><br/>
Hiermee wordt het SHA-1-overzicht van een set kolom van verschillende primitieve gegevens typen berekend en wordt een hexadecimale teken reeks van 40 tekens geretourneerd. Het kan worden gebruikt voor het berekenen van een vinger afdruk voor een rij* ``sha1(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> '46d3b478e8ec4e1f3b453ac3d8e59d5854e282bb'``  
___
### <code>sha2</code>
<code><b>sha2(<i>&lt;value1&gt;</i> : integer, <i>&lt;value2&gt;</i> : any, ...) => string</b></code><br/><br/>
Hiermee wordt het SHA-2-overzicht van een set kolom van verschillende primitieve gegevens typen berekend met een bitlengte die alleen uit waarden 0 (256), 224, 256, 384, 512 kan zijn. Het kan worden gebruikt voor het berekenen van een vinger afdruk voor een rij* ``sha2(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 'afe8a553b1761c67d76f8c31ceef7f71b66a1ee6f4e6d3b5478bf68b47d06bd3'``  
___
### <code>sin</code>
<code><b>sin(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt een sinus waarde berekend* ``sin(2) -> 0.9092974268256817``  
___
### <code>sinh</code>
<code><b>sinh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt een hyperbolische sinus waarde berekend* ``sinh(0) -> 0.0``  
___
### <code>slice</code>
<code><b>slice(<i>&lt;array to slice&gt;</i> : array, <i>&lt;from 1-based index&gt;</i> : integral, [<i>&lt;number of items&gt;</i> : integral]) => array</b></code><br/><br/>
Hiermee wordt een subset van een matrix geëxtraheerd uit een positie. De positie is op basis van 1. Als de lengte wordt wegge laten, wordt deze standaard ingesteld op einde van de teken reeks* ``slice([10, 20, 30, 40], 1, 2) -> [10, 20]``  
* ``slice([10, 20, 30, 40], 2) -> [20, 30, 40]``  
* ``slice([10, 20, 30, 40], 2)[1] -> 20``  
* ``isNull(slice([10, 20, 30, 40], 2)[0]) -> true``  
* ``isNull(slice([10, 20, 30, 40], 2)[20]) -> true``  
* ``slice(['a', 'b', 'c', 'd'], 8) -> []``  
___
### <code>sort</code>
<code><b>sort(<i>&lt;value1&gt;</i> : array, <i>&lt;value2&gt;</i> : binaryfunction) => array</b></code><br/><br/>
Sorteert de matrix met behulp van de meegeleverde predicaat functie. Bij het sorteren wordt een verwijzing verwacht naar twee opeenvolgende elementen in de expressie functie als #item1 en #item2* ``sort([4, 8, 2, 3], compare(#item1, #item2)) -> [2, 3, 4, 8]``  
* ``sort(['a3', 'b2', 'c1'], iif(right(#item1, 1) >= right(#item2, 1), 1, -1)) -> ['c1', 'b2', 'a3']``  
___
### <code>soundex</code>
<code><b>soundex(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Hiermee wordt de Soundex-code voor de teken reeks opgehaald* ``soundex('genius') -> 'G520'``  
___
### <code>split</code>
<code><b>split(<i>&lt;string to split&gt;</i> : string, <i>&lt;split characters&gt;</i> : string) => array</b></code><br/><br/>
Splitst een teken reeks op basis van een scheidings teken en retourneert een matrix met teken reeksen* ``split('bojjus,guchus,dumbo', ',') -> ['bojjus', 'guchus', 'dumbo']``  
* ``split('bojjus,guchus,dumbo', '|') -> ['bojjus,guchus,dumbo']``  
* ``split('bojjus, guchus, dumbo', ', ') -> ['bojjus', 'guchus', 'dumbo']``  
* ``split('bojjus, guchus, dumbo', ', ')[1] -> 'bojjus'``  
* ``isNull(split('bojjus, guchus, dumbo', ', ')[0]) -> true``  
* ``isNull(split('bojjus, guchus, dumbo', ', ')[20]) -> true``  
* ``split('bojjusguchusdumbo', ',') -> ['bojjusguchusdumbo']``  
___
### <code>sqrt</code>
<code><b>sqrt(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt de vierkantswortel van een getal berekend* ``sqrt(9) -> 3``  
___
### <code>startsWith</code>
<code><b>startsWith(<i>&lt;string&gt;</i> : string, <i>&lt;substring to check&gt;</i> : string) => boolean</b></code><br/><br/>
Hiermee wordt gecontroleerd of de teken reeks begint met de opgegeven teken reeks* ``startsWith('dumbo', 'du') -> true``  
___
### <code>subDays</code>
<code><b>subDays(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;days to subtract&gt;</i> : integral) => datetime</b></code><br/><br/>
Maanden aftrekken van een datum of tijds tempel. Hetzelfde als de-operator voor datum* ``subDays(toDate('2016-08-08'), 1) -> toDate('2016-08-07')``  
___
### <code>subMonths</code>
<code><b>subMonths(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;months to subtract&gt;</i> : integral) => datetime</b></code><br/><br/>
Maanden aftrekken van een datum of tijds tempel* ``subMonths(toDate('2016-09-30'), 1) -> toDate('2016-08-31')``  
___
### <code>substring</code>
<code><b>substring(<i>&lt;string to subset&gt;</i> : string, <i>&lt;from 1-based index&gt;</i> : integral, [<i>&lt;number of characters&gt;</i> : integral]) => string</b></code><br/><br/>
Hiermee wordt een subtekenreeks van een bepaalde lengte opgehaald uit een positie. De positie is op basis van 1. Als de lengte wordt wegge laten, wordt deze standaard ingesteld op einde van de teken reeks* ``substring('Cat in the hat', 5, 2) -> 'in'``  
* ``substring('Cat in the hat', 5, 100) -> 'in the hat'``  
* ``substring('Cat in the hat', 5) -> 'in the hat'``  
* ``substring('Cat in the hat', 100, 100) -> ''``  
___
### <code>tan</code>
<code><b>tan(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt een raaklijn waarde berekend* ``tan(0) -> 0.0``  
___
### <code>tanh</code>
<code><b>tanh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt een hyperbolische tangens waarde berekend* ``tanh(0) -> 0.0``  
___
### <code>toBase64</code>
<code><b>toBase64(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Codeert de gegeven teken reeks in base64* ``toBase64('bojjus') -> 'Ym9qanVz'``  
___
### <code>toBinary</code>
<code><b>toBinary(<i>&lt;value1&gt;</i> : any) => binary</b></code><br/><br/>
Converteert wille keurige numerieke/datum/tijds tempel/teken reeks naar binaire weer gave* ``toBinary(3) -> [0x11]``  
___
### <code>toBoolean</code>
<code><b>toBoolean(<i>&lt;value1&gt;</i> : string) => boolean</b></code><br/><br/>
Converteert een waarde van ('t ', ' True ', ' y ', ' Yes ', ' 1 ') naar waar en (' f ', ' onwaar ', ' n ', ' nee ', ' 0 ') naar onwaar en NULL voor een andere waarde* ``toBoolean('true') -> true``  
* ``toBoolean('n') -> false``  
* ``isNull(toBoolean('truthy')) -> true``  
___
### <code>toDate</code>
<code><b>toDate(<i>&lt;string&gt;</i> : any, [<i>&lt;date format&gt;</i> : string]) => date</b></code><br/><br/>
De invoer datum teken reeks wordt geconverteerd naar een datum met een optionele notatie voor de invoer datum. Raadpleeg de SimpleDateFormat van Java voor beschik bare indelingen. Als de indeling van de invoer datum wordt wegge laten, is de standaard notatie JJJJ-[M] M-[d] d. Geaccepteerde notaties zijn: [jjjj, yyyy-[M] M, yyyy-[M] M-[d] d, yyyy-[M] M-[d] dT *]* ``toDate('2012-8-18') -> toDate('2012-08-18')``  
* ``toDate('12/18/2012', 'MM/dd/yyyy') -> toDate('2012-12-18')``  
___
### <code>toDecimal</code>
<code><b>toDecimal(<i>&lt;value&gt;</i> : any, [<i>&lt;precision&gt;</i> : integral], [<i>&lt;scale&gt;</i> : integral], [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => decimal(10,0)</b></code><br/><br/>
Converteert een wille keurig getal of teken reeks naar een decimale waarde. Als er geen precisie en schaal is opgegeven, wordt het standaard ingesteld op (10, 2). Een optionele Java-decimale notatie kan worden gebruikt voor de conversie. Een optionele indeling voor land instellingen in de vorm van BCP47 taal zoals en-US, de, zh-CN* ``toDecimal(123.45) -> 123.45``  
* ``toDecimal('123.45', 8, 4) -> 123.4500``  
* ``toDecimal('$123.45', 8, 4,'$###.00') -> 123.4500``  
* ``toDecimal('Ç123,45', 10, 2, 'Ç###,##', 'de') -> 123.45``  
___
### <code>toDouble</code>
<code><b>toDouble(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => double</b></code><br/><br/>
Converteert een wille keurig getal of teken reeks naar een dubbele waarde. Een optionele Java-decimale notatie kan worden gebruikt voor de conversie. Een optionele indeling voor land instellingen in de vorm van BCP47 taal zoals en-US, de, zh-CN* ``toDouble(123.45) -> 123.45``  
* ``toDouble('123.45') -> 123.45``  
* ``toDouble('$123.45', '$###.00') -> 123.45``  
* ``toDouble('Ç123,45', 'Ç###,##', 'de') -> 123.45``  
___
### <code>toFloat</code>
<code><b>toFloat(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => float</b></code><br/><br/>
Converteert een wille keurig getal of teken reeks naar een float-waarde. Een optionele Java-decimale notatie kan worden gebruikt voor de conversie. Verkapt een dubbel* ``toFloat(123.45) -> 123.45f``  
* ``toFloat('123.45') -> 123.45f``  
* ``toFloat('$123.45', '$###.00') -> 123.45f``  
___
### <code>toInteger</code>
<code><b>toInteger(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => integer</b></code><br/><br/>
Converteert een wille keurige numerieke waarde of teken reeks naar een geheel getal. Een optionele Java-decimale notatie kan worden gebruikt voor de conversie. Kapte Long, float, double* ``toInteger(123) -> 123``  
* ``toInteger('123') -> 123``  
* ``toInteger('$123', '$###') -> 123``  
___
### <code>toLong</code>
<code><b>toLong(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => long</b></code><br/><br/>
Converteert een wille keurig getal of teken reeks naar een lange waarde. Een optionele Java-decimale notatie kan worden gebruikt voor de conversie. Verkapt wille keurig zweven, dubbel* ``toLong(123) -> 123``  
* ``toLong('123') -> 123``  
* ``toLong('$123', '$###') -> 123``  
___
### <code>toShort</code>
<code><b>toShort(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string], [<i>&lt;locale&gt;</i> : string]) => short</b></code><br/><br/>
Converteert een wille keurig getal of teken reeks naar een korte waarde. Een optionele Java-decimale notatie kan worden gebruikt voor de conversie. Kapt geheel getal, lang, zweven, dubbel* ``toShort(123) -> 123``  
* ``toShort('123') -> 123``  
* ``toShort('$123', '$###') -> 123``  
___
### <code>toString</code>
<code><b>toString(<i>&lt;value&gt;</i> : any, [<i>&lt;number format/date format&gt;</i> : string]) => string</b></code><br/><br/>
Hiermee wordt een primitieve gegevens type geconverteerd naar een teken reeks. Voor getallen en datum kan een notatie worden opgegeven. Als u niet opgeeft, wordt de standaard waarde van het systeem gekozen. De decimale notatie voor de Java wordt gebruikt voor getallen. Raadpleeg Java SimpleDateFormat voor alle mogelijke datum notaties. de standaard indeling is JJJJ-MM-DD* ``toString(10) -> '10'``  
* ``toString('engineer') -> 'engineer'``  
* ``toString(123456.789, '##,###.##') -> '123,456.79'``  
* ``toString(123.78, '000000.000') -> '000123.780'``  
* ``toString(12345, '##0.#####E0') -> '12.345E3'``  
* ``toString(toDate('2018-12-31')) -> '2018-12-31'``  
* ``isNull(toString(toDate('2018-12-31', 'MM/dd/yy'))) -> true``  
* ``toString(4 == 20) -> 'false'``  
___
### <code>toTimestamp</code>
<code><b>toTimestamp(<i>&lt;string&gt;</i> : any, [<i>&lt;timestamp format&gt;</i> : string], [<i>&lt;time zone&gt;</i> : string]) => timestamp</b></code><br/><br/>
Hiermee wordt een teken reeks geconverteerd naar een tijds tempel met een optionele time stamp-notatie. Raadpleeg Java SimpleDateFormat voor alle mogelijke indelingen. Als de tijds tempel wordt wegge laten, wordt het standaard patroon gebruikt. jjjj-[M] M-[d] d uu: mm: SS [. f...] U kunt een optionele tijd zone door geven in de vorm ' GMT ', ' PST ', ' UTC ', ' America/Caymaneilanden '. Time Stamp ondersteunt een nauw keurigheid van Maxi maal milliseconden met waarde van 999Refer Java SimpleDateFormat voor beschik bare indelingen. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``toTimestamp('2016-12-31 00:12:00') -> toTimestamp('2016-12-31 00:12:00')``  
* ``toTimestamp('2016-12-31T00:12:00', 'yyyy-MM-dd\'T\'HH:mm:ss', 'PST') -> toTimestamp('2016-12-31 00:12:00')``  
* ``toTimestamp('12/31/2016T00:12:00', 'MM/dd/yyyy\'T\'HH:mm:ss') -> toTimestamp('2016-12-31 00:12:00')``  
* ``millisecond(toTimestamp('2019-02-03 05:19:28.871', 'yyyy-MM-dd HH:mm:ss.SSS')) -> 871``  
___
### <code>toUTC</code>
<code><b>toUTC(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => timestamp</b></code><br/><br/>
Hiermee wordt de tijds tempel geconverteerd naar UTC. U kunt een optionele tijd zone door geven in de vorm ' GMT ', ' PST ', ' UTC ', ' America/Caymaneilanden '. Het wordt standaard ingesteld op de huidige SimpleDateFormat van timezoneRefer Java voor beschik bare indelingen. https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html * ``toUTC(currentTimeStamp()) == toTimestamp('2050-12-12 19:18:12') -> false``  
* ``toUTC(currentTimeStamp(), 'Asia/Seoul') != toTimestamp('2050-12-12 19:18:12') -> true``  
___
### <code>translate</code>
<code><b>translate(<i>&lt;string to translate&gt;</i> : string, <i>&lt;lookup characters&gt;</i> : string, <i>&lt;replace characters&gt;</i> : string) => string</b></code><br/><br/>
Een reeks tekens vervangen door een andere set tekens in de teken reeks. Tekens hebben 1 tot 1 vervanging* ``translate('(bojjus)', '()', '[]') -> '[bojjus]'``  
* ``translate('(gunchus)', '()', '[') -> '[gunchus'``  
___
### <code>trim</code>
<code><b>trim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></code><br/><br/>
Hiermee wordt een teken reeks van voor loop-en volg tekens verkleind. Als de tweede para meter niet is opgegeven, wordt witruimte verkleind. Anders wordt het teken dat in de tweede para meter is opgegeven, verkleind* ``trim('  dumbo  ') -> 'dumbo'``  
* ``trim('!--!du!mbo!', '-!') -> 'du!mbo'``  
___
### <code>true</code>
<code><b>true() => boolean</b></code><br/><br/>
Retourneert altijd een waarde waar. Gebruik de functie syntaxis (True ()) als er een kolom is met de naam True* ``(10 + 20 == 30) -> true``  
* ``(10 + 20 == 30) -> true()``  
___
### <code>typeMatch</code>
<code><b>typeMatch(<i>&lt;type&gt;</i> : string, <i>&lt;base type&gt;</i> : string) => boolean</b></code><br/><br/>
Komt overeen met het type van de kolom. Kan alleen worden gebruikt in patroon expressies. getal komt overeen met kort, geheel getal, lang, dubbel, zwevend of decimaal teken, integraal komt overeen met short, integer, Long, breuken die overeenkomen met de datum of het tijds tempel type* ``typeMatch(type, 'number')``  
* ``typeMatch('date', 'datetime')``  
___
### <code>upper</code>
<code><b>upper(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Een teken reeks in hoofd letters* ``upper('bojjus') -> 'BOJJUS'``  
___
### <code>uuid</code>
<code><b>uuid() => string</b></code><br/><br/>
Retourneert de gegenereerde UUID* ``uuid()``  
___
### <code>weekOfYear</code>
<code><b>weekOfYear(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Hiermee wordt de week van het jaar opgehaald op basis van een datum* ``weekOfYear(toDate('2008-02-20')) -> 8``  
___
### <code>weeks</code>
<code><b>weeks(<i>&lt;value1&gt;</i> : integer) => long</b></code><br/><br/>
De duur in milliseconden voor het aantal weken* ``weeks(2) -> 1209600000L``  
___
### <code>xor</code>
<code><b>xor(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Logische XOR-operator. Zelfde als ^-operator* ``xor(true, false) -> true``  
* ``xor(true, true) -> false``  
* ``true ^ false -> true``  
___
### <code>year</code>
<code><b>year(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Hiermee wordt de jaar waarde van een datum opgehaald* ``year(toDate('2012-8-8')) -> 2012``  
##Statistische functies de volgende functies zijn alleen beschikbaar in de trans formaties aggregatie, Pivot, UNPIVOT en Window___
### <code>avg</code>
<code><b>avg(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Hiermee wordt het gemiddelde van de waarden van een kolom opgehaald* ``avg(sales)``  
___
### <code>avgIf</code>
<code><b>avgIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Op basis van een criterium wordt het gemiddelde van de waarden van een kolom opgehaald* ``avgIf(region == 'West', sales)``  
___
### <code>count</code>
<code><b>count([<i>&lt;value1&gt;</i> : any]) => long</b></code><br/><br/>
Hiermee wordt het totale aantal waarden opgehaald. Als de optionele kolom (men) is opgegeven, worden de NULL-waarden in het aantal genegeerd* ``count(custId)``  
* ``count(custId, custName)``  
* ``count()``  
* ``count(iif(isNull(custId), 1, NULL))``  
___
### <code>countDistinct</code>
<code><b>countDistinct(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : any], ...) => long</b></code><br/><br/>
Hiermee wordt het totale aantal afzonderlijke waarden van een set kolommen opgehaald* ``countDistinct(custId, custName)``  
___
### <code>countIf</code>
<code><b>countIf(<i>&lt;value1&gt;</i> : boolean, [<i>&lt;value2&gt;</i> : any]) => long</b></code><br/><br/>
Op basis van een criterium wordt het totale aantal waarden opgehaald. Als de optionele kolom is opgegeven, worden de NULL-waarden in het aantal genegeerd* ``countIf(state == 'CA' && commission < 10000, name)``  
___
### <code>covariancePopulation</code>
<code><b>covariancePopulation(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt de afwijking van de populatie tussen twee kolommen opgehaald* ``covariancePopulation(sales, profit)``  
___
### <code>covariancePopulationIf</code>
<code><b>covariancePopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number, <i>&lt;value3&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criterium, wordt de afwijking van de populatie van twee kolommen opgehaald* ``covariancePopulationIf(region == 'West', sales)``  
___
### <code>covarianceSample</code>
<code><b>covarianceSample(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt de voorbeeld covariantie van twee kolommen opgehaald* ``covarianceSample(sales, profit)``  
___
### <code>covarianceSampleIf</code>
<code><b>covarianceSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number, <i>&lt;value3&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criterium, wordt de voorbeeld covariantie van twee kolommen opgehaald* ``covarianceSampleIf(region == 'West', sales, profit)``  
___
### <code>first</code>
<code><b>first(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : boolean]) => any</b></code><br/><br/>
Hiermee wordt de eerste waarde van een kolom groep opgehaald. Als de tweede para meter ignoreNulls wordt wegge laten, wordt aangenomen dat deze ONWAAR is* ``first(sales)``  
* ``first(sales, false)``  
___
### <code>kurtosis</code>
<code><b>kurtosis(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt de kurtosis van een kolom opgehaald* ``kurtosis(sales)``  
___
### <code>kurtosisIf</code>
<code><b>kurtosisIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt de kurtosis van een kolom opgehaald op basis van een criterium* ``kurtosisIf(region == 'West', sales)``  
___
### <code>last</code>
<code><b>last(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : boolean]) => any</b></code><br/><br/>
Hiermee wordt de laatste waarde van een kolom groep opgehaald. Als de tweede para meter ignoreNulls wordt wegge laten, wordt aangenomen dat deze ONWAAR is* ``last(sales)``  
* ``last(sales, false)``  
___
### <code>max</code>
<code><b>max(<i>&lt;value1&gt;</i> : any) => any</b></code><br/><br/>
Hiermee wordt de maximum waarde van een kolom opgehaald* ``max(sales)``  
___
### <code>maxIf</code>
<code><b>maxIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Hiermee wordt de maximum waarde van een kolom opgehaald op basis van een criterium* ``maxIf(region == 'West', sales)``  
___
### <code>mean</code>
<code><b>mean(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Hiermee wordt het gemiddelde van de waarden van een kolom opgehaald. Hetzelfde als Gem* ``mean(sales)``  
___
### <code>meanIf</code>
<code><b>meanIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Op basis van een criterium wordt het gemiddelde van de waarden van een kolom opgehaald. Hetzelfde als avgIf* ``meanIf(region == 'West', sales)``  
___
### <code>min</code>
<code><b>min(<i>&lt;value1&gt;</i> : any) => any</b></code><br/><br/>
Hiermee wordt de minimum waarde van een kolom opgehaald* ``min(sales)``  
___
### <code>minIf</code>
<code><b>minIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Hiermee wordt de minimum waarde van een kolom opgehaald op basis van een criterium* ``minIf(region == 'West', sales)``  
___
### <code>skewness</code>
<code><b>skewness(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt de scheefheid van een kolom opgehaald* ``skewness(sales)``  
___
### <code>skewnessIf</code>
<code><b>skewnessIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criterium, haalt de scheefheid van een kolom op* ``skewnessIf(region == 'West', sales)``  
___
### <code>stddev</code>
<code><b>stddev(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt de standaard deviatie van een kolom opgehaald* ``stdDev(sales)``  
___
### <code>stddevIf</code>
<code><b>stddevIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criterium, wordt de standaard deviatie van een kolom opgehaald* ``stddevIf(region == 'West', sales)``  
___
### <code>stddevPopulation</code>
<code><b>stddevPopulation(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt de standaard deviatie van de populatie van een kolom opgehaald* ``stddevPopulation(sales)``  
___
### <code>stddevPopulationIf</code>
<code><b>stddevPopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criterium, wordt de standaard deviatie van de populatie van een kolom opgehaald* ``stddevPopulationIf(region == 'West', sales)``  
___
### <code>stddevSample</code>
<code><b>stddevSample(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt de voor beeld-standaard deviatie van een kolom opgehaald* ``stddevSample(sales)``  
___
### <code>stddevSampleIf</code>
<code><b>stddevSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criterium, wordt de standaard afwijking van een kolom opgehaald* ``stddevSampleIf(region == 'West', sales)``  
___
### <code>sum</code>
<code><b>sum(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Hiermee wordt de cumulatieve som van een numerieke kolom opgehaald* ``sum(col)``  
___
### <code>sumDistinct</code>
<code><b>sumDistinct(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Hiermee wordt de cumulatieve som opgehaald van afzonderlijke waarden van een numerieke kolom* ``sumDistinct(col)``  
___
### <code>sumDistinctIf</code>
<code><b>sumDistinctIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Op basis van criteria haalt de cumulatieve som van een numerieke kolom op. De voor waarde kan worden gebaseerd op elke kolom* ``sumDistinctIf(state == 'CA' && commission < 10000, sales)``  
* ``sumDistinctIf(true, sales)``  
___
### <code>sumIf</code>
<code><b>sumIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Op basis van criteria haalt de cumulatieve som van een numerieke kolom op. De voor waarde kan worden gebaseerd op elke kolom* ``sumIf(state == 'CA' && commission < 10000, sales)``  
* ``sumIf(true, sales)``  
___
### <code>variance</code>
<code><b>variance(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt de variantie van een kolom opgehaald* ``variance(sales)``  
___
### <code>varianceIf</code>
<code><b>varianceIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt de variantie van een kolom opgehaald op basis van een criterium* ``varianceIf(region == 'West', sales)``  
___
### <code>variancePopulation</code>
<code><b>variancePopulation(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt de populatie variantie van een kolom opgehaald* ``variancePopulation(sales)``  
___
### <code>variancePopulationIf</code>
<code><b>variancePopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criterium, wordt de populatie variantie van een kolom opgehaald* ``variancePopulationIf(region == 'West', sales)``  
___
### <code>varianceSample</code>
<code><b>varianceSample(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Hiermee wordt de onzuivere variantie van een kolom opgehaald* ``varianceSample(sales)``  
___
### <code>varianceSampleIf</code>
<code><b>varianceSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criterium, wordt de onzuivere variantie van een kolom opgehaald* ``varianceSampleIf(region == 'West', sales)``  
##Venster functies de volgende functies zijn alleen beschikbaar in venster transformaties___
### <code>cumeDist</code>
<code><b>cumeDist() => integer</b></code><br/><br/>
De functie CumeDist berekent de positie van een waarde ten opzichte van alle waarden in de partitie. Het resultaat is het aantal rijen dat voorafgaat aan of gelijk is aan de huidige rij in de volg orde van de partitie gedeeld door het totaal aantal rijen in de venster partitie. Alle gelijke waarden in de volg orde worden geëvalueerd op dezelfde positie.  
* ``cumeDist()``  
___
### <code>denseRank</code>
<code><b>denseRank() => integer</b></code><br/><br/>
Hiermee wordt de positie van een waarde berekend in een groep waarden die is opgegeven in de component order by van een venster. Het resultaat is één plus het aantal rijen dat voorafgaat aan of gelijk is aan de huidige rij in de volg orde van de partitie. De waarden veroorzaken geen hiaten in de reeks. Een dichte rang werkt zelfs wanneer er geen gegevens worden gesorteerd en er wordt gekeken naar een wijziging in waarden* ``denseRank()``  
___
### <code>lag</code>
<code><b>lag(<i>&lt;value&gt;</i> : any, [<i>&lt;number of rows to look before&gt;</i> : number], [<i>&lt;default value&gt;</i> : any]) => any</b></code><br/><br/>
Hiermee wordt de waarde opgehaald van de eerste para meter die n rijen voor de huidige rij is geëvalueerd. De tweede para meter is het aantal rijen dat u wilt terugkijken en de standaard waarde is 1. Als er niet zoveel rijen zijn, wordt een waarde van Null geretourneerd, tenzij een standaard waarde is opgegeven* ``lag(amount, 2)``  
* ``lag(amount, 2000, 100)``  
___
### <code>lead</code>
<code><b>lead(<i>&lt;value&gt;</i> : any, [<i>&lt;number of rows to look after&gt;</i> : number], [<i>&lt;default value&gt;</i> : any]) => any</b></code><br/><br/>
Hiermee wordt de waarde opgehaald van de eerste para meter, geëvalueerd n rijen na de huidige rij. De tweede para meter is het aantal rijen dat u wilt zoeken en de standaard waarde is 1. Als er niet zoveel rijen zijn, wordt een waarde van Null geretourneerd, tenzij een standaard waarde is opgegeven* ``lead(amount, 2)``  
* ``lead(amount, 2000, 100)``  
___
### <code>nTile</code>
<code><b>nTile([<i>&lt;value1&gt;</i> : integer]) => integer</b></code><br/><br/>
De functie NTile verdeelt de rijen voor elke venster partitie `n` in buckets, variërend van 1 tot de `n`meeste. Bucket waarden kunnen Maxi maal 1 zijn. Als het aantal rijen in de partitie niet gelijkmatig is verdeeld over het aantal buckets, worden de waarden van de rest gedistribueerd per Bucket, beginnend met de eerste Bucket. De functie NTile is handig voor het berekenen van tertiles, kwartielen, deciles en andere algemene samenvattings statistieken. De functie berekent twee variabelen tijdens de initialisatie: er wordt één extra rij aan de grootte van een gewone Bucket toegevoegd. Beide variabelen zijn gebaseerd op de grootte van de huidige partitie. Tijdens het berekenings proces houdt de functie het huidige rijnummer, het huidige Bucket nummer en het rijnummer waarbij de Bucket wordt gewijzigd (bucketThreshold) bij. Wanneer het huidige rijnummer de Bucket drempel bereikt, wordt de Bucket waarde verhoogd met één en wordt de drempel verhoogd met de Bucket grootte (plus één extra als de huidige Bucket wordt opgevuld).  
* ``nTile()``  
* ``nTile(numOfBuckets)``  
___
### <code>rank</code>
<code><b>rank() => integer</b></code><br/><br/>
Hiermee wordt de positie van een waarde berekend in een groep waarden die is opgegeven in de component order by van een venster. Het resultaat is één plus het aantal rijen dat voorafgaat aan of gelijk is aan de huidige rij in de volg orde van de partitie. De waarden veroorzaken hiaten in de reeks. Rang werkt zelfs wanneer er geen gegevens worden gesorteerd en er wordt gekeken naar wijziging in waarden* ``rank()``  
___
### <code>rowNumber</code>
<code><b>rowNumber() => integer</b></code><br/><br/>
Hiermee wijst u een sequentiële rijnummer nummering toe voor rijen in een venster, te beginnen met 1* ``rowNumber()``  

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over het gebruik van Expression Builder](concepts-data-flow-expression-builder.md).
