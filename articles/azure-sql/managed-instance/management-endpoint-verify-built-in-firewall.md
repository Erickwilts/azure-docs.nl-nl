---
title: Poort beveiliging in de ingebouwde firewall verifiëren
description: Meer informatie over het controleren van de ingebouwde firewall beveiliging in Azure SQL Managed instance.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, carlrab
ms.date: 12/04/2018
ms.openlocfilehash: 1d2fffabba3615394bdf96ed487177bf21f3ecec
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2020
ms.locfileid: "84708702"
---
# <a name="verify-the-azure-sql-managed-instance-built-in-firewall"></a>De ingebouwde firewall van het door Azure SQL beheerde exemplaar controleren
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Voor de [verplichte binnenkomende beveiligings regels](connectivity-architecture-overview.md#mandatory-inbound-security-rules) van Azure SQL Managed instance zijn beheer poorten 9000, 9003, 1438, 1440 en 1452 vereist om te kunnen worden geopend vanuit **elke bron** in de netwerk beveiligings groep (NSG) die een SQL-beheerd exemplaar beveiligt. Hoewel deze poorten zijn geopend op het niveau van de NSG, worden ze op netwerk niveau beschermd door de ingebouwde firewall.

## <a name="verify-firewall"></a>Firewall controleren

Als u deze poorten wilt controleren, gebruikt u een hulp programma voor beveiligings scanner om deze poorten te testen. In de volgende scherm afbeelding ziet u hoe u een van deze hulpprogram ma's gebruikt.

![De ingebouwde firewall controleren](./media/management-endpoint-verify-built-in-firewall/03_verify_firewall.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over SQL Managed instance en Connectivity de [Azure SQL Managed instance connectivity-architectuur](connectivity-architecture-overview.md).
