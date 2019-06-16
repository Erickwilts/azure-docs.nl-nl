---
title: Implementatie en verbruik
titleSuffix: Azure Machine Learning Studio
description: U kunt Azure Machine Learning Studio gebruiken om machine learning-werkstromen en modellen als webservices te implementeren. Deze webservices kunnen vervolgens worden gebruikt voor het aanroepen van de machine learning-modellen van toepassingen via internet wilt voorspellingen in realtime of in de batchmodus.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 04/19/2017
ms.openlocfilehash: 0a29d763ab54ee716e514df23576e9c3b294d792
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60751073"
---
# <a name="azure-machine-learning-studio-web-services-deployment-and-consumption"></a>Azure Machine Learning Studio Web Services: Implementatie en verbruik

U kunt Azure Machine Learning Studio gebruiken om machine learning-werkstromen en modellen als webservices te implementeren. Deze webservices kunnen vervolgens worden gebruikt voor het aanroepen van de machine learning-modellen van toepassingen via Internet wilt voorspellingen in realtime of in de batchmodus. Omdat de webservices RESTful zijn, kunt u deze aanroepen van verschillende programmeertalen en platforms, zoals .NET, Java, en toepassingen, zoals Excel.

De volgende secties vindt u koppelingen naar scenario's, code en documentatie waarmee kunt u aan de slag.

## <a name="deploy-a-web-service"></a>Een webservice implementeren

### <a name="with-azure-machine-learning-studio"></a>Met Azure Machine Learning Studio

De Studio-portal en de Microsoft Azure Machine Learning-webservices-portal kunt u implementeren en beheren van een webservice zonder code te schrijven.

De volgende koppelingen bieden algemene informatie over het implementeren van een nieuwe webservice:

* Zie voor een overzicht over het implementeren van een nieuwe webservice die gebaseerd op Azure Resource Manager, [een nieuwe webservice implementeren](publish-a-machine-learning-web-service.md).
* Zie voor een overzicht over het implementeren van een webservice, [een Azure Machine Learning-webservice implementeren](publish-a-machine-learning-web-service.md).
* Voor een overzicht over het maken en implementeren van een webservice, beginnen met [zelfstudie 1: Kredietrisico voorspellen](tutorial-part1-credit-risk.md).
* Zie voor specifieke voorbeelden die een webservice implementeren:

  * [Zelfstudie 3: Tegoed risicomodel implementeren](tutorial-part3-credit-risk-deploy.md)
  * [Een webservice implementeren in meerdere regio 's](/azure/machine-learning/studio/publish-a-machine-learning-web-service#multi-region)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a>Bij de resourceprovider voor web services API's (Azure Resource Manager-API's)

De Azure Machine Learning Studio-resourceprovider voor web-services kunt implementeren en beheren van webservices met behulp van REST API-aanroepen. Zie voor meer informatie de [Machine Learning Web Service (REST)](/rest/api/machinelearning/index) verwijzing.

<!-- [Machine Learning Web Service (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) reference. -->

### <a name="with-powershell-cmdlets"></a>Met PowerShell-cmdlets

De Azure Machine Learning Studio-resourceprovider voor web-services kunt implementeren en beheren van webservices met behulp van PowerShell-cmdlets.

De cmdlets wilt gebruiken, moet u eerst aanmelden bij uw Azure-account van de PowerShell-omgeving met behulp van de [Connect AzAccount](/powershell/module/az.accounts/connect-azaccount) cmdlet. Als u niet bekend bent met het aanroepen van de PowerShell-opdrachten die zijn gebaseerd op Resource Manager, Zie [met behulp van Azure PowerShell met Azure Resource Manager](../../azure-resource-manager/manage-resources-powershell.md).

Voor het exporteren van uw Voorspellend experiment [deze voorbeeldcode](https://github.com/ritwik20/AzureML-WebServices). Nadat u het .exe-bestand van de code hebt gemaakt, kunt u het volgende typen:

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

De toepassing wordt uitgevoerd, maakt een web-service JSON-sjabloon. Als u de sjabloon wilt een webservice implementeert, moet u de volgende informatie toevoegen:

* Storage-accountnaam en -sleutel

    U kunt de storage-accountnaam en de sleutel ophalen uit de [Azure-portal](https://portal.azure.com/).
* Toezegging plan-ID

    U krijgt de plan-ID van de [Azure Machine Learning-webservices](https://services.azureml.net) portal door het aanmelden en klikt u op de Plannaam van een.

Toevoegen aan de JSON-sjabloon als onderliggende items van de *eigenschappen* knooppunt op hetzelfde niveau als het *MachineLearningWorkspace* knooppunt.

Hier volgt een voorbeeld:

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

Zie de volgende artikelen en voorbeeldcode voor meer informatie:

* [Azure Machine Learning Studio-Cmdlets](https://docs.microsoft.com/powershell/module/az.machinelearning) verwijzingen op MSDN
* Voorbeeld [scenario](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) op GitHub

## <a name="consume-the-web-services"></a>De webservices gebruiken

### <a name="from-the-azure-machine-learning-web-services-ui-testing"></a>Vanuit de Azure Machine Learning-webservices UI (testen)

U kunt uw webservice van Azure Machine Learning-webserviceportal testen. Dit omvat de Request Response-service (RRS) testen en interfaces voor batchuitvoeringsservice (BES).

* [Een nieuwe webservice implementeren](publish-a-machine-learning-web-service.md)
* [Een Azure Machine Learning-webservice implementeren](publish-a-machine-learning-web-service.md)
* [Zelfstudie 3: Tegoed risicomodel implementeren](tutorial-part3-credit-risk-deploy.md)

### <a name="from-excel"></a>Vanuit Excel

U kunt een Excel-sjabloon die de webservice verbruikt downloaden:

* [Een Azure Machine Learning-webservice uit Excel](consuming-from-excel.md)
* [Excel-invoegtoepassing voor Azure Machine Learning-webservices](excel-add-in-for-web-services.md)

### <a name="from-a-rest-based-client"></a>Van een REST-client

Azure Machine Learning-webservices zijn RESTful API's. U kunt deze API's van verschillende platformen, zoals .NET, Python, R, Java, enzovoort verbruiken. De **verbruiken** pagina voor uw webservice op de [Microsoft Azure Machine Learning-webserviceportal](https://services.azureml.net) heeft voorbeeldcode waarmee u aan de slag kunt. Zie [How to consume an Azure Machine Learning Web service](consume-web-services.md) (Azure Machine Learning-webservice gebruiken) voor meer informatie.
