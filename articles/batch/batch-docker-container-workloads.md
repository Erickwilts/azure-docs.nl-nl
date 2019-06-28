---
title: Workloads voor containers - Azure Batch | Microsoft Docs
description: Informatie over het uitvoeren van toepassingen van containerinstallatiekopieën op Azure Batch.
services: batch
author: laurenhughes
manager: jeconnoc
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 11/19/2018
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: cfd00ad124db33cec8e30e8e1bb701388ee71838
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/24/2019
ms.locfileid: "67340229"
---
# <a name="run-container-applications-on-azure-batch"></a>Containertoepassingen uitvoeren op Azure Batch

Azure Batch kunt u uitvoeren en schalen van grote aantallen computing-taken op Azure batch. Batch-taken rechtstreeks op virtuele machines (knooppunten) in een Batch-pool kunnen uitvoeren, maar u kunt ook een Batch-pool instellen voor uitvoering van taken in compatibel is met Docker-containers op de knooppunten. Dit artikel laat u het maken van een pool van rekenknooppunten met actieve container taken ondersteunen en die vervolgens container taken uitvoeren in de pool. 

U moet bekend zijn met de container-concepten en over het maken van een Batch-pool en de taak. De codevoorbeelden gebruikt de Batch .NET en Python-SDK's. U kunt ook andere Batch-SDK's en hulpprogramma's, waaronder de Azure-portal gebruiken om te maken van container-functionaliteit van de Batch-pools en container-taken uitvoeren.

## <a name="why-use-containers"></a>Waarom containers gebruiken?

Met behulp van containers biedt een eenvoudige manier Batch-taken uitvoeren zonder voor het beheren van een omgeving en afhankelijkheden toepassingen uit te voeren. Containers implementeren van toepassingen als lichtgewicht, draagbare, zelfvoorzienende eenheden die kunnen worden uitgevoerd in verschillende omgevingen. Bijvoorbeeld, bouwen en testen van een container is lokaal uitgevoerd en vervolgens de containerinstallatiekopie uploaden naar een register in Azure of ergens anders. Het implementatiemodel container zorgt ervoor dat de runtime-omgeving van uw toepassing altijd correct is geïnstalleerd en geconfigureerd wanneer u de toepassing wordt gehost. Op containers gebaseerde taken in Batch kunnen ook profiteren van functies van niet-container-taken, waaronder toepassingspakketten en beheer van resourcebestanden en uitvoerbestanden. 

## <a name="prerequisites"></a>Vereisten

* **SDK-versies**: De Batch-SDK's ondersteunen containerinstallatiekopieën vanaf de volgende versies:
    * Batch REST-API versie 2017-09-01.6.0
    * Batch .NET SDK versie 8.0.0
    * Batch Python SDK versie 4.0
    * Batch Java SDK versie 3.0
    * Batch Node.js SDK versie 3.0

* **Accounts**: In uw Azure-abonnement moet u een Batch-account en optioneel een Azure Storage-account maken.

* **Een ondersteunde VM-installatiekopie**: Containers worden alleen ondersteund in pools die zijn gemaakt met de configuratie van de virtuele Machine, van afbeeldingen in de volgende sectie worden beschreven, 'ondersteund installatiekopieën voor virtuele machines." Als u een aangepaste installatiekopie, Zie de overwegingen met betrekking tot in de volgende sectie en de vereisten in [een beheerde aangepaste installatiekopie gebruiken om een pool van virtuele machines te maken](batch-custom-images.md). 

### <a name="limitations"></a>Beperkingen

* Batch biedt RDMA-ondersteuning voor containers die worden uitgevoerd op Linux-pools

* Voor Windows-containerwerkbelastingen, is het raadzaam om te kiezen voor uw pool een multicore VM-grootte

## <a name="supported-virtual-machine-images"></a>Installatiekopieën van ondersteunde virtuele machines

