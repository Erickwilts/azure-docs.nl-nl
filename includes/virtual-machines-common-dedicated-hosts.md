---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 07/26/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 438f450efe207999947795878a9815fca64f45ca
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68702921"
---
De toegewezen Azure-host is een service die fysieke servers biedt en een of meer virtuele machines kan hosten die zijn toegewezen aan één Azure-abonnement. Toegewezen hosts zijn dezelfde fysieke servers die worden gebruikt in onze data centers als een resource. U kunt toegewezen hosts inrichten binnen een regio, een beschikbaarheids zone en een fout domein. Vervolgens kunt u Vm's rechtstreeks op uw ingerichte hosts plaatsen, in welke configuratie het beste voldoet aan uw behoeften.

> [!IMPORTANT]
> Exclusieve Azure-hosts bevindt zich momenteel in de open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.
>
> **Bekende preview-beperkingen**
> - Virtuele-machine schaal sets worden momenteel niet ondersteund op toegewezen hosts.
> - De eerste release van de preview-versie ondersteunt de volgende VM-serie: DSv3 en ESv3. 
> - Tijdens de preview-periode kunt u de grootte van een virtuele machine die is geïmplementeerd op een specifieke host niet wijzigen.
> - Controle over de onderhouds mogelijkheden bevindt zich in een beperkte preview. Begin met het nemen van deze [Benoemings enquête](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR6lJf7DwiQxNmz51ksQvxV9UNUM3UllWUjBMTFZQUFhHUDI0VTBPQlJFNS4u) om ze uit te proberen.  
> Tijdens de preview-periode bieden we geen optie voor gereserveerde capaciteit. 

## <a name="benefits"></a>Voordelen 

Het reserveren van de hele host biedt de volgende voor delen:

-   Hardwarematige isolatie op het niveau van de fysieke server. Er worden geen andere Vm's op uw hosts geplaatst. Toegewezen hosts worden in dezelfde data centers geïmplementeerd en delen hetzelfde netwerk en onderliggende opslag infrastructuur als andere, niet-geïsoleerde hosts.
-   Controle over onderhouds gebeurtenissen geïnitieerd door het Azure-platform. Hoewel het meren deel van de onderhouds gebeurtenissen weinig gevolgen heeft voor uw virtuele machines, zijn er enkele gevoelige werk belastingen waarbij elke seconde van de onderbreking een impact kan hebben. Met toegewezen hosts kunt u zich aanmelden bij een onderhouds venster om de gevolgen voor uw service te verminderen.
-   Met Azure Hybrid voordelen kunt u uw eigen licenties voor Windows en SQL naar Azure brengen. Het gebruik van de hybride voor delen biedt extra voor delen. Zie [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/)voor meer informatie.



## <a name="groups-hosts-and-vms"></a>Groepen, hosts en Vm's  

![Weer gave van de nieuwe resources voor toegewezen hosts.](./media/virtual-machines-common-dedicated-hosts/dedicated-hosts2.png)

Een **hostgroep** is een resource die een verzameling toegewezen hosts vertegenwoordigt. U maakt een hostgroep in een regio en een beschikbaarheids zone en voegt hierop hosts toe.

Een **host** is een resource die is toegewezen aan een fysieke server in een Azure-Data Center. De fysieke server wordt toegewezen wanneer de host wordt gemaakt. Er wordt een host gemaakt binnen een hostgroep. Een host heeft een SKU waarin wordt beschreven welke VM-grootten er kunnen worden gemaakt. Elke host kan meerdere Vm's hosten, van verschillende grootten, zolang ze afkomstig zijn uit dezelfde grootte reeks.

Wanneer u een virtuele machine in azure maakt, kunt u selecteren welke specifieke host moet worden gebruikt voor uw VM. U hebt de volledige controle over de virtuele machines die op uw hosts worden geplaatst.


## <a name="high-availability-considerations"></a>Overwegingen voor hoge Beschik baarheid 

Voor maximale Beschik baarheid implementeert u meerdere Vm's, verspreid over meerdere hosts (mini maal 2). Met Azure dedicated hosts hebt u verschillende opties om uw infra structuur in te richten op de grenzen van de isolatie van fouten.

