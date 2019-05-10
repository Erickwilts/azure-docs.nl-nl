---
title: Afbeeldingsgegevens uploaden in de cloud met Azure Storage | Microsoft Docs
description: Azure Blob-opslag gebruiken met een web-app om toepassingsgegevens op te slaan
services: storage
author: normesta
ms.service: storage
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 11/26/2018
ms.author: normesta
ms.reviewer: seguler
ms.custom: mvc
ms.openlocfilehash: 26b92db330c882aaf258b6e24560cbf2f7930a5f
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65237120"
---
# <a name="tutorial-upload-image-data-in-the-cloud-with-azure-storage"></a>Zelfstudie: Afbeeldingsgegevens uploaden in de cloud met Azure Storage

Deze zelfstudie is deel één van een serie. In deze zelfstudie leert u hoe u een web-app implementeert die de Azure Storage-clientbibliotheek gebruikt om afbeeldingen te uploaden in een opslagaccount. Wanneer u klaar bent, hebt u een web-app die afbeeldingen vanuit Azure-opslag kan opslaan en weergeven.

# <a name="nettabdotnet"></a>[\.NET](#tab/dotnet)
![Containerweergave van afbeeldingen](media/storage-upload-process-images/figure2.png)

# <a name="nodejs-v2-sdktabnodejs"></a>[Node.js V2 SDK](#tab/nodejs)
![Containerweergave van afbeeldingen](media/storage-upload-process-images/upload-app-nodejs-thumb.png)

# <a name="nodejs-v10-sdktabnodejsv10"></a>[Node.js V10 SDK](#tab/nodejsv10)
![Containerweergave van afbeeldingen](media/storage-upload-process-images/upload-app-nodejs-thumb.png)

---

In deel 1 van de reeks leert u het volgende:

> [!div class="checklist"]
> * Create a storage account
> * Een container maken en machtigingen instellen
> * Een toegangssleutel ophalen
> * Een web-app implementeren in Azure
> * App-instellingen configureren
> * Interactie met de web-app

## <a name="prerequisites"></a>Vereisten

U hebt een Azure-abonnement nodig om deze zelfstudie te voltooien. Maak een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u de CLI lokaal installeert en gebruikt, moet u voor deze zelfstudie de Azure CLI versie 2.0.4 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Een resourcegroep maken 

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.  

In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt.

```azurecli-interactive
az group create --name myResourceGroup --location southeastasia 
```

## <a name="create-a-storage-account"></a>Create a storage account

Met het voorbeeld worden afbeeldingen geüpload naar een blobcontainer in een Azure Storage-account. Een opslagaccount biedt een unieke naamruimte voor het opslaan en openen van uw Azure Storage-gegevensobjecten. Maak een opslagaccount in de resourcegroep die u hebt gemaakt met behulp van de opdracht [az storage account create](/cli/azure/storage/account).

