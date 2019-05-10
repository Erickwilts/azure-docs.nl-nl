---
title: Maken en gebruiken van compute-doelen voor modeltraining
titleSuffix: Azure Machine Learning service
description: Configureer de training-omgevingen (compute-doelen) voor machine learning-modeltraining. U kunt eenvoudig schakelen tussen omgevingen voor training. Start lokaal training. Als u nodig hebt om uit te schalen, kunt u overschakelen naar een cloud-gebaseerde compute-doel.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 01/07/2019
ms.custom: seodec18
ms.openlocfilehash: c49b9d5fdc0c17f16f1c80471a00dd53625dc6e8
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65236948"
---
# <a name="set-up-compute-targets-for-model-training"></a>Compute-doelen voor modeltraining instellen

Met Azure Machine Learning-service, kunt u uw model op een groot aantal bronnen of omgevingen, gezamenlijk aangeduid als trainen [ __compute-doelen__](concept-azure-machine-learning-architecture.md#compute-target). Een compute-doel is een lokale computer of een cloudresource, zoals een Azure Machine Learning-Computing, Azure HDInsight of een externe virtuele machine.  U kunt ook compute-doelen voor de implementatie van model maken zoals beschreven in ["waar en hoe u uw modellen implementeren '](how-to-deploy-and-where.md).

U kunt maken en beheren van een compute-doel met behulp van de SDK van Azure Machine Learning, Azure-portal of Azure CLI. Als u de compute-doelen die zijn gemaakt via een andere service (bijvoorbeeld: een HDInsight-cluster) hebt, kunt u ze kunt gebruiken door ze te koppelen aan uw werkruimte van Azure Machine Learning-service.
 
In dit artikel leert u hoe u met verschillende compute-doelen voor modeltraining.  De stappen voor alle compute-doelen Volg dezelfde werkstroom:
1. __Maak__ een compute-doel als u er nog geen hebt.
2. __Koppelen__ de compute-doel aan uw werkruimte.
3. __Configureer__ de compute-doel zodat deze het Python-omgeving en afhankelijkheden die nodig zijn voor uw script bevat.


>[!NOTE]
> Code in dit artikel is getest met Azure Machine Learning SDK versie 1.0.6.

## <a name="compute-targets-for-training"></a>COMPUTE-doelen voor training

Azure Machine Learning-service heeft verschillende ondersteuning voor verschillende compute-doelen. Een typische model ontwikkelingscyclus begint met dev/experimenten op een kleine hoeveelheid gegevens. In deze fase, wordt u aangeraden een lokale omgeving. Bijvoorbeeld, de lokale computer of een cloud-gebaseerde VM. Als u uw training voor grotere gegevenssets opschalen of gedistribueerde training doen, wordt u aangeraden een één of meerdere node cluster maken dat automatisch wordt geschaald telkens wanneer die u een uitvoering verzenden met Azure Machine Learning-Computing. U kunt ook uw eigen compute-resource koppelen, hoewel ondersteuning voor verschillende scenario's als variëren kunnen hieronder uitgelegd:


|COMPUTE-doel voor training| GPU-versnelling | Geautomatiseerd<br/> hyperparameter afstemmen | Geautomatiseerd</br> machine learning | Azure Machine Learning-pijplijnen |
|----|:----:|:----:|:----:|:----:|
|[Lokale computer](#local)| Misschien | &nbsp; | ✓ | &nbsp; |
|[Azure Machine Learning-Computing](#amlcompute)| ✓ | ✓ | ✓ | ✓ |
|[Externe virtuele machine](#vm) | ✓ | ✓ | ✓ | ✓ |
|[Azure Databricks](how-to-create-your-first-pipeline.md#databricks)| &nbsp; | &nbsp; | ✓ | ✓ |
|[Azure Data Lake Analytics](how-to-create-your-first-pipeline.md#adla)| &nbsp; | &nbsp; | &nbsp; | ✓ |
|[Azure HDInsight](#hdinsight)| &nbsp; | &nbsp; | &nbsp; | ✓ |
|[Azure Batch](#azbatch)| &nbsp; | &nbsp; | &nbsp; | ✓ |

**Alle compute-doelen kunnen worden hergebruikt voor meerdere trainingstaken**. Bijvoorbeeld, wanneer u een externe VM aan uw werkruimte koppelen, u deze opnieuw kunt gebruiken voor meerdere taken.

> [!NOTE]
> Azure Machine Learning-Computing kan worden gemaakt als een permanente resource of dynamisch gemaakt wanneer u een uitvoering aanvragen. Maken op basis van het uitvoeren van het Hiermee verwijdert u de compute-doel nadat de training-uitvoering voltooid, is zodat u niet opnieuw van compute-doelen op deze manier gemaakt gebruiken.

## <a name="whats-a-run-configuration"></a>Wat is een configuratie uitvoeren?

Bij het trainen, is het gebruikelijk dat start op uw lokale computer en later deze trainingsscript uitvoeren op een andere compute-doel. Met Azure Machine Learning-service, kunt u uw script uitvoeren op verschillende compute-doelen zonder te hoeven wijzigen van uw script. 

U hoeft alleen is definiëren van de omgeving voor elke compute-doel met een **uitvoerconfiguratie**.  Als u uw trainingsexperiment uitvoeren op een andere compute-doel wilt, geef vervolgens de configuratie van de uitvoering voor die compute. 

Meer informatie over [experimenten verzenden](#submit) aan het einde van dit artikel.

### <a name="manage-environment-and-dependencies"></a>Beheer van omgeving en afhankelijkheden

Wanneer u een uitvoerconfiguratieprofiel maakt, moet u bepalen hoe u voor het beheren van de omgeving en de afhankelijkheden van de compute-doel. 

#### <a name="system-managed-environment"></a>Systeem-beheerde omgeving

Gebruik van een systeem-beheerde omgeving als u wilt [Conda](https://conda.io/docs/) voor het beheren van de Python-omgeving en de Scriptafhankelijkheden voor u. Een systeem-beheerde omgeving wordt aangenomen dat door de standaard- en de meest voorkomende keuze. Het is nuttig op externe compute-doelen, vooral wanneer u die zijn gericht niet configureren. 

U hoeft alleen is geeft u elk pakket afhankelijkheid met de [CondaDependency klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py) vervolgens Conda maakt u een bestand met de naam **conda_dependencies.yml** in de **aml_config** de map in uw werkruimte met de lijst met pakketafhankelijkheden en stelt u de Python-omgeving wanneer u uw trainingsexperiment verzendt. 

De eerste installatie van een nieuwe omgeving kan enige tijd duren, afhankelijk van de grootte van de vereiste afhankelijkheden. Zolang de lijst met pakketten ongewijzigd blijft, wordt de insteltijd slechts één keer uitgevoerd.
  
De volgende code toont een voorbeeld van een systeem-beheerde omgeving vereisen scikit-informatie:
    
[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/runconfig.py?name=run_system_managed)]

#### <a name="user-managed-environment"></a>Gebruiker-beheerde omgeving

Voor een gebruiker beheerde omgeving bent u verantwoordelijk voor het instellen van uw omgeving en het installeren van elk pakket uw trainingsscript moet op de compute-doel. Als uw omgeving instrueren al is geconfigureerd (zoals op uw lokale computer), kunt u de setup-stap overslaan door in te stellen `user_managed_dependencies` op ' True '. Conda wordt niet controleren van uw omgeving of iets voor u installeren.

De volgende code toont een voorbeeld van het configureren van trainingsuitvoeringen voor een gebruiker beheerde omgeving:

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/runconfig.py?name=run_user_managed)]
  
## <a name="set-up-compute-targets-with-python"></a>Instellen van compute-doelen met Python

Gebruik de onderstaande secties voor het configureren van deze compute-doelen:

* [Lokale computer](#local)
* [Azure Machine Learning-Computing](#amlcompute)
* [Externe virtuele machines](#vm)
* [Azure HDInsight](#hdinsight)


### <a id="local"></a>Lokale computer

1. **Maak en koppel**: Er is niet nodig om te maken of koppelen van een compute-doel voor het gebruik van uw lokale computer als de trainingsomgeving.  

1. **Configureren**:  Wanneer u uw lokale computer als een compute-doel gebruikt, de training-code wordt uitgevoerd uw [ontwikkelomgeving](how-to-configure-environment.md).  Als deze omgeving al de Python-pakketten die u nodig hebt heeft, gebruikt u de gebruiker beheerde omgeving.

 [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=run_local)]

Nu dat u hebt de berekening die is gekoppeld en geconfigureerd uw uitvoering, de volgende stap is het [verzenden van de uitvoering van de training](#submit).

### <a id="amlcompute"></a>Azure Machine Learning-Computing

Azure Machine Learning-Computing is een beheerde-compute-infrastructuur waarmee de gebruiker eenvoudig een rekenkracht van één of meerdere knooppunten te maken. De berekening die wordt gemaakt binnen de regio van uw werkruimte als een resource die kan worden gedeeld met andere gebruikers in uw werkruimte. De berekening die automatisch wordt geschaald als een taak wordt verzonden en kan worden geplaatst in een Azure-netwerk. De berekening die wordt uitgevoerd in een omgeving met beperkte en de afhankelijkheden van uw model in pakketten een [Docker-container](https://www.docker.com/why-docker).

U kunt Azure Machine Learning-Computing gebruiken voor het distribueren van het trainingsproces in een cluster van de CPU of GPU-computerknooppunten in de cloud. Zie voor meer informatie over de VM-grootten die GPU's omvatten [GPU-geoptimaliseerde VM-grootten](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu).

Azure Machine Learning-Computing heeft standaardlimieten, zoals het aantal kernen dat kan worden toegewezen. Zie voor meer informatie, [beheren en aanvraag quota's voor Azure-resources](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-quotas).


U kunt een Azure Machine Learning-rekenomgeving maken op aanvraag wanneer u plant een uitvoering, of als een permanente resource.

#### <a name="run-based-creation"></a>Uitvoeren op basis van het maken

U kunt Azure Machine Learning-Computing maken als een compute-doel tijdens runtime. De berekening wordt automatisch gemaakt voor de uitvoering. De berekening wordt automatisch verwijderd nadat de uitvoering is voltooid. 

> [!NOTE]
> Als u het maximale aantal knooppunten te gebruiken, moet u normaal gesproken instellen `node_count` aan het aantal knooppunten. Er is momenteel (04/04/2019) een bug waarmee wordt voorkomen dit niet werkt dat. Als tijdelijke oplossing, gebruikt u de `amlcompute._cluster_max_node_count` eigenschap van de configuratie van de uitvoering. Bijvoorbeeld `run_config.amlcompute._cluster_max_node_count = 5`.

> [!IMPORTANT]
> Uitvoeren op basis van het maken van Azure Machine Learning-Computing is momenteel in Preview. Gebruik niet maken op basis van een uitvoeren als u automatische hyperparameter afstemmen of automatische leerprocessen. Als u wilt gebruiken hyperparameter afstemmen of geautomatiseerde voor machine learning, een [permanente compute](#persistent) in plaats daarvan richten.

1.  **Maken, koppelen en configureren van**: Het maken van de uitvoeren op basis van voert de benodigde stappen voor het maken, koppelen en configureren van de compute-doel met de configuratie van de uitvoering.  

  [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute.py?name=run_temp_compute)]


Nu dat u hebt de berekening die is gekoppeld en geconfigureerd uw uitvoering, de volgende stap is het [verzenden van de uitvoering van de training](#submit).

#### <a id="persistent"></a>Permanente compute

Een permanente Azure Machine Learning-Computing kunnen worden hergebruikt binnen taken. De berekening die kan worden gedeeld met andere gebruikers in de werkruimte en tussen taken wordt bewaard.

1. **Maak en koppel**: Voor het maken van een permanente resource van Azure Machine Learning-Computing in Python, geef de **vm_size** en **max_nodes** eigenschappen. Azure Machine Learning maakt vervolgens gebruik van slimme standaardinstellingen voor de overige eigenschappen. De compute automatisch wordt geschaald naar nul knooppunten wanneer deze wordt niet gebruikt.   Toegewezen virtuele machines worden gemaakt voor het uitvoeren van uw taken, indien nodig.
    
    * **vm_size**: De VM-reeks van de knooppunten die zijn gemaakt door Azure Machine Learning-Computing.
    * **max_nodes**: Het maximale aantal knooppunten automatisch te schalen tot wanneer u een taak op Azure Machine Learning-Computing uitvoert.
    
   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute2.py?name=cpu_cluster)]

   U kunt ook verschillende geavanceerde eigenschappen configureren bij het maken van Azure Machine Learning-Computing. De eigenschappen kunnen u een permanente cluster vaste grootte of binnen een bestaand virtueel Azure-netwerk maken in uw abonnement.  Zie de [AmlCompute klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py
    ) voor meer informatie.
    
   Of u kunt maken en koppelen van een permanente resource van Azure Machine Learning-Computing [in Azure portal](#portal-create).

1. **Configureren**: Maak een uitvoeren-configuratie voor de permanente compute-doel.

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute2.py?name=run_amlcompute)]

Nu dat u hebt de berekening die is gekoppeld en geconfigureerd uw uitvoering, de volgende stap is het [verzenden van de uitvoering van de training](#submit).


### <a id="vm"></a>Externe virtuele machines

Azure Machine Learning ondersteunt ook uw eigen compute-resource halen en deze te koppelen aan uw werkruimte. Een dergelijke resourcetype is een willekeurige externe virtuele machine, zolang deze toegankelijk is vanaf de Azure Machine Learning-service. De bron kan bestaan uit een Azure-VM, een externe server in uw organisatie, of on-premises. Specifiek, kunt gezien het IP-adres en referenties (gebruikersnaam en wachtwoord of SSH-sleutel), u elke VM die toegankelijk is voor extern worden uitgevoerd.

U kunt een systeem gebouwd conda-omgeving, een al bestaande Python-omgeving of een Docker-container. Als u wilt uitvoeren op een Docker-container, moet u een Docker-Engine die wordt uitgevoerd op de virtuele machine hebben. Deze functionaliteit is vooral nuttig als u wilt dat een meer flexibele, cloud-gebaseerde dev/experimentele omgeving dan uw lokale computer.

Gebruik de Azure Data Science Virtual Machine (DSVM) als de Azure-VM van keuze voor dit scenario. Deze virtuele machine is een vooraf geconfigureerde datatechnologie en AI-ontwikkelomgeving in Azure. De virtuele machine biedt een gecureerde ruime keuze aan hulpprogramma's en frameworks voor volledige-levenscyclus van machine learning-ontwikkeling. Zie voor meer informatie over het gebruik van de DSVM met Azure Machine Learning [een ontwikkelomgeving configureren](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-environment#dsvm).

1. **Maak**: Maak een DSVM voordat u deze gebruikt met het trainen van uw model. Zie voor het maken van deze resource [inrichten van de virtuele Machine voor Datatechnologie voor Linux (Ubuntu)](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro).

    > [!WARNING]
    > Azure Machine Learning biedt alleen ondersteuning voor virtuele machines met Ubuntu. Wanneer u een virtuele machine maken of kies een bestaande virtuele machine, moet u een virtuele machine die gebruikmaakt van Ubuntu.

1. **Koppelen**: Als u wilt koppelen van een bestaande virtuele machine als een compute-doel, moet u de volledig gekwalificeerde domeinnaam (FQDN), de gebruikersnaam en het wachtwoord opgeven voor de virtuele machine. Vervang in het voorbeeld \<FQDN-naam > met de openbare FQDN-naam van de virtuele machine of het openbare IP-adres. Vervang \<username > en \<wachtwoord > met de SSH-gebruikersnaam en het wachtwoord voor de virtuele machine.

   ```python
   from azureml.core.compute import RemoteCompute, ComputeTarget

   # Create the compute config 
   compute_target_name = "attach-dsvm"
   attach_config = RemoteCompute.attach_configuration(address = "<fqdn>",
                                                    ssh_port=22,
                                                    username='<username>',
                                                    password="<password>")

   # If you authenticate with SSH keys instead, use this code:
   #                                                  ssh_port=22,
   #                                                  username='<username>',
   #                                                  password=None,
   #                                                  private_key_file="<path-to-file>",
   #                                                  private_key_passphrase="<passphrase>")

   # Attach the compute
   compute = ComputeTarget.attach(ws, compute_target_name, attach_config)

   compute.wait_for_completion(show_output=True)
   ```

   Of u kunt de DSVM koppelen aan uw werkruimte [met behulp van de Azure-portal](#portal-reuse).

1. **Configureren**: Maak een uitvoeren-configuratie voor de DSVM compute-doel. Docker en conda worden gebruikt voor het maken en configureren van de omgeving instrueren op de DSVM.

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/dsvm.py?name=run_dsvm)]


Nu dat u hebt de berekening die is gekoppeld en geconfigureerd uw uitvoering, de volgende stap is het [verzenden van de uitvoering van de training](#submit).

### <a id="hdinsight"></a>Azure HDInsight 

Azure HDInsight is een populair platform voor big data-analyses. Het platform biedt een Apache Spark, die kunnen worden gebruikt om uw model te trainen.

1. **Maak**:  Het HDInsight-cluster maken voordat u deze gebruiken om uw model te trainen. Zie voor informatie over het maken van een Spark op HDInsight-cluster [maken van een Spark-Cluster in HDInsight](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql). 

    Wanneer u het cluster maakt, moet u een SSH-gebruikersnaam en wachtwoord opgeven. Noteer deze waarden als dat nodig HDInsight gebruiken als een compute-doel.
    
    Nadat het cluster is gemaakt, verbinding maken met de hostnaam \<clustername >-ssh.azurehdinsight.net, waar \<clustername > is de naam die u hebt opgegeven voor het cluster. 

1. **Koppelen**: Als u wilt koppelen als een compute-doel in een HDInsight-cluster, moet u de hostnaam, de gebruikersnaam en het wachtwoord opgeven voor het HDInsight-cluster. Het volgende voorbeeld wordt de SDK te koppelen van een cluster aan uw werkruimte. Vervang in het voorbeeld \<clustername > met de naam van uw cluster. Vervang \<username > en \<wachtwoord > met de SSH-gebruikersnaam en het wachtwoord voor het cluster.

   ```python
   from azureml.core.compute import ComputeTarget, HDInsightCompute
   from azureml.exceptions import ComputeTargetException

   try:
    # if you want to connect using SSH key instead of username/password you can provide parameters private_key_file and private_key_passphrase
    attach_config = HDInsightCompute.attach_configuration(address='<clustername>-ssh.azureinsight.net', 
                                                          ssh_port=22, 
                                                          username='<ssh-username>', 
                                                          password='<ssh-pwd>')
    hdi_compute = ComputeTarget.attach(workspace=ws, 
                                       name='myhdi', 
                                       attach_configuration=attach_config)

   except ComputeTargetException as e:
    print("Caught = {}".format(e.message))

   hdi_compute.wait_for_completion(show_output=True)
   ```

   Of u kunt de HDInsight-cluster koppelen aan uw werkruimte [met behulp van de Azure-portal](#portal-reuse).

1. **Configureren**: Maak een uitvoeren-configuratie voor het HDI-compute-doel. 

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/hdi.py?name=run_hdi)]


Nu dat u hebt de berekening die is gekoppeld en geconfigureerd uw uitvoering, de volgende stap is het [verzenden van de uitvoering van de training](#submit).


### <a id="azbatch"></a>Azure Batch 

Azure Batch is gebruikt voor het uitvoeren van grootschalige parallelle en high performance computing (HPC) toepassingen efficiënt in de cloud. AzureBatchStep kan worden gebruikt in een Azure Machine Learning-pijplijn voor het verzenden van taken naar een Azure Batch-pool van machines.

Als u wilt koppelen Azure Batch als een compute-doel, moet u de Azure Machine Learning-SDK gebruiken en geef de volgende informatie:

-   **Azure Batch-rekenknooppunt naam**: Een beschrijvende naam moet worden gebruikt voor de berekening die in de werkruimte
-   **Azure Batch-accountnaam**: De naam van de Azure Batch-account
-   **Resourcegroep**: De resourcegroep met de Azure Batch-account.

De volgende code ziet u hoe u Azure Batch als een compute-doel toevoegen:

```python
from azureml.core.compute import ComputeTarget, BatchCompute
from azureml.exceptions import ComputeTargetException

batch_compute_name = 'mybatchcompute' # Name to associate with new compute in workspace

# Batch account details needed to attach as compute to workspace
batch_account_name = "<batch_account_name>" # Name of the Batch account
batch_resource_group = "<batch_resource_group>" # Name of the resource group which contains this account

try:
    # check if the compute is already attached
    batch_compute = BatchCompute(ws, batch_compute_name)
except ComputeTargetException:
    print('Attaching Batch compute...')
    provisioning_config = BatchCompute.attach_configuration(resource_group=batch_resource_group, account_name=batch_account_name)
    batch_compute = ComputeTarget.attach(ws, batch_compute_name, provisioning_config)
    batch_compute.wait_for_completion()
    print("Provisioning state:{}".format(batch_compute.provisioning_state))
    print("Provisioning errors:{}".format(batch_compute.provisioning_errors))

print("Using Batch compute:{}".format(batch_compute.cluster_resource_id))
```

## <a name="set-up-compute-in-the-azure-portal"></a>Compute in Azure portal instellen

U kunt toegang tot de compute-doelen die gekoppeld aan uw werkruimte in de Azure-portal zijn.  U kunt de portal om te gebruiken:

* [Weergave van compute-doelen](#portal-view) die is gekoppeld aan uw werkruimte
* [Maken van een compute-doel](#portal-create) in uw werkruimte
* [Koppelen van een compute-doel](#portal-reuse) die buiten de werkruimte is gemaakt

Nadat een doel is gemaakt en gekoppeld aan uw werkruimte, gebruikt u dit in uw configuratie uitvoeren met een `ComputeTarget` object: 

```python
from azureml.core.compute import ComputeTarget
myvm = ComputeTarget(workspace=ws, name='my-vm-name')
```

### <a id="portal-view"></a>Compute-doelen weergeven


Als de compute-doelen voor uw werkruimte weergeven, gebruikt u de volgende stappen uit:

1. Navigeer naar de [Azure-portal](https://portal.azure.com) en opent u uw werkruimte. 
1. Onder __toepassingen__, selecteer __Compute__.

    ![Compute-tabblad weergave](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace.png)

### <a id="portal-create"></a>Een compute-doel maken

Volg de vorige stappen om de lijst met compute-doelen weer te geven. Gebruik vervolgens deze stappen om een compute-doel te maken: 

1. Selecteer het plusteken (+) om toe te voegen een compute-doel.

    ![Een compute-doel toevoegen](./media/how-to-set-up-training-targets/add-compute-target.png) 

1. Voer een naam voor de compute-doel. 

1. Selecteer **Machine Learning-Computing** als het type compute moet worden gebruikt voor __Training__. 

    >[!NOTE]
    >Azure Machine Learning-Computing is de in Azure portal kunt u alleen beheerde compute-resource.  Alle andere compute-resources kunnen worden gekoppeld, nadat ze zijn gemaakt.

1. Vul het formulier. Geef waarden op voor de vereiste eigenschappen, met name **VM-reeks**, en de **maximum aantal knooppunten** gebruiken instellen de rekenkracht.  

    ![Vul het formulier in](./media/how-to-set-up-training-targets/add-compute-form.png) 

1. Selecteer __Maken__.


1. De status van de maakbewerking weergeven door de compute-doel selecteren in de lijst:

    ![Selecteer een compute-doel om weer te geven van de status van de bewerking maken](./media/how-to-set-up-training-targets/View_list.png)

1. Vervolgens ziet u de details voor de compute-doel: 

    ![Bekijk de details van de doel-computer](./media/how-to-set-up-training-targets/compute-target-details.png) 



### <a id="portal-reuse"></a>Compute-doelen koppelen

Voor het gebruik van compute-doelen die buiten de werkruimte van de Azure Machine Learning-service zijn gemaakt, moet u ze koppelt. Bezig met koppelen van een compute-doel, maakt ze beschikbaar voor uw werkruimte.

Volg de stappen hierboven om de lijst met compute-doelen weer te geven. Gebruik vervolgens de volgende stappen uit om te koppelen van een compute-doel: 

1. Selecteer het plusteken (+) om toe te voegen een compute-doel. 
1. Voer een naam voor de compute-doel. 
1. Selecteer het type compute koppelen voor __Training__:

    > [!IMPORTANT]
    > Niet alle compute-typen kunnen worden gekoppeld via de Azure-portal. De compute-typen die op dit moment kunnen worden gekoppeld voor training onder meer:
    >
    > * Een externe virtuele machine
    > * Azure Databricks (voor gebruik in machine learning-pijplijnen)
    > * Azure Data Lake Analytics (voor gebruik in machine learning-pijplijnen)
    > * Azure HDInsight

1. Vul het formulier in en geef waarden op voor de vereiste eigenschappen.

    > [!NOTE]
    > Microsoft raadt u SSH-sleutels veiliger dan wachtwoorden zijn te gebruiken. Wachtwoorden zijn kwetsbaar voor beveiligingsaanvallen. SSH-sleutels zijn, is afhankelijk van cryptografische handtekeningen. Zie de volgende documenten voor informatie over het maken van SSH-sleutels voor gebruik met Azure Virtual Machines:
    >
    > * [Maken en gebruiken van SSH-sleutels in Linux of macOS](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Maken en gebruiken van SSH-sleutels op Windows](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

1. Selecteer __koppelen__. 
1. Bekijk de status van de koppelingsbewerking door het selecteren van de compute-doel in de lijst.

## <a name="set-up-compute-with-the-cli"></a>Computing met de CLI instellen

U hebt toegang tot de compute-doelen die gekoppeld aan uw werkruimte met zijn de [CLI-extensie](reference-azure-machine-learning-cli.md) voor Azure Machine Learning-service.  U kunt de CLI te gebruiken:

* Maken van een beheerde compute-doel
* Een beheerde compute-doel bijwerken
* Een niet-beheerde compute-doel toevoegen

Zie voor meer informatie, [resourcebeheer](reference-azure-machine-learning-cli.md#resource-management).

## <a id="submit"></a>Indienen van training uitvoeren

Nadat u een uitvoerconfiguratieprofiel gemaakt, kunt u deze gebruiken om uit te voeren van uw experiment.  De code-patroon om in te dienen een training-uitvoering is hetzelfde voor alle typen compute-doelen:

1. Maken van een experiment uitvoeren
1. De uitvoering verzenden.
1. Wacht totdat de uitvoering om te voltooien.

> [!IMPORTANT]
> Wanneer u de uitvoering van de training verzendt, wordt een momentopname van de map waarin uw trainingsscripts gemaakt en verzonden naar de compute-doel. Dit wordt ook opgeslagen als onderdeel van het experiment in uw werkruimte. Als u bestanden wijzigen en het verzenden van de uitvoering opnieuw, alleen de gewijzigde bestanden worden geüpload.
>
> Om te voorkomen dat bestanden worden opgenomen in de momentopname maken van een [.gitignore](https://git-scm.com/docs/gitignore) of `.amlignore` -bestand in de map en de bestanden toevoegen. De `.amlignore` bestand maakt gebruik van dezelfde syntaxis en patronen als de [.gitignore](https://git-scm.com/docs/gitignore) bestand. Als beide bestanden bestaat, de `.amlignore` bestand heeft voorrang.
> 
> Zie voor meer informatie, [momentopnamen](concept-azure-machine-learning-architecture.md#snapshot).

### <a name="create-an-experiment"></a>Een experiment maken

Maak eerst een experiment in uw werkruimte.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=experiment)]

### <a name="submit-the-experiment"></a>Het experiment verzenden

Verzenden van het experiment met een `ScriptRunConfig` object.  Dit object bevat de:

* **bronmap**: De bronmap die met het trainingsscript
* **script**: Het trainingsscript identificeren
* **run_config**: De uitvoering configuratie, die op zijn beurt definieert waar de training wordt uitgevoerd.

Bijvoorbeeld, gebruik [het lokale doel](#local) configuratie:

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=local_submit)]

Overschakelen van het hetzelfde experiment om uit te voeren in een andere compute-doel met behulp van een andere configuratie uitvoeren, zoals de [amlcompute doel](#amlcompute):

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute2.py?name=amlcompute_submit)]

Of u kunt:

* Verzenden van het experiment met een `Estimator` object, zoals wordt weergegeven in [Train ML-modellen met loopt](how-to-train-ml-models.md). 
* Een experiment verzenden [met behulp van de CLI-extensie](reference-azure-machine-learning-cli.md#experiments).

## <a name="notebook-examples"></a>Laptop-voorbeelden

Zie deze laptops voor voorbeelden van training met diverse compute-doelen:
* [procedure-naar-gebruik-azureml/training](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)
* [zelfstudies/img-classificatie-deel 1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Volgende stappen

* [Zelfstudie: Een model te trainen](tutorial-train-models-with-aml.md) maakt gebruik van een beheerde compute-doel voor een model te trainen.
* Meer informatie over het [efficiënt afstemmen van hyperparameters](how-to-tune-hyperparameters.md) om betere modellen te bouwen.
* Zodra u een getraind model hebt, meer [hoe en waar u kunt modellen implementeren](how-to-deploy-and-where.md).
* Weergave de [RunConfiguration klasse](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) SDK-referentie.
* [Azure Machine Learning-service gebruiken met Azure Virtual Networks](how-to-enable-virtual-network.md)