### <a name="use-availability-zones-for-fault-isolation"></a>Beschikbaarheidszones gebruiken voor fout isolatie

Beschikbaarheids zones zijn unieke fysieke locaties binnen een Azure-regio. Elke zone bestaat uit een of meer datacenters die zijn uitgerust met onafhankelijke voeding, koeling en netwerken. Er wordt een hostgroep gemaakt in één beschikbaarheids zone. Nadat deze is gemaakt, worden alle hosts in die zone geplaatst. Als u een hoge Beschik baarheid in meerdere zones wilt bezorgen, moet u meerdere hostgroepen maken (één per zone) en de hosts vervolgens dienovereenkomstig verspreiden.

Als u een hostgroep toewijst aan een beschikbaarheids zone, moeten alle Vm's die op die host worden gemaakt, in dezelfde zone worden gemaakt.

### <a name="use-fault-domains-for-fault-isolation"></a>Fout domeinen gebruiken voor fout isolatie

Een host kan worden gemaakt in een specifiek fout domein. Net als bij een VM in een schaalset of beschikbaarheidsset worden hosts in verschillende fout domeinen op verschillende fysieke racks geplaatst in het Data Center. Wanneer u een hostgroep maakt, moet u het aantal fout domeinen opgeven. Wanneer u hosts in de hostgroep maakt, wijst u fout domein toe voor elke host. Voor de virtuele machines is geen fout domein toewijzing vereist.

Fout domeinen zijn niet hetzelfde als plaatsing. Als u hetzelfde fout domein voor twee hosts gebruikt, betekent dit niet dat ze zich in de buurt van elkaar bevinden.

Fout domeinen zijn binnen het bereik van de hostgroep. U moet geen veronderstelling doen over de anti-affiniteit tussen twee hostgroepen (tenzij ze zich in verschillende beschikbaarheids zones bevinden).

Vm's die zijn geïmplementeerd op hosts met verschillende fout domeinen, hebben hun onderliggende services voor beheerde schijven op meerdere opslag tempels om de fout isolatie beveiliging te verg Roten.

### <a name="using-availability-zones-and-fault-domains"></a>Beschikbaarheidszones en fout domeinen gebruiken

U kunt beide mogelijkheden samen gebruiken om nog meer fouten te isoleren. In dit geval geeft u de beschikbaarheids zone en het aantal fout domeinen op voor elke hostgroep, wijst u een fout domein toe aan elk van uw hosts in de groep en wijst u een beschikbaarheids zone toe aan elke virtuele machine.

De Resource Manager-voorbeeld sjabloon die [hier](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md) wordt aangetroffen, maakt gebruik van zones en fout domeinen om hosts te verspreiden voor maximale tolerantie in een regio.

## <a name="maintenance-control"></a>Onderhouds beheer

De infra structuur die uw virtuele machines ondersteunt, kan af en toe worden bijgewerkt om de betrouw baarheid, prestaties en beveiliging te verbeteren en om nieuwe functies te kunnen starten. Het Azure-platform probeert de impact van platform onderhoud zoveel mogelijk te minimaliseren, maar klanten met *onderhoud gevoelige* workloads kunnen zelfs enkele seconden niet verdragen dat de virtuele machine moet worden bevroren of ontkoppeld voor onderhoud.

**Onderhouds beheer** biedt klanten een optie voor het overs laan van regel matige platform updates die zijn gepland op hun toegewezen hosts en pas deze toe op het tijdstip van hun keuze in een voortschrijdende periode van 35 dagen.

> [!NOTE]
>  Onderhouds beheer bevindt zich momenteel in een beperkte preview-fase en vereist een voorbereidings proces. Gelden voor deze preview door een [inspectie enquête](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR6lJf7DwiQxNmz51ksQvxV9UNUM3UllWUjBMTFZQUFhHUDI0VTBPQlJFNS4u)in te dienen.

## <a name="capacity-considerations"></a>Overwegingen voor capaciteit

