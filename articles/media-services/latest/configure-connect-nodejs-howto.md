---
title: Verbinding maken met Azure Media Services v3 API - Node.js
description: Leer hoe u verbinding maken met Media Services v3 API met behulp van Node.js.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: juliako
ms.openlocfilehash: 40880a2c28ce28a671930ef8837082247e61e24b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60733127"
---
# <a name="connect-to-media-services-v3-api---nodejs"></a>Verbinding maken met de API van Media Services v3 - Node.js

In dit artikel wordt beschreven hoe u verbinding maken met de Azure Media Services v3 node.js SDK met behulp van de service principal-aanmelding in de methode.

## <a name="prerequisites"></a>Vereisten

- Installeer [Node.js](https://nodejs.org/en/download/).
- [Een Azure Media Services-account maken](create-account-cli-how-to.md). Moet u om te onthouden naam van de resourcegroep en de naam van de Media Services-account.

## <a name="create-packagejson"></a>Package.json maken

1. Maak een package.json-bestand met behulp van uw favoriete editor.
1. Open het bestand en plak de volgende code:

```json
{
  "name": "media-services-node-sample",
  "version": "0.1.0",
  "description": "",
  "main": "./index.js",
  "dependencies": {
    "azure-arm-mediaservices": "^4.1.0",
    "azure-storage": "^2.8.0",
    "ms-rest": "^2.3.3",
    "ms-rest-azure": "^2.5.5"
  }
}
```

De volgende pakketten moeten worden opgegeven:

|Pakket|Description|
|---|---|
|`azure-arm-mediaservices`|Azure Media Services SDK. <br/>Controleer of u de meest recente pakket met Azure Media Services, [NPM install azure-arm-mediaservices](https://www.npmjs.com/package/azure-arm-mediaservices/).|
|`azure-storage`|Storage-SDK. Gebruikt bij het uploaden van bestanden naar de activa.|
|`ms-rest-azure`| Gebruikt voor aanmelding bij.|

U kunt de volgende opdracht uit om ervoor te zorgen dat u met behulp van de meest recente pakket uitvoeren:

```
npm install azure-arm-mediaservices
```

## <a name="connect-to-nodejs-client"></a>Verbinding maken met Node.js-client

1. Maak een .js-bestand met behulp van uw favoriete editor.
1. Open het bestand en plak de volgende code.
1. Stel de waarden in de sectie 'endpoint config' met waarden u hebt verkregen via [toegang tot API's](access-api-cli-how-to.md).

```js
'use strict';

const MediaServices = require('azure-arm-mediaservices');
const msRestAzure = require('ms-rest-azure');
const msRest = require('ms-rest');
const azureStorage = require('azure-storage');

// endpoint config
// make sure your URL values end with '/'
const armAadAudience = "";
const aadEndpoint = "";
const armEndpoint = "";
const subscriptionId = "";
const accountName = "";
const region = "";
const aadClientId = "";
const aadSecret = "";
const aadTenantId = "";
const resourceGroup = "";

let azureMediaServicesClient;

///////////////////////////////////////////
//     Entrypoint for sample script      //
///////////////////////////////////////////

msRestAzure.loginWithServicePrincipalSecret(aadClientId, aadSecret, aadTenantId, {
  environment: {
    activeDirectoryResourceId: armAadAudience,
    resourceManagerEndpointUrl: armEndpoint,
    activeDirectoryEndpointUrl: aadEndpoint
  }
}, async function(err, credentials, subscriptions) {
    if (err) return console.log(err);
    azureMediaServicesClient = new MediaServices(credentials, subscriptionId, armEndpoint, { noRetryPolicy: true });
    
    console.log("connected");

});
```

## <a name="run-your-app"></a>Uw app uitvoeren

Open een opdrachtprompt. Blader naar de directory van het voorbeeld en voer de volgende opdrachten uit:

```
npm install 
node index.js
```

## <a name="see-also"></a>Zie ook

- [Media Services-concepten](concepts-overview.md)
- [NPM install azure-arm-mediaservices](https://www.npmjs.com/package/azure-arm-mediaservices/)

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [Node.js-naslagdocumentatie](https://aka.ms/ams-v3-nodejs-ref) van Media Services en bekijk [voorbeelden](https://github.com/Azure-Samples/media-services-v3-node-tutorials) waarin u ziet hoe u Media Services-API gebruikt met Node.js.

