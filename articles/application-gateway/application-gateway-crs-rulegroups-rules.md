---
title: Azure Application Gateway web application firewall CRS-regelgroepen en -regels regel
description: Deze pagina bevat informatie over web application firewall CRS-regelgroepen en -regels.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 4/11/2019
ms.author: victorh
ms.openlocfilehash: 0ad5cc76c0f4631fd60eea7d0a57e4740b6a9db3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60832911"
---
# <a name="web-application-firewall-crs-rule-groups-and-rules"></a>Web application firewall CRS-regelgroepen en -regels

Web application firewall (WAF) voor Application Gateway beschermt webtoepassingen tegen veelvoorkomende beveiligingsproblemen. Dit wordt gedaan door regels die zijn gedefinieerd op basis van de OWASP core rule set 3.0 of 2.2.9. Deze regels kunnen worden uitgeschakeld op basis van de regel door de regel. In dit artikel bevat de huidige regels en rulesets aangeboden.

De volgende regelgroepen en regels zijn beschikbaar bij het gebruik van Application Gateway met web application firewall.

# <a name="owasp-30tabowasp3"></a>[OWASP 3.0](#tab/owasp3)

## <a name="owasp30"></a> Regelsets

### <a name="General"></a> <p x-ms-format-detection="none">Algemeen</p>

|RuleId|Description|
|---|---|
|200004|Mogelijk is gedeeltelijk niet-overeenkomende grens.|

### <a name="crs911"></a> <p x-ms-format-detection="none">REQUEST-911-METHOD-ENFORCEMENT</p>

|RuleId|Description|
|---|---|
|911100|Methode is niet toegestaan door het beleid|


### <a name="crs913"></a> <p x-ms-format-detection="none">REQUEST-913-SCANNER-DETECTION</p>

|RuleId|Description|
|---|---|
|913100|Gebruiker-Agent die is gekoppeld aan een beveiligingsscanner gevonden|
|913110|Aanvraagheader die zijn gekoppeld aan een beveiligingsscanner gevonden|
|913120|Aanvraag filename/argument dat is gekoppeld aan een beveiligingsscanner gevonden|
|913101|Gebruiker-Agent die is gekoppeld aan het uitvoeren van scripts/algemene HTTP-client gevonden|
|913102|Gebruiker-Agent die is gekoppeld aan web crawler/bot gevonden|

### <a name="crs920"></a> <p x-ms-format-detection="none">REQUEST-920-PROTOCOL-ENFORCEMENT</p>

|RuleId|Description|
|---|---|
|920100|Ongeldige HTTP-aanvraag regel|
|920130|Parseren van de hoofdtekst van de aanvraag is mislukt.|
|920140|Gedeeltelijk aanvraagtekst strikte validatie is mislukt|
|920160|Content-Length-HTTP-header is niet numeriek.|
|920170|GET- of HEAD aanvragen met inhoud van de hoofdtekst.|
|920180|POST-aanvraag Content-Length-Header ontbreekt.|
|920190|Bereik = Ongeldige laatste bytewaarde.|
|920210|Meerdere conflicterende/koptekst verbindingsgegevens gevonden.|
|920220|URL-codering: Aanvalspoging misbruik|
|920240|URL-codering: Aanvalspoging misbruik|
|920250|UTF8-Codering Aanvalspoging misbruik|
|920260|Unicode-volledige/halve breedte misbruik Aanvalspoging|
|920270|Ongeldig teken in de aanvraag (null-teken)|
|920280|Aanvraag voor een Host-Header ontbreekt|
|920290|Lege Host-Header|
|920310|Aanvraag heeft een lege Header accepteren|
|920311|Aanvraag heeft een lege Header accepteren|
|920330|Lege gebruiker Agent-Header|
|920340|Aanvragen die inhoud, maar de header Content-Type ontbreekt|
|920350|Host-header is een numerieke IP-adres|
|920380|Te veel argumenten in de aanvraag|
|920360|Argumentnaam te lang.|
|920370|Argumentwaarde is te lang|
|920390|Totaal aantal argumenten is te groot|
|920400|Grootte van het geüploade bestand is te groot|
|920410|Totaal aantal geüploade bestanden te groot|
|920420|Inhoud van het type is niet toegestaan door het beleid|
|920430|HTTP-protocolversie is niet toegestaan door het beleid|
|920440|URL-bestandsextensie wordt beperkt door het beleid|
|920450|HTTP-header is beperkt door het beleid (%@{MATCHED_VAR})|
|920200|Bereik = te veel velden (6 of meer)|
|920201|Bereik = te veel velden voor PDF-aanvraag (35 of meer)|
|920230|Meerdere URL-codering wordt gedetecteerd|
|920300|Aanvraag ontbreekt een Header accepteren|
|920271|Ongeldig teken in de aanvraag (niet-afdrukbare tekens)|
|920320|Ontbrekende Header van de gebruiker Agent|
|920272|Ongeldig teken in de aanvraag (buiten afdrukbare tekens hieronder 127 ascii)|
|920202|Bereik = te veel velden voor PDF-aanvraag (6 of meer)|
|920273|Ongeldig teken in de aanvraag (buiten zeer strikte set)|
|920274|Ongeldig teken in de aanvraagheaders (buiten zeer strikte set)|
|920460|Abnormaal escape-tekens|

