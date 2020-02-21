---
title: 'Connectiviteitsproviders en -locaties: Azure ExpressRoute | Microsoft Docs'
description: Dit artikel bevat een gedetailleerd overzicht van de locaties waar services worden aangeboden en hoe u verbinding maakt met Azure-regio's. Gesorteerd op connectiviteitsprovider.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 02/15/2020
ms.author: cherylmc
ms.openlocfilehash: 5eb837ac6aa336a6a3f889a8bb5279be413ec31f
ms.sourcegitcommit: f255f869c1dc451fd71e0cab340af629a1b5fb6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2020
ms.locfileid: "77371229"
---
# <a name="expressroute-partners-and-peering-locations"></a>Partners en peeringlocaties voor ExpressRoute

> [!div class="op_single_selector"]
> * [Locaties per provider](expressroute-locations.md)
> * [Providers per locatie](expressroute-locations-providers.md)


De tabellen in dit artikel bevatten informatie over ExpressRoute geografische dekking en locaties, ExpressRoute-connectiviteits providers en ExpressRoute-systeem integrators (SIs).

> [!Note]
> Azure-regio's en ExpressRoute-locaties zijn twee verschillende concepten, en het verschil tussen de twee is van cruciaal belang voor het verkennen van een Azure Hybrid Networking-verbinding. 
>
>

## <a name="azure-regions"></a>Azure-regio's
Azure-regio's zijn wereld wijde data centers waar Azure compute-, netwerk-en opslag bronnen zich bevinden. Bij het maken van een Azure-resource moet een klant een resource locatie selecteren. De resource locatie bepaalt in welk Azure-Data Center (of beschikbaarheids zone) de resource wordt gemaakt.

## <a name="expressroute-locations"></a>ExpressRoute-locaties
ExpressRoute-locaties (soms ook wel peering-locaties of Vergader locaties genoemd) zijn co-locatie faciliteiten waar micro soft Enter prise Edge (MSEE)-apparaten zich bevinden. ExpressRoute locaties zijn het ingangs punt naar het netwerk van micro soft, en zijn wereld wijd gedistribueerd, waardoor klanten de mogelijkheid hebben om verbinding te maken met het netwerk van micro soft over de hele wereld. Deze locaties zijn waar ExpressRoute partners en ExpressRoute direct-klanten cross-connections naar het netwerk van micro soft verlenen. Over het algemeen hoeft de ExpressRoute-locatie niet overeen te komen met de Azure-regio. Een klant kan bijvoorbeeld een ExpressRoute-circuit maken met de resource locatie *VS-Oost*, op de vestiging *Seattle* peering.

U hebt toegang tot Azure-services in alle regio's binnen een geopolitieke regio als u bent verbonden met ten minste één ExpressRoute-locatie in die geopolitieke regio.

## <a name="locations"></a>Azure-regio's naar ExpressRoute-locaties binnen een geopolitieke regio.
In de volgende tabel vindt u een toewijzing van Azure-regio's aan ExpressRoute-locaties binnen een geopolitieke regio.

| **Geopolitieke regio** | **Azure-regio's** | **ExpressRoute-locaties** |
| --- | --- | --- |
| **Australië - overheid** |Australië Centraal, Australië Centraal 2 |Canberra, Canberra2 |
| **Europa** | Frankrijk-centraal, Frankrijk-zuid, Duitsland-noord, Duitsland-west-centraal, Europa-noord, Noor wegen Oost, Noor wegen West, Zwitserland-noord, Zwitserland-west, UK-west, UK-zuid, Europa-west |Amsterdam, Amsterdam2, kopen Hagen, Dublin, Frankfurt, Genève, Londen, London2, Marseille, Milaan, München, Newport (Wales), Utrecht, Parijs, Stavanger, Stockholm, Zürich |
| **Noord-Amerika** |VS - oost, VS - west, VS - oost 2, VS - west 2, VS - centraal, VS - zuid-centraal, VS - noord-centraal, VS - west-centraal, Canada - centraal, Canada - oost |Atlanta, Chicago, Amsterdam, Denver, Las Miami, Los Angeles,, New York, San Antonio, Seattle, Silicon dal, Silicon Valley2, Washington DC, Washington DC2, Montreal, Quebec, Amsterdam |
| **Azië** | Azië - oost, Azië - zuidoost | Bangkok, Hongkong, Hongkong Kong2, Jakarta, Kuala Lumpur, Singapore, Singapore2, Taiwan |
| **India** | India - west, India - centraal, India - zuid |Chennai, Chennai2, Mumbai, Mumbai2 |
| **Japan** | Japan - west, Japan - oost |Osaka, Tokio, Tokyo2 |
| **Oceania** | Australië - zuidoost, Australië - oost |Auckland, Melbourne, Perth, Sydney, Sydney2 |
| **Zuid-Korea** | Korea - centraal, Korea - zuid |Busan, Seoul|
| **VAE** | UAE-centraal, UAE-noord | Dubai, Dubai2 |
| **Zuid-Afrika** | Zuid-Afrika-west, Zuid-Afrika-noord |Kaapstad, Johannesburg |
| **Zuid-Amerika** | Brazilië - zuid |Sao Paulo |


