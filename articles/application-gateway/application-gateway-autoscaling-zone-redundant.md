---
title: Automatisch schalen en zone-redundantie in Application Gateway v2
description: In dit artikel wordt de Azure-toepassing Standard_v2 en WAF_v2 SKU geïntroduceerd, waaronder automatisch schalen en zones redundante functies.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 06/06/2020
ms.author: victorh
ms.custom: fasttrack-edit, references_regions
ms.openlocfilehash: df68bec859a3c2456dd1cf5f69387e8affb4b3e2
ms.sourcegitcommit: f57fa5f3ce40647eda93f8be4b0ab0726d479bca
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2020
ms.locfileid: "84484532"
---
# <a name="autoscaling-and-zone-redundant-application-gateway-v2"></a>Automatisch schalen en zone-redundantie in Application Gateway v2 

Application Gateway en Web Application firewall (WAF) zijn ook beschikbaar onder een Standard_v2 en WAF_v2 SKU. De v2-SKU biedt prestatie verbeteringen en voegt ondersteuning toe voor essentiële nieuwe functies, zoals automatisch schalen, zone redundantie en ondersteuning voor statische Vip's. Bestaande functies onder de Standard-en WAF SKU worden nog steeds ondersteund in de nieuwe v2-SKU, met een paar uitzonde ringen die worden vermeld in het gedeelte [vergelijking](#differences-from-v1-sku) .

De nieuwe v2-SKU bevat de volgende verbeteringen:

- Automatische **schaling**: Application Gateway-of WAF-implementaties onder de SKU voor automatisch schalen kunnen omhoog of omlaag worden geschaald op basis van het wijzigen van de verkeers laad patronen. Automatisch schalen heft ook de vereiste op om tijdens het inrichten een implementatiegrootte of het aantal instanties te kiezen. Deze SKU biedt echte elasticiteit. In de Standard_v2 en WAF_v2 SKU kunnen Application Gateway zowel in vaste capaciteit (automatisch schalen uitgeschakeld) als in de modus voor automatisch schalen worden uitgevoerd. De modus vaste capaciteit is handig voor scenario's met consistente en voorspel bare werk belastingen. De modus voor automatisch schalen is handig voor toepassingen die verschillen in toepassings verkeer bekijken.
- **Zone redundantie**: een Application Gateway-of WAF-implementatie kan meerdere Beschikbaarheidszones omvatten, waardoor de nood zaak om afzonderlijke Application Gateway instanties in elke zone met een Traffic Manager te inrichten, wordt verwijderd. U kunt kiezen uit één zone of meerdere zones waar Application Gateway instanties worden geïmplementeerd, waardoor de zone storing moeilijker wordt. De back-end-groep voor toepassingen kan net zo worden gedistribueerd over beschikbaarheids zones.

  Zone redundantie is alleen beschikbaar wanneer Azure-zones beschikbaar zijn. In andere regio's worden alle andere functies ondersteund. Zie [regio's en Beschikbaarheidszones in azure](../availability-zones/az-overview.md) voor meer informatie.
- **Statische VIP**: Application Gateway v2-SKU ondersteunt alleen het statische VIP-type. Dit zorgt ervoor dat het VIP dat is gekoppeld aan de toepassings gateway niet wordt gewijzigd voor de levens cyclus van de implementatie, zelfs na het opnieuw opstarten.  Er is geen statische VIP in v1. Daarom moet u de URL van de toepassings gateway gebruiken in plaats van het IP-adres voor de route ring van domein namen naar App Services via de Application Gateway.
- **Header herschrijven**: met Application Gateway kunt u HTTP-aanvraag-en reactie headers toevoegen, verwijderen of bijwerken met v2-SKU. Zie [HTTP-headers herschrijven met Application Gateway](rewrite-http-headers.md) voor meer informatie
- **Key Vault-integratie**: Application Gateway v2 ondersteunt de integratie met Key Vault voor server certificaten die zijn gekoppeld aan LISTENERS waarvoor https is ingeschakeld. Zie [TLS Terminate with Key Vault certificates](key-vault-certs.md)(Engelstalig) voor meer informatie.
- **Azure Kubernetes service ingangs controller**: Application Gateway de Azure-toepassing gateway kan worden gebruikt als binnenkomend verkeer voor een Azure Kubernetes service (AKS), ook wel AKS-cluster genoemd. Zie [Wat is Application Gateway ingress-controller?](ingress-controller-overview.md)voor meer informatie.
- **Prestatie verbeteringen**: de v2-SKU biedt tot 5X betere TLS-offload-prestaties, vergeleken met de Standard-WAF SKU.
- **Snellere implementatie en update tijd** De v2-SKU biedt een snellere implementatie en tijd voor het bijwerken in vergelijking met Standard/WAF SKU. Dit omvat ook wijzigingen in de configuratie van WAF.

