---
title: Wat is er gebeurd met Machine Learning Workbench?
titleSuffix: Azure Machine Learning service
description: Meer informatie over wat er met de Machine Learning Workbench-toepassing is gebeurd, wat er in de Azure Machine Learning-service is veranderd en wat de ondersteuningstijdlijn is.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: overview
ms.reviewer: jmartens
author: j-martens
ms.author: jmartens
ms.date: 05/14/2019
ms.custom: seodec18
ms.openlocfilehash: 1bcbaf5ec3b15a36b28aa7d4b3346b85e1a7cc24
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65785791"
---
# <a name="what-happened-to-azure-machine-learning-workbench"></a>Wat is er gebeurd met Azure Machine Learning Workbench?

De Azure Machine Learning Workbench-toepassing en een aantal andere vroege functies zijn afgeschaft en vervangen in de versie van september 2018 om plaats te maken voor een verbeterde [architectuur](concept-azure-machine-learning-architecture.md). 

De nieuwe versie bevat veel belangrijke updates op basis van feedback van klanten, om uw ervaring te verbeteren. De kernfunctionaliteit voor experimentele uitvoeringen tot modelimplementatie is niet gewijzigd. U kunt nu echter de robuuste <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> en de [Azure CLI](reference-azure-machine-learning-cli.md) gebruiken om uw machine learning-taken en -pijplijnen uit te voeren.  

De meeste artefacten die in de eerdere versie van Azure Machine Learning Service zijn gemaakt, worden opgeslagen in uw eigen lokale cloud of cloudopslag. Deze artefacten worden nooit verwijderd.

In dit artikel leest u wat er is veranderd en hoe dit van invloed is op uw bestaande werk met de Azure Machine Learning Workbench en de bijbehorende API's.

>[!Warning]
>Dit artikel is niet bedoeld voor gebruikers van Azure Machine Learning Studio, maar voor klanten van Azure Machine Learning Service die de Workbench-toepassing (preview) hebben geïnstalleerd en/of beschikken over preview-accounts voor experimenten en modelbeheer.


## <a name="what-changed"></a>Wat is er veranderd?

De meest recente versie van Azure Machine Learning Service bevat de volgende functies:
+ Een [vereenvoudigd model voor Azure-resources](concept-azure-machine-learning-architecture.md).
+ Een [nieuwe gebruikersinterface in de portal](how-to-track-experiments.md) voor het beheren van uw experimenten en compute-doelen.
+ Een nieuwe, uitgebreidere Python-<a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>.
+ De nieuwe, uitgebreide [Azure CLI-extensie](reference-azure-machine-learning-cli.md) voor machine learning.