Zodra een toegewezen host is ingericht, wijst Azure deze toe aan de fysieke server. Hiermee wordt de beschik baarheid van de capaciteit gegarandeerd wanneer u uw virtuele machine moet inrichten. Azure gebruikt de volledige capaciteit in de regio (of zone) om een fysieke server voor uw host te kiezen. Het betekent ook dat klanten kunnen verwachten dat ze hun toegewezen host footprint kunnen verg Roten zonder dat er bijna geen ruimte meer is in het cluster.

## <a name="quotas"></a>Quota

Er is een standaard quotum limiet van 3000 Vcpu's voor toegewezen hosts per regio. Maar het aantal hosts dat u kunt implementeren, wordt ook beperkt door het quotum voor de VM-grootte familie die voor de host wordt gebruikt. Een abonnement op basis van **betalen per gebruik** heeft bijvoorbeeld alleen een quotum van 10 vcpu's beschikbaar voor de Dsv3 grootte serie, in de regio VS-Oost. In dit geval moet u een quotum verhoging tot ten minste 64 Vcpu's aanvragen voordat u een specifieke host kunt implementeren. Selecteer de knop **verhogen** in de rechter bovenhoek om zo nodig een aanvraag in te dienen.

![Scherm afbeelding van de pagina gebruik en quota's in de portal](./media/virtual-machines-common-dedicated-hosts/quotas.png)

Zie [vCPU quota's voor virtuele machines](/azure/virtual-machines/windows/quotas)voor meer informatie.

## <a name="pricing"></a>Prijzen

Gebruikers worden in rekening gebracht per toegewezen host, ongeacht het aantal Vm's dat is geïmplementeerd. In uw maandelijkse afschriften ziet u een nieuw Factureerbaar resource type van hosts. De Vm's op een toegewezen host worden nog steeds weer gegeven in uw instructie, maar de prijs is 0.

De prijs van de host wordt ingesteld op basis van de VM-familie, type (hardware-grootte) en regio. De prijs van een host is relatief ten opzichte van de grootste VM-grootte die op de host wordt ondersteund.

Software licentie verlening, opslag en netwerk gebruik worden afzonderlijk van de host en Vm's gefactureerd. Deze factureer bare items worden niet gewijzigd.

Zie [prijzen voor toegewezen hosts voor Azure](https://aka.ms/ADHPricing)voor meer informatie.
 
## <a name="vm-families-and-hardware-generations"></a>VM-families en hardware-generaties

Er wordt een SKU gedefinieerd voor een host en deze vertegenwoordigt de VM-grootte reeks en-type. U kunt meerdere virtuele machines van verschillende grootten op één host combi neren, zolang ze dezelfde grootte reeks hebben. Het type is de hardware-generatie die momenteel beschikbaar is in de regio.

Een `types` andere VM-reeks is van verschillende CPU-leveranciers en heeft verschillende CPU-generaties en aantal kernen.

Raadpleeg de pagina met [prijzen](https://aka.ms/ADHPricing) voor de host voor meer informatie.

Tijdens de preview-periode ondersteunen we de volgende SKU\types:  DSv3_Type1 en ESv3_Type1

 
## <a name="host-life-cycle"></a>Levens cyclus van host


Azure bewaakt en beheert de integriteits status van uw hosts. De volgende statussen worden geretourneerd wanneer u een query op uw host uitvoert:

| Status   | Description       |
|----------|----------------|
| Host beschikbaar     | Er zijn geen bekende problemen met de host.   |
| Host wordt onderzocht  | Er zijn enkele problemen met de host die we op zoek zijn. Dit is een overgangs status die vereist is voor Azure om het bereik en de hoofd oorzaak voor het geïdentificeerde probleem te identificeren. Voor virtuele machines die op de host worden uitgevoerd, kan dit gevolgen hebben. |
| Toewijzing in afwachting van host   | Azure kan de host niet herstellen naar een goede status en u vraagt u uw virtuele machines opnieuw te implementeren vanaf deze host. Als `autoHealingOnFailure` is ingeschakeld, worden uw virtuele machines retoucheerd naar goede hardware. Anders kan de virtuele machine worden uitgevoerd op een host die op het punt staat om te mislukken.|
| Toewijzing van host is ongedaan gemaakt  | Alle virtuele machines zijn van de host verwijderd. Er worden geen kosten meer in rekening gebracht voor deze host omdat de hardware niet is gedraaid.   |

