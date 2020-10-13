---
title: Score machine learning modellen met voor spel
description: Meer informatie over het beoordelen van machine learning modellen met behulp van de T-SQL-functie voors PELLEn in Synapse SQL.
services: synapse-analytics
author: anumjs
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: machine-learning
ms.date: 07/21/2020
ms.author: anjangsh
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: ef56274e0bda3f1a9d494852520a77ecdfc25799
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "89048003"
---
# <a name="score-machine-learning-models-with-predict"></a>Score machine learning modellen met voor spel

Synapse SQL biedt u de mogelijkheid om machine learning modellen te scoren met behulp van de vertrouwde T-SQL-taal. Met T-SQL-voor [spel](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=azure-sqldw-latest)kunt u uw bestaande machine learning-modellen getraind met historische gegevens en ze een score geven binnen de beveiligde grenzen van uw data warehouse. De functie voors PELLEn heeft een [ONNX-model (open Neural Network Exchange)](https://onnx.ai/) en gegevens als invoer. Deze functie elimineert de stap voor het verplaatsen van waardevolle gegevens buiten het Data Warehouse voor een score. Het is erop gericht om data professionals in staat te stellen om eenvoudig machine learning modellen te implementeren met de vertrouwde T-SQL-interface en naadloos samen te werken met gegevens wetenschappers die samen werken met het juiste Framework voor hun taak.

> [!NOTE]
> Deze functionaliteit wordt momenteel niet ondersteund in SQL op aanvraag.

Voor de functionaliteit moet het model buiten Synapse SQL worden getraind. Nadat u het model hebt gemaakt, laadt u het in het Data Warehouse en verdeelt u het met de syntaxis van de T-SQL-voor speling om inzicht te krijgen in de gegevens.

![predictoverview](./media/sql-data-warehouse-predict/datawarehouse-overview.png)

## <a name="training-the-model"></a>Het model trainen

Synapse SQL verwacht een vooraf getraind model. Houd rekening met de volgende factoren wanneer u een machine learning model wilt trainen dat wordt gebruikt voor het uitvoeren van voor spellingen in Synapse SQL.

- Synapse SQL ondersteunt alleen ONNX-indelings modellen. ONNX is een open-source model indeling waarmee u modellen tussen verschillende Frameworks kunt uitwisselen om interoperabiliteit mogelijk te maken. U kunt uw bestaande modellen converteren naar de ONNX-indeling met behulp van frameworks die de systeem eigen ondersteuning bieden of pakketten converteren. Bijvoorbeeld [sklearn-onnx-](https://github.com/onnx/sklearn-onnx) pakket converteren scikit-modellen naar onnx. [ONNX github-opslag plaats](https://github.com/onnx/tutorials#converting-to-onnx-format) bevat een lijst met ondersteunde frameworks en voor beelden.

   Als u gebruikmaakt van [automatische ml](https://docs.microsoft.com/azure/machine-learning/concept-automated-ml) voor training, moet u de para meter *ENABLE_ONNX_COMPATIBLE_MODELS* instellen op True om een onnx-indelings model te maken. [Automatische machine learning notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb) toont een voor beeld van hoe u AutoML kunt gebruiken om een machine learning model van ONNX-indeling te maken.

- De volgende gegevens typen worden ondersteund voor de invoer gegevens:
    - int, bigint, Real, float
    - char, varchar, nvarchar

- De Score gegevens moeten dezelfde indeling hebben als de trainings gegevens. Complexe gegevens typen, zoals matrices met meerdere dimensies, worden niet ondersteund door voor SPELing. Voor de training moet u er dus voor zorgen dat elke invoer van het model overeenkomt met één kolom van de score tabel in plaats van één matrix met alle invoer te geven.

- Zorg ervoor dat de namen en gegevens typen van de model invoer overeenkomen met de kolom namen en gegevens typen van de nieuwe Voorspellings gegevens. Het visualiseren van een ONNX-model met behulp van verschillende open-source hulpprogram ma's die online beschikbaar zijn, kunnen meer hulp bieden bij fout opsporing.

## <a name="loading-the-model"></a>Het model laden

Het model wordt opgeslagen in een Synapse SQL-gebruikers tabel als een hexadecimale teken reeks. Aanvullende kolommen, zoals ID en beschrijving, kunnen worden toegevoegd in de model tabel om het model te identificeren. Gebruik varbinary (max) als het gegevens type van de kolom model. Hier volgt een code voorbeeld voor een tabel die kan worden gebruikt voor het opslaan van modellen:

```sql
-- Sample table schema for storing a model and related data
CREATE TABLE [dbo].[Models]
(
    [Id] [int] IDENTITY(1,1) NOT NULL,
    [Model] [varbinary](max) NULL,
    [Description] [varchar](200) NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN,
    HEAP
)
GO

```

Wanneer het model is geconverteerd naar een hexadecimale teken reeks en de definitie van de tabel, gebruikt u de [Kopieer opdracht](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) of poly Base om het model in de SQL-tabel Synapse te laden. In het volgende code voorbeeld wordt de Kopieer opdracht gebruikt om het model te laden.

```sql
-- Copy command to load hexadecimal string of the model from Azure Data Lake storage location
COPY INTO [Models] (Model)
FROM '<enter your storage location>'
WITH (
    FILE_TYPE = 'CSV',
    CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='<enter your storage key here>')
)
```

## <a name="scoring-the-model"></a>Het model scoren

Zodra het model en de gegevens in het Data Warehouse zijn geladen, gebruikt u de functie voor het voors **pellen van T-SQL** om het model te scoren. Zorg ervoor dat de nieuwe invoer gegevens zich in dezelfde indeling bevinden als de trainings gegevens die worden gebruikt voor het bouwen van het model. T-SQL-voor SPELing heeft twee invoer: model en nieuwe score invoer gegevens en genereert nieuwe kolommen voor de uitvoer. Het model kan worden opgegeven als een variabele, een letterlijke of scalaire sub_query. Gebruik [WITH common_table_expression](https://docs.microsoft.com/sql/t-sql/queries/with-common-table-expression-transact-sql?view=sql-server-ver15) om een benoemde resultatenset voor de para meter data op te geven.

In het onderstaande voor beeld ziet u een voorbeeld query met de functie voor spelling controle. Er wordt een extra kolom met de naam *Score* en het gegevens type *float* gemaakt met de Voorspellings resultaten. Alle kolommen met invoer gegevens en uitvoer Voorspellings kolommen kunnen met de instructie SELECT worden weer gegeven. Zie voor [spel (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=azure-sqldw-latest)voor meer informatie.

```sql
-- Query for ML predictions
SELECT d.*, p.Score
FROM PREDICT(MODEL = (SELECT Model FROM Models WHERE Id = 1),
DATA = dbo.mytable AS d) WITH (Score float) AS p;
```

## <a name="next-steps"></a>Volgende stappen

Zie voor [speling (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=azure-sqldw-latest)voor meer informatie over de functie VOORs pellen.
