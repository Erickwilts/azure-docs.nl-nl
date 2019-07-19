---
title: Werken met grote Azure Virtual Machine Scale Sets | Microsoft Docs
description: Wat u moet weten wanneer u grote Azure-virtuele-machineschaalsets wilt gaan gebruiken
services: virtual-machine-scale-sets
documentationcenter: ''
author: rajsqr
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/9/2017
ms.author: jeconnoc
ms.openlocfilehash: 46ca46c99187b14974b78ccc4acc134a5f716b05
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68326692"
---
# <a name="working-with-large-virtual-machine-scale-sets"></a>Werken met grote virtuele-machineschaalsets
U kunt nu Azure-[virtuele-machineschaalsets](/azure/virtual-machine-scale-sets/) maken met een capaciteit van maximaal 1000 virtuele machines. In dit document wordt een _grote virtuele-machineschaalset_ gedefinieerd als een schaalset waarmee u kunt schalen tot meer dan 100 virtuele machines. Deze mogelijkheid wordt ingesteld met een schaalseteigenschap (_singlePlacementGroup=False_). 

Bepaalde aspecten van grote schaalsets, zoals taakverdeling en foutdomeinen, werken anders dan in een standaardschaalset. In dit document worden de kenmerken van grote schaalsets uitgelegd en wordt beschreven wat u moet weten om ze in uw toepassingen te gebruiken. 

Een algemene aanpak voor de implementatie van een grootschalige cloudinfrastructuur is het maken van een reeks _schaaleenheden_, bijvoorbeeld door meerdere virtuele-machineschaalsets op meerdere VNETs en opslagaccounts te maken. Deze aanpak is eenvoudiger te beheren dan enkelvoudige virtuele machines, en meerdere schaaleenheden zijn nuttig voor veel toepassingen, met name voor toepassingen die ook andere stapelbare onderdelen nodig hebben, zoals meerdere virtuele netwerken en eindpunten. Als uw toepassing echter een groot cluster nodig heeft, is het misschien eenvoudiger om één schaalset van maximaal 1000 virtuele machines te implementeren. Voorbeeldscenario's zijn gecentraliseerde big data-implementaties, of rekenrasters die een eenvoudig beheer van een grote groep werkrolknooppunten vereisen. In combinatie met aan een virtuele-machineschaalset [gekoppelde gegevensschijven](virtual-machine-scale-sets-attached-disks.md) kunt u met grootschalige sets in één bewerking een schaalbare infrastructuur van duizenden vCPU's en petabytes aan opslag implementeren.

## <a name="placement-groups"></a>Plaatsingsgroepen 
Wat een _grote_ schaalset zo bijzonder maakt, is niet het aantal virtuele machines, maar het aantal _plaatsingsgroepen_ dat deze bevat. Een plaatsingsgroep is een constructie die vergelijkbaar is met een Azure-beschikbaarheidsset, met eigen foutdomeinen en upgradedomeinen. Standaard bestaat een schaalset uit één plaatsingsgroep met een omvang van maximaal 100 virtuele machines. Als een schaaleigenschap met de naam _singlePlacementGroup_ is ingesteld op _onwaar_, kan de schaal bestaan uit meerdere plaatsingsgroepen en heeft deze een capaciteit van 0-1000 virtuele machines. Als de eigenschap de standaardwaarde _waar_ heeft, bestaat een schaalset uit één plaatsingsgroep en heeft deze een capaciteit van 0-100 virtuele machines.

## <a name="checklist-for-using-large-scale-sets"></a>Controlelijst voor het gebruik van grote schaalsets
Overweeg de volgende vereisten voordat u beslist of uw toepassing doeltreffend gebruik kan maken van grootschalige sets:

- Als u een groot aantal VM’s wilt implementeren, moeten de quotumlimieten voor uw reken-vCPU’s mogelijk worden verhoogd. 
- Schaalsets die vanuit Azure Marketplace zijn gemaakt, kunnen worden geschaald tot maximaal 1000 virtuele machines.
- Schaalsets die vanuit aangepaste installatiekopieën (VM-installatiekopieën die u zelf maakt en uploadt) zijn gemaakt, kunnen op dit moment worden geschaald tot maximaal 600 VM’s.
- Grote schaalsets vereisen Azure Managed Disks. Voor schaalsets die niet worden gemaakt met Managed Disks zijn meerdere opslagaccounts vereist (één voor elke 20 virtuele machines). Grote schaalsets zijn ontworpen om exclusief met Managed Disks te werken om de overhead voor opslagbeheer te beperken en om het risico te vermijden dat u met abonnementslimieten van opslagaccounts wordt geconfronteerd. 
- Laag-4 taakverdeling met schaalsets die uit meerdere plaatsingsgroepen bestaan vereist [Azure Load Balancer standaard-SKU](../load-balancer/load-balancer-standard-overview.md). De Load Balancer standaard-SKU biedt extra voordelen, zoals de mogelijkheid om taken te verdelen tussen meerdere schaalsets. Standaard-SKU vereist ook dat er een netwerkbeveiligingsgroep is gekoppeld aan de schaalset, anders werken de NAT-pools niet correct. Als u de Azure Load Balancer basis-SKU moet gebruiken, zorgt u dat de schaalset is geconfigureerd voor het gebruik van één plaatsingsgroep. Dit is de standaardinstelling.
- Laag-7 taakverdeling met de Azure Application Gateway wordt voor alle schaalsets ondersteund.
- Een schaalset wordt gedefinieerd met één subnet. Zorg dat het subnet een adresruimte heeft die groot genoeg is voor alle virtuele machines die u nodig hebt. Standaard wordt een schaalset te groot ingericht (dat wil zeggen dat er tijdens de implementatie of bij het uitschalen extra virtuele machines worden gemaakt, waarvoor u niet hoeft te betalen), om de betrouwbaarheid en prestaties van de implementatie te verbeteren. Zorg daarom voor een adresruimte die 20% groter is dan het aantal virtuele machines waarnaar u wilt gaan schalen.
- Fout- en upgradedomeinen zijn alleen consistent binnen een plaatsingsgroep. Deze architectuur verandert niet de algemene beschikbaarheid van een schaalset, omdat virtuele machines evenredig worden verdeeld over verschillende fysieke hardware. Als u moet garanderen dat twee virtuele machines zich op verschillende hardware bevinden, betekent dit echter wel dat u ervoor moet zorgen dat ze zich in verschillende foutdomeinen in dezelfde plaatsingsgroep bevinden. Raadpleeg deze beschik bare [Opties](/azure/virtual-machines/windows/availability)voor de koppeling. 
- Het foutdomein en de id van de plaatsingsgroep worden weergegeven in de _exemplaarweergave_  van een schaalset-VM. U kunt de exemplaarweergave van een schaalset-VM bekijken in de [Azure Resource Explorer](https://resources.azure.com/).

## <a name="creating-a-large-scale-set"></a>Een grote schaalset maken
Als u een schaalset maakt in de Azure-portal, hoeft u alleen maar de waarde bij het *aantal exemplaren* van maximaal 1.000 in te vullen. Als het om meer dan 100 exemplaren gaat, wordt *Schalen naar meer dan 100 exemplaren inschakelen* ingesteld op *Ja*. Hierdoor kan de schaal worden aangepast naar meerdere plaatsingsgroepen. 

![](./media/virtual-machine-scale-sets-placement-groups/portal-large-scale.png)

U kunt u een grote virtuele-machineschaalset maken met de [Azure CLI](https://github.com/Azure/azure-cli)-opdracht _az vmss create_. Met deze opdracht stelt u intelligente standaardinstellingen in, zoals een subnetgrootte op basis van het argument _instance-count_:

```bash
az group create -l southcentralus -n biginfra
az vmss create -g biginfra -n bigvmss --image ubuntults --instance-count 1000
```
De opdracht _vmss create_ gebruikt standaard bepaalde configuratiewaarden als u ze niet opgeeft. Probeer het volgende om de beschikbare opties te bekijken die u kunt overschrijven:
```bash
az vmss create --help
```

Als u een grote schaalset maakt door een Azure Resource Manager-sjabloon te maken, zorg dan dat de sjabloon een schaalset maakt op basis van Azure Managed Disks. U kunt de eigenschap _singlePlacementGroup_ in de sectie _Properties_ van de resource _Microsoft.Compute/virtualMAchineScaleSets_ instellen op _false_. In het volgende JSON-fragment ziet u het begin van een schaalsetsjabloon, met een capaciteit voor 1000 virtuele machines en de instelling _"singlePlacementGroup": false_:
```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "location": "australiaeast",
  "name": "bigvmss",
  "sku": {
    "name": "Standard_DS1_v2",
    "tier": "Standard",
    "capacity": 1000
  },
  "properties": {
    "singlePlacementGroup": false,
    "upgradePolicy": {
      "mode": "Automatic"
    }
```
Raadpleeg [https://github.com/gbowerman/azure-myriad/blob/master/bigtest/bigbottle.json](https://github.com/gbowerman/azure-myriad/blob/master/bigtest/bigbottle.json) voor een compleet voorbeeld van een grote schaalset-sjabloon.

## <a name="converting-an-existing-scale-set-to-span-multiple-placement-groups"></a>Een bestaande schaalset converteren zodat deze meerdere plaatsingsgroepen omvat
Als u een bestaande virtuele-machineschaalset geschikt wilt maken voor schaling naar meer dan 100 virtuele machines, moet u de eigenschap _singlePlacementGroup_ in het schaalsetmodel wijzigen naar _false_. Met de [Azure Resource Explorer](https://resources.azure.com/) kunt u controleren of deze eigenschap is gewijzigd. Zoek een bestaande schaalset, selecteer _Edit_ (Bewerken) en wijzig de eigenschap _singlePlacementGroup_. Als u deze eigenschap niet ziet, bekijkt u de schaalset misschien met een oudere versie van de Microsoft.Compute-API.

> [!NOTE]
> U kunt een schaalset zo wijzigen dat deze in plaats van slechts één plaatsingsgroep (de standaardinstelling) ondersteuning biedt voor meerdere plaatsingsgroepen. Andersom is echter niet mogelijk. Zorg daarom dat u begrijpt waarvoor de eigenschappen van grote schaalsets dienen, voordat u deze converteert.


