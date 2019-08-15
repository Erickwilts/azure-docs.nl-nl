---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 08/12/2019
ms.author: jingwang
ms.openlocfilehash: 2e90d218aa6dc90746ba0e928fb3393f0bdb5e5a
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68966368"
---
<!--
    Separate the generic requirement on Self-hosted Integration Runtime set-up from connector articles.
-->
Als uw gegevens archief op een van de volgende manieren is geconfigureerd, moet u een [zelf-hostende Integration runtime](../articles/data-factory/create-self-hosted-integration-runtime.md) instellen om verbinding te maken met dit gegevens archief:

- Het gegevens archief bevindt zich in een on-premises netwerk, in azure Virtual Network, of in de virtuele particuliere cloud van Amazon.
- Het gegevens archief is een beheerde Cloud gegevens service waarbij de toegang wordt beperkt tot Ip's white list in de firewall regels.
