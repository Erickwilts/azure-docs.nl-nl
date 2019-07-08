---
title: 'Zelfstudie: Kubernetes in Azure: een containerregister maken'
description: In deze zelfstudie over AKS (Azure Kubernetes Service) gaat u een exemplaar van Azure Container Registry maken en een containerinstallatiekopie van een voorbeeldtoepassing uploaden.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: mlearned
ms.custom: mvc
ms.openlocfilehash: 5089326af1d7f6e057667cd916f35de92bf517ef
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67614251"
---
# <a name="tutorial-deploy-and-use-azure-container-registry"></a>Zelfstudie: Azure Container Registry implementeren en gebruiken

Azure Container Registry (ACR) is een privéregister voor installatiekopieën van de container. Een particulier containerregister stelt u in staat om op een veilige manier toepassingen en aangepaste code te bouwen en implementeren. In deze zelfstudie, deel twee van zeven, implementeert u een ACR-exemplaar en pusht u vervolgens een containerinstallatiekopie naar het exemplaar. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een ACR-exemplaar (Azure Container Registry) maken
> * Een containerinstallatiekopie voor ACR taggen
> * De installatiekopie uploaden naar ACR
> * Installatiekopieën weergeven in het register

In aanvullende zelfstudies wordt dit ACR-exemplaar geïntegreerd met een Kubernetes-cluster in AKS en wordt er met behulp van de installatiekopie een toepassing geïmplementeerd.

## <a name="before-you-begin"></a>Voordat u begint

In de [vorige zelfstudie][aks-tutorial-prepare-app] hebt u een containerinstallatiekopie voor een eenvoudige Azure Voting-toepassing gemaakt. Als u de installatiekopie voor de Azure Voting-toepassing niet hebt gemaakt, ga dan terug naar [Zelfstudie 1: Containerinstallatiekopieën maken][aks-tutorial-prepare-app].

Voor deze zelfstudie moet u Azure CLI versie 2.0.53 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren][azure-cli-install] als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="create-an-azure-container-registry"></a>Een Azure Container Registry maken

Om een Azure-containerregister te maken, hebt u eerst een resourcegroep nodig. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

Een resourcegroep maken met de opdracht [az group create][az-group-create]. In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt in de regio *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Maak een Azure Container Registry-exemplaar met de [az acr maken][az-acr-create] opdracht en geeft u de registernaam van uw eigen. De registernaam moet uniek zijn binnen Azure en mag 5 tot 50 alfanumerieke tekens bevatten. In de rest van deze zelfstudie wordt `<acrName>` gebruikt als een tijdelijke aanduiding voor de naam van het containerregister. Geef een unieke naam voor uw register op. De SKU *Basic* is een toegangspunt voor ontwikkelingsdoeleinden dat is geoptimaliseerd voor kosten, met een balans tussen opslag en doorvoer.

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

## <a name="log-in-to-the-container-registry"></a>Aanmelden bij het containerregister

Als u het ACR-exemplaar wilt gebruiken, moet u zich eerst aanmelden. Gebruik de [az acr login][az-acr-login] opdracht en geeft u de unieke naam van het containerregister in de vorige stap.

```azurecli
az acr login --name <acrName>
```

De opdracht retourneert het bericht *Aanmelden geslaagd* wanneer deze is uitgevoerd.

## <a name="tag-a-container-image"></a>Een containerinstallatiekopie taggen

Als een lijst met uw huidige lokale installatiekopieën wilt weergeven, gebruikt de [docker-installatiekopieën][docker-images] opdracht:

```
$ docker images

REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front             latest              4675398c9172        13 minutes ago      694MB
redis                        latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask               788ca94b2313        9 months ago        694MB
```

Als u de containerinstallatiekopie *azure-vote-front* wilt gebruiken met ACR, moet de installatiekopie worden getagd met het adres van de aanmeldingsserver van uw register. Deze tag wordt gebruikt voor routering bij het pushen van containerinstallatiekopieën naar een installatiekopieregister.

