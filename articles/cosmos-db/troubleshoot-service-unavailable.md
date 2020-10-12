---
title: Problemen oplossen met uitzonde ringen Azure Cosmos DB service niet beschikbaar
description: Meer informatie over het opsporen en oplossen van uitzonde ringen voor Azure Cosmos DB service niet beschikbaar.
author: j82w
ms.service: cosmos-db
ms.date: 08/06/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 749592b778612c6903c9c15e336de3fb00978199
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "88870864"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-service-unavailable-exceptions"></a>Problemen vaststellen en oplossen Azure Cosmos DB service niet-beschik bare uitzonde ringen
De SDK kan geen verbinding maken met Azure Cosmos DB.

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing
De volgende lijst bevat bekende oorzaken en oplossingen voor niet-beschik bare uitzonde ringen voor services.

### <a name="the-required-ports-are-being-blocked"></a>De vereiste poorten worden geblokkeerd
Controleer of alle [vereiste poorten](performance-tips-dotnet-sdk-v3-sql.md#networking) zijn ingeschakeld.

### <a name="client-side-transient-connectivity-issues"></a>Problemen met de tijdelijke verbinding aan de client zijde
Onbeschikbaare uitzonde ringen voor services kunnen zich op het Opper vlak bevinden wanneer er tijdelijke verbindings problemen zijn die time-outs veroorzaken. Normaal gesp roken bevat de stack tracering die betrekking heeft op dit scenario, een `TransportException` fout. Bijvoorbeeld:

```C#
TransportException: A client transport error occurred: The request timed out while waiting for a server response. 
(Time: xxx, activity ID: xxx, error code: ReceiveTimeout [0x0010], base error: HRESULT 0x80131500
```

Volg de stappen voor het oplossen van de [aanvraag time-out](troubleshoot-dot-net-sdk-request-timeout.md#troubleshooting-steps) om deze op te lossen.

### <a name="service-outage"></a>Service-onderbreking
Controleer de [status van Azure](https://status.azure.com/status) om te zien of er een doorlopend probleem is.


## <a name="next-steps"></a>Volgende stappen
* Problemen [vaststellen en oplossen](troubleshoot-dot-net-sdk.md) wanneer u de Azure Cosmos db .NET SDK gebruikt.
* Meer informatie over prestatie richtlijnen voor [.net v3](performance-tips-dotnet-sdk-v3-sql.md) en [.NET v2](performance-tips.md).