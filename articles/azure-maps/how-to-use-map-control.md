---
title: Aan de slag met web kaartbesturingselement in Azure Maps | Microsoft Docs
description: Informatie over het gebruik van de Map Control van Azure Maps-client-side Javascript-bibliotheek.
author: walsehgal
ms.author: v-musehg
ms.date: 10/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: aa923fa7e2d5e673e6a2db2b349e54d433d1817b
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65957271"
---
# <a name="use-the-azure-maps-map-control"></a>Azure Maps Map Control gebruiken

De Map Control-client-side Javascript-bibliotheek kunt u voor rendering van kaarten en ingesloten Azure kaarten-functionaliteit in uw web- of mobiele toepassing.

## <a name="create-a-new-map-in-a-web-page"></a>Een nieuwe map maken in een webpagina

U kunt een kaart insluiten in webpagina's met behulp van de Map Control-client-side Javascript-bibliotheek.

1. Maak een nieuwe HTML-bestand.

2. In de Web-SDK van Azure kaarten worden geladen. Dit kan worden gedaan met behulp van een van twee opties.

    a. Gebruik de wereldwijd gehoste CDN-versie van Azure Maps Web SDK door de URL-eindpunten toe te voegen aan de verwijzingen stylesheet en het script in de `<head>` element van het bestand:

    ```HTML
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>
    ```

    b. U kunt ook laden van de Web-SDK van Azure kaarten-broncode lokaal via de [azure-kaarten-control](https://www.npmjs.com/package/azure-maps-control) NPM verpakt en hosten van de App met uw app. Dit pakket bevat ook TypeScript-definities.

    > npm install azure-kaarten-besturingselement

    Voeg deze verwijzingen naar de Azure-kaarten stylesheet en script bron verwijzingen naar de `<head>` element van het bestand:

    ```HTML
    <link rel="stylesheet" href="node_modules/azure-maps-control/dist/css/atlas.min.css" type="text/css">
    <script src="node_modules/azure-maps-control/dist/js/atlas.min.js"></script>
    ```

3. Voor het renderen van de kaart zodat deze de volledige hoofdtekst van de pagina vult, voeg de volgende `<style>` element op de `<head>` element.

    ```HTML
    <style>
        html, body {
            margin: 0;
        }

        #myMap {
            height: 100vh;
            width: 100vw;
        }
    </style>
    ```

4. Voeg in de hoofdtekst van de pagina, een `<div>` element en wijs hieraan een `id` van **myMap**.

    ```HTML
    <body>
        <div id="myMap"></div>
    </body>
    ```

5. Voor het initialiseren van het kaartbesturingselement, definieert een nieuwe sectie in de HTML-tekst en een script maken. Doorgeven de `id` van de kaart `<div>` of een `HTMLElement` (bijvoorbeeld `document.getElementById('myMap')`) als de eerste parameter bij het maken van een exemplaar van de `Map` klasse. Uw eigen sleutel voor Azure kaarten-account of Azure Active Directory (AAD)-referenties gebruiken om te verifiëren van de kaart met behulp [verificatieopties](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.authenticationoptions). Als u nodig hebt voor het maken van een account of het vinden van uw sleutel, Zie [over het beheren van uw Azure kaarten-account en sleutels](how-to-manage-account-keys.md). De **taal** optie geeft u de taal die moet worden gebruikt voor Kaartlabels en besturingselementen. Zie voor meer informatie over ondersteunde talen [ondersteunde talen](supported-languages.md). Als een abonnementssleutel voor verificatie.

    ```HTML
    <script type="text/javascript">
        var map = new atlas.Map('myMap', {
            center: [-122.33, 47.6],
            zoom: 12,
            language: 'en-US',
            authOptions: {
                authType: 'subscriptionKey',
                subscriptionKey: '<Your Azure Maps Key>'
            }
        });
    </script>
    ```

    Als u met behulp van Azure Active Directory (AAD) voor verificatie:

    ```HTML
    <script type="text/javascript">
        var map = new atlas.Map('myMap', {
            center: [-122.33, 47.6],
            zoom: 12,
            language: 'en-US',
            authOptions: {
                authType: 'aad',
                clientId: '<Your AAD Client Id>',
                aadAppId: '<Your AAD App Id>',
                aadTenant: 'msft.ccsctp.net'
            }
        });
    </script>
    ```

    Zie voor meer informatie, [verificatie met Azure Maps](azure-maps-authentication.md) voor meer informatie.

6. (Optioneel) wellicht vindt u de volgende elementen van de meta-tag toe te voegen aan de kop van de pagina die handig zijn:

    ```HTML
    <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
    <meta http-equiv="x-ua-compatible" content="IE=Edge">

    <!-- Ensures the web page looks good on all screen sizes. -->
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    ```

7. Dit alles ziet uw HTML-bestand er ongeveer als de volgende code:

    ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title></title>

        <meta charset="utf-8">

        <!-- Ensures that IE and Edge uses the latest version and doesn't emulate an older version -->
        <meta http-equiv="x-ua-compatible" content="IE=Edge">

        <!-- Ensures the web page looks good on all screen sizes. -->
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

        <style>
            html, body {
                margin: 0;
            }

            #myMap {
                height: 100vh;
                width: 100vw;
            }
        </style>
    </head>
    <body>
        <div id="myMap"></div>

        <script type="text/javascript">
            //Create an instance of the map control and set some options.
            var map = new atlas.Map('myMap', {
                center: [-122.33, 47.6],
                zoom: 12,
                language: 'en-US',
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });
        </script>
    </body>
    </html>
    ```

8. Open het bestand in uw webbrowser en de gerenderde kaart weergeven. Deze moet de volgende code uitzien:

    <iframe height="700" style="width: 100%;" scrolling="no" title="Map control gebruiken" src="//codepen.io/azuremaps/embed/yZpEYL/?height=557&theme-id=0&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true">Zie de Pen <a href='https://codepen.io/azuremaps/pen/yZpEYL/'>over het gebruik van het kaartbesturingselement</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
    </iframe>

## <a name="next-steps"></a>Volgende stappen

Informatie over het maken van en interactie met een kaart:

> [!div class="nextstepaction"]
> [Een kaart maken](map-create.md)

Meer informatie over voor de stijl van een kaart:

> [!div class="nextstepaction"]
> [Kies een stijl kaart](choose-map-style.md)
