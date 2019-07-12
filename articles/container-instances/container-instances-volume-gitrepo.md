---
title: Koppelen van een volume gitRepo Azure Container Instances
description: Meer informatie over het koppelen van een volume gitRepo als u wilt klonen van een Git-opslagplaats naar uw containerinstanties
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 06/15/2018
ms.author: danlep
ms.openlocfilehash: 86f8c099061cd3b75b77330c567f34dea2b34928
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657589"
---
# <a name="mount-a-gitrepo-volume-in-azure-container-instances"></a>Koppelen van een volume gitRepo in Azure Container Instances

Meer informatie over het koppelen van een *gitRepo* volume voor het klonen van een Git-opslagplaats naar uw containerinstanties.

> [!NOTE]
> Koppelen van een *gitRepo* volume is momenteel beperkt tot Linux-containers. Terwijl we werken om alle functies op Windows-containers, vindt u de huidige platform verschillen in de [overzicht](container-instances-overview.md#linux-and-windows-containers).

## <a name="gitrepo-volume"></a>gitRepo-volume

De *gitRepo* volume koppelt u een map en de opgegeven Git-opslagplaats gekloond erin bij het opstarten van de container. Met behulp van een *gitRepo* volume in uw containerinstanties, kunt u voorkomen dat de code voor het in dat geval in uw toepassingen toe te voegen.

Wanneer u koppelt een *gitRepo* volume, kunt u drie eigenschappen voor het configureren van het volume instellen:

| Eigenschap | Verplicht | Description |
| -------- | -------- | ----------- |
| `repository` | Ja | De volledige URL, met inbegrip van `http://` of `https://`, van de Git-opslagplaats te klonen.|
| `directory` | Nee | De map waarin de opslagplaats moet worden gekloond. Het pad moet niet bevatten of beginnen met '`..`'.  Als u '`.`', de opslagplaats hebt gekloond naar de map van het volume. Anders wordt de Git-opslagplaats gekloond naar een submap van de opgegeven naam binnen de volume-map. |
| `revision` | Nee | De commit-hash van de revisie om te worden gekloond. Als u niets opgeeft, de `HEAD` revisie is gekloond. |

## <a name="mount-gitrepo-volume-azure-cli"></a>Koppelpunt gitRepo volume: Azure-CLI

Een volume gitRepo koppelen bij het implementeren van containerinstanties met de [Azure CLI](/cli/azure), leveringen de `--gitrepo-url` en `--gitrepo-mount-path` parameters voor de [az container maken][az-container-create] opdracht. U kunt eventueel de map op het volume klonen in opgeven (`--gitrepo-dir`) en de hash van het doorvoeren van de revisie om te worden gekloond (`--gitrepo-revision`).

Deze opdracht wordt de Microsoft gekloond [aci-helloworld][aci-helloworld] voorbeeldtoepassing in `/mnt/aci-helloworld` in de containerexemplaar:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name hellogitrepo \
    --image mcr.microsoft.com/azuredocs/aci-helloworld \
    --dns-name-label aci-demo \
    --ports 80 \
    --gitrepo-url https://github.com/Azure-Samples/aci-helloworld \
    --gitrepo-mount-path /mnt/aci-helloworld
```

Als u wilt controleren of het volume gitRepo is gekoppeld, Open een shell in de container met [az container exec][az-container-exec] en de map te vermelden:

```console
$ az container exec --resource-group myResourceGroup --name hellogitrepo --exec-command /bin/sh
/usr/src/app # ls -l /mnt/aci-helloworld/
total 16
-rw-r--r--    1 root     root           144 Apr 16 16:35 Dockerfile
-rw-r--r--    1 root     root          1162 Apr 16 16:35 LICENSE
-rw-r--r--    1 root     root          1237 Apr 16 16:35 README.md
drwxr-xr-x    2 root     root          4096 Apr 16 16:35 app
```

## <a name="mount-gitrepo-volume-resource-manager"></a>Koppelpunt gitRepo volume: Resource Manager

Een volume gitRepo koppelen bij het implementeren van containerinstanties met een [Azure Resource Manager-sjabloon](/azure/templates/microsoft.containerinstance/containergroups), eerst vullen de `volumes` -matrix in de containergroep `properties` gedeelte van de sjabloon. Klik voor elke container in containergroep waarin u wilt koppelen de *gitRepo* volume, vullen het `volumeMounts` matrix in de `properties` sectie van de containerdefinitie van de.

De volgende Resource Manager-sjabloon maakt bijvoorbeeld een containergroep die bestaan uit een enkele container. De container wordt gekloond twee GitHub-opslagplaatsen, opgegeven door de *gitRepo* volumeblokken. Het tweede volume bevat aanvullende eigenschappen op te geven van een directory voor het klonen naar en de hash van het doorvoeren van een specifieke revisie om te klonen.

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-gitrepo.json -->
[!code-json[volume-gitrepo](~/azure-docs-json-samples/container-instances/aci-deploy-volume-gitrepo.json)]

De resulterende directorystructuur van de twee gekloonde opslagplaatsen die is gedefinieerd in de voorgaande sjabloon is:

```
/mnt/repo1/aci-helloworld
/mnt/repo2/my-custom-clone-directory
```

Zie voor een voorbeeld van de implementatie van de containers-instantie met een Azure Resource Manager-sjabloon [groepen met meerdere containers in Azure Container Instances implementeren](container-instances-multi-container-group.md).

## <a name="private-git-repo-authentication"></a>Verificatie van privé-Git-opslagplaats

Geef referenties op in de opslagplaats-URL voor het koppelen van een volume gitRepo voor een privé gitopslagplaats. Normaal gesproken referenties zijn in de vorm van een gebruikersnaam en een persoonlijk toegangstoken (PAT) waarmee wordt verleend binnen het bereik van de toegang tot de opslagplaats.

Bijvoorbeeld, de Azure CLI `--gitrepo-url` parameter voor een privé-GitHub-opslagplaats die vergelijkbaar is met de volgende (waarbij "gituser" is de GitHub-gebruikersnaam en 'abcdef1234fdsa4321abcdef' is van de gebruiker persoonlijk toegangstoken) wordt weergegeven:

```azurecli
--gitrepo-url https://gituser:abcdef1234fdsa4321abcdef@github.com/GitUser/some-private-repository
```

Geef een gebruikersnaam (u kunt "azurereposuser" gebruiken zoals in het volgende voorbeeld) in combinatie met een geldige PAT voor een Azure-opslagplaatsen Git-opslagplaats:

```azurecli
--gitrepo-url https://azurereposuser:abcdef1234fdsa4321abcdef@dev.azure.com/your-org/_git/some-private-repository
```

Voor meer informatie over persoonlijke toegangstokens van GitHub en Azure-opslagplaatsen, Zie de volgende:

GitHub: [Het maken van een persoonlijk toegangstoken voor de opdrachtregel][pat-github]

Azure-opslagplaatsen: [Persoonlijke toegangstokens voor het verifiëren van toegang maken][pat-repos]

## <a name="next-steps"></a>Volgende stappen

Informatie over het koppelen van andere typen in Azure Container Instances:

* [Een Azure-bestandsshare in Azure Containerexemplaren koppelen](container-instances-volume-azure-files.md)
* [Een volume emptyDir in Azure Containerexemplaren koppelen](container-instances-volume-emptydir.md)
* [Koppelen van een geheime volume in Azure Container Instances](container-instances-volume-secret.md)

<!-- LINKS - External -->
[aci-helloworld]: https://github.com/Azure-Samples/aci-helloworld
[pat-github]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
[pat-repos]: https://docs.microsoft.com/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
