---
title: Azure Storage-voorbeelden met behulp van Java | Microsoft Docs
description: Weergeven, downloaden en uitvoeren van toepassingen voor Azure Storage en voorbeeldcode. Ontdek aan de slag-voorbeelden voor blobs, wachtrijen, tabellen en bestanden, met behulp van de opslagclientbibliotheken voor Java.
services: storage
author: mhopkins-msft
ms.service: storage
ms.devlang: java
ms.topic: article
ms.date: 05/03/2019
ms.author: mhopkins
ms.subservice: common
ms.openlocfilehash: 3d241f1905244d3a8039372262f84ba0fd25220d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65209775"
---
# <a name="azure-storage-samples-using-java"></a>Azure Storage-voorbeelden met behulp van Java

## <a name="java-sample-index"></a>Index van de Java-voorbeeld

De volgende tabel bevat een overzicht van onze opslagplaats met voorbeelden en de scenario's in elk voorbeeld behandeld. Klik op de koppelingen om weer te geven van de bijbehorende voorbeeldcode in GitHub.

<table style="font-size:90%"><thead><tr><th style="font-size:110%">Eindpunt</th><th style="font-size:110%">Scenario</th><th style="font-size:110%">Voorbeeldcode</th></tr></thead><tbody>
<tr>
<td rowspan="16"><b>Blob</b></td>
<td>Toevoeg-Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr>
<tr>
<td>Blok-Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr>
<tr>
<td>Clientversleuteling</td>
<td><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Aan de slag met Azure-Client Side Encryption in Java</a></td>
</tr>
<tr>
<td>Blob kopiëren</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr>
<tr>
<td>Container maken</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr>
<tr>
<td>Blob verwijderen</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr>
<tr>
<td>Container verwijderen</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr>
<tr>
<td>BLOB-metagegevens/eigenschappen/Stats</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr>
<tr>
<td>ACL/metagegevens/eigenschappen van de container</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr>
<tr>
<td>Get Page-bereiken</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java#L399">Aan de slag met Azure Blob-Service in Java</a></td>
</tr>
<tr>
<td>Lease-Blob/Container</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr>
<tr>
<td>Blob/Container vermelden</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr>
<tr>
<td>Pagina-blobs</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr>
<tr>
<td>SAS</td>
<td><a href="https://github.com/Azure/azure-storage-java/blob/89540f018f1160ce55619c6fe7b5f5ff57d0ce10/src/test/java/com/microsoft/azure/storage/Samples.java#L513">Voorbeeld van de SAS-Tests</a></td>
</tr>   
<tr>
<td>Service-eigenschappen</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr>
<tr>
<td>Momentopname maken van Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Aan de slag met Azure Blob-Service in Java</a></td>
</tr>
<tr>
<td rowspan="9"><b>File</b></td>
<td>Shares/mappen/bestanden maken</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Aan de slag met Azure File-Service in Java</a></td>
</tr>
<tr>
<td>Shares/mappen/bestanden verwijderen</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Aan de slag met Azure File-Service in Java</a></td>
</tr>
<tr>
<td>Directory eigenschappen/metagegevens</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Aan de slag met Azure File-Service in Java</a></td>
</tr>
<tr>
<td>Bestanden downloaden</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Aan de slag met Azure File-Service in Java</a></td>
</tr>
<tr>
<td>Bestand eigenschappen/metagegevens/metrische gegevens</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Aan de slag met Azure File-Service in Java</a></td>
</tr>
<tr>
<td>File Service Properties</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Aan de slag met Azure File-Service in Java</a></td>
</tr>
<tr>
<td>Lijst met mappen en bestanden</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Aan de slag met Azure File-Service in Java</a></td>
</tr>
<tr>
<td>Lijst met Shares</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Aan de slag met Azure File-Service in Java</a></td>
</tr>
<tr>
<td>Delen van eigenschappen/metagegevens/Stats</td>
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Aan de slag met Azure File-Service in Java</a></td>
</tr>
<tr>
<td rowspan="8"><b>wachtrij</b></td>
<td>Bericht toevoegen</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java#L63">Aan de slag met Azure Queue-Service in Java</a></td>
</tr>
<tr>
<td>Clientversleuteling</td>
<td><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption/blob/master/src/gettingstarted/KeyVaultGettingStarted.java">Aan de slag met Azure-Client Side Encryption in Java</a></td>
</tr>
<tr>
<td>Wachtrijen maken</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Aan de slag met Azure Queue-Service in Java</a></td>
</tr>
<tr>
<td>Bericht/wachtrij verwijderen</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Aan de slag met Azure Queue-Service in Java</a></td>
</tr>
<tr>
<td>Bericht bekijken</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Aan de slag met Azure Queue-Service in Java</a></td>
</tr>
<tr>
<td>Wachtrij ACL/metagegevens/Stats</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Aan de slag met Azure Queue-Service in Java</a></td>
</tr>
<tr>
<td>Eigenschappen van de Queue-Service</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Aan de slag met Azure Queue-Service in Java</a></td>
</tr>
<tr>
<td>Bericht bijwerken</td>
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Aan de slag met Azure Queue-Service in Java</a></td>
</tr>
<tr>
<td rowspan="7"><b>Tabel</b></td>
<td>Tabel maken</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Aan de slag met de Azure-tabelservice in Java</a></td>
</tr>
<tr>
<td>Entiteit/tabel verwijderen</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Aan de slag met de Azure-tabelservice in Java</a></td>
</tr>
<tr>
<td>De entiteit invoegen/Merge/vervangen</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Aan de slag met de Azure-tabelservice in Java</a></td>
</tr>
<tr>
<td>Entiteiten opvragen</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Aan de slag met de Azure-tabelservice in Java</a></td>
</tr>
<tr>
<td>Querytabellen</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Aan de slag met de Azure-tabelservice in Java</a></td>
</tr>
<tr>
<td>Tabeleigenschappen ACL</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableAdvanced.java">Aan de slag met de Azure-tabelservice in Java</a></td>
</tr>
<tr>
<td>Een entiteit bijgewerkt</td>
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/main/java/com/microsoft/azure/cosmosdb/tablesample/TableBasics.java">Aan de slag met de Azure-tabelservice in Java</a></td>
</tr>
</tbody>
</table>
<br/>

