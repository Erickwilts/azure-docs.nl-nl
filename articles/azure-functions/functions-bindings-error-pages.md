---
title: Richt lijnen voor het afhandelen van Azure Functions
description: Meer informatie over het afhandelen van fouten in Azure Functions met koppelingen naar specifieke bindings fouten.
author: craigshoemaker
ms.topic: conceptual
ms.date: 09/11/2019
ms.author: cshoe
ms.openlocfilehash: 0617d55f7c67c788b1e898d963f7d509cef72d49
ms.sourcegitcommit: 93329b2fcdb9b4091dbd632ee031801f74beb05b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2020
ms.locfileid: "92096841"
---
# <a name="azure-functions-error-handling"></a>Fout afhandeling Azure Functions

Het afhandelen van fouten in Azure Functions is belang rijk om te voor komen dat gegevens verloren gaan, gemiste gebeurtenissen en de status van uw toepassing te controleren.

In dit artikel worden algemene strategieën beschreven voor het afhandelen van fouten, samen met koppelingen naar binding-specifieke fouten.

## <a name="handling-errors"></a>Afhandeling van fouten

[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

## <a name="binding-error-codes"></a>Bindings fout codes

Wanneer u integreert met Azure-Services, kunnen fouten afkomstig zijn van de Api's van de onderliggende services. Informatie met betrekking tot binding-specifieke fouten is beschikbaar in de sectie **uitzonde ringen en retour codes** van de volgende artikelen:

+ [Azure Cosmos DB](functions-bindings-cosmosdb.md#exceptions-and-return-codes)

+ [Blob Storage](functions-bindings-storage-blob-output.md#exceptions-and-return-codes)

+ [Event Hubs](functions-bindings-event-hubs-output.md#exceptions-and-return-codes)

+ [IoT-hubs](functions-bindings-event-iot-output.md#exceptions-and-return-codes)

+ [Notification Hubs](functions-bindings-notification-hubs.md#exceptions-and-return-codes)

+ [Queue Storage](functions-bindings-storage-queue-output.md#exceptions-and-return-codes)

+ [Service Bus](functions-bindings-service-bus-output.md#exceptions-and-return-codes)

+ [Table Storage](functions-bindings-storage-table-output.md#exceptions-and-return-codes)