### <a name="crs921"></a> <p x-ms-format-detection="none">REQUEST-921-PROTOCOL-ATTACK</p>

|RuleId|Description|
|---|---|
|921100|Aanval smokkelen van HTTP-aanvraag.|
|921110|HTTP-aanvraag ' smokkelen '-aanval|
|921120|HTTP-antwoord splitsen aanval|
|921130|HTTP-antwoord splitsen aanval|
|921140|HTTP-Header ingebracht bij een aanval via kopteksten|
|921150|HTTP-Header ingebracht bij een aanval via nettolading (CR/LF gedetecteerd)|
|921160|HTTP-Header ingebracht bij een aanval via nettolading (CR/LF en headernaam gedetecteerd)|
|921151|HTTP-Header ingebracht bij een aanval via nettolading (CR/LF gedetecteerd)|
|921170|HTTP-Parameter verontreiniging|
|921180|HTTP-Parameter verontreiniging (% @{TX.1})|

### <a name="crs930"></a> <p x-ms-format-detection="none">REQUEST-930-APPLICATION-ATTACK-LFI</p>

|RuleId|Description|
|---|---|
|930100|Pad verandering aanval (/... /)|
|930110|Pad verandering aanval (/... /)|
|930120|De poging tot toegang OS-bestand|
|930130|De poging tot toegang beperkt bestand|

### <a name="crs931"></a> <p x-ms-format-detection="none">REQUEST-931-APPLICATION-ATTACK-RFI</p>

|RuleId|Description|
|---|---|
|931100|Mogelijke Remote File Inclusion (RFI) aanval URL-Parameter met behulp van IP-adres =|
|931110|Mogelijke Remote File Inclusion (RFI) aanval = algemene RFI kwetsbaar parameternaam gebruikt w/URL nettolading|
|931120|Mogelijke Remote File Inclusion (RFI) aanval = URL Payload gebruikt w/navolgende vraag Mark teken (?)|
|931130|Mogelijke Remote File Inclusion (RFI) aanval uit domein/koppeling =|

### <a name="crs932"></a> <p x-ms-format-detection="none">REQUEST-932-APPLICATION-ATTACK-RCE</p>

|RuleId|Description|
|---|---|
|932120|Uitvoering van externe opdracht Windows PowerShell-opdracht gevonden =|
|932130|Uitvoering van externe opdracht = Unix Shell-expressie gevonden|
|932140|Uitvoering van externe opdracht Windows = voor / als de opdracht gevonden|
|932160|Uitvoering van externe opdracht = Unix Shell Code gevonden|
|932170|Uitvoering van externe opdracht = Shellshock (CVE-2014-6271)|
|932171|Uitvoering van externe opdracht = Shellshock (CVE-2014-6271)|

### <a name="crs933"></a> <p x-ms-format-detection="none">REQUEST-933-APPLICATION-ATTACK-PHP</p>

|RuleId|Description|
|---|---|
|933100|PHP-injectie-aanval = openen/eindcode gevonden|
|933110|PHP-injectie-aanval = PHP-Script uploaden van het bestand gevonden|
|933120|PHP-injectie-aanval configuratie richtlijn gevonden =|
|933130|PHP-injectie-aanval = variabelen gevonden|
|933150|PHP-injectie-aanval = de naam van de functie met een hoog risico PHP gevonden|
|933160|PHP-injectie-aanval met een hoog risico PHP-functieaanroep gevonden =|
|933180|PHP-injectie-aanval = variabele functie-oproep|
|933151|PHP-injectie-aanval = de naam van de functie gemiddeld risico PHP gevonden|
|933131|PHP-injectie-aanval = variabelen gevonden|
|933161|PHP-injectie-aanval = lage waarde PHP-functieaanroep gevonden|
|933111|PHP-injectie-aanval = PHP-Script uploaden van het bestand gevonden|

