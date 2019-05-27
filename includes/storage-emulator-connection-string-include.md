---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 8c577db3e9f2bff9e86c3a7c37274630f90dd680
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66114679"
---
De opslagemulator ondersteunt één vast account en een bekende verificatiesleutel voor gedeelde sleutelverificatie. Dit account en de sleutel zijn de enige gedeelde sleutel-referenties zijn toegestaan voor gebruik met de opslagemulator. Dit zijn:

```
Account name: devstoreaccount1
Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

> [!NOTE]
> De verificatiesleutel die wordt ondersteund door de opslagemulator is alleen bedoeld voor testen van de functionaliteit van uw client verificatiecode op te geven. Deze worden niet elk doeleinde security dienen. U kunt uw productie-opslagaccount en de sleutel niet gebruiken met de opslagemulator. U moet het account ontwikkeling niet gebruiken met productiegegevens.
> 
> De opslagemulator ondersteunt alleen verbindingen via HTTP. HTTPS is echter het aanbevolen-protocol voor toegang tot resources in een productie-Azure storage-account.
> 

#### <a name="connect-to-the-emulator-account-using-a-shortcut"></a>Verbinding maken met de emulator-account met behulp van een snelkoppeling
De eenvoudigste manier om verbinding maken met de opslagemulator vanuit uw toepassing is het configureren van een verbindingsreeks in het configuratiebestand van de toepassing die verwijst naar de snelkoppeling naar de `UseDevelopmentStorage=true`. Hier volgt een voorbeeld van een verbindingsreeks voor de opslagemulator in een *app.config* bestand: 

```xml
<appSettings>
  <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
</appSettings>
```

#### <a name="connect-to-the-emulator-account-using-the-well-known-account-name-and-key"></a>Verbinding maken met de emulator-account met behulp van de bekende accountnaam en -sleutel
Voor het maken van een verbindingsreeks die verwijst naar de emulator accountnaam en sleutel, moet u de eindpunten voor elk van de services die u wilt gebruiken van de emulator in de verbindingsreeks opgeven. Dit is nodig zodat de verbindingsreeks verwijst naar de emulator-eindpunten anders dan die voor een productie-storage-account zijn. Bijvoorbeeld, er de waarde van de verbindingsreeks als volgt:

```
DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;
```

Deze waarde is gelijk aan de snelkoppeling naar de bovenstaande `UseDevelopmentStorage=true`.

#### <a name="specify-an-http-proxy"></a>Geef een HTTP-proxy
U kunt ook een HTTP-proxy moet worden gebruikt wanneer u bij het testen van uw service op basis van de opslagemulator opgeven. Dit kan nuttig zijn voor de naleving van HTTP-aanvragen en antwoorden tijdens de foutopsporing bewerkingen op de storage-services zijn. Als u een proxy, voeg de `DevelopmentStorageProxyUri` optie in als de verbindingstekenreeks en stel de waarde op de proxy-URI. Dit is bijvoorbeeld een verbindingsreeks die verwijst naar de opslagemulator en configureert u een HTTP-proxy:

```
UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
```

