---
title: Werk ruimten maken in de portal
titleSuffix: Azure Machine Learning
description: Meer informatie over het maken, weer geven en verwijderen van Azure Machine Learning-werk ruimten in de Azure Portal.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: sgilley
author: sdgilley
ms.date: 09/30/2020
ms.topic: conceptual
ms.custom: how-to, fasttrack-edit
ms.openlocfilehash: d2885c6cc259cba74ab991ecf5046856984824f1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91631240"
---
# <a name="create-and-manage-azure-machine-learning-workspaces-in-the-azure-portal"></a>Azure Machine Learning-werkruimten maken en beheren in de Azure-portal


In dit artikel maakt, bekijkt en verwijdert u [**Azure machine learning-werk ruimten**](concept-workspace.md) in de Azure Portal voor [Azure machine learning](overview-what-is-azure-ml.md).  De portal is de eenvoudigste manier om aan de slag te gaan met werk ruimten, maar als uw behoeften wijzigen of vereisten voor automatisering verhogen, kunt u ook werk ruimten maken en verwijderen [met behulp van de CLI](reference-azure-machine-learning-cli.md), [met Python-code](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py&preserve-view=true) of [via de VS code-extensie](tutorial-setup-vscode-extension.md).

## <a name="create-a-workspace"></a>Een werkruimte maken

Als u een werk ruimte wilt maken, hebt u een Azure-abonnement nodig. Als u geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer vandaag nog de [gratis of betaalde versie van Azure Machine Learning](https://aka.ms/AMLFree).

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/) met behulp van de referenties voor uw Azure-abonnement. 

1. Selecteer in de linkerbovenhoek van Azure Portal **+ een resource maken**.

      ![Een nieuwe resource maken](./media/how-to-manage-workspace/create-workspace.gif)

1. Gebruik de zoek balk om **machine learning**te vinden.

1. Selecteer **machine learning**.

1. Selecteer in het deel venster **machine learning** de optie **maken** om te beginnen.

1. Geef de volgende informatie op om uw nieuwe werk ruimte te configureren:

   Veld|Beschrijving 
   ---|---
   Werkruimtenaam |Voer een unieke naam in die uw werk ruimte identificeert. In dit voor beeld gebruiken we **docs-WS**. De namen moeten uniek zijn in de resource groep. Gebruik een naam die gemakkelijk kan worden ingetrokken en om onderscheid te maken tussen werk ruimten die door anderen zijn gemaakt. De naam van de werk ruimte is niet hoofdletter gevoelig.
   Abonnement |Selecteer het Azure-abonnement dat u wilt gebruiken.
   Resourcegroep | Gebruik een bestaande resourcegroep in uw abonnement of voer een naam in om een nieuwe resourcegroep te maken. Een resource groep bevat gerelateerde resources voor een Azure-oplossing. In dit voor beeld gebruiken we **docs-AML**. U hebt de rol *Inzender* of *eigenaar* nodig voor het gebruik van een bestaande resource groep.  Zie [toegang tot een Azure machine learning-werk ruimte beheren](how-to-assign-roles.md)voor meer informatie over toegang.
   Regio | Selecteer de Azure-regio die het dichtst bij uw gebruikers ligt en de gegevens bronnen om uw werk ruimte te maken.
   Werkruimte editie | Selecteer **Basic** of **Enter prise**.  Deze werk ruimte-editie bepaalt de functies waartoe u toegang hebt en de prijzen. Meer informatie over [Azure machine learning](overview-what-is-azure-ml.md). 

    ![Uw werk ruimte configureren](./media/how-to-manage-workspace/select-edition.png)

