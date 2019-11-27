---
title: Overzicht van ACR-taken
description: Een inleiding tot ACR-taken, een reeks functies in Azure Container Registry die beveiligde, geautomatiseerde build van container installatie kopieën biedt, beheer en patches in de Cloud.
ms.topic: article
ms.date: 09/05/2019
ms.openlocfilehash: b4710591dfd78f0633d5071c78d80e300349f498
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/24/2019
ms.locfileid: "74456156"
---
# <a name="automate-container-image-builds-and-maintenance-with-acr-tasks"></a>Bouw en onderhoud van container installatie kopieën automatiseren met ACR-taken

Containers bieden nieuwe niveaus van virtualisatie, het isoleren van afhankelijkheden van toepassingen en ontwikkel aars uit infra structuur en operationele vereisten. Wat er nog steeds blijft, is echter de nood zaak om te bepalen hoe deze Application Virtualization wordt beheerd en gepatchd over de levens cyclus van de container.

## <a name="what-is-acr-tasks"></a>Wat zijn ACR-taken?

**ACR-taken** is een reeks functies in azure container Registry. Het bevat een Cloud installatie kopie voor het maken van een container voor [platforms](#image-platforms) , waaronder Linux, Windows en arm, en kan het [besturings systeem en Framework-patches](#automate-os-and-framework-patching) voor uw docker-containers automatiseren. ACR-taken breiden de ontwikkelings cyclus "binnenste loop" niet alleen uit naar de Cloud met builds van installatie kopieën op aanvraag, maar biedt ook automatische builds die worden geactiveerd door bron code-updates, updates naar de basis installatie kopie van een container of timers. Zo kunt u met de basis installatie-update triggers de werk stroom voor het besturings systeem en toepassings raamwerk automatiseren, waarbij beveiligde omgevingen worden onderhouden en de principes van onveranderlijke containers worden gevolgd.

## <a name="task-scenarios"></a>Taak scenario's

ACR-taken bieden ondersteuning voor verschillende scenario's voor het maken en onderhouden van container installatie kopieën en andere artefacten. Zie de volgende secties in dit artikel voor meer informatie.

* **[Snelle taak](#quick-task)** : bouw en push één container installatie kopie naar een container register op aanvraag in azure, zonder dat hiervoor een lokale docker-engine moet worden geïnstalleerd. Denk `docker build`, `docker push` in de Cloud.
* **Automatisch geactiveerde taken** : Schakel een of meer *Triggers* in om een installatie kopie samen te stellen.
  * **[Activeren bij het bijwerken van de bron code](#trigger-task-on-source-code-update)** 
  * **[Activeren bij basis installatie kopie-update](#automate-os-and-framework-patching)** 
  * **[Activeren volgens een planning](#schedule-a-task)** 
* **[Taken met meerdere stappen](#multi-step-tasks)** : Breid de build-and-push mogelijkheden van één installatie kopie uit van ACR-taken met meerdere stappen, op meerdere containers gebaseerde werk stromen. 

Elke ACR-taak heeft een gekoppelde [broncode context](#context-locations) : de locatie van een set bron bestanden die wordt gebruikt om een container installatie kopie of ander artefact te bouwen. Voor beelden van contexten zijn een Git-opslag plaats of een lokaal bestands systeem.

Taken kunnen ook gebruikmaken van [variabelen uitvoeren](container-registry-tasks-reference-yaml.md#run-variables), zodat u taak definities opnieuw kunt gebruiken en tags voor installatie kopieën en artefacten standaardisert.

## <a name="quick-task"></a>Snelle taak

Het proces voor de duur van de binnenste cyclus, het iteratieproces voor het schrijven van code, het ontwikkelen en testen van uw toepassing voordat u een bron beheer uitvoert, is het begin van het beheer van de container levenscyclus.

Voordat u uw eerste regel code doorvoert, kunt u met de [snelle taak](container-registry-tutorial-quick-task.md) functie van ACR-taken een geïntegreerde ontwikkel ervaring bieden door de build-installatie kopieën van de container te offloaden naar Azure. Met snelle taken kunt u uw geautomatiseerde build-definities controleren en mogelijke problemen ondervangen voordat u uw code doorvoert.

Met de vertrouwde `docker build`-indeling wordt met de opdracht [AZ ACR build][az-acr-build] in azure cli een [context](#context-locations) (de set te bouwen bestanden) gemaakt, wordt de ACR-taken verzonden en wordt standaard de ingebouwde installatie kopie naar het REGI ster gepusht na voltooiing.

Zie voor een inleiding de Snelstartgids voor het [maken en uitvoeren van een container installatie kopie](container-registry-quickstart-task-cli.md) in azure container Registry.  

ACR-taken is ontworpen als een levens cyclus voor containers. U kunt bijvoorbeeld ACR-taken integreren in uw CI/CD-oplossing. Door de opdracht [AZ login][az-login] met een [Service-Principal][az-login-service-principal]uit te voeren, kan uw CI/cd-oplossing [AZ ACR build][az-acr-build] -opdrachten verzenden om installatie kopieën te starten.

Informatie over het gebruik van snelle taken in de eerste zelf studie over ACR-taken, [bouw container installatie kopieën in de Cloud met Azure container Registry taken](container-registry-tutorial-quick-task.md).

> [!TIP]
> Als u een installatie kopie rechtstreeks vanuit de bron code wilt maken en pushen zonder Dockerfile, levert Azure Container Registry de opdracht [AZ ACR Pack build][az-acr-pack-build] (preview). Dit hulp programma bouwt en duwt een installatie kopie van de bron code van de toepassing met behulp van de [native Buildpacks](https://buildpacks.io/)van de Cloud.

## <a name="trigger-task-on-source-code-update"></a>Taak activeren bij het bijwerken van de bron code

Een taak voor het maken van een container installatie kopie of een taken uit meerdere stappen activeren wanneer de code wordt vastgelegd, of een pull-aanvraag wordt gedaan of bijgewerkt naar een Git-opslag plaats in GitHub of Azure DevOps. Configureer bijvoorbeeld een build-taak met de Azure CLI-opdracht [AZ ACR Task Create][az-acr-task-create] door een Git-opslag plaats en optioneel een vertakking en Dockerfile op te geven. Wanneer uw team code in de opslag plaats bijwerkt, activeert een webhook met ACR-gemaakte taken een build van de container installatie kopie die is gedefinieerd in de opslag plaats. 

ACR-taken bieden ondersteuning voor de volgende triggers wanneer u een Git-opslag plaats instelt als de context van de taak:

| Trigger | Standaard ingeschakeld |
| ------- | ------------------ |
| Doorvoeren | Ja |
| Pull-aanvraag | Nee |

Als u de trigger wilt configureren, geeft u de taak een persoonlijk toegangs token (PAT) voor het instellen van de webhook in de GitHub-of Azure DevOps opslag plaats.

Meer informatie over het activeren van builds op basis van de bron code in de tweede zelf studie ACR-taken, het [automatiseren van container installatie kopieën met Azure container Registry taken](container-registry-tutorial-build-task.md).

## <a name="automate-os-and-framework-patching"></a>Reparatie van besturings systemen en Framework automatiseren

De kracht van ACR taken om de werk stroom voor het bouwen van containers echt te verbeteren, is van de mogelijkheid om een update naar een basis installatie kopie te detecteren. Wanneer de bijgewerkte basis installatie kopie naar het REGI ster wordt gepusht of een basis installatie kopie is bijgewerkt in een open bare opslag plaats, zoals in docker hub, kunnen ACR-taken automatisch eventuele installatie kopieën van toepassingen maken op basis van het bestand.

Container installatie kopieën kunnen breed worden gecategoriseerd in *basis* installatie kopieën en *toepassings* installatie kopieën. Uw basis installatie kopieën bevatten doorgaans het besturings systeem en toepassings raamwerken waarop uw toepassing is gebouwd, samen met andere aanpassingen. Deze basis installatie kopieën zijn normaal gesp roken gebaseerd op open bare upstream-installatie kopieën, bijvoorbeeld: [Alpine Linux][base-alpine], [Windows][base-windows], [.net][base-dotnet]of [node. js][base-node]. Diverse installatie kopieën van uw toepassing kunnen een gemeen schappelijke basis installatie kopie delen.

Wanneer een installatie kopie van een besturings systeem of app-Framework wordt bijgewerkt door de upstream-Maintainer, bijvoorbeeld met een kritieke beveiligings patch voor het besturings systeem, moet u ook uw basis installatie kopieën bijwerken om de essentiële oplossing op te nemen. Elke toepassings installatie kopie moet vervolgens ook opnieuw worden opgebouwd om deze upstream-oplossingen op te nemen die nu zijn opgenomen in uw basis installatie kopie.

Omdat ACR-taken de afhankelijkheden van basis installatie kopieën dynamisch detecteren bij het samen stellen van een container installatie kopie, kan deze detecteert wanneer de basis installatie kopie van de toepassings installatie kopie wordt bijgewerkt. Met één vooraf geconfigureerde [Build-taak](container-registry-tutorial-base-image-update.md#create-a-task)bouwt ACR taken vervolgens **automatisch elke installatie kopie van de toepassing opnieuw** op. Met deze automatische detectie en het opnieuw samen stellen van ACR-taken bespaart u de tijd en inspanningen die normaal gesp roken nodig zijn voor het hand matig bijhouden en bijwerken van elke toepassings installatie kopie die verwijst naar de bijgewerkte basis installatie kopie.

Voor installatie kopieën van een Dockerfile houdt een ACR-taak een update van een basis installatie kopie bij wanneer de basis installatie kopie zich op een van de volgende locaties bevindt:

* Hetzelfde Azure container Registry waarin de taak wordt uitgevoerd
* Een ander Azure container registry in dezelfde regio 
* Een open bare opslag plaats in docker hub
* Een open bare opslag plaats in micro soft Container Registry

> [!NOTE]
> * De trigger voor het bijwerken van de basis installatie kopie is standaard ingeschakeld in een ACR-taak. 
> * Op dit moment worden ACR-updates voor installatie kopieën van de toepassing (*runtime*) alleen door de taken bijgehouden. ACR-taken volgen geen updates van basis afbeeldingen voor tussenliggende (*buildtime*)-installatie kopieën die worden gebruikt in de Dockerfiles met meerdere fasen. 

Meer informatie over patches voor besturings systemen en Framework in de ACR-zelf studie voor de derde taak [voor het automatiseren van installatie kopieën voor de update van de basis installatie kopie met Azure container Registry taken](container-registry-tutorial-base-image-update.md).

## <a name="schedule-a-task"></a>Een taak plannen

U kunt een taak optioneel plannen door een of meer *Timer triggers* in te stellen wanneer u de taak maakt of bijwerkt. Het plannen van een taak is handig voor het uitvoeren van container werkbelastingen op een gedefinieerd schema, of het uitvoeren van onderhouds bewerkingen of tests op installatie kopieën die regel matig naar het REGI ster worden gepusht. Zie [een ACR-taak uitvoeren volgens een gedefinieerd schema](container-registry-tasks-scheduled.md)voor meer informatie.

## <a name="multi-step-tasks"></a>Taken met meerdere stappen

Taken met meerdere stappen bieden taak definitie en uitvoering op basis van een stap voor het bouwen, testen en bijwerken van container installatie kopieën in de Cloud. Taak stappen die in een [yaml-bestand](container-registry-tasks-reference-yaml.md) zijn gedefinieerd, geven afzonderlijke compilatie-en push bewerkingen voor container installatie kopieën of andere artefacten. Ze kunnen ook de uitvoering definiëren van een of meer containers, waarbij elke stap de container als uitvoeringsomgeving gebruikt.

U kunt bijvoorbeeld een taak met meerdere stappen maken waarmee het volgende wordt geautomatiseerd:

1. Een installatie kopie voor een webtoepassing bouwen
1. De Web Application-container uitvoeren
1. Een test installatie kopie voor een webtoepassing bouwen
1. Voer de test container voor webtoepassingen uit, waarmee tests worden uitgevoerd voor de actieve toepassings container
1. Als de tests zijn geslaagd, bouwt u een helm-archief pakket voor grafieken op
1. Een `helm upgrade` uitvoeren met het nieuwe helm-archief pakket voor grafieken

Met taken met meerdere stappen kunt u het bouwen, uitvoeren en testen van een afbeelding in meer samenstel bare stappen splitsen, met ondersteuning voor afhankelijkheden tussen verschillende stappen. Met taken met meerdere stappen in ACR-taken hebt u meer gedetailleerde controle over het bouwen, testen en OS-en Framework-patch werk stromen.

Meer informatie over taken in meerdere stappen in de stappen voor het [bouwen, testen en patchen van taken uitvoeren in ACR-taken](container-registry-tasks-multi-step.md).

## <a name="context-locations"></a>Context locaties

De volgende tabel bevat enkele voor beelden van ondersteunde context locaties voor ACR-taken:

| Context locatie | Beschrijving | Voorbeeld |
| ---------------- | ----------- | ------- |
| Lokaal bestands systeem | Bestanden in een map op het lokale bestands systeem. | `/home/user/projects/myapp` |
| Hoofd vertakking GitHub | Bestanden in de hoofd vertakking (of een andere standaard) van een GitHub-opslag plaats.  | `https://github.com/gituser/myapp-repo.git` |
| GitHub-vertakking | Specifieke vertakking van een GitHub-opslag plaats.| `https://github.com/gituser/myapp-repo.git#mybranch` |
| Submap GitHub | Bestanden in een submap van een GitHub-opslag plaats. Voor beeld wordt een combi natie van een vertakking en submap opgegeven. | `https://github.com/gituser/myapp-repo.git#mybranch:myfolder` |
| Submap voor Azure DevOps | Bestanden in een submap van een Azure-opslag plaats. Voor beeld wordt een combi natie van een specificatie van branch en submap weer gegeven. | `https://dev.azure.com/user/myproject/_git/myapp-repo#mybranch:myfolder` |
| Externe tarball | Bestanden in een gecomprimeerd archief op een externe webserver. | `http://remoteserver/myapp.tar.gz` |

## <a name="image-platforms"></a>Afbeeldings platforms

Standaard bouwt ACR-taken installatie kopieën voor het Linux-besturings systeem en de amd64-architectuur. Geef het `--platform` label op om Windows-installatie kopieën of Linux-installatie kopieën voor andere architecturen te maken. Geef het besturings systeem en eventueel een ondersteunde architectuur op in de indeling van het besturings systeem/de architectuur (bijvoorbeeld `--platform Linux/arm`). Voor ARM-architecturen geeft u optioneel een variant op in de indeling OS/Architecture/variant (bijvoorbeeld `--platform Linux/arm64/v8`):

| OS | Architectuur|
| --- | ------- | 
| Linux | amd64<br/>scherp<br/>arm64<br/>386 |
| Windows | amd64 |

## <a name="view-task-logs"></a>Taak logboeken weer geven

Elke taak wordt uitgevoerd, genereert een logboek uitvoer die u kunt inspecteren om te bepalen of de taak stappen zijn uitgevoerd. Als u de opdracht [AZ ACR build](/cli/azure/acr#az-acr-build), [AZ ACR run](/cli/azure/acr#az-acr-run)of [AZ ACR Task run](/cli/azure/acr/task#az-acr-task-run) gebruikt om de taak te activeren, wordt de logboek uitvoer voor de uitvoering van de taak naar de console gestreamd en ook opgeslagen om later op te halen. Wanneer een taak automatisch wordt geactiveerd, bijvoorbeeld door het door voeren van een bron code of een update van een basis installatie kopie, worden de taak Logboeken alleen opgeslagen. Bekijk de logboeken voor een taak die wordt uitgevoerd in de Azure Portal, of gebruik de opdracht [AZ ACR taak logs](/cli/azure/acr/task#az-acr-task-logs) .

Gegevens en logboeken voor taak uitvoeringen in een REGI ster worden standaard 30 dagen bewaard en vervolgens automatisch opgeschoond. Als u de gegevens voor een taak uitvoering wilt archiveren, schakelt u archiveren in met de opdracht [AZ ACR Task update-run](/cli/azure/acr/task#az-acr-task-update-run) . In het volgende voor beeld wordt het archiveren voor de taak *cf11* in REGI ster *myregistry*ingeschakeld.

```azurecli
az acr task update-run --registry myregistry --run-id cf11 --no-archive false
```

## <a name="next-steps"></a>Volgende stappen

Als u klaar bent voor het automatiseren van container installatie kopieën en onderhoud in de Cloud, raadpleegt u de [reeks zelf](container-registry-tutorial-quick-task.md)studies over ACR-taken.

Installeer eventueel de [docker-extensie voor Visual Studio code](https://code.visualstudio.com/docs/azure/docker) en de [Azure-account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) extensie om met uw Azure-container registers te werken. Pull-en push-installatie kopieën naar een Azure container Registry, of voer ACR-taken uit, allemaal in Visual Studio code.

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-pack-build]: /cli/azure/acr/pack#az-acr-pack-build
[az-acr-task]: /cli/azure/acr/task
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