### <a name="crs941"></a> <p x-ms-format-detection="none">REQUEST-941-APPLICATION-ATTACK-XSS</p>

|RuleId|Description|
|---|---|
|941100|XSS-aanval gedetecteerd via libinjection|
|941110|XSS-Filter - categorie 1 = Script Tag Vector|
|941130|XSS-Filter - categorie 3 = kenmerk Vector|
|941140|XSS-Filter - categorie 4 = Vector Javascript-URI|
|941150|XSS-Filter - categorie 5 = niet-toegestane HTML-kenmerken|
|941180|Node-Validator Blacklist Keywords|
|941190|Met behulp van opmaakmodellen XSS|
|941200|XSS met VML frames|
|941210|XSS met verborgen Javascript|
|941220|XSS met verborgen VB-Script|
|941230|XSS met behulp van 'ingesloten' tag|
|941240|XSS met kenmerk 'import' of 'implementatie'|
|941260|XSS met behulp van ' metalabel '|
|941270|XSS href 'koppelen' met|
|941280|XSS met 'base' label|
|941290|XSS met behulp van 'applet'-tag|
|941300|XSS met behulp van de tag 'object'|
|941310|US-ASCII-onjuiste codering XSS-Filter - aanval gedetecteerd.|
|941330|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|941340|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|941350|UTF-7 codering IE XSS - aanval gedetecteerd.|
|941320|Mogelijke XSS-aanval gedetecteerd - Handler voor HTML-code|

### <a name="crs942"></a> <p x-ms-format-detection="none">REQUEST-942-APPLICATION-ATTACK-SQLI</p>

|RuleId|Description|
|---|---|
|942100|SQL-injectie-aanval gedetecteerd via libinjection|
|942110|SQL-injectieaanvallen verminderen: Algemene webweergave gedetecteerde testen|
|942130|SQL-injectieaanvallen verminderen: SQL-Tautology gedetecteerd.|
|942140|SQL-injectieaanvallen = algemene DB namen gedetecteerd|
|942160|Blind sqli tests met sleep() of benchmark() detecteert.|
|942170|Detecteert benchmark en slaapstand voor SQL-injectie probeert, waaronder voorwaardelijke query 's|
|942190|Detecteert MSSQL tot uitvoering van code en pogingen verzamelen van informatie|
|942200|Detecteert MySQL Opmerking- / ruimte verborgen injecties en backtick beëindiging|
|942230|Detecteert voorwaardelijke SQL-injectie pogingen|
|942260|Detecteert basic SQL-verificatie overslaan probeert 2/3|
|942270|Op zoek naar basic sql-injectie. Veelvoorkomende aanval-tekenreeks voor mysql oracle en andere.|
|942290|Basic MongoDB SQL injection pogingen gevonden|
|942300|MySQL-opmerkingen, voorwaarden en ch (a) r injecties gedetecteerd|
|942320|Detecteert MySQL en PostgreSQL opgeslagen procedure/functie injecties|
|942330|Detecteert klassieke SQL-injectie probings 1/2|
|942340|Detecteert basic SQL-verificatie overslaan probeert 3/3|
|942350|MySQL UDF-injectie en andere manipulatie gegevensstructuur/detecteert pogingen|
|942360|Detecteert samengevoegde basic SQL-injectie en SQLLFI pogingen|
|942370|Detecteert klassieke SQL-injectie probings 2/2|
|942150|SQL-injectieaanvallen|
|942410|SQL-injectieaanvallen|
|942430|Beperkte Anomaliedetectie SQL-teken (argumenten): het aantal speciale tekens overschreden (12)|
|942440|Volgorde van SQL-opmerking is gedetecteerd.|
|942450|SQL hexadecimale codering geïdentificeerd|
|942251|Detecteert de HAVING-injecties|
|942460|Waarschuwing META teken Anomaliedetectie - terugkerende niet Word tekens|

### <a name="crs943"></a> <p x-ms-format-detection="none">REQUEST-943-APPLICATION-ATTACK-SESSION-FIXATION</p>

