---
title: Informatie over reserveringen korting voor Azure SQL-Databases | Microsoft Docs
description: Meer informatie over hoe een reserveringskorting wordt toegepast op het uitvoeren van Azure SQL-Databases.
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/13/2019
ms.author: banders
ms.openlocfilehash: 4b4c6b390e9b3a0cf764f998523fe3c1cdc66026
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60370284"
---
# <a name="how-a-reservation-discount-is-applied-to-azure-sql-databases"></a>Hoe een reserveringskorting wordt toegepast op Azure SQL-Databases

Nadat u een Azure SQL Database gereserveerde capaciteit koopt, wordt de reserveringskorting wordt automatisch toegepast op SQL-Databases die overeenkomen met de kenmerken en de hoeveelheid van de reservering. Een reservering bevat informatie over de compute-kosten van uw SQL-Database. U betaalt voor de software, opslag en netwerken op basis van de normale tarieven. U kunt de licentiekosten voor SQL-Databases met dekt [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/).

Zie voor Reserved Virtual Machine Instances, [inzicht in Azure Reserved VM Instances korting](billing-understand-vm-reservation-charges.md).

## <a name="how-reservation-discount-is-applied"></a>Hoe reserveringskorting wordt toegepast

Een reserveringskorting is '*gebruik-it-of-verliezen-it*'. Dus als u geen overeenkomende resources voor elk uur, verliest klikt u een reserveringshoeveelheid voor dat uur. U niet uitvoeren voor het doorsturen van niet-gebruikte gereserveerde uur.

Als u een resource wordt afgesloten, past de reserveringskorting automatisch naar een andere overeenkomende resource in het opgegeven bereik. Als er geen overeenkomende bronnen u in het opgegeven bereik vindt, wordt de gereserveerde uren *verloren*.

## <a name="discount-applied-to-sql-databases"></a>Korting toegepast op de SQL-Databases

 De SQL-Database gereserveerde capaciteit korting wordt toegepast op het uitvoeren van SQL-Databases op uurbasis. De reservering die u koopt, wordt vergeleken met de compute-gebruik verzonden door de actieve SQL-Databases. Voor SQL Databases die geen vol uur worden uitgevoerd, wordt de reservering automatisch toegepast op andere SQL Databases die aan de reserveringskenmerken voldoen. De korting kunt toepassen op de SQL-Databases die gelijktijdig worden uitgevoerd. Als u geen SQL-Databases die voor een volledig uur worden uitgevoerd die overeenkomen met de kenmerken van de reservering hebt, krijgt u niet het volledige voordeel van de reserveringskorting voor dat uur.

De volgende voorbeelden ziet u hoe de SQL-Database gereserveerde capaciteit korting van toepassing is afhankelijk van het aantal kernen die u hebt gekocht, en wanneer ze worden uitgevoerd.

- Scenario 1: U koopt een capaciteit van de SQL-Database die zijn gereserveerd voor een 8-core SQL-Database. U uitvoeren een 16-core SQL-Database die overeenkomen met de rest van de kenmerken van de reservering. U betaalt het betalen per gebruik betalen prijs voor 8 kernen van compute-gebruik van SQL-Database. U krijgt de reserveringskorting voor één uur van 8 kernen compute-gebruik van SQL-Database.

Voor de rest van deze voorbeelden wordt ervan uitgegaan dat de SQL-Database gereserveerde capaciteit die u kopen voor een 16-core SQL-Database en de rest van de kenmerken van de reservering overeenkomen met de actieve SQL-Databases.

- Scenario 2: U hebt twee SQL-Databases met 8 kernen uitvoeren voor een uur. De reserveringskorting 16 kernen wordt toegepast om de compute-gebruik voor zowel de 8 cores SQL-Databases.
- Scenario 3: Uitvoeren van een 16-core, SQL-Database van 1 uur aan 13:30 uur. U hebt uitgevoerd van een andere 16 kernen SQL-Database van 1:30 tot 2 uur. Beide zijn inbegrepen bij de reserveringskorting.
- Scenario 4: Uitvoeren van een 16-core, SQL-Database van 1 uur aan 13:45 uur. U hebt uitgevoerd van een andere 16 kernen SQL-Database van 1:30 tot 2 uur. U betaalt het betalen per gebruik betalen prijs voor de overlapping van 15 minuten. De reserveringskorting is van toepassing op de compute-gebruik voor de rest van de tijd.

Om te begrijpen en te bekijken van de toepassing van uw Azure-reserveringen in gebruiksrapporten facturering, Zie [over Azure-reservering gebruik](billing-understand-reserved-instance-usage-ea.md).

## <a name="need-help-contact-us"></a>Hulp nodig? Contact opnemen

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure-reserveringen, de volgende artikelen:

- [Wat zijn Azure-reserveringen?](billing-save-compute-costs-reservations.md)
- [Vooruitbetalen voor Virtual Machines met Azure Reserved VM Instances](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Vooruitbetalen voor compute-resources van SQL Database met gereserveerde capaciteit voor Azure SQL Database](../sql-database/sql-database-reserved-capacity.md)
- [Azure-reserveringen beheren](billing-manage-reserved-vm-instance.md)
- [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
- [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](billing-understand-reserved-instance-usage-ea.md)
- [Informatie over het gebruik van de reservering voor CSP-abonnementen](/partner-center/azure-reservations)
