---
title: Een galerie met gedeelde installatie kopieën configureren in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het configureren van een galerie met gedeelde afbeeldingen in Azure DevTest Labs
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2019
ms.author: spelluru
ms.openlocfilehash: 80610168e0d293b65626da71ee349f25e456576b
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2019
ms.locfileid: "68774572"
---
# <a name="configure-a-shared-image-gallery-in-azure-devtest-labs"></a>Een gedeelde galerie met installatiekopieën configureren in Azure DevTest Labs
DevTest Labs ondersteunt nu de functie [gedeelde installatie kopie galerie](../virtual-machines/windows/shared-image-galleries.md) . Hiermee kunnen labgebruikers toegang krijgen tot installatiekopieën vanaf een gedeelde locatie terwijl ze labresources maken. Het zorgt ook voor structuur en organisatie in uw aangepaste, beheerde VM-installatiekopieën. De functie Gedeelde afbeeldingen galerie ondersteunt:

- Beheerde globale replicatie van installatiekopieën
- Versiebeheer en groepering van installatiekopieën voor eenvoudiger beheer
- Maak uw installatiekopieën optimaal beschikbaar met ZRS-accounts (Zone Redundant Storage) in regio's die beschikbaarheidszones ondersteunen. ZRS biedt een betere veerkracht tegen zonegebonden fouten.
- Installatiekopieën delen binnen abonnementen en zelfs tenants met toegangsbeheer op basis van rollen van Azure (RBAC).

Zie de documentatie van de [Galerie met gedeelde afbeeldingen](../virtual-machines/windows/shared-image-galleries.md)voor meer informatie. 
 
Als u een groot aantal beheerde installatiekopieën hebt dat u moet onderhouden en deze beschikbaar wilt maken binnen uw bedrijf, kunt u een gedeelde installatiekopiegalerie gebruiken als een opslagplaats waar u uw installatiekopieën eenvoudig kunt bijwerken en delen. Als labeigenaar kunt u een bestaande gedeelde installatiekopiegalerie koppelen aan uw lab. Zodra deze galerie is gekoppeld, kunnen labgebruikers machines maken met deze nieuwste installatiekopieën. Een groot voordeel van deze functie is dat DevTest Labs nu kunnen profiteren van het delen van installatiekopieën in labs, in abonnementen en in regio's. 

## <a name="considerations"></a>Overwegingen
- U kunt slechts één gedeelde installatie kopie galerie tegelijk aan een Lab koppelen. Als u een andere galerie wilt koppelen, moet u de bestaande Gallery loskoppelen en er een toevoegen. 
- DevTest Labs biedt momenteel geen ondersteuning voor het uploaden van installatie kopieën naar de galerie via het lab. 
- Bij het maken van een virtuele machine met behulp van een afbeelding in de galerie met gedeelde installatie kopieën, gebruikt DevTest Labs altijd de meest recente gepubliceerde versie van deze installatie kopie.
- Hoewel in DevTest Labs automatisch een beste poging wordt gedaan om ervoor te zorgen dat de galerie met gedeelde afbeeldingen installatie kopieën repliceert naar de regio waarin het lab zich bevindt, is het niet altijd mogelijk. Om te voor komen dat gebruikers problemen hebben met het maken van Vm's van deze installatie kopieën, moet u ervoor zorgen dat de installatie kopieën al worden gerepliceerd naar de regio van het lab.

## <a name="use-azure-portal"></a>Azure Portal gebruiken
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer **alle services** in het navigatie menu links.
1. Selecteer **DevTest Labs** uit de lijst.
1. Selecteer in de lijst met Labs uw **Lab**.
1. Selecteer **configuratie en beleid** in de sectie **instellingen** in het menu links.
1. Selecteer **gedeelde installatie kopie galerieën** onder basis van **virtuele machines** in het menu links.

    ![Menu met galerieën voor gedeelde afbeeldingen](./media/configure-shared-image-gallery/shared-image-galleries-menu.png)
