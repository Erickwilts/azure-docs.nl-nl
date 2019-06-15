---
title: Wat is Azure-web application firewall voordeur voor Azure? (Preview)
description: Meer informatie over hoe Azure web application firewall voor de voordeur Azure-service biedt uw webtoepassingen beveiliging tegen schadelijke aanvallen.
services: frontdoor
documentationcenter: ''
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: kumud;tyao
ms.openlocfilehash: 122e9687ee313edff34e5a4fd9a44b1026a63811
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66478781"
---
# <a name="what-is-azure-web-application-firewall-for-azure-front-door"></a>Wat is Azure-web application firewall voordeur voor Azure?

Azure Web Application Firewall (WAF) biedt gecentraliseerde beveiliging voor uw web-apps die wereldwijd worden aangeboden met behulp van Azure Front Door. De firewall is ontworpen om uw webservices te beschermen tegen bekende zwakke plekken en beveiligingsproblemen. Daarnaast zorgt de firewall ervoor dat uw service maximaal beschikbaar is voor uw gebruikers en dat er aan de nalevingsvereisten wordt voldaan.


Webtoepassingen worden steeds vaker het doel van aanvallen zoals denial of service overstromingen, SQL-injectieaanvallen en aanvallen via cross-site scripting. Deze aanvallen van buitenaf kan ertoe leiden dat de service-onderbreking en gegevensverlies, een aanzienlijke bedreiging voor web application eigenaars.

Het kan een hele uitdaging zijn om dergelijke aanvallen in toepassingscode te voorkomen en dit kan tevens veel onderhoud, patching en controle vereisen op meerdere lagen van de toepassingstopologie. Een gecentraliseerde firewall voor webtoepassingen maakt het beveiligingsbeheer veel eenvoudiger en biedt toepassingsbeheerders meer veiligheid tegen bedreigingen of aanvallen. Een WAF-oplossing kan ook reageren op een snellere beveiligingsrisico patches voor een bekend beveiligingsprobleem op een centrale locatie, in plaats van elke afzonderlijke webtoepassing te beveiligen.

WAF voor voordeur is een globale en gecentraliseerde oplossing. Deze is geïmplementeerd op Azure-netwerk edge-locaties over de hele wereld en elke inkomende aanvraag voor een webtoepassing voor WAF is ingeschakeld door de voordeur geleverd aan de rand van het netwerk wordt geïnspecteerd. Hiermee WAF om te voorkomen dat schadelijke aanvallen dicht bij de bronnen van de aanval, voordat uw virtuele netwerk binnenkomt en biedt algemene beveiliging op schaal zonder verlies van prestaties. Een WAF-beleid kan eenvoudig worden gekoppeld aan elk profiel voordeur in uw abonnement en nieuwe regels kunnen worden geïmplementeerd binnen enkele minuten, zodat u snel reageren op veranderende bedreigingen patronen.

![Azure-web application firewall](./media/waf-overview/web-application-firewall-overview2.png)

Azure WAF kan ook worden ingeschakeld met Application Gateway. Zie voor meer informatie, [Web application firewall](../application-gateway/waf-overview.md).

## <a name="waf-policy-and-rules"></a>WAF-beleid en regels

U kunt een WAF-beleid configureren en koppelen van dat beleid op een of meer voordeur front-ends voor beveiliging. Een WAF-beleid bestaat uit twee typen beveiligingsregels voor verbindingen:

- aangepaste regels die zijn ontworpen en door de klant.

- beheerde regelsets die een verzameling van door Azure beheerde zijn vooraf geconfigureerde regels.

Wanneer beide aanwezig zijn, worden aangepaste regels worden verwerkt voordat de regels in een beheerde regelset verwerkt. Een regel is gemaakt van een voorwaarde voor overeenkomst, een prioriteit en een actie. Actie die worden ondersteund zijn: TOESTAAN, blokkeren, het logboek en -OMLEIDING. U kunt een volledig aangepaste beleid dat voldoet aan de beveiligingsvereisten voor uw specifieke toepassing door een combinatie van beheerde en aangepaste regels maken.

Regels binnen een beleid worden verwerkt in een volgorde van prioriteit waar prioriteit is een unieke geheel getal dat bepaalt de volgorde van de regels worden verwerkt. Kleinere geheel getal geeft een hogere prioriteit aan en die worden geëvalueerd voor regels met een hogere geheel getal zijn. Zodra een regel is gekoppeld, worden de bijbehorende actie die is gedefinieerd in de regel wordt toegepast op de aanvraag. Wanneer een dergelijke overeenkomst is verwerkt, worden regels met een lagere prioriteit niet meer verwerkt.

