---
title: 'Zelfstudie: Een maximaal beschikbare toepassing bouwen met Blob-opslag - Azure Storage'
description: Geografisch redundante opslag met leestoegang gebruiken om uw toepassingsgegevens maximaal beschikbaar te maken
services: storage
author: tamram
ms.service: storage
ms.topic: tutorial
ms.date: 01/03/2019
ms.author: tamram
ms.reviewer: artek
ms.custom: mvc
ms.subservice: blobs
ms.openlocfilehash: cbf6409efa2fbb56500c6919edc6c741c4a2c45a
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306757"
---
# <a name="tutorial-build-a-highly-available-application-with-blob-storage"></a>Zelfstudie: Een maximaal beschikbare toepassing bouwen met Blob-opslag

Deze zelfstudie is deel één van een serie. In deze zelfstudie leert u hoe u uw toepassingsgegevens maximaal beschikbaar maakt in Azure.

Wanneer u deze zelfstudie hebt afgerond, hebt u een consoletoepassing die een blob uploadt en ophaalt uit een [geografisch redundant](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) opslagaccount met leestoegang (RA-GRS).

RA-GRS werkt door transacties te repliceren van een primaire regio naar een secundaire regio. Dit replicatieproces zorgt ervoor dat de gegevens in de secundaire regio uiteindelijk consistent zijn. De toepassing gebruikt het patroon [Circuit Breaker](/azure/architecture/patterns/circuit-breaker) om te bepalen met welk eindpunt er verbinding moet worden gemaakt, waarbij er automatisch wordt overgeschakeld tussen eindpunten terwijl er fouten en herstelbewerkingen worden gesimuleerd.

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

In deel 1 van de reeks leert u het volgende:

> [!div class="checklist"]
> * Create a storage account
> * De verbindingsreeks instellen
> * De consoletoepassing uitvoeren

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

