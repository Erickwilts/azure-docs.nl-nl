---
title: Een reken instantie maken en beheren
titleSuffix: Azure Machine Learning
description: Meer informatie over het maken en beheren van een reken instantie in uw Azure Machine Learning-werk ruimte. Gebruik het reken proces als uw ontwikkel omgeving of voor training en het afleiden van dev/test-doel einden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
ms.author: sgilley
author: sdgilley
ms.reviewer: sgilley
ms.date: 10/02/2020
ms.openlocfilehash: 3b5698c782b691dd8ae91913115db184fc83a2eb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91756616"
---
# <a name="create-and-manage-an-azure-machine-learning-compute-instance"></a>Een Azure Machine Learning Compute-exemplaar maken en beheren

Meer informatie over het maken en beheren van een [reken instantie](concept-compute-instance.md) in uw Azure machine learning-werk ruimte.

Gebruik een reken instantie als uw volledig geconfigureerde en beheerde ontwikkel omgeving in de Cloud. Voor ontwikkel-en test doeleinden kunt u het exemplaar ook gebruiken als een [trainings berekenings doel](concept-compute-target.md#train) of voor een [doel gerichtheid](concept-compute-target.md#deploy).   Een reken instantie kan meerdere taken parallel uitvoeren en heeft een taak wachtrij. Als ontwikkel omgeving kan een reken instantie niet worden gedeeld met andere gebruikers in uw werk ruimte.

In dit artikel leert u het volgende:

* Een rekenproces maken 
* Een reken instantie beheren (starten, stoppen, opnieuw starten, verwijderen)
* Het Terminal venster openen 
* R-of python-pakketten installeren
* Nieuwe omgevingen of Jupyter-kernels maken

Reken instanties kunnen taken veilig uitvoeren in een [virtuele netwerk omgeving](how-to-secure-training-vnet.md), zonder dat ondernemingen de SSH-poorten hoeven te openen. De taak wordt uitgevoerd in een omgeving met containers en verpakt uw model afhankelijkheden in een docker-container. 

## <a name="prerequisites"></a>Vereisten

* Een Azure Machine Learning-werkruimte. Zie [een Azure machine learning-werk ruimte maken](how-to-manage-workspace.md)voor meer informatie.

* De [Azure cli-extensie voor machine learning service](reference-azure-machine-learning-cli.md), [Azure machine learning python SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py&preserve-view=true)of de [Azure machine learning Visual Studio code extension](tutorial-setup-vscode-extension.md).

## <a name="create"></a>Maken

**Geschatte tijd**: ongeveer 5 minuten.

Het maken van een reken instantie is een eenmalig proces voor uw werk ruimte. U kunt deze berekening opnieuw gebruiken als een ontwikkel werkstation of als reken doel voor training. Er kunnen meerdere reken instanties aan uw werk ruimte zijn gekoppeld.

De toegewezen kernen per regio per VM-serie quota en het totale regionale quotum, dat van toepassing is op het maken van een reken instantie, worden gecombineerd en gedeeld met Azure Machine Learning trainings berekenings cluster quotum. Wanneer het reken exemplaar wordt gestopt, wordt er geen quotum vrijgegeven om ervoor te zorgen dat u het reken exemplaar opnieuw kunt starten. Houd er rekening mee dat het niet mogelijk is om de grootte van de virtuele machine van reken instanties te wijzigen nadat deze is gemaakt.

In het volgende voor beeld ziet u hoe u een reken instantie maakt:

# <a name="python"></a>[Python](#tab/python)

```python
import datetime
import time

from azureml.core.compute import ComputeTarget, ComputeInstance
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your instance
# Compute instance name should be unique across the azure region
compute_name = "ci{}".format(ws._workspace_id)[:10]

# Verify that instance does not exist already
try:
    instance = ComputeInstance(workspace=ws, name=compute_name)
    print('Found existing instance, use it.')
except ComputeTargetException:
    compute_config = ComputeInstance.provisioning_configuration(
        vm_size='STANDARD_D3_V2',
        ssh_public_access=False,
        # vnet_resourcegroup_name='<my-resource-group>',
        # vnet_name='<my-vnet-name>',
        # subnet_name='default',
        # admin_user_ssh_public_key='<my-sshkey>'
    )
    instance = ComputeInstance.create(ws, compute_name, compute_config)
    instance.wait_for_completion(show_output=True)
```

Voor meer informatie over de klassen, methoden en para meters die in dit voor beeld worden gebruikt, raadpleegt u de volgende referentie documenten:

* [Klasse ComputeInstance](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computeinstance.computeinstance?view=azure-ml-py&preserve-view=true)
* [ComputeTarget. Create](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computetarget?view=azure-ml-py&preserve-view=true#create-workspace--name--provisioning-configuration-)
* [ComputeInstance.wait_for_completion](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.computeinstance(class)?view=azure-ml-py&preserve-view=true#wait-for-completion-show-output-false--is-delete-operation-false-)


# <a name="azure-cli"></a>[Azure-CLI](#tab/azure-cli)

```azurecli-interactive
az ml computetarget create computeinstance  -n instance -s "STANDARD_D3_V2" -v
```

Zie voor meer informatie de referentie [AZ ml computetarget Create computeinstance](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/create?view=azure-cli-latest&preserve-view=true#ext_azure_cli_ml_az_ml_computetarget_create_computeinstance) .

# <a name="studio"></a>[Studio](#tab/azure-studio)

Maak in uw werk ruimte in Azure Machine Learning Studio een nieuw reken exemplaar van het gedeelte **Compute** of in de sectie **notebooks** , wanneer u klaar bent om een van uw notitie blokken uit te voeren.

Zie [Compute-doelen maken in azure machine learning Studio](how-to-create-attach-compute-studio.md#compute-instance)voor meer informatie over het maken van een reken instantie in de Studio.

---

U kunt ook een reken instantie met een [Azure Resource Manager sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-machine-learning-compute-create-computeinstance)maken. 

### <a name="create-on-behalf-of-preview"></a>Maken namens (preview-versie)

Als beheerder kunt u een compute-instantie maken namens een gegevens wetenschapper en de instantie hieraan toewijzen met:
* [Azure Resource Manager sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-machine-learning-compute-create-computeinstance).  Zie [identiteits object-Id's zoeken voor verificatie configuratie](../healthcare-apis/find-identity-object-ids.md)voor meer informatie over het vinden van de TenantID en ObjectID die nodig zijn in deze sjabloon.  U kunt deze waarden ook vinden in de Azure Active Directory Portal.
* REST-API

De gegevens wetenschapper u het Compute-exemplaar maakt voor de behoeften van [Azure op rollen gebaseerde toegangs beheer (RBAC)-](../role-based-access-control/overview.md) machtigingen: 
* *Micro soft. MachineLearningServices/werk ruimten/berekeningen/starten/actie*
* *Micro soft. MachineLearningServices/werk ruimten/berekeningen/stoppen/actie*
* *Micro soft. MachineLearningServices/werk ruimten/berekeningen/opnieuw opstarten/actie*
* *Micro soft. MachineLearningServices/werk ruimten/berekeningen/applicationaccess/actie*

De gegevens wetenschapper kunnen het reken proces starten, stoppen en opnieuw starten. Ze kunnen het reken exemplaar gebruiken voor:
* Jupyter
* Jjupyterlab
* RStudio
* Geïntegreerde notebooks

## <a name="manage"></a>Beheren

Een reken instantie starten, stoppen, opnieuw starten en verwijderen. Een reken instantie wordt niet automatisch omlaag geschaald, dus zorg ervoor dat u de resource stopt om lopende kosten te voor komen.

# <a name="python"></a>[Python](#tab/python)

In de onderstaande voor beelden is de naam van het reken exemplaar **instance**

* Status ophalen

    ```python
    # get_status() gets the latest status of the ComputeInstance target
    instance.get_status()
    ```

* Stoppen

    ```python
    # stop() is used to stop the ComputeInstance
    # Stopping ComputeInstance will stop the billing meter and persist the state on the disk.
    # Available Quota will not be changed with this operation.
    instance.stop(wait_for_completion=True, show_output=True)
    ```

* Starten

    ```python
    # start() is used to start the ComputeInstance if it is in stopped state
    instance.start(wait_for_completion=True, show_output=True)
    ```

* Opnieuw starten

    ```python
    # restart() is used to restart the ComputeInstance
    instance.restart(wait_for_completion=True, show_output=True)
    ```

* Verwijderen

    ```python
    # delete() is used to delete the ComputeInstance target. Useful if you want to re-use the compute name 
    instance.delete(wait_for_completion=True, show_output=True)
    ```

# <a name="azure-cli"></a>[Azure-CLI](#tab/azure-cli)

In de onderstaande voor beelden is de naam van het reken exemplaar **instance**

* Stoppen

    ```azurecli-interactive
    az ml computetarget stop computeinstance -n instance -v
    ```

    Zie [AZ ml computetarget stop computeinstance](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/computeinstance?view=azure-cli-latest&preserve-view=true#ext-azure-cli-ml-az-ml-computetarget-computeinstance-stop)voor meer informatie.

* Starten 

    ```azurecli-interactive
    az ml computetarget start computeinstance -n instance -v
    ```

    Zie [AZ ml computetarget start computeinstance](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/computeinstance?view=azure-cli-latest&preserve-view=true#ext-azure-cli-ml-az-ml-computetarget-computeinstance-start)voor meer informatie.

* Opnieuw starten 

    ```azurecli-interactive
    az ml computetarget restart computeinstance -n instance -v
    ```

    Zie [AZ ml computetarget restart computeinstance](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget/computeinstance?view=azure-cli-latest&preserve-view=true#ext-azure-cli-ml-az-ml-computetarget-computeinstance-restart)voor meer informatie.

* Verwijderen

    ```azurecli-interactive
    az ml computetarget delete -n instance -v
    ```

    Zie [AZ ml computetarget delete computeinstance](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/computetarget?view=azure-cli-latest&preserve-view=true#ext-azure-cli-ml-az-ml-computetarget-delete)voor meer informatie.

# <a name="studio"></a>[Studio](#tab/azure-studio)

Selecteer in uw werk ruimte in Azure Machine Learning Studio **Compute**en selecteer vervolgens **Compute instance** bovenin.

![Een reken instantie beheren](./media/concept-compute-instance/manage-compute-instance.png)

U kunt de volgende acties uitvoeren:

* Een nieuw reken exemplaar maken 
* Vernieuw het tabblad Compute instances.
* Een reken instantie starten, stoppen en opnieuw starten.  U betaalt voor de instantie wanneer deze wordt uitgevoerd. Stop de reken instantie wanneer u deze niet gebruikt om de kosten te verlagen. Als u een reken instantie stopt, wordt de toewijzing ervan ongedaan. Start het vervolgens opnieuw wanneer u het nodig hebt.
* Een reken instantie verwijderen.
* De lijst met reken instanties filteren om alleen de items weer te geven die u hebt gemaakt.

Voor elk reken exemplaar in uw werk ruimte die u hebt gemaakt (of die u voor u hebt gemaakt) kunt u het volgende doen:

* Toegang tot Jupyter, Jjupyterlab, RStudio op het reken exemplaar
* SSH naar Compute-instantie. SSH-toegang is standaard uitgeschakeld, maar kan worden ingeschakeld op het moment dat het reken proces wordt gemaakt. SSH-toegang is via het mechanisme voor open bare/persoonlijke sleutels. Op het tabblad krijgt u details over SSH-verbindingen zoals IP-adres, gebruikers naam en poort nummer.
* Details ophalen over een specifiek reken exemplaar, zoals het IP-adres en de regio.

---

Met [RBAC](/azure/role-based-access-control/overview) kunt u bepalen welke gebruikers in de werk ruimte een reken instantie kunnen maken, verwijderen, starten, stoppen en opnieuw starten. Alle gebruikers in de rol Inzender en eigenaar van de werk ruimte kunnen reken instanties maken, verwijderen, starten, stoppen en opnieuw starten in de werk ruimte. Maar alleen de maker van een specifiek reken exemplaar of de gebruiker die is toegewezen als deze namens hen is gemaakt, heeft toegang tot Jupyter, Jjupyterlab en RStudio op die reken instantie. Een reken instantie is toegewezen aan één gebruiker met hoofd toegang en kan worden terminal in via Jupyter/Jjupyterlab/RStudio. Reken instantie heeft aanmelding voor één gebruiker en alle acties gebruiken de identiteit van die gebruiker voor RBAC en de toewijzing van experimenten. SSH-toegang wordt beheerd via het mechanisme voor open bare/persoonlijke sleutels.

Deze acties kunnen worden beheerd door RBAC:
* *Micro soft. MachineLearningServices/werk ruimten/reken-en lees bewerkingen*
* *Micro soft. MachineLearningServices/werk ruimten/reken kracht/schrijven*
* *Micro soft. MachineLearningServices/werk ruimten/berekenen/verwijderen*
* *Micro soft. MachineLearningServices/werk ruimten/berekeningen/starten/actie*
* *Micro soft. MachineLearningServices/werk ruimten/berekeningen/stoppen/actie*
* *Micro soft. MachineLearningServices/werk ruimten/berekeningen/opnieuw opstarten/actie*


## <a name="access-the-terminal-window"></a>Het Terminal venster openen

Open het Terminal venster van uw reken exemplaar op een van de volgende manieren:

* RStudio: Selecteer het tabblad **Terminal** aan de rechter bovenhoek.
* Jupyter Lab: Selecteer de tegel **Terminal** onder de **andere** kop op het tabblad Start.
* Jupyter: Selecteer **nieuwe>Terminal** rechtsboven op het tabblad bestanden.
* SSH naar de computer als u SSH-toegang hebt ingeschakeld toen het reken exemplaar werd gemaakt.

Gebruik het Terminal venster om pakketten te installeren en extra kernels te maken.

## <a name="install-packages"></a>Pakketten installeren

U kunt pakketten rechtstreeks installeren in Jupyter Notebook of RStudio:

* RStudio gebruik het tabblad **pakketten** aan de rechter kant of op het tabblad **console** linksboven.  
* Python: Voeg een installatie code toe en voer deze uit in een Jupyter Notebook-cel.

U kunt ook installeren vanuit een Terminal venster. Installeer Python-pakketten in de **Python 3,6-AzureML-** omgeving.  R-pakketten installeren in de **R** -omgeving.

## <a name="add-new-kernels"></a>Nieuwe kernels toevoegen

> [!WARNING]
>  Zorg er tijdens het aanpassen van het reken proces voor dat u de **azureml_py36** Conda-omgeving of de **python 3,6-azureml-** kernel niet verwijdert. Dit is nodig voor de functionaliteit van Jupyter/Jjupyterlab

Een nieuwe Jupyter-kernel toevoegen aan het reken exemplaar:

1. Nieuwe terminal maken van het deel venster Jupyter, Jjupyterlab of van notitie blokken of SSH in het reken exemplaar
2. Gebruik het Terminal venster om een nieuwe omgeving te maken.  De onderstaande code maakt bijvoorbeeld `newenv` :

    ```shell
    conda create --name newenv
    ```

3. Activeer de omgeving.  Bijvoorbeeld na het maken van `newenv` :

    ```shell
    conda activate newenv
    ```

4. PIP-en ipykernel-pakket installeren in de nieuwe omgeving en een kernel maken voor die Conda env

    ```shell
    conda install pip
    conda install ipykernel
    python -m ipykernel install --user --name newenv --display-name "Python (newenv)"
    ```

Een van de [beschik bare Jupyter-kernels](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels) kan worden geïnstalleerd.



## <a name="next-steps"></a>Volgende stappen

* [Een trainings uitvoering verzenden](how-to-set-up-training-targets.md) 
