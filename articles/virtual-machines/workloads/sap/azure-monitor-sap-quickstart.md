---
title: Azure Monitor implementeren voor SAP-oplossingen met de Azure Portal
description: Azure Monitor implementeren voor SAP-oplossingen met de Azure Portal
author: sameeksha91
ms.author: sakhare
ms.topic: how-to
ms.service: virtual-machines
ms.date: 08/17/2020
ms.reviewer: cynthn
ms.openlocfilehash: 6deb7b535c3876ae8a8e83174b97a75582e82e58
ms.sourcegitcommit: 83610f637914f09d2a87b98ae7a6ae92122a02f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2020
ms.locfileid: "91996438"
---
# <a name="deploy-azure-monitor-for-sap-solutions-with-azure-portal"></a>Azure Monitor implementeren voor SAP-oplossingen met Azure Portal

Azure Monitor voor SAP-oplossings resources kunnen worden gemaakt via de [Azure Portal](https://azure.microsoft.com/features/azure-portal). Deze methode biedt een gebruikers interface op basis van een browser voor het implementeren van Azure Monitor voor SAP-oplossingen en voor het configureren van providers.

## <a name="sign-in-to-azure-portal"></a>Meld u aan bij Azure Portal

Meld u aan bij Azure Portal op https://portal.azure.com

## <a name="create-monitoring-resource"></a>Bewakings bron maken

1. Selecteer **Azure monitor voor SAP-oplossingen** van **Azure Marketplace**.

   :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-1.png" alt-text="Afbeelding toont het selecteren van de Azure Monitor voor SAP-oplossingen aanbod vanuit Azure Marketplace." lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-1.png":::

2. Geef op het tabblad **basis beginselen** de vereiste waarden op. Als dit van toepassing is, kunt u een bestaande Log Analytics-werk ruimte gebruiken.

   :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-2.png" alt-text="Afbeelding toont het selecteren van de Azure Monitor voor SAP-oplossingen aanbod vanuit Azure Marketplace." lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-2.png":::

3. Wanneer u een virtueel netwerk selecteert, moet u ervoor zorgen dat de systemen die u wilt bewaken bereikbaar zijn vanuit dat VNET. 

   > [!IMPORTANT]
   > Als u **delen** selecteert voor het delen van gegevens met micro soft, kunnen onze ondersteunings teams extra ondersteuning bieden.

## <a name="configure-providers"></a>Providers configureren

### <a name="sap-hana-provider"></a>SAP HANA provider 

1. Selecteer het tabblad **provider** om de providers toe te voegen die u wilt configureren. U kunt na het implementeren van de bewakings resource meerdere providers toevoegen, of toevoegen. 

   :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-3.png" alt-text="Afbeelding toont het selecteren van de Azure Monitor voor SAP-oplossingen aanbod vanuit Azure Marketplace." lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-3.png":::

2. Selecteer **provider toevoegen** en kies **SAP Hana** in de vervolg keuzelijst. 

3. Voer het privé-IP-adres voor de HANA-server in.

4. Voer de naam in van de data base-Tenant die u wilt gebruiken. U kunt een wille keurige Tenant kiezen. Wij raden u echter aan **SYSTEMDB** te gebruiken, omdat hiermee een bredere matrix van bewakings gebieden mogelijk is. 

5. Voer het SQL-poort nummer in dat is gekoppeld aan uw HANA-data base. Het poort nummer moet de indeling **[3]**  +  **[exemplaar #]**  +  **[13]** of **[3]**  +  **[exemplaar #]**  +  **[15]** hebben. Bijvoorbeeld 30013 of 30015. 

6. Geef de data base-gebruikers naam op die u wilt gebruiken. Zorg ervoor dat aan de database gebruiker de rollen **bewaking** en **catalogus lezen** zijn toegewezen. 

7. Wanneer u klaar bent, selecteert u **provider toevoegen**. Ga verder met het toevoegen van extra providers of selecteer **controleren + maken** om de implementatie te volt ooien.

   :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-4.png" alt-text="Afbeelding toont het selecteren van de Azure Monitor voor SAP-oplossingen aanbod vanuit Azure Marketplace." lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-4.png":::

### <a name="high-availability-cluster-pacemaker-provider"></a>Pacemaker-provider (high-availability cluster)

1. Selecteer een **cluster met hoge Beschik baarheid (pacemaker)** in de vervolg keuzelijst. 

   > [!IMPORTANT]
   > Als u de pacemaker-provider (high-availability cluster) wilt configureren, moet u ervoor zorgen dat ha_cluster_provider in elk knoop punt is geïnstalleerd. Zie [HA cluster exporteur](https://github.com/ClusterLabs/ha_cluster_exporter#installation) (Engelstalig) voor meer informatie

2. Voer het Prometheus-eind punt in de vorm van in http://IP:9664/metrics . 
 
3. Voer de systeem-ID (SID), de hostnaam en de cluster naam in.

4. Wanneer u klaar bent, selecteert u **provider toevoegen**. Ga verder met het toevoegen van extra providers of selecteer **controleren + maken** om de implementatie te volt ooien.

   :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-5.png" alt-text="Afbeelding toont het selecteren van de Azure Monitor voor SAP-oplossingen aanbod vanuit Azure Marketplace." lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-5.png":::


### <a name="microsoft-sql-server-provider"></a>Microsoft SQL Server provider

1. Voordat u de Microsoft SQL Server provider toevoegt, moet u het volgende script uitvoeren in SQL Server Management Studio om een gebruiker te maken met de juiste machtigingen die nodig zijn voor het configureren van de provider.

   ```sql
   USE [<Database to monitor>]
   DROP USER [AMS]
   GO
   USE [master]
   DROP USER [AMS]
   DROP LOGIN [AMS]
   GO
   CREATE LOGIN [AMS] WITH PASSWORD=N'<password>', DEFAULT_DATABASE=[<Database to monitor>], DEFAULT_LANGUAGE=[us_english], CHECK_EXPIRATION=OFF, CHECK_POLICY=OFF
   CREATE USER AMS FOR LOGIN AMS
   ALTER ROLE [db_datareader] ADD MEMBER [AMS]
   ALTER ROLE [db_denydatawriter] ADD MEMBER [AMS]
   GRANT CONNECT TO AMS
   GRANT VIEW SERVER STATE TO AMS
   GRANT VIEW SERVER STATE TO AMS
   GRANT VIEW ANY DEFINITION TO AMS
   GRANT EXEC ON xp_readerrorlog TO AMS
   GO
   USE [<Database to monitor>]
   CREATE USER [AMS] FOR LOGIN [AMS]
   ALTER ROLE [db_datareader] ADD MEMBER [AMS]
   ALTER ROLE [db_denydatawriter] ADD MEMBER [AMS]
   GO
   ``` 

2. Selecteer **provider toevoegen** en kies **Microsoft SQL Server** in de vervolg keuzelijst. 

3. Vul de velden in met behulp van de informatie die is gekoppeld aan uw Microsoft SQL Server. 

4. Wanneer u klaar bent, selecteert u **provider toevoegen**. Ga verder met het toevoegen van extra providers of selecteer **controleren + maken** om de implementatie te volt ooien.

     :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-6.png" alt-text="Afbeelding toont het selecteren van de Azure Monitor voor SAP-oplossingen aanbod vanuit Azure Marketplace." lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-6.png":::

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [Azure monitor voor SAP-oplossingen](azure-monitor-overview.md)