|RuleId|Description|
|---|---|
|943100|Mogelijke sessie vastlegging aanval = cookiewaarden instellen in HTML-code|
|943110|Mogelijke sessie vastlegging aanval = naam van de sessie-id-Parameter met de verwijzende site uit het domein|
|943120|Mogelijke sessie vastlegging aanval = naam van de sessie-id-Parameter met geen verwijzende site|

# <a name="owasp-229tabowasp2"></a>[OWASP 2.2.9](#tab/owasp2)

## <a name="owasp229"></a> Regelsets

### <a name="crs20"></a> crs_20_protocol_violations

|RuleId|Description|
|---|---|
|960911|Ongeldige HTTP-aanvraag regel|
|981227|Apache-fout = Ongeldige URI in de aanvraag.|
|960912|Parseren van de hoofdtekst van de aanvraag is mislukt.|
|960914|Gedeeltelijk aanvraagtekst strikte validatie is mislukt|
|960915|Meerdelige parser heeft de grens van een mogelijk niet-overeenkomende gedetecteerd.|
|960016|Content-Length-HTTP-header is niet numeriek.|
|960011|GET- of HEAD aanvragen met inhoud van de hoofdtekst.|
|960012|POST-aanvraag Content-Length-Header ontbreekt.|
|960902|Ongeldig gebruik van de identiteit codering.|
|960022|Verwacht dat de Header is niet toegestaan voor de HTTP 1.0.|
|960020|Pragma-koptekst vereist een Cache-Control-Header voor HTTP/1.1-aanvragen.|
|958291|Bereik = veld bestaat en begint met 0.|
|958230|Bereik = Ongeldige laatste bytewaarde.|
|958295|Meerdere conflicterende/koptekst verbindingsgegevens gevonden.|
|950107|URL-codering: Aanvalspoging misbruik|
|950109|Meerdere URL-codering wordt gedetecteerd|
|950108|URL-codering: Aanvalspoging misbruik|
|950801|UTF8-Codering Aanvalspoging misbruik|
|950116|Unicode-volledige/halve breedte misbruik Aanvalspoging|
|960901|Ongeldig teken in de aanvraag|
|960018|Ongeldig teken in de aanvraag|

### <a name="crs21"></a> crs_21_protocol_anomalies

|RuleId|Description|
|---|---|
|960008|Aanvraag voor een Host-Header ontbreekt|
|960007|Lege Host-Header|
|960015|Aanvraag ontbreekt een Header accepteren|
|960021|Aanvraag heeft een lege Header accepteren|
|960009|Aanvraag voor een gebruiker Agent-Header ontbreekt|
|960006|Lege gebruiker Agent-Header|
|960904|Aanvragen die inhoud, maar de header Content-Type ontbreekt|
|960017|Host-header is een numerieke IP-adres|

### <a name="crs23"></a> crs_23_request_limits

|RuleId|Description|
|---|---|
|960209|Argumentnaam te lang.|
|960208|Argumentwaarde is te lang|
|960335|Te veel argumenten in de aanvraag|
|960341|Totaal aantal argumenten is te groot|
|960342|Grootte van het geüploade bestand is te groot|
|960343|Totaal aantal geüploade bestanden te groot|

### <a name="crs30"></a> crs_30_http_policy

|RuleId|Description|
|---|---|
|960032|Methode is niet toegestaan door het beleid|
|960010|Inhoud van het type is niet toegestaan door het beleid|
|960034|HTTP-protocolversie is niet toegestaan door het beleid|
|960035|URL-bestandsextensie wordt beperkt door het beleid|
|960038|HTTP-header is beperkt door het beleid|

### <a name="crs35"></a> crs_35_bad_robots

|RuleId|Description|
|---|---|
|990002|Aanvraag geeft aan dat een beveiligingsscanner gescand de Site|
|990901|Aanvraag geeft aan dat een beveiligingsscanner gescand de Site|
|990902|Aanvraag geeft aan dat een beveiligingsscanner gescand de Site|
|990012|Rogue-website crawler|

### <a name="crs40"></a> crs_40_generic_attacks

