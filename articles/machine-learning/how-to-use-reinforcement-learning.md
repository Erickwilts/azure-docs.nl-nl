---
title: Train en implementeer een versterkt leer model (preview).
titleSuffix: Azure Machine Learning
description: Meer informatie over het gebruik van Azure Machine Learning versterking (preview) om een RL-agent te trainen om Pong af te spelen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: peterlu
author: peterclu
ms.date: 05/05/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python
ms.openlocfilehash: 0a043bf79818480e045fb1f82640639fd7dcb232
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2020
ms.locfileid: "87847632"
---
# <a name="reinforcement-learning-preview-with-azure-machine-learning"></a>Educatief leren (preview) met Azure Machine Learning

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

> [!NOTE]
> Azure Machine Learning versterking van het onderwijs is momenteel een preview-functie. Op dit moment worden alleen Ray-en RLlib-Frameworks ondersteund.

In dit artikel leert u hoe u een RL-agent (versterking leren) kunt trainen om de Pong van het video spel af te spelen. U maakt gebruik van de open-source python-bibliotheek van de [RLlib](https://ray.readthedocs.io/en/master/rllib.html) met Azure machine learning om de complexiteit van gedistribueerde RL-taken te beheren.

In dit artikel leert u het volgende:
> [!div class="checklist"]
> * Een experiment instellen
> * Hoofd-en worker-knoop punten definiëren
> * Een RL-Estimator maken
> * Een experiment verzenden om een uitvoering te starten
> * Resultaten weergeven

Dit artikel is gebaseerd op het [RLlib Pong-voor beeld](https://aka.ms/azureml-rl-pong) dat u kunt vinden in de [github-opslag plaats](https://aka.ms/azureml-rl-notebooks)van Azure machine learning notebook.

## <a name="prerequisites"></a>Vereisten

Voer deze code uit in een van de volgende omgevingen. U wordt aangeraden om Azure Machine Learning Reken instantie uit te voeren voor de snelste opstart ervaring. De beproefde voorbeeld notitieblokken zijn beschikbaar om snel te klonen en uit te voeren op Azure Machine Learning Compute-instantie.

 - Azure Machine Learning-rekeninstantie

     - Meer informatie over het klonen van voorbeeld notitieblokken in [zelf studie: installatie omgeving en werk ruimte](tutorial-1st-experiment-sdk-setup.md).
         - De map **How-to-use-azureml** klonen in plaats van **zelf studies**
     - Voer de installatie notitieblok van het virtuele netwerk dat zich bevindt bij `/how-to-use-azureml/reinforcement-learning/setup/devenv_setup.ipynb` om netwerk poorten te openen die worden gebruikt voor gedistribueerde versterking van het onderwijs.
     - Het voorbeeld notitieblok uitvoeren`/how-to-use-azureml/reinforcement-learning/atari-on-distributed-compute/pong_rllib.ipynb`
 
 - Uw eigen Jupyter Notebook-server

    - Installeer de [Azure machine learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).
    - Installeer de [Azure machine learning RL SDK](https://docs.microsoft.com/python/api/azureml-contrib-reinforcementlearning/?view=azure-ml-py):`pip install --upgrade azureml-contrib-reinforcementlearning`
    - Maak een [configuratie bestand voor de werk ruimte](how-to-configure-environment.md#workspace).
    - Voer de [installatie notitieblok](https://aka.ms/azure-rl-env-setup) van het virtuele netwerk uit om de netwerk poorten te openen die worden gebruikt voor Distributed versterking learning.


## <a name="how-to-train-a-pong-playing-agent"></a>Een Pong-agent trainen

RL (versterking leren) is een benadering van machine learning die u kunt leren. Hoewel andere machine learning technieken leren door het bewaakt passief van het invoeren van gegevens en het vinden van patronen erin, worden met RL **trainings agenten** gebruikt om actief beslissingen te nemen en te leren van hun resultaten.

Uw trainings medewerkers leren Pong in een **gesimuleerde omgeving**te spelen. Trainings medewerkers maken elk frame van het spel een beslissing om de verwerker omhoog, omlaag of blijvend te verplaatsen. Er wordt gekeken naar de status van het spel (een RGB-afbeelding van het scherm) om een beslissing te nemen.

RL maakt gebruik van **beloningen** om de agent te informeren als de beslissingen succesvol zijn. In deze omgeving krijgt de agent een positieve beloning wanneer hij een punt en een negatieve beloning afgeeft wanneer er een punt wordt gescoord. De trainings agent leert veel herhalingen om de actie te kiezen op basis van de huidige status, die de som van de verwachte toekomstige beloningen optimaliseert.

Het is gebruikelijk om een DNN-model ( **diepe Neural Network** ) te gebruiken om deze optimalisatie uit te voeren in RL. In eerste instantie wordt de learning-agent slecht uitgevoerd, maar elk spel genereert extra voor beelden om het model verder te verbeteren.

De training loopt af wanneer de agent een gemiddelde belonings Score van 18 in een trainings-epoche bereikt. Dit betekent dat de onderdoen zijn opruiming met een gemiddelde van ten minste 18 punten in overeenkomsten van Maxi maal 21.

Het proces voor het door lopen van simulatie en het opnieuw trainen van een DNN is rekenbaar kostbaar en vereist grote hoeveel heden gegevens. Een van de manieren om de prestaties van RL-taken te verbeteren, is door **gelijktijdig werk** , zodat meerdere mede werkers gelijktijdig kunnen handelen en leren. Het beheer van een gedistribueerde RL-omgeving kan echter een ingewikkelde onderneming zijn.

Azure Machine Learning biedt het Framework voor het beheren van deze gecompliceerdies om uw RL-workloads uit te schalen.

## <a name="set-up-the-environment"></a>De omgeving instellen

Stel de lokale RL-omgeving in door de vereiste Python-pakketten te laden, uw werk ruimte te initialiseren, een experiment te maken en een geconfigureerd virtueel netwerk op te geven.

### <a name="import-libraries"></a>Bibliotheken importeren

Importeer de benodigde Python-pakketten om de rest van dit voor beeld uit te voeren.

```python
# Azure ML Core imports
import azureml.core
from azureml.core import Workspace
from azureml.core import Experiment
from azureml.core.compute import AmlCompute
from azureml.core.compute import ComputeTarget
from azureml.core.runconfig import EnvironmentDefinition
from azureml.widgets import RunDetails
from azureml.tensorboard import Tensorboard

# Azure ML Reinforcement Learning imports
from azureml.contrib.train.rl import ReinforcementLearningEstimator, Ray
from azureml.contrib.train.rl import WorkerConfiguration
```

### <a name="initialize-a-workspace"></a>Een werk ruimte initialiseren

De [Azure machine learning werk ruimte](concept-workspace.md) is de resource op het hoogste niveau voor Azure machine learning. Het biedt u een centrale locatie voor het werken met alle artefacten die u maakt.

Initialiseer een werkruimte object uit het `config.json` bestand dat in de [sectie vereisten](#prerequisites)is gemaakt. Als u deze code uitvoert in een Azure Machine Learning Compute-exemplaar, is het configuratie bestand al voor u gemaakt.

```Python
ws = Workspace.from_config()
```

### <a name="create-a-reinforcement-learning-experiment"></a>Een studie experiment voor versterking maken

Maak een [experiment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py) om uw leer proces voor versterking te volgen. In Azure Machine Learning zijn experimenten logische verzamelingen van gerelateerde experimenten om uitvoerings logboeken, geschiedenis, uitvoer en meer te organiseren.

```python
experiment_name='rllib-pong-multi-node'

exp = Experiment(workspace=ws, name=experiment_name)
```

### <a name="specify-a-virtual-network"></a>Een virtueel netwerk opgeven

Voor RL-taken die gebruikmaken van meerdere Compute-doelen moet u een virtueel netwerk met open poorten opgeven waarmee werk knooppunten en hoofd knooppunten met elkaar kunnen communiceren. Het virtuele netwerk kan zich in elke resource groep bevinden, maar moet zich in dezelfde regio bevinden als uw werk ruimte. Zie voor meer informatie over het instellen van uw virtuele netwerk het [notitie blok](https://aka.ms/azure-rl-env-setup) voor het instellen van de werk ruimte die u kunt vinden in de sectie vereisten. Hier geeft u de naam op van het virtuele netwerk in de resource groep.

```python
vnet = 'your_vnet'
```

## <a name="define-head-and-worker-compute-targets"></a>Doelen voor hoofd-en werk nemers definiëren

In dit voor beeld worden afzonderlijke reken doelen gebruikt voor de knoop punten Ray Head en Workers. Met deze instellingen kunt u uw reken resources omhoog en omlaag schalen, afhankelijk van de verwachte werk belasting. Stel het aantal knoop punten en de grootte van elk knoop punt in op basis van de behoeften van uw experiment.

### <a name="head-computing-target"></a>Doel computing

In dit voor beeld wordt een met GPU uitgeruste hoofd cluster gebruikt om de prestaties van diepe learning te optimaliseren. Het hoofd knooppunt traint het Neural-netwerk dat de agent gebruikt om beslissingen te nemen. Het hoofd knooppunt verzamelt ook gegevens punten van de worker-knoop punten om het Neural-netwerk verder te trainen.

De Head Compute maakt gebruik van één [ `STANDARD_NC6` virtuele machine](https://docs.microsoft.com/azure/virtual-machines/nc-series) (VM). Het heeft zes virtuele Cpu's, wat betekent dat IT werk kan verdelen over 6 werk Cpu's.


```python
from azureml.core.compute import AmlCompute, ComputeTarget

# choose a name for the Ray head cluster
head_compute_name = 'head-gpu'
head_compute_min_nodes = 0
head_compute_max_nodes = 2

# This example uses GPU VM. For using CPU VM, set SKU to STANDARD_D2_V2
head_vm_size = 'STANDARD_NC6'

if head_compute_name in ws.compute_targets:
    head_compute_target = ws.compute_targets[head_compute_name]
    if head_compute_target and type(head_compute_target) is AmlCompute:
        print(f'found head compute target. just use it {head_compute_name}')
else:
    print('creating a new head compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(vm_size = head_vm_size,
                                                                min_nodes = head_compute_min_nodes, 
                                                                max_nodes = head_compute_max_nodes,
                                                                vnet_resourcegroup_name = ws.resource_group,
                                                                vnet_name = vnet_name,
                                                                subnet_name = 'default')

    # create the cluster
    head_compute_target = ComputeTarget.create(ws, head_compute_name, provisioning_config)
    
    # can poll for a minimum number of nodes and for a specific timeout. 
    # if no min node count is provided it will use the scale settings for the cluster
    head_compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current AmlCompute status, use get_status()
    print(head_compute_target.get_status().serialize())
```

[!INCLUDE [low-pri-note](../../includes/machine-learning-low-pri-vm.md)]

### <a name="worker-computing-cluster"></a>Worker Computing Cluster

In dit voor beeld worden vier [ `STANDARD_D2_V2` virtuele machines](https://docs.microsoft.com/azure/virtual-machines/nc-series) gebruikt voor het reken doel van de werk nemer. Elk worker-knoop punt heeft twee beschik bare Cpu's voor een totaal van 8 beschik bare Cpu's om parallelliseren te kunnen gebruiken.

Er zijn geen Gpu's nodig voor de worker-knoop punten, omdat ze geen diep gaande training uitvoeren. De werk nemers voeren Game simulaties uit en verzamelen gegevens.

```python
# choose a name for your Ray worker cluster
worker_compute_name = 'worker-cpu'
worker_compute_min_nodes = 0 
worker_compute_max_nodes = 4

# This example uses CPU VM. For using GPU VM, set SKU to STANDARD_NC6
worker_vm_size = 'STANDARD_D2_V2'

# Create the compute target if it hasn't been created already
if worker_compute_name in ws.compute_targets:
    worker_compute_target = ws.compute_targets[worker_compute_name]
    if worker_compute_target and type(worker_compute_target) is AmlCompute:
        print(f'found worker compute target. just use it {worker_compute_name}')
else:
    print('creating a new worker compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(vm_size = worker_vm_size,
                                                                min_nodes = worker_compute_min_nodes, 
                                                                max_nodes = worker_compute_max_nodes,
                                                                vnet_resourcegroup_name = ws.resource_group,
                                                                vnet_name = vnet_name,
                                                                subnet_name = 'default')

    # create the cluster
    worker_compute_target = ComputeTarget.create(ws, worker_compute_name, provisioning_config)
    
    # can poll for a minimum number of nodes and for a specific timeout. 
    # if no min node count is provided it will use the scale settings for the cluster
    worker_compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current AmlCompute status, use get_status()
    print(worker_compute_target.get_status().serialize())
```

## <a name="create-a-reinforcement-learning-estimator"></a>Een Estimator voor versterking leren maken

In deze sectie leert u hoe u [ReinforcementLearningEstimator](https://docs.microsoft.com/python/api/azureml-contrib-reinforcementlearning/azureml.contrib.train.rl.reinforcementlearningestimator?view=azure-ml-py) kunt gebruiken om een trainings taak naar Azure machine learning te verzenden.

Azure Machine Learning maakt gebruik van Estimator-klassen voor het inkapselen van configuratie-informatie over de uitvoering. Zo kunt u eenvoudig opgeven hoe u een script uitvoering wilt configureren. Zie [modellen trainen met schattingen](how-to-train-ml-models.md)voor meer informatie over het Estimator-patroon van Azure machine learning.

### <a name="define-a-worker-configuration"></a>Een werknemers configuratie definiëren

Het WorkerConfiguration-object vertelt Azure Machine Learning hoe u het worker-cluster initialiseert waarmee het invoer script wordt uitgevoerd.

```python
# Pip packages we will use for both head and worker
pip_packages=["ray[rllib]==0.8.3"] # Latest version of Ray has fixes for isses related to object transfers

# Specify the Ray worker configuration
worker_conf = WorkerConfiguration(
    
    # Azure ML compute cluster to run Ray workers
    compute_target=worker_compute_target, 
    
    # Number of worker nodes
    node_count=4,
    
    # GPU
    use_gpu=False, 
    
    # PIP packages to use
    pip_packages=pip_packages
)
```

### <a name="define-script-parameters"></a>Script parameters definiëren

Het invoer script `pong_rllib.py` accepteert een lijst met para meters die definiëren hoe de trainings taak moet worden uitgevoerd. Door deze para meters door de Estimator als een laag van inkapseling door te geven, kunt u eenvoudig script parameters wijzigen en configuraties onafhankelijk van elkaar uitvoeren.

Als u het juiste type opgeeft `num_workers` , worden uw parallel Lise ring-inspanningen optimaal. Stel het aantal werk nemers in op dezelfde waarde als het aantal beschik bare Cpu's. Voor dit voor beeld kunt u dit als volgt berekenen:

Het hoofd knooppunt is een [Standard_NC6](https://docs.microsoft.com/azure/virtual-machines/nc-series) met 6 vcpu's. Het worker-cluster is 4 [Standard_D2_V2 vm's](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs#dv2-series) met twee cpu's elk, voor een totaal van 8 cpu's. U moet echter 1 CPU aftrekken van het aantal werk nemers sinds 1 moet worden toegewezen aan de rol van het hoofd knooppunt. 6 Cpu's + 8 Cpu's-1 Head-CPU = 13 gelijktijdige werk nemers. Azure Machine Learning maakt gebruik van hoofd-en work-clusters om reken resources te onderscheiden. Ray maakt echter geen onderscheid tussen hoofd-en werk nemers en alle Cpu's beschik bare Cpu's voor het uitvoeren van werk threads.


```python
training_algorithm = "IMPALA"
rl_environment = "PongNoFrameskip-v4"

# Training script parameters
script_params = {
    
    # Training algorithm, IMPALA in this case
    "--run": training_algorithm,
    
    # Environment, Pong in this case
    "--env": rl_environment,
    
    # Add additional single quotes at the both ends of string values as we have spaces in the 
    # string parameters, outermost quotes are not passed to scripts as they are not actually part of string
    # Number of GPUs
    # Number of ray workers
    "--config": '\'{"num_gpus": 1, "num_workers": 13}\'',
    
    # Target episode reward mean to stop the training
    # Total training time in seconds
    "--stop": '\'{"episode_reward_mean": 18, "time_total_s": 3600}\'',
}
```

### <a name="define-the-reinforcement-learning-estimator"></a>De Estimator voor de versterking van het leer proces definiëren

Gebruik de parameter lijst en het worker-configuratie object om de Estimator te maken.

```python
# RL estimator
rl_estimator = ReinforcementLearningEstimator(
    
    # Location of source files
    source_directory='files',
    
    # Python script file
    entry_script="pong_rllib.py",
    
    # Parameters to pass to the script file
    # Defined above.
    script_params=script_params,
    
    # The Azure ML compute target set up for Ray head nodes
    compute_target=head_compute_target,
    
    # Pip packages
    pip_packages=pip_packages,
    
    # GPU usage
    use_gpu=True,
    
    # RL framework. Currently must be Ray.
    rl_framework=Ray(),
    
    # Ray worker configuration defined above.
    worker_configuration=worker_conf,
    
    # How long to wait for whole cluster to start
    cluster_coordination_timeout_seconds=3600,
    
    # Maximum time for the whole Ray job to run
    # This will cut off the run after an hour
    max_run_duration_seconds=3600,
    
    # Allow the docker container Ray runs in to make full use
    # of the shared memory available from the host OS.
    shm_size=24*1024*1024*1024
)
```

### <a name="entry-script"></a>Invoer script

Het [invoer script](https://aka.ms/azure-rl-pong-script) `pong_rllib.py` traint een Neural-netwerk met behulp van de [OpenAI Gym-omgeving](https://github.com/openai/gym/) `PongNoFrameSkip-v4` . OpenAI Gyms zijn gestandaardiseerde interfaces voor het testen van de algoritmen voor versterking van het onderwijs op klassieke Atari-Games.

In dit voor beeld wordt een trainings algoritme gebruikt dat bekend staat als [IMPALA](https://arxiv.org/abs/1802.01561) (de architectuur van de leer factor met belang rijke actors). IMPALA parallelizes afzonderlijke Learning actor om te schalen over verschillende reken knooppunten zonder dat de snelheid of stabiliteit wordt gereduceerd.

Met [Ray Tune](https://ray.readthedocs.io/en/latest/tune.html) worden de IMPALA-werk taken beheerd.

```python
import ray
import ray.tune as tune
from ray.rllib import train

import os
import sys

from azureml.core import Run
from utils import callbacks

DEFAULT_RAY_ADDRESS = 'localhost:6379'

if __name__ == "__main__":

    # Parse arguments
    train_parser = train.create_parser()

    args = train_parser.parse_args()
    print("Algorithm config:", args.config)

    if args.ray_address is None:
        args.ray_address = DEFAULT_RAY_ADDRESS

    ray.init(address=args.ray_address)

    tune.run(run_or_experiment=args.run,
             config={
                 "env": args.env,
                 "num_gpus": args.config["num_gpus"],
                 "num_workers": args.config["num_workers"],
                 "callbacks": {"on_train_result": callbacks.on_train_result},
                 "sample_batch_size": 50,
                 "train_batch_size": 1000,
                 "num_sgd_iter": 2,
                 "num_data_loader_buffers": 2,
                 "model": {
                    "dim": 42
                 },
             },
             stop=args.stop,
             local_dir='./logs')
```

### <a name="logging-callback-function"></a>Call back functie voor logboek registratie


Het invoer script maakt gebruik van een functie voor het definiëren van een [aangepaste call back functie voor RLlib](https://docs.ray.io/en/latest/rllib-training.html#callbacks-and-custom-metrics) om de metrische gegevens naar uw Azure machine learning-werk ruimte te registreren. Meer informatie over het weer geven van deze metrische gegevens in de sectie [monitor en resultaten weer geven](#monitor-and-view-results) .

```python
'''RLlib callbacks module:
    Common callback methods to be passed to RLlib trainer.
'''
from azureml.core import Run

def on_train_result(info):
    '''Callback on train result to record metrics returned by trainer.
    '''
    run = Run.get_context()
    run.log(
        name='episode_reward_mean',
        value=info["result"]["episode_reward_mean"])
    run.log(
        name='episodes_total',
        value=info["result"]["episodes_total"])
```

## <a name="submit-a-run"></a>Een run verzenden

Met [uitvoeren](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py) wordt de uitvoerings geschiedenis van taken in uitvoering of volt ooien verwerkt. 

```python
run = exp.submit(config=rl_estimator)
```
> [!NOTE]
> Het kan tot 30 tot 45 minuten duren voordat de uitvoering is voltooid.

## <a name="monitor-and-view-results"></a>Resultaten controleren en weer geven

Gebruik de Azure Machine Learning Jupyter-widget om de status van uw uitvoeringen in realtime te bekijken. In dit voor beeld toont de widget twee onderliggende uitvoeringen: één voor kop en één voor werk nemers. 

```python
from azureml.widgets import RunDetails

RunDetails(run).show()
run.wait_for_completion()
```

1. Wacht totdat de widget is geladen.
1. Selecteer de kop-uitvoering in de lijst met uitvoeringen.

Selecteer **hier om de uitvoering in azure machine learning Studio te bekijken** voor meer informatie over de uitvoering in Studio. U kunt deze informatie openen terwijl de uitvoering wordt uitgevoerd of nadat deze is voltooid.

![Lijn diagram waarin wordt getoond hoe de widget Details uitvoeren wordt weer gegeven](./media/how-to-use-reinforcement-learning/pong-run-details-widget.png)

Het **episode_reward_mean** plot toont het gemiddelde aantal punten per trainings-epoche. U kunt zien dat de trainings agent voor het eerst goed is uitgevoerd, omdat de bijbehorende overeenkomsten verloren gaan zonder één punt te scoren (weer gegeven met een reward_mean van-21). Binnen 100 herhalingen heeft de trainings agent geleerd dat de computer op een gemiddeld 18 punten kan worden opgedaan.

Als u door de logboeken van de onderliggende uitvoering bladert, ziet u de evaluatie resultaten die in driver_log.txt bestand zijn vastgelegd. Mogelijk moet u enkele minuten wachten voordat deze metrische gegevens beschikbaar zijn op de pagina uitvoeren.

Kortom, u hebt geleerd hoe u meerdere reken resources kunt configureren om een versterking van een Learning-agent te trainen om Pong heel goed af te spelen.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u een versterkende Learning agent kunt trainen met behulp van een IMPALA Learning-agent. Als u meer voor beelden wilt bekijken, gaat u naar de [Azure machine learning-github-opslag plaats voor versterking](https://aka.ms/azureml-rl-notebooks).
