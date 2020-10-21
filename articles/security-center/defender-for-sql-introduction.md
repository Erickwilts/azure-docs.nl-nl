---
title: Azure Defender voor SQL - de voordelen en functies
description: Meer informatie over de voordelen en functies van Azure Defender voor SQL.
author: memildin
ms.author: memildin
ms.date: 9/22/2020
ms.topic: overview
ms.service: security-center
ms.custom: references_regions
manager: rkarlin
ms.openlocfilehash: 7301f6bbb33c3eedff2e8c41edeaacb3df66d4c2
ms.sourcegitcommit: f88074c00f13bcb52eaa5416c61adc1259826ce7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/21/2020
ms.locfileid: "92342022"
---
# <a name="introduction-to-azure-defender-for-sql"></a>Inleiding tot Azure Defender voor SQL

Azure Defender voor SQL bevat twee Azure Defender-abonnementen die het [gegevensbeveiligingspakket van Azure Security Center uitbreiden](../azure-sql/database/azure-defender-for-sql.md) om uw databases en de bijbehorende gegevens te beveiligen, waar ze zich ook bevinden. 

## <a name="availability"></a>Beschikbaarheid

|Aspect|Details|
|----|:----|
|Releasestatus:|**Azure Defender voor Azure SQL-databaseservers** - algemeen beschikbaar<br>**Azure Defender voor SQL-servers op computers** - preview|
|Prijzen:|De twee abonnementen die **Azure Defender voor SQL** vormen, worden gefactureerd zoals wordt weergegeven op [de pagina met prijzen](security-center-pricing.md)|
|Beveiligde SQL-versies:|Azure SQL Database <br>Azure SQL Managed Instance<br>Azure Synapse Analytics (voorheen SQL DW)<br>SQL Server (alle ondersteunde versies)|
|Clouds:|![Ja](./media/icons/yes-icon.png) Commerciële clouds<br>![Ja](./media/icons/yes-icon.png) US Gov<br>![Nee](./media/icons/no-icon.png) China Gov, Other Gov|
|||

## <a name="what-does-azure-defender-for-sql-protect"></a>Wat beveiligt Azure Defender voor SQL?

**Azure Defender voor SQL** bestaat uit twee afzonderlijke Azure Defender-abonnementen:

- **Azure Defender voor Azure SQL-databaseservers** beveiligt:
  - [Azure SQL Database](../azure-sql/database/sql-database-paas-overview.md)
  - [Azure SQL Managed Instance](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md)
  - [Azure Synapse Analytics](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

- **Azure Defender voor SQL-servers op computers (preview)** breidt de beveiliging voor uw systeemeigen Azure SQL-servers uit om hybride omgevingen volledig te ondersteunen en SQL-servers (alle ondersteunde versies) die worden gehost in Azure, andere cloudomgevingen en zelfs on-premises machines te beveiligen


## <a name="what-are-the-benefits-of-azure-defender-for-sql"></a>Wat zijn de voordelen van Azure Defender voor SQL?

Deze twee abonnementen bevatten functionaliteit voor het identificeren en beperken van mogelijke beveiligingsproblemen van uw databases en het detecteren van afwijkende activiteiten die kunnen duiden op een bedreiging van uw databases:

- [Evaluatie van beveiligingsproblemen](../azure-sql/database/sql-vulnerability-assessment.md): de scanservice waarmee u potentiële zwakke plekken in de beveiliging van de database kunt detecteren, volgen en verhelpen. Evaluatiescans bieden een overzicht van de beveiligingsstatus van uw SQL-machines en details van eventuele beveiligingsresultaten.

- [Geavanceerde beveiliging tegen bedreigingen](../azure-sql/database/threat-detection-overview.md): de detectieservice die uw SQL-servers continu bewaakt als het gaat om bedreigingen, zoals SQL-injectie, brute-force aanvallen en misbruik van bevoegdheden. Deze service biedt actiegerichte beveiligingswaarschuwingen in Azure Security Center met details van de verdachte activiteit, richtlijnen voor het oplossen van problemen met de bedreigingen en opties voor het voortzetten van uw onderzoeken met Azure Sentinel.


## <a name="what-kind-of-alerts-does-azure-defender-for-sql-provide"></a>Wat voor soort waarschuwingen biedt Azure Defender voor SQL?

Beveiligingswaarschuwingen worden geactiveerd in het geval van:

- **Potentiële SQL-injectieaanvallen** - inclusief beveiligingsproblemen die worden gedetecteerd bij het genereren van een mislukte SQL-instructie in de database
- **Afwijkende databasetoegangs- en querypatronen**, bijvoorbeeld een abnormaal groot aantal mislukte aanmeldingspogingen met andere referenties (een poging tot een brute-force aanval)
- **Verdachte databaseactiviteit**, bijvoorbeeld een wijziging in de opslagbestemming voor het exporteren van een SQL-import- en-exportbewerking

Waarschuwingen bevatten details over het incident waardoor ze zijn geactiveerd evenals aanbevelingen voor het onderzoeken en oplossen van bedreigingen.



## <a name="next-steps"></a>Volgende stappen

In dit artikel bent u meer te weten gekomen over Azure Defender voor SQL.

Raadpleeg de volgende artikelen voor gerelateerd materiaal: 

- [Azure Defender voor SQL-servers inschakelen op computers](defender-for-sql-usage.md)
- [Azure Defender voor SQL inschakelen voor databaseservers](../azure-sql/database/azure-defender-for-sql.md)
- [De lijst met Azure Defender-waarschuwingen voor SQL](alerts-reference.md#alerts-sql-db-and-warehouse)