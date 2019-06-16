---
title: Eigenschappen van het object en de metagegevens in Azure Storage instellen en ophalen | Microsoft Docs
description: Aangepaste metagegevens voor objecten in Azure Storage, Store en instellen en ophalen van Systeemeigenschappen.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/03/2019
ms.author: tamram
ms.openlocfilehash: e8a85319a12f04a11e3914716d9ff84cdb6de8d8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65787866"
---
# <a name="set-and-retrieve-properties-and-metadata"></a>Eigenschappen en metagegevens instellen en ophalen

Objecten in Azure Storage ondersteuning Systeemeigenschappen en door gebruiker gedefinieerde metagegevens, naast de gegevens die ze bevatten. Dit artikel worden de eigenschappen van het beheren en de gebruiker gedefinieerde metagegevens met de [Azure Storage-clientbibliotheek voor .NET](/dotnet/api/overview/azure/storage/client).

* **Systeemeigenschappen**: Systeemeigenschappen aanwezig zijn op elke resource voor opslag. Enkele van deze kunnen worden gelezen of ingesteld, terwijl andere alleen-lezen zijn. Op de achtergrond, sommige Systeemeigenschappen komen overeen met bepaalde standaard HTTP-headers. Azure Storage-clientbibliotheken onderhouden deze eigenschappen voor u.

* **Gebruiker gedefinieerde metagegevens**: Gebruiker gedefinieerde metagegevens bestaat uit een of meer naam / waarde-paren die u voor een Azure Storage-resource opgeeft. U kunt de metagegevens voor het opslaan van aanvullende waarden met een resource. De metagegevens van waarden zijn voor alleen uw eigen doeleinden, en niet van invloed op het gedrag van de resource.

Bij het ophalen van waarden van eigenschappen en metagegevens voor een opslagresource is een proces in twee stappen. Voordat u deze waarden lezen kunt, u moet expliciet ophalen ze door het aanroepen van de **FetchAttributes** of **FetchAttributesAsync** methode. De uitzondering hierop is als u verbinding maakt met de **Exists** of **ExistsAsync** methode op een resource. Wanneer u een van deze methoden aanroepen, Azure Storage roept de juiste **FetchAttributes** methode op de achtergrond als onderdeel van de aanroep naar de **Exists** methode.

> [!IMPORTANT]
> Als u merkt dat de eigenschap of metagegevens waarden voor een opslagresource niet zijn ingevuld, controleert u dat uw code roept de **FetchAttributes** of **FetchAttributesAsync** methode.
>
> Metagegevens van de naam/waarde-paren zijn geldige HTTP-headers en dus moeten voldoen aan alle beperkingen met betrekking tot HTTP-headers. Namen van metagegevens moet een geldige naam van de HTTP-header en geldige C# -id's, mag alleen ASCII-tekens bevatten en moet worden behandeld als niet-hoofdlettergevoelig. Waarden voor metagegevens die niet-ASCII-tekens bevat die moeten worden met Base64 gecodeerde of gecodeerde URL.

## <a name="setting-and-retrieving-properties"></a>Instellen en ophalen van eigenschappen
Aanroepen om op te halen eigenschapswaarden, de **FetchAttributesAsync** methode voor uw blob of container voor het vullen van de eigenschappen, leest u de waarden.

Eigenschappen worden ingesteld op een object, geeft u de eigenschap value en roep vervolgens de **SetProperties** methode.

Het volgende codevoorbeeld wordt een container gemaakt, en sommige van de eigenschapswaarden schrijft naar een consolevenster.

```csharp
//Parse the connection string for the storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

//Create the service client object for credentialed access to the Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Create the container if it does not already exist.
container.CreateIfNotExists();

// Fetch container properties and write out their values.
await container.FetchAttributesAsync();
Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
Console.WriteLine("ETag: {0}", container.Properties.ETag);
Console.WriteLine();
```

## <a name="setting-and-retrieving-metadata"></a>Instellen en ophalen van metagegevens
U kunt metagegevens opgeven als een of meer naam / waarde-paren van een blob of container-resource. Toevoegen om in te stellen metagegevens, naam / waarde-paren aan de **metagegevens** verzameling op de resource, roept u vervolgens de **SetMetadata** of **SetMetadataAsync** methode voor het opslaan van de waarden die de de service.

> [!NOTE]
> De naam van de metagegevens moet voldoen aan de naamgevingsconventies voor C#-id's.
>
>

Het volgende codevoorbeeld Hiermee stelt u de metagegevens voor een container. Een waarde is ingesteld met behulp van de verzameling **toevoegen** methode. De andere waarde is ingesteld met behulp van impliciete sleutel/waarde-syntaxis. Beide zijn geldig.

```csharp
public static async Task AddContainerMetadataAsync(CloudBlobContainer container)
{
    // Add some metadata to the container.
    container.Metadata.Add("docType", "textDocuments");
    container.Metadata["category"] = "guidance";

    // Set the container's metadata.
    await container.SetMetadataAsync();
}
```

Als u wilt ophalen van metagegevens, aanroepen de **FetchAttributes** of **FetchAttributesAsync** methode voor uw blob of container voor het vullen van de **metagegevens** verzameling, leest u de waarden, zoals wordt weergegeven in het onderstaande voorbeeld.

```csharp
public static async Task ListContainerMetadataAsync(CloudBlobContainer container)
{
    // Fetch container attributes in order to populate the container's properties and metadata.
    await container.FetchAttributesAsync();

    // Enumerate the container's metadata.
    Console.WriteLine("Container metadata:");
    foreach (var metadataItem in container.Metadata)
    {
        Console.WriteLine("\tKey: {0}", metadataItem.Key);
        Console.WriteLine("\tValue: {0}", metadataItem.Value);
    }
}
```

## <a name="next-steps"></a>Volgende stappen
* [Azure Storage-clientbibliotheek voor .NET-verwijzing](/dotnet/api/?term=Microsoft.Azure.Storage)
* [Azure Blob storage-clientbibliotheek voor .NET-pakket](https://www.nuget.org/packages/Microsoft.Azure.Storage.Blob/)
* [Azure Queue storage-clientbibliotheek voor .NET-pakket](https://www.nuget.org/packages/Microsoft.Azure.Storage.Queue/)
* [Azure File storage-clientbibliotheek voor .NET-pakket](https://www.nuget.org/packages/Microsoft.Azure.Storage.File/)

