---
title: 'Java SDK: Bestandssysteembewerkingen in Azure Data Lake Storage Gen1 | Microsoft Docs'
description: Gebruik Azure Data Lake Storage Gen1 Java SDK voor het uitvoeren van bestandssysteembewerkingen in Data Lake Storage Gen1 zoals om mappen te maken enzovoort.
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: d10e09db-5232-4e84-bb50-52efc2c21887
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: bc6e0718cdc4ccb18480dc760279da9c177db4cb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60877461"
---
# <a name="filesystem-operations-on-azure-data-lake-storage-gen1-using-java-sdk"></a>Bestandssysteembewerkingen in Azure Data Lake Storage Gen1 met behulp van Java-SDK
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java-SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
> 

Informatie over het gebruik van de Azure Data Lake Storage Gen1 Java SDK basisbewerkingen, zoals het maken van mappen, uploaden en downloaden van gegevensbestanden enzovoort uitvoert. Zie voor meer informatie over Data Lake Storage Gen1 [Azure Data Lake Storage Gen1](data-lake-store-overview.md).

U hebt toegang tot de Java SDK API-documenten voor Data Lake Storage Gen1 op [Azure Data Lake Storage Gen1 Java API-documenten](https://azure.github.io/azure-data-lake-store-java/javadoc/).

## <a name="prerequisites"></a>Vereisten
* Java Development Kit (JDK 7 of hoger met Java versie 1.7 of hoger)
* Data Lake Storage Gen1-account. Volg de instructies op [aan de slag met Azure Data Lake Storage Gen1 met behulp van de Azure-portal](data-lake-store-get-started-portal.md).
* [Maven](https://maven.apache.org/install.html). In deze zelfstudie wordt Maven gebruikt voor build- en projectafhankelijkheden. Hoewel het mogelijk is te ontwikkelen zonder een buildsysteem als Maven of Gradle, maken deze systemen het veel eenvoudiger om afhankelijkheden te beheren.
* (Optioneel) Een IDE zoals [IntelliJ IDEA](https://www.jetbrains.com/idea/download/), [Eclipse](https://www.eclipse.org/downloads/) of vergelijkbaar.

## <a name="create-a-java-application"></a>Een Java-toepassing maken
Dit codevoorbeeld beschikbaar [in GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) doorloopt het proces waarin bestanden in het archief worden gemaakt, bestanden worden samengevoegd, een bestand wordt gedownload en een aantal bestanden uit het archief wordt verwijderd. In dit gedeelte van het artikel komen de belangrijkste onderdelen van de code aan bod.

1. Maak een Maven-project met behulp van [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) vanaf de opdrachtregel of met behulp van een IDE. [Hier](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html) vindt u instructies over het maken van een Java-project met behulp van IntelliJ. [Hier](https://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm) vindt u instructies over het maken van een Java-project met behulp van Eclipse. 

2. Voeg de volgende afhankelijkheden toe aan het **pom.xml**-bestand in Maven. Voeg het volgende codefragment toe vóór de tag **\</project>** :
   
        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.1.5</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>
   
    De eerste afhankelijkheid is het gebruik van de Data Lake Storage-SDK met Gen1 (`azure-data-lake-store-sdk`) vanuit de maven-opslagplaats. De tweede afhankelijkheid dient om op te geven welk framework voor logboekregistratie (`slf4j-nop`) voor deze toepassing moet worden gebruikt. Maakt gebruik van de Data Lake Storage-SDK met Gen1 [slf4j](https://www.slf4j.org/) logboekregistratie gevel, kunt u kiezen uit een aantal populaire logboekregistratie frameworks als log4j, logback, enz., logboekregistratie Java of niet vastleggen. In dit voorbeeld wordt logboekregistratie uitgeschakeld. Daarom wordt de binding **slf4j-nop** gebruikt. [Hier](https://www.slf4j.org/manual.html#projectDep) vindt u andere opties voor logboekregistratie voor uw toepassing.

3. Voeg de volgende importinstructies toe aan uw toepassing.

        import com.microsoft.azure.datalake.store.ADLException;
        import com.microsoft.azure.datalake.store.ADLStoreClient;
        import com.microsoft.azure.datalake.store.DirectoryEntry;
        import com.microsoft.azure.datalake.store.IfExists;
        import com.microsoft.azure.datalake.store.oauth2.AccessTokenProvider;
        import com.microsoft.azure.datalake.store.oauth2.ClientCredsTokenProvider;

        import java.io.*;
        import java.util.Arrays;
        import java.util.List;

## <a name="authentication"></a>Verificatie

* Zie voor verificatie van eindgebruikers voor uw toepassing, [eindgebruiker End-User-authentication met Data Lake Storage Gen1 met behulp van Java](data-lake-store-end-user-authenticate-java-sdk.md).
* Zie voor service-naar-serviceverificatie voor uw toepassing, [Service-naar-serviceverificatie met Data Lake Storage Gen1 met behulp van Java](data-lake-store-service-to-service-authenticate-java.md).

## <a name="create-a-data-lake-storage-gen1-client"></a>Maak een Data Lake Storage Gen1-client
Het maken van een [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) object moet u de naam van het Data Lake Storage Gen1 en de tokenprovider die zijn gegenereerd tijdens uw verificatie bij Data Lake Storage Gen1 opgeven (Zie [verificatie](#authentication) sectie). De naam van het Data Lake Storage Gen1 moet een volledig gekwalificeerde domeinnaam. Vervang bijvoorbeeld **Fill-in-HERE** door iets zoals **mydatalakestoragegen1.azuredatalakestore.net**.

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just the account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);

De codefragmenten in de volgende secties bevatten voorbeelden van enkele veelvoorkomende bestandssysteembewerkingen. U kunt de volledige kijken [Data Lake Storage Gen1 Java SDK API-documenten](https://azure.github.io/azure-data-lake-store-java/javadoc/) van de **ADLStoreClient** object om te zien van andere bewerkingen.

## <a name="create-a-directory"></a>Een map maken

Het volgende codefragment wordt een mapstructuur gemaakt in de hoofdmap van het Data Lake Storage Gen1-account dat u hebt opgegeven.

    // create directory
    client.createDirectory("/a/b/w");
    System.out.println("Directory created.");

## <a name="create-a-file"></a>Een bestand maken

In het volgende codefragment wordt een bestand (c.txt) in de mapstructuur gemaakt en worden er gegevens naar het bestand geschreven.

    // create file and write some content
    String filename = "/a/b/c.txt";
    OutputStream stream = client.createFile(filename, IfExists.OVERWRITE  );
    PrintStream out = new PrintStream(stream);
    for (int i = 1; i <= 10; i++) {
        out.println("This is line #" + i);
        out.format("This is the same line (%d), but using formatted output. %n", i);
    }
    out.close();
    System.out.println("File created.");

U kunt ook een bestand (d.txt) maken met behulp van bytematrices.

    // create file using byte arrays
    stream = client.createFile("/a/b/d.txt", IfExists.OVERWRITE);
    byte[] buf = getSampleContent();
    stream.write(buf);
    stream.close();
    System.out.println("File created using byte array.");

De definitie van functie `getSampleContent` die in het bovenstaande codefragment wordt gebruikt, is beschikbaar als onderdeel van het voorbeeld [op GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/). 

## <a name="append-to-a-file"></a>Toevoegen aan een bestand

In het volgende codefragment wordt inhoud toegevoegd aan een bestaand bestand.

    // append to file
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();
    System.out.println("File appended.");

De definitie van functie `getSampleContent` die in het bovenstaande codefragment wordt gebruikt, is beschikbaar als onderdeel van het voorbeeld [op GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/).

## <a name="read-a-file"></a>Een bestand lezen

Het volgende codefragment wordt inhoud gelezen uit een bestand in een Data Lake Storage Gen1-account.

    // Read File
    InputStream in = client.getReadStream(filename);
    BufferedReader reader = new BufferedReader(new InputStreamReader(in));
    String line;
    while ( (line = reader.readLine()) != null) {
        System.out.println(line);
    }
    reader.close();
    System.out.println();
    System.out.println("File contents read.");

## <a name="concatenate-files"></a>Bestanden samenvoegen

Het volgende codefragment worden samengevoegd in twee bestanden in een Data Lake Storage Gen1-account. Als dit lukt, worden de twee bestaande bestanden vervangen door het samengevoegde bestand.

    // concatenate the two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);
    System.out.println("Two files concatenated into a new file.");

## <a name="rename-a-file"></a>De naam van een bestand wijzigen

Het volgende codefragment wijzigt de naam van een bestand in een Data Lake Storage Gen1-account.

    //rename the file
    client.rename("/a/b/f.txt", "/a/b/g.txt");
    System.out.println("New file renamed.");

## <a name="get-metadata-for-a-file"></a>Metagegevens ophalen voor een bestand

Het volgende codefragment haalt de metagegevens voor een bestand in een Data Lake Storage Gen1-account.

    // get file metadata
    DirectoryEntry ent = client.getDirectoryEntry(filename);
    printDirectoryInfo(ent);
    System.out.println("File metadata retrieved.");

## <a name="set-permissions-on-a-file"></a>Machtigingen instellen voor een bestand

In het volgende codefragment worden machtigingen ingesteld voor het bestand dat u in de vorige sectie hebt gemaakt.

    // set file permission
    client.setPermission(filename, "744");
    System.out.println("File permission set.");

## <a name="list-directory-contents"></a>Mapinhoud weergeven

In het volgende codefragment wordt recursief de inhoud van een map weergegeven.

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }
    System.out.println("Directory contents listed.");

De definitie van functie `printDirectoryInfo` die in het bovenstaande codefragment wordt gebruikt, is beschikbaar als onderdeel van het voorbeeld [op GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/).

## <a name="delete-files-and-folders"></a>Bestanden en mappen verwijderen

Het volgende codefragment wordt de opgegeven bestanden en mappen in een Data Lake Storage Gen1-account, recursief verwijderd.

    // delete directory along with all the subdirectories and files in it
    client.deleteRecursive("/a");
    System.out.println("All files and folders deleted recursively");
    promptEnterKey();

## <a name="build-and-run-the-application"></a>De toepassing bouwen en uitvoeren.
1. Klik op de knop **Uitvoeren** om de toepassing vanuit een IDE uit te voeren. Gebruik [exec:exec](https://www.mojohaus.org/exec-maven-plugin/exec-mojo.html) om deze vanuit Maven uit te voeren.
2. Als u een afzonderlijke jar wilt maken die u vanuit de opdrachtregel kunt uitvoeren, bouwt u de jar met alle afhankelijkheden geïntegreerd met behulp van de [Maven assembly-invoegtoepassing](https://maven.apache.org/plugins/maven-assembly-plugin/usage.html). De pom.xml in de [voorbeeldbroncode van GitHub](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) bevat een voorbeeld.

## <a name="next-steps"></a>Volgende stappen
* [JavaDoc verkennen voor de Java-SDK](https://azure.github.io/azure-data-lake-store-java/javadoc/)
* [Gegevens beveiligen in Data Lake Storage Gen1](data-lake-store-secure-data.md)


