---
title: Installatiekopie bouwen, testen en patchen met Azure Container Registry WebTest met meerdere stappen taken automatiseren
description: Een inleiding tot taken meerdere stappen, een functie van de ACR-taken in Azure Container Registry waarmee werkstromen op basis van een taak voor het ontwikkelen, testen en patchen van containerinstallatiekopieën in de cloud.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 03/28/2019
ms.author: danlep
ms.openlocfilehash: ac0e4e9019a35d3fdb35c0b7af9cb1289f4bceeb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60829579"
---
# <a name="run-multi-step-build-test-and-patch-tasks-in-acr-tasks"></a>WebTest met meerdere stappen bouwen, testen en patch-taken uitvoeren in de ACR-taken

Taken met meerdere stappen Breid de mogelijkheden van de installatiekopie van één-build-en-push van ACR taken met meerdere stappen, meerdere-container op basis van werkstromen. WebTest met meerdere stappen taken te maken en pushen van verschillende afbeeldingen, in de reeks of gelijktijdig gebruiken. Voer vervolgens deze installatiekopieën als opdrachten binnen een enkele taak uitvoeren. Elke stap definieert u een containerinstallatiekopie bouwen of push-bewerking en kunt ook de uitvoering van een container definiëren. Elke stap in een taak meerdere stappen maakt gebruik van een container als de uitvoeringsomgeving.

> [!IMPORTANT]
> Als u eerder taken hebt gemaakt tijdens de preview met de opdracht `az acr build-task`, moeten deze taken opnieuw worden gemaakt met de opdracht [az acr task][az-acr-task].

U kunt bijvoorbeeld een taak uitvoeren met stappen die de volgende logica automatiseren:

1. De afbeelding van een web-toepassing bouwen
1. De web-App-container uitvoeren
1. Bouw een web-toepassing test-installatiekopie
1. Uitvoeren van de toepassing test webcontainer die tests uit op basis van de actieve voert App-container
1. Als de tests lukken, een Helm-grafiek Archiefpakket bouwen
1. Voer een `helm upgrade` met behulp van de nieuwe Archiefpakket van de Helm-grafiek

Alle stappen worden uitgevoerd in Azure, het offloaden van het werk van Azure compute-resources en u uit het beheer van vrij te maken. Naast uw Azure-containerregister betaalt u alleen voor de resources die u gebruikt. Zie voor meer informatie over prijzen voor de **Container bouwen** in sectie [prijzen voor Azure Container Registry][pricing].


## <a name="common-task-scenarios"></a>Algemene scenario's voor taak

Taken met meerdere stappen inschakelen scenario's zoals de volgende logica:

* Maken, code, en een of meer installatiekopieën van containers, push in serie of gelijktijdig.
* Uitvoeren en vast te leggen eenheid test- en code dekking resultaten.
* Uitvoeren en functionele tests vast te leggen. ACR taken ondersteunt de uitvoering van meer dan één container, een reeks aanvragen tussen beide uitvoeren.
* Uitvoeren op basis van een taak wordt uitgevoerd, met inbegrip van de stappen voor/na van een build van container-installatiekopie.
* Een of meer containers implementeren met uw favoriete implementatie-engine op uw doelomgeving.

## <a name="multi-step-task-definition"></a>WebTest met meerdere stappen taakdefinitie

Een taak meerdere stappen in de ACR-taken is gedefinieerd als een reeks stappen binnen een YAML-bestand. Elke stap kunt afhankelijkheden opgeven op de voltooiing van een of meer van de vorige stappen. De volgende stap taaktypen zijn beschikbaar:

