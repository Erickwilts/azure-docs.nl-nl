---
title: Aangepaste regels voor Azure Web Application firewall (WAF) v2
description: Dit artikel bevat een overzicht van aangepaste WAF-regels (Web Application firewall) in Azure-toepassing gateway.
services: application-gateway
ms.topic: article
author: vhorne
ms.service: application-gateway
ms.date: 6/18/2019
ms.author: victorh
ms.openlocfilehash: 9c04f805cf410d2306eda76c84a201a67b022b84
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68716629"
---
# <a name="custom-rules-for-web-application-firewall-v2"></a>Aangepaste regels voor Web Application Firewall versie 2

De Azure-toepassing gateway Web Application firewall (WAF) v2 wordt geleverd met een vooraf geconfigureerde, door een platform beheerde ruleset die bescherming biedt tegen een groot aantal verschillende soorten aanvallen. Deze aanvallen omvatten cross-site scripting, SQL-injectie en andere. Als u een WAF-beheerder bent, kunt u uw eigen regels schrijven om de regels voor basisregel sets (CRS) te verbeteren. Uw regels kunnen het aangevraagde verkeer blok keren of toestaan op basis van de overeenkomende criteria.

Met aangepaste regels kunt u uw eigen regels maken die worden geëvalueerd voor elke aanvraag die via de WAF wordt door gegeven. Deze regels bevatten een hogere prioriteit dan de rest van de regels in de beheerde regel sets. De aangepaste regels bevatten de naam van de regel, de prioriteit van de regel en een matrix met overeenkomende voor waarden. Als aan deze voor waarden wordt voldaan, wordt er een actie ondernomen (om toe te staan of te blok keren).

U kunt bijvoorbeeld alle aanvragen van een IP-adres in het bereik 192.168.5.4/24 blok keren. In deze regel is de operator *IPMatch*, het matchValues is het IP-adres bereik (192.168.5.4/24) en de actie om het verkeer te blok keren. U kunt ook de naam en prioriteit van de regel instellen.

Aangepaste regels bieden ondersteuning voor het gebruik van samengestelde logica om geavanceerde regels te maken die voldoen aan uw beveiligings behoeften. Bijvoorbeeld (Condition 1 **en** condition 2) **of** condition 3).  Dit voor beeld betekent dat als wordt voldaan aan voor waarde 1 **en** voor waarde 2, **of** als aan voor waarde 3 wordt voldaan, de WAF de actie moet uitvoeren die is opgegeven in de aangepaste regel.

Verschillende overeenkomende voor waarden binnen dezelfde regel worden altijd samengesteld met **en**. Bijvoorbeeld verkeer blok keren van een specifiek IP-adres en alleen als ze een bepaalde browser gebruiken.

Als u **of** twee verschillende voor waarden wilt, moeten de twee voor waarden in verschillende regels zijn. Bijvoorbeeld verkeer blok keren van een specifiek IP-adres of verkeer blok keren als ze een specifieke browser gebruiken.

