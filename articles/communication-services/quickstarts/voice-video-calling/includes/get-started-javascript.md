---
title: 'Snelstart: VOIP-oproepen toevoegen aan een web-app met behulp van Azure Communication Services'
description: In deze zelfstudie leert u meer over het gebruik van de clientbibliotheek voor oproepen van Azure Communication Services voor JavaScript
author: ddematheu
ms.author: nimag
ms.date: 08/11/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: 652566efda4d4f274dc5700d35bcf45c1ebfb9e2
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/21/2020
ms.locfileid: "92347304"
---
In deze snelstart leert u hoe u de clientbibliotheek voor oproepen van Azure Communication Services voor JavaScript.

## <a name="prerequisites"></a>Vereisten

- Een Azure-account met een actief abonnement. [Gratis een account maken](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Node.js](https://nodejs.org/), Active LTS en Maintenance LTS-versies (8.11.1 en 10.14.1 aanbevolen).
- Een actieve Communication Services-resource. [Een Communication Services maken](../../create-communication-resource.md).
- Een token voor gebruikerstoegang voor het instantiëren van de client voor oproepen. Meer informatie over [tokens voor gebruikerstoegang maken en beheren](../../access-tokens.md).

## <a name="setting-up"></a>Instellen

### <a name="create-a-new-nodejs-application"></a>Een nieuwe Node.js-toepassing maken

Open uw terminal of opdrachtvenster, maak een nieuwe map voor uw app en navigeer daar naartoe.

```console
mkdir calling-quickstart && cd calling-quickstart
```

Voer `npm init -y` uit om een **package.json** -bestand te maken met de standaardinstellingen.

```console
npm init -y
```

### <a name="install-the-package"></a>Het pakket installeren

Gebruik de opdracht `npm install` voor het installeren van de clientbibliotheek voor oproepen voor Javascript in Azure Communication Services.

```console
npm install @azure/communication-common --save
npm install @azure/communication-calling --save
```

De volgende versies van webpack worden aanbevolen voor deze quickstart:

```console
"webpack": "^4.42.0",
"webpack-cli": "^3.3.11",
"webpack-dev-server": "^3.10.3"
```

De optie `--save` geeft de bibliotheek weer als afhankelijkheid in het **package.json** -bestand.

### <a name="set-up-the-app-framework"></a>Het app-framework instellen

In deze snelstart wordt webpack gebruikt om de toepassingsassets te bundelen. Voer de volgende opdracht uit om de webpack-, webpack-CLI- en webpack-dev-server npm-pakketten te installeren en deze weer te geven als ontwikkelingsafhankelijkheden in uw **package.json** :

```console
npm install webpack webpack-cli webpack-dev-server --save-dev
```

Maak een **index. html** -bestand in de hoofdmap van uw project. Dit bestand wordt gebruikt voor het configureren van een basisindeling waarmee de gebruiker een oproep naar een Azure Communications-bot kan plaatsen.

Dit is de code:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Communication Client - Calling Sample</title>
  </head>
  <body>
    <h4>Azure Communication Services</h4>
    <h1>Calling Quickstart</h1>
    <input 
      id="callee-id-input"
      type="text"
      placeholder="Who would you like to call?"
      style="margin-bottom:1em; width: 200px;"
    />
    <div>
      <button id="call-button" type="button" disabled="true">
        Start Call
      </button>
      &nbsp;
      <button id="hang-up-button" type="button" disabled="true">
        Hang Up
      </button>
    </div>
    <script src="./bundle.js"></script>
  </body>
</html>
```

Maak een bestand in de hoofdmap van uw project met de naam **client.js** om de toepassingslogica voor deze snelstart te bevatten. Voeg de volgende code toe om de oproepclient te importeren en verwijzingen naar de DOM-elementen op te halen, zodat we onze bedrijfslogica kunnen koppelen. 

```javascript
import { CallClient, CallAgent } from "@azure/communication-calling";
import { AzureCommunicationUserCredential } from '@azure/communication-common';

let call;
let callAgent;
const calleeInput = document.getElementById("callee-id-input");
const callButton = document.getElementById("call-button");
const hangUpButton = document.getElementById("hang-up-button");
```

## <a name="object-model"></a>Objectmodel

De volgende klassen en interfaces verwerken enkele van de belangrijkste functies van de Azure Communication Services-clientbibliotheek voor aanroepen:

| Naam                             | Beschrijving                                                                                                                                 |
| ---------------------------------| ------------------------------------------------------------------------------------------------------------------------------------------- |
| CallClient                       | De CallClient is het belangrijkste ingangspunt voor de clientbibliotheek voor oproepen.                                                                       |
| CallAgent                        | De CallAgent wordt gebruikt om oproepen te starten en te beheren.                                                                                            |
| AzureCommunicationUserCredential | De klasse AzureCommunicationUserCredential implementeert de CommunicationUserCredential-interface die wordt gebruikt om de CallAgent te instantiëren. |


## <a name="authenticate-the-client"></a>De client verifiëren

U moet `<USER_ACCESS_TOKEN>` vervangen door een geldig gebruikerstoegangstoken voor uw bron. Raadpleeg de documentatie inzake [Token voor gebruikerstoegang](../../access-tokens.md) als u nog geen token hebt. Met behulp van de `CallClient`initialiseert u een `CallAgent`-instantie met een `CommunicationUserCredential` waarmee we oproepen kunnen doen en ontvangen. Voeg de volgende code toe aan **client.js** :

```javascript
async function init() {
    const callClient = new CallClient();
    const tokenCredential = new AzureCommunicationUserCredential("<USER ACCESS TOKEN>");
    callAgent = await callClient.createCallAgent(tokenCredential);
    callButton.disabled = false;
}
init();
```

## <a name="start-a-call"></a>Een oproep starten

Voeg een gebeurtenis-handler toe om een oproep te initiëren wanneer op de `callButton` wordt geklikt:

```javascript
callButton.addEventListener("click", () => {
    // start a call
    const userToCall = calleeInput.value;
    call = callAgent.call(
        [{ communicationUserId: userToCall }],
        {}
    );
    // toggle button states
    hangUpButton.disabled = false;
    callButton.disabled = true;
});
```

## <a name="end-a-call"></a>Een oproep beëindigen

Voeg een gebeurtenis-listener toe om de huidige oproep te beëindigen wanneer op de `hangUpButton` wordt geklikt:

```javascript
hangUpButton.addEventListener("click", () => {
  // end the current call
  call.hangUp({ forEveryone: true });

  // toggle button states
  hangUpButton.disabled = true;
  callButton.disabled = false;
});
```

De eigenschap `forEveryone` beëindigt de oproep voor alle deelnemers aan de oproep.

## <a name="run-the-code"></a>De code uitvoeren

Gebruik de `webpack-dev-server` om uw app te bouwen en uit te voeren. Voer de volgende opdracht uit om de toepassingshost op een lokale webserver te bundelen:

```console
npx webpack-dev-server --entry ./client.js --output bundle.js --debug --devtool inline-source-map
```

Open uw browser en ga naar http://localhost:8080/. U ziet nu het volgende:

:::image type="content" source="../media/javascript/calling-javascript-app.png" alt-text="Schermopname van de voltooide JavaScript-toepassing.":::

U kunt een uitgaande VOIP-oproep maken door een gebruikers-ID op te geven in het tekstveld en te klikken op de knop **Oproep starten**. Door `8:echo123` te bellen, wordt u verbonden met een echo-bot. Dit is handig om aan de slag te gaan en te controleren of uw audio-apparaten werken.
