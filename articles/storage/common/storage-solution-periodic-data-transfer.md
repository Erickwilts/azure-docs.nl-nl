---
title: Kies een Azure-oplossing voor de overdracht van periodieke | Microsoft Docs
description: Informatie over het kiezen van een Azure-oplossing voor gegevensoverdracht wanneer periodiek van gegevens overbrengen.
services: storage
author: alkohli
ms.service: storage
ms.subservice: blobs
ms.topic: article
ms.date: 06/24/2019
ms.author: alkohli
ms.openlocfilehash: fb49802adf6242f445b700d06622d7e6aa336b4d
ms.sourcegitcommit: a7ea412ca4411fc28431cbe7d2cc399900267585
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2019
ms.locfileid: "67357041"
---
# <a name="solutions-for-periodic-data-transfer"></a>Oplossingen voor de overdracht van periodieke
 
In dit artikel bevat een overzicht van de gegevensoverdracht oplossingen bij periodiek van gegevens overbrengen. Periodieke gegevensoverdracht via het netwerk kan worden gecategoriseerd als terugkerende met regelmatige intervallen of continue gegevensverplaatsing. Het artikel beschrijft ook de aanbevolen data transfer-opties en de bijbehorende sleutel mogelijkhedenmatrix voor dit scenario.

Voor een overzicht van alle beschikbare overdracht Gegevensopties, gaat u naar [een Azure data transfer-oplossing kiezen](storage-choose-data-transfer-solution.md).

## <a name="recommended-options"></a>Aanbevolen opties

De aanbevolen opties voor de overdracht van periodieke kunnen worden onderverdeeld in twee categorieën, afhankelijk van de overdracht terugkerende of continue.

- **Hulpprogramma's voor een script vastgelegd/programmatische** – voor het overdragen van gegevens die wordt uitgevoerd op regelmatige intervallen, scripts en programmatische hulpprogramma's zoals AzCopy en Azure Storage REST-API's. Deze hulpprogramma's zijn gericht op IT-professionals en ontwikkelaars.

    - **AzCopy** : Gebruik deze opdrachtregel-hulpprogramma eenvoudig om gegevens te kopiëren naar en van Azure-Blobs, bestanden, en Table storage met optimale prestaties. AzCopy biedt ondersteuning voor gelijktijdigheid van taken en parallelle uitvoering en de mogelijkheid om te hervatten kopieerbewerkingen wanneer onderbroken.
    - **Azure Storage REST API's / SDK's** – bij het bouwen van een toepassing, kunt u de toepassing op basis van Azure Storage REST-API's ontwikkelen en gebruiken van de Azure-SDK's die worden aangeboden in meerdere talen. De REST API's kunnen ook gebruikmaken van de Azure Storage-bibliotheek voor gegevensverplaatsing speciaal ontworpen voor het high-performance kopiëren van gegevens naar en van Azure.

- **Continue gegevensopnamehulpprogramma's** – voor continue, continue gegevensopname, kunt u een van de Data Box-apparaat voor online overdracht of Azure Data Factory. Deze hulpprogramma's zijn ingesteld door IT-professionals en transparant gegevensoverdracht kunnen automatiseren.

    - **Azure Data Factory** : Data Factory moet worden gebruikt voor het opschalen van een overdrachtbewerking van, en indien nodig is voor de orchestration- en enterprise uitvoeren voor de mogelijkheden voor bewaking. Azure Data Factory gebruiken voor het instellen van een cloud-pijplijn die regelmatig bestanden tussen verschillende Azure-services, on-premises of een combinatie van beide overbrengt. Azure Data Factory kunt die u gegevensgestuurde werkstromen die gegevens uit verschillende gegevensarchieven opnemen en automatiseren gegevensverplaatsingen en gegevenstransformaties indelen.
    - **Azure Data Box-familie voor online-overdrachten** -gegevens in het Edge- en Data Box-Gateway zijn online netwerkapparaten die gegevens van en naar Azure verplaatsen kunnen. Kunstmatige intelligentie (AI) maakt gebruik van gegevens in het Edge-Edge Computing vooraf om gegevens te verwerken voordat uploaden ingeschakeld. Data Box-Gateway is een virtuele versie van het apparaat met de dezelfde mogelijkheden voor de overdracht van gegevens.


