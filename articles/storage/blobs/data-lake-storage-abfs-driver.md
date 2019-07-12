---
title: Het bestandssysteem van Azure Blob-stuurprogramma voor Azure Data Lake Storage Gen2
description: Het stuurprogramma ABFS Hadoop-bestandssysteem
services: storage
author: normesta
ms.topic: conceptual
ms.author: normesta
ms.reviewer: jamesbak
ms.date: 12/06/2018
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: abe3f67141011c765f9de93bcf51998ddae002cb
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67696145"
---
# <a name="the-azure-blob-filesystem-driver-abfs-a-dedicated-azure-storage-driver-for-hadoop"></a>Het stuurprogramma van het Azure Blob-bestandssysteem (ABFS): Een speciale Azure Storage-stuurprogramma voor Hadoop

Een van de primaire methoden voor gegevens in Azure Data Lake Storage Gen2 is via de [Hadoop FileSystem](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/index.html). Data Lake Storage Gen2 kunnen gebruikers van Azure Blob Storage-toegang tot een nieuw stuurprogramma, het bestandssysteem van Azure Blob-stuurprogramma of `ABFS`. ABFS maakt deel uit van Apache Hadoop en is opgenomen in veel van de commerciële distributies van Hadoop. Dit stuurprogramma gebruikt, veel toepassingen en frameworks toegang tot gegevens in Azure Blob-opslag zonder code expliciet verwijzen naar Data Lake Storage Gen2.

## <a name="prior-capability-the-windows-azure-storage-blob-driver"></a>Eerdere mogelijkheid: Het Windows Azure Storage-Blob-stuurprogramma

Het Windows Azure Storage-Blob-stuurprogramma of [WASB-stuurprogramma](https://hadoop.apache.org/docs/current/hadoop-azure/index.html) opgegeven van de oorspronkelijke ondersteuning voor Azure Blob Storage. Dit stuurprogramma uitgevoerd de moeilijke taak van het bestandssysteem toewijzing semantiek (zoals vereist door de interface Hadoop FileSystem) met die van het object opslaan stijlinterface beschikbaar is gemaakt door Azure Blob Storage. Dit stuurprogramma blijft ondersteuning bieden voor dit model, hoge prestaties toegang tot gegevens in Blobs, maar bevat een aanzienlijke hoeveelheid code voor het uitvoeren van deze toewijzing, waardoor het moeilijk te onderhouden. Bovendien enkele bewerkingen zoals [FileSystem.rename()](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_renamePath_src_Path_d) en [FileSystem.delete()](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_deletePath_p_boolean_recursive) vereisen dat het stuurprogramma voor het uitvoeren van een groot aantal bewerkingen (vanwege een object winkels gebrek wanneer toegepast op mappen ondersteuning voor mappen) die vaak leiden tot slechtere prestaties. Het stuurprogramma ABFS is ontworpen om te strijden tegen de inherente tekortkomingen van WASB.

## <a name="the-azure-blob-file-system-driver"></a>Het bestandssysteem van Azure Blob-stuurprogramma

De [Azure Data Lake Storage REST-interface](https://docs.microsoft.com/rest/api/storageservices/data-lake-storage-gen2) is ontworpen voor ondersteuning van semantiek van het bestandssysteem via Azure Blob Storage. Gezien het feit dat de Hadoop FileSystem ook ontworpen is ter ondersteuning van dezelfde semantiek is niet vereist voor de toewijzing van een complexe in het stuurprogramma. Het stuurprogramma (of ABFS) Azure Blob-bestandssysteem is dus een shim louter client voor de REST-API.

Er zijn echter enkele functies die het stuurprogramma moet nog steeds uitvoeren:

### <a name="uri-scheme-to-reference-data"></a>URI-schema met verwijzingsgegevens

Consistent is met andere implementaties van het bestandssysteem in Hadoop, het stuurprogramma ABFS definieert een eigen URI-schema zodat bronnen (mappen en bestanden) wachtwoorddelen afzonderlijk kunnen worden opgelost. Het URI-schema wordt gedocumenteerd in [gebruiken de Azure Data Lake Storage Gen2 URI](./data-lake-storage-introduction-abfs-uri.md). De structuur van de URI is: `abfs[s]://file_system@account_name.dfs.core.windows.net/<path>/<path>/<file_name>`

Met behulp van de bovenstaande URI-indeling, standaard Hadoop-hulpprogramma's en frameworks kunnen worden gebruikt om te verwijzen naar deze resources:

```bash
hdfs dfs -mkdir -p abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data 
hdfs dfs -put flight_delays.csv abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data/ 
```

Intern, het stuurprogramma ABFS vertaalt de resource (s) opgegeven in de URI op bestanden en mappen en aanroepen van de Azure Data Lake Storage REST API met deze verwijzingen.

### <a name="authentication"></a>Authentication

Het stuurprogramma ABFS ondersteunt twee soorten verificatie zodat de Hadoop-toepassing veilig toegang krijgen resources die zich in een Data Lake Storage Gen2 geschikt-account tot kan. Volledige details van de beschikbare verificatiemethoden zijn opgegeven de [Azure Storage-beveiligingshandleiding](../common/storage-security-guide.md). Dit zijn:

- **Gedeelde sleutel:** Hierdoor kunnen gebruikers toegang tot alle resources in het account. De sleutel is versleuteld en opgeslagen in de configuratie van Hadoop.

- **OAuth-Bearer-Token van Azure Active Directory:** Azure AD-bearer-tokens worden verkregen en vernieuwd door het stuurprogramma met de identiteit van de eindgebruiker of via een geconfigureerde Service-Principal. Met dit verificatiemodel, is alle toegang geautoriseerd op basis van per aanroep met behulp van de identiteit die is gekoppeld aan het opgegeven token en geëvalueerd op basis van de toegewezen POSIX lijst met ACL (Access Control).

### <a name="configuration"></a>Configuratie

Alle configuratie voor het stuurprogramma ABFS wordt opgeslagen in de <code>core-site.xml</code> configuratiebestand. In een Hadoop-distributie van [Ambari](https://ambari.apache.org/), de configuratie kan ook worden beheerd met behulp van de webportal of de Ambari REST-API.

Details van alle ondersteunde configuratie-items zijn opgegeven in de [officiële Hadoop-documentatie](https://hadoop.apache.org/docs/current/hadoop-azure/index.html).

### <a name="hadoop-documentation"></a>Hadoop-documentatie

Het stuurprogramma ABFS zijn volledig gedocumenteerd in de [officiële Hadoop-documentatie](https://github.com/apache/hadoop/blob/trunk/hadoop-tools/hadoop-azure/src/site/markdown/abfs.md)

## <a name="next-steps"></a>Volgende stappen

- [Een Azure Databricks-Cluster maken](./data-lake-storage-quickstart-create-databricks-account.md)
- [URI van Azure Data Lake Storage Gen2 gebruiken](./data-lake-storage-introduction-abfs-uri.md)