> [!IMPORTANT]
> In deel 2 van de zelfstudie gebruikt u Azure Event Grid met Blob-opslag. Zorg dat u het opslagaccount maakt in een Azure-regio die ondersteuning biedt voor Event Grid. Zie [Azure-producten per regio](https://azure.microsoft.com/global-infrastructure/services/?products=event-grid&regions=all) voor een lijst met ondersteunde regio's.

Vervang de tijdelijke aanduiding `<blob_storage_account>` in de volgende opdracht door uw eigen, wereldwijd unieke naam voor het Blob-opslagaccount.  

```azurecli-interactive
blobStorageAccount=<blob_storage_account>

az storage account create --name $blobStorageAccount \
--location southeastasia --resource-group myResourceGroup \
--sku Standard_LRS --kind blobstorage --access-tier hot 
```

## <a name="create-blob-storage-containers"></a>Blob-opslagcontainers maken

De app gebruikt twee containers in het Blob Storage-account. Containers zijn vergelijkbaar met mappen. Hier worden blobs opgeslagen. In de container *images* worden afbeeldingen in volledige resolutie opgeslagen. In een later deel van de serie leert u hoe een Azure-functie-app verkleinde afbeeldingsminiaturen naar de container *thumbnails* uploadt.

Haal de opslagaccountsleutel op met behulp van de opdracht [az storage account keys list](/cli/azure/storage/account/keys). Gebruik vervolgens deze sleutel om twee containers te maken met de opdracht [az storage container create](/cli/azure/storage/container).  

De openbare toegang tot de container met de *installatiekopieën* is ingesteld op `off`. De openbare toegang tot de container met *miniaturen* is ingesteld op `container`. De instelling `container` van de openbare toegang zorgt ervoor dat gebruikers die de webpagina bezoeken, de miniaturen kunnen bekijken.

```azurecli-interactive
blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
-n $blobStorageAccount --query [0].value --output tsv)

az storage container create -n images --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access off

az storage container create -n thumbnails --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access container

echo "Make a note of your Blob storage account key..."
echo $blobStorageAccountKey
```

Noteer de naam en sleutel van het Blob-opslagaccount. De voorbeeld-app maakt gebruik van deze instellingen om verbinding te maken met het opslagaccount om de afbeeldingen te uploaden. 

## <a name="create-an-app-service-plan"></a>Een App Service-plan maken

Een [App Service-plan](../../app-service/overview-hosting-plans.md) geeft de locatie, de grootte en de functies van de webserverfarm aan die als host fungeert voor uw app.

Maak een App Service-plan met de opdracht [az appservice plan create](/cli/azure/appservice/plan).

In het volgende voorbeeld wordt een App Service-plan gemaakt met de naam `myAppServicePlan` en de prijscategorie **Gratis**:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku Free
```

## <a name="create-a-web-app"></a>Een webtoepassing maken

De web-app biedt een hostingruimte voor de code van de voorbeeld-app. De ruimte wordt geïmplementeerd vanuit de GitHub-voorbeeld opslagplaats. Maak een [web-app](../../app-service/overview.md) in het App Service-plan `myAppServicePlan` met de opdracht [az webapp create](/cli/azure/webapp).  

Vervang `<web_app>` in de volgende opdracht door een unieke naam. Geldige tekens zijn `a-z`, `0-9` en `-`. Als `<web_app>` niet uniek is, krijgt u het volgende foutbericht: _Er bestaat al een website met de naam `<web_app>`._ De standaard-URL van de web-app is `https://<web_app>.azurewebsites.net`.  

```azurecli-interactive
webapp=<web_app>

az webapp create --name $webapp --resource-group myResourceGroup --plan myAppServicePlan
```

## <a name="deploy-the-sample-app-from-the-github-repository"></a>Voorbeeld-app implementeren vanuit de GitHub-opslagplaats

# <a name="nettabdotnet"></a>[\.NET](#tab/dotnet)

App Service ondersteunt diverse manieren om inhoud in een web-app te implementeren. In deze zelfstudie implementeert u de web-app vanaf een [openbare GitHub-voorbeeldopslagplaats](https://github.com/Azure-Samples/storage-blob-upload-from-webapp) (Engelstalig). Configureer GitHub-implementatie naar de webtoepassing met de opdracht [az webapp deployment source config](/cli/azure/webapp/deployment/source).

Het voorbeeldproject bevat een [ASP.NET MVC](https://www.asp.net/mvc)-app. De app accepteert een afbeelding, slaat deze op in een opslagaccount en geeft afbeeldingen weer vanuit een miniaturencontainer. De web-app gebruikt de [Microsoft.WindowsAzure.Storage](/dotnet/api/overview/azure/storage), [Microsoft.WindowsAzure.Storage.Blob](/dotnet/api/microsoft.azure.storage.blob), en de Microsoft.WindowsAzure.Storage.Auth naamruimten van de Azure storage-clientbibliotheek voor interactie met Azure storage.

```azurecli-interactive
az webapp deployment source config --name $webapp \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
```

# <a name="nodejs-v2-sdktabnodejs"></a>[Node.js V2 SDK](#tab/nodejs)
App Service ondersteunt diverse manieren om inhoud in een web-app te implementeren. In deze zelfstudie implementeert u de web-app vanaf een [openbare GitHub-voorbeeldopslagplaats](https://github.com/Azure-Samples/storage-blob-upload-from-webapp-node) (Engelstalig). Configureer GitHub-implementatie naar de webtoepassing met de opdracht [az webapp deployment source config](/cli/azure/webapp/deployment/source). 

```azurecli-interactive
az webapp deployment source config --name $webapp \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp-node
```

# <a name="nodejs-v10-sdktabnodejsv10"></a>[Node.js V10 SDK](#tab/nodejsv10)
App Service ondersteunt diverse manieren om inhoud in een web-app te implementeren. In deze zelfstudie implementeert u de web-app vanaf een [openbare GitHub-voorbeeldopslagplaats](https://github.com/Azure-Samples/storage-blob-upload-from-webapp-node-v10) (Engelstalig). Configureer GitHub-implementatie naar de webtoepassing met de opdracht [az webapp deployment source config](/cli/azure/webapp/deployment/source). 

```azurecli-interactive
az webapp deployment source config --name $webapp \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp-node-v10
```

---

## <a name="configure-web-app-settings"></a>Web-app-instellingen configureren

# <a name="nettabdotnet"></a>[\.NET](#tab/dotnet)

De voorbeeldweb-app gebruikt de [Azure Storage-clientbibliotheek](/dotnet/api/overview/azure/storage?view=azure-dotnet) om toegangstokens aan te vragen. Hiermee worden afbeeldingen geüpload. De referenties van het opslagaccount die worden gebruikt door de Storage-SDK, worden ingesteld in de app-instellingen voor de web-app. Voeg app-instellingen toe aan de geïmplementeerde app met de opdracht [az webapp config appsettings set](/cli/azure/webapp/config/appsettings).

```azurecli-interactive
az webapp config appsettings set --name $webapp --resource-group myResourceGroup \
--settings AzureStorageConfig__AccountName=$blobStorageAccount \
AzureStorageConfig__ImageContainer=images  \
AzureStorageConfig__ThumbnailContainer=thumbnails \
AzureStorageConfig__AccountKey=$blobStorageAccountKey  
```

# <a name="nodejs-v2-sdktabnodejs"></a>[Node.js V2 SDK](#tab/nodejs)

De voorbeeldweb-app gebruikt de [Azure Storage-clientbibliotheek](https://docs.microsoft.com/javascript/api/azure-storage) om toegangstokens aan te vragen. Hiermee worden afbeeldingen geüpload. De referenties van het opslagaccount die worden gebruikt door de Storage-SDK, worden ingesteld in de app-instellingen voor de web-app. Voeg app-instellingen toe aan de geïmplementeerde app met de opdracht [az webapp config appsettings set](/cli/azure/webapp/config/appsettings).

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string --resource-group $resourceGroupName \
--name $blobStorageAccount --query connectionString --output tsv)

az webapp config appsettings set --name $webapp --resource-group myResourceGroup \
--settings AzureStorageConfig__ImageContainer=images  \
AzureStorageConfig__ThumbnailContainer=thumbnails \
AzureStorageConfig__AccountName=$blobStorageAccount \
AzureStorageConfig__AccountKey=$blobStorageAccountKey \
AZURE_STORAGE_CONNECTION_STRING=$storageConnectionString
```

# <a name="nodejs-v10-sdktabnodejsv10"></a>[Node.js V10 SDK](#tab/nodejsv10)

De voorbeeldweb-app gebruikt de [Azure Storage-clientbibliotheek](https://github.com/Azure/azure-storage-js) om toegangstokens aan te vragen. Hiermee worden afbeeldingen geüpload. De referenties van het opslagaccount die worden gebruikt door de Storage-SDK, worden ingesteld in de app-instellingen voor de web-app. Voeg app-instellingen toe aan de geïmplementeerde app met de opdracht [az webapp config appsettings set](/cli/azure/webapp/config/appsettings).

```azurecli-interactive
az webapp config appsettings set --name $webapp --resource-group myResourceGroup \
--settings AZURE_STORAGE_ACCOUNT_NAME=$blobStorageAccount \
AZURE_STORAGE_ACCOUNT_ACCESS_KEY=$blobStorageAccountKey
```

---

Wanneer de web-app is geïmplementeerd en geconfigureerd, kunt u de functionaliteit voor het uploaden van afbeeldingen in de app testen.

## <a name="upload-an-image"></a>Een installatiekopie uploaden

U kunt de web-app testen door naar de URL van de gepubliceerde app te gaan. De standaard-URL van de web-app is `https://<web_app>.azurewebsites.net`.

# <a name="nettabdotnet"></a>[\.NET](#tab/dotnet)

Selecteer het gebied **Upload photos** als u een bestand wilt selecteren en uploaden, of sleep een bestand naar het gebied. Als de afbeelding is geüpload, verdwijnt deze. De sectie **Gegenereerde miniaturen** blijft leeg totdat we deze verderop in dit onderwerp testen.

![App ImageResizer](media/storage-upload-process-images/figure1.png)

In de voorbeeldcode wordt de taak `UploadFiletoStorage` in bestand *Storagehelper.cs* gebruikt om de afbeeldingen door middel van de methode [UploadFromStreamAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet) te uploaden naar de container *images* in het opslagaccount. Het volgende codevoorbeeld bevat de taak `UploadFiletoStorage`.

```csharp
public static async Task<bool> UploadFileToStorage(Stream fileStream, string fileName, AzureStorageConfig _storageConfig)
{
    // Create storagecredentials object by reading the values from the configuration (appsettings.json)
    StorageCredentials storageCredentials = new StorageCredentials(_storageConfig.AccountName, _storageConfig.AccountKey);

    // Create cloudstorage account by passing the storagecredentials
    CloudStorageAccount storageAccount = new CloudStorageAccount(storageCredentials, true);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get reference to the blob container by passing the name by reading the value from the configuration (appsettings.json)
    CloudBlobContainer container = blobClient.GetContainerReference(_storageConfig.ImageContainer);

    // Get the reference to the block blob from the container
    CloudBlockBlob blockBlob = container.GetBlockBlobReference(fileName);

    // Upload the file
    await blockBlob.UploadFromStreamAsync(fileStream);

    return await Task.FromResult(true);
}
```

In de vorige taak zijn de volgende klassen en methoden gebruikt:

|Klasse  |Methode  |
|---------|---------|
|[StorageCredentials](/dotnet/api/microsoft.azure.cosmos.table.storagecredentials)     |         |
|[CloudStorageAccount](/dotnet/api/microsoft.azure.cosmos.table.cloudstorageaccount)    |  [CreateCloudBlobClient](/dotnet/api/microsoft.azure.storage.blob.blobaccountextensions.createcloudblobclient)       |
|[CloudBlobClient](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient)     |[GetContainerReference](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.getcontainerreference)         |
|[CloudBlobContainer](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer)    | [GetBlockBlobReference](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.getblockblobreference)        |
|[CloudBlockBlob](/dotnet/api/microsoft.azure.storage.blob.cloudblockblob)     | [UploadFromStreamAsync](/dotnet/api/microsoft.azure.storage.file.cloudfile.uploadfromstreamasync)        |

# <a name="nodejs-v2-sdktabnodejs"></a>[Node.js V2 SDK](#tab/nodejs)

Selecteer **Bestand kiezen** om een bestand te selecteren en klik u vervolgens op **Installatiekopie uploaden**. De sectie **Gegenereerde miniaturen** blijft leeg totdat we deze verderop in dit onderwerp testen. 

![App voor het uploaden van afbeeldingen](media/storage-upload-process-images/upload-app-nodejs.png)

In de voorbeeldcode is de `post`-route verantwoordelijk voor het uploaden van de afbeelding naar een blobcontainer. De route gebruikt de modules om de upload te verwerken:

- [multer](https://github.com/expressjs/multer) implementeert de uploadstrategie voor de routehandler.
- [in-stream](https://github.com/sindresorhus/into-stream) converteert de buffer in een stream, zoals is vereist door [createBlockBlobFromStream].(https://azure.github.io/azure-sdk-for-node/azure-storage-legacy/latest/BlobService.html)

Wanneer het bestand naar de route wordt verzonden, blijft de inhoud van het bestand in het geheugen staan totdat het bestand is geüpload naar de blobcontainer.

> [!IMPORTANT]
> Het laden van grote bestanden in het geheugen kan een negatief effect hebben op de prestaties van uw web-app. Als u verwacht dat gebruikers grote bestanden posten, kunt u overwegen om bestanden tijdelijk weg te zetten op het bestandssysteem van de webserver en vervolgens uploads naar Blob-opslag te plannen. Zodra de bestanden in Blob-opslag staan, kunt u ze verwijderen van het bestandssysteem van de server.

```javascript
const
      express = require('express')
    , router = express.Router()

    , multer = require('multer')
    , inMemoryStorage = multer.memoryStorage()
    , uploadStrategy = multer({ storage: inMemoryStorage }).single('image')

    , azureStorage = require('azure-storage')
    , blobService = azureStorage.createBlobService()

    , getStream = require('into-stream')
    , containerName = 'images'
;

const handleError = (err, res) => {
    res.status(500);
    res.render('error', { error: err });
};

const getBlobName = originalName => {
    const identifier = Math.random().toString().replace(/0\./, ''); // remove "0." from start of string
    return `${originalName}-${identifier}`;
};

router.post('/', uploadStrategy, (req, res) => {

    const
          blobName = getBlobName(req.file.originalname)
        , stream = getStream(req.file.buffer)
        , streamLength = req.file.buffer.length
    ;

    blobService.createBlockBlobFromStream(containerName, blobName, stream, streamLength, err => {

        if(err) {
            handleError(err);
            return;
        }

        res.render('success', { 
            message: 'File uploaded to Azure Blob storage.' 
        });
    });
});
```

# <a name="nodejs-v10-sdktabnodejsv10"></a>[Node.js V10 SDK](#tab/nodejsv10)

Selecteer **Bestand kiezen** om een bestand te selecteren en klik u vervolgens op **Installatiekopie uploaden**. De sectie **Gegenereerde miniaturen** blijft leeg totdat we deze verderop in dit onderwerp testen. 

![App voor het uploaden van afbeeldingen](media/storage-upload-process-images/upload-app-nodejs.png)

In de voorbeeldcode is de `post`-route verantwoordelijk voor het uploaden van de afbeelding naar een blobcontainer. De route gebruikt de modules om de upload te verwerken:

- [multer](https://github.com/expressjs/multer) implementeert de uploadstrategie voor de routehandler.
- [into-stream](https://github.com/sindresorhus/into-stream) converteert de buffer in een stream, zoals is vereist door [createBlockBlobFromStream](https://azure.github.io/azure-sdk-for-node/azure-storage-legacy/latest/BlobService.html).

Wanneer het bestand naar de route wordt verzonden, blijft de inhoud van het bestand in het geheugen staan totdat het bestand is geüpload naar de blobcontainer.

> [!IMPORTANT]
> Het laden van grote bestanden in het geheugen kan een negatief effect hebben op de prestaties van uw web-app. Als u verwacht dat gebruikers grote bestanden posten, kunt u overwegen om bestanden tijdelijk weg te zetten op het bestandssysteem van de webserver en vervolgens uploads naar Blob-opslag te plannen. Zodra de bestanden in Blob-opslag staan, kunt u ze verwijderen van het bestandssysteem van de server.

```javascript
const {
  Aborter,
  BlobURL,
  BlockBlobURL,
  ContainerURL,
  ServiceURL,
  StorageURL,
  SharedKeyCredential,
  uploadStreamToBlockBlob
} = require('@azure/storage-blob');

const express = require('express');
const router = express.Router();
const multer = require('multer');
const inMemoryStorage = multer.memoryStorage();
const uploadStrategy = multer({ storage: inMemoryStorage }).single('image');
const getStream = require('into-stream');
const containerName = 'images';
const ONE_MEGABYTE = 1024 * 1024;
const uploadOptions = { bufferSize: 4 * ONE_MEGABYTE, maxBuffers: 20 };
const ONE_MINUTE = 60 * 1000;
const aborter = Aborter.timeout(30 * ONE_MINUTE);

const sharedKeyCredential = new SharedKeyCredential(
  process.env.AZURE_STORAGE_ACCOUNT_NAME,
  process.env.AZURE_STORAGE_ACCOUNT_ACCESS_KEY);
const pipeline = StorageURL.newPipeline(sharedKeyCredential);
const serviceURL = new ServiceURL(
  `https://${process.env.AZURE_STORAGE_ACCOUNT_NAME}.blob.core.windows.net`,
  pipeline
);

const getBlobName = originalName => {
  // Use a random number to generate a unique file name, 
  // removing "0." from the start of the string.
  const identifier = Math.random().toString().replace(/0\./, ''); 
  return `${identifier}-${originalName}`;
};

router.post('/', uploadStrategy, async (req, res) => {

    const blobName = getBlobName(req.file.originalname);
    const stream = getStream(req.file.buffer);
    const containerURL = ContainerURL.fromServiceURL(serviceURL, containerName);
    const blobURL = BlobURL.fromContainerURL(containerURL, blobName);
    const blockBlobURL = BlockBlobURL.fromBlobURL(blobURL);

    try {
      
      await uploadStreamToBlockBlob(aborter, stream,
        blockBlobURL, uploadOptions.bufferSize, uploadOptions.maxBuffers);

      res.render('success', { message: 'File uploaded to Azure Blob storage.' });   

    } catch (err) {

      res.render('error', { message: 'Something went wrong.' });

    }
});
```
---

## <a name="verify-the-image-is-shown-in-the-storage-account"></a>Controleren of de afbeelding in het opslagaccount wordt weergegeven

Meld u aan bij [Azure Portal](https://portal.azure.com). In het linkermenu selecteert u **opslagaccounts** en vervolgens de naam van uw opslagaccount. Selecteer onder **Blob-service** de optie **Blobs** en selecteer de container **images**.

Controleer of de afbeelding in de container wordt weergegeven.

![Containerweergave van afbeeldingen](media/storage-upload-process-images/figure13.png)

## <a name="test-thumbnail-viewing"></a>Weergave van miniaturen testen

Als u de weergave van miniaturen wilt testen, uploadt u een afbeelding naar de container **thumbnails** om te controleren of de app de container **thumbnails** kan lezen.

Meld u aan bij [Azure Portal](https://portal.azure.com). In het linkermenu selecteert u **opslagaccounts** en vervolgens de naam van uw opslagaccount. Selecteer onder **Blob-service** de optie **Blobs**en selecteer vervolgens de container **thumbnails**. Selecteer **Uploaden** om het deelvenster **Blob uploaden** te openen.

Kies een bestand met de bestandenkiezer en selecteer **Uploaden**.

Ga terug naar de app om te controleren of de naar de **thumbnails**-container geüploade afbeelding zichtbaar is.

# <a name="nettabdotnet"></a>[\.NET](#tab/dotnet)
![Containerweergave van afbeeldingen](media/storage-upload-process-images/figure2.png)

# <a name="nodejs-v2-sdktabnodejs"></a>[Node.js V2 SDK](#tab/nodejs)
![Containerweergave van afbeeldingen](media/storage-upload-process-images/upload-app-nodejs-thumb.png)

# <a name="nodejs-v10-sdktabnodejsv10"></a>[Node.js V10 SDK](#tab/nodejsv10)
![Containerweergave van afbeeldingen](media/storage-upload-process-images/upload-app-nodejs-thumb.png)

---

In deel twee van de serie automatiseert u het maken van miniatuurafbeeldingen zodat u deze afbeelding niet nodig hebt. In de **thumbnails**-container in Azure Portal selecteert u de geüploade afbeelding en vervolgens **Verwijderen** om de afbeelding te verwijderen. 

U kunt CDN naar cache-inhoud inschakelen vanuit het Azure-opslagaccount. Voor informatie over hoe u CDN inschakelt met uw Azure-opslagaccount, gaat u naar [Een Azure-opslagaccount integreren met Azure CDN](../../cdn/cdn-create-a-storage-account-with-cdn.md).

## <a name="next-steps"></a>Volgende stappen

In deel één van de serie hebt u geleerd hoe u een web-app configureert om te communiceren met opslag.

Ga verder met deel twee van de serie om te leren hoe u Event Grid gebruikt om een Azure-functie te activeren om het formaat van een afbeelding aan te passen.

> [!div class="nextstepaction"]
> [Event Grid gebruiken om een Azure-functie te activeren bij het aanpassen van geüploade afbeeldingen](../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
