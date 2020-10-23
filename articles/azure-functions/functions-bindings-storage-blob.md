---
title: Trigger-en bindingen voor Azure Blob Storage voor Azure Functions
description: Meer informatie over het gebruik van de Azure Blob Storage-trigger en bindingen in Azure Functions.
author: craigshoemaker
ms.topic: reference
ms.date: 02/13/2020
ms.author: cshoe
ms.openlocfilehash: e56d1add36d4296526348d12d7c0b6eb03108f27
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2020
ms.locfileid: "92104356"
---
# <a name="azure-blob-storage-bindings-for-azure-functions-overview"></a>Azure Blob-opslag bindingen voor Azure Functions-overzicht

Azure Functions integreert met [Azure Storage](../storage/index.yml) via [Triggers en bindingen](./functions-triggers-bindings.md). Door te integreren met Blob Storage kunt u functies bouwen die reageren op wijzigingen in BLOB-gegevens en lees-en schrijf waarden.

| Bewerking | Type |
|---------|---------|
| Een functie uitvoeren als wijzigingen in Blob Storage-gegevens | [Trigger](./functions-bindings-storage-blob-trigger.md) |
| Blob Storage-gegevens in een functie lezen | [Invoer binding](./functions-bindings-storage-blob-input.md) |
| Een functie toestaan Blob Storage-gegevens te schrijven |[Uitvoer binding](./functions-bindings-storage-blob-output.md) |

## <a name="add-to-your-functions-app"></a>Toevoegen aan uw functions-app

### <a name="functions-2x-and-higher"></a>Functions 2.x en hoger

Voor het werken met de trigger en bindingen moet u verwijzen naar het juiste pakket. Het NuGet-pakket wordt gebruikt voor .NET-klassen bibliotheken terwijl de uitbreidings bundel wordt gebruikt voor alle andere toepassings typen.

| Taal                                        | Toevoegen door...                                   | Opmerkingen 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | Het [NuGet-pakket]installeren, versie 3. x | |
| C#-script, Java, java script, Python, Power shell | De [uitbreidings bundel] registreren          | De [extensie voor Azure-Hulpprogram ma's](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) wordt aanbevolen voor gebruik met Visual Studio code. |
| C#-script (alleen online in Azure Portal)         | Een binding toevoegen                            | Zie [uw extensies bijwerken]om bestaande bindings extensies bij te werken zonder uw functie-app opnieuw te publiceren. |

[core tools]: ./functions-run-local.md
[uitbreidings bundel]: ./functions-bindings-register.md#extension-bundles
[NuGet-pakket]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage
[Uw extensies bijwerken]: ./functions-bindings-register.md
[Azure Tools extension]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>Functions 1.x

Functions 1. x apps hebben automatisch een verwijzing naar het pakket [micro soft. Azure. webjobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) , versie 2. x.

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="next-steps"></a>Volgende stappen

- [Een functie uitvoeren wanneer Blob Storage-gegevens worden gewijzigd](./functions-bindings-storage-blob-trigger.md)
- [Blob Storage-gegevens lezen wanneer een functie wordt uitgevoerd](./functions-bindings-storage-blob-input.md)
- [Blob Storage-gegevens van een functie schrijven](./functions-bindings-storage-blob-output.md)