|RuleId|Description|
|---|---|
|960024|Waarschuwing META teken Anomaliedetectie - terugkerende niet Word tekens|
|950008|Injecteren van niet-gedocumenteerde ColdFusion-Tags|
|950010|LDAP-injectie-aanval|
|950011|SSI injectie-aanval|
|950018|Universele PDF XSS-URL gedetecteerd.|
|950019|E-injectie-aanval|
|950012|Aanval smokkelen van HTTP-aanvraag.|
|950910|HTTP-antwoord splitsen aanval|
|950911|HTTP-antwoord splitsen aanval|
|950117|Remote File Inclusion aanval|
|950118|Remote File Inclusion aanval|
|950119|Remote File Inclusion aanval|
|950120|Mogelijke Remote File Inclusion (RFI) aanval uit domein/koppeling =|
|981133|Regel 981133|
|981134|Regel 981134|
|950009|Sessie vastlegging aanval|
|950003|Sessiefixatie|
|950000|Sessiefixatie|
|950005|De poging tot toegang extern bestand|
|950002|Toegang tot het systeem opdracht|
|950006|System opdracht injectie|
|959151|PHP-injectie-aanval|
|958976|PHP-injectie-aanval|
|958977|PHP-injectie-aanval|

### <a name="crs41sql"></a> crs_41_sql_injection_attacks

|RuleId|Description|
|---|---|
|981231|Volgorde van SQL-opmerking is gedetecteerd.|
|981260|SQL hexadecimale codering geïdentificeerd|
|981320|SQL-injectieaanvallen = algemene DB namen gedetecteerd|
|981300|Regel 981300|
|981301|Regel 981301|
|981302|Regel 981302|
|981303|Regel 981303|
|981304|Regel 981304|
|981305|Regel 981305|
|981306|Regel 981306|
|981307|Regel 981307|
|981308|Regel 981308|
|981309|Regel 981309|
|981310|Regel 981310|
|981311|Regel 981311|
|981312|Regel 981312|
|981313|Regel 981313|
|981314|Regel 981314|
|981315|Regel 981315|
|981316|Regel 981316|
|981317|SQL SELECT-instructie Anomaly Detection waarschuwing|
|950007|Blind SQL-injectieaanvallen|
|950001|SQL-injectieaanvallen|
|950908|SQL-injectieaanvallen verminderen.|
|959073|SQL-injectieaanvallen|
|981272|Blind sqli tests met sleep() of benchmark() detecteert.|
|981250|Detecteert benchmark en slaapstand voor SQL-injectie probeert, waaronder voorwaardelijke query 's|
|981241|Detecteert voorwaardelijke SQL-injectie pogingen|
|981276|Op zoek naar basic sql-injectie. Veelvoorkomende aanval-tekenreeks voor mysql oracle en andere.|
|981270|Basic MongoDB SQL injection pogingen gevonden|
|981253|Detecteert MySQL en PostgreSQL opgeslagen procedure/functie injecties|
|981251|MySQL UDF-injectie en andere manipulatie gegevensstructuur/detecteert pogingen|

### <a name="crs41xss"></a> crs_41_xss_attacks

