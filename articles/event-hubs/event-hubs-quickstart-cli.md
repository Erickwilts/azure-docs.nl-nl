---
title: Een event hub maken met behulp van Azure CLI - Azure Event Hubs | Microsoft Docs
description: In deze snelstart wordt beschreven hoe u een event hub maakt met behulp van de Azure CLI en vervolgens gebeurtenissen verzendt en ontvangt met behulp van Java.
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: java
ms.topic: quickstart
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 3bc7b409d590ac096b70431ae009fed36c968307
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "66148250"
---
# <a name="quickstart-create-an-event-hub-using-azure-cli"></a>Snelstartgids: Een event hub maken met behulp van Azure CLI

Azure Event Hubs is een big data-platform voor het streamen van gegevens en een gebeurtenisopneemservice die miljoenen gebeurtenissen per seconde kan opnemen en verwerken. Event Hubs kan gebeurtenissen, gegevens of telemetrie die wordt geproduceerd door gedistribueerde software en apparaten verwerken en opslaan. Gegevens die naar een Event Hub worden verzonden, kunnen worden omgezet en opgeslagen via een provider voor realtime analytische gegevens of batchverwerking/opslagadapters. Zie [Overzicht van Event Hubs](event-hubs-about.md) en [Functies van Event Hubs](event-hubs-features.md) voor een gedetailleerd overzicht van Event Hubs.

In deze snelstart maakt u een Event Hub met behulp van Azure CLI.

## <a name="prerequisites"></a>Vereisten
U hebt een Azure-abonnement nodig om deze snelstart te voltooien. Als u nog geen abonnement hebt, [maakt u een gratis account][] voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de Azure CLI lokaal te installeren en te gebruiken, moet u voor deze zelfstudie de Azure CLI 2.0.4 of hoger uitvoeren. Voer `az --version` uit om uw versie te controleren. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren]( /cli/azure/install-azure-cli).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

De volgende stappen zijn niet vereist als u opdrachten in Cloud Shell uitvoert. Als u de CLI lokaal uitvoert, moet u de volgende stappen uitvoeren om u aan te melden bij Azure en uw huidige abonnement in te stellen:

Voer de volgende opdracht uit om u aan te melden bij Azure:

```azurecli-interactive
az login
```

Stel de context van het huidige abonnement in. Vervang `MyAzureSub` door de naam van het Azure-abonnement dat u wilt gebruiken:

```azurecli-interactive
az account set --subscription MyAzureSub
``` 

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Een resourcegroep is een logische verzameling Azure-resources. Alle resources worden geïmplementeerd en beheerd in een resourcegroep. Voer de volgende opdracht uit om een resourcegroep te maken:

```azurecli-interactive
# Create a resource group. Specify a name for the resource group.
az group create --name <resource group name> --location eastus
```

## <a name="create-an-event-hubs-namespace"></a>Een Event Hubs-naamruimte maken
Een Event Hubs-naamruimte biedt een unieke scopingcontainer, waarnaar wordt verwezen met de volledig gekwalificeerde domeinnaam (FQDN), waarin u een of meer Event Hubs maakt. Voer de volgende opdracht uit om een ​​naamruimte te maken in de resourcegroep:

```azurecli-interactive
# Create an Event Hubs namespace. Specify a name for the Event Hubs namespace.
az eventhubs namespace create --name <Event Hubs namespace> --resource-group <resource group name> -l <region, for example: East US>
```

## <a name="create-an-event-hub"></a>Een Event Hub maken
Voer de volgende opdracht uit om een Event Hub te maken:

```azurecli-interactive
# Create an event hub. Specify a name for the event hub. 
az eventhubs eventhub create --name <event hub name> --resource-group <resource group name> --namespace-name <Event Hubs namespace>
```

Gefeliciteerd! U hebt Azure CLI gebruikt om een ​​Event Hubs-naamruimte te maken en om binnen deze naamruimte een Event Hub te maken. 

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een resourcegroep, een Event Hubs-naamruimte en een Event Hub gemaakt. Zie voor stapsgewijze instructies voor het verzenden van gebeurtenissen naar (of) gebeurtenissen ontvangen van een event hub, de **verzenden en ontvangen van gebeurtenissen** zelfstudies: 

- [.NET Core](event-hubs-dotnet-standard-getstarted-send.md)
- [.NET Framework](event-hubs-dotnet-framework-getstarted-send.md)
- [Java](event-hubs-java-get-started-send.md)
- [Python](event-hubs-python-get-started-send.md)
- [Node.js](event-hubs-node-get-started-send.md)
- [Go](event-hubs-go-get-started-send.md)
- [C (alleen verzenden)](event-hubs-c-getstarted-send.md)
- [Apache Storm (alleen reecive)](event-hubs-storm-getstarted-receive.md)

[maakt u een gratis account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[Install the Azure CLI]: /cli/azure/install-azure-cli
[az group create]: /cli/azure/group#az_group_create
[fully qualified domain name]: https://wikipedia.org/wiki/Fully_qualified_domain_name