1. Koppel een bestaande galerie met gedeelde afbeeldingen aan uw Lab door te klikken op de knop **koppelen** en de galerie te selecteren in de vervolg keuzelijst.

    ![Koppelen](./media/configure-shared-image-gallery/attach-options.png)
1. Ga naar de gekoppelde galerie en configureer uw galerie om gedeelde installatie kopieën voor het maken van VM'S in **of uit** te scha kelen. Selecteer een galerie met installatie kopieën in de lijst om deze te configureren. 

    Standaard **toestaan dat alle installatie kopieën worden gebruikt als basis van de virtuele machine** is ingesteld op **Ja**. Dit betekent dat alle beschik bare installatie kopieën in de galerie met gedeelde installatie kopieën beschikbaar zijn voor een test gebruiker bij het maken van een nieuwe VM van het lab. Als toegang tot bepaalde installatie kopieën moet worden beperkt, wijzigt u **alle installatie kopieën die moeten worden gebruikt als** basis voorde virtuele machine, en selecteert u de installatie kopieën die u wilt toestaan bij het maken van vm's en selecteert u vervolgens de knop **Opslaan** .

    ![In-of uitschakelen](./media/configure-shared-image-gallery/enable-disable.png)
1. Gebruikers met een Lab kunnen vervolgens een virtuele machine maken met behulp van de ingeschakelde installatie kopieën door op **+ toevoegen** te klikken en de installatie kopie te zoeken in de pagina **Kies uw basis** .

    ![Labgebruikers](./media/configure-shared-image-gallery/lab-users.png)
## <a name="use-azure-resource-manager-template"></a>Azure Resource Manager-sjabloon gebruiken

### <a name="attach-a-shared-image-gallery-to-your-lab"></a>Een galerie met gedeelde afbeeldingen aan uw Lab koppelen
Als u een Azure Resource Manager sjabloon gebruikt om een galerie met gedeelde afbeeldingen aan uw Lab toe te voegen, moet u deze toevoegen onder de sectie resources van uw Resource Manager-sjabloon, zoals wordt weer gegeven in het volgende voor beeld:

```json
"resources": [
{
    "apiVersion": "2018-10-15-preview",
    "type": "Microsoft.DevTestLab/labs",
    "name": "mylab",
    "location": "eastus",
    "resources": [
    {
        "apiVersion":"2018-10-15-preview",
        "name":"myGallery",
        "type":"sharedGalleries",
        "properties": {
            "galleryId":"/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/mySharedGalleryRg/providers/Microsoft.Compute/galleries/mySharedGallery",
            "allowAllImages": "Enabled"
        }
    }
    ]
}
```

Voor een volledig voor beeld van een resource manager-sjabloon raadpleegt u deze resource manager-sjabloon voorbeelden in onze open bare GitHub-opslag plaats: [Een galerie met gedeelde installatie kopieën configureren tijdens het maken van een Lab](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates/101-dtl-create-lab-shared-gallery-configured).

## <a name="use-api"></a>API gebruiken

### <a name="shared-image-galleries---create-or-update"></a>Galerieën met gedeelde afbeeldingen-maken of bijwerken

```rest
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DevTestLab/labs/{labName}/sharedgalleries/{name}?api-version= 2018-10-15-preview
Body: 
{
    "properties":{
        "galleryId": "[Shared Image Gallery resource Id]",
        "allowAllImages": "Enabled"
    }
}

```

### <a name="shared-image-galleries-images---list"></a>Afbeeldingen van de galerie met gedeelde afbeeldingen-lijst 

```rest
GET  https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DevTestLab/labs/{labName}/sharedgalleries/{name}/sharedimages?api-version= 2018-10-15-preview
```




## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende artikelen over het maken van een virtuele machine met behulp van een installatie kopie in de galerie met gekoppelde gedeelde afbeeldingen: [Een virtuele machine maken met behulp van een gedeelde installatie kopie uit de galerie](add-vm-use-shared-image.md)
