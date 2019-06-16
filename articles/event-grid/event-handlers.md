---
title: Azure Event Grid-gebeurtenis-handlers
description: Beschrijving van ondersteunde gebeurtenis-handlers voor Azure Event Grid
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/21/2019
ms.author: spelluru
ms.openlocfilehash: 6093e1017af2fb8c54eaf1c3192f937172567982
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67080561"
---
# <a name="event-handlers-in-azure-event-grid"></a>Gebeurtenis-handlers in Azure Event Grid

Een gebeurtenis-handler is de plek waar de gebeurtenis wordt verzonden. De handler duurt enkele verdere actie voor het verwerken van de gebeurtenis. Meerdere Azure-services worden automatisch geconfigureerd om gebeurtenissen te verwerken. U kunt ook een WebHook gebruiken voor het verwerken van gebeurtenissen. De WebHook hoeft niet te worden gehost in Azure om gebeurtenissen te verwerken. Event Grid biedt alleen ondersteuning voor HTTPS-WebHook-eindpunten.

In dit artikel bevat koppelingen naar inhoud voor elke gebeurtenis-handler.

## <a name="azure-automation"></a>Azure Automation

Azure Automation gebruiken om gebeurtenissen te verwerken met geautomatiseerde runbooks.

|Titel  |Description  |
|---------|---------|
|[Zelfstudie: Azure Automation met Event Grid en Microsoft Teams](ensure-tags-exists-on-new-virtual-machines.md) |Maak een virtuele machine, waarmee een gebeurtenis wordt verzonden. De gebeurtenis activeert een Automation-runbook dat labels van de virtuele machine en een bericht dat wordt verzonden naar een Microsoft Teams-kanaal wordt geactiveerd. |

## <a name="azure-functions"></a>Azure Functions

Gebruik Azure Functions voor serverloze reactie op gebeurtenissen.

