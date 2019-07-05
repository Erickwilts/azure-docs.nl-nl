---
title: Azure SQL Database Machine Learning-Services met R (preview)-overzicht
description: In dit artikel wordt beschreven van Azure SQL Database Machine Learning-Services (met R) en wordt uitgelegd hoe het werkt.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: carlrab
manager: cgronlun
ms.date: 03/01/2019
ms.openlocfilehash: 186b986fe1931365ee34efab2e04e58908402cc0
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67427946"
---
# <a name="azure-sql-database-machine-learning-services-with-r-preview"></a>Azure SQL Database Machine Learning-Services met R (preview)

Machine Learning-Services is een functie van Azure SQL Database, die wordt gebruikt voor het uitvoeren van R-scripts in de database. De functie bevat Microsoft R-pakketten voor krachtige voorspellende analyse en machine learning. De relationele gegevens kunnen worden gebruikt in de R-scripts via opgeslagen procedures, overzichten van de T-SQL-script met-R of R-code met T-SQL.

> [!IMPORTANT]
> Azure SQL Database Machine Learning-Services (met R) is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.
>
> De openbare preview-versie is beschikbaar voor individuele databases en elastische pools met behulp van de vCore gebaseerde aankoopmodel in de **algemeen** en **bedrijfskritiek** Servicelagen. In deze eerste preview-versie, de **grootschalige** servicelaag en het **beheerd exemplaar** Implementatieoptie worden niet ondersteund. Momenteel is R de enige ondersteunde taal. Er is op dit moment geen ondersteuning voor Python.
>
> De Preview-versie is momenteel beschikbaar in de volgende regio's: West-Europa, Noord-Europa, VS-West 2, VS-Oost, VS Zuid-centraal, Noord-centraal VS, Canada centraal, Zuidoost-Azië, India-Zuid en Australië-Zuidoost.
>
> [Aanmelden voor de Preview-versie](#signup) hieronder.

## <a name="what-you-can-do-with-r"></a>U kunt doen met R

Gebruik de kracht van de taal R om geavanceerde analyses en in-database machine learning te bieden. Deze mogelijkheid zorgt voor berekeningen en verwerking op de plaats waar de gegevens zich bevinden, u hoeft de gegevens niet op te halen via het netwerk. U kunt ook gebruikmaken van de kracht van R-pakketten voor enterprise leveren van geavanceerde analyses op schaal.

Machine Learning-services bevatten een basisdistributie van R, overlapt met zakelijke R-pakketten van Microsoft. R-functies en -algoritmen van Microsoft zijn ontworpen voor zowel schaal als bruikbaarheid en bieden voorspellende analyses, statistische modellen, gegevensvisualisaties en toonaangevende algoritmen voor machine learning.

### <a name="r-packages"></a>R-pakketten

Meest voorkomende open-source R-pakketten zijn vooraf geïnstalleerd in Machine Learning-Services. De volgende R-pakketten van Microsoft zijn ook opgenomen:

| R-pakket | Description|
|-|-|
| [Microsoft R Open](https://mran.microsoft.com/rro) | Microsoft R Open is de verbeterde distributie van R van Microsoft. Het is een volledige open source-platform voor statistische analyse en datatechnologie. Het is gebaseerd op en 100% compatibel met R en meer mogelijkheden voor verbeterde prestaties en reproduceerbaarheid bevat. |
| [RevoScaleR](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler) | RevoScaleR is de primaire tapewisselaar voor schaalbare r-functies in deze bibliotheek een van de meest gebruikte zijn. Gegevenstransformaties en manipuleren, statistische overzichten, visualisatie en vele vormen van modellen en analyses zijn gevonden in deze bibliotheken. Functies in deze bibliotheken worden ook automatisch de workloads verdelen over beschikbare cores voor parallelle verwerking, met de mogelijkheid om te werken op segmenten van de gegevens die worden gecoördineerd en worden beheerd door de berekenings-engine. |
| [MicrosoftML (R)](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-microsoftml) | MicrosoftML toegevoegd machine learning-algoritmen voor het maken van aangepaste modellen voor tekstanalyse, analyse van de afbeelding en sentimentanalyse. |

Naast de vooraf geïnstalleerde pakketten, kunt u [extra pakketten installeren](sql-database-machine-learning-services-add-r-packages.md).

<a name="signup"></a>

## <a name="sign-up-for-the-preview"></a>Registreren voor de preview-versie

Als u zich voor de openbare preview-versie, de volgende stappen uit:

1. Als u geen Azure-abonnement hebt, [maakt u een account](https://azure.microsoft.com/free/) voordat u begint.

2. Stuur een e-mailbericht naar Microsoft op [sqldbml@microsoft.com](mailto:sqldbml@microsoft.com) om u te registreren voor de openbare preview. De openbare preview van Machine Learning Services (met R) in SQL Database is niet standaard ingeschakeld.

Nadat u bent geregistreerd voor het programma, Microsoft zullen we u aan de openbare preview en R inschakelen voor uw bestaande of nieuwe database.

Machine Learning-Services met R wordt niet aanbevolen voor productie-werkbelasting tijdens de openbare preview.

## <a name="next-steps"></a>Volgende stappen

- Zie de [belangrijke verschillen van SQL Server Machine Learning Services](sql-database-machine-learning-services-differences.md).
- Zie voor meer informatie over het gebruik van R op Machine Learning Services (preview) voor Azure SQL Database-query, de [snelstartgids](sql-database-connect-query-r.md).
- Als u wilt beginnen met enkele eenvoudige R-scripts, Zie [maken en uitvoeren eenvoudige R-scripts in Azure SQL Database Machine Learning-Services (preview)](sql-database-quickstart-r-create-script.md).