## <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>Regio's en geopolitieke grenzen voor nationale clouds
De volgende tabel bevat informatie over regio's en geopolitieke grenzen voor nationale clouds.

| **Geopolitieke regio** | **Azure-regio's** | **ExpressRoute-locaties** |
| --- | --- | --- |
| **Cloud van de Amerikaanse overheid** |US Gov - AZ, US Gov - Iowa, US Gov - TX, US Gov - Virginia, US DoD Central, US DoD East  |Chicago, Dallas, New York, Phoenix, San Antonio, Seattle, Silicon Valley, Washington DC |
| **China - oost** |China - oost, China - oost2 |Shanghai, Shanghai2 |
| **China - noord** |China - noord, China - noord2 |Beijing, Beijing2 |
| **Duitsland** |Duitsland Centraal, Duitsland Oost |Berlijn, Frankfurt |

Connectiviteit tussen de geopolitieke regio's wordt niet ondersteund op de standaard ExpressRoute-SKU. U moet de invoegtoepassing ExpressRoute Premium inschakelen voor ondersteuning van globale connectiviteit. Connectiviteit met nationale cloudomgevingen wordt niet ondersteund. U kunt met uw connectiviteitsprovider samenwerken als de noodzaak daartoe zich voordoet.

## <a name="partners"></a>ExpressRoute-connectiviteitsproviders

De volgende tabel toont de locaties per serviceprovider. Als u alle beschikbare providers per locatie wilt weergeven, raadpleegt u [Serviceproviders per locatie](expressroute-locations-providers.md).


### <a name="global-commercial-azure"></a>Wereld wijde commerciële Azure