Een webtoepassing die is geleverd door de voordeur kan slechts één WAF-beleid dat is gekoppeld aan deze tegelijk hebben. U kunt echter een configuratie voordeur hebben zonder eventuele WAF-beleidsregels die zijn gekoppeld aan deze. Als een WAF-beleid aanwezig is, moeten deze worden gerepliceerd naar alle onze edge-locaties om te zorgen voor consistentie in het beveiligingsbeleid over de hele wereld.

## <a name="waf-modes"></a>WAF-modi

WAF-beleid kan worden geconfigureerd om te worden uitgevoerd in de volgende twee modi:

- **Detectiemodus:** Wanneer in de modus voor detectie uitvoert, wordt WAF neemt eventuele andere vereiste acties dan monitors en vastgelegd in de aanvraag en de overeenkomende WAF-regel voor WAF-Logboeken. U kunt Diagnostische logboekregistratie inschakelen voor de voordeur (wanneer u portal gebruikt, kunt u dit doen door te gaan naar de **Diagnostics** sectie in Azure portal).

- **Preventiemodus:** Als geconfigureerd om te worden uitgevoerd in preventiemodus, gaat de WAF de opgegeven actie als een aanvraag overeenkomt met een regel en als een overeenkomst wordt gevonden, geen verdere regels met lagere prioriteit worden geëvalueerd. Geen overeenkomende aanvragen worden ook geregistreerd in de WAF-Logboeken.

## <a name="waf-actions"></a>WAF-acties

WAF-klanten kunnen kiezen uit een van de acties worden uitgevoerd wanneer een aanvraag overeenkomt met de voorwaarden van een regel:

- **Toestaan:**  Aanvraag wordt doorgegeven via de WAF en wordt doorgestuurd naar de back-end. Regels met lagere prioriteit kunnen geen verdere blokkeren voor deze aanvraag.
- **Blokkeren:** De aanvraag is geblokkeerd en WAF stuurt een antwoord naar de client zonder dat de aanvraag naar de back-end.
- **Logboekbestanden:**  Aanvraag wordt geregistreerd in de WAF-logboeken en WAF blijft evalueren regels met lagere prioriteit.
- **Omleiden:** WAF leidt de aanvraag naar de opgegeven URI. De opgegeven URI is een niveau beleidsinstelling. Wanneer dit is geconfigureerd, alle aanvragen die voldoen aan de **omleiden** actie wordt verzonden naar deze URI.

## <a name="waf-rules"></a>WAF-regels

Een WAF-beleid kan bestaan uit twee typen beveiligingsregels - aangepaste regels door de klant en zijn beheerde rulesets set regels Azure beheerde vooraf is geconfigureerd.

### <a name="custom-authored-rules"></a>Aangepaste opgestelde regels

U kunt aangepaste regels WAF als volgt configureren:

- **IP-lijst en lijst met geblokkeerde websites toestaan:** U kunt aangepaste regels voor het beheren van toegang tot uw webtoepassingen op basis van een lijst van de client-IP-adressen of IP-adresbereiken configureren. Zowel IPv4 als IPv6-adrestypen worden ondersteund. Deze lijst kan worden geconfigureerd om te blokkeren of toestaan deze aanvragen waarbij het bron-IP-adres overeenkomt met een IP-adres in de lijst.

- **Geografische gebaseerd toegangsbeheer:** U kunt aangepaste regels voor het beheren van toegang tot uw webtoepassingen op basis van de landcode die is gekoppeld aan IP-adres van een client configureren.

- **HTTP-parameters gebaseerd toegangsbeheer:** U kunt aangepaste regels op basis van de tekenreeks die overeenkomt met HTTP/HTTPS-aanvraagparameters zoals queryreeksen, POST argumenten, aanvraag-URI, aanvraagheader en aanvraagtekst configureren.

- **Op basis van de methode van aanvraag:** U kunt aangepaste regels op basis van de aanvraag HTTP-methode van de aanvraag, zoals GET, PUT of HEAD configureren.

- **Formaat beperking:** U kunt aangepaste regels op basis van de lengte van de specifieke onderdelen van een aanvraag, zoals de queryreeks, Uri, configureren of de hoofdtekst van de aanvraag.