## <a name="comparison-of-key-capabilities"></a>Vergelijking van de belangrijkste mogelijkheden

De volgende tabel geeft een overzicht van de verschillen in de belangrijkste mogelijkheden.

### <a name="scriptedprogrammatic-network-data-transfer"></a>Gegevensoverdracht in een script vastgelegd/Programmatic netwerk

| Mogelijkheid                  | AzCopy                                 | Azure Storage REST APIs       |
|-----------------------------|----------------------------------------|-------------------------------|
| Vormfactor                 | Opdrachtregel-hulpprogramma van Microsoft       | Klanten ontwikkelen met betrekking tot opslag <br> REST-API's met behulp van Azure-clientbibliotheken |
| Eerste eenmalige configuratie     | Minimaal                                | Gemiddeld, variabele-ontwikkeling    |
| Gegevensindeling                 | Azure-Blobs, Azure Files, Azure-tabellen | Azure-Blobs, Azure Files, Azure-tabellen   |
| Prestaties                 | Al geoptimaliseerd                      | Tijdens het ontwikkelen van optimaliseren                  |
| Prijzen                     | Gratis, kosten voor uitgaand verkeer toepassen      | Gratis, kosten voor uitgaand verkeer toepassen        |

### <a name="continuous-data-ingestion-over-network"></a>Continue gegevensopname via netwerk

| Functie                                       | Data Box Gateway | Data Box Edge   | Azure Data Factory        |
|----------------------------------|-----------------------------------------|--------------------------|---------------------------|
| Vormfactor                                   | Virtueel apparaat             | Fysiek apparaat          | -Service in Azure portal, agent on-premises                                                            |
| Hardware                                      | De hypervisor            | Geleverd door Microsoft    | N.V.T.                                                            |
| Eerste configuratie inspanning                          | Laag (< 30 minuten.)            | Gemiddeld (~ Combineer uur) | Large (~days)                                                 |
| Gegevensindeling                                   | Azure-Blobs, Azure Files   | Azure-Blobs, Azure Files | [Biedt ondersteuning voor 70 gegevensconnectors voor data-archieven en indelingen](https://docs.microsoft.com/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats)|
| Gegevens vooraf verwerken                           | Nee                         | Ja, via Edge-Computing    | Ja                                                           |
| Lokale cache<br>(voor het opslaan van on-premises gegevens)    | Ja                        | Ja                      | Nee                                                            |
| Overdracht uit andere clouds                    | Nee                         | Nee                       | Ja                                                           |
| Prijzen                                       | [Prijzen](https://azure.microsoft.com/pricing/details/storage/databox/gateway/)                    | [Prijzen](https://azure.microsoft.com/pricing/details/storage/databox/edge/)                  | [Prijzen](https://azure.microsoft.com/pricing/details/data-factory/)                                                       |

## <a name="next-steps"></a>Volgende stappen

- [Gegevens overdragen met AzCopy](/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json).
- [Meer informatie over gegevens overdragen met Storage REST-API's](https://docs.microsoft.com/dotnet/api/overview/azure/storage?view=azure-dotnet).
- Begrijpen hoe:
    - [Gegevens overdragen met Data Box Gateway](https://docs.microsoft.com/azure/databox-online/data-box-gateway-deploy-add-shares).
    - [Gegevens transformeren met gegevens in Edge voordat ze worden verzonden naar Azure](https://docs.microsoft.com/azure/databox-online/data-box-edge-deploy-configure-compute).
- [Meer informatie over het overbrengen van gegevens met Azure Data Factory](https://docs.microsoft.com/azure/data-factory/tutorial-bulk-copy-portal).
