---
title: Modellen implementeren in azure Kubernetes service
titleSuffix: Azure Machine Learning service
description: Meer informatie over het implementeren van uw Azure Machine Learning-service modellen als een webservice met behulp van de Azure Kubernetes-service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 07/08/2019
ms.openlocfilehash: 4a0aab2ca2f0bbcee07f09124e68c3623d16004d
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68848155"
---
# <a name="deploy-a-model-to-an-azure-kubernetes-service-cluster"></a>Een model implementeren in een Azure Kubernetes service-cluster

Meer informatie over het gebruik van de Azure Machine Learning-service voor het implementeren van een model als een webservice op Azure Kubernetes service (AKS). De Azure Kubernetes-service is geschikt voor grootschalige productie-implementaties. Gebruik de Azure Kubernetes-service als u een of meer van de volgende mogelijkheden nodig hebt:

- __Snelle reactie tijd__.
- Automatisch __schalen__ van de geïmplementeerde service.
- Opties voor hardwareversnelling, zoals GPU en Programmeer bare poort matrices (FPGA).

> [!IMPORTANT]
> Cluster schalen wordt niet gegeven via de Azure Machine Learning SDK. Zie [het aantal knoop punten schalen in een AKS-cluster](../../aks/scale-cluster.md)voor meer informatie over het schalen van de knooppunten in een AKS-cluster.

Wanneer u implementeert in azure Kubernetes service, implementeert u naar een AKS-cluster dat is __verbonden met uw werk ruimte__. Er zijn twee manieren om een AKS-cluster te verbinden met uw werk ruimte:

* Maak het AKS-cluster met behulp van de SDK van de Azure Machine Learning-service, de Machine Learning CLI of de Azure Portal. Dit proces verbindt automatisch het cluster met de werk ruimte.
* Koppel een bestaand AKS-cluster aan uw Azure Machine Learning service-werk ruimte. Een cluster kan worden gekoppeld met behulp van de Azure Machine Learning Service SDK, Machine Learning CLI of de Azure Portal.

> [!IMPORTANT]
> Het maken of verzenden van een bijlage is een eenmalige taak. Zodra een AKS-cluster is verbonden met de werk ruimte, kunt u het gebruiken voor implementaties. U kunt het AKS-cluster loskoppelen of verwijderen als u het niet meer nodig hebt. Als detatched of verwijderd is, kunt u niet meer implementeren naar het cluster.

## <a name="prerequisites"></a>Vereisten

- Een werkruimte van Azure Machine Learning-service. Zie [een Azure machine learning service-werk ruimte maken](how-to-manage-workspace.md)voor meer informatie.

- Een machine learning model dat in uw werk ruimte is geregistreerd. Als u geen geregistreerd model hebt, raadpleegt u [hoe en hoe u modellen implementeert](how-to-deploy-and-where.md).

- De [Azure cli-extensie voor machine learning service](reference-azure-machine-learning-cli.md), [Azure machine learning python SDK](https://aka.ms/aml-sdk)of de [Azure machine learning Visual Studio code extension](how-to-vscode-tools.md).

- In de code fragmenten van __python__ in dit artikel wordt ervan uitgegaan dat de volgende variabelen zijn ingesteld:

    * `ws`-Ingesteld op uw werk ruimte.
    * `model`-Ingesteld op uw geregistreerde model.
    * `inference_config`-Stel in op de configuratie voor het afstellen van het model.

    Zie [hoe en wanneer u modellen wilt implementeren](how-to-deploy-and-where.md)voor meer informatie over het instellen van deze variabelen.

- In het __cli__ -fragment in dit artikel wordt ervan uitgegaan dat `inferenceconfig.json` u een document hebt gemaakt. Zie [hoe en wanneer u modellen wilt implementeren](how-to-deploy-and-where.md)voor meer informatie over het maken van dit document.

## <a name="create-a-new-aks-cluster"></a>Een nieuw AKS-cluster maken

**Geschatte tijd**: Ongeveer 20 minuten.

Het maken of koppelen van een AKS-cluster is een eenmalig proces voor uw werk ruimte. U kunt dit cluster voor meerdere implementaties opnieuw gebruiken. Als u het cluster of de resource groep verwijdert die het bevat, moet u de volgende keer dat u moet implementeren een nieuw cluster maken. Er kunnen meerdere AKS-clusters aan uw werk ruimte zijn gekoppeld.

Als u een AKS-cluster wilt maken voor __ontwikkeling__, __validatie__en __testen__ in plaats van productie, kunt u het __cluster doel__ opgeven voor __dev test__.

> [!WARNING]
> Als u dit `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST`hebt ingesteld, is het cluster dat is gemaakt niet geschikt voor verkeer op productie niveau en kunnen de tijden voor de afnemen toenemen. Dev/test-clusters garanderen ook geen fout tolerantie. We raden ten minste twee virtuele Cpu's aan voor dev/test-clusters.

De volgende voor beelden laten zien hoe u een nieuw AKS-cluster maakt met behulp van de SDK en CLI:

**De SDK gebruiken**

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (you can also provide parameters to customize this).
# For example, to create a dev/test cluster, use:
# prov_config = AksCompute.provisioning_configuration(cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'myaks'
# Create the cluster
aks_target = ComputeTarget.create(workspace = ws,
                                    name = aks_name,
                                    provisioning_configuration = prov_config)

# Wait for the create process to complete
aks_target.wait_for_completion(show_output = True)
```

> [!IMPORTANT]
> Voor [`provisioning_configuration()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py), als u aangepaste waarden voor `agent_count` en `vm_size`selecteert en `cluster_purpose` niet `DEV_TEST`, moet u ervoor zorgen dat `agent_count` vermenigvuldigd door `vm_size` is groter dan of gelijk aan 12 virtuele cpu's. Als u bijvoorbeeld een `vm_size` van ' Standard_D3_v2 ' gebruikt, die 4 virtuele cpu's heeft, moet u een van de `agent_count` drie of grotere kiezen.
>
> De Azure Machine Learning SDK biedt geen ondersteuning voor het schalen van een AKS-cluster. Als u de knoop punten in het cluster wilt schalen, gebruikt u de gebruikers interface voor uw AKS-cluster in de Azure Portal. U kunt alleen het aantal knoop punten wijzigen, niet de VM-grootte van het cluster.