1. Wanneer u klaar bent met het configureren van de werk ruimte, selecteert u **controleren + maken**. Gebruik eventueel de secties [netwerken](#networking) en [Geavanceerd](#advanced) om meer instellingen te configureren voor de werk ruimte.

1. Controleer de instellingen en breng eventuele aanvullende wijzigingen of correcties aan. Wanneer u tevreden bent met de instellingen, selecteert u **maken**.

   > [!Warning] 
   > Het kan enkele minuten duren om uw werk ruimte in de cloud te maken.

   Wanneer het proces is voltooid, wordt een bericht over een geslaagde implementatie weer gegeven. 
 
 1. Als u de nieuwe werk ruimte wilt weer geven, selecteert u **Ga naar resource**.

### <a name="networking"></a>Netwerken  

> [!IMPORTANT]  
> Zie [netwerk isolatie en privacy](how-to-enable-virtual-network.md)voor meer informatie over het gebruik van een persoonlijk eind punt en een virtueel netwerk met uw werk ruimte.
    
1. De standaard netwerk configuratie is het gebruik van een __openbaar eind punt__dat toegankelijk is op het open bare Internet. Als u de toegang tot uw werk ruimte wilt beperken tot een Azure-Virtual Network die u hebt gemaakt, kunt u in plaats daarvan __persoonlijk eind punt__ selecteren als de __verbindings methode__en vervolgens __+ toevoegen__ gebruiken om het eind punt te configureren. 
    
   :::image type="content" source="media/how-to-manage-workspace/select-private-endpoint.png" alt-text="Persoonlijke eindpunt selectie":::  

1. Stel op het formulier __persoonlijk eind punt maken__ de locatie, naam en het virtuele netwerk in op gebruik. Als u het eind punt met een Privé-DNS zone wilt gebruiken, selecteert u __integreren met privé-DNS-zone__ en selecteert u de zone in het veld __privé-DNS zone__ . Selecteer __OK__ om het eind punt te maken.   

   :::image type="content" source="media/how-to-manage-workspace/create-private-endpoint.png" alt-text="Persoonlijke eindpunt selectie":::   

1. Wanneer u klaar bent met het configureren van het netwerk, kunt u __controleren + maken__selecteren of door gaan naar de optionele __Geavanceerde__ configuratie. 

    > [!WARNING]    
    > Wanneer u een persoonlijk eind punt maakt, wordt er een nieuwe Privé-DNS zone gemaakt met de naam __privatelink.API.azureml.MS__ . Dit bevat een koppeling naar het virtuele netwerk. Als u meerdere werk ruimten met persoonlijke eind punten in dezelfde resource groep maakt, kan alleen het virtuele netwerk voor het eerste persoonlijke eind punt worden toegevoegd aan de DNS-zone. Gebruik de volgende stappen om vermeldingen toe te voegen voor de virtuele netwerken die worden gebruikt door de extra werk ruimten/persoonlijke eind punten: 
    >   
    > 1. Selecteer in de [Azure Portal](https://portal.azure.com)de resource groep die de werk ruimte bevat. Selecteer vervolgens de Privé-DNS zone resource met de naam __privatelink.API.azureml.MS__.    
    > 2. Selecteer in de __instellingen__ __virtuele netwerk koppelingen__. 
    > 3. Selecteer __Toevoegen__. Geef op de pagina __virtuele netwerk koppeling toevoegen__ een unieke naam op voor de __koppeling__en selecteer vervolgens het __virtuele netwerk__ dat u wilt toevoegen. Selecteer __OK__ om de netwerk koppeling toe te voegen.    
    >   
    > Zie voor meer informatie [Azure private endpoint DNS-configuratie](/azure/private-link/private-endpoint-dns).   

### <a name="vulnerability-scanning"></a>Scannen van beveiligings problemen

Azure Security Center biedt geïntegreerd beveiligingsbeheer en geavanceerde bedreigingsbeveiliging voor verschillende hybride cloudworkloads. U moet Azure Security Center toestaan om uw resources te scannen en de aanbevelingen te volgen. Zie  [Azure container Registry Image scanning door Security Center](https://docs.microsoft.com/azure/security-center/azure-container-registry-integration) en [Azure Kubernetes Services-integratie met Security Center](https://docs.microsoft.com/azure/security-center/azure-kubernetes-service-integration)voor meer.

### <a name="advanced"></a>Geavanceerd    

Standaard worden metrische gegevens en meta data voor de werk ruimte opgeslagen in een Azure Cosmos DB exemplaar dat door micro soft wordt beheerd. Deze gegevens zijn versleuteld met door micro soft beheerde sleutels.  

Als u de gegevens wilt beperken die door micro soft worden verzameld op uw werk ruimte, selecteert u een __werk ruimte met een grote bedrijfs impact__. Zie [versleuteling op rest](concept-enterprise-security.md#encryption-at-rest)voor meer informatie over deze instelling.

> [!IMPORTANT]  
> Het selecteren van belang rijke bedrijfs impact kan alleen worden uitgevoerd bij het maken van een werk ruimte. U kunt deze instelling niet wijzigen nadat de werk ruimte is gemaakt.   
Als u de __Enter prise__ -versie van Azure machine learning gebruikt, kunt u in plaats daarvan uw eigen sleutel opgeven. Dit maakt het Azure Cosmos DB-exemplaar dat metrische gegevens en meta data opslaat in uw Azure-abonnement. Gebruik de volgende stappen om uw eigen sleutel te gebruiken:    

> [!IMPORTANT]  
> Voordat u deze stappen volgt, moet u eerst de volgende acties uitvoeren:   
>   
> 1. Machtig de __machine learning-app__ (in identiteits-en toegangs beheer) met Inzender machtigingen voor uw abonnement.  
> 1. Volg de stappen in door de [klant beheerde sleutels configureren](/azure/cosmos-db/how-to-setup-cmk) voor:   
>     * De Azure Cosmos DB provider registreren   
>     * Een Azure Key Vault maken en configureren 
>     * Een sleutel genereren  
>   
>     U hoeft het Azure Cosmos DB exemplaar niet hand matig te maken, er wordt een voor u gemaakt tijdens het maken van de werk ruimte. Dit Azure Cosmos DB exemplaar wordt in een afzonderlijke resource groep gemaakt met behulp van een naam op basis van dit patroon: `<your-workspace-resource-name>_<GUID>` .   
>   
> U kunt deze instelling niet wijzigen nadat de werk ruimte is gemaakt. Als u de Azure Cosmos DB die door uw werk ruimte worden gebruikt, verwijdert, moet u ook de werk ruimte verwijderen die er gebruik van maakt.

1. Selecteer door de __klant beheerde sleutels__en selecteer vervolgens __klikken om een sleutel te selecteren__.   

    :::image type="content" source="media/how-to-manage-workspace/advanced-workspace.png" alt-text="Persoonlijke eindpunt selectie":::   

1. Selecteer in het formulier __sleutel selecteren van Azure Key Vault__ een bestaande Azure Key Vault, een sleutel die deze bevat en de versie van de sleutel. Deze sleutel wordt gebruikt voor het versleutelen van de gegevens die zijn opgeslagen in Azure Cosmos DB. Gebruik tot slot de knop __selecteren__ om deze sleutel te gebruiken. 

   :::image type="content" source="media/how-to-manage-workspace/select-key-vault.png" alt-text="Persoonlijke eindpunt selectie":::

### <a name="download-a-configuration-file"></a>Een configuratiebestand downloaden

1. Als u een [reken instantie](tutorial-1st-experiment-sdk-setup.md#azure)wilt maken, slaat u deze stap over.

1. Als u van plan bent om code te gebruiken in uw lokale omgeving die verwijst naar deze werk ruimte, selecteert u  **config.jsdownloaden in** het gedeelte **overzicht** van de werk ruimte.  

   ![Config.json downloaden](./media/how-to-manage-workspace/configure.png)
   
   Plaats het bestand in de mapstructuur met uw python-scripts of Jupyter-notebooks. Deze kan zich in dezelfde map bevindt, in een submap met de naam *. azureml*of in een bovenliggende map. Wanneer u een reken instantie maakt, wordt dit bestand voor u toegevoegd aan de juiste map op de virtuele machine.
## <a name="find-a-workspace"></a><a name="view"></a>Een werk ruimte zoeken

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Typ **machine learning**in het bovenste zoek veld.  

1. Selecteer **machine learning**.

   ![Zoeken naar Azure Machine Learning-werk ruimte](./media/how-to-manage-workspace/find-workspaces.png)

1. Bekijk de lijst met werk ruimten die zijn gevonden. U kunt filteren op basis van abonnement, resource groepen en locaties.  

1. Selecteer een werk ruimte om de eigenschappen ervan weer te geven.

## <a name="delete-a-workspace"></a>Een werkruimte verwijderen

Selecteer in de [Azure Portal](https://portal.azure.com/) **verwijderen**  boven aan de werk ruimte die u wilt verwijderen.

:::image type="content" source="./media/how-to-manage-workspace/delete-workspace.png" alt-text="Persoonlijke eindpunt selectie":::

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="resource-provider-errors"></a>Fouten van de resource provider

[!INCLUDE [machine-learning-resource-provider](../../includes/machine-learning-resource-provider.md)]

### <a name="moving-the-workspace"></a>De werk ruimte verplaatsen

> [!WARNING]
> Het verplaatsen van uw Azure Machine Learning-werk ruimte naar een ander abonnement of het verplaatsen van het abonnement dat eigenaar is naar een nieuwe Tenant, wordt niet ondersteund. Dit kan fouten veroorzaken.

### <a name="deleting-the-azure-container-registry"></a>De Azure Container Registry verwijderen

In de Azure Machine Learning-werk ruimte wordt Azure Container Registry (ACR) gebruikt voor bepaalde bewerkingen. Er wordt automatisch een ACR-exemplaar gemaakt wanneer dit voor het eerst nodig is.

[!INCLUDE [machine-learning-delete-acr](../../includes/machine-learning-delete-acr.md)]

## <a name="next-steps"></a>Volgende stappen

Volg de zelf studie met volledige lengte voor meer informatie over het gebruik van een werk ruimte om modellen te bouwen, te trainen en te implementeren met Azure Machine Learning.

> [!div class="nextstepaction"]
> [Zelf studie: modellen trainen](tutorial-train-models-with-aml.md)
