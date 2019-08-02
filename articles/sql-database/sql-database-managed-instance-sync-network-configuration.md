---
title: Azure App Service-netwerk configuratie synchroniseren | Microsoft Docs
description: In dit artikel wordt beschreven hoe u uw netwerk configuratie synchroniseert voor Azure App Service hosting abonnement.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 12/13/2018
ms.openlocfilehash: 920851e33255b0bcb5b085c541ae636c5988be72
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68567310"
---
# <a name="sync-networking-configuration-for-azure-app-service-hosting-plan"></a>De netwerk configuratie voor het hosting abonnement voor Azure App Service synchroniseren

Het kan gebeuren dat u [uw app hebt geïntegreerd met een Azure-Virtual Network](../app-service/web-sites-integrate-with-vnet.md), maar u kunt geen verbinding maken met het beheerde exemplaar. U kunt proberen om de netwerk configuratie voor uw service abonnement te vernieuwen.

## <a name="sync-network-configuration-for-app-service-hosting-plan"></a>Netwerk configuratie voor App Service hosting abonnement synchroniseren

Voer hiervoor de volgende stappen uit:  

1. Ga naar uw web apps App Service plan.

   ![App Service-plan](./media/sql-database-managed-instance-sync-networking/app-service-plan.png)

2. Klik op **netwerken** en klik vervolgens op **Klik hier om te beheren**.

   ![service abonnement beheren](./media/sql-database-managed-instance-sync-networking/manage-plan.png)

3. Selecteer uw **VNet** en klik op **netwerk synchroniseren**.

   ![netwerk synchroniseren](./media/sql-database-managed-instance-sync-networking/sync.png)

4. Wacht totdat de synchronisatie is voltooid.
  
   ![synchronisatie is voltooid](./media/sql-database-managed-instance-sync-networking/sync-done.png)

U kunt nu proberen om de verbinding met uw beheerde exemplaar te herstellen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het configureren van uw VNet voor beheerde exemplaren [beheerde exemplaar VNet-architectuur](sql-database-managed-instance-connectivity-architecture.md) en [het configureren van bestaande vnet](sql-database-managed-instance-configure-vnet-subnet.md).