## <a name="azure-code-samples-library"></a>Azure-codevoorbeelden-bibliotheek

Als u het volledige voorbeeld van een bibliotheek, gaat u naar de [Azure-codevoorbeelden](https://azure.microsoft.com/resources/samples/?service=storage) bibliotheek, waaronder voorbeelden voor Azure-opslag die u kunt downloaden en lokaal uitvoeren. De bibliotheek met voorbeeld wordt een voorbeeldcode in .zip-indeling. U kunt ook bladeren en kloon de GitHub-opslagplaats voor elk voorbeeld.

[!INCLUDE [storage-java-samples-include](../../../includes/storage-java-samples-include.md)]

## <a name="getting-started-guides"></a>Introductiehandleidingen

Bekijk de volgende handleidingen in als u op zoek bent voor instructies over het installeren en aan de slag met de Azure Storage-clientbibliotheken.

* [Aan de slag met Azure Blob-Service in Java](../blobs/storage-quickstart-blobs-java.md)
* [Aan de slag met Azure Queue-Service in Java](../queues/storage-java-how-to-use-queue-storage.md)
* [Aan de slag met de Azure-tabelservice in Java](../../cosmos-db/table-storage-how-to-use-java.md)
* [Aan de slag met Azure File-Service in Java](../files/storage-java-how-to-use-file-storage.md)

## <a name="next-steps"></a>Volgende stappen

Voor informatie over voorbeelden voor andere talen:

* .NET: [Azure Storage-voorbeelden met behulp van .NET](storage-samples-dotnet.md)
* Alle andere talen: [Azure Storage-voorbeelden](storage-samples.md)