Als u het adres van de aanmelding, gebruikt de [az acr list][az-acr-list] opdracht en query's uitvoeren voor de *loginServer* als volgt:

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Tag nu de lokale installatiekopie *azure-vote-front* met het adres *acrloginServer* van het containerregister. U kunt de versie van de installatiekopie aangeven door *:v1* toe te voegen aan het eind van de naam van de installatiekopie:

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

Uitvoeren om te controleren of de labels worden toegepast, [docker-installatiekopieën][docker-images] opnieuw. Een installatiekopie wordt getagd met het adres van het ACR-exemplaar en een versienummer.

```
$ docker images

REPOSITORY                                           TAG           IMAGE ID            CREATED             SIZE
azure-vote-front                                     latest        eaf2b9c57e5e        8 minutes ago       716 MB
mycontainerregistry.azurecr.io/azure-vote-front      v1            eaf2b9c57e5e        8 minutes ago       716 MB
redis                                                latest        a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask                           flask         788ca94b2313        8 months ago        694 MB
```

## <a name="push-images-to-registry"></a>Installatiekopieën naar het register pushen

U kunt de gemaakte en getagde installatiekopie *azure-vote-front* nu pushen naar het ACR-exemplaar. Gebruik [docker push][docker-push] en geef uw eigen *acrLoginServer* adres voor de naam van de installatiekopie als volgt:

```console
docker push <acrLoginServer>/azure-vote-front:v1
```

Het kan enkele minuten duren voordat de installatiekopie naar ACR is gepusht.

## <a name="list-images-in-registry"></a>Installatiekopieën vermelden in het register

U kunt een lijst met installatiekopieën die naar het ACR-exemplaar zijn gepusht, gebruiken de [az acr repository list][az-acr-repository-list] opdracht. Geef als volgt uw eigen `<acrName>` op:

```azurecli
az acr repository list --name <acrName> --output table
```

In het volgende voorbeeld van uitvoer ziet u dat de installatiekopie *azure-vote-front* beschikbaar is in het register:

```
Result
----------------
azure-vote-front
```

Als de tags voor een specifieke installatiekopie wilt weergeven, gebruikt de [az acr repository show-tags][az-acr-repository-show-tags] opdracht als volgt:

```azurecli
az acr repository show-tags --name <acrName> --repository azure-vote-front --output table
```

In het volgende voorbeeld van uitvoer ziet u de tag *v1* zoals toegevoegd aan de installatiekopie in een eerdere stap:

```
Result
--------
v1
```

U beschikt nu over een containerinstallatiekopie die is opgeslagen in een particulier Azure Container Registry-exemplaar. Deze installatiekopie wordt in de volgende zelfstudie vanuit ACR geïmplementeerd naar een Kubernetes-cluster.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een Azure Container Registry gemaakt en een installatiekopie gepusht voor gebruik in een AKS-cluster. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een ACR-exemplaar (Azure Container Registry) maken
> * Een containerinstallatiekopie voor ACR taggen
> * De installatiekopie uploaden naar ACR
> * Installatiekopieën weergeven in het register

Ga naar de volgende zelfstudie voor informatie over het implementeren van een Kubernetes-cluster in Azure.

> [!div class="nextstepaction"]
> [Kubernetes-cluster implementeren][aks-tutorial-deploy-cluster]

<!-- LINKS - external -->
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/

<!-- LINKS - internal -->
[az-acr-create]: /cli/azure/acr
[az-acr-list]: /cli/azure/acr
[az-acr-login]: https://docs.microsoft.com/cli/azure/acr#az-acr-login
[az-acr-list]: https://docs.microsoft.com/cli/azure/acr#az-acr-list
[az-acr-repository-list]: /cli/azure/acr/repository
[az-acr-repository-show-tags]: /cli/azure/acr/repository
[az-group-create]: /cli/azure/group#az-group-create
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-tutorial-deploy-cluster]: ./tutorial-kubernetes-deploy-cluster.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
