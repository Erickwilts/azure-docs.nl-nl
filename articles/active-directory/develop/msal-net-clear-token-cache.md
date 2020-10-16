---
title: De token cache wissen (MSAL.NET) | Azure
titleSuffix: Microsoft identity platform
description: Meer informatie over het wissen van de token cache met behulp van de micro soft Authentication Library voor .NET (MSAL.NET).
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 05/07/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: devx-track-csharp, aaddev
ms.openlocfilehash: 9a86a535bf429dcc81810c6c39ba415a158b20ec
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "88166209"
---
# <a name="clear-the-token-cache-using-msalnet"></a>De token cache wissen met behulp van MSAL.NET

Wanneer u [een toegangs token aanschaft](msal-acquire-cache-tokens.md) met behulp van micro soft Authentication Library voor .net (MSAL.net), wordt het token in de cache opgeslagen. Wanneer de toepassing een token vereist, moet het eerst de methode aanroepen `AcquireTokenSilent` om te controleren of een acceptabel token zich in de cache bevindt. 

Het wissen van de cache wordt bereikt door de accounts uit de cache te verwijderen. Hiermee wordt de sessie cookie die zich in de browser bevindt, echter niet verwijderd.  In het volgende voor beeld wordt een open bare client toepassing geïnstantieerd, worden de accounts voor de toepassing opgehaald en worden de accounts verwijderd.

```csharp
private readonly IPublicClientApplication _app;
private static readonly string ClientId = ConfigurationManager.AppSettings["ida:ClientId"];
private static readonly string Authority = string.Format(CultureInfo.InvariantCulture, AadInstance, Tenant);

_app = PublicClientApplicationBuilder.Create(ClientId)
                .WithAuthority(Authority)
                .Build();

var accounts = (await _app.GetAccountsAsync()).ToList();

// clear the cache
while (accounts.Any())
{
   await _app.RemoveAsync(accounts.First());
   accounts = (await _app.GetAccountsAsync()).ToList();
}

```

Lees [een toegangs Token ophalen](msal-acquire-cache-tokens.md)voor meer informatie over het verkrijgen en caching van tokens.
