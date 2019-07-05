---
title: Azure CLI-voorbeeldscript - Een SignalR-service maken met App Service
description: Azure CLI-voorbeeldscript - Een SignalR-service maken met App Service
author: sffamily
ms.service: signalr
ms.devlang: azurecli
ms.topic: sample
ms.date: 04/20/2018
ms.author: zhshang
ms.custom: mvc
ms.openlocfilehash: d0f0747aa393475265be4aeb9ca05000fbd5b97b
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565741"
---
# <a name="create-a-signalr-service-with-an-app-service"></a>Een SignalR-service maken met App Service

Met dit voorbeeldscript maakt u een nieuwe resource voor de Azure SignalR-service, die vervolgens wordt gebruikt om realtime updates van inhoud te pushen naar clients. Met dit script worden ook een nieuwe web-app en een App Service-plan toegevoegd voor het hosten van een web-app van ASP.NET Core die de SignalR-service gebruikt. De web-app wordt geconfigureerd met een app-instelling met de naam *AzureSignalRConnectionString* om verbinding te maken met de nieuwe resource voor de SignalR-service.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om de versie te bekijken. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeldscript

Dit script maakt gebruik van de extensie *signalr* voor Azure CLI. Voer de volgende opdracht uit om de extensie *signalr* voor Azure CLI te installeren voordat u dit voorbeeldscript gaat gebruiken:

```azurecli-interactive
az extension add -n signalr
```

[!code-azurecli-interactive[main](../../../cli_scripts/azure-signalr/create-signalr-with-app-service/create-signalr-with-app-service.sh "Create a new Azure SignalR Service and Web App")]

Noteer de naam die wordt gegenereerd voor de nieuwe resourcegroep. Deze naam wordt weergegeven in de uitvoer. U hebt deze naam nodig als u later alle groepsresources wilt verwijderen.

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Uitleg van het script

Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht. In dit script worden de volgende opdrachten gebruikt:

| Opdracht | Opmerkingen |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Hiermee wordt een resourcegroep gemaakt waarin alle resources worden opgeslagen. |
| [az signalr create](/cli/azure/signalr#az-signalr-create) | Hiermee maakt u een resource voor de Azure SignalR-service. |
| [az signalr key list](/cli/azure/signalr/key#az-signalr-key-list) | Hiermee vraagt u de sleutels op die door uw toepassing worden gebruikt om realtime updates van inhoud te pushen met SignalR. |
| [az appservice plan create](/cli/azure/appservice/plan#az-appservice-plan-create) | Hiermee maakt u een Azure App Service-plan voor het hosten van web-apps. |
| [az webapp create](/cli/azure/webapp#az-webapp-create) | Hiermee maakt u een Azure-web-app met behulp van het App Service-hostingplan. |
| [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#az-webapp-config-appsettings-set) | Hiermee voegt u een nieuwe app-instelling toe voor de web-app. Deze app-instelling wordt gebruikt voor het opslaan van de verbindingsreeks van SignalR. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](/cli/azure) voor meer informatie over de Azure CLI.

Extra CLI-voorbeeldscripts voor de Azure SignalR-service vindt u in de [documentatie van de Azure SignalR-service](../signalr-reference-cli.md).
