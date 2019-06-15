---
title: Gekoppelde services in Azure Data Factory parameteriseren | Microsoft Docs
description: Leer hoe u voorzien van gekoppelde services in Azure Data Factory en dynamische waarden doorgeven tijdens runtime.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/18/2018
author: gauravmalhot
ms.author: gamal
manager: craigg
ms.openlocfilehash: 0239c53f98fba201b6d70e1e2212eea36134e30d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60635525"
---
# <a name="parameterize-linked-services-in-azure-data-factory"></a>Gekoppelde services in Azure Data Factory parameteriseren

U kunt nu voorzien van een gekoppelde service en dynamische waarden doorgeven tijdens runtime. Bijvoorbeeld, als u wilt verbinding maken met verschillende databases op dezelfde Azure SQL Database-server, kunt u nu parameter van de naam van de database in het definitie van de gekoppelde service. Dit voorkomt u dat niet hoeft te maken van een gekoppelde service voor elke database op de Azure SQL database-server. U kunt bijvoorbeeld andere eigenschappen in de definitie van de gekoppelde service ook - parameteriseren *gebruikersnaam.*

U kunt de gebruikersinterface van Data Factory in Azure Portal of een programmeerinterface gebruiken om te voorzien van gekoppelde services.

> [!TIP]
> We raden niet om te voorzien van wachtwoorden of geheimen. Alle verbindingsreeksen in plaats daarvan Store in Azure Key Vault en parameteriseren de *geheime naam*.

Bekijk de volgende video voor een inleiding van 7 minuten en demonstratie van deze functie:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Parameterize-connections-to-your-data-stores-in-Azure-Data-Factory/player]

## <a name="supported-data-stores"></a>Ondersteunde gegevensarchieven

Parameterisering van de gekoppelde service wordt op dit moment ondersteund in de gebruikersinterface van Data Factory in Azure portal voor de volgende gegevensarchieven. Voor alle andere gegevensarchieven, kunt u de gekoppelde service voorzien door het selecteren van de **Code** pictogram op de **verbindingen** tabblad en met behulp van de JSON-editor.
- Azure SQL Database
- Azure SQL Data Warehouse
- SQL Server
- Oracle
- Cosmos DB
- Amazon Redshift
- MySQL
- Azure Database for MySQL

## <a name="data-factory-ui"></a>Gebruikersinterface van Data Factory

![Dynamische inhoud toevoegen aan de definitie van de gekoppelde Service](media/parameterize-linked-services/parameterize-linked-services-image1.png)

![Een nieuwe parameter maken](media/parameterize-linked-services/parameterize-linked-services-image2.png)

## <a name="json"></a>JSON

```json
{
    "name": "AzureSqlDatabase",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "value": "Server=tcp:myserver.database.windows.net,1433;Database=@{linkedService().DBName};User ID=user;Password=fake;Trusted_Connection=False;Encrypt=True;Connection Timeout=30",
                "type": "SecureString"
            }
        },
        "connectVia": null,
        "parameters": {
            "DBName": {
                "type": "String"
            }
        }
    }
}
```
