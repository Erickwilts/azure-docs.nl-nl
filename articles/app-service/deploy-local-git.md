---
title: Implementeren vanuit lokale Git-repo - Azure App Service
description: Informatie over het inschakelen van lokale Git-implementatie in Azure App Service.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.author: dariagrigoriu;cephalin
ms.custom: seodec18
ms.openlocfilehash: b879036dcd79901cb634fa197932e833cb22d12a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "65956056"
---
# <a name="local-git-deployment-to-azure-app-service"></a>Lokale Git-implementatie op de Azure App Service

In deze gebruiksaanwijzing ziet u hoe u implementeert uw code wordt [Azure App Service](overview.md) vanuit een Git-opslagplaats op uw lokale computer.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

De stappen in deze gebruiksaanwijzing:

* [Installeer Git](https://www.git-scm.com/downloads).
* Onderhouden een lokale Git-opslagplaats met code die u wilt implementeren.

Als u een opslagplaats met voorbeeld wilt volgen, voer de volgende opdracht in uw lokale terminalvenster:

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world.git
```

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-with-kudu-builds"></a>Implementeren met Kudu builds

De eenvoudigste manier om lokale Git-implementatie voor uw app met de Kudu-build-server in te schakelen, is met de Cloud Shell.

### <a name="configure-a-deployment-user"></a>Een implementatiegebruiker configureren

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="enable-local-git-with-kudu"></a>Lokale Git met Kudu inschakelen

Als u lokale Git-implementatie voor uw app met de Kudu-build-server wilt inschakelen, voert u [`az webapp deployment source config-local-git`](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-local-git) in de Cloud Shell uit.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group <group_name>
```

Voer voor het maken van een app met Git in plaats daarvan [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) in de Cloud Shell uit met de parameter `--deployment-local-git`.

```azurecli-interactive
az webapp create --name <app_name> --resource-group <group_name> --plan <plan_name> --deployment-local-git
```

De uitvoer na opdracht `az webapp create` moet er ongeveer uitzien zoals hieronder is weergegeven:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="deploy-your-project"></a>Uw project implementeren

Voeg, eenmaal terug in het _lokale terminalvenster_, een externe Azure-instantie toe aan uw lokale Git-opslagplaats. Vervang _\<url >_ met de URL van de externe Git-instantie die u hebt verkregen via [Git inschakelen voor uw app](#enable-local-git-with-kudu).

```bash
git remote add azure <url>
```

Push naar de externe Azure-instantie om uw app te implementeren met de volgende opdracht. Zorg er als u om een wachtwoord wordt gevraagd voor dat u het wachtwoord dat u hebt gemaakt bij [Een implementatiegebruiker configureren](#configure-a-deployment-user) gebruikt en niet het wachtwoord dat u gebruikt om u aan te melden bij Azure Portal.

```bash
git push azure master
```

U ziet mogelijk runtime-specifieke automatisering in de uitvoer, zoals MSBuild voor ASP.NET, `npm install` voor Node.js, en `pip install` voor Python. 

Blader naar uw app om te controleren dat de inhoud wordt geïmplementeerd.

## <a name="deploy-with-azure-devops-builds"></a>Implementeren met Azure DevOps-builds

> [!NOTE]
> Voor App Service voor het maken van de benodigde Azure-pijplijnen in uw organisatie Azure DevOps-Services, moet uw Azure-account de rol van **eigenaar** in uw Azure-abonnement.
>

Om in te schakelen lokale Git-implementatie voor uw app met de Kudu-build-server, gaat u naar uw app in de [Azure-portal](https://portal.azure.com).

In het linkernavigatievenster van de app-pagina, klikt u op **Implementatiecentrum** > **lokale Git** > **doorgaan**.

![](media/app-service-deploy-local-git/portal-enable.png)

Klik op **Azure pijplijnen (Preview)** > **blijven**.

![](media/app-service-deploy-local-git/pipeline-builds.png)

In de **configureren** pagina, configureren van een nieuwe Azure DevOps-organisatie of een bestaande organisatie opgeven. Wanneer u klaar bent, klikt u op **Doorgaan**.

> [!NOTE]
> Als u gebruiken van een bestaande Azure DevOps-organisatie die niet wordt vermeld wilt, moet u [de Services van Azure DevOps-organisatie koppelen aan uw Azure-abonnement](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

Afhankelijk van de [prijscategorie](https://azure.microsoft.com/pricing/details/app-service/plans/) van uw App Service-plan, ziet u mogelijk ook een **implementeren voor fasering** pagina. Kies of u wilt inschakelen implementatiesites gebruiken en klik vervolgens op **doorgaan**.

In de **Samenvatting** pagina, controleer uw opties en klik op **Voltooien**.

Het duurt een paar minuten voor de Services van Azure DevOps-organisatie gereed is. Wanneer dit klaar is, kopieert u de URL van de Git-opslagplaats in het midden van de implementatie.

![](media/app-service-deploy-local-git/vsts-repo-ready.png)

Voeg, eenmaal terug in het _lokale terminalvenster_, een externe Azure-instantie toe aan uw lokale Git-opslagplaats. Vervang  _\<url >_ met de URL die u hebt verkregen via de laatste stap.

```bash
git remote add vsts <url>
```

Push naar de externe Azure-instantie om uw app te implementeren met de volgende opdracht. Wanneer u hierom wordt gevraagd door Git Credential Manager, meldt u zich aan met uw gebruiker visualstudio.com. Zie voor aanvullende verificatiemethoden [Services van Azure DevOps-verificatieoverzicht](/vsts/git/auth-overview?view=vsts).

```bash
git push vsts master
```

Als de implementatie is voltooid, kunt u de voortgang van de build op vinden `https://<vsts_account>.visualstudio.com/<project_name>/_build` en de voortgang van de implementatie op `https://<vsts_account>.visualstudio.com/<project_name>/_release`.

Blader naar uw app om te controleren dat de inhoud wordt geïmplementeerd.

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="troubleshooting-kudu-deployment"></a>Problemen met Kudu-implementatie oplossen

Hier volgen algemene fouten of problemen bij het publiceren naar een App Service-app in Azure met behulp van Git:

---
**Symptoom**: `Unable to access '[siteURL]': Failed to connect to [scmAddress]`

**Oorzaak**: Deze fout kan optreden als de app niet actief en werkend.

**Resolutie**: De app te starten in Azure portal. GIT-implementatie is niet beschikbaar wanneer de Web-App is gestopt.

---
**Symptoom**: `Couldn't resolve host 'hostname'`

**Oorzaak**: Deze fout kan optreden als de gegevens hebt ingevoerd bij het maken van de 'azure' remote onjuist is.

**Resolutie**: Gebruik de `git remote -v` opdracht om een lijst van alle remotes, samen met de bijbehorende URL. Controleer of dat de URL voor de externe 'azure' juist is. Indien nodig, verwijderen en opnieuw maken van deze externe met behulp van de juiste URL.

---
**Symptoom**: `No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'master'.`

**Oorzaak**: Deze fout kan optreden als u geen dat een vertakking tijdens opgeeft `git push`, of als u dit nog niet hebt ingesteld de `push.default` waarde in de `.gitconfig`.

**Resolutie**: Voer `git push` opnieuw op te geven van de master-vertakking. Bijvoorbeeld:

```bash
git push azure master
```

---
**Symptoom**: `src refspec [branchname] does not match any.`

**Oorzaak**: Deze fout kan optreden als u probeert te pushen naar een vertakking dan master in de 'azure' remote.

**Resolutie**: Voer `git push` opnieuw op te geven van de master-vertakking. Bijvoorbeeld:

```bash
git push azure master
```

---
**Symptoom**: `RPC failed; result=22, HTTP code = 5xx.`

**Oorzaak**: Deze fout kan optreden als u probeert een grote git-opslagplaats pushen via HTTPS.

**Resolutie**: Wijzigen van de git-configuratie op de lokale computer naar de postBuffer groter maken

```bash
git config --global http.postBuffer 524288000
```

---
**Symptoom**: `Error - Changes committed to remote repository but your web app not updated.`

**Oorzaak**: Deze fout kan optreden als u een Node.js-app met implementeert een _package.json_ -bestand dat aanvullende vereiste modules specificeert.

**Resolutie**: Extra berichten met "npm ERR!" voordat u deze fout moet worden vastgelegd en kunt aanvullende context te bieden over de fout. Hier volgen enkele bekende oorzaken van deze fout en de bijbehorende 'npm ERR!" Bericht:

* **Onjuist gevormd package.json-bestand**: npm ERR! Afhankelijkheden kan niet worden gelezen.
* **Systeemeigen module die een binaire distributie voor Windows geen**:

  * `npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1`

      OR
  * `npm ERR! [modulename@version] preinstall: \make || gmake\`

## <a name="additional-resources"></a>Aanvullende resources

* [Project Kudu-documentatie](https://github.com/projectkudu/kudu/wiki)
* [Continue implementatie in Azure App Service](deploy-continuous-deployment.md)
* [Voorbeeld: Web-App maken en code implementeren vanuit een lokale Git-opslagplaats (Azure CLI)](./scripts/cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json)
* [Voorbeeld: Web-App maken en code implementeren vanuit een lokale Git-opslagplaats (PowerShell)](./scripts/powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json)