Voor meer informatie over de klassen, methoden en para meters die in dit voor beeld worden gebruikt, raadpleegt u de volgende referentie documenten:

* [AksCompute.ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py)
* [AksCompute.provisioning_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-)
* [ComputeTarget. Create](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computetarget?view=azure-ml-py#create-workspace--name--provisioning-configuration-)
* [ComputeTarget.wait_for_completion](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computetarget?view=azure-ml-py#wait-for-completion-show-output-false-)

**De CLI gebruiken**

```azurecli
az ml computetarget create aks -n myaks
```

Zie voor meer informatie de computetarget voor het maken van een ASK-verwijzing [AZ ml](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/create?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-create-aks) .

## <a name="attach-an-existing-aks-cluster"></a>Een bestaand AKS-cluster koppelen

**Geschatte tijd:** Ongeveer 5 minuten.

Als u al een AKS-cluster in uw Azure-abonnement hebt en dit versie 1.12. # # is, kunt u het gebruiken om uw installatie kopie te implementeren.

> [!TIP]
> De bestaande AKS-cluster kan zich in een Azure-regio bevinden dan uw Azure Machine Learning service-werk ruimte.

> [!WARNING]
> Wanneer u een AKS-cluster koppelt aan een werk ruimte, kunt u definiëren hoe u het cluster gaat gebruiken `cluster_purpose` door de para meter in te stellen.
>
> Als u de `cluster_purpose` para meter of set `cluster_purpose = AksCompute.ClusterPurpose.FAST_PROD`niet instelt, moet het cluster ten minste 12 virtuele cpu's hebben.
>
> Als u instelt `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST`, hoeft het cluster niet 12 virtuele cpu's te hebben. We raden ten minste twee virtuele Cpu's aan voor dev/test. Een cluster dat is geconfigureerd voor dev/test is echter niet geschikt voor verkeer op productie niveau en kan de tijd voor de afnemen verhogen. Dev/test-clusters garanderen ook geen fout tolerantie.

Raadpleeg de volgende artikelen voor meer informatie over het maken van een AKS-cluster met behulp van de Azure CLI of portal:

* [Een AKS-cluster maken (CLI)](https://docs.microsoft.com/cli/azure/aks?toc=%2Fazure%2Faks%2FTOC.json&bc=%2Fazure%2Fbread%2Ftoc.json&view=azure-cli-latest#az-aks-create)
* [Een AKS-cluster maken (Portal)](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal?view=azure-cli-latest)

In de volgende voor beelden ziet u hoe u een bestaand AKS-1.12. # #-cluster koppelt aan uw werk ruimte:

**De SDK gebruiken**

```python
from azureml.core.compute import AksCompute, ComputeTarget
# Set the resource group that contains the AKS cluster and the cluster name
resource_group = 'myresourcegroup'
cluster_name = 'myexistingcluster'

# Attach the cluster to your workgroup. If the cluster has less than 12 virtual CPUs, use the following instead:
# attach_config = AksCompute.attach_configuration(resource_group = resource_group,
#                                         cluster_name = cluster_name,
#                                         cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST)
attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
aks_target = ComputeTarget.attach(ws, 'myaks', attach_config)
```

Voor meer informatie over de klassen, methoden en para meters die in dit voor beeld worden gebruikt, raadpleegt u de volgende referentie documenten:

* [AksCompute.attach_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-)
* [AksCompute.ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py)
* [AksCompute. attach](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computetarget?view=azure-ml-py#attach-workspace--name--attach-configuration-)

**De CLI gebruiken**

Als u een bestaand cluster wilt koppelen met behulp van de CLI, moet u de bron-ID van het bestaande cluster ophalen. Gebruik de volgende opdracht om deze waarde op te halen. Vervang `myexistingcluster` door de naam van uw AKS-cluster. Vervang `myresourcegroup` door de resource groep die het cluster bevat:

```azurecli
az aks show -n myexistingcluster -g myresourcegroup --query id
```

Met deze opdracht wordt een waarde geretourneerd die vergelijkbaar is met de volgende tekst:

```text
/subscriptions/{GUID}/resourcegroups/{myresourcegroup}/providers/Microsoft.ContainerService/managedClusters/{myexistingcluster}
```

Gebruik de volgende opdracht om het bestaande cluster aan uw werk ruimte te koppelen. Vervang `aksresourceid` door de waarde die door de vorige opdracht is geretourneerd. Vervang `myresourcegroup` door de resource groep die uw werk ruimte bevat. Vervang `myworkspace` door de naam van uw werk ruimte.

```azurecli
az ml computetarget attach aks -n myaks -i aksresourceid -g myresourcegroup -w myworkspace
```

Zie voor meer informatie de referentie [AZ ml computetarget attach AKS](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/attach?view=azure-cli-latest#ext-azure-cli-ml-az-ml-computetarget-attach-aks) .

## <a name="deploy-to-aks"></a>Implementeren naar AKS

Als u een model wilt implementeren in azure Kubernetes service, maakt u een __implementatie configuratie__ waarin de benodigde reken resources worden beschreven. Bijvoorbeeld het aantal kernen en het geheugen. U hebt ook een Afleidings __configuratie__nodig, waarmee de omgeving wordt beschreven die nodig is voor het hosten van het model en de webservice. Zie [hoe en wanneer u modellen wilt implementeren](how-to-deploy-and-where.md)voor meer informatie over het maken van de configuratie voor demijnen.

### <a name="using-the-sdk"></a>De SDK gebruiken

```python
aks_target = AksCompute(ws,"myaks")
# If deploying to a cluster configured for dev/test, ensure that it was created with enough
# cores and memory to handle this deployment configuration. Note that memory is also used by
# things such as dependencies and AML components.
deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config, aks_target)
service.wait_for_deployment(show_output = True)
print(service.state)
print(service.get_logs())
```

Voor meer informatie over de klassen, methoden en para meters die in dit voor beeld worden gebruikt, raadpleegt u de volgende referentie documenten:

* [AksCompute](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute?view=azure-ml-py)
* [AksWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py)
* [Model. implementeren](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config--deployment-config-none--deployment-target-none-)
* [Webservice-wait_for_deployment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#wait-for-deployment-show-output-false-)

### <a name="using-the-cli"></a>De CLI gebruiken

Als u wilt implementeren met behulp van de CLI, gebruikt u de volgende opdracht. Vervang `myaks` door de naam van het AKS Compute-doel. Vervang `mymodel:1` door de naam en versie van het geregistreerde model. Vervang `myservice` door de naam om deze service te verlenen:

```azurecli-interactive
az ml model deploy -ct myaks -m mymodel:1 -n myservice -ic inferenceconfig.json -dc deploymentconfig.json
```

[!INCLUDE [deploymentconfig](../../../includes/machine-learning-service-aks-deploy-config.md)]

Zie voor meer informatie de referentie [AZ ml model Deploy](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext-azure-cli-ml-az-ml-model-deploy) . 

## <a name="using-vs-code"></a>VS code gebruiken

Zie voor meer informatie over het gebruik van VS code [implementeren naar AKS via de VS code-extensie](how-to-vscode-tools.md#deploy-and-manage-models).

> [!IMPORTANT] 
> Voor de implementatie via VS code moet het AKS-cluster vooraf worden gemaakt of aan uw werk ruimte zijn gekoppeld.

## <a name="update-the-web-service"></a>Bijwerken van de webservice

[!INCLUDE [aml-update-web-service](../../../includes/machine-learning-update-web-service.md)]

## <a name="next-steps"></a>Volgende stappen

* [Een model implementeren met behulp van een aangepaste docker-installatie kopie](how-to-deploy-custom-docker-image.md)
* [Problemen met implementatie oplossen](how-to-troubleshoot-deployment.md)
* [Azure Machine Learning-webservices met SSL beveiligde](how-to-secure-web-service.md)
* [Een ML-Model dat is geïmplementeerd als een webservice gebruiken](how-to-consume-web-service.md)
* [Uw Azure Machine Learning modellen bewaken met Application Insights](how-to-enable-app-insights.md)
* [Gegevens verzamelen voor modellen in productie](how-to-enable-data-collection.md)
