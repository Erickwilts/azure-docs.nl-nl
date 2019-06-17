---
title: Gegevens visualiseren met behulp van de Azure Data Explorer-connector voor Power BI
description: 'In dit artikel leert u hoe u een van de drie opties voor het visualiseren van gegevens in Power BI: de Power BI-connector voor Azure Data Explorer.'
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/04/2019
ms.openlocfilehash: 0bfacc968a04f8ef9e39a31cff5e81cf4e04c6fb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66494637"
---
# <a name="visualize-data-using-the-azure-data-explorer-connector-for-power-bi"></a>Gegevens visualiseren met behulp van de Azure Data Explorer-connector voor Power BI

Azure Data Explorer is een snelle en zeer schaalbare service om gegevens in logboeken en telemetrie te verkennen. Power BI is een business analytics-oplossing waarmee u uw gegevens kunt visualiseren en de gegevens kunt delen in uw organisatie.

Azure Data Explorer biedt drie opties om gegevens te verbinden in Power BI: de ingebouwde connector gebruiken, een query importeren uit Azure Data Explorer, of een SQL-query gebruiken. Dit artikel leest u hoe de ingebouwde connector gebruiken voor het ophalen van gegevens en visualiseren in een Power BI-rapport.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis Azure-account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende om te voltooien in dit artikel nodig:

* Een organisatie-e-mailaccount dat lid is van Azure Active Directory, zodat u verbinding kunt maken met het [Azure Data Explorer-helpcluster](https://dataexplorer.azure.com/clusters/help/databases/samples).

* [Power BI Desktop](https://powerbi.microsoft.com/get-started/) (selecteer **GRATIS DOWNLOADEN**)

## <a name="get-data-from-azure-data-explorer"></a>Gegevens ophalen uit Azure Data Explorer

Eerst maakt u verbinding met het Azure Data Explorer-helpcluster en daarna haalt u een subset gegevens op uit de tabel *StormEvents*. [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

1. Ga in Power BI Desktop naar het tabblad **Start** en selecteer de optie **Gegevens ophalen** en vervolgens **Meer**.

    ![Gegevens ophalen](media/power-bi-connector/get-data-more.png)

1. Zoeken naar *Azure Data Explorer*, selecteer **Azure Data Explorer** vervolgens **Connect**.

    ![Gegevens zoeken en ophalen](media/power-bi-connector/search-get-data.png)

1. Op de **Azure Data Explorer (Kusto)** scherm, vul het formulier in met de volgende informatie.

    ![Cluster-, database-, tabelopties](media/power-bi-connector/cluster-database-table.png)

    **Instelling** | **Waarde** | **Beschrijving van veld**
    |---|---|---|
    | Cluster | *https://help.kusto.windows.net* | De URL voor het helpcluster. Voor andere clusters heeft de URL de notatie *https://\<ClusterName\>.\<Regio\>. kusto.windows.net*. |
    | Database | Leeg laten | Een database die wordt gehost op het cluster waarmee u verbinding maakt. Deze selecteren we in een latere stap. |
    | Tabelnaam | Leeg laten | Een van de tabellen in de database, of een query zoals <code>StormEvents \| take 1000</code>. Deze selecteren we in een latere stap. |
    | Geavanceerde opties | Leeg laten | Opties voor uw query's, zoals grootte van resultatenset. |
    | Gegevensverbindingsmodus | *DirectQuery* | Bepaalt of Power BI de gegevens importeert of rechtstreeks verbinding maakt met de gegevensbron. Met deze connector kunt u een van beide opties gebruiken. |
    | | | |

1. Meld u aan als u nog geen verbinding hebt met het helpcluster. Meld u aan met een organisatieaccount en selecteer **Verbinding maken**.

    ![Aanmelden](media/power-bi-connector/sign-in.png)

1. In het scherm **Navigator** vouwt u de database **Voorbeelden** uit en selecteert u eerst **StormEvents** en vervolgens **Bewerken**.

    ![Tabel selecteren](media/power-bi-connector/select-table.png)

    De tabel wordt geopend in Power Query-editor. Hier kunt u rijen en kolommen bewerken voordat u de gegevens importeert.

1. In de Power Query-editor selecteert u de pijl naast de kolom **DamageCrops** en klikt u op **Aflopend sorteren**.

    ![DamageCrops aflopend sorteren](media/power-bi-connector/sort-descending.png)

1. Op het tabblad **Start** selecteert u **Rijen behouden** en vervolgens **Bovenste rijen behouden**. Voer de waarde *1000* in om de bovenste 1000 rijen van de gesorteerde tabel te behouden.

    ![Bovenste rijen behouden](media/power-bi-connector/keep-top-rows.png)

1. Selecteer op het tabblad **Start** de optie **Sluiten en toepassen**.

    ![Sluiten en toepassen](media/power-bi-connector/close-apply.png)

## <a name="visualize-data-in-a-report"></a>Gegevens visualiseren in een rapport

[!INCLUDE [data-explorer-power-bi-visualize-basic](../../includes/data-explorer-power-bi-visualize-basic.md)]

## <a name="clean-up-resources"></a>Resources opschonen

Als u het rapport dat u hebt gemaakt niet meer nodig voor dit artikel, verwijdert u de Power BI Desktop (.pbix)-bestand.

## <a name="next-steps"></a>Volgende stappen

[Gegevens visualiseren met behulp van een geïmporteerde query in Power BI](power-bi-imported-query.md)
