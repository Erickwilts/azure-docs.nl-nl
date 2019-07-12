---
title: Werken met grote gegevenssets
description: Informatie over het ophalen en beheren van grote gegevenssets tijdens het werken met Azure Resource Graph.
author: DCtheGeek
ms.author: dacoulte
ms.date: 04/01/2019
ms.topic: conceptual
ms.service: resource-graph
manager: carmonm
ms.openlocfilehash: d04f46dbc60a7242e44d76915e15281cc6248d20
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786537"
---
# <a name="working-with-large-azure-resource-data-sets"></a>Werken met grote Azure-resource-gegevenssets

Azure Resource Graph is ontworpen voor het werken met en het ophalen van gegevens over resources in uw Azure-omgeving. Resourcegrafiek kunt ophalen van deze gegevens snel, zelfs bij het opvragen van duizenden records. Grafiek van resources beschikt over verschillende opties voor het werken met deze grote gegevenssets.

Zie voor instructies over het werken met query's met een hoge frequentie [richtlijnen voor beperkte aanvragen](./guidance-for-throttled-requests.md).

## <a name="data-set-result-size"></a>Grootte van de gegevensset

Standaard beperkt Resourcegrafiek een query voor het retourneren van alleen **100** records. Dit besturingselement voorkomt onbedoelde query's die in grote gegevenssets resulteren zouden dat de gebruiker en de service. Dit gebeurt meestal wanneer een klant experimenteren met query's om te zoeken en filteren van bronnen in de manier die bij hun specifieke behoeften past. Dit besturingselement is anders dan met behulp van de [boven](/azure/kusto/query/topoperator) of [limiet](/azure/kusto/query/limitoperator) Azure Data Explorer taal operators om de resultaten te beperken.

> [!NOTE]
> Bij het gebruik van **eerste**, het is raadzaam om de resultaten op ten minste één kolom met `asc` of `desc`. Zonder te sorteren zijn de geretourneerde resultaten willekeurige en niet opnieuw kunt.

De standaardlimiet kan worden genegeerd door alle methoden van interactie met Graph-Resource. De volgende voorbeelden laten zien hoe u de maximale grootte van de gegevensset te wijzigen _200_:

```azurecli-interactive
az graph query -q "project name | order by name asc" --first 200 --output table
```

```azurepowershell-interactive
Search-AzGraph -Query "project name | order by name asc" -First 200
```

In de [REST-API](/rest/api/azureresourcegraph/resources/resources), is het besturingselement **$top** en maakt deel uit van **QueryRequestOptions**.

Het besturingselement dat _meest beperkende_ wint. Bijvoorbeeld, als uw query gebruikt de **boven** of **limiet** operators en zou leiden tot meer records zijn dan **eerste**, het maximum aantal records geretourneerd zijn gelijk aan **Eerste**. Op dezelfde manier als **boven** of **limiet** kleiner is dan **eerste**, de recordset geretourneerde zou de kleinste waarde die is geconfigureerd door **boven** of **limiet**.

**Eerste** heeft momenteel een van de toegestane maximumwaarde _5000_.

## <a name="skipping-records"></a>Records overslaan

De volgende optie voor het werken met grote gegevenssets is de **overslaan** besturingselement. Met dit beheer kunt uw query te gaan of het opgegeven aantal records overslaan voordat het resultaat wordt geretourneerd. **Overslaan** is handig voor query's die resultaten sorteren op een begrijpelijke manier waar de bedoeling is om op te halen records ergens in het midden van de resultatenset. Als de resultaten die nodig aan het einde van de geretourneerde gegevens zijn, is het efficiënter en de configuratie van een andere sortering gebruiken in plaats daarvan de resultaten ophalen vanaf de bovenkant van de gegevensset.

> [!NOTE]
> Bij het gebruik van **overslaan**, het is raadzaam om de resultaten op ten minste één kolom met `asc` of `desc`. Zonder te sorteren zijn de geretourneerde resultaten willekeurige en niet opnieuw kunt.

De volgende voorbeelden laten zien hoe de eerste overslaan _10_ records een query zou leiden, in plaats daarvan vanaf het geretourneerde resultaat ingesteld met de 11e record:

```azurecli-interactive
az graph query -q "project name | order by name asc" --skip 10 --output table
```

```azurepowershell-interactive
Search-AzGraph -Query "project name | order by name asc" -Skip 10
```

In de [REST-API](/rest/api/azureresourcegraph/resources/resources), is het besturingselement **$skip** en maakt deel uit van **QueryRequestOptions**.

## <a name="paging-results"></a>Resultaten pagineren

Wanneer het nodig zijn aan het einde van een resultatenset in kleinere groepen van de records voor de verwerking is of omdat een resultatenset de maximaal toegestane aantal van overschrijdt _1000_ wisselbestand records geretourneerd, gebruiken. De [REST-API](/rest/api/azureresourcegraph/resources/resources) **QueryResponse** waarden om aan te geven van een set opgedeeld resultaten: **resultTruncated** en **$skipToken** .
**resultTruncated** is een Booleaanse waarde die u de consument informeert als er extra records niet in het antwoord geretourneerd. Dit probleem kan ook worden geïdentificeerd wanneer de **aantal** eigenschap is kleiner dan de **totalRecords** eigenschap. **totalRecords** definieert hoeveel records die overeenkomen met de query.

Wanneer **resultTruncated** is **waar**, wordt de **$skipToken** eigenschap is ingesteld in het antwoord. Deze waarde wordt gebruikt door de dezelfde waarden voor query's en -abonnement om op te halen van de volgende set met records die overeenkomen met de query.

De volgende voorbeelden ziet u hoe u **overslaan** het eerste 3000 records en geretourneerd de **eerste** 1000 records na die zijn overgeslagen met Azure CLI en Azure PowerShell:

```azurecli-interactive
az graph query -q "project id, name | order by id asc" --first 1000 --skip 3000
```

```azurepowershell-interactive
Search-AzGraph -Query "project id, name | order by id asc" -First 1000 -Skip 3000
```

> [!IMPORTANT]
> De query moet **project** de **id** veld in de volgorde voor paginering om te werken. Als deze ontbreekt uit de query, wordt het antwoord niet opgenomen de **$skipToken**.

Zie voor een voorbeeld [volgende pagina query](/rest/api/azureresourcegraph/resources/resources#next-page-query) in de REST-API-documenten.

## <a name="next-steps"></a>Volgende stappen

- Zie de taal die wordt gebruikt in [Starter query's](../samples/starter.md).
- Maakt gebruik van geavanceerde Zie in [geavanceerde query's](../samples/advanced.md).
- Meer informatie over het [resources verkennen](explore-resources.md).