![Diagram van de zone voor automatisch schalen.](./media/application-gateway-autoscaling-zone-redundant/application-gateway-autoscaling-zone-redundant.png)

## <a name="supported-regions"></a>Ondersteunde regio’s

De Standard_v2-en WAF_v2 SKU is beschikbaar in de volgende regio's: Noord-Centraal VS, Zuid-Centraal VS, VS-West, VS-West 2, VS-Oost, VS-Oost 2, centraal VS, Europa-noord, Europa-west, Zuidoost-Azië, Frankrijk-centraal, UK-west, Japan-Oost, Japan-West, Australië-Oost, Australië-Zuidoost, Brazilië-zuid, Canada-centraal, Canada-oost, Azië-oost, Korea-centraal, Korea-Zuid , UK-zuid, Centraal-India, West-India, India-zuid.

## <a name="pricing"></a>Prijzen

Met de v2-SKU wordt het prijs model aangestuurd door verbruik en niet meer gekoppeld aan het aantal exemplaren of de grootte van het exemplaar. De v2-SKU-prijzen hebben twee onderdelen:

- **Vaste prijs** : dit is elk uur (of een gedeeltelijke uur) prijs om een Standard_v2 of WAF_v2 gateway in te richten. 0 extra minimum aantal exemplaren garandeert nog steeds een hoge Beschik baarheid van de service, die altijd is opgenomen in een vaste prijs.
- **Eenheids prijs van capaciteit** : dit is een kosten op basis van verbruik die naast de vaste kosten worden berekend. De kosten voor de capaciteits eenheid worden ook elk uur of gedeeltelijk per uur berekend. Er zijn drie dimensies tot capaciteits eenheid: reken eenheid, permanente verbindingen en door voer. Rekeneenheid is een meting van de verbruikte processorcapaciteit. Factoren die de rekeneenheid beïnvloeden, zijn: TLS-verbindingen per seconde, berekeningen voor het herschrijven van URL's en de verwerking van WAF-regels. Permanente verbinding is een meting van de tot stand gebrachte TCP-verbindingen met de toepassings gateway in een bepaald facturerings interval. De door Voer is het gemiddelde aantal megabits per seconde dat door het systeem wordt verwerkt in een opgegeven facturerings interval.  De facturering vindt plaats op het niveau van de capaciteits eenheid voor alle boven het gereserveerde exemplaar aantal.

Elke capaciteits eenheid bestaat uit Maxi maal: 1 reken eenheid, 2500 permanente verbindingen en door Voer van 2,22-Mbps.

Richt lijnen voor reken eenheden:

- **Standard_v2** -elke reken eenheid kan ongeveer 50 verbindingen per seconde met een RSA 2048-bits sleutel TLS-certificaat zijn.
- **WAF_v2** -elke Compute-eenheid kan ongeveer 10 gelijktijdige aanvragen per seconde ondersteunen voor 70-30% mix van verkeer met 70% aanvragen van minder dan 2 KB Get/post en resterend. WAF-prestaties worden op dit moment niet beïnvloed door de antwoord grootte.

> [!NOTE]
> Elk exemplaar kan op dit moment ongeveer 10 capaciteits eenheden ondersteunen.
> Het aantal aanvragen dat een Compute-eenheid kan verwerken, is afhankelijk van verschillende criteria, zoals de sleutel grootte van TLS-certificaten, het algoritme voor sleutel uitwisseling, het opnieuw schrijven van kopteksten en in het geval van WAF binnenkomende aanvraag grootte. U wordt aangeraden om toepassings tests uit te voeren om de aanvraag snelheid per Compute-eenheid te bepalen. Zowel de capaciteits eenheid als de reken eenheid worden beschikbaar gesteld als metrische waarde voordat facturering wordt gestart.

In de volgende tabel ziet u voor beelden van prijzen en zijn alleen bedoeld ter illustratie.

**Prijzen in VS Oost**:

|              SKU-naam                             | Vaste prijs ($/uur)  | Eenheids prijs van capaciteit ($/CU-hr)   |
| ------------------------------------------------- | ------------------- | ------------------------------- |
| Standard_v2                                       |    0,20             | 0,0080                          |
| WAF_v2                                            |    0.36             | 0,0144                          |

Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/application-gateway/)voor meer informatie over prijzen. 

**Voorbeeld 1**

Een Application Gateway Standard_v2 is ingericht zonder automatisch te schalen in hand matige schaal modus met vaste capaciteit van vijf exemplaren.

Vaste prijs = 744 (uren) * $0,20 = $148,8 <br>
Capaciteits eenheden = 744 (uur) * 10 capaciteits eenheid per instantie * 5 exemplaren * $0,008 per capaciteits eenheid uur = $297,6

Totale prijs = $148,8 + $297,6 = $446,4

**Voorbeeld 2**

Een Application Gateway standard_v2 is ingericht voor een maand, met een minimum van nul instanties en gedurende deze tijd 25 nieuwe TLS-verbindingen per seconde, gemiddelde van 8,88-Mbps-gegevens overdracht. Als er verbinding is met de korte levens duur, is uw prijs:

Vaste prijs = 744 (uren) * $0,20 = $148,8

Eenheids prijs van capaciteit = 744 (uur) * Max (25/50 reken eenheid voor verbindingen per seconde, 8.88/2.22 capaciteits eenheid voor door Voer) * $0,008 = 744 * 4 * 0,008 = $23,81

Totale prijs = $148.8 + 23.81 = $172,61

Zoals u kunt zien, wordt u alleen gefactureerd voor vier capaciteits eenheden, niet voor het hele exemplaar. 

> [!NOTE]
> De functie Max retourneert de grootste waarde in een paar waarden.


**Voorbeeld 3**

Een Application Gateway standard_v2 is ingericht voor een maand, met een minimum van vijf exemplaren. Ervan uitgaande dat er geen verkeer is en er geen verbinding is, is uw prijs:

Vaste prijs = 744 (uren) * $0,20 = $148,8

Eenheids prijs van capaciteit = 744 (uur) * Max (0/50 reken eenheid voor verbindingen per seconde, 0/2.22 capaciteits eenheid voor door Voer) * $0,008 = 744 * 50 * 0,008 = $297,60

Totale prijs = $148.80 + 297.60 = $446,4

In dit geval wordt u gefactureerd voor het geheel van de vijf instanties, zelfs als er geen verkeer is.

**Voor beeld 4**

Een Application Gateway standard_v2 is ingericht voor een maand, met een minimum van vijf instanties, maar deze keer is een gemiddelde van 125-Mbps-gegevens overdracht en 25 TLS-verbindingen per seconde. Ervan uitgaande dat er geen verkeer is en er geen verbinding is, is uw prijs:

Vaste prijs = 744 (uren) * $0,20 = $148,8

Eenheids prijs van capaciteit = 744 (uur) * Max (25/50 reken eenheid voor verbindingen per seconde, 125/2.22-capaciteits eenheid voor door Voer) * $0,008 = 744 * 57 * 0,008 = $339,26

Totale prijs = $148.80 + 339.26 = $488,06

In dit geval wordt u gefactureerd voor de Maxi maal vijf instanties, plus zeven capaciteits eenheden (7/10 van een exemplaar).  

**Voorbeeld 5**

Een Application Gateway WAF_v2 is ingericht voor een maand. Gedurende deze periode ontvangt het 25 nieuwe TLS-verbindingen per seconde, gemiddelde van 8,88-Mbps-gegevens overdracht en heeft 80 aanvragen per seconde. Als er een korte levens duur is en de berekening van de reken eenheid voor de toepassing 10 RPS per reken eenheid ondersteunt, is uw prijs:

Vaste prijs = 744 (uren) * $0,36 = $267,84

Prijs per capaciteits eenheid = 744 (uur) * Max (maximale reken eenheid (25/50 voor verbindingen/sec, 80/10 WAF RPS), 8.88/2.22 capaciteits eenheid voor door Voer) * $0,0144 = 744 * 8 * 0,0144 = $85,71

Totale prijs = $267,84 + $85,71 = $353,55

> [!NOTE]
> De functie Max retourneert de grootste waarde in een paar waarden.

## <a name="scaling-application-gateway-and-waf-v2"></a>Application Gateway en WAF v2 schalen