> [!NOTE]
> Het maximum aantal aangepaste WAF-regels is 100. Zie [Azure-abonnement en service limieten, quota's en beperkingen](../azure-subscription-service-limits.md#application-gateway-limits)voor meer informatie over Application Gateway limieten.

Reguliere expressies worden ook ondersteund in aangepaste regels, net als in de CRS-rules. Voor voor beelden van deze, zie voor beelden 3 en 5 in [aangepaste Web Application firewall regels maken en gebruiken](create-custom-waf-rules.md).

## <a name="allowing-vs-blocking"></a>Toestaan versus blok keren

Het toestaan en blok keren van verkeer is eenvoudig met aangepaste regels. U kunt bijvoorbeeld al het verkeer dat afkomstig is van een bereik van IP-adressen blok keren. U kunt een andere regel maken om verkeer toe te staan als de aanvraag afkomstig is van een specifieke browser.

Als u iets wilt toestaan, moet `-Action` u ervoor zorgen dat de para meter is ingesteld op **toestaan**. Als u iets wilt blok keren, `-Action` controleert u of de para meter is ingesteld op **blok keren**.

```azurepowershell
$AllowRule = New-AzApplicationGatewayFirewallCustomRule `
   -Name example1 `
   -Priority 2 `
   -RuleType MatchRule `
   -MatchCondition $condition `
   -Action Allow

$BlockRule = New-AzApplicationGatewayFirewallCustomRule `
   -Name example2 `
   -Priority 2 `
   -RuleType MatchRule `
   -MatchCondition $condition `
   -Action Block
```

In het `$BlockRule` vorige voor gaande wordt de volgende aangepaste regel in azure Resource Manager:

```json
"customRules": [
      {
        "name": "blockEvilBot",
        "priority": 2,
        "ruleType": "MatchRule",
        "action": "Block",
        "matchConditions": [
          {
            "matchVariables": [
              {
                "variableName": "RequestHeaders",
                "selector": "User-Agent"
              }
            ],
            "operator": "Contains",
            "negationConditon": false,
            "matchValues": [
              "evilbot"
            ],
            "transforms": [
              "Lowercase"
            ]
          }
        ]
      }
    ], 
```

Deze aangepaste regel bevat een naam, prioriteit, een actie en de matrix van overeenkomende voor waarden waaraan moet worden voldaan voordat de actie wordt uitgevoerd. Zie de volgende veld beschrijvingen voor meer uitleg van deze velden. Zie aangepaste regels voor [Web Application firewall maken en gebruiken](create-custom-waf-rules.md)voor meer informatie.

## <a name="fields-for-custom-rules"></a>Velden voor aangepaste regels

### <a name="name-optional"></a>Naam [Optioneel]

Dit is de naam van de regel. Deze naam wordt weer gegeven in de logboeken.

### <a name="priority-required"></a>Prioriteit [vereist]

- Bepaalt de volg orde waarin regels worden geëvalueerd. Hoe lager de waarde, hoe eerder de evaluatie van de regel.
-Moet uniek zijn voor alle aangepaste regels. Een regel met prioriteit 100 wordt geëvalueerd vóór een regel met de prioriteit 200.

### <a name="rule-type-required"></a>Regel type [vereist]

Momenteel moet **MatchRule**zijn.

### <a name="match-variable-required"></a>Overeenkomende variabele [required]

Moet een van de variabelen zijn:

- RemoteAddr: IP-adres/hostnaam van de verbinding met de externe computer
- RequestMethod: HTTP-aanvraag methode (GET, POST, PUT, DELETE, enzovoort)
- Query string: variabele in de URI
- PostArgs: argumenten verzonden in de hoofd tekst van het bericht. Aangepaste regels die gebruikmaken van deze match-variabele worden alleen toegepast als de header content-type is ingesteld op Application/x-www-form-urlencoded en meerdelige/form-data.
- RequestUri: de URI van de aanvraag
- RequestHeaders: headers van de aanvraag
- RequestBody: dit bevat de volledige hoofd tekst van de aanvraag. Aangepaste regels die gebruikmaken van deze match-variabele worden alleen toegepast als de header content-type is ingesteld op Application/x-www-form-urlencoded. 
- RequestCookies: cookies van de aanvraag

### <a name="selector-optional"></a>Selector [Optioneel]

Beschrijft het veld van de matchVariable-verzameling. Als de matchVariable bijvoorbeeld RequestHeaders is, kan de selector zich op de header *User-agent* bevindt.

### <a name="operator-required"></a>Operator [vereist]

Dit moet een van de volgende Opera tors zijn:

- IPMatch: wordt alleen gebruikt als de overeenkomende variabele *RemoteAddr* is
- Is gelijk aan-invoer is hetzelfde als de MatchValue
- bevat
- LessThan
- GreaterThan
- LessThanOrEqual
- GreaterThanOrEqual
- BeginsWith
- EndsWith
- Reguliere

### <a name="negate-condition-optional"></a>Voor waarde voor negatie [Optioneel]

De huidige voor waarde wordt genegeerd.

### <a name="transform-optional"></a>Transformeren [Optioneel]

Een lijst met teken reeksen met namen van trans formaties voordat de overeenkomst wordt geprobeerd. Dit kunnen de volgende trans formaties zijn:

- Kleine letters
- Omloopspaties wissen
- UrlDecode
- UrlEncode 
- RemoveNulls
- HtmlEntityDecode

### <a name="match-values-required"></a>Overeenkomende waarden [required]

Lijst met waarden die moeten worden vergeleken, wat kan worden beschouwd als *or*' Ed '. Het kan bijvoorbeeld IP-adressen of andere teken reeksen zijn. De notatie van de waarde is afhankelijk van de vorige operator.

### <a name="action-required"></a>Actie [vereist]

- Toestaan: Hiermee wordt de trans actie geautoriseerd, waarbij alle volgende regels worden overgeslagen. Dit betekent dat de opgegeven aanvraag wordt toegevoegd aan de lijst met toegestane aanvragen en eenmaal overeenkomt met de aanvraag en wordt verzonden naar de back-end-groep. Regels die zich op de acceptatie lijst bevinden, worden niet geëvalueerd voor verdere aangepaste regels of beheerde regels.
- Blok: blokkeert de trans actie op basis van *SecDefaultAction* (detectie/preventie modus). Net als bij de actie voor toestaan, zodra de aanvraag is geëvalueerd en toegevoegd aan de lijst met geblokkeerde websites, wordt de evaluatie gestopt en wordt de aanvraag geblokkeerd. Aanvragen die voldoen aan dezelfde voor waarden worden niet geëvalueerd en worden alleen geblokkeerd. 
- Log: Hiermee kan de regel naar het logboek schrijven, maar kan de rest van de regels worden uitgevoerd voor evaluatie. Volgende aangepaste regels worden geëvalueerd in volg orde van prioriteit, gevolgd door de beheerde regels.

## <a name="next-steps"></a>Volgende stappen

Nadat u meer informatie over aangepaste regels hebt [gemaakt, maakt u uw eigen aangepaste regels](create-custom-waf-rules.md).