De [architectuur](concept-azure-machine-learning-architecture.md) is vernieuwd voor meer gebruiksgemak. In plaats van meerdere Azure-resources en -accounts hebt u alleen een [werkruimte voor Azure Machine Learning service](concept-azure-machine-learning-architecture.md#workspace) nodig. U kunt werkruimten snel maken in de [Azure portal]\((setup-create-workspace.md#portal). Met een werkruimte kunnen meerdere gebruikers rekendoelen voor training en implementatie, modelexperimenten, Docker-installatiekopieën, geïmplementeerde modellen, enzovoort, opslaan.

De huidige versie bevat nieuwe en verbeterde CLI- en SDK-clients, maar de Workbench-bureaubladtoepassing zelf is afgeschaft. Experimenten kunnen worden beheerd in het [Werkruimte-dashboard in de Azure-portal](how-to-track-experiments.md#view-the-experiment-in-the-azure-portal). Gebruik het dashboard om uw experimentgeschiedenis op te halen, de compute-doelen die zijn gekoppeld aan uw werkruimte te beheren, uw modellen en Docker-installatiekopieën te beheren en zelfs om webservices te implementeren.

<a name="timeline"></a>

## <a name="support-timeline"></a>Ondersteuningstijdlijn

Met ingang van 9 januari 2019 is de ondersteuning voor Machine Learning Workbench, accounts van Azure Machine Learning voor experimenten en modelbeheer en de bijbehorende SDK en CLI beëindigd. 

Alle nieuwste mogelijkheden zijn beschikbaar via deze nieuwe <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>, de [CLI](reference-azure-machine-learning-cli.md) en de [portal](setup-create-workspace.md#portal).

## <a name="what-about-run-histories"></a>Hoe zit het met uitvoeringsgeschiedenissen?

Oudere uitvoeringsgeschiedenissen zijn niet meer toegankelijk, maar uw uitvoeringen zijn nog wel zichtbaar in de nieuwste versie. 

Uitvoeringsgeschiedenissen heten voortaan **experimenten**. U kunt de experimenten van uw model verzamelen en ze verkennen met behulp van de SDK, de CLI of Azure Portal.

Het werkruimtedashboard van de portal wordt alleen ondersteund in Microsoft Edge, Chrome en Firefox:

[![Online-portal](./media/overview-what-happened-to-workbench/image001.png)](./media/overview-what-happened-to-workbench/image001.png#lightbox)

Start uw modellen trainen en de uitvoeringsgeschiedenis met behulp van de nieuwe SDK en CLI bij te houden. Ga voor meer informatie naar de zelfstudie [ een model voor de classificatie van afbeeldingen trainen met de Azure Machine Learning Service](tutorial-train-models-with-aml.md).

## <a name="can-i-still-prep-data"></a>Kan ik nog steeds gegevens voorbereiden?

Uw bestaande gegevensvoorbereidingsbestanden zijn niet overdraagbaar naar de nieuwste versie omdat Machine Learning Workbench niet meer bestaat. U kunt echter nog steeds u een gegevensset van elke grootte voorbereiden voor modellering.   

Met gegevenssets van elke grootte, kunt u de [pakket voor gegevensvoorbereiding van Azure Machine Learning](https://aka.ms/data-prep-sdk) snel uw gegevens voorafgaand aan het maken van modellering voorbereiden door Python-code te schrijven. 

U kunt [deze zelfstudie](tutorial-data-prep.md) volgen voor meer informatie over het gebruik van de Azure Machine Learning Data Prep SDK.

## <a name="will-projects-persist"></a>Blijven projecten behouden?

U verliest geen code of werk. Projecten zijn in de oudere versie cloudentiteiten met een lokale map. In de nieuwste versie kunt u lokale mappen koppelen aan de werkruimte van Azure Machine Learning Service met behulp van een lokaal configuratiebestand. Bekijk een [diagram van de meest recente architectuur](concept-azure-machine-learning-architecture.md).

Veel van de projectinhoud staat al op uw lokale computer. U hoeft alleen een configuratiebestand in die map te maken en ernaar te verwijzen in uw code om het aan uw werkruimte te koppelen. Om door te gaan met behulp van de lokale directory met uw bestanden en scripts, geef de naam van de map in de ['experiment.submit'](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py) Python-opdracht, of met behulp van de `az ml project attach` CLI-opdracht.  Bijvoorbeeld:
```python
run = exp.submit(source_directory = script_folder, script = 'train.py', run_config = run_config_system_managed)
```

[Een werkruimte maken](setup-create-workspace.md#portal) aan de slag.

## <a name="what-about-my-registered-models-and-images"></a>Hoe zit het met mijn geregistreerde modellen en installatiekopieën?

De modellen die u in uw oude modelregister hebt geregistreerd, moeten worden gemigreerd naar uw nieuwe werkruimte als u ze wilt blijven gebruiken. Voor het migreren van uw modellen downloadt u de modellen en registreert u deze opnieuw in uw nieuwe werkruimte. 

De installatiekopieën die u hebt gemaakt in het register van de oude installatiekopie kunnen niet rechtstreeks worden gemigreerd naar de nieuwe werkruimte. In de meeste gevallen kan het model worden geïmplementeerd zonder te hoeven maken van een installatiekopie. Indien nodig, kunt u een installatiekopie voor het model in de nieuwe werkruimte maken. Zie voor meer informatie, [beheren, registreren, implementeren en bewaken van machine learning-modellen](concept-model-management-and-deployment.md).

## <a name="what-about-deployed-web-services"></a>Hoe zit het met geïmplementeerde webservices?

Aangezien de ondersteuning voor de oude CLI is beëindigd, kunt u modellen of webservices die u oorspronkelijk hebt geïmplementeerd met uw account voor modelbeheer niet meer opnieuw implementeren of beheren. Deze webservices blijven echter werken zolang Azure Container Service (ACS) wordt ondersteund.

In de nieuwste versie worden modellen als webservices geïmplementeerd in ACI-clusters (Azure Container Instances) of AKS-clusters (Azure Kubernetes Service). U kunt ook in FPGA's en Azure IoT Edge implementeren. 

Ga voor meer informatie naar deze artikelen:
+ [Modellen implementeren met Azure Machine Learning Service](how-to-deploy-and-where.md)
+ [Zelfstudie: Een afbeeldingsclassificatiemodel implementeren in Azure Container Instances](tutorial-deploy-models-with-aml.md)

## <a name="what-about-the-old-sdk-and-cli"></a>Hoe zit het met de oude SDK en CLI?

Ja, deze blijven gewoon werken tot januari. Zie de voorgaande [tijdlijn](#timeline). Het is raadzaam om uw nieuwe experimenten en modellen met de meest recente SDK of CLI te maken.

Met de nieuwe Python-SDK in de nieuwste versie kunt u met Azure Machine Learning Service in elke Python-omgeving communiceren. Informatie over het installeren van de meest recente <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>. U kunt ook de bijgewerkte [Azure Machine Learning CLI-extensie](reference-azure-machine-learning-cli.md) gebruiken met de uitgebreide set `az ml`-opdrachten om in elke opdrachtregelomgeving met de service te communiceren, waaronder Azure Cloud Shell.

## <a name="what-about-visual-studio-code-tools-for-ai"></a>Hoe zit het met Visual Studio Code-hulpprogramma's voor AI?

In deze nieuwste release is de naam van de extensie gewijzigd in Azure Machine Learning voor Visual Studio Code. De extensie is uitgebreid en verbeterd voor gebruik met de eerdergenoemde nieuwe functies.

[![Azure Machine Learning voor Visual Studio Code](./media/overview-what-happened-to-workbench/vscode.png)](./media/overview-what-happened-to-workbench/vscode-big.png#lightbox)

## <a name="what-about-domain-packages"></a>Hoe zit het met domeinpakketten?

De domeinpakketten voor Computer Vision, tekstanalyse en prognose kunnen niet worden gebruikt met de nieuwste versie van Azure Machine Learning. U kunt echter nog steeds Computer Vision-, tekst- en prognosemodellen bouwen en trainen met de meest recente Azure Machine Learning <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> voor Python. Voor informatie over het migreren van bestaande modellen die zijn gebouwd met Computer Vision-, tekstanalyse- en prognosepakketten, kunt u contact opnemen via [AML-Packages@microsoft.com](mailto:AML-Packages@microsoft.com).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [meest recente architectuur voor Azure Machine Learning Service](concept-azure-machine-learning-architecture.md). 

Zie [Wat is Azure Machine Learning Service?](overview-what-is-azure-ml.md) voor een overzicht van de service.

Voor een snelstart u voor het uitvoeren van een script en de uitvoeringsgeschiedenis van het script met de meest recente versie van Azure Machine Learning-service te verkennen, kunt u proberen [aan de slag met Azure Machine Learning-service](quickstart-run-cloud-notebook.md).

Voor een uitgebreid overzicht van deze werkstroom volgt u de [volledige zelfstudie](tutorial-train-models-with-aml.md), met gedetailleerde stappen voor het trainen en implementeren van modellen met Azure Machine Learning service. 