|RuleId|Description|
|---|---|
|973336|XSS-Filter - categorie 1 = Script Tag Vector|
|973338|XSS-Filter - categorie 3 = Vector Javascript-URI|
|981136|Regel 981136|
|981018|Regel 981018|
|958016|Cross-site Scripting (XSS)-aanval|
|958414|Cross-site Scripting (XSS)-aanval|
|958032|Cross-site Scripting (XSS)-aanval|
|958026|Cross-site Scripting (XSS)-aanval|
|958027|Cross-site Scripting (XSS)-aanval|
|958054|Cross-site Scripting (XSS)-aanval|
|958418|Cross-site Scripting (XSS)-aanval|
|958034|Cross-site Scripting (XSS)-aanval|
|958019|Cross-site Scripting (XSS)-aanval|
|958013|Cross-site Scripting (XSS)-aanval|
|958408|Cross-site Scripting (XSS)-aanval|
|958012|Cross-site Scripting (XSS)-aanval|
|958423|Cross-site Scripting (XSS)-aanval|
|958002|Cross-site Scripting (XSS)-aanval|
|958017|Cross-site Scripting (XSS)-aanval|
|958007|Cross-site Scripting (XSS)-aanval|
|958047|Cross-site Scripting (XSS)-aanval|
|958410|Cross-site Scripting (XSS)-aanval|
|958415|Cross-site Scripting (XSS)-aanval|
|958022|Cross-site Scripting (XSS)-aanval|
|958405|Cross-site Scripting (XSS)-aanval|
|958419|Cross-site Scripting (XSS)-aanval|
|958028|Cross-site Scripting (XSS)-aanval|
|958057|Cross-site Scripting (XSS)-aanval|
|958031|Cross-site Scripting (XSS)-aanval|
|958006|Cross-site Scripting (XSS)-aanval|
|958033|Cross-site Scripting (XSS)-aanval|
|958038|Cross-site Scripting (XSS)-aanval|
|958409|Cross-site Scripting (XSS)-aanval|
|958001|Cross-site Scripting (XSS)-aanval|
|958005|Cross-site Scripting (XSS)-aanval|
|958404|Cross-site Scripting (XSS)-aanval|
|958023|Cross-site Scripting (XSS)-aanval|
|958010|Cross-site Scripting (XSS)-aanval|
|958411|Cross-site Scripting (XSS)-aanval|
|958422|Cross-site Scripting (XSS)-aanval|
|958036|Cross-site Scripting (XSS)-aanval|
|958000|Cross-site Scripting (XSS)-aanval|
|958018|Cross-site Scripting (XSS)-aanval|
|958406|Cross-site Scripting (XSS)-aanval|
|958040|Cross-site Scripting (XSS)-aanval|
|958052|Cross-site Scripting (XSS)-aanval|
|958037|Cross-site Scripting (XSS)-aanval|
|958049|Cross-site Scripting (XSS)-aanval|
|958030|Cross-site Scripting (XSS)-aanval|
|958041|Cross-site Scripting (XSS)-aanval|
|958416|Cross-site Scripting (XSS)-aanval|
|958024|Cross-site Scripting (XSS)-aanval|
|958059|Cross-site Scripting (XSS)-aanval|
|958417|Cross-site Scripting (XSS)-aanval|
|958020|Cross-site Scripting (XSS)-aanval|
|958045|Cross-site Scripting (XSS)-aanval|
|958004|Cross-site Scripting (XSS)-aanval|
|958421|Cross-site Scripting (XSS)-aanval|
|958009|Cross-site Scripting (XSS)-aanval|
|958025|Cross-site Scripting (XSS)-aanval|
|958413|Cross-site Scripting (XSS)-aanval|
|958051|Cross-site Scripting (XSS)-aanval|
|958420|Cross-site Scripting (XSS)-aanval|
|958407|Cross-site Scripting (XSS)-aanval|
|958056|Cross-site Scripting (XSS)-aanval|
|958011|Cross-site Scripting (XSS)-aanval|
|958412|Cross-site Scripting (XSS)-aanval|
|958008|Cross-site Scripting (XSS)-aanval|
|958046|Cross-site Scripting (XSS)-aanval|
|958039|Cross-site Scripting (XSS)-aanval|
|958003|Cross-site Scripting (XSS)-aanval|
|973300|Mogelijke XSS-aanval gedetecteerd - Handler voor HTML-code|
|973301|XSS-aanval gedetecteerd|
|973302|XSS-aanval gedetecteerd|
|973303|XSS-aanval gedetecteerd|
|973304|XSS-aanval gedetecteerd|
|973305|XSS-aanval gedetecteerd|
|973306|XSS-aanval gedetecteerd|
|973307|XSS-aanval gedetecteerd|
|973308|XSS-aanval gedetecteerd|
|973309|XSS-aanval gedetecteerd|
|973311|XSS-aanval gedetecteerd|
|973313|XSS-aanval gedetecteerd|
|973314|XSS-aanval gedetecteerd|
|973331|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973315|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973330|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973327|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973326|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973346|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973345|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973324|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973323|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973348|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973321|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973320|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973318|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973317|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973329|Filters in Internet Explorer XSS - aanval gedetecteerd.|
|973328|Filters in Internet Explorer XSS - aanval gedetecteerd.|

### <a name="crs42"></a> crs_42_tight_security

|RuleId|Description|
|---|---|
|950103|Pad verandering aanval|

### <a name="crs45"></a> crs_45_trojans

|RuleId|Description|
|---|---|
|950110|Achterdeurtoegang|
|950921|Achterdeurtoegang|
|950922|Achterdeurtoegang|

---

## <a name="next-steps"></a>Volgende stappen

Informatie over het uitschakelen van de WAF-regels: [WAF-regels aanpassen](application-gateway-customize-waf-rules-portal.md)