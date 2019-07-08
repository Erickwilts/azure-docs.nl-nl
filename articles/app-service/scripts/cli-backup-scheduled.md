---
title: Voorbeeld van Azure CLI-script - Een geplande back-up voor een app maken | Microsoft Docs
description: Voorbeeld van Azure CLI-script - Een geplande back-up voor een app maken
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: sample
ms.date: 12/11/2017
ms.author: msangapu
ms.reviewer: cephalin
ms.custom: seodec18
ms.openlocfilehash: 48ac455da32084f0ea2c5cf4f9f8b2d2cdc09719
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67613503"
---
# <a name="create-a-scheduled-backup-for-an-app-service-app-using-cli"></a>Een geplande back-up voor een App Service-app maken met CLI

Met dit voorbeeldscript wordt een app gemaakt in App Service, inclusief de bijbehorende resources, en wordt er vervolgens een geplande back-up van de app gemaakt. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, hebt u Azure CLI versie 2.0 of hoger nodig. Voer `az --version` uit om de versie te bekijken. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Voorbeeldscript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/backup-scheduled/backup-scheduled.sh?highlight=3-7 "Create a scheduled backup for an app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt. Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create) | Hiermee wordt een resourcegroep gemaakt waarin alle resources worden opgeslagen. |
| [`az storage account create`](/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create) | Hiermee maakt u een opslagaccount. |
| [`az storage container create`](/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-create) | Hiermee wordt een Azure-opslagcontainer gemaakt. |
| [`az storage container generate-sas`](/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-generate-sas) | Hiermee wordt een SAS-token gegenereerd voor een Azure-opslagcontainer.  |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) | Hiermee maakt u een App Service-plan. |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) | Hiermee maakt u een App Service-app. |
| [`az webapp config backup update`](/cli/azure/webapp/config/backup?view=azure-cli-latest#az-webapp-config-backup-update) | Hiermee configureert u het nieuwe back-upschema voor een App Service-app. |
| [`az webapp config backup show`](/cli/azure/webapp/config/backup?view=azure-cli-latest#az-webapp-config-backup-show) | Geeft het back-upschema voor een App Service-app weer. |
| [`az webapp config backup list`](/cli/azure/webapp/config/backup?view=azure-cli-latest#az-webapp-config-backup-list) | Hiermee haalt u een lijst op van back-ups voor een App Service-app. |

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [documentatie van Azure CLI](https://docs.microsoft.com/cli/azure) voor meer informatie over de Azure CLI.

Meer voorbeelden van App Service CLI-scripts vindt u in de [documentatie van Azure App Service](../samples-cli.md).
