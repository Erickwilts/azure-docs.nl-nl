---
title: Overzicht van de relationele databaseservice Azure Database for PostgreSQL
description: Dit biedt een overzicht van de relationele databaseservice Azure Database for PostgreSQL.
author: jonels-msft
ms.author: jonels
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 11/25/2019
ms.openlocfilehash: 9ea0610811f6906526afe55d577e04a8decd5f49
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/25/2019
ms.locfileid: "74481655"
---
# <a name="what-is-azure-database-for-postgresql"></a>Wat is Azure Database for PostgreSQL?
Azure Database for PostgreSQL is a relational database service in the Microsoft cloud built for developers. It is based on the community version of open-source [PostgreSQL](https://www.postgresql.org/) database engine, and is available in two deployment options: Single Server and Hyperscale (Citus).

## <a name="azure-database-for-postgresql---single-server"></a>Azure Database for PostgreSQL - Single Server
The Single Server deployment option delivers:

- Built-in [high availability](concepts-high-availability.md) with no additional cost (99.99% SLA)
- Voorspelbare prestaties, tegen all-inclusive prijzen op basis van betalen per gebruik
- [Vertical scale as needed](concepts-pricing-tiers.md) within seconds
- [Monitoring and alerting](concepts-monitoring.md) to assess your server
- Beveiliging en naleving van zakelijk niveau
- [Secured to protect](concepts-security.md) sensitive data at-rest and in-motion
- [Automatic backups and point-in-time-restore](concepts-business-continuity.md) for up to 35 days


Alle deze mogelijkheden vereisen nauwelijks beheer, en alles wordt zonder extra kosten geleverd. They allow you to focus on rapid application development and accelerating your time to market, rather than spending precious time and resources to manage virtual machines and infrastructure. You can continue to develop your application with the open-source tools and platform of your choice, without having to learn new skills.

The Single Server deployment option offers three pricing tiers: Basic, General Purpose, and Memory Optimized. Elke categorie biedt verschillende resources ter ondersteuning van de workloads van uw databases. U kunt uw eerste app op een kleine database bouwen voor een paar euro met maand en vervolgens de schaal ervan aanpassen om aan de vereisten van uw oplossing te voldoen. Doordat de schaalbaarheid dynamisch is, kan uw database op een transparante manier reageren op snel veranderende resourcevereisten. U betaalt alleen voor de resources die u nodig hebt op het moment dat u ze nodig hebt. Zie  [Prijscategorieën](concepts-pricing-tiers.md) voor meer informatie.

## <a name="azure-database-for-postgresql---hyperscale-citus"></a>Azure Database for PostgreSQL - Hyperscale (Citus)
The Hyperscale (Citus) option horizontally scales queries across multiple machines using sharding. Its query engine parallelizes incoming SQL queries across these servers for faster responses on large datasets. It serves applications that require greater scale and performance, generally workloads that are approaching -- or already exceed -- 100 GB of data.

The Hyperscale (Citus) deployment option delivers:

- Horizontal scaling across multiple machines using sharding
- Query parallelization across these servers for faster responses on large datasets
- Excellent support for multi-tenant applications, real time operational analytics, and high throughput transactional workloads

Applications built for PostgreSQL can run distributed queries on Hyperscale (Citus) with standard [connection libraries](./concepts-connection-libraries.md) and minimal changes.

## <a name="contacts"></a>Contactpersonen
For any questions or suggestions about working with Azure Database for PostgreSQL, send an email to the Azure Database for PostgreSQL Team ([@Ask Azure DB for PostgreSQL](mailto:AskAzureDBforPostgreSQL@service.microsoft.com)). This address is for general questions rather than support tickets.

In addition, consider these points of contact as appropriate:
- To contact Azure Support or fix an issue with your account, [file a ticket from the Azure portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Als u feedback wilt geven of een nieuwe functie wilt aanvragen, maakt u een vermelding via [UserVoice](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).

## <a name="next-steps"></a>Volgende stappen
- Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/postgresql/) voor kostenvergelijkingen en calculators.
- Get started by creating your first Azure Database for PostgreSQL [Single Server](./quickstart-create-server-database-portal.md) or [Hyperscale (Citus)](./quickstart-create-hyperscale-portal.md)
- Bouw uw eerste app in Python, PHP, Ruby, C\#, Java, Node.js: [verbindingsbibliotheken](./concepts-connection-libraries.md)
