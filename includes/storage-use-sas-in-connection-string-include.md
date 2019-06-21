---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 2f27c50b1d016265c20102521a137bcbb0646115
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176253"
---
Als u een shared access signature (SAS)-URL die u toegang tot bronnen in een storage-account hebt hebben, kunt u de SAS in een verbindingsreeks. Omdat de SAS de vereiste informatie bevat op de aanvraag worden geverifieerd, wordt een verbindingsreeks met een SAS biedt het protocol, de service-eindpunt en de vereiste referenties voor toegang tot de resource.

Voor het maken van een verbindingsreeks met een shared access signature, geef de tekenreeks in de volgende indeling:

```
BlobEndpoint=myBlobEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
FileEndpoint=myFileEndpoint;
SharedAccessSignature=sasToken
```

Alle service-eindpunten is optioneel, maar de verbindingsreeks moet ten minste één bevatten.

> [!NOTE]
> Gebruik van HTTPS met een SAS wordt aanbevolen als een best practice.
>
> Als u een SAS in een verbindingsreeks in een configuratiebestand opgeeft, moet u mogelijk codeert speciale tekens in de URL.
>
>

### <a name="service-sas-example"></a>Voorbeeld van de service-SAS
Hier volgt een voorbeeld van een verbindingsreeks met een service-SAS voor Blob-opslag:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
SharedAccessSignature=sv=2015-04-05&sr=b&si=tutorial-policy-635959936145100803&sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D
```

En Hier volgt een voorbeeld van de verbindingsreeks met codering van speciale tekens bevatten:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
SharedAccessSignature=sv=2015-04-05&amp;sr=b&amp;si=tutorial-policy-635959936145100803&amp;sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D
```

### <a name="account-sas-example"></a>Voorbeeld van de account-SAS
Hier volgt een voorbeeld van een verbindingsreeks met een account-SAS voor Blob-en-bestand. Houd er rekening mee dat de eindpunten voor beide services worden opgegeven:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
FileEndpoint=https://storagesample.file.core.windows.net;
SharedAccessSignature=sv=2015-07-08&sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&spr=https&st=2016-04-12T03%3A24%3A31Z&se=2016-04-13T03%3A29%3A31Z&srt=s&ss=bf&sp=rwl
```

En Hier volgt een voorbeeld van de verbindingsreeks met het URL-codering:

```
BlobEndpoint=https://storagesample.blob.core.windows.net;
FileEndpoint=https://storagesample.file.core.windows.net;
SharedAccessSignature=sv=2015-07-08&amp;sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&amp;spr=https&amp;st=2016-04-12T03%3A24%3A31Z&amp;se=2016-04-13T03%3A29%3A31Z&amp;srt=s&amp;ss=bf&amp;sp=rwl
```