Wanneer u Azure Functions als de handler gebruikt, gebruikt u de Event Grid-trigger in plaats van algemene HTTP-triggers. Functie-triggers van Event Grid worden namelijk automatisch gevalideerd. Bij gebruik van algemene HTTP-triggers moet u een [validatie-antwoord](security-authentication.md#webhook-event-delivery) implementeren.

|Titel  |Description  |
|---------|---------|
| [Trigger Gebeurtenisraster voor Azure Functions](../azure-functions/functions-bindings-event-grid.md) | Overzicht van het gebruik van de trigger van Event Grid in functies. |
| [Zelfstudie: automatiseren formaat van geüploade afbeeldingen met behulp van Event Grid](resize-images-on-storage-blob-upload-event.md) | Gebruikers uploaden afbeeldingen via web-app naar storage-account. Wanneer een opslag-blob wordt gemaakt, wordt in Event Grid een gebeurtenis verzendt naar de functie-app vergroot / de geüploade installatiekopie verkleint. |
| [Zelfstudie: big data streamen naar een datawarehouse](event-grid-event-hubs-integration.md) | Wanneer Event Hubs een bestand vastleggen maakt, verzendt Event Grid een gebeurtenis aan een functie-app. De app haalt de Capture-bestand en migreert gegevens naar een datawarehouse. |
| [Zelfstudie: Azure Service Bus met voorbeelden van de Azure Event Grid-integratie](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid verzendt berichten van Service Bus-onderwerp functioneren app en de logische app. |

## <a name="event-hubs"></a>Event Hubs

Gebruik Event Hubs als uw oplossing opgehaald gebeurtenissen sneller dan de gebeurtenissen kunnen worden verwerkt. Uw toepassing verwerkt de gebeurtenissen van Event Hubs op het eigen planning. U kunt uw gebeurtenisverwerking voor het afhandelen van de inkomende gebeurtenissen op te schalen.

Eventhubs kunnen fungeren als een bron van gebeurtenis- of gebeurtenis-handler. Het volgende artikel wordt beschreven hoe u Event Hubs gebruikt als een handler.

|Titel  |Description  |
|---------|---------|
| [Snelstartgids: aangepaste gebeurtenissen routeren naar Azure Event Hubs met Azure CLI en Event Grid](custom-event-to-eventhub.md) | Een aangepaste gebeurtenis verzendt naar een event hub voor verwerking door een toepassing. |
| [Resource Manager-sjabloon: aangepast onderwerp en het eindpunt van de Event Hubs](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-event-hubs-handler)| Een Resource Manager-sjabloon maakt u een abonnement voor een aangepast onderwerp. Verzendt gebeurtenissen naar een Azure Event Hubs. |

Zie voor meer voorbeelden van Event Hubs als een bron [Event Hubs bron](event-sources.md#event-hubs).

## <a name="hybrid-connections"></a>Hybride verbindingen

Hybride Azure Relay-verbindingen gebruiken voor het verzenden van gebeurtenissen voor toepassingen die zich binnen een bedrijfsnetwerk en hoeft niet een eindpunt dat openbaar toegankelijk is.

|Titel  |Description  |
|---------|---------|
| [Zelfstudie: gebeurtenissen verzenden naar de hybride verbinding](custom-event-to-hybrid-connection.md) | Verzendt een aangepaste gebeurtenis naar een bestaande hybride verbinding voor verwerking door een listener-toepassing. |

## <a name="logic-apps"></a>Logic Apps

Logische Apps gebruiken voor het automatiseren van bedrijfsprocessen voor het reageren op gebeurtenissen.

|Titel  |Description  |
|---------|---------|
| [Zelfstudie: wijzigingen van de virtuele machine met Azure Event Grid en Logic Apps bewaken](monitor-virtual-machine-changes-event-grid-logic-app.md) | Een logische app bewaakt van wijzigingen aan een virtuele machine en e-mailberichten over deze wijzigingen verzendt. |
| [Zelfstudie: verzenden van e-mailmeldingen over Azure IoT Hub-gebeurtenissen met Logic Apps](publish-iot-hub-events-to-logic-apps.md) | Een logische app verzendt een e-mailmelding telkens wanneer een apparaat wordt toegevoegd aan uw IoT-hub. |
| [Zelfstudie: Azure Service Bus met voorbeelden van de Azure Event Grid-integratie](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid verzendt berichten van Service Bus-onderwerp functioneren app en de logische app. |

## <a name="service-bus-queue-preview"></a>Service Bus-wachtrij (Preview)

Service Bus gebruiken als een gebeurtenis-handler voor het routeren van de gebeurtenissen in Event Grid rechtstreeks naar Service Bus-wachtrijen voor gebruik in scenario's met buffer of opdracht en controle in bedrijfstoepassingen. De Preview-versie werkt niet met Service Bus-onderwerpen en sessies, maar het werkt met alle lagen van Service Bus-wachtrijen.

Opmerking: tijdens het Service Bus een handler is in openbare preview, moet u de CLI of PowerShell uitbreiding installeren wanneer u deze Gebeurtenisabonnementen maken.

### <a name="install-extension-for-azure-cli"></a>Extensie voor Azure CLI installeren

Voor Azure CLI, moet u de [Event Grid-extensie](/cli/azure/azure-cli-extensions-list).

In [CloudShell](/azure/cloud-shell/quickstart):

* Als u de uitbreiding eerder hebt geïnstalleerd, werk deze bij met `az extension update -n eventgrid`.
* Als u de uitbreiding eerder nog niet hebt geïnstalleerd, installeert u deze met behulp van `az extension add -n eventgrid`.

Voor een lokale installatie:

1. [De Azure CLI installeren](/cli/azure/install-azure-cli). Zorg ervoor dat u de meest recente versie hebt door te controleren met `az --version`.
1. Verwijder eerdere versies van de uitbreiding met de `az extension remove -n eventgrid`.
1. Installeer de `eventgrid` uitbreiding met `az extension add -n eventgrid`.

### <a name="install-module-for-powershell"></a>Module voor PowerShell installeren

Voor PowerShell, moet u de [AzureRM.EventGrid module](https://www.powershellgallery.com/packages/AzureRM.EventGrid/0.4.1-preview).

In [CloudShell](/azure/cloud-shell/quickstart-powershell):

* Installeren van de module met `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`.

Voor een lokale installatie:

1. Open de PowerShell-console als beheerder.
1. Installeren van de module met `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`.

Als de `-AllowPrerelease` parameter is niet beschikbaar, gebruikt u de volgende stappen uit:

1. Voer `Install-Module PowerShellGet -Force` uit.
1. Voer `Update-Module PowerShellGet` uit.
1. Sluit de PowerShell-console.
1. Start opnieuw op PowerShell als beheerder.
1. Installeren van de module `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`.

### <a name="using-cli-to-add-a-service-bus-handler"></a>Een Service Bus-handler toevoegen met behulp van CLI

Voor Azure CLI het volgende voorbeeld is geabonneerd en een Event Grid-onderwerp verbindt met een Service Bus-wachtrij:

```azurecli-interactive
# If you haven't already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid event-subscription create \
    --name <my-event-subscription> \
    --source-resource-id /subscriptions/{SubID}/resourceGroups/{RG}/providers/Microsoft.EventGrid/topics/topic1 \
    --endpoint-type servicebusqueue \
    --endpoint /subscriptions/{SubID}/resourceGroups/TestRG/providers/Microsoft.ServiceBus/namespaces/ns1/queues/queue1
```

## <a name="queue-storage"></a>Queue Storage

Queue storage gebruiken voor het ontvangen van gebeurtenissen die moeten worden opgehaald. U kunt Queue storage gebruiken wanneer u een langlopende proces hebt dat duurt te lang om te reageren. Door het verzenden van gebeurtenissen naar Queue storage, de app kunt ophalen en verwerken van gebeurtenissen op een eigen planning.

|Titel  |Description  |
|---------|---------|
| [Snelstartgids: aangepaste gebeurtenissen routeren naar Azure Queue storage met Azure CLI en Event Grid](custom-event-to-queue-storage.md) | Beschrijft hoe u aangepaste gebeurtenissen verzenden naar een Queue storage. |

## <a name="webhooks"></a>WebHooks

Webhooks gebruiken voor aanpasbare eindpunten die op gebeurtenissen reageren.

|Titel  |Description  |
|---------|---------|
| QuickStart: maken en routeren van aangepaste gebeurtenissen met - [Azure CLI](custom-event-quickstart.md), [PowerShell](custom-event-quickstart-powershell.md), en [portal](custom-event-quickstart-portal.md). | Laat zien hoe u aangepaste gebeurtenissen verzenden naar een WebHook. |
| Snelstartgids: gebeurtenissen van Blob storage naar een aangepaste web-eindpunt met - routeren [Azure CLI](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json), [PowerShell](../storage/blobs/storage-blob-event-quickstart-powershell.md?toc=%2fazure%2fevent-grid%2ftoc.json), en [portal](blob-event-quickstart-portal.md). | Laat zien hoe voor het verzenden van gebeurtenissen van blob storage naar een WebHook. |
| [Snelstartgids: container registry gebeurtenissen verzenden](../container-registry/container-registry-event-grid-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Laat zien hoe u Azure CLI gebruiken voor het verzenden van gebeurtenissen voor Container Registry. |
| [Overzicht: gebeurtenissen naar een HTTP-eindpunt ontvangen](receive-events.md) | Beschrijft hoe u een HTTP-eindpunt voor het ontvangen van gebeurtenissen van een gebeurtenisabonnement, en ontvangen en deserialisatie van gebeurtenissen te valideren. |

## <a name="next-steps"></a>Volgende stappen

* Zie [Een inleiding tot Event Grid](overview.md) voor een inleiding tot Event Grid.
* Als u wilt snel aan de slag met Event Grid, Zie [aangepaste gebeurtenissen maken en routeren met Azure Event Grid](custom-event-quickstart.md).
