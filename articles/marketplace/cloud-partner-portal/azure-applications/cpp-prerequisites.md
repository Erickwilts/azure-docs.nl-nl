---
title: Vereisten voor Azure Application aanbieding | Azure Marketplace
description: De vereisten voor het publiceren van een Azure-toepassing aanbieden op Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: pabutler
ms.openlocfilehash: 64039234a3863332ca19b915fb59a5271625d695
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66258190"
---
# <a name="azure-application-prerequisites"></a>Vereisten voor Azure-toepassing

Dit artikel beschrijft de technische en zakelijke vereisten voor het publiceren van een beheerder toepassingsbieding op Azure Marketplace.  Wanneer u hebt nog niet gedaan, controleert u de volgende bronnen:
- Afhankelijk van uw SKU-type, ofwel [Azure-toepassingen: Oplossingssjabloon bieden Publicatiehandleiding voor](../../marketplace-solution-templates.md) of [Azure-toepassingen: Beheerder Toepassingsbieding Publicatiehandleiding voor](../../marketplace-managed-apps.md)
- [Het bouwen van Oplossingssjablonen en beheerde toepassingen voor Azure Marketplace](https://channel9.msdn.com/Events/Build/2018/BRK3603) video


## <a name="technical-requirements"></a>Technische vereisten

De technische vereisten zijn onder andere de volgende items:

*   Azure Resource Manager-sjablonen voor meer informatie, Zie [inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates). Dit artikel beschrijft de structuur van een Azure Resource Manager-sjabloon. Deze geeft de verschillende secties van een sjabloon en de eigenschappen die beschikbaar in deze secties zijn. De sjabloon bestaat uit JSON en uitdrukkingen die u gebruiken kunt om waarden voor uw implementatie samen te stellen. 
* Azure Quickstart-sjablonen.<br> Zie voor meer informatie:

  * [Azure-snelstartsjablonen](https://azure.microsoft.com/documentation/templates/). Implementeer Azure-bronnen via Azure Resource Manager met sjablonen die zijn aangeleverd door de community, zodat u meer gedaan krijgt. Met Azure Resource Manager kunt u uw toepassingen inrichten aan de hand van een declaratieve sjabloon. U kunt in één enkele sjabloon meerdere services plus de bijbehorende afhankelijkheden implementeren. U gebruikt dezelfde sjabloon om uw toepassing herhaaldelijk te implementeren in elke fase van de levenscyclus van de toepassing.
  * [GitHub: Azure Resource Manager Quickstart-sjablonen](https://github.com/azure/azure-quickstart-templates). Deze opslagplaats bevat alle beschikbare Azure Resource Manager-sjablonen door de community bijgedragen. Een sjabloon voor doorzoekbare index wordt onderhouden op https://azure.microsoft.com/documentation/templates/.
* UI-definitie maken<br>
Zie voor meer informatie, [maken-Azure portal gebruikersinterface voor uw beheerde toepassing](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview). Dit artikel worden de belangrijkste concepten van het bestand createUiDefinition.json geïntroduceerd. Dit bestand in de Azure-portal wordt gebruikt voor het genereren van de gebruikersinterface voor het maken van een beheerde toepassing.


## <a name="business-requirements"></a>Zakelijke vereisten

De zakelijke vereisten zijn onder andere de volgende procedure, contractuele en wettelijke verplichtingen:

* U moet een geregistreerde Cloud-Marketplace-uitgever. Als u niet bent geregistreerd, volg de stappen in het artikel [geworden van een Cloud-Marketplace-uitgever](https://docs.microsoft.com/azure/marketplace/become-publisher
).

>[!NOTE]
>Het account voor registratie van dezelfde Microsoft Developer Center moet u zich aanmeldt bij de Cloud Partner-Portal. U hebt slechts één Microsoft-account voor uw aanbiedingen op Azure Marketplace. Dit account mag niet zijn specifiek voor afzonderlijke services of aanbiedingen.

* Uw bedrijf (of een dochteronderneming) moet zich in een verkoop-van-land/regio via de Azure Marketplace wordt ondersteund. Zie voor een huidige lijst van deze landen/regio's, [Deelnamebeleid voor Microsoft Azure Marketplace](https://azure.microsoft.com/support/legal/marketplace/participation-policies/).
* Uw product moet op een manier die compatibel is met de factureringsmodellen ondersteund door de Azure Marketplace in licentie worden gegeven. Zie voor meer informatie, [opties voor facturering](https://docs.microsoft.com/azure/marketplace/marketplace-commercial-transaction-capabilities-and-considerations) in de Azure Marketplace.
* U bent verantwoordelijk voor het maken van technische ondersteuning beschikbaar voor klanten in een commercieel redelijke manier. Deze ondersteuning is gratis, betaald, of via de community-benaderingen.
* U bent zelf verantwoordelijk voor licentiëring van uw software en eventuele afhankelijkheden voor software van derden.
* Inhoud die voldoet aan criteria voor uw aanbieding wilt laten vermelden op Azure Marketplace en in Azure portal, moet u opgeven.
* U moet akkoord gaan met de voorwaarden van de Deelnamebeleid voor Microsoft Azure Marketplace en de overeenkomst voor uitgevers.
* U moet voldoen aan de Microsoft Azure Website gebruiksvoorwaarden, de privacyverklaring van Microsoft en de overeenkomst van Microsoft Azure Certified-programma.


## <a name="publishing-requirements"></a>Vereisten voor publiceren

Als u een nieuwe aanbieding voor Azure-toepassing publiceren, moet u voldoen aan de volgende vereisten:

* Uw metagegevens klaar voor gebruik hebben. De volgende (niet volledige) lijst toont een voorbeeld van deze metagegevens:
  * Een titel
  * Een beschrijving (in HTML-indeling)
  * Een logoafbeelding (in PNG-indeling) en in deze vaste grootte: 40, 40 pixels, 90 x 90 pixels, 115 x 115 pixels en 255 x 115 pixels.
* Een *gebruiksvoorwaarden* en een *privacybeleid* documenten
* Documentatie voor Application
* Contactpersonen voor ondersteuning


## <a name="next-steps"></a>Volgende stappen

Als u alle vereisten hebt voldaan, u bent er klaar voor [maken van een Azure-toepassing-aanbieding](./cpp-create-offer.md). 
 
