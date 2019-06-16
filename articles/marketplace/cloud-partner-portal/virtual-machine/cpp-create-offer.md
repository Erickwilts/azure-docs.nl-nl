---
title: Virtuele Machine-aanbieding maken in Azure Marketplace
description: Een lijst met de stappen die nodig zijn om te maken van een nieuwe virtuele machine (VM) bieden voor Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: article
ms.date: 10/19/2018
ms.author: pabutler
ms.openlocfilehash: 4cd635c6f664a5260b79e62ea72bbb86fc4e1e4f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64938360"
---
# <a name="create-virtual-machine-offer"></a>Virtuele Machine-aanbieding maken

In deze sectie bevat de stappen die zijn vereist voor het maken van een nieuwe aanvraag van de virtuele machine (VM)-aanbieding voor Azure Marketplace.  Elke aanbieding wordt weergegeven als een eigen entiteit in Azure Marketplace en is gekoppeld aan een of meer SKU's.  Een VM-aanbieding bestaat uit de volgende groeperingen van assets en ondersteunende services: 

![Assets voor een VM-aanbieding](./media/publishvm_002.png)

Waar:

|  **Activa-groep**   |  **Beschrijving**  |
|  ---------------   |  ---------------  |
|    SKU 's            |  De kleinste die kunnen worden gekocht eenheid van een aanbieding. Één aanbieding (productklasse) kan meerdere SKU's die zijn gekoppeld aan het onderscheid maken tussen de ondersteunde functies, typen VM-installatiekopieën en factureringsmodellen hebben. |
|  Marketplace       | Marketing, juridische en potentiële klanten beheer activa en specificaties bevat.  <ul><li> Marketing activa omvatten aanbiedingsnaam, beschrijving en logo 's</li> <li> Juridische activa omvatten een privacybeleid, gebruiksvoorwaarden en andere juridische documentatie</li>  <li> Potentiële klanten beleid kunt u opgeven hoe worden verwerkt via de portal van Azure Marketplace door eindgebruikers leidt.</li> </ul> |
| Ondersteuning            | Bevat informatie over ondersteuning en neem contact op met het beleid |
| Test Drive         | Definieert de activa die eindgebruikers kunnen om te testen van uw aanbieding voordat ze kopen |
|  |  |


## <a name="new-offer-form"></a>Nieuwe aanbieding formulier

Zodra uw Meld u aan bij de [Cloud Partner-Portal](https://cloudpartner.azure.com/), klikt u op de **+ nieuwe aanbieding** item op de menubalk linksboven. Klik in het menu aan de resulterende op **virtuele Machines** om weer te geven de **nieuwe aanbieding** vormen en start het proces van het definiëren van assets voor een nieuwe VM-aanbieding. 
<!-- not all publishers see corevm or azure apps test, you need to be whitelisted to see them. we should hide those in these images. -->

![Nieuwe virtuele machine-aanbieding interface gebruikersselectie](./media/publishvm_003.png)

> [!WARNING]
> Als de **virtuele Machines** optie wordt niet weergegeven of niet is ingeschakeld, wordt uw account is niet gemachtigd om dit aanbiedingtype te maken.  Controleer of dat u voldoet aan alle de [vereisten](./cpp-prerequisites.md) voor dit aanbiedingtype, met inbegrip van registreren voor een developer-account.


## <a name="next-steps"></a>Volgende stappen

De volgende onderwerpen in deze sectie mirror van de tabbladen in de **nieuwe aanbieding** pagina (voor een VM-aanbieding-type).  Elk artikel wordt uitgelegd hoe u het bijbehorende tabblad gebruiken om te definiëren van de asset groepen en de ondersteunende services voor uw nieuwe virtuele machine-aanbieding.

- [Tabblad voor aanbiedingsinstellingen](./cpp-offer-settings-tab.md)
- [Tabblad voor SKU's](./cpp-skus-tab.md)
- [Tabblad voor Test Drive](./cpp-test-drive-tab.md)
- [Tabblad voor Marketplace](./cpp-marketplace-tab.md)
- [Tabblad voor ondersteuning](./cpp-support-tab.md)
