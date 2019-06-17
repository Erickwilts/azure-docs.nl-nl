---
title: bestand opnemen
description: bestand opnemen
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/29/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 060bc1039982cc0a77214d5dbe2a08de7a839c84
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66138199"
---
Als u wilt openen een directe SSH-sessie met de container, moet uw app worden uitgevoerd.

Plak de volgende URL in uw browser en vervang \<app-naam > door de naam van uw app:

```
https://<app-name>.scm.azurewebsites.net/webssh/host
```

Als u bent nog niet is geverifieerd, bent u verplicht om te verifiëren met uw Azure-abonnement om verbinding te maken. Eenmaal is geverifieerd, ziet u een shell in de browser, waar u opdrachten kunt uitvoeren in de container.

![SSH-verbinding](./media/app-service-web-ssh-connect-no-h/app-service-linux-ssh-connection.png)