Application Gateway en WAF kunnen worden geconfigureerd om in twee modi te schalen:

- Automatisch **schalen** : als automatische schaling is ingeschakeld, worden de Application Gateway-en WAF v2-sku's omhoog of omlaag geschaald op basis van de vereisten voor toepassings verkeer. Deze modus biedt een betere elasticiteit voor uw toepassing en elimineert de nood zaak om de grootte van de toepassings gateway of het aantal instanties te raden. Met deze modus kunt u ook kosten besparen door te voor komen dat de gateway op piek ingerichte capaciteit wordt uitgevoerd voor de verwachte maximale belasting van het verkeer. U moet een minimum en optioneel maximum aantal exemplaren opgeven. De minimale capaciteit zorgt ervoor dat Application Gateway en WAF v2 niet onder het opgegeven minimum aantal instanties vallen, zelfs als er geen verkeer is. Elk exemplaar is ongeveer gelijk aan 10 extra gereserveerde capaciteits eenheden. Nul geeft geen gereserveerde capaciteit aan en is uitsluitend automatisch geschaald. U kunt eventueel ook een maximum aantal exemplaren opgeven, waardoor het Application Gateway niet groter wordt dan het opgegeven aantal exemplaren. Er worden alleen kosten in rekening gebracht voor de hoeveelheid verkeer die door de gateway wordt geleverd. Het aantal exemplaren kan variëren van 0 tot en met 125. De standaard waarde voor het maximum aantal exemplaren is 20 als deze niet is opgegeven.
- **Hand matig** : u kunt ook hand matige modus kiezen waarbij de gateway niet automatisch wordt geschaald. Als er in deze modus meer verkeer is dan wat Application Gateway of WAF kan verwerken, kan dit leiden tot verlies van verkeer. Bij hand matige modus wordt het aantal exemplaren opgegeven dat verplicht is. Aantal exemplaren kan variëren van 1 tot 125 exemplaren.

## <a name="autoscaling-and-high-availability"></a>Automatisch schalen en hoge Beschik baarheid

Azure-toepassing gateways worden altijd op een Maxi maal beschik bare manier geïmplementeerd. De service is gemaakt met meerdere exemplaren die zijn gemaakt als geconfigureerd (als automatisch schalen is uitgeschakeld) of die vereist zijn voor het laden van de toepassing (als automatisch schalen is ingeschakeld). Houd er rekening mee dat u in het oogpunt van de gebruiker niet per se de afzonderlijke instanties kunt zien, maar alleen in de Application Gateway Service als geheel. Als er een probleem is met een bepaald exemplaar en er wordt gestaakt, wordt door Azure-toepassing gateway transparant een nieuw exemplaar gemaakt.

Houd er rekening mee dat zelfs als u automatisch schalen configureert met een minimum aantal instanties, de service nog steeds Maxi maal beschikbaar is, wat altijd is opgenomen in de vaste prijs.

Het maken van een nieuw exemplaar kan echter ongeveer zes of zeven minuten duren. Als u geen gebruik wilt maken van deze downtime, kunt u dus een minimum aantal exemplaren van 2 configureren, in het ideale geval met ondersteuning voor de beschikbaarheids zone. Op deze manier hebt u onder normale omstandigheden ten minste twee exemplaren in uw Azure-toepassing-gateway, dus als er een probleem is met de andere, wordt er tijdens het moment dat er een nieuwe instantie wordt gemaakt, met het verkeer gecommuniceerd. Houd er rekening mee dat een instantie van de Azure-toepassing-gateway ongeveer 10 capaciteits eenheden kan ondersteunen, dus afhankelijk van hoeveel verkeer u normaal gesp roken wilt, kunt u de instelling voor de minimale automatische schaal aanpassing configureren op een waarde hoger dan 2.

## <a name="feature-comparison-between-v1-sku-and-v2-sku"></a>Vergelijking van functies tussen v1 SKU en v2 SKU

De volgende tabel vergelijkt de functies die beschikbaar zijn voor elke SKU.