* [`build`](container-registry-tasks-reference-yaml.md#build): Een of meer installatiekopieën met de vertrouwde compileren `docker build` syntaxis en voorbeelden, in de reeks of gelijktijdig.
* [`push`](container-registry-tasks-reference-yaml.md#push): Ingebouwde installatiekopieën pushen naar een containerregister. Persoonlijke registers zoals Azure Container Registry worden ondersteund, omdat de openbare Docker Hub.
* [`cmd`](container-registry-tasks-reference-yaml.md#cmd): Uitvoeren van een container, zodat deze als een functie binnen de context van de actieve taak werken kan. U kunt parameters doorgeven aan van de container `[ENTRYPOINT]`, en geef eigenschappen op, zoals env, loskoppelen, en andere vertrouwde `docker run` parameters. De `cmd` staptype kan eenheid en functionele tests, met gelijktijdige container kan worden uitgevoerd.

De volgende codefragmenten laten zien hoe u kunt deze stap taaktypen combineren. WebTest met meerdere stappen taken kunnen worden net zo eenvoudig als het opbouwen van een één installatiekopie van een docker-bestand en het pushen naar uw register met een YAML-bestand die vergelijkbaar is met:

```yml
version: v1.0.0
steps:
  - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
  - push: ["{{.Run.Registry}}/hello-world:{{.Run.ID}}"]
```

Of complexer worden, zoals deze fictieve definitie WebTest met meerdere stappen stappen voor het bouwen omvat, testen, helm-pakket en helm implementeren (containerregister en configuratie van de Helm-opslagplaats niet weergegeven):

```yml
version: v1.0.0
steps:
  - id: build-web
    build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
    when: ["-"]
  - id: build-tests
    build -t {{.Run.Registry}}/hello-world-tests ./funcTests
    when: ["-"]
  - id: push
    push: ["{{.Run.Registry}}/helloworld:{{.Run.ID}}"]
    when: ["build-web", "build-tests"]
  - id: hello-world-web
    cmd: {{.Run.Registry}}/helloworld:{{.Run.ID}}
  - id: funcTests
    cmd: {{.Run.Registry}}/helloworld:{{.Run.ID}}
    env: ["host=helloworld:80"]
  - cmd: {{.Run.Registry}}/functions/helm package --app-version {{.Run.ID}} -d ./helm ./helm/helloworld/
  - cmd: {{.Run.Registry}}/functions/helm upgrade helloworld ./helm/helloworld/ --reuse-values --set helloworld.image={{.Run.Registry}}/helloworld:{{.Run.ID}}
```

Zie [taak voorbeelden] [ task-examples] voltooien voor de taak meerdere stappen YAML-bestanden en docker-bestanden voor verschillende scenario's.

## <a name="run-a-sample-task"></a>Een voorbeeld-taak uitvoeren

Taken ondersteunen beide handmatig uitvoeren, een "snelle uitvoert,' genoemd en geautomatiseerde kan worden uitgevoerd op de installatiekopie doorvoeren of base Git bijwerken.

Als u wilt een taak uitvoert, u eerst de stappen van de taak in een YAML-bestand definiëren en geeft u de Azure CLI-opdracht [az acr uitvoeren][az-acr-run].

Hier volgt een voorbeeld van Azure CLI-opdracht die een taak met behulp van een YAML-bestand van de voorbeeld-taak wordt uitgevoerd. De stappen bouwen en vervolgens een installatiekopie pushen. Update `\<acrName\>` met de naam van uw eigen Azure-containerregister voordat u de opdracht uitvoert.

```azurecli
az acr run --registry <acrName> -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

Wanneer u de taak uitvoert, moet de voortgang van elke stap die is gedefinieerd in het YAML-bestand weergeven in de uitvoer. De stappen worden in de volgende uitvoer weergegeven als `acb_step_0` en `acb_step_1`.

```console
$ az acr run --registry myregistry -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
Sending context to registry: myregistry...
Queued a run with ID: yd14
Waiting for an agent...
2018/09/12 20:08:44 Using acb_vol_0467fe58-f6ab-4dbd-a022-1bb487366941 as the home volume
2018/09/12 20:08:44 Creating Docker network: acb_default_network
2018/09/12 20:08:44 Successfully set up Docker network: acb_default_network
2018/09/12 20:08:44 Setting up Docker configuration...
2018/09/12 20:08:45 Successfully set up Docker configuration
2018/09/12 20:08:45 Logging in to registry: myregistry.azurecr-test.io
2018/09/12 20:08:46 Successfully logged in
2018/09/12 20:08:46 Executing step: acb_step_0
2018/09/12 20:08:46 Obtaining source code and scanning for dependencies...
2018/09/12 20:08:47 Successfully obtained source code and scanned for dependencies
Sending build context to Docker daemon  109.6kB
Step 1/1 : FROM hello-world
 ---> 4ab4c602aa5e
Successfully built 4ab4c602aa5e
Successfully tagged myregistry.azurecr-test.io/hello-world:yd14
2018/09/12 20:08:48 Executing step: acb_step_1
2018/09/12 20:08:48 Pushing image: myregistry.azurecr-test.io/hello-world:yd14, attempt 1
The push refers to repository [myregistry.azurecr-test.io/hello-world]
428c97da766c: Preparing
428c97da766c: Layer already exists
yd14: digest: sha256:1a6fd470b9ce10849be79e99529a88371dff60c60aab424c077007f6979b4812 size: 524
2018/09/12 20:08:55 Successfully pushed image: myregistry.azurecr-test.io/hello-world:yd14
2018/09/12 20:08:55 Step id: acb_step_0 marked as successful (elapsed time in seconds: 2.035049)
2018/09/12 20:08:55 Populating digests for step id: acb_step_0...
2018/09/12 20:08:57 Successfully populated digests for step id: acb_step_0
2018/09/12 20:08:57 Step id: acb_step_1 marked as successful (elapsed time in seconds: 6.832391)
The following dependencies were found:
- image:
    registry: myregistry.azurecr-test.io
    repository: hello-world
    tag: yd14
    digest: sha256:1a6fd470b9ce10849be79e99529a88371dff60c60aab424c077007f6979b4812
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: latest
    digest: sha256:0add3ace90ecb4adbf7777e9aacf18357296e799f81cabc9fde470971e499788
  git: {}


Run ID: yd14 was successful after 19s
```

Zie voor meer informatie over geautomatiseerde builds bij Git doorvoeren of base-installatiekopie bijwerken, de [compileren van installatiekopieën automatiseren](container-registry-tutorial-build-task.md) en [baseren compileren van installatiekopieën update](container-registry-tutorial-base-image-update.md) zelfstudie-artikelen.

## <a name="next-steps"></a>Volgende stappen

U kunt verwijzing van de taak meerdere stappen en voorbeelden hier vinden:

* [Naslaginformatie over de taak](container-registry-tasks-reference-yaml.md) -taak stap typen, hun eigenschappen en het gebruik.
* [Taak voorbeelden] [ task-examples] -voorbeeld `task.yaml` bestanden voor verschillende scenario's, eenvoudig tot complex.
* [Cmd opslagplaats](https://github.com/AzureCR/cmd) -een verzameling van containers als opdrachten voor het ACR-taken.

<!-- IMAGES -->

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/
[task-examples]: https://github.com/Azure-Samples/acr-tasks
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-acr-task]: /cli/azure/acr/task