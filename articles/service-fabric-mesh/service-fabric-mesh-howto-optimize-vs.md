---
title: Visual Studio voor Azure Service Fabric-net optimaliseren
description: In dit artikel ziet u hoe u de prestaties van Visual Studio kunt optimaliseren voor Service Fabric Mesh-projecten, zodat de eerste keer dat u foutopsporing (F5) uitvoert veel sneller verloopt.
author: georgewallace
ms.author: gwallace
ms.date: 11/29/2018
ms.topic: conceptual
ms.openlocfilehash: aa7a959128d3bcdfcce67d3abeac245975339a9f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91840299"
---
# <a name="optimize-visual-studio-performance-for-service-fabric-mesh-projects"></a>Prestaties van Visual Studio optimaliseren voor Service Fabric Mesh-projecten

In dit artikel ziet u hoe u de prestaties van Visual Studio kunt optimaliseren voor Service Fabric Mesh-projecten, zodat de eerste keer dat u foutopsporing (F5) uitvoert veel sneller verloopt.  

## <a name="change-visual-studio-settings"></a>Visual Studio-instellingen wijzigen
 
In Visual Studio kunt u onder **extra**  >  **Opties**   >  **service Fabric net-hulpprogram ma's**  >  **Algemeen**de volgende instellingen aanpassen:

- Met **Pull required Docker images on project open** (Vereiste Docker-installatiekopieën ophalen bij openen van project) verloopt uw eerste foutopsporing (F5) sneller, omdat het downloaden van de installatiekopie wordt gestart tijdens het laden van het project.  
- Met **Deploy application on project open** (Toepassing implementeren bij openen van project) kan uw eerste foutopsporing (F5) sneller verlopen, omdat het implementeren wordt gestart zodra het project is geopend.  
- Met **Remove application on project close** (Toepassing verwijderen bij sluiten van project) worden resources (CPU, RAM) die zijn toegewezen aan de app, vrijgemaakt door de Mesh-app te verwijderen wanneer het project wordt gesloten.  

Als u in het uitvoervenster van Service Fabric berichten over Visual Studio ziet met daarin termen als 'pulling images', 'warming up' of 'removing application', heeft dit te maken met bovenstaande instellingen. U kunt deze instellingen uitschakelen.

## <a name="next-steps"></a>Volgende stappen

Lees de [zelfstudie Fouten opsporen in een Mesh-app](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)