|                                                   | v1-SKU   | v2 SKU   |
| ------------------------------------------------- | -------- | -------- |
| Automatisch schalen                                       |          | &#x2713; |
| Zone redundantie                                   |          | &#x2713; |
| Statisch VIP                                        |          | &#x2713; |
| Azure Kubernetes service (AKS) ingangs controller |          | &#x2713; |
| Integratie van Azure Sleutelkluis                       |          | &#x2713; |
| HTTP (S)-headers opnieuw schrijven                           |          | &#x2713; |
| URL-gebaseerde routering                                 | &#x2713; | &#x2713; |
| Hosting van meerdere sites                             | &#x2713; | &#x2713; |
| Omleiding van verkeer                               | &#x2713; | &#x2713; |
| Web Application Firewall (WAF)                    | &#x2713; | &#x2713; |
| Aangepaste WAF-regels                                  |          | &#x2713; |
| Beëindiging van Transport Layer Security (TLS)/Secure Sockets Layer (SSL)            | &#x2713; | &#x2713; |
| End-to-end TLS-versleuteling                         | &#x2713; | &#x2713; |
| Sessieaffiniteit                                  | &#x2713; | &#x2713; |
| Aangepaste foutpagina's                                | &#x2713; | &#x2713; |
| Ondersteuning voor WebSocket                                 | &#x2713; | &#x2713; |
| Ondersteuning voor HTTP/2                                    | &#x2713; | &#x2713; |
| Verwerkingsstop voor verbindingen                               | &#x2713; | &#x2713; |

> [!NOTE]
> De v2 SKU voor automatisch schalen ondersteunt nu [standaard status tests](application-gateway-probe-overview.md#default-health-probe) om de status van alle resources in de back-end-pool te controleren en de back-end-leden te markeren die als slecht worden beschouwd. De standaard status test wordt automatisch geconfigureerd voor back-ends die geen aangepaste test configuratie hebben. Zie [Health tests in Application Gateway](application-gateway-probe-overview.md)voor meer informatie.

## <a name="differences-from-v1-sku"></a>Verschillen van v1 SKU

In deze sectie worden de functies en beperkingen beschreven van de v2-SKU die verschilt van de V1-SKU.

|Verschil|Details|
|--|--|
|Verificatie certificaat|Niet ondersteund.<br>Zie [overzicht van end-to-end-TLS met Application Gateway](ssl-overview.md#end-to-end-tls-with-the-v2-sku)voor meer informatie.|
|Standard_v2 en standaard Application Gateway op hetzelfde subnet mengen|Niet ondersteund|
|Door de gebruiker gedefinieerde route (UDR) op Application Gateway subnet|Ondersteund (specifieke scenario's). In de preview-versie.<br> Zie [Application Gateway configuratie-overzicht](configuration-overview.md#user-defined-routes-supported-on-the-application-gateway-subnet)voor meer informatie over ondersteunde scenario's.|
|NSG voor binnenkomend poort bereik| -65200 tot 65535 voor Standard_v2 SKU<br>-65503 tot 65534 voor standaard-SKU.<br>Raadpleeg de [Veelgestelde vragen](application-gateway-faq.md#are-network-security-groups-supported-on-the-application-gateway-subnet)voor meer informatie.|
|Prestatie Logboeken in azure Diagnostics|Niet ondersteund.<br>De metrische gegevens van Azure moeten worden gebruikt.|
|Billing|De facturering is gepland om te beginnen op 1 juli 2019.|
|FIPS-modus|Deze worden momenteel niet ondersteund.|
|Modus alleen ILB|Dit wordt momenteel niet ondersteund. De open bare en ILB modus samen worden ondersteund.|
|Netwerk-Watcher-integratie|Niet ondersteund.|
|Azure Security Center-integratie|Nog niet beschikbaar.

## <a name="migrate-from-v1-to-v2"></a>Migreren van v1 naar v2

Een Azure PowerShell script is beschikbaar in de Power shell Gallery, waarmee u kunt migreren van uw v1-Application Gateway/WAF naar de v2-SKU voor automatisch schalen. Met dit script kunt u de configuratie van uw v1-gateway kopiëren. Verkeer migratie is nog steeds uw verantwoordelijkheid. Zie [Azure-toepassing gateway migreren van v1 naar v2](migrate-v1-v2.md)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

- [Snelstart: Webverkeer omleiden met Azure Application Gateway - Azure Portal](quick-create-portal.md)
- [Een automatische schaling maken, een zone redundante toepassings gateway met een gereserveerd virtueel IP-adres met behulp van Azure PowerShell](tutorial-autoscale-ps.md)
- Meer informatie over [Application Gateway](overview.md).
- Meer informatie over [Azure firewall](../firewall/overview.md).
