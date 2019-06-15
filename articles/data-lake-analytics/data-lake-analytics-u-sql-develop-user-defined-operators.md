---
title: U-SQL-gebruiker gedefinieerde operators ontwikkelen (UDO's) in Azure Data Lake Analytics
description: Informatie over het ontwikkelen van de gebruiker gedefinieerde operators om te worden gebruikt en hergebruikt in Azure Data Lake Analytics-taken.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: e5189e4e-9438-46d1-8686-ed4836bf3356
ms.topic: conceptual
ms.date: 12/05/2016
ms.openlocfilehash: 122a4b6af78a22f74d5057da75767077f8d9b978
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60813798"
---
# <a name="develop-u-sql-user-defined-operators-udos"></a>Ontwikkelen van U-SQL-gebruiker gedefinieerde operators (UDO's)
Dit artikel wordt beschreven hoe u voor het ontwikkelen van de gebruiker gedefinieerde operators voor het verwerken van gegevens in een U-SQL-taak.

## <a name="define-and-use-a-user-defined-operator-in-u-sql"></a>Definiëren en gebruiken van een gebruiker gedefinieerde operator in U-SQL
**Maken en verzenden van een U-SQL-taak**

1. In de Visual Studio-Selecteer **bestand > Nieuw > Project > U-SQL Project**.
2. Klik op **OK**. Visual Studio maakt een oplossing met een bestand Script.usql.
3. Van **Solution Explorer**, vouw Script.usql en dubbelklikt u vervolgens op **Script.usql.cs**.
4. Plak de volgende code in het bestand:

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;

        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Suisse", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };

                public override IRow Process(IRow input, IUpdatableRow output)
                {

                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");

                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);

                    return output.AsReadOnly();
                }
            }
        }
6. Open **Script.usql**, en plak het volgende U-SQL-script:

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);

        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    

        OUTPUT @drivers_CountryName
            TO "/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);
7. Geef het Data Lake Analytics-account, de database en het schema op.
8. Ga naar **Solution Explorer**, klik met de rechtermuisknop op **Script.usql** en klik vervolgens op **Build Script**.
9. Ga naar **Solution Explorer**, klik met de rechtermuisknop op **Script.usql** en klik vervolgens op **Submit Script**.
10. Als u dit nog niet hebt verbonden met uw Azure-abonnement, wordt u gevraagd de referenties van uw Azure-account in te voeren.
11. Klik op **Indienen**. Resultaat van het verzenden en een koppeling naar de taak zijn beschikbaar in het resultatenvenster wanneer het verzenden is voltooid.
12. Klik op de **vernieuwen** om weer te geven van de meest recente taakstatus en het scherm te vernieuwen.

**Om de uitvoer te bekijken**

1. Van **Server Explorer**, vouw **Azure**, vouw **Data Lake Analytics**, vouw uw Data Lake Analytics-account uit, vouw **Opslagaccounts**, met de rechtermuisknop op de standaard-opslag en klik vervolgens op **Explorer**.
2. Vouw van voorbeelden, uitvoer uit en dubbelklik vervolgens op **Stuurprogramma's.csv**.

## <a name="see-also"></a>Zie ook
* [U-SQL-expressies met gebruikerscode uitbreiden](/u-sql/concepts/extending-u-sql-expressions-with-user-code)
* [Data Lake Tools voor Visual Studio gebruiken voor het ontwikkelen van U-SQL-toepassingen](data-lake-analytics-data-lake-tools-get-started.md)
