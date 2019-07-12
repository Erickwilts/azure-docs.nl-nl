---
title: Aan de slag met Azure Data Lake Analytics met Azure CLI
description: Leer hoe u de Azure-opdrachtregelinterface gebruiken om te maken van een Azure Data Lake Analytics-account en een U-SQL-taak verzenden.
ms.service: data-lake-analytics
services: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.topic: conceptual
ms.date: 06/18/2017
ms.openlocfilehash: 2af6d499bafb0e00b31d0379baac6a390bd6ca3f
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67626239"
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-cli"></a>Aan de slag met Azure Data Lake Analytics met Azure CLI
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

In dit artikel wordt beschreven hoe u de opdrachtregelinterface van Azure CLI gebruiken voor het maken van Azure Data Lake Analytics-accounts, USQL taken en -catalogussen indienen. De taak leest een TSV-bestand (door tabs gescheiden waarden) en converteert dat naar een CSV-bestand (door komma's gescheiden waarden). 

## <a name="prerequisites"></a>Vereisten
Voordat u begint, hebt u de volgende items nodig:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* In dit artikel is vereist dat u de Azure CLI versie 2.0 of hoger worden uitgevoerd. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. 



## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Aanmelden bij uw Azure-abonnement:

```
azurecli
az login
```

U wordt gevraagd naar een URL te gaan en een verificatiecode in te voeren.  Volg daarna de instructies om uw referenties in te voeren.

Wanneer u bent aangemeld, worden uw abonnementen weergegeven.

Een specifiek abonnement gebruiken:

```
az account set --subscription <subscription id>
```

## <a name="create-data-lake-analytics-account"></a>Een Data Lake Analytics-account maken
U hebt een Data Lake Analytics-account nodig voordat u taken kunt uitvoeren. Geef de volgende items op om een Data Lake Analytics-account te maken:

* **Azure-resourcegroep**. Er moet een Data Lake Analytics-account zijn gemaakt binnen een Azure-resourcegroep. Met [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) kunt u met de resources in uw toepassing als groep gebruiken. U kunt alle resources voor uw toepassing implementeren, bijwerken of verwijderen in een enkele, gecoördineerde bewerking.  

De bestaande resourcegroepen onder uw abonnement weergeven:

```
az group list
```

Een nieuwe resourcegroep maken:

```
az group create --name "<Resource Group Name>" --location "<Azure Location>"
```

* **Naam van Data Lake Analytics-account**. Elk Data Lake Analytics-account heeft een naam.
* **Locatie**. Gebruik een van de Azure-datacenters die ondersteuning bieden voor Data Lake Analytics.
* **Standaard Data Lake Store-account**: Elk Data Lake Analytics-account heeft een Data Lake Store-standaardaccount.

De bestaande Data Lake Store-accounts weergeven:

```
az dls account list
```

Een nieuw Data Lake Store-account maken:

```azurecli
az dls account create --account "<Data Lake Store Account Name>" --resource-group "<Resource Group Name>"
```

Gebruik de volgende syntaxis om een Data Lake Analytics-account te maken:

```
az dla account create --account "<Data Lake Analytics Account Name>" --resource-group "<Resource Group Name>" --location "<Azure location>" --default-data-lake-store "<Default Data Lake Store Account Name>"
```

Nadat u een account hebt gemaakt, kunt u de volgende opdrachten gebruiken om de accounts met de accountdetails weer te geven:

```
az dla account list
az dla account show --account "<Data Lake Analytics Account Name>"            
```

## <a name="upload-data-to-data-lake-store"></a>Gegevens uploaden naar Data Lake Store
In deze zelfstudie verwerkt u een aantal zoeklogboeken.  Het zoeklogboek kan worden opgeslagen in de Data Lake Store of Azure Blob-opslag.

Azure Portal biedt een gebruikersinterface waarmee u een aantal voorbeeldbestanden kunt kopiëren naar het Data Lake Store-account, waaronder een zoeklogboekbestand. Zie [Brongegevens voorbereiden](data-lake-analytics-get-started-portal.md) om de gegevens te uploaden naar het Data Lake Store-standaardaccount.

Om bestanden te uploaden met behulp van Azure CLI, gebruikt u de volgende opdrachten:

```
az dls fs upload --account "<Data Lake Store Account Name>" --source-path "<Source File Path>" --destination-path "<Destination File Path>"
az dls fs list --account "<Data Lake Store Account Name>" --path "<Path>"
```

Data Lake Analytics heeft ook toegang tot Azure Blob-opslag.  Zie [De Azure CLI gebruiken met Azure Storage](../storage/common/storage-azure-cli.md) voor informatie over het uploaden van gegevens naar Azure Blob-opslag.

## <a name="submit-data-lake-analytics-jobs"></a>Data Lake Analytics-taken verzenden
Data Lake Analytics-taken worden geschreven in de U-SQL-taal. Zie [Aan de slag met de U-SQL-taal](data-lake-analytics-u-sql-get-started.md) en [Naslaginformatie voor de U-SQL-taal](https://docs.microsoft.com/u-sql/) voor meer informatie over U-SQL.

**Een Data Lake Analytics-taakscript maken**

Maak een tekstbestand met het volgende U-SQL-script en bewaar het tekstbestand op uw werkstation:

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

Dit U-SQL-script leest het brongegevensbestand met **Extractors.Tsv()** en maakt vervolgens een CSV-bestand met **Outputters.Csv()** .

Wijzig de twee paden niet, tenzij u het bronbestand naar een andere locatie kopieert.  Data Lake Analytics maakt de uitvoermap als deze nog niet bestaat.

Het is eenvoudiger om relatieve paden te gebruiken voor bestanden die zijn opgeslagen in Data Lake Store-standaardaccounts. Maar u kunt ook absolute paden gebruiken.  Bijvoorbeeld:

```
adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
```

U moet absolute paden gebruiken om toegang te krijgen tot bestanden in gekoppelde Storage-accounts.  De syntaxis voor bestanden die zijn opgeslagen in het gekoppelde Azure Storage-account is:

```
wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv
```

> [!NOTE]
> Azure Blob-containers met openbare blobs worden niet ondersteund.      
> Azure Blob-containers met openbare containers worden niet ondersteund.      
>

**Taken verzenden**

Gebruik de volgende syntaxis om een taak te verzenden.

```
az dla job submit --account "<Data Lake Analytics Account Name>" --job-name "<Job Name>" --script "<Script Path and Name>"
```

Bijvoorbeeld:

```
az dla job submit --account "myadlaaccount" --job-name "myadlajob" --script @"C:\DLA\myscript.txt"
```

**Taken en taakdetails weergeven**

```
azurecli
az dla job list --account "<Data Lake Analytics Account Name>"
az dla job show --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

**Taken annuleren**

```
az dla job cancel --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

## <a name="retrieve-job-results"></a>Taakresultaten ophalen

Wanneer een taak is voltooid, kunt u de volgende opdrachten gebruiken om de uitvoerbestanden weer te geven en te downloaden:

```
az dls fs list --account "<Data Lake Store Account Name>" --source-path "/Output" --destination-path "<Destination>"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv" --length 128 --offset 0
az dls fs download --account "<Data Lake Store Account Name>" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destination-path "<Destination Path and File Name>"
```

Bijvoorbeeld:

```
az dls fs download --account "myadlsaccount" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destination-path "C:\DLA\myfile.csv"
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor het naslagdocument van Data Lake Analytics Azure CLI, [Data Lake Analytics](/cli/azure/dla).
* Zie voor het naslagdocument van Data Lake Store Azure CLI, [Data Lake Store](/cli/azure/dls).
* Zie [Websitelogboeken analyseren met Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md) voor een complexere query.
