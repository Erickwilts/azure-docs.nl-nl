---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: f6bd574c83d309ce6d6f54fdb1c7d23cb713420d
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2019
ms.locfileid: "73182201"
---
## <a name="tagging-a-virtual-machine-through-templates"></a>Een virtuele machine labelen via sjablonen
Eerst gaan we Tags bekijken via sjablonen. Met [deze sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) worden tags op de volgende resources geplaatst: Compute (virtuele machine), opslag (opslag account) en netwerk (openbaar IP-adres, Virtual Network, en netwerk interface). Deze sjabloon is voor een Windows-VM, maar kan worden aangepast voor Linux-Vm's.

Klik op de knop **implementeren naar Azure** van de [sjabloon koppeling](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags). Hiermee gaat u naar de [Azure Portal](https://portal.azure.com/) waar u deze sjabloon kunt implementeren.

![Eenvoudige implementatie met Tags](./media/virtual-machines-common-tag/deploy-to-azure-tags.png)

Deze sjabloon bevat de volgende Tags: *afdeling*, *toepassing*en *gemaakt door*. U kunt deze Tags rechtstreeks in de sjabloon toevoegen/bewerken als u andere label namen wilt.

![Azure Tags in een sjabloon](./media/virtual-machines-common-tag/azure-tags-in-a-template.png)

Zoals u kunt zien, worden de tags gedefinieerd als sleutel-waardeparen, gescheiden door een dubbele punt (:). De labels moeten worden gedefinieerd in deze indeling:

        "tags": {
            "Key1" : "Value1",
            "Key2" : "Value2"
        }

Sla het sjabloon bestand op nadat u klaar bent met het bewerken van de gewenste labels.

Vervolgens kunt u in het gedeelte **para meters bewerken** de waarden voor uw Tags invullen.

![Tags bewerken in Azure Portal](./media/virtual-machines-common-tag/edit-tags-in-azure-portal.png)

Klik op **maken** om deze sjabloon te implementeren met de label waarden.

## <a name="tagging-through-the-portal"></a>Tags toevoegen via de portal
Nadat u uw resources met tags hebt gemaakt, kunt u tags in de portal weer geven, toevoegen en verwijderen.

Selecteer het pictogram Tags om uw tags weer te geven:

![Pictogram labels in Azure Portal](./media/virtual-machines-common-tag/azure-portal-tags-icon.png)

Voeg een nieuwe tag toe via de portal door uw eigen sleutel/waarde-paar te definiëren en op te slaan.

![Nieuwe tag toevoegen in Azure Portal](./media/virtual-machines-common-tag/azure-portal-add-new-tag.png)

Het nieuwe label wordt nu weer gegeven in de lijst met tags voor uw resource.

![Nieuwe tag opgeslagen in Azure Portal](./media/virtual-machines-common-tag/azure-portal-saved-new-tag.png)

