---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/09/2018
ms.author: wesmc
ms.openlocfilehash: 8ebf5ddfa118e0aeadeab0c00a981871a4b5708e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66132894"
---
.NET-toepassingen kunnen de cache-client **StackExchange.Redis** gebruiken. Deze kan in Visual Studio worden geconfigureerd met een NuGet-pakket dat de configuratie van de cacheclienttoepassingen eenvoudiger maakt. 

> [!NOTE]
> Zie voor meer informatie de [StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis) GitHub-pagina en de [StackExchange.Azure Cache voor documentatie voor Redis-client](http://github.com/StackExchange/StackExchange.Redis#documentation).
>
>

Als u in Visual Studio een clienttoepassing wilt configureren met het NuGet-pakket StackExchange.Redis, klikt u in **Solution Explorer** met de rechtermuisknop op het project en kiest u **Manage NuGet Packages**. 

![Manage NuGet Packages](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-manage-nuget-menu.png)

Typ in het zoekvak **StackExchange.Redis** of **StackExchange.Redis.StrongName**, selecteer de gewenste versie in de resultaten en klik op **Install**.

> [!NOTE]
> Als u liever een versie van de clientbibliotheek **StackExchange.Redis** gebruikt met een sterke naam, kiest u **StackExchange.Redis.StrongName**. Kies anders **StackExchange.Redis**.
>
>

![NuGet-pakket StackExchange.Redis](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-stackexchange-redis.png)

Het NuGet-pakket gedownload en de vereiste assembly-verwijzingen voor de clienttoepassing voor toegang tot Azure-Cache voor Redis met de StackExchange.Azure Cache voor Redis-client wordt toegevoegd.

> [!NOTE]
> Als u uw project eerder hebt geconfigureerd voor gebruik van StackExchange.Redis, kunt u bij **NuGet Package Manager** controleren op updates voor het pakket. Als u wilt zoeken en installeren van de bijgewerkte versies van het NuGet-pakket StackExchange.Redis, klikt u op **Updates** in de **NuGet Package Manager** venster. Als er een update voor het StackExchange.Redis-pakket NuGet beschikbaar is, kunt u uw project bijwerken zodat de bijgewerkte versie wordt gebruikt.
>
>

U kunt het StackExchange.Redis-pakket NuGet ook installeren door in het menu **Hulpprogramma's** te klikken op **NuGet Package Manager**, **Package Manager Console** en in het venster **Package Manager Console** de volgende opdracht uit te voeren.

```
Install-Package StackExchange.Redis
```
