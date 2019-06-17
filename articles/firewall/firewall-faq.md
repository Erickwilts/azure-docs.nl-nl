---
title: Veelgestelde vragen over de Firewall van Azure
description: Veelgestelde vragen over de Firewall van Azure
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 5/30/2019
ms.author: victorh
ms.openlocfilehash: 75b1131f2853cb444481b9c7a6c96e28f8537538
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66384677"
---
# <a name="azure-firewall-faq"></a>Veelgestelde vragen over de Firewall van Azure

## <a name="what-is-azure-firewall"></a>Wat is Azure Firewall?

Azure Firewall is een beheerde, cloudgebaseerde netwerkbeveiligingsservice die uw Azure Virtual Network-resources beschermt. Het is een volledig stateful firewall-as-a-service met de ingebouwde hoge beschikbaarheid en cloudschaalbaarheid van de onbeperkte. U kunt beleid voor toepassings- en netwerkconnectiviteit centraal maken, afdwingen en registreren voor abonnementen en virtuele netwerken.

## <a name="what-capabilities-are-supported-in-azure-firewall"></a>Welke mogelijkheden worden ondersteund in de Firewall van Azure?

* Stateful firewall als een service
* Ingebouwde hoge beschikbaarheid met onbeperkte cloudschaalbaarheid
* Filteren op FQDN
* FQDN-tags
* Regels voor het filteren van netwerkverkeer
* Ondersteuning voor uitgaande SNAT
* Ondersteuning voor inkomende DNAT
* Centraal maken, afdwingen en meld u beleid van toepassing en netwerk connectiviteit tussen Azure-abonnementen en VNETs
* Volledig geïntegreerd met Azure Monitor voor registratie en analyses

## <a name="what-is-the-typical-deployment-model-for-azure-firewall"></a>Wat is het typische implementatiemodel voor de Firewall van Azure?

U kunt de Firewall van Azure implementeren op een virtueel netwerk, maar klanten doorgaans implementeren op een centrale virtueel netwerk en andere virtuele netwerken toe in een hub en spoke-model. Vervolgens stelt u de standaardroute uit de gekoppelde virtuele netwerken om te verwijzen naar dit centrale firewall virtuele netwerk. Wereldwijde VNet-peering wordt ondersteund, maar dit wordt niet aanbevolen vanwege mogelijke prestaties en latentieproblemen met tussen regio's. Voor de beste prestaties, implementeert u een firewall per regio.

Het voordeel van dit model is de mogelijkheid om een centraal beheer van meerdere knooppunt VNETs in verschillende abonnementen. Er zijn ook kosten te besparen als u niet nodig een firewall in elk VNet afzonderlijk implementeren. De kosten te besparen moeten worden gemeten en de koppelen peering kosten op basis van de verkeerspatronen van de klant.

## <a name="how-can-i-install-the-azure-firewall"></a>Hoe kan ik de Azure-Firewall installeren?

U kunt Azure Firewall instellen met behulp van de Azure portal, PowerShell, REST-API of met behulp van sjablonen. Zie [zelfstudie: Implementeren en configureren van de Firewall van Azure met behulp van de Azure-portal](tutorial-firewall-deploy-portal.md) voor stapsgewijze instructies.

## <a name="what-are-some-azure-firewall-concepts"></a>Wat zijn enkele concepten Firewall van Azure?

Firewall van Azure biedt ondersteuning voor regels en regelverzamelingen. Verzameling van een regel is een reeks regels die de dezelfde volgorde en prioriteit delen. Regelverzamelingen worden uitgevoerd in de volgorde van hun prioriteit. Netwerk regelverzamelingen hogere prioriteit dan de regelverzamelingen van toepassing zijn, en alle regels worden beëindigd.

Er zijn drie typen regelverzamelingen:

* *Regels voor Application*: Volledig gekwalificeerde domeinnamen (FQDN's) die kunnen worden benaderd vanaf een subnet configureren.
* *Regels voor*: Configureer regels die bronadressen, protocollen, doelpoorten en doeladressen bevatten.
* *NAT-regels*: DNAT regels voor binnenkomende verbindingen configureren.

## <a name="does-azure-firewall-support-inbound-traffic-filtering"></a>Biedt Azure Firewall ondersteuning voor binnenkomend verkeer filteren?

Firewall van Azure biedt ondersteuning voor binnenkomend en uitgaand filteren. Binnenkomende beveiliging is voor niet-HTTP/S-protocollen. Bijvoorbeeld RDP, SSH en FTP-protocollen.

## <a name="which-logging-and-analytics-services-are-supported-by-the-azure-firewall"></a>Welke services logboekregistratie en analyse worden ondersteund door de Firewall van Azure?

Firewall van Azure is geïntegreerd met Azure Monitor voor het weergeven en analyseren van de firewall-Logboeken. Logboeken kunnen worden verzonden naar Log Analytics, Azure Storage of Event Hubs. Ze kunnen worden geanalyseerd in Log Analytics of met verschillende hulpprogramma's zoals Excel en Power BI. Zie [Zelfstudie: Azure-Firewall-logboeken bewaken](tutorial-diagnostics.md).

## <a name="how-does-azure-firewall-work-differently-from-existing-services-such-as-nvas-in-the-marketplace"></a>Hoe Azure Firewall werkt anders van bestaande services zoals NVA's in de marketplace?

Firewall van Azure is een eenvoudige firewall-service die in bepaalde scenario's van klanten voorzien kan. Verwacht wordt dat u een combinatie van de NVA's van derden en de Firewall van Azure hebt. Samen beter is een prioriteit core.

## <a name="what-is-the-difference-between-application-gateway-waf-and-azure-firewall"></a>Wat is het verschil tussen de Application Gateway WAF- en firewallinstellingen van Azure?

De Web Application Firewall (WAF) is een functie van Application Gateway die gecentraliseerde binnenkomende beveiliging van uw webtoepassingen tegen algemene aanvallen en beveiligingsproblemen biedt. Firewall van Azure biedt binnenkomende beveiliging voor niet-HTTP/S-protocollen (bijvoorbeeld RDP, SSH, FTP), uitgaande op netwerkniveau beveiliging voor alle poorten en protocollen en de beveiliging op toepassingsniveau voor uitgaande HTTP/S.

## <a name="what-is-the-difference-between-network-security-groups-nsgs-and-azure-firewall"></a>Wat is het verschil tussen Netwerkbeveiligingsgroepen (nsg's) en de Firewall van Azure?

De Firewall van de Azure-service is een aanvulling op network security group functionaliteit. Samen bieden ze beter 'verdediging in de diepte' netwerkbeveiliging. Netwerkbeveiligingsgroepen bevatten gedistribueerde laag filteren van netwerkverkeer om te beperken van verkeer naar resources in virtuele netwerken in elk abonnement. Firewall van Azure is een volledig stateful, gecentraliseerde netwerk firewall as-a-service, waarmee de netwerk - en -beveiliging op toepassingsniveau in verschillende abonnementen en virtuele netwerken.

## <a name="are-network-security-groups-nsgs-supported-on-the-azure-firewall-subnet"></a>Netwerkbeveiligingsgroepen (nsg's) is worden ondersteund op het subnet van de Firewall van Azure?

Firewall van Azure is een beheerde service met meerdere lagen van de beveiliging, met inbegrip van de platform-beveiliging met NIC niveau Nsg (niet zichtbaar).  Nsg's op subnetniveau zijn niet vereist op de Firewall van Azure-subnet en zijn uitgeschakeld om te controleren of er geen service wordt onderbroken.


## <a name="how-do-i-set-up-azure-firewall-with-my-service-endpoints"></a>Hoe stel ik Azure-Firewall in met de service-eindpunten?

Voor beveiligde toegang tot PaaS-services raden wij service-eindpunten. U kunt de service-eindpunten in de Firewall van Azure-subnet inschakelen en uitschakelen verbonden spoke-netwerken. Op deze manier die u profiteren van functies, service-eindpunt beveiligings- en centrale logboekregistratie voor al het verkeer.

## <a name="what-is-the-pricing-for-azure-firewall"></a>Wat zijn de prijzen voor de Firewall van Azure?

Zie [Firewall van Azure-prijzen](https://azure.microsoft.com/pricing/details/azure-firewall/).

## <a name="how-can-i-stop-and-start-azure-firewall"></a>Hoe kan ik stoppen en starten van de Firewall van Azure?

U kunt Azure PowerShell gebruiken *toewijzing* en *toewijzen* methoden.

Bijvoorbeeld:

```azurepowershell
# Stop an existing firewall

$azfw = Get-AzFirewall -Name "FW Name" -ResourceGroupName "RG Name"
$azfw.Deallocate()
Set-AzFirewall -AzureFirewall $azfw
```

```azurepowershell
# Start a firewall

$azfw = Get-AzFirewall -Name "FW Name" -ResourceGroupName "RG Name"
$vnet = Get-AzVirtualNetwork -ResourceGroupName "RG Name" -Name "VNet Name"
$publicip = Get-AzPublicIpAddress -Name "Public IP Name" -ResourceGroupName " RG Name"
$azfw.Allocate($vnet,$publicip)
Set-AzFirewall -AzureFirewall $azfw
```

> [!NOTE]
> U moet een firewall en een openbaar IP-adres toewijzen aan de oorspronkelijke resourcegroep en het abonnement.

## <a name="what-are-the-known-service-limits"></a>Wat zijn de bekende servicebeperkingen?

Zie voor de Firewall van Azure-Servicelimieten, [Azure-abonnement en Servicelimieten, quotums en beperkingen](../azure-subscription-service-limits.md#azure-firewall-limits).

## <a name="can-azure-firewall-in-a-hub-virtual-network-forward-and-filter-network-traffic-between-two-spoke-virtual-networks"></a>Firewall van Azure kunnen in een hub-netwerk doorsturen en filteren van netwerkverkeer tussen twee virtuele spoke-netwerken?

Ja, kunt u Azure-Firewall in een hub-netwerk te routeren en filteren verkeer tussen twee spoke virtueel netwerk. Subnetten in elk van de virtuele spoke-netwerken beschikken over UDR die verwijst naar de Firewall van Azure als een standaard-gateway voor dit scenario goed te laten werken.

## <a name="can-azure-firewall-forward-and-filter-network-traffic-between-subnets-in-the-same-virtual-network-or-peered-virtual-networks"></a>Kan Azure Firewall doorsturen en filteren van netwerkverkeer tussen subnetten in het hetzelfde virtuele netwerk of de gekoppelde virtuele netwerken?

Ja. Configureren van de udr's als u wilt omleiden van verkeer tussen subnetten in hetzelfde VNET vereist echter extra aandacht. Tijdens het gebruik van de VNET-adresbereik als een doel-voorvoegsel voor de UDR voldoende is, stuurt deze ook al het verkeer van één machine naar een andere computer in hetzelfde subnet via de Firewall van Azure-instantie. Om dit te voorkomen, neem er een route voor het subnet in de UDR met een volgend hoptype van **VNET**. Beheren van deze routes, kan omslachtig en foutgevoelige klus zijn. De aanbevolen methode voor de segmentering van het interne netwerk is het gebruik van Netwerkbeveiligingsgroepen, die geen udr's is vereist.

## <a name="is-forced-tunnelingchaining-to-a-network-virtual-appliance-supported"></a>Geforceerde tunneling/koppelen aan een virtueel netwerkapparaat ondersteund?

Geforceerde tunneling wordt niet ondersteund door standaard, maar deze kan worden ingeschakeld met behulp van ondersteuning.

Firewall van Azure moet directe verbinding met Internet hebben. Als uw AzureFirewallSubnet een standaardroute naar uw on-premises netwerk via BGP achterhaalt, moet u deze overschrijven met een UDR 0.0.0.0/0 met de **NextHopType** waarde ingesteld als **Internet** direct onderhouden Verbinding met Internet. Standaard, dienen de Firewall van Azure biedt geen ondersteuning voor een geforceerde tunneling naar een on-premises netwerk.

Echter, als uw configuratie geforceerde tunneling naar een on-premises netwerk vereist, Microsoft wordt hiervoor ondersteuning bieden op basis van per geval. Neem contact op met ondersteuning voor zodat we uw aanvraag kunt controleren. Als geaccepteerd, we uw abonnement en zorg ervoor dat de internetverbinding vereist firewall wordt onderhouden.

## <a name="are-there-any-firewall-resource-group-restrictions"></a>Zijn er firewall beperkingen van de resource?

Ja. De firewall, subnet, VNet en het openbare IP-adres moeten zich in dezelfde resourcegroep bevinden.

## <a name="when-configuring-dnat-for-inbound-network-traffic-do-i-also-need-to-configure-a-corresponding-network-rule-to-allow-that-traffic"></a>Bij het configureren van DNAT voor binnenkomend netwerkverkeer, heb ik ook nodig om te configureren van een regel voor de bijbehorende als u wilt toestaan dat dit verkeer?

Nee. NAT-regels toevoegen impliciet een overeenkomende regel om de vertaalde verkeer te staan. U kunt dit gedrag overschrijven door expliciet een verzameling netwerkregels toe te voegen met regels voor weigeren die overeenkomen met het omgezette verkeer. Zie [Verwerkingslogica voor Azure Firewall-regels](rule-processing.md) voor meer informatie over de verwerkingslogica voor Azure Firewall-regels.

## <a name="how-do-wildcards-work-in-an-application-rule-target-fqdn"></a>Hoe werken jokertekens in het doel in een toepassing-regel FQDN?

Als u configureert * **. contoso.com**, hierdoor *anyvalue*. contoso.com, maar niet contoso.com (het toppunt van de domein). Als u toestaan dat het toppunt van het domein wilt, moet u deze expliciet configureren als een doel-FQDN.

## <a name="what-does-provisioning-state-failed-mean"></a>Wat doet *Inrichtingsstatus: Kan geen* betekent?

Wanneer een wijziging in de configuratie is toegepast, probeert de Firewall van Azure om bij te werken van de onderliggende back-end-exemplaren. In zeldzame gevallen, een van deze back-end-instanties om bij te werken met de nieuwe configuratie kan mislukken en het updateproces stopt met een mislukte Inrichtingsstatus. Uw Azure-Firewall nog steeds operationeel is, maar de configuratie van de toegepaste mogelijk in een inconsistente status, waar sommige exemplaren beschikken over de vorige configuratie waarin anderen de bijgewerkte regelset hebben. Als dit het geval is, probeert u het bijwerken van de configuratie nog een keer totdat de bewerking is geslaagd en uw Firewall wordt een *geslaagd* Inrichtingsstatus.
