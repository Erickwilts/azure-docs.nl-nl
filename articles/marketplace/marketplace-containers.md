---
title: Containers bieden handleiding voor Azure Marketplace publiceren
description: In dit artikel beschrijft de vereisten voor het publiceren van Containers in de Marketplace
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
author: ellacroi
manager: nunoc
ms.service: marketplace
ms.topic: article
ms.date: 07/09/2018
ms.author: ellacroi
ms.openlocfilehash: 41a09be36262ff09c383b8ccb64a94230a11d3f1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64937918"
---
# <a name="containers-offer-publishing-guide"></a>Containers bieden Publicatiehandleiding voor

Container-aanbiedingen te publiceren om de containerinstallatiekopie naar de Azure Marketplace. Deze handleiding gebruiken om informatie over de systeemvereisten voor deze aanbieding. 

Dit zijn de transactie-aanbiedingen die zijn geïmplementeerd en worden gefactureerd via de Marketplace. De aanroep naar de actie die door een gebruiker is "Nu downloaden."

Het type van de aanbieding Container gebruiken als uw oplossing een Docker-containerinstallatiekopie ingericht als een Azure container op basis van een Kubernetes-service is.

>[!NOTE]
>Bijvoorbeeld, zoals een Azure container op basis van een Kubernetes-service Azure Kubernetes Service of Azure Container Instances, de keuze van de Azure-klanten voor een container op basis van een Kubernetes-runtime.  

Microsoft biedt momenteel ondersteuning voor gratis en bring-your-own-license (BYOL)-licentiemodellen.

## <a name="containers-offer"></a>Aanbieding voor containers

| Vereiste | Details |  
|:--- |:--- |  
| Facturering en betaling naar gebruik | Ondersteuning voor een van beide de gratis of BYOL-model voor facturering. |  
| Afbeelding die zijn gebouwd op basis van docker-bestand | Containerinstallatiekopieën moeten worden gebaseerd op de Docker-installatiekopie-specificatie en moeten worden gemaakt van een docker-bestand.<ul> <li>Voor meer informatie over het bouwen van docker-installatiekopieën, gaat u naar het gedeelte van het gebruik dat zich bevindt in [docs.docker.com/engine/reference/builder/#usage](https://docs.docker.com/engine/reference/builder/#usage).</li> </ul> |  
| Hosting in ACR | Containerinstallatiekopieën moeten worden gehost in een opslagplaats voor Azure Container Registry (ACR).<ul> <li>Voor meer informatie over het werken met ACR, gaat u naar de Quick Start: Een containerregister maken met de Azure portal-pagina te vinden op [docs.microsoft.com/azure/container-registry/container-registry-get-started-portal](https://docs.microsoft.com/azure/container-registry/container-registry-get-started-portal).</li> </ul> |  
| Afbeeldingen taggen | Containerinstallatiekopieën moeten ten minste 1 tag bevatten (maximum van labels: 16).<ul> <li>Voor meer informatie over het taggen van een afbeelding, gaat u naar de docker-tag-pagina te vinden op [docs.docker.com/engine/reference/commandline/tag](https://docs.docker.com/engine/reference/commandline/tag).</li> </ul> |  

## <a name="next-steps"></a>Volgende stappen

Als u dit nog niet hebt gedaan, 

- [Registreren](https://azuremarketplace.microsoft.com/sell) in de marketplace.

Als u bent geregistreerd en het maken van een nieuwe aanbieding of werken aan een bestaande resourcegroep

- [Meld u aan bij de Cloud Partner-Portal](https://cloudpartner.azure.com) maken of uitvoeren van uw aanbieding.
- Zie [Containers](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/containers/cpp-containers-offer) voor meer informatie.
