---
title: DevTest Labs gebruiken in azure DevOps-pijp lijnen bouwen en vrijgeven | Microsoft Docs
description: Meer informatie over het gebruik van Azure DevTest Labs in azure DevOps build-en release-pijp lijnen.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2019
ms.author: spelluru
ms.openlocfilehash: 606563cd7d7adcdd79bf9561876eb0640fb68b21
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68620894"
---
# <a name="use-devtest-labs-in-azure-devops-build-and-release-pipelines"></a>DevTest Labs gebruiken in Azure DevOps om pijplijnen te bouwen en uit te brengen
Dit artikel bevat informatie over de manier waarop DevTest Labs kan worden gebruikt in azure DevOps-pijp lijnen voor Build en release. 

## <a name="overall-flow"></a>Algehele stroom
De basis stroom is een build- **pijp lijn** die de volgende taken doet:

1. Bouw de toepassings code op.
1. Maak de basis omgeving in DevTest Labs.
1. Werk de omgeving bij met aangepaste informatie.
1. De toepassing implementeren in de DevTest Labs-omgeving
1. Test de code. 

Zodra de build is voltooid, worden de build-artefacten door de **release pijplijn** gebruikt voor het implementeren van staging of productie. 

Een van de nodige lokalen is dat alle informatie die nodig is voor het opnieuw maken van het geteste ecosysteem beschikbaar is binnen de bouw artefacten, inclusief de configuratie van de Azure-resources. Als Azure-resources kosten in rekening brengen, willen bedrijven het gebruik van deze resources zelf kunnen beheren of bijhouden. In sommige gevallen kunnen Azure Resource Manager sjablonen die worden gebruikt om de resources te maken en te configureren, worden beheerd door een andere afdeling zoals het. Deze sjablonen kunnen worden opgeslagen in een andere opslag plaats. Het leidt naar een interessante situatie waarin een build wordt gemaakt en getest, en zowel de code als de configuratie moeten worden opgeslagen in de constructie artefacten om het systeem op de juiste manier opnieuw in productie te maken. 

Wanneer u DevTest Labs gebruikt tijdens de build/test-fase, kunt u Azure Resource Manager sjablonen en ondersteunende bestanden toevoegen aan de build-bronnen, zodat tijdens de release fase de exacte configuratie die wordt gebruikt om te testen met wordt geïmplementeerd voor productie. Met de taak **Azure DevTest Labs omgeving maken** met de juiste configuratie worden de Resource Manager-sjablonen in de build-artefacten opgeslagen. In dit voor beeld gebruikt u de code uit de [zelf studie: Bouw een .net core-en SQL database-web-](../app-service/app-service-web-tutorial-dotnetcore-sqldb.md)app in azure app service om de web-app in azure te implementeren en te testen.

![Algehele stroom](./media/use-devtest-labs-build-release-pipelines/overall-flow.png)

## <a name="set-up-azure-resources"></a>Azure-resources instellen
Er zijn een aantal items die moeten worden gemaakt:

- Twee opslag plaatsen. Het eerste met de code uit de zelf studie en een resource manager-sjabloon met twee extra Vm's. De tweede bevat de basis Azure Resource Manager sjabloon (bestaande configuratie).
- Een resource groep voor de implementatie van de productie code en configuratie.
- Een lab moet worden ingesteld met een verbinding met [de configuratie opslagplaats](devtest-lab-create-environment-from-arm.md) voor de build-pijp lijn. De Resource Manager-sjabloon moet worden gecontroleerd in de configuratie opslagplaats als azuredeploy. json met de meta gegevens. json zodat DevTest Labs de sjabloon kan herkennen en implementeren.

Met de build-pijp lijn wordt een DevTest Labs-omgeving gemaakt en de code voor het testen geïmplementeerd.

## <a name="set-up-a-build-pipeline"></a>Een build-pijp lijn instellen
Maak in azure DevOps een build-pijp lijn met behulp van de [code in de zelf studie: Een .NET core-en SQL Database-web-app](../app-service/app-service-web-tutorial-dotnetcore-sqldb.md)maken in azure app service. Gebruik de **ASP.net core** sjabloon, waarmee de benodigde taak wordt gevuld om de code te bouwen, te testen en te publiceren.

