---
title: Azure App Service - netwerkconfiguratie synchroniseren | Microsoft Docs
description: In dit artikel wordt beschreven hoe u uw netwerkconfiguratie voor Azure App Service-hostingabonnement synchroniseren.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
manager: craigg
ms.date: 12/13/2018
ms.openlocfilehash: 0d7920080fd61389741fbe785f5141003bef5251
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61314690"
---
# <a name="sync-networking-configuration-for-azure-app-service-hosting-plan"></a>Configuratie van netwerken voor Azure App Service-hostingabonnement synchroniseren

Het gebeuren dat hoewel u [geïntegreerd van uw app met een Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md), u kunt geen verbinding maken met Managed Instance. Wat die kunt u proberen is om te vernieuwen voor de configuratie van netwerken voor uw service-plan.

## <a name="sync-network-configuration-for-app-service-hosting-plan"></a>Netwerkconfiguratie synchroniseren voor App Service-hostingplan

Voer hiervoor de volgende stappen uit:  

1. Ga naar uw web-apps met App Service-plan.

   ![App service-plan](./media/sql-database-managed-instance-sync-networking/app-service-plan.png)

2. Klik op **netwerken** en klik vervolgens op **Klik hier om te beheren**.

   ![beheren van service-plan](./media/sql-database-managed-instance-sync-networking/manage-plan.png)

3. Selecteer uw **VNet** en klikt u op **netwerk synchroniseren**.

   ![netwerk synchroniseren](./media/sql-database-managed-instance-sync-networking/sync.png)

4. Wacht totdat de synchronisatie is voltooid.
  
   ![synchronisatie is uitgevoerd](./media/sql-database-managed-instance-sync-networking/sync-done.png)

U bent nu klaar om te proberen voor uw verbinding met uw beheerde exemplaar opnieuw tot stand te brengen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het configureren van uw VNet voor beheerd exemplaar [beheerd exemplaar VNet architectuur](sql-database-managed-instance-connectivity-architecture.md) en [het configureren van bestaande VNet](sql-database-managed-instance-configure-vnet-subnet.md).