Gebruik een van de volgende ondersteund Windows of Linux-installatiekopieën om een pool van virtuele machine te maken van rekenknooppunten voor workloads voor containers. Zie voor meer informatie over de Marketplace-installatiekopieën die compatibel met Batch zijn [lijst met installatiekopieën van virtuele machines](batch-linux-nodes.md#list-of-virtual-machine-images). 

### <a name="windows-images"></a>Windows-installatiekopieën

Voor Windows-containerwerkbelastingen, Batch ondersteunt momenteel de **Windows Server 2016 Datacenter met Containers** installatiekopie in de Azure Marketplace. Alleen de installatiekopieën van Docker-containers worden ondersteund op Windows.

U kunt ook aangepaste installatiekopieën maken van virtuele machines met Docker op Windows.

### <a name="linux-images"></a>Linux-installatiekopieën

Batch ondersteunt momenteel de volgende Linux-installatiekopieën gepubliceerd door Microsoft Azure Batch in de Azure Marketplace voor Linux-container-workloads:

* **CentOS voor Azure Batch-pools van container**

* **CentOS (met RDMA-stuurprogramma's) voor Azure Batch-pools van container**

* **Ubuntu-Server voor Azure Batch-pools van container**

* **Ubuntu-Server (met RDMA-stuurprogramma's) voor Azure Batch-pools van container**

Deze installatiekopieën worden alleen ondersteund voor gebruik in Azure Batch-pools. Deze functie:

* Een vooraf geïnstalleerde [Moby](https://github.com/moby/moby) container-runtime 

* Vooraf geïnstalleerde NVIDIA GPU-stuurprogramma's, voor het stroomlijnen van implementatie op Azure uit de N-serie VM 's

* Uw keuze van afbeeldingen met of zonder RDMA-stuurprogramma's die vooraf zijn geïnstalleerd. Deze stuurprogramma's kunnen knooppunten toegang hebben tot de Azure RDMA-netwerk wanneer geïmplementeerd op RDMA-compatibele VM-grootten in de pool. 

U kunt ook aangepaste installatiekopieën maken van virtuele machines met Docker op een van de Linux-distributies die compatibel is met Batch. Als u kiest voor uw eigen aangepaste installatiekopie van Linux, raadpleegt u de instructies in [een beheerde aangepaste installatiekopie gebruiken om een pool van virtuele machines te maken](batch-custom-images.md).

Voor Docker-ondersteuning op een aangepaste installatiekopie, installeren [Docker Community Edition (CE)](https://www.docker.com/community-edition) of [Docker Enterprise Edition (EE)](https://www.docker.com/enterprise-edition).

Aanvullende overwegingen voor het gebruik van een aangepaste installatiekopie van Linux:

* Als u wilt profiteren van de GPU-prestaties van Azure N-serie beschikbaar wanneer u een aangepaste installatiekopie, voorafgaand aan installatie NVIDIA-stuurprogramma's. Bovendien moet u het hulpprogramma voor Docker-Engine installeren voor NVIDIA GPU's, [NVIDIA Docker](https://github.com/NVIDIA/nvidia-docker).

* Voor toegang tot het Azure-RDMA-netwerk, gebruikt u een RDMA-compatibele VM-grootte. Vereiste RDMA-stuurprogramma's zijn geïnstalleerd in de HPC CentOS en Ubuntu-installatiekopieën ondersteund door de Batch. Aanvullende configuratie kan nodig zijn voor het uitvoeren van MPI-workloads. Zie [gebruik RDMA-compatibele of met GPU exemplaren in de Batch-pool](batch-pool-compute-intensive-sizes.md).


## <a name="container-configuration-for-batch-pool"></a>Containerconfiguratie voor Batch-pool

Om in te schakelen in een Batch-pool containerwerkbelastingen uitvoeren, moet u [ContainerConfiguration](/dotnet/api/microsoft.azure.batch.containerconfiguration) instellingen in van de pool [VirtualMachineConfiguration](/dotnet/api/microsoft.azure.batch.virtualmachineconfiguration) object. (In dit artikel vindt u koppelingen naar de Batch .NET API-verwijzing. Bijbehorende instellingen zijn de [Batch Python](/python/api/azure.batch) API.)

U kunt een container-functionaliteit van toepassingen maken met of zonder tabelruimte containerinstallatiekopieën, zoals wordt weergegeven in de volgende voorbeelden. Het proces pull (of prefetch) kunt u vooraf laden containerinstallatiekopieën uit Docker Hub of een andere containerregister op het Internet. Gebruik voor de beste prestaties een [Azure-containerregister](../container-registry/container-registry-intro.md) in dezelfde regio als het Batch-account.

Het voordeel van veelgevraagde containerinstallatiekopieën is dat wanneer u taken voor het eerst uitgevoerd ze hoeft te wachten op de container-installatiekopie te downloaden. De containerconfiguratie van de wordt deze containerinstallatiekopieën naar de virtuele machines wanneer de groep wordt gemaakt. Taken die worden uitgevoerd op de pool kunnen vervolgens verwijzen naar de lijst met installatiekopieën van containers en container opties voor het uitvoeren.


### <a name="pool-without-prefetched-container-images"></a>Groep zonder tabelruimte containerinstallatiekopieën

Voor het configureren van een groep container ingeschakeld zonder tabelruimte containerinstallatiekopieën definiëren `ContainerConfiguration` en `VirtualMachineConfiguration` objecten, zoals wordt weergegeven in het volgende voorbeeld van Python. Dit voorbeeld wordt de Ubuntu-Server voor Azure Batch-containerinstallatiekopie groepen vanuit de Marketplace.


```python
image_ref_to_use = batch.models.ImageReference(
    publisher='microsoft-azure-batch',
    offer='ubuntu-server-container',
    sku='16-04-lts',
    version='latest')

"""
Specify container configuration. This is required even though there are no prefetched images.
"""

container_conf = batch.models.ContainerConfiguration()

new_pool = batch.models.PoolAddParameter(
    id=pool_id,
    virtual_machine_configuration=batch.models.VirtualMachineConfiguration(
        image_reference=image_ref_to_use,
        container_configuration=container_conf,
        node_agent_sku_id='batch.node.ubuntu 16.04'),
    vm_size='STANDARD_D1_V2',
    target_dedicated_nodes=1)
...
```


### <a name="prefetch-images-for-container-configuration"></a>Prefetch-installatiekopieën voor de containerconfiguratie van de

Toevoegen als u wilt prefetch-containerinstallatiekopieën in de pool, de lijst met installatiekopieën van containers (`container_image_names`, in Python) naar de `ContainerConfiguration`. 

De volgende eenvoudige Python-voorbeeld laat zien hoe vooraf ophalen van een standard Ubuntu-containerinstallatiekopie van [Docker Hub](https://hub.docker.com).

```python
image_ref_to_use = batch.models.ImageReference(
    publisher='microsoft-azure-batch',
    offer='ubuntu-server-container',
    sku='16-04-lts',
    version='latest')

"""
Specify container configuration, fetching the official Ubuntu container image from Docker Hub. 
"""

container_conf = batch.models.ContainerConfiguration(
    container_image_names=['ubuntu'])

new_pool = batch.models.PoolAddParameter(
    id=pool_id,
    virtual_machine_configuration=batch.models.VirtualMachineConfiguration(
        image_reference=image_ref_to_use,
        container_configuration=container_conf,
        node_agent_sku_id='batch.node.ubuntu 16.04'),
    vm_size='STANDARD_D1_V2',
    target_dedicated_nodes=1)
...
```


De volgende C# voorbeeld wordt ervan uitgegaan dat u wilt een TensorFlow-installatiekopie uit prefetch [Docker Hub](https://hub.docker.com). In dit voorbeeld bevat een begintaak die wordt uitgevoerd in de VM-host op de knooppunten in de pool. U kunt bijvoorbeeld een begintaak op de host uitvoeren voor het koppelen van een bestandsserver die kan worden benaderd vanaf de containers.

```csharp

ImageReference imageReference = new ImageReference(
    publisher: "microsoft-azure-batch",
    offer: "ubuntu-server-container",
    sku: "16-04-lts",
    version: "latest");

// Specify container configuration, prefetching Docker images
ContainerConfiguration containerConfig = new ContainerConfiguration(
    containerImageNames: new List<string> { "tensorflow/tensorflow:latest-gpu" } );

// VM configuration
VirtualMachineConfiguration virtualMachineConfiguration = new VirtualMachineConfiguration(
    imageReference: imageReference,
    containerConfiguration: containerConfig,
    nodeAgentSkuId: "batch.node.ubuntu 16.04");

// Set a native host command line start task
StartTask startTaskNative = new StartTask( CommandLine: "<native-host-command-line>" );

// Create pool
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 4,
    virtualMachineSize: "Standard_NC6",
    virtualMachineConfiguration: virtualMachineConfiguration, startTaskContainer);
...
```


### <a name="prefetch-images-from-a-private-container-registry"></a>Prefetch afbeeldingen uit een privécontainerregister

U kunt installatiekopieën van de container ook vooraf ophalen door te verifiëren met een privé-container registry-server. In het volgende voorbeeld wordt de `ContainerConfiguration` en `VirtualMachineConfiguration` objecten vooraf een persoonlijke TensorFlow-installatiekopie ophalen vanuit een persoonlijke Azure container registry. De verwijzing naar de installatiekopie is hetzelfde als in het vorige voorbeeld.

```csharp
// Specify a container registry
ContainerRegistry containerRegistry = new ContainerRegistry (
    registryServer: "myContainerRegistry.azurecr.io",
    username: "myUserName",
    password: "myPassword");

// Create container configuration, prefetching Docker images from the container registry
ContainerConfiguration containerConfig = new ContainerConfiguration(
    containerImageNames: new List<string> {
        "myContainerRegistry.azurecr.io/tensorflow/tensorflow:latest-gpu" },
    containerRegistries: new List<ContainerRegistry> { containerRegistry } );

// VM configuration
VirtualMachineConfiguration virtualMachineConfiguration = new VirtualMachineConfiguration(
    imageReference: imageReference,
    containerConfiguration: containerConfig,
    nodeAgentSkuId: "batch.node.ubuntu 16.04");

// Create pool
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 4,
    virtualMachineSize: "Standard_NC6",
    virtualMachineConfiguration: virtualMachineConfiguration);
...
```

## <a name="container-settings-for-the-task"></a>Container-instellingen voor de taak

Geef een container-taak uitvoeren op een groep container is ingeschakeld, container-specifieke instellingen. Instellingen omvatten de afbeelding om te gebruiken, het register en uitvoeringsopties.

* Gebruik de `ContainerSettings` eigenschap van de taak klassen om container-specifieke instellingen te configureren. Deze instellingen worden gedefinieerd door de [TaskContainerSettings](/dotnet/api/microsoft.azure.batch.taskcontainersettings) klasse.

* Als u taken op containerinstallatiekopieën uitvoeren de [cloud taak](/dotnet/api/microsoft.azure.batch.cloudtask) en [jobbeheertaak](/dotnet/api/microsoft.azure.batch.cloudjob.jobmanagertask) containerinstellingen vereisen. Echter, de [begintaak](/dotnet/api/microsoft.azure.batch.starttask), [jobvoorbereidingstaak](/dotnet/api/microsoft.azure.batch.cloudjob.jobpreparationtask), en [jobvrijgevingstaak](/dotnet/api/microsoft.azure.batch.cloudjob.jobreleasetask) vereisen geen container-instellingen (dat wil zeggen, ze kunnen worden uitgevoerd binnen de context van een container of rechtstreeks op het knooppunt).

### <a name="container-task-command-line"></a>De opdrachtregel van de container-taak

Wanneer u een container-taak uitvoert, wordt automatisch Batch gebruikt de [docker maken](https://docs.docker.com/engine/reference/commandline/create/) opdracht om een container met behulp van de opgegeven in de taak te maken. Batch bepaalt vervolgens de uitvoering van de taak in de container. 

Als met de Batch-taken niet-container instellen u een opdrachtregel voor de taak van een container. Omdat de container wordt automatisch gemaakt in Batch, de opdrachtregel alleen Hiermee geeft u de opdracht of opdrachten die in de container wordt uitgevoerd.

Als de installatiekopie van de container voor een batchtaak is geconfigureerd met een [ENTRYPOINT](https://docs.docker.com/engine/reference/builder/#exec-form-entrypoint-example) script, die u kunt de opdrachtregel te gebruiken de standaard ENTRYPOINT instellen of deze overschrijven: 

* Voor het gebruik van de standaard ENTRYPOINT van de containerinstallatiekopie, stelt u de opdrachtregel van de taak op een lege tekenreeks `""`.

* De standaardwaarde van het INGANGSPUNT vervangen, of als de afbeelding niet over een ENTRYPOINT instellen een opdrachtregel geschikt is voor de container, bijvoorbeeld `/app/myapp` of `/bin/sh -c python myscript.py`.

Optionele [ContainerRunOptions](/dotnet/api/microsoft.azure.batch.taskcontainersettings.containerrunoptions) extra argumenten die u aan te bieden de `docker create` opdracht dat Batch maakt gebruik van maken en uitvoeren van de container. Bijvoorbeeld, om een werkmap voor de container, stel de `--workdir <directory>` optie. Zie de [docker maken](https://docs.docker.com/engine/reference/commandline/create/) verwijzing naar extra opties.

### <a name="container-task-working-directory"></a>Werkmap van de container-taak

Een Batch-container-taak wordt uitgevoerd in een werkmap in de container die erg lijkt op de map die batch heeft ingesteld voor een normale (niet-container)-taak. Houd er rekening mee dat deze werkmap van verschilt de [WORKDIR](https://docs.docker.com/engine/reference/builder/#workdir) als geconfigureerd in de afbeelding of de werkmap van de standaard-container (`C:\` op een Windows-container of `/` op een Linux-container). 

Voor de taak van een Batch-container:

* Alle mappen recursief hieronder de `AZ_BATCH_NODE_ROOT_DIR` op de host-knooppunt (de hoofdmap van de Azure Batch-mappen) naar de container zijn toegewezen
* Alle variabelen voor de omgeving worden toegewezen aan de container
* De werkmap van de taak `AZ_BATCH_TASK_WORKING_DIR` op het knooppunt is dezelfde als die voor een normale taak ingesteld en naar de container is toegewezen. 

Deze toewijzingen kunnen u werken met taken van de container op ongeveer dezelfde manier als niet-container-taken. Bijvoorbeeld, installeren van toepassingen die gebruikmaken van toepassingspakketten, toegang krijgen tot resourcebestanden van Azure Storage gebruiken omgevingsinstellingen voor taken en uitvoerbestanden van de taak behouden nadat de container wordt gestopt.

### <a name="troubleshoot-container-tasks"></a>Container taken oplossen

Als uw container-taak niet wordt uitgevoerd zoals verwacht, moet u mogelijk voor informatie over de configuratie van WORKDIR of ENTRYPOINT van de containerinstallatiekopie. Als u wilt zien van de configuratie, de [docker-installatiekopie inspecteren](https://docs.docker.com/engine/reference/commandline/image_inspect/) opdracht. 

Wijzig indien nodig de instellingen van de container-taak op basis van de installatiekopie:

* Geef een absoluut pad in de opdrachtregel van de taak. Als de afbeelding standaard ENTRYPOINT wordt gebruikt voor de opdrachtregel van de taak, moet u controleren of een absoluut pad is ingesteld.

* Wijzig de werkmap zodat deze overeenkomt met de WORKDIR in de afbeelding in opties voor het uitvoeren van de taak container. Bijvoorbeeld, stel `--workdir /app`.

## <a name="container-task-examples"></a>Voorbeelden van container-taak

Het volgende fragment van Python ziet u een eenvoudige opdrachtregel die wordt uitgevoerd in een container gemaakt op basis van de installatiekopie van een fictieve opgehaald uit Docker Hub. Hier de `--rm` container optie wordt de container verwijderd nadat de taak is voltooid, en de `--workdir` optie stelt u een werkmap. De container ENTRYPOINT met een eenvoudige shellopdracht die u een klein bestand naar de werkmap van de taak op de host schrijft heeft voorrang op de opdrachtregel. 

```python
task_id = 'sampletask'
task_container_settings = batch.models.TaskContainerSettings(
    image_name='myimage',
    container_run_options='--rm --workdir /')
task = batch.models.TaskAddParameter(
    id=task_id,
    command_line='/bin/sh -c \"echo \'hello world\' > $AZ_BATCH_TASK_WORKING_DIR/output.txt\"',
    container_settings=task_container_settings
)
```

De volgende C#-voorbeeld ziet u eenvoudige container-instellingen voor een cloud-taak:

```csharp
// Simple container task command

string cmdLine = "c:\\app\\myApp.exe";

TaskContainerSettings cmdContainerSettings = new TaskContainerSettings (
    imageName: "myimage",
    containerRunOptions: "--rm --workdir c:\\app"
    );

CloudTask containerTask = new CloudTask (
    id: "Task1",
    containerSettings: cmdContainerSettings,
    commandLine: cmdLine); 
```


## <a name="next-steps"></a>Volgende stappen

* Zie ook de [Batch scheepswerf](https://github.com/Azure/batch-shipyard) toolkit voor een gemakkelijke implementatie van containerworkloads op Azure Batch via [scheepswerf recepten](https://github.com/Azure/batch-shipyard/tree/master/recipes).

* Zie voor meer informatie over het installeren en met behulp van Docker CE voor Linux de [Docker](https://docs.docker.com/engine/installation/) documentatie.

* Zie voor meer informatie over het gebruik van aangepaste installatiekopieën [een beheerde aangepaste installatiekopie gebruiken om een pool van virtuele machines te maken](batch-custom-images.md).

* Meer informatie over de [Moby project](https://mobyproject.org/), een framework voor het maken van container-systemen.