![De ASP.NET-sjabloon selecteren](./media/use-devtest-labs-build-release-pipelines/select-asp-net.png)

U moet drie extra taken toevoegen om de omgeving in DevTest Labs te maken en te implementeren in de omgeving.

![Pijp lijn met drie taken](./media/use-devtest-labs-build-release-pipelines/pipeline-tasks.png)

### <a name="create-environment-task"></a>Omgevings taak maken
Gebruik in de taak omgeving maken (**Azure DevTest Labs omgeving maken** ) de vervolg keuzelijsten om de volgende waarden te selecteren:

- Azure-abonnement
- naam van het lab
- de naam van de opslag plaats
- de naam van de sjabloon (waarin de map wordt weer gegeven waarin de omgeving is opgeslagen). 

We raden u aan vervolg keuzelijsten op de pagina te gebruiken in plaats van de gegevens hand matig in te voeren. Als u de informatie hand matig invoert, voert u volledig gekwalificeerde Azure-resource-Id's in. In de taak worden de beschrijvende namen weer gegeven in plaats van resource-Id's. 

De naam van de omgeving is de weer gegeven naam die wordt weer gegeven in DevTest Labs. Dit moet een unieke naam zijn voor elke build. Bijvoorbeeld: **TestEnv $ (build. BuildId)** . 

U kunt para meters bestand of para meters opgeven voor het door geven van gegevens in de Resource Manager-sjabloon. 

Selecteer de **uitvoer variabelen maken op basis van de optie voor de uitvoer van de omgevings sjabloon** en voer een referentie naam in. Voor dit voor beeld voert u **BaseEnv** in als referentie naam. U gebruikt deze BaseEnv bij het configureren van de volgende taak. 

![Taak voor Azure DevTest Labs omgeving maken](./media/use-devtest-labs-build-release-pipelines/create-environment.png)

### <a name="populate-environment-task"></a>Omgevings taak vullen
De tweede taak (**Azure DevTest Labs het invullen** van de omgevings taak) is het bijwerken van de bestaande DevTest Labs-omgeving. De taak voor het maken van een omgeving **BaseEnv. environmentResourceId** die wordt gebruikt voor het configureren van de omgevings naam voor deze taak. De Resource Manager-sjabloon voor dit voor beeld heeft twee para meters: **adminUserName** en **adminPassword**. 

![Taak voor Azure DevTest Labs omgeving vullen](./media/use-devtest-labs-build-release-pipelines/populate-environment.png)

## <a name="app-service-deploy-task"></a>Taak implementeren App Service
De derde taak is de taak **implementatie Azure app service** . Het app-type is ingesteld op **Web-app** en de naam van de app service is ingesteld op **$ (website)** .

![Taak implementeren App Service](./media/use-devtest-labs-build-release-pipelines/app-service-deploy.png)

## <a name="setup-release-pipeline"></a>Pijp lijn installatie-release
U maakt een release pijplijn met twee taken: **Azure-implementatie: Resource groep** maken of bijwerken en **Azure app service implementeren**. 

Geef voor de eerste taak de naam en de locatie van de resource groep op. De locatie van de sjabloon is een gekoppeld artefact. Als de Resource Manager-sjabloon gekoppelde sjablonen bevat, moet een aangepaste implementatie van een resource groep worden geïmplementeerd. De sjabloon bevindt zich in het gepubliceerde drop-artefact. Temp late para meters voor de Resource Manager-sjabloon negeren. U kunt de resterende instellingen met standaard waarden laten staan. 

Voor de tweede taak **implementeren Azure app service**, geeft u het Azure-abonnement op, selecteert u **Web-app** voor het **app-type**en **$ (website)** voor de naam van de **app service**. U kunt de resterende instellingen met standaard waarden laten staan. 

## <a name="test-run"></a>Test uitvoering
Nu beide pijp lijnen zijn ingesteld, kunt u een build hand matig in de wachtrij plaatsen en de IT-afdeling weer geven. De volgende stap is het instellen van de juiste trigger voor de build en het maken van de build met de release pijplijn.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

- [Azure DevTest Labs integreren in uw continue integratie-en leverings pijplijn voor Azure DevOps](devtest-lab-integrate-ci-cd-vsts.md)
- [Omgevingen integreren in uw Azure DevOps CI/CD-pijp lijnen](integrate-environments-devops-pipeline.md)
