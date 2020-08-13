---
title: bestand opnemen
description: bestand opnemen
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 07/10/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: d76bcf7e7b251f8c371658d8fca34f4162fe0b87
ms.sourcegitcommit: 2ffa5bae1545c660d6f3b62f31c4efa69c1e957f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/11/2020
ms.locfileid: "88080042"
---
Een [web-app](../articles/app-service/overview.md#app-service-on-linux) maken in het App Service-plan `myAppServicePlan`. 

In Cloud Shell kunt u de opdracht [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest) gebruiken. Vervang in het volgende voorbeeld `<app-name>` door een unieke naam (geldige tekens zijn `a-z`, `0-9`, en `-`). De runtime is ingesteld op `RUBY|2.6.2`. Voer [`az webapp list-runtimes --linux`](/cli/azure/webapp?view=azure-cli-latest) uit als u alle ondersteunde runtimes wilt zien. 

```azurecli-interactive
# Bash
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --runtime "RUBY|2.6.2" --deployment-local-git
# PowerShell
az --% webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --runtime "RUBY|2.6.2" --deployment-local-git
```

Wanneer de web-app is gemaakt, toont de Azure CLI soortgelijke uitvoer als in het volgende voorbeeld:

<pre>
Local git is configured with url of 'https://&lt;username&gt;@&lt;app-name&gt;.scm.azurewebsites.net/&lt;app-name&gt;.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "&lt;app-name&gt;.azurewebsites.net",
  "deploymentLocalGitUrl": "https://&lt;username&gt;@&lt;app-name&gt;.scm.azurewebsites.net/&lt;app-name&gt;.git",
  "enabled": true,
  &lt; JSON data removed for brevity. &gt;
}
</pre>

U hebt een nieuwe lege web-app gemaakt, met Git-implementatie ingeschakeld.

> [!NOTE]
> De URL van de externe Git wordt weergegeven in de eigenschap `deploymentLocalGitUrl`, met de indeling `https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git`. Sla deze URL op, want u hebt deze later nodig.
>
