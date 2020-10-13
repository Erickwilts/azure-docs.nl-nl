---
title: Machine Learning Services in Azure SQL Managed instance (preview-versie)
description: Dit artikel bevat een overzicht of Machine Learning Services in Azure SQL Managed instance.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.reviewer: sstein, davidph
manager: cgronlun
ms.date: 06/03/2020
ms.openlocfilehash: d7a3c86f3d9cf083a8746f753b8c5287c774a93e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91263264"
---
# <a name="machine-learning-services-in-azure-sql-managed-instance-preview"></a>Machine Learning Services in Azure SQL Managed instance (preview-versie)

Machine Learning Services is een functie van Azure SQL Managed instance (preview) dat in-data base-machine learning biedt en zowel python-als R-scripts ondersteunt. De functie bevat micro soft python en R-pakketten voor predictive analytics met hoge prestaties en machine learning. De relationele gegevens kunnen worden gebruikt in scripts via opgeslagen procedures, een T-SQL-script met python-of R-instructies of python-of R-code met T-SQL.

> [!IMPORTANT]
> Machine Learning Services is een functie van Azure SQL Managed Instance die momenteel beschikbaar is als openbare preview.
> Deze preview-functie is in eerste instantie beschikbaar in een beperkt aantal regio's in de VS, Azië en Australië, met extra regio's die later worden toegevoegd.
>
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.
>
> [Meld u aan voor de preview-versie](#signup) hieronder.

## <a name="what-is-machine-learning-services"></a>Wat is Machine Learning Services?

Met Machine Learning Services in Azure SQL Managed Instance kunt u python-en R-scripts uitvoeren in de-data base. U kunt dit gebruiken om gegevens voor te bereiden en op te schonen, functie techniek uit te voeren en machine learning modellen in een Data Base te trainen, te evalueren en te implementeren. De functie voert uw scripts uit waarin de gegevens zich bevinden en elimineert de overdracht van de gegevens over het netwerk naar een andere server.

Gebruik Machine Learning Services met R/python-ondersteuning in Azure SQL Managed instance voor het volgende:

- **Voer r-en python-scripts uit om gegevens voorbereiding en gegevens verwerking voor algemeen gebruik** uit te voeren. u kunt nu uw R/python-scripts overbrengen naar Azure SQL Managed instance waar uw gegevens zich bevinden, in plaats van gegevens naar een andere server te verplaatsen om R-en python-scripts uit te voeren. U kunt voor komen dat gegevens verplaatsing en gerelateerde problemen met betrekking tot latentie, beveiliging en naleving overbodig zijn.

- **Machine learning modellen trainen in data base** : u kunt modellen trainen met behulp van open-source-algoritmen. U kunt uw training eenvoudig schalen naar de hele gegevensset, in plaats van te vertrouwen op voorbeeld gegevens sets die uit de Data Base worden gehaald.

- **Implementeer uw modellen en scripts in productie in opgeslagen procedures** : de scripts en getrainde modellen kunnen eenvoudigweg worden operationeel door ze in de opgeslagen T-SQL-procedures in te sluiten. Apps die verbinding maken met een beheerd exemplaar van Azure SQL kunnen profiteren van voor spellingen en intelligentie in deze modellen door alleen een opgeslagen procedure aan te roepen. U kunt ook de native T-SQL-functie voors PELLEn gebruiken om modellen te operationeel maken, zodat u snel kunt scoren in zeer gelijktijdige Score scenario's voor realtime.

Basis distributies van python en R zijn opgenomen in Machine Learning Services. U kunt open source-pakketten en-frameworks installeren en gebruiken, zoals PyTorch, tensor flow en scikit-learn, naast de micro soft-pakketten [revoscalepy](https://docs.microsoft.com/sql/advanced-analytics/python/ref-py-revoscalepy) en [microsoftml](https://docs.microsoft.com/sql/advanced-analytics/python/ref-py-microsoftml) voor python, en [RevoScaleR](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler), [Microsoftml](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-microsoftml), [olapr](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-olapr)en [sqlrutils](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-sqlrutils) voor R.

<a name="signup"></a>

## <a name="sign-up-for-the-preview"></a>Registreren voor de preview-versie

Deze beperkte open bare preview is onderhevig aan de voor [waarden van Azure preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

Als u geïnteresseerd bent in het toevoegen van het preview-programma en deze voor waarden accepteert, kunt u de inschrijving aanvragen door een ondersteunings ticket voor Azure te maken op [**https://azure.microsoft.com/support/create-ticket/**](https://azure.microsoft.com/support/create-ticket/) . 

1. Selecteer de volgende opties:
   - Probleem type- **technisch**
   - Abonnement: *Selecteer uw abonnement*
   - **Beheerd exemplaar** van Service-SQL database
   - Samen vatting: *Voer een korte beschrijving van uw aanvraag in*
   - Probleem type- **Machine Learning Services voor een SQL-beheerd exemplaar (preview-versie)**
   - Subtype van probleem: **ander probleem of ' How to '-vragen**

1. Klik op **volgende: oplossingen**.

1. Lees de informatie over de preview-versie en klik vervolgens op **Details**.

1. In **Beschrijving**voert u de specifieke kenmerken van uw aanvraag in, waaronder de naam van de logische server, de regio en de abonnements-id die u wilt inschrijven in de preview-versie. Voer indien nodig nog meer details in.

1. Wanneer u klaar bent, klikt u op **volgende: bekijken en maken**en klikt u vervolgens op **maken**.

Zodra u voor het programma staat ingeschreven, vindt onboarding plaats voor de openbare preview en wordt Machine Learning Services voor uw bestaande of nieuwe database ingeschakeld.

Machine Learning Services in SQL Managed Instance wordt niet aanbevolen voor productieworkloads tijdens de openbare preview.

## <a name="next-steps"></a>Volgende stappen

- Bekijk de belangrijkste verschillen ten opzichte [van SQL Server machine learning Services](machine-learning-services-differences.md).
- Zie [python-scripts uitvoeren](https://docs.microsoft.com/sql/machine-learning/tutorials/quickstart-python-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=sql-server-ver15)voor meer informatie over het gebruik van python in Machine Learning Services.
- Zie [r-scripts uitvoeren](https://docs.microsoft.com/sql/machine-learning/tutorials/quickstart-r-create-script?context=/azure/azure-sql/managed-instance/context/ml-context&view=sql-server-ver15)voor meer informatie over het gebruik van r in Machine Learning Services.
- Zie de [documentatie voor sql machine learning](https://docs.microsoft.com/sql/machine-learning/)voor meer informatie over machine learning op andere SQL-platforms.