* Installeer [Visual Studio 2019](https://www.visualstudio.com/downloads/) met de volgende workloads:
  - **Azure-ontwikkeling**

  ![Azure-ontwikkeling (onder Web en Cloud)](media/storage-create-geo-redundant-storage/workloads.png)

# <a name="pythontabpython"></a>[Python](#tab/python)

* [Python](https://www.python.org/downloads/) installeren
* [Azure Storage-SDK voor Python](https://github.com/Azure/azure-storage-python) downloaden en installeren

# <a name="java-v10-sdktabjava-v10"></a>[Java V10 SDK](#tab/java-v10)

* Installeer en configureer [Maven](https://maven.apache.org/download.cgi) om te werken vanuit de opdrachtregel
* Installeer en configureer een [JDK](https://www.oracle.com/technetwork/java/javase/downloads/index.html)

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)

* Installeer [Node.js](https://nodejs.org).

---

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-storage-account"></a>Create a storage account

Een opslagaccount biedt een unieke naamruimte voor het opslaan en openen van uw Azure Storage-gegevensobjecten.

Volg deze stappen om een account voor geografisch redundante opslag met leestoegang te maken:

1. Selecteer de knop **Een resource maken** in de linkerbovenhoek van Azure Portal.
2. Selecteer **Opslag** op de pagina **Nieuw**.
3. Selecteer **Storage-account: Blob, File, Table, Queue** onder **Aanbevolen**.
4. Vul het formulier voor het opslagaccount in met de informatie uit de volgende afbeelding en selecteer **Maken**:

   | Instelling       | Voorgestelde waarde | Description |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Naam** | mystorageaccount | Een unieke naam voor uw opslagaccount |
   | **Implementatiemodel** | Resource Manager  | Resource Manager bevat de nieuwste functies.|
   | **Type account** | StorageV2 | Zie [Typen opslagaccounts](../common/storage-introduction.md#types-of-storage-accounts) voor meer informatie over de verschillende typen accounts |
   | **Prestaties** | Standard | Standard is voldoende voor het voorbeeldscenario. |
   | **Replicatie**| Geografisch redundante opslag met leestoegang (RA-GRS) | Deze instelling is vereist voor het voorbeeld te laten werken. |
   |**Abonnement** | uw abonnement |Zie [Abonnementen](https://account.windowsazure.com/Subscriptions) voor meer informatie over uw abonnementen. |
   | **Replicatie**| Geografisch redundante opslag met leestoegang (RA-GRS) | Dit is nodig om het voorbeeld te laten werken. |
   |**Abonnement** | uw abonnement |Zie [Abonnementen](https://account.azure.com/Subscriptions) voor meer informatie over uw abonnementen. |
   |**ResourceGroup** | myResourceGroup |Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige namen van resourcegroepen. |
   |**Locatie** | US - oost | Kies een locatie. |

![opslagaccount maken](media/storage-create-geo-redundant-storage/createragrsstracct.png)

## <a name="download-the-sample"></a>Het voorbeeld downloaden

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

[Download het voorbeeldproject](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip) en pak (unzip) het bestand storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.zip uit. U kunt ook [git](https://git-scm.com/) gebruiken om een kopie van de toepassing te downloaden naar uw ontwikkelomgeving. Het voorbeeldproject bevat een consoletoepassing.

```bash
git clone https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.git
```

# <a name="pythontabpython"></a>[Python](#tab/python)

[Download het voorbeeldproject](https://github.com/Azure-Samples/storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip) en pak (unzip) het bestand storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs.zip uit. U kunt ook [git](https://git-scm.com/) gebruiken om een kopie van de toepassing te downloaden naar uw ontwikkelomgeving. Het voorbeeldproject bevat een basis Python-toepassing.

```bash
git clone https://github.com/Azure-Samples/storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs.git
```

# <a name="java-v10-sdktabjava-v10"></a>[Java V10 SDK](#tab/java-v10)

[Download het voorbeeldproject](https://github.com/Azure-Samples/storage-java-V10-ha-ra-grs) en pak het opslag-java-ragrs.zip-bestand uit. U kunt ook [git](https://git-scm.com/) gebruiken om een kopie van de toepassing te downloaden naar uw ontwikkelomgeving. Het voorbeeldproject bevat een eenvoudige Java-toepassing.

```bash
git clone https://github.com/Azure-Samples/storage-java-V10-ha-ra-grs
```

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)

[Download het voorbeeldproject](https://github.com/Azure-Samples/storage-node-v10-ha-ra-grs) en Decomprimeer het bestand. U kunt ook [git](https://git-scm.com/) gebruiken om een kopie van de toepassing te downloaden naar uw ontwikkelomgeving. Het voorbeeldproject bevat een eenvoudige Node.js-toepassing.

```bash
git clone https://github.com/Azure-Samples/storage-java-V10-ha-ra-grs
```

---

## <a name="configure-the-sample"></a>Configureren van het voorbeeld

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

In de toepassing moet u de verbindingsreeks voor uw opslagaccount opgeven. U kunt deze verbindingsreeks opslaan in een omgevingsvariabele op de lokale computer waarop de toepassing wordt uitgevoerd. Volg een van de onderstaande voorbeelden afhankelijk van uw besturingssysteem voor het maken van de omgevingsvariabele.

Ga in Azure Portal naar uw opslagaccount. Selecteer bij **Instellingen** in uw opslagaccount de optie **Toegangssleutels**. Kopieer de **verbindingsreeks** uit de primaire of secundaire sleutel. Voer een van de volgende opdrachten op basis van uw besturingssysteem vervangen \<yourconnectionstring\> door uw werkelijke verbindingsreeks. Met deze opdracht slaat u een omgevingsvariabele naar de lokale machine op. In Windows is de omgevingsvariabele pas beschikbaar wanneer u de **opdrachtprompt** of shell die u gebruikt opnieuw laadt.

### <a name="linux"></a>Linux

```
export storageconnectionstring=<yourconnectionstring>
```

### <a name="windows"></a>Windows

```powershell
setx storageconnectionstring "<yourconnectionstring>"
```

# <a name="pythontabpython"></a>[Python](#tab/python)

In de toepassing, moet u de referenties van uw storage-account opgeven. U kunt deze informatie opslaan in omgevingsvariabelen op de lokale computer waarop de toepassing wordt uitgevoerd. Volg een van de onderstaande voorbeelden afhankelijk van uw besturingssysteem om de omgevingsvariabelen te maken.

Ga in Azure Portal naar uw opslagaccount. Selecteer bij **Instellingen** in uw opslagaccount de optie **Toegangssleutels**. Plak de **opslagaccountnaam** en **sleutel** waarden in de volgende opdrachten, vervangen de \<youraccountname\> en \<youraccountkey\>tijdelijke aanduidingen. Met deze opdracht slaat de omgevingsvariabelen op de lokale computer. In Windows is de omgevingsvariabele pas beschikbaar wanneer u de **opdrachtprompt** of shell die u gebruikt opnieuw laadt.

### <a name="linux"></a>Linux

```
export accountname=<youraccountname>
export accountkey=<youraccountkey>
```

### <a name="windows"></a>Windows

```powershell
setx accountname "<youraccountname>"
setx accountkey "<youraccountkey>"
```

# <a name="java-v10-sdktabjava-v10"></a>[Java V10 SDK](#tab/java-v10)

Voor dit voorbeeld moet u de naam en sleutel van uw opslagaccount veilig opslaan. Bewaar ze in omgevingsvariabelen die lokaal zijn op de computer waarop het voorbeeld wordt uitgevoerd. Volg het Linux- of het Windows-voorbeeld, afhankelijk van uw besturingssysteem, voor het maken van de omgevingsvariabelen. In Windows, de omgevingsvariabele is niet beschikbaar totdat u opnieuw laden de **opdrachtprompt** of shell die u gebruikt.

### <a name="linux-example"></a>Linux-voorbeeld

```
export AZURE_STORAGE_ACCOUNT="<youraccountname>"
export AZURE_STORAGE_ACCESS_KEY="<youraccountkey>"
```

### <a name="windows-example"></a>Windows-voorbeeld

```powershell
setx AZURE_STORAGE_ACCOUNT "<youraccountname>"
setx AZURE_STORAGE_ACCESS_KEY "<youraccountkey>"
```

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)

Deze als voorbeeld wilt uitvoeren, moet u uw storage-accountreferenties voor toevoegen de `.env.example` -bestand en wijzig de naam aan `.env`.

```
AZURE_STORAGE_ACCOUNT_NAME=<replace with your storage account name>
AZURE_STORAGE_ACCOUNT_ACCESS_KEY=<replace with your storage account access key>
```

U kunt deze informatie vinden in Azure portal door te navigeren naar uw opslagaccount te selecteren **toegangssleutels** in de **instellingen** sectie.

Installeer de vereiste afhankelijkheden. U doet dit door open een opdrachtprompt, gaat u naar de Voorbeeldmap, voert u `npm install`.

---

## <a name="run-the-console-application"></a>De consoletoepassing uitvoeren

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

Druk in Visual Studio op **F5** of selecteer **Start** om foutopsporing van de toepassing te starten. Visual Studio herstelt automatisch ontbrekende NuGet-pakketten als dit zo is geconfigureerd. Ga naar [Installing and reinstalling packages with package restore](https://docs.microsoft.com/nuget/consume-packages/package-restore#package-restore-overview) (Pakketten installeren en opnieuw installeren met pakketherstel) voor meer informatie.

Er wordt een consolevenster geopend en de toepassing wordt gestart. De toepassing uploadt de afbeelding **HelloWorld.png** vanuit de oplossing naar het opslagaccount. De toepassing controleert of de afbeelding is gerepliceerd naar het secundaire RA-GRS-eindpunt. Daarna wordt de afbeelding maximaal 999 keer gedownload. Elke leesbewerking wordt voorgesteld door een **P** of een **S**. **P** staat voor het primaire eindpunt en **S** voor het secundaire eindpunt.

![Console-app die wordt uitgevoerd](media/storage-create-geo-redundant-storage/figure3.png)

In de voorbeeldcode wordt de taak `RunCircuitBreakerAsync` in het bestand `Program.cs` gebruikt om een afbeelding met behulp van de [DownloadToFileAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.downloadtofileasync)-methode te downloaden vanuit het opslagaccount. Voorafgaand aan de download is een [OperationContext](/dotnet/api/microsoft.azure.cosmos.table.operationcontext) gedefinieerd. De bewerkingscontext definieert de gebeurtenis-handlers. Deze worden geactiveerd wanneer een download is voltooid of wanneer een download is mislukt en opnieuw wordt uitgevoerd.

# <a name="pythontabpython"></a>[Python](#tab/python)

Als u de toepassing wilt uitvoeren op een terminal of opdrachtprompt, gaat u naar de map **circuitbreaker.py** en voert u `python circuitbreaker.py` in. De toepassing uploadt de afbeelding **HelloWorld.png** vanuit de oplossing naar het opslagaccount. De toepassing controleert of de afbeelding is gerepliceerd naar het secundaire RA-GRS-eindpunt. Daarna wordt de afbeelding maximaal 999 keer gedownload. Elke leesbewerking wordt voorgesteld door een **P** of een **S**. **P** staat voor het primaire eindpunt en **S** voor het secundaire eindpunt.

![Console-app die wordt uitgevoerd](media/storage-create-geo-redundant-storage/figure3.png)

In de voorbeeldcode wordt de methode `run_circuit_breaker` in het bestand `circuitbreaker.py` gebruikt om een afbeelding met behulp van de [get_blob_to_path](https://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html)-methode te downloaden vanuit het opslagaccount.

De functie Opnieuw van het opslagobject is ingesteld op een lineair beleid voor nieuwe pogingen. De functie Opnieuw bepaalt of een aanvraag opnieuw moet worden geprobeerd en geeft het aantal seconden op dat moet worden gewacht voordat opnieuw wordt geprobeerd de aanvraag uit te voeren. Stel de waarde van **retry\_to\_secondary** in op True als de aanvraag bij een volgende poging worden uitgevoerd naar het secundaire eindpunt als de eerste aanvraag naar het primaire eindpunt is mislukt. In de voorbeeldtoepassing is een aangepast beleid voor nieuwe pogingen gedefinieerd in de functie `retry_callback` van het opslagobject.

Voordat u het downloaden van het serviceobject [retry_callback](https://docs.microsoft.com/python/api/azure.storage.common.storageclient.storageclient?view=azure-python) en [response_callback](https://docs.microsoft.com/python/api/azure.storage.common.storageclient.storageclient?view=azure-python) functie is gedefinieerd. Deze functies definiëren gebeurtenis-handlers die worden geactiveerd wanneer een download is voltooid of wanneer een download is mislukt en opnieuw wordt uitgevoerd.

# <a name="java-v10-sdktabjava-v10"></a>[Java V10 SDK](#tab/java-v10)

Gebruik Maven op de opdrachtregel om het voorbeeld uit te voeren.

1. Open een shell en blader naar **storage-blobs-java-v10-quickstart** binnen de gekloonde map.
2. Voer `mvn compile exec:java` in.

In dit voorbeeld wordt een testbestand gemaakt in de standaardmap. Voor Windows-gebruikers, deze map is **AppData\Local\Temp**. Het voorbeeld biedt vervolgens een keuze uit deze opdrachten die u kunt opgeven:

- Voer **P** met deze opdracht wordt voor het uitvoeren van een bewerking put blob, een tijdelijk bestand geüpload naar uw storage-account.
- Voer **L** om uit te voeren in een lijst met blob-bewerking, met deze opdracht de blobs die momenteel in de container te vermelden.
- Voer **G** om uit te voeren een get-bewerking blob, met deze opdracht downloadt een bestand van uw storage-account naar uw lokale computer.
- Voer **D** voor het uitvoeren van een bewerking van de blob verwijderen, deze opdracht verwijdert u de blob van uw opslagaccount.
- Voer **E** het voorbeeld om af te sluiten met deze opdracht verwijdert u ook alle resources in het voorbeeld dat is gemaakt.

Dit voorbeeld toont uw uitvoer als u de toepassing op Windows uitvoert.

```
Created quickstart container
Enter a command
(P)utBlob | (L)istBlobs | (G)etBlob | (D)eleteBlobs | (E)xitSample
# Enter a command :
P
Uploading the sample file into the container: https://<storageaccount>.blob.core.windows.net/quickstart
# Enter a command :
L
Listing blobs in the container: https://<storageaccount>.blob.core.windows.net/quickstart
Blob name: SampleBlob.txt
# Enter a command :
G
Get the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
The blob was downloaded to C:\Users\<useraccount>\AppData\Local\Temp\downloadedFile13097087873115855761.txt
# Enter a command :
D
Delete the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt

# Enter a command :
>> Blob deleted: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
E
Cleaning up the sample and exiting!
```

U beheert het voorbeeld; voer dus opdrachten in om de code te laten uitvoeren. De invoer is hoofdlettergevoelig.

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)

Open een opdrachtprompt, gaat u naar de voorbeeldmap en voer de als voorbeeld wilt uitvoeren, `node index.js`.

Het voorbeeld wordt een container in uw Blob storage-account wordt gemaakt, uploadt **HelloWorld.png** naar de container vervolgens herhaaldelijk wordt gecontroleerd of de container en de installatiekopie zijn gerepliceerd naar de secundaire regio. Na de replicatie, wordt u gevraagd om in te voeren **D** of **Q** (gevolgd door ENTER) te downloaden of te sluiten. Uw uitvoer moet eruitzien zoals in het volgende voorbeeld:

```
Created container successfully: newcontainer1550799840726
Uploaded blob: HelloWorld.png
Checking to see if container and blob have replicated to secondary region.
[0] Container has not replicated to secondary region yet: newcontainer1550799840726 : ContainerNotFound
[1] Container has not replicated to secondary region yet: newcontainer1550799840726 : ContainerNotFound
...
[31] Container has not replicated to secondary region yet: newcontainer1550799840726 : ContainerNotFound
[32] Container found, but blob has not replicated to secondary region yet.
...
[67] Container found, but blob has not replicated to secondary region yet.
[68] Blob has replicated to secondary region.
Ready for blob download. Enter (D) to download or (Q) to quit, followed by ENTER.
> D
Attempting to download blob...
Blob downloaded from primary endpoint.
> Q
Exiting...
Deleted container newcontainer1550799840726
```

---

## <a name="understand-the-sample-code"></a>De voorbeeldcode begrijpen

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

### <a name="retry-event-handler"></a>Gebeurtenis-handler opnieuw proberen

De gebeurtenis-handler `OperationContextRetrying` wordt aangeroepen wanneer het downloaden van de afbeelding is mislukt en is ingesteld op Opnieuw proberen. Als het maximale aantal nieuwe pogingen dat in de toepassing is gedefinieerd, is bereikt, verandert de [LocationMode](/dotnet/api/microsoft.azure.storage.blob.blobrequestoptions.locationmode) van de aanvraag in `SecondaryOnly`. Deze instelling zorgt ervoor dat de toepassing de afbeelding probeert te downloaden van het secundaire eindpunt. Deze configuratie vermindert de tijd die nodig is om de afbeelding op te vragen omdat het primaire eindpunt niet oneindig opnieuw wordt geprobeerd.

```csharp
private static void OperationContextRetrying(object sender, RequestEventArgs e)
{
    retryCount++;
    Console.WriteLine("Retrying event because of failure reading the primary. RetryCount = " + retryCount);

    // Check if we have had more than n retries in which case switch to secondary.
    if (retryCount >= retryThreshold)
    {

        // Check to see if we can fail over to secondary.
        if (blobClient.DefaultRequestOptions.LocationMode != LocationMode.SecondaryOnly)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.SecondaryOnly;
            retryCount = 0;
        }
        else
        {
            throw new ApplicationException("Both primary and secondary are unreachable. Check your application's network connection. ");
        }
    }
}
```

### <a name="request-completed-event-handler"></a>Gebeurtenis-handler Aanvraag voltooid

De gebeurtenis-handler `OperationContextRequestCompleted` wordt aangeroepen wanneer het downloaden van de afbeelding is geslaagd. Als de toepassing het secundaire eindpunt gebruikt, blijft de toepassing dit eindpunt maximaal 20 keer gebruiken. Na 20 keer stelt de toepassing de [LocationMode](/dotnet/api/microsoft.azure.storage.blob.blobrequestoptions.locationmode) weer in op `PrimaryThenSecondary` en wordt het primaire eindpunt weer geprobeerd. Als een aanvraag is geslaagd, blijft de toepassing van het primaire eindpunt lezen.

```csharp
private static void OperationContextRequestCompleted(object sender, RequestEventArgs e)
{
    if (blobClient.DefaultRequestOptions.LocationMode == LocationMode.SecondaryOnly)
    {
        // You're reading the secondary. Let it read the secondary [secondaryThreshold] times,
        //    then switch back to the primary and see if it's available now.
        secondaryReadCount++;
        if (secondaryReadCount >= secondaryThreshold)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.PrimaryThenSecondary;
            secondaryReadCount = 0;
        }
    }
}
```

# <a name="pythontabpython"></a>[Python](#tab/python)

### <a name="retry-event-handler"></a>Gebeurtenis-handler opnieuw proberen

De gebeurtenis-handler `retry_callback` wordt aangeroepen wanneer het downloaden van de afbeelding is mislukt en is ingesteld op Opnieuw proberen. Als het maximale aantal nieuwe pogingen dat in de toepassing is gedefinieerd, is bereikt, verandert de [LocationMode](https://docs.microsoft.com/python/api/azure.storage.common.models.locationmode?view=azure-python) van de aanvraag in `SECONDARY`. Deze instelling zorgt ervoor dat de toepassing de afbeelding probeert te downloaden van het secundaire eindpunt. Deze configuratie vermindert de tijd die nodig is om de afbeelding op te vragen omdat het primaire eindpunt niet oneindig opnieuw wordt geprobeerd.

```python
def retry_callback(retry_context):
    global retry_count
    retry_count = retry_context.count
    sys.stdout.write("\nRetrying event because of failure reading the primary. RetryCount= {0}".format(retry_count))
    sys.stdout.flush()

    # Check if we have more than n-retries in which case switch to secondary
    if retry_count >= retry_threshold:

        # Check to see if we can fail over to secondary.
        if blob_client.location_mode != LocationMode.SECONDARY:
            blob_client.location_mode = LocationMode.SECONDARY
            retry_count = 0
        else:
            raise Exception("Both primary and secondary are unreachable. "
                            "Check your application's network connection.")
```

### <a name="request-completed-event-handler"></a>Gebeurtenis-handler Aanvraag voltooid

De gebeurtenis-handler `response_callback` wordt aangeroepen wanneer het downloaden van de afbeelding is geslaagd. Als de toepassing het secundaire eindpunt gebruikt, blijft de toepassing dit eindpunt maximaal 20 keer gebruiken. Na 20 keer stelt de toepassing de [LocationMode](https://docs.microsoft.com/python/api/azure.storage.common.models.locationmode?view=azure-python) weer in op `PRIMARY` en wordt het primaire eindpunt weer geprobeerd. Als een aanvraag is geslaagd, blijft de toepassing van het primaire eindpunt lezen.

```python
def response_callback(response):
    global secondary_read_count
    if blob_client.location_mode == LocationMode.SECONDARY:

        # You're reading the secondary. Let it read the secondary [secondaryThreshold] times,
        # then switch back to the primary and see if it is available now.
        secondary_read_count += 1
        if secondary_read_count >= secondary_threshold:
            blob_client.location_mode = LocationMode.PRIMARY
            secondary_read_count = 0
```

# <a name="java-v10-sdktabjava-v10"></a>[Java V10 SDK](#tab/java-v10)

Met de SDK voor Java V10, het definiëren van retouraanroep-handlers niet nodig is en de SDK heeft nu enkele fundamentele verschillen van de SDK voor Bing versie 7. In plaats van LocationMode hebben we een secundaire **Pipeline**. U kunt een secundaire pijplijn definiëren via **RequestRetryOptions**. Als dat is gebeurd, kan de toepassing automatisch overschakelen naar de secundaire pijplijn als uw gegevens niet bereikbaar zijn via de primaire pijplijn.

```java
// We create pipeline options here so that they can be easily used between different pipelines
PipelineOptions myOptions = new PipelineOptions();
myOptions.withRequestRetryOptions(new RequestRetryOptions(RetryPolicyType.EXPONENTIAL, 3, 10, 500L, 1000L, accountName + "-secondary.blob.core.windows.net"));
// We are using a default pipeline here, you can learn more about it at https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview
final ServiceURL serviceURL = new ServiceURL(new URL("https://" + accountName + ".blob.core.windows.net"), StorageURL.createPipeline(creds, myOptions));
```

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)

Met de SDK voor Node.js V10 zijn de retouraanroep-handlers niet nodig. In plaats daarvan maakt het voorbeeld u een pijplijn die is geconfigureerd met opties voor opnieuw proberen en een secundaire eindpunt. Hierdoor kan de toepassing automatisch overschakelen naar de secundaire pijplijn als deze niet kan bereiken van uw gegevens via de primaire pijplijn.

```javascript
const accountName = process.env.AZURE_STORAGE_ACCOUNT_NAME;
const storageAccessKey = process.env.AZURE_STORAGE_ACCOUNT_ACCESS_KEY;
const sharedKeyCredential = new SharedKeyCredential(accountName, storageAccessKey);

const primaryAccountURL = `https://${accountName}.blob.core.windows.net`;
const secondaryAccountURL = `https://${accountName}-secondary.blob.core.windows.net`;

const pipeline = StorageURL.newPipeline(sharedKeyCredential, {
  retryOptions: {
    maxTries: 3,
    tryTimeoutInMs: 10000,
    retryDelayInMs: 500,
    maxRetryDelayInMs: 1000,
    secondaryHost: secondaryAccountURL
  }
});
```

---

## <a name="next-steps"></a>Volgende stappen

In deel één van de serie hebt u geleerd hoe u een toepassing maximaal beschikbaar maakt met RA-GRS-opslagaccounts.

Ga naar deel twee van de serie voor informatie over hoe u een fout simuleert en uw toepassing dwingt om het secundaire RA-GRS-eindpunt te gebruiken.

> [!div class="nextstepaction"]
> [Een fout in de verbinding met het primaire eindpunt van de opslag simuleren](storage-simulate-failure-ragrs-account-app.md)