- **Frequentie regels:** Een regel voor snelheid is abnormaal veel verkeer vanaf elke client-IP-beperken. U kunt een drempelwaarde configureren voor het aantal webaanvragen toegestaan vanaf een client-IP-adres tijdens een duur van één minuut. Dit is hetzelfde als een IP op basis van een lijst toestaan/blokkeren aangepaste regel waarmee u alle of alle van een client-IP aanvragen-adresblokken. Gelden enkele beperkingen kan worden gecombineerd met de criteria voor aanvullende overeenkomst zoals parameters van HTTP (S) die overeenkomt met voor granulaire besturingselement.

### <a name="azure-managed-rule-sets"></a>Azure beheerde regelsets

Azure beheerde regelsets bieden een eenvoudige manier om te implementeren bescherming tegen een gemeenschappelijke set beveiligingsrisico's. Omdat dergelijke rulesets worden beheerd door Azure, worden de regels indien nodig om u te beschermen tegen nieuwe aanvallen handtekeningen bijgewerkt. Openbare preview-versie is bevat Azure beheerde standaard regelset regels op basis van de volgende threat categorieën:

- Cross-site scripting
- Java-aanvallen
- Lokale insluiting
- PHP-injectieaanvallen
- Externe opdracht uitvoeren
- Externe insluiting
- Sessiefixatie
- Beveiliging tegen SQL-injecties
- Protocol aanvallers

Het versienummer van de standaard regelset wordt verhoogd wanneer nieuwe aanval handtekeningen worden toegevoegd aan de regelset.
Standaard regelset is standaard in de modus van de detectie van uw WAF-beleid ingeschakeld. U kunt uitschakelen of schakelt u afzonderlijke regels in de standaard-regel is ingesteld om te voldoen aan de vereisten van uw toepassing. U kunt ook specifieke acties (toestaan/blokkeren/OMLEIDEN/logboek) per regel instellen. Standaard wordt voor het blokkeren. Bovendien kunnen aangepaste regels in de dezelfde WAF-beleid worden geconfigureerd als u wilt overslaan van een van de vooraf geconfigureerde regels in de standaard-regel is ingesteld.
Aangepaste regels worden altijd toegepast voordat de regels in de standaard regelset zijn geëvalueerd. Als een aanvraag overeenkomt met een aangepaste regel, bijbehorende regelactie wordt toegepast en de aanvraag is geblokkeerd of doorgegeven aan de back-end, zonder de aanroep van een meer aangepaste regels of de regels in de standaard-regel is ingesteld. U hebt bovendien de optie voor het verwijderen van de regelset standaard uit uw WAF-beleid.


### <a name="bot-protection-rule-preview"></a>De regel bot protection (preview)

Een beheerde regelset van de Bot-beveiliging kan worden ingeschakeld voor uw WAF aan aangepaste acties uitvoeren op aanvragen van bekende schadelijke IP-adressen. De IP-adressen zijn afkomstig van de Microsoft-Bedreigingsinformatie-feed. [Intelligent Security Graph](https://www.microsoft.com/security/operations/intelligence) wordt gebruikt door bedreigingsinformatie van Microsoft en wordt gebruikt door meerdere services, met inbegrip van Azure Security Center.

![Regelset voor bot-beveiliging](./media/waf-front-door-configure-bot-protection/BotProtect2.png)

> [!IMPORTANT]
> Regelset voor bot-beveiliging is momenteel in openbare preview en is voorzien van een Preview-versie service level agreement. De reden hiervoor is dat bepaalde functies mogelijk niet worden ondersteund of beperkte mogelijkheden hebben.  Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Als Bot-beveiliging is ingeschakeld, worden binnenkomende aanvragen die voldoen aan schadelijke Bots client-IP's op FrontdoorWebApplicationFirewallLog logboek geregistreerd. U hebt toegang tot WAF-logboeken van de storage-account, event hub of log analytics. 

## <a name="configuration"></a>Configuratie

Configuratie en implementatie van alle typen WAF wordt volledig ondersteund met behulp van Azure portal, REST-API's, Azure Resource Manager-sjablonen en Azure PowerShell.

## <a name="monitoring"></a>Bewaking

Bewaking voor WAF aan voordeur is geïntegreerd met Azure Monitor voor het bijhouden van waarschuwingen en gemakkelijk trends van verkeer.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [een WAF-beleid configureren voor de voordeur met behulp van de Azure portal](waf-front-door-create-portal.md)