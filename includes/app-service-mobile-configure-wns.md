---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 79459be30a5a2018dc82486a84895b1a954941bc
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66140444"
---
1. In de [Azure-portal](https://portal.azure.com/), selecteer **door alles bladeren** > **App Services**. Vervolgens Selecteer uw Mobile Apps-back-end. Onder **instellingen**, selecteer **App Service-Push**. Selecteer vervolgens de naam van uw notification hub.
2. Ga naar **Windows (WNS)**. Voer vervolgens de **beveiligingssleutel** (clientgeheim) en **pakket-SID** die u hebt verkregen via de site van Live-Services. Selecteer vervolgens **opslaan**.

    ![Stel de WNS-sleutel in de portal](./media/app-service-mobile-configure-wns/mobile-push-wns-credentials.png)

Uw back-end is nu geconfigureerd voor het gebruik van WNS om pushmeldingen te verzenden.
