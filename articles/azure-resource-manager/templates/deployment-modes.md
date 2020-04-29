---
title: Implementatiemodi
description: Hierin wordt beschreven hoe u kunt opgeven of u een volledige of incrementele implementatie modus met Azure Resource Manager wilt gebruiken.
ms.topic: conceptual
ms.date: 01/17/2020
ms.openlocfilehash: 1077d92f076797fb03c4fe750b353e2306f9b6de
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "79460242"
---
# <a name="azure-resource-manager-deployment-modes"></a>Implementatie modi Azure Resource Manager

Wanneer u uw resources implementeert, geeft u op dat de implementatie een incrementele update of een volledige update is. Het verschil tussen deze twee modi is hoe Resource Manager bestaande resources in de resource groep die zich niet in de sjabloon bevinden, afhandelt.

Voor beide modi probeert Resource Manager alle resources te maken die zijn opgegeven in de sjabloon. Als de resource al bestaat in de resource groep en de bijbehorende instellingen niet worden gewijzigd, wordt er geen bewerking voor die resource uitgevoerd. Als u de eigenschaps waarden voor een resource wijzigt, wordt de resource bijgewerkt met de nieuwe waarden. Als u de locatie of het type van een bestaande resource probeert bij te werken, mislukt de implementatie met een fout. Implementeer in plaats daarvan een nieuwe resource met de locatie of het type dat u nodig hebt.

De standaard modus is incrementeel.

## <a name="complete-mode"></a>Volledige modus

In de volledige modus **verwijdert** Resource Manager resources die voor komen in de resource groep, maar die niet zijn opgegeven in de sjabloon.

Als uw sjabloon een resource bevat die niet is geïmplementeerd omdat de [voor waarde](conditional-resource-deployment.md) wordt geëvalueerd als onwaar, is het resultaat afhankelijk van de rest API versie die u gebruikt om de sjabloon te implementeren. Als u een eerdere versie dan 2019-05-10 gebruikt, wordt de resource **niet verwijderd**. Met 2019-05-10 of hoger wordt de resource **verwijderd**. Met de nieuwste versies van Azure PowerShell en Azure CLI verwijdert u de resource.

Wees voorzichtig met het gebruik van de volledige modus met [Kopieer lussen](copy-resources.md). Alle resources die niet zijn opgegeven in de sjabloon na het oplossen van de Kopieer-lus, worden verwijderd.

Als u naar [meer dan één resource groep in een sjabloon](cross-resource-group-deployment.md)implementeert, kunnen resources in de resource groep die zijn opgegeven in de implementatie bewerking worden verwijderd. Resources in de secundaire resource groepen worden niet verwijderd.

Er zijn een aantal verschillen in de manier waarop bron typen het verwijderen van de modus volt ooien verwerken. Bovenliggende resources worden automatisch verwijderd als ze niet worden gebruikt in een sjabloon die wordt geïmplementeerd in de modus voltooid. Sommige onderliggende resources worden niet automatisch verwijderd wanneer ze niet in de sjabloon staan. Deze onderliggende resources worden echter verwijderd als de bovenliggende resource wordt verwijderd.

Als uw resource groep bijvoorbeeld een DNS-zone (het resource type micro soft. Network/dnsZones) en een CNAME-record (micro soft. Network/dnsZones/CNAME-resource type) bevat, is de DNS-zone de bovenliggende resource voor de CNAME-record. Als u implementeert in de modus volledig en u de DNS-zone niet in uw sjabloon opneemt, worden de DNS-zone en de CNAME-record verwijderd. Als u de DNS-zone in uw sjabloon opneemt, maar niet de CNAME-record opneemt, wordt de CNAME niet verwijderd.

Zie [Azure-resources verwijderen voor implementaties in de volledige modus](complete-mode-deletion.md)voor een lijst met de verwijdering van resource typen die worden verwijderd.

Als de resource groep is [vergrendeld](../management/lock-resources.md), worden de resources niet verwijderd in de modus volt ooien.

> [!NOTE]
> Alleen hoofd niveau sjablonen ondersteunen de volledige implementatie modus. Voor [gekoppelde of geneste sjablonen](linked-templates.md)moet u de incrementele modus gebruiken.
>
> [Implementaties op abonnements niveau](deploy-to-subscription.md) bieden geen ondersteuning voor de modus volt ooien.
>
> De portal biedt momenteel geen ondersteuning voor de volledige modus.
>

## <a name="incremental-mode"></a>Incrementele modus

In de incrementele modus blijven Resource Manager **ongewijzigde** resources die voor komen in de resource groep, maar niet in de sjabloon opgegeven. Resources in de sjabloon **worden toegevoegd** aan de resource groep.

> [!NOTE]
> Wanneer een bestaande resource opnieuw wordt geïmplementeerd in de incrementele modus, worden alle eigenschappen opnieuw toegepast. De **eigenschappen zijn niet incrementeel toegevoegd**. Een veelvoorkomende misverstand is om te denken dat eigenschappen die niet zijn opgegeven in de sjabloon, ongewijzigd blijven. Als u bepaalde eigenschappen niet opgeeft, interpreteert Resource Manager de implementatie als het overschrijven van die waarden. Eigenschappen die niet in de sjabloon zijn opgenomen, worden opnieuw ingesteld op de standaard waarden. Geef alle niet-standaard waarden voor de resource op, niet alleen voor de resources die u wilt bijwerken. De resource definitie in de sjabloon bevat altijd de laatste status van de resource. Het kan geen gedeeltelijke update voor een bestaande resource vertegenwoordigen.

## <a name="example-result"></a>Voorbeeld resultaat

Houd rekening met het volgende scenario om het verschil tussen de modus incrementeel en volledig te illustreren.

**Resource groep** bevat:

* Resource A
* Resource B
* Resource C

**Sjabloon** bevat:

* Resource A
* Resource B
* Resource D

Wanneer de resource groep wordt geïmplementeerd in de **incrementele** modus, heeft:

* Resource A
* Resource B
* Resource C
* Resource D

Wanneer de implementatie is geïmplementeerd in de modus **voltooid** , wordt bron C verwijderd. De resource groep heeft:

* Resource A
* Resource B
* Resource D

## <a name="set-deployment-mode"></a>Implementatie modus instellen

Als u de implementatie modus wilt instellen bij het implementeren met Power `Mode` shell, gebruikt u de para meter.

```azurepowershell-interactive
New-AzResourceGroupDeployment `
  -Mode Complete `
  -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json
```

Als u de implementatie modus wilt instellen wanneer u met Azure CLI implementeert, gebruikt u de `mode` para meter.

```azurecli-interactive
az deployment group create \
  --name ExampleDeployment \
  --mode Complete \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters storageAccountType=Standard_GRS
```

In het volgende voor beeld ziet u een gekoppelde sjabloon die is ingesteld op incrementele implementatie modus:

```json
"resources": [
  {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "properties": {
          "mode": "Incremental",
          <nested-template-or-external-template>
      }
  }
]
```

## <a name="next-steps"></a>Volgende stappen

* Zie [Azure Resource Manager sjablonen ontwerpen](template-syntax.md)voor meer informatie over het maken van Resource Manager-sjablonen.
* Zie [een toepassing implementeren met Azure Resource Manager sjabloon](deploy-powershell.md)voor meer informatie over het implementeren van resources.
* Zie [Azure rest API](/rest/api/)om de bewerkingen voor een resource provider weer te geven.