| **Serviceprovider** | **Microsoft Azure** | **Office 365**  | **Locaties** |
| --- | --- | --- | --- |
| **[AARNet](https://www.aarnet.edu.au/network-and-services/connectivity-services/azure-expressroute)** |Ondersteund |Ondersteund |Melbourne, Sydney |
| **[Airtel](https://www.airtel.in/business/#/)** | Ondersteund | Ondersteund | Chennai2, Mumbai2 |
| **[AIS](http://business.ais.co.th/solution/microsoft-azure.html?category=cloud)** | Ondersteund | Ondersteund | Bangkok |
| **[Aryaka Networks](https://www.aryaka.com/)** |Ondersteund |Ondersteund |Amsterdam, Chicago, Amsterdam, Hong Kong SAR, Sao Paulo, Seattle, silicone dal, Singapore, Tokio, Washington DC |
| **[Ascenty Data Centers](https://www.ascenty.com/en/cloud/microsoft-express-route)** |Ondersteund |Ondersteund |Sao Paulo |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Ondersteund |Ondersteund |Amsterdam, Chicago, Dallas, Londen, Silicon Valley, Singapore, Sydney, Tokio, Toronto, Washington DC |
| **[BBIX](https://www.bbix.net/en/service/ix/)** | Ondersteund | Ondersteund | Tokio |
| **[BCX](https://www.bcx.co.za/solutions)** |Ondersteund |Ondersteund |Kaap stad|
| **[Bell Canada](https://business.bell.ca/shop/enterprise/cloud-connect-access-to-cloud-partner-services)** |Ondersteund |Ondersteund |Montreal, Toronto, Quebec |
| **[British Telecom](https://www.globalservices.bt.com/en/solutions/products/cloud-connect-azure)** |Ondersteund |Ondersteund |Amsterdam, Hong Kong SAR, Johannes Burg, Londen, Newport (Wales), Sao Paulo, silicium dal, Singapore, Sydney, Tokio, Washington DC |
| **[C3ntro](https://www.c3ntro.com/data1/express-route1.php)** |Ondersteund |Ondersteund |Miami |
| **CDC** | Ondersteund | Ondersteund | Canberra, Canberra2 |
| **[CenturyLink Cloud Connect](https://www.centurylink.com/cloudconnect)** |Ondersteund |Ondersteund |Amsterdam2, Chicago, Hongkong, Las London2, New York, Parijs, San Antonio, silicium dal, Tokio, Toronto, Washington DC, Washington DC2 |
| **[Chief telecom municatie](https://www.chief.com.tw/dispPageBox/ct.aspx?ddsPageID=ENCLOUDSERVICE&dbid=4852937342)** |Ondersteund |Ondersteund |Hong Kong, Taiwan |
| **China-Mobiel internationaal** |Ondersteund |Ondersteund | Singapore |
| **China Telecom Global** |Ondersteund |Ondersteund |Hongkong |
| **Unicom-China-wereld wijd** |Ondersteund |Ondersteund | Singapore2 |
| **[Cologix](https://www.cologix.com/hyperscale/microsoft-azure/)** |Ondersteund |Ondersteund |Chicago, Dallas, Montreal, Toronto, Washington DC |
| **[Colt](https://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Ondersteund |Ondersteund |Amsterdam, Amsterdam2, Chicago, Dublin, Frankfurt, Londen, London2, Newport, New York, Osaka, Parijs, silicium dal, Silicon Valley2, Singapore2, Tokio, Washington DC |
| **[Comcast](https://business.comcast.com/landingpage/microsoft-azure)** |Ondersteund |Ondersteund |Chicago, Silicon Valley, Washington DC |
| **[CoreSite](https://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)** |Ondersteund |Ondersteund |Chicago, Denver, Los Angeles, New York, Silicon Valley, Silicon Valley2, Washington DC, Washington DC2 |
| **[DE CIX](https://www.de-cix.net/en/de-cix-service-world/directcloud/find-a-cloud-service/detail/microsoft-azure)** | Ondersteund |Ondersteund |Amsterdam2, Frankfurt, Marseille|
| **[Devoli](https://devoli.com/expressroute)** | Ondersteund |Ondersteund | Auckland, Melbourne, Sydney |
| **du datamena** |Ondersteund |Ondersteund | Dubai2 |
| **eir** |Ondersteund |Ondersteund |Dublin|
| **[Wereld wijde communicatie met Epsilon](https://www.epsilontel.com/solutions/direct-cloud-connect)** |Ondersteund |Ondersteund |Singapore, Singapore2 |
| **[Equinix](https://www.equinix.com/partners/microsoft-azure/)** |Ondersteund |Ondersteund |Amsterdam, Atlanta, Chicago, Groningen, Dublin, Frankfurt, Genève, Hongkong SAR, Londen, London2, Los Angeles, Melbourne, Miami, New York, Osaka, Parijs, Sao Paulo, Seattle, silicone dal, Singapore, Stockholm, Sydney, Tokio, Toronto, Washington DC |
| **Etisalat VAE** |Ondersteund |Ondersteund |Dubai|
| **[euNetworks](https://eunetworks.com/services/solutions/cloud-connect/microsoft-azure-expressroute/)** |Ondersteund |Ondersteund |Amsterdam, Amsterdam2, Dublin, Londen |
| **FarEasTone** |Ondersteund |Ondersteund |Taipei|
| **GÉANT** |Ondersteund |Ondersteund |Amsterdam, Frankfurt, Marseille |
| **[Global Cloud Xchange (GCX)](https://globalcloudxchange.com/cloud-platform/cloud-x-fusion/)** | Ondersteund| Ondersteund | Chennai, Mumbai |
| **Intelsat** | Ondersteund | Ondersteund | Washington DC2 |
| **[InterCloud](https://www.intercloud.com/)** |Ondersteund |Ondersteund |Amsterdam, Chicago, Hongkong, Londen, New York, Parijs, Silicon dal, Singapore, Washington DC, Zürich |
| **[Internet2](https://www.internet2.edu/products-services/cloud-services-applications/microsoft-azure/#service-cloud-connect)** |Ondersteund |Ondersteund |Chicago, Amsterdam, silicium dal, Washington DC |
| **[Internet Initiative Japan Inc. - IIJ](https://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)** |Ondersteund |Ondersteund |Osaka, Tokio |
| **[Internet Solutions - Cloud Connect](https://www.is.co.za/solution/cloud-connect/)** |Ondersteund |Ondersteund |Kaapstad, Johannesburg, Londen |
| **[Interxion](https://www.interxion.com/why-interxion/colocate-with-the-clouds/Microsoft-Azure/)** |Ondersteund |Ondersteund |Amsterdam, Amsterdam2, kopen Hagen, Dublin, Frankfurt, Londen, Marseille, Parijs, Zürich |
| **[IX Reach](https://www.ixreach.com/partners/cloud-partners/microsoft-azure/)**|Ondersteund |Ondersteund | Amsterdam, London2, silicone dal, Toronto, Washington DC |
| **Jaguar-netwerk** |Ondersteund |Ondersteund |Marseille|
| **[Jisc](https://www.jisc.ac.uk/microsoft-azure-expressroute)** |Ondersteund |Ondersteund |Londen |
| **[KINX](https://www.kinx.net/service/network/cloudhub/ms-expressroute/?lang=en)** |Ondersteund |Ondersteund |Seoul |
| **[Kordia](https://www.kordia.co.nz/cloudconnect)** | Ondersteund |Ondersteund |Auckland, Sydney |
| **[KPN](https://www.kpn.com/zakelijk/cloud/connect.htm)** | Ondersteund | Ondersteund | Amsterdam |
| **[KT](https://cloud.kt.com/portal/ktcloudportal.epc.productintro.partnershipcloud_ConnectHub.html)** | Ondersteund | Ondersteund | Seoul |
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Ondersteund |Ondersteund |Amsterdam, Chicago, Dallas, Londen, Newport (Wales), Sao Paulo, Seattle, Silicon Valley, Singapore, Washington DC |
| **LG CNS** |Ondersteund |Ondersteund |Busan, Seoul |
| **[Liquid Telecom](https://www.liquidtelecom.com/products-and-services/cloud.html)** |Ondersteund |Ondersteund |Kaapstad, Johannesburg |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Ondersteund |Ondersteund |Amsterdam, Atlanta, Auckland, Chicago, Groningen, Denver, Dubai2, Dublin, Frankfurt, Genève, Hongkong SAR, Las, Londen, London2, Los Angeles, Melbourne, Miami, Montreal, New York, Utrecht, Perth, Quebec City, San Antonio, Seattle, Silicon dal, Singapore, Singapore2, Sydney, Sydney2, Tokio, Toronto, Washington DC, Zürich |
| **[MTN](https://www.mtnbusiness.com/en/enterprise/Pages/microsoft-express-route.aspx)** |Ondersteund |Ondersteund |Londen |
| **[Neutrona Networks](https://www.neutrona.com/index.php/azure-expressroute/)** |Ondersteund |Ondersteund |Dallas, Los Angeles, Miami, Sao Paulo, Washington DC |
| **[Next Generation Data](https://www.nextgenerationdata.co.uk/ngd-cloud-gateway/)** |Ondersteund |Ondersteund |Newport (Wales) |
| **[NEXTDC](https://www.nextdc.com/services/axon-ethernet/microsoft-expressroute)** |Ondersteund |Ondersteund |Melbourne, Perth, Sydney, Sydney2 |
| **[NTT Communications](https://www.ntt.com/en/services/network/virtual-private-network.html)** |Ondersteund |Ondersteund |Amsterdam, Hongkong SAR, Londen, Los Angeles, Osaka, Singapore, Sydney, Tokio, Washington DC |
| **[NTT EAST](https://flets.com/cloudgateway/crossconnect/)** |Ondersteund |Ondersteund |Tokio |
| **[NTT SmartConnect](https://cloud.nttsmc.com/cxc/azure.html)** |Ondersteund |Ondersteund |Osaka |
| **[Optus](https://www.optus.com.au/enterprise/)** |Ondersteund |Ondersteund |Melbourne, Sydney |
| **[Orange](https://www.orange-business.com/en/products/business-vpn-galerie)** |Ondersteund |Ondersteund |Amsterdam, Amsterdam2, Dubai2, Frankfurt, Hong Kong SAR, Johannes Burg, Londen, Parijs, Sao Paulo, silicone dal, Singapore, Sydney, Tokio, Washington DC |
| **[Orixcom](https://www.orixcom.com/cloud-solutions/)** | Ondersteund | Ondersteund | Dubai2 |
| **[PacketFabric](https://www.packetfabric.com/cloud-connectivity/microsoft-azure)** |Ondersteund |Ondersteund |Chicago, Silicon Valley, Washington DC |
| **[PCCW Global Limited](https://consoleconnect.com/clouds/#azureRegions)** |Ondersteund |Ondersteund |Chicago, Hongkong SAR, Londen |
| **[Sejong Telecom](https://www.sejongtelecom.net/en/pages/service/cloud_ms)** |Ondersteund |Ondersteund |Seoul |
| **[SES](https://www.ses.com/networks/signature-solutions/signature-cloud/ses-and-azure-expressroute)** | Ondersteund |Ondersteund | Washington DC |
| **[SIFY](http://telecom.sify.com/azure-expressroute.html)** |Ondersteund |Ondersteund |Chennai, Mumbai2 |
| **[SingTel](https://www.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |Ondersteund |Ondersteund |Singapore, Singapore2 |
| **[Softbank](https://www.softbank.jp/biz/cloud/cloud_access/direct_access_for_az/)** |Ondersteund |Ondersteund |Osaka, Tokio |
| **[Spark NZ](https://www.sparkdigital.co.nz/solutions/connectivity/cloud-connect/)** |Ondersteund |Ondersteund |Auckland, Sydney |
| **[Sprint](https://business.sprint.com/solutions/cloud-networking/)** |Ondersteund |Ondersteund |Chicago, Silicon Valley, Washington DC |
| **[Swisscom](https://www.swisscom.ch/en/business/enterprise/offer/cloud-data-center/microsoft-cloud-services/microsoft-azure-von-swisscom.html)** | Ondersteund | Ondersteund | Zurich |
| **[Tata Communications](https://www.tatacommunications.com/lp/izo/azure/azure_index.html)** |Ondersteund |Ondersteund |Amsterdam, Chennai, Hongkong SAR, Londen, Mumbai, Sao Paulo, silicium dal, Singapore, Washington DC |
| **[Telefonica](https://www.business-solutions.telefonica.com/es/enterprise/solutions/efficient-infrastructure/managed-voice-data-connectivity/)** |Ondersteund |Ondersteund |Amsterdam, Sao Paulo |
| **[Telehouse - KDDI](https://www.telehouse.net/solutions/cloud-services/cloud-link)** |Ondersteund |Ondersteund |Londen, London2 |
| **Telenor** |Ondersteund |Ondersteund |Amsterdam, Londen, Utrecht |
| **[Telia Carrier](https://www.teliacarrier.com/)** | Ondersteund | Ondersteund |Amsterdam, Chicago, Amsterdam, Frankfurt, Hongkong, Londen, Utrecht, Parijs, silicone dal, Stockholm, Washington DC |
| **Telmex Uninet**| Ondersteund | Ondersteund | Dallas |
| **[Telstra Corporation](https://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** |Ondersteund |Ondersteund |Melbourne, Singapore, Sydney |
| **[Telus](https://www.telus.com)** |Ondersteund |Ondersteund |Montreal, Seattle, Toronto |
| **[Teraco](https://www.teraco.co.za/services/africa-cloud-exchange/)** |Ondersteund |Ondersteund |Kaapstad, Johannesburg |
| **[TIME dotCom](https://www.time.com.my/enterprise/connectivity/cloud-interconnect)** | Ondersteund | Ondersteund | Kuala Lumpur |
| **[Transtelco](https://transtelco.net/enterprise-services/)** |Ondersteund |Ondersteund |Dallas, Los Angeles|
| **[UOLDIVEO](https://www.uoldiveo.com.br/)** |Ondersteund |Ondersteund |Sao Paulo |
| **[Verizon](https://enterprise.verizon.com/products/network/application-enablement/secure-cloud-interconnect/)** |Ondersteund |Ondersteund |Amsterdam, Chicago, Amsterdam, Hongkong SAR, Londen, Mumbai, silicium dal, Singapore, Sydney, Tokio, Toronto, Washington DC |
| **[Viasat](http://www.directcloud.viasatbusiness.com/)** | Ondersteund | Ondersteund | Washington DC2 |
| **[Vocus groep NZ](https://www.vocus.co.nz/business/cloud-data-centres)** | Ondersteund | Ondersteund | Auckland, Sydney |
| **[Vodafone](https://www.vodafone.com/business/global-enterprise/global-connectivity/vodafone-ip-vpn-cloud-connect)** |Ondersteund |Ondersteund |Amsterdam2, Londen, Singapore |
| **[Vodafone-idee](https://discover.vodafone.in/business/enterprise-solutions/connectivity/vpn-extended-connect)** | Ondersteund | Ondersteund | Mumbai, Mumbai2 |
| **[Zayo](https://www.zayo.com/solutions/industries/cloud-connectivity/microsoft-expressroute)** |Ondersteund |Ondersteund |Amsterdam, Chicago, Amsterdam, Denver, Londen, Los Angeles, Montreal, New York, Parijs, Seattle, Silicon dal, Toronto, Washington DC, Washington DC2 |

 **+** betekent binnenkort beschikbaar

### <a name="national-cloud-environment"></a>Nationale cloudomgeving

Azure National Clouds zijn geïsoleerd van elkaar en van wereld wijde commerciële Azure. ExpressRoute voor een Azure-Cloud kan geen verbinding maken met de Azure-regio's in de andere. 

### <a name="us-government-cloud"></a>Cloud van de Amerikaanse overheid

| **Serviceprovider** | **Microsoft Azure** | **Office 365** | **Locaties** |
| --- | --- | --- | --- |
| **[AT&T NetBond](https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** |Ondersteund |Ondersteund |Chicago, Phoenix, silicone dal, Washington DC |
| **[CenturyLink Cloud Connect](https://www.centurylink.com/cloudconnect)** |Ondersteund |Ondersteund |New York, Phoenix, San Antonio, Washington DC |
| **[Equinix](https://www.equinix.com/partners/microsoft-azure/)** |Ondersteund |Ondersteund |Chicago, Dallas, New York, Seattle, Silicon Valley, Washington DC |
| **[Level 3 Communications](http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** |Ondersteund |Ondersteund |Chicago, Silicon Valley, Washington DC |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Ondersteund | Ondersteund | Chicago, Dallas, San Antonio, Seattle, Washington DC |
| **[Verizon](http://news.verizonenterprise.com/2014/04/secure-cloud-interconnect-solutions-enterprise/)** |Ondersteund |Ondersteund |Chicago, Dallas, New York, Silicon Valley, Washington DC |

### <a name="china"></a>China

| **Serviceprovider** | **Microsoft Azure** | **Office 365** | **Locaties** |
| --- | --- | --- | --- |
| **China Telecom** |Ondersteund |Niet ondersteund |Beijing, Beijing2, Shanghai, Shanghai2 |
| **Unicom China** | Ondersteund | Niet ondersteund | Beijing2 |
| **[GDS](http://www.gds-services.com/en/about_2.html)** |Ondersteund |Niet ondersteund |Beijing2, Shanghai2 |

Zie [ExpressRoute in China](http://www.windowsazure.cn/home/features/expressroute/) voor meer informatie.

### <a name="germany"></a>Duitsland

| **Serviceprovider** | **Microsoft Azure** | **Office 365** | **Locaties** |
| --- | --- | --- | --- |
| **[Colt](https://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** |Ondersteund |Niet ondersteund |Frankfurt |
| **[Equinix](https://www.equinix.com/partners/microsoft-azure/)** |Ondersteund |Niet ondersteund |Frankfurt |
| **[e-shelter](https://www.e-shelter.de/en/microsoft-expressroutetm)** |Ondersteund |Niet ondersteund |Berlijn |
| **Interxion** |Ondersteund |Niet ondersteund |Frankfurt |
| **[Megaport](https://www.megaport.com/services/microsoft-expressroute/)** |Ondersteund  | Niet ondersteund | Berlijn |
| **T-Systems** |Ondersteund |Niet ondersteund |Berlijn |

## <a name="connectivity-through-exchange-providers"></a>Connectiviteit via Exchange-providers

Als uw connectiviteitsprovider niet wordt vermeld in de vorige secties, kunt u alsnog verbinding maken.

* Neem contact op met uw connectiviteitsprovider om na te gaan of ze zijn verbonden met een van de exchange-punten in de bovenstaande tabel. Via de volgende koppelingen vindt u meer informatie over services die door de exchange-providers worden verstrekt. Verschillende connectiviteitsproviders zijn al verbonden met Ethernet-exchange-punten.
  * [Cologix](https://www.cologix.com/)
  * [CoreSite](https://www.coresite.com/)
  * [Equinix Cloud Exchange](https://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [Interxion](https://www.interxion.com/products/interconnection/cloud-connect/)
  * [IX Reach](https://www.ixreach.com/partners/cloud-partners/microsoft-azure/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [NextDC](https://www.nextdc.com/)
  * [PacketFabric](https://www.packetfabric.com/cloud-connectivity/microsoft-azure) 
  * [Teraco](https://www.teraco.co.za/platform-teraco/africa-cloud-exchange/)

* Vraag uw connectiviteitsprovider om uw netwerk uit te breiden tot de gewenste peeringlocatie.
  * Vergewis u ervan dat de connectiviteitsprovider uw connectiviteit uitbreidt op een maximaal beschikbare manier, zodat er geen storingspunten zijn.
* Vraag een ExpressRoute-circuit aan met het exchange-punt wanneer uw connectiviteitsprovider verbinding maakt met Microsoft.
  * Volg de stappen in [Create an ExpressRoute circuit](expressroute-howto-circuit-classic.md) (Een ExpressRoute-circuit maken) om connectiviteit in te stellen.

## <a name="connectivity-through-satellite-operators"></a>Connectiviteit via satelliet operators
Als u extern bent en geen glasvezel connectiviteit hebt of als u andere connectiviteits opties wilt verkennen, kunt u de volgende satelliet-Opera tors controleren. 

* Intelsat
* [SES](https://www.ses.com/networks/signature-solutions/signature-cloud/ses-and-azure-expressroute)
* [Viasat](http://www.directcloud.viasatbusiness.com/)

## <a name="connectivity-through-additional-service-providers"></a>Connectiviteit via aanvullende service providers

| **Connectiviteitsprovider** | **Exchange** | **Locaties** |
| --- | --- | --- |
| **[1CLOUDSTAR](https://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)** |Equinix |Singapore |
| **[Airgate Technologies, Inc.](https://www.airgate.ca/expressroute)** | Equinix, Cologix | Toronto, Montreal |
| **[Alaska Communications](https://www.alaskacommunications.com/For-Your-Business/Direct-Cloud-Service)** |Equinix |Seattle |
| **[Altice Business](https://golightpath.com/transport)** |Equinix |New York, Washington DC |
| **[Arteria Networks Corporation](https://www.arteria-net.com/business/service/cloud/sca/)** |Equinix |Tokio |
| **[Axtel](https://alestra.mx/landing/expressrouteazure/)** |Equinix |Dallas|
| **[Beanfield Metroconnect](https://www.beanfield.com/cloud-exchange/)** |Megaport |Toronto|
| **[Bezeq International Ltd.](https://www.bezeqint.net/english)** | euNetworks | Londen |
| **[BICS](https://bics.com/bics-solutions-suite/cloud-connect/)** | Equinix | Amsterdam, Frankfurt, Londen, Singapore, Washington DC |
| **[BroadBand Tower, Inc.](https://www.bbtower.co.jp/product-service/data-center/network/dcconnect-for-azure/)** | Equinix | Tokio |
| **[C3ntro Telecom](https://www.c3ntro.com/data/express-route)** | Equinix, Megaport | Dallas |
| **[Chief](https://www.chief.com.tw/dispPageBox/ct.aspx?ddsPageID=ENCLOUDSERVICE&dbid=4852937342)** | Equinix | Hongkong SAR |
| **[Cinia](https://www.cinia.fi/en/services/connectivity-services/direct-public-cloud-connection.html)** | Equinix, Megaport | Frankfurt, Hamburg |
| **[CloudXpress](https://www2.telenet.be/fr/business/produits-services/internet/cloudxpress/)** | Equinix | Amsterdam | 
| **[CMC Telecom](https://cmctelecom.vn/san-pham/value-added-service-and-it/cmc-telecom-cloud-express-en/)** | Equinix | Singapore | 
| **[Aptum Technologies](https://aptum.com/services/cloud/managed-azure/)**| Equinix | Montreal, Toronto |
| **[CoreAzure](http://www.coreazure.com/expressroute/)**| Equinix | Londen |
| **[Cox Business](https://www.cox.com/business/networking/cloud-connectivity.html)**| Equinix | Dallas, Silicon Valley, Washington DC |
| **[Kroon Castle](https://fiber.crowncastle.com/solutions/added/cloud-connect)**| Equinix | Atlanta, Chicago, Dallas, Los Angeles, New York, Washington DC |
| **[Data Foundry](https://www.datafoundry.com/services/cloud-connect)** | Megaport | Dallas |
| **[Epsilon Telecommunications Limited](https://www.epsilontel.com/data-connectivity/cloud-access/)** | Equinix | London, Singapore, Washington DC |
| **[Eurofiber](https://eurofiber.nl/microsoft-azure/)** | Equinix | Amsterdam |
| **[Exponential E](https://www.exponential-e.com/services/connectivity-services/cloud-connect-exchange)** | Equinix | Londen |
| **[Fastweb S.p.A](https://www.fastweb.it/grandi-aziende/connessione-voce-e-wifi/scheda-prodotto/rete-privata-virtuale/)** | Equinix | Amsterdam |
| **[Fibrenoire](https://www.fibrenoire.ca/en/cloudextn)** | Megaport | Quebec |
| **[Gtt Communications Inc](https://www.gtt.net)** |Equinix | Washington DC |
| **[Gulf Bridge International](https://www.gbiinc.com/microsoft-azure-expressroute/)** | Equinix | Amsterdam |
| **[HSO](https://www.hso.co.uk/products/cloud-direct)** |Equinix | Londen, Slough |
| **[IVedha Inc](http://www.ivedha.com/cloud/manage-azure-cloud/express-route-4/)**| Equinix | Toronto |
| **[Kaalam Telecom Bahrein B. S. C](http://www.kalaam-telecom.com/en/inbusiness/expressroute.html)**| Level 3 Communications |Amsterdam |
| **LGA Telecom** |Equinix |Singapore|
| **[Macroview Telecom](http://www.macroview.com/en/scripts/catitem.php?catid=solution&sectionid=expressroute)** |Equinix |Hongkong SAR 
| **[Macquarie Telecom Group](https://macquariegovernment.com/secure-cloud/secure-cloud-exchange/)** | Megaport | Sydney |
| **[MainOne](https://www.mainone.net/services/connectivity/cloud-connect/)** |Equinix | Amsterdam |
| **[Masergy](https://www.masergy.com/solutions/hybrid-networking/cloud-marketplace/microsoft-azure)** | Equinix | Washington DC |
| **[MTN](https://www.mtnbusiness.com/en/enterprise/Pages/microsoft-express-route.aspx)** | Teraco | Kaapstad, Johannesburg |
| **[NexGen Networks](https://www.nexgen-net.com/nexgen-networks-direct-connect-microsoft-azure-expressroute.html)** | Interxion | Londen |
| **[Nianet](https://nianet.dk/produkter/internet/microsoft-expressroute)** |Equinix | Amsterdam, Frankfurt |
| **[POST Telecom Luxembourg](https://www.teralinksolutions.com/cloud-connectivity/cloudbridge-to-azure-expressroute/)**|Equinix | Amsterdam |
| **[Proximus](https://www.proximus.be/en/id_b_cl_proximus_external_cloud_connect/companies-and-public-sector/discover/magazines/expert-blog/proximus-external-cloud-connect.html)**|Equinix | Amsterdam, Dublin, Londen, Parijs |
| **[QSC AG](https://www.qsc.de/de/produkte-loesungen/cloud-services-und-it-outsourcing/pure-enterprise-cloud/multi-cloud-management/azure-expressroute/)** |Interxion | Frankfurt |  
| **Rogers** | Cologix, Equinix | Montreal, Toronto |
| **[Spectrum onderneming](https://enterprise.spectrum.com/services/cloud/cloud-connect.html)** | Equinix | Chicago, Dallas, Los Angeles, New York, Silicon Valley | 
| **[Tamares Telecom](https://www.tamarestelecom.com/our-services/#Connectivity)** | Equinix | Londen | 
| **[TDC Erhverv](https://tdc.dk/Produkter/cloudaccessplus)** | Equinix | Amsterdam | 
| **[Telecom Italia Sparkle](https://www.tisparkle.com/our-platform/corporate-platform/sparkle-cloud-connect#catalogue)**| Equinix | Amsterdam |
| **[Telekom Deutschland GmbH](https://cloud.telekom.de/de/infrastruktur/managed-it-services/managed-hybrid-infrastructure-mit-microsoft-azure)** | Interxion | Amsterdam, Frankfurt |
| **[Telia](https://www.telia.se/foretag/losningar/produkter-tjanster/datanet)** | Equinix | Amsterdam |
| **[ThinkTel](https://www.thinktel.ca/services/agile-ix-data/expressroute/)** | Equinix | Toronto | 
| **[United Information Highway (UIH)](https://www.uih.co.th/en/internet-solution/cloud-direct/uih-cloud-direct-for-microsoft-azure-expressroute)**| Equinix | Singapore |
| **[Venha Pra Nuvem](https://venhapranuvem.com.br/)** | Equinix | Sao Paulo |
| **[Webair](https://www.webair.com/microsoft-express-route-partnership/)**| Megaport | New York |
| **[Windstream](https://www.windstreambusiness.com/solutions/cloud-services/cloud-and-managed-hosting-services)**| Equinix | Chicago, Silicon Valley, Washington DC |
| **[X2nsat Inc.](https://www.x2nsat.com/expressroute/)** |Coresite |Silicium dal, silicium dal 2|
| **Zain** |Equinix |Londen|
| **[Zertia](https://www.zertia.es)**| Level 3 | Madrid |
| **[Zirro](https://zirro.com/services/)**| Cologix, Equinix | Montreal, Toronto |

## <a name="connectivity-through-datacenter-providers"></a>Connectiviteit via datacenter providers

| **Provider** | **Exchange** |
| --- | --- |
| **[CyrusOne](https://cyrusone.com/enterprise-data-center-services/connectivity-and-interconnection/cloud-connectivity-reaching-amazon-microsoft-google-and-more/microsoft-azure-expressroute/?doing_wp_cron=1498512235.6733090877532958984375)** | Megaport, PacketFabric |
| **[Cyxtera](https://www.cyxtera.com/data-center-services/interconnection)** | Megaport, PacketFabric |
| **[Banken](https://www.databank.com/platforms/connectivity/cloud-direct-connect/)** | Megaport |
| **[DataFoundry](https://www.datafoundry.com/services/cloud-connect/)** | Megaport |
| **[Digital Realty](https://www.digitalrealty.com/services/interconnection/service-exchange/)** | IX REACH, Mega Port PacketFabric |
| **[EdgeConnex](https://www.edgeconnex.com/services/edge-data-centers-proximity-matters/)** | Megaport, PacketFabric |
| **[Flexential](https://www.flexential.com/connectivity/cloud-connect-microsoft-azure-expressroute)** | IX Reach, Megaport, PacketFabric |
| **[QTS-data centers](https://www.qtsdatacenters.com/hybrid-solutions/connectivity/azure-cloud )** | Megaport, PacketFabric |
| **[Data Centers streamen]( https://www.streamdatacenters.com/products-services/network-cloud/ )** | Megaport |
| **[RagingWire Data Centers](https://www.ragingwire.com/wholesale/wholesale-data-centers-worldwide-nexcenters)** | IX Reach, Megaport, PacketFabric |
| **[vXchnge](https://www.vxchnge.com/colocation-services/interconnection)** | IX REACH, Mega Port |
| **[T5 Datacenters](https://t5datacenters.com/network-cloud-connect/)** | IX Reach |

## <a name="connectivity-through-national-research-and-education-networks-nren"></a>Connectiviteit via National Research en Education Networks (NREN)

| **Provider**|
| --- |
| **AARNET**| 
| **DeIC, through GÉANT**|
| **GARR via GÉANT**|
| **GÉANT**|
| **HEAnet via GÉANT**|
| **Internet2**|
| **JISC**|
| **RedIRIS via GÉANT**|
| **SINET**|
| **Surfnet via GÉANT**|

* Als uw connectiviteitsprovider hier niet wordt vermeld, controleert u of ze zijn verbonden aan een van de hierboven vermelde ExpressRoute Exchange-Partners.

## <a name="expressroute-system-integrators"></a>ExpressRoute-SI's
Het inschakelen van particuliere connectiviteit conform uw specifieke behoeften kan lastig zijn, al naargelang de schaal van uw netwerk. U kunt alle SI's uit de volgende tabel gebruiken om u te helpen met de voorbereidingen voor ExpressRoute.

| **System integrator** | **Continent** |
| --- | --- |
| **[Altogee](https://altogee.be/diensten/express-route/)** | Europa |
| **[Avanade Inc.](https://www.avanade.com/)** | Azië, Europa, Noord-Amerika, Zuid-Amerika |
| **[Bright Skies GmbH](https://bskies.io/expressroute)** | Europa
| **[Ensyst](https://www.ensyst.com.au)** | Azië
| **[Equinix Professional Services](https://www.equinix.com/services/consulting/)** | Noord-Amerika |
| **[FlexManage](https://www.flexmanage.com/cloud)** | Noord-Amerika |
| **[Lightstream](https://www.lightstream.tech/partners/microsoft-azure/)** | Noord-Amerika |
| **[The IT Consultancy Group](https://itconsult.com.au/)** | Australië |
| **[MOQdigital](https://www.moqdigital.com.au/insights/technical/network-connectivity-options-for-azure)** | Australië |
| **[MSG Services](https://www.msg-services.de/it-services/managed-services/cloud-outsourcing/)** | Europa (Duitsland) |
| **[Nelite](https://www.exakis-nelite.com/offres/)** | Europa |
| **[Nieuwe handtekening](https://newsignature.com/technologies/express-route/)** | Europa |
| **[OneAs1a](https://www.oneas1a.com/connectivity.html)** | Azië |
| **[Orange Networks](https://orange-networks.com/blog/88-azureexpressroute)** | Europa |
| **[Perficient](https://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** | Noord-Amerika |
| **[Presidio](https://info.presidio.com/microsoft-azure-expressroute)** | Noord-Amerika |
| **[sol-tec](https://www.sol-tec.com/what-we-do/)** | Europa |
| **[Venha Pra Nuvem](https://venhapranuvem.com.br/)** | Zuid-Amerika |
| **[Vigilant.IT](https://vigilant.it/expressroute)** | Australië |

## <a name="next-steps"></a>Volgende stappen
* Voor meer informatie over ExpressRoute raadpleegt u de [Veelgestelde vragen over ExpressRoute](expressroute-faqs.md).
* Controleer of aan alle vereisten is voldaan. Zie [ExpressRoute prerequisites](expressroute-prerequisites.md) (Vereisten voor ExpressRoute).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Locatiekaart"
