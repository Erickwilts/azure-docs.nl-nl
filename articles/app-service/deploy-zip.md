---
title: Code implementeren met een ZIP-of WAR-bestand-Azure App Service | Microsoft Docs
description: Meer informatie over het implementeren van uw app naar Azure App Service met een ZIP-bestand (of een WAR-bestand voor Java-Ontwikkel aars).
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/12/2019
ms.author: cephalin
ms.reviewer: sisirap
ms.custom: seodec18
ms.openlocfilehash: 470caa37067b8429fd16e508c43383e4d3db3d41
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "68954059"
---
# <a name="deploy-your-app-to-azure-app-service-with-a-zip-or-war-file"></a>Uw app implementeren voor Azure App Service met een ZIP-of WAR-bestand

In dit artikel leest u hoe u een ZIP-bestand of een WAR-bestand gebruikt om uw web-app te implementeren op [Azure app service](overview.md). 

Bij deze ZIP-bestands implementatie wordt gebruikgemaakt van dezelfde kudu-service die doorlopende implementaties op basis van integratie aanstuurt. Kudu ondersteunt de volgende functionaliteit voor de implementatie van ZIP-bestanden: 

- Het verwijderen van bestanden van een vorige implementatie is overgebleven.
- Optie voor het inschakelen van het standaard constructie proces, dat package Restore bevat.
- [Aanpassing](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings)van de implementatie, waaronder het uitvoeren van implementatie scripts.  
- Implementatie Logboeken. 
- De maximale bestands grootte van 2048 MB.

Zie onze [Kudu-documentatie](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file) voor meer informatie.

De WAR-bestands implementatie implementeert uw [War](https://wikipedia.org/wiki/WAR_(file_format)) -bestand op app service om uw Java-Web-app uit te voeren. Zie [war-bestand implementeren](#deploy-war-file).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Als u de stappen in dit artikel wilt uitvoeren:

* [Maak een App Service-app](/azure/app-service/), of gebruik een app die u hebt gemaakt voor een andere zelfstudie.

## <a name="create-a-project-zip-file"></a>Een ZIP-bestand van het project maken

>[!NOTE]
> Als u de bestanden in een ZIP-bestand hebt gedownload, pakt u eerst de bestanden uit. Als u bijvoorbeeld een ZIP-bestand van GitHub hebt gedownload, kunt u dat bestand niet implementeren. GitHub voegt extra geneste directory's toe, die niet samen werken met App Service. 
>

Navigeer in een lokaal Terminal venster naar de hoofdmap van uw app-project. 

Deze map moet het invoer bestand bevatten naar uw web-app, zoals _index. html_, _index. php_en _app. js_. Het kan ook pakket beheer bestanden bevatten zoals _project. json_, _Composer. json_, _package. json_, _Bower. json_en _Requirements. txt_.

Maak een ZIP-archief van alle bestanden in uw project. De volgende opdracht maakt gebruik van het standaardhulpprogramma in de terminal:

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

[!INCLUDE [Deploy ZIP file](../../includes/app-service-web-deploy-zip.md)]

## <a name="deploy-zip-file-with-azure-cli"></a>ZIP-bestand implementeren met Azure CLI

Zorg ervoor dat uw Azure CLI-versie 2.0.21 of hoger is. Als u wilt zien welke versie u hebt `az --version` , voert u de opdracht uit in het Terminal venster.

Implementeer het geüploade ZIP-bestand naar uw web-app met behulp van de opdracht [AZ webapp Deployment source config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-zip) .  

In het volgende voor beeld wordt het ZIP-bestand dat u hebt geüpload, geïmplementeerd. Wanneer u een lokale installatie van Azure CLI gebruikt, geeft u het pad op naar het lokale `--src`zip-bestand voor.

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResourceGroup --name <app_name> --src clouddrive/<filename>.zip
```

Met deze opdracht worden de bestanden en mappen uit het ZIP-bestand geïmplementeerd in de standaardmap voor de App Service-toepassing (`\home\site\wwwroot`) en wordt de app opnieuw opgestart.

De implementatie-engine veronderstelt standaard dat een ZIP-bestand gereed is om te worden uitgevoerd, en voert geen automatisering op basis van builds uit. Als u dezelfde build-automatisering wilt inschakelen als in een [Git](deploy-local-git.md)-implementatie `SCM_DO_BUILD_DURING_DEPLOYMENT` , stelt u de app-instelling in door de volgende opdracht uit te voeren in de [Cloud shell](https://shell.azure.com):

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings SCM_DO_BUILD_DURING_DEPLOYMENT=true
```



Zie onze [Kudu-documentatie](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file-or-url) voor meer informatie.

[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]  

## <a name="deploy-war-file"></a>WAR-bestand implementeren

Als u een WAR-bestand wilt implementeren op App Service, stuurt u `https://<app_name>.scm.azurewebsites.net/api/wardeploy`een post-aanvraag naar. Het WAR-bestand moet zijn opgenomen in de hoofdtekst van de POST-aanvraag. De implementatiereferenties voor uw app moet u opgegeven in de aanvraag met behulp van HTTP-basisverificatie.

Voor de HTTP-basis verificatie hebt u uw App Service implementatie referenties nodig. Zie [referenties voor gebruikers niveau instellen en opnieuw](deploy-configure-credentials.md#userscope)instellen om te zien hoe u uw implementatie referenties kunt instellen.

### <a name="with-curl"></a>Met krul

In het volgende voor beeld wordt het gereedschap krul gebruikt om een WAR-bestand te implementeren. Vervang de tijdelijke `<username>`aanduidingen `<war-file-path>`, en `<app-name>`. Wanneer u wordt gevraagd door krul, typt u het wacht woord.

```bash
curl -X POST -u <username> --data-binary @"<war-file-path>" https://<app_name>.scm.azurewebsites.net/api/wardeploy
```

### <a name="with-powershell"></a>Met PowerShell

In het volgende voor beeld wordt het WAR-bestand geüpload [-AzWebapp](/powershell/module/az.websites/publish-azwebapp) . Vervang de tijdelijke `<group-name>`aanduidingen `<app-name>`, en `<war-file-path>`.

```powershell
Publish-AzWebapp -ResourceGroupName <group-name> -Name <app-name> -ArchivePath <war-file-path>
```

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="next-steps"></a>Volgende stappen

Probeer [te implementeren naar Azure met git](deploy-local-git.md)voor meer geavanceerde implementatie scenario's. Met op Git gebaseerde implementatie naar Azure kunt u versie beheer, pakket herstel, MSBuild en meer.

## <a name="more-resources"></a>Meer bronnen

* [Kudu: Implementeren vanuit een zip-bestand](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)
* [Implementatie referenties Azure App Service](deploy-ftp.md)
