---
title: Toevoegen en uitvoeren van code in Azure Logic Apps met Azure Functions
description: Toevoegen en uitvoeren van code in Azure Logic Apps met Azure Functions
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.topic: article
ms.date: 08/20/2018
ms.reviewer: klam, LADocs
ms.openlocfilehash: 9b304f2d4d2e498701be5977decf202cb0fa995b
ms.sourcegitcommit: d73c46af1465c7fd879b5a97ddc45c38ec3f5c0d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65922065"
---
# <a name="add-and-run-code-by-using-azure-functions-in-azure-logic-apps"></a>Toevoegen en code uitvoeren met behulp van Azure Functions in Azure Logic Apps

Als u uitvoeren van code dat een specifieke taak in uw logische apps wilt uitvoert, kunt u uw eigen functies met [Azure Functions](../azure-functions/functions-overview.md). Deze service helpt u bij het maken van Node.js, C#, en F# code, zodat u hoeft te maken van een volledige app of de infrastructuur voor het uitvoeren van uw code. U kunt ook [logische apps aanroepen in Azure functions](#call-logic-app).
Azure Functions biedt serverloze computing in de cloud en is handig voor het uitvoeren van taken, zoals deze voorbeelden:

* Breid uw logische app gedrag met Node.js of C#-functies.
* Uitvoeren van berekeningen in uw werkstroom voor logische Apps.
* Geavanceerde opmaak toepassen of compute-velden in uw logische apps.

Als u wilt codefragmenten uitvoeren zonder te maken van Azure functions, informatie over hoe u [toevoegen en uitvoeren van Inlinecode](../logic-apps/logic-apps-add-run-inline-code.md).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, [registreer u dan nu voor een gratis Azure-account](https://azure.microsoft.com/free/).

* Een Azure-functie-app, dat een container voor Azure functions en uw Azure-functie is. Als u een functie-app geen [maakt u eerst uw functie-app](../azure-functions/functions-create-first-azure-function.md). U kunt de functie vervolgens ofwel maken [afzonderlijk buiten uw logische app](#create-function-external), of [van binnen uw logische app](#create-function-designer) in Logic App Designer.

  Bestaande en nieuwe functie-apps en functies hebben dezelfde vereisten voor het werken met logic apps:

  * Uw functie-app moet hetzelfde Azure-abonnement als uw logische app hebben.

  * Gebruikt de functie een HTTP-trigger, bijvoorbeeld, de **HTTP-trigger** functiesjabloon voor **JavaScript** of **C#**. 

    De HTTP-trigger-sjabloon kunt accepteren inhoud waarvoor `application/json` type van uw logische app. 
    Wanneer u een Azure-functie aan uw logische app toevoegt, toont de ontwerper van logische App aangepaste functies die zijn gemaakt met deze sjabloon binnen uw Azure-abonnement. 

  * De functie aangepaste routes niet gebruiken tenzij u hebt gedefinieerd een [OpenAPI-definitie](../azure-functions/functions-openapi-definition.md), voorheen bekend als een [Swagger-bestand](https://swagger.io/). 
  
  * Als u een OpenAPI-definitie voor de functie hebt gedefinieerd, kunt u in de ontwerper van logische Apps een rijkere ervaring voor het werken met de parameters van de functie. Voordat uw logische app kunt vinden en toegang tot functies met OpenAPI-definities [instellen van uw functie-app door de volgende stappen](#function-swagger).

* De logische app waaraan u wilt toevoegen van de functie, met inbegrip van een [trigger](../logic-apps/logic-apps-overview.md#logic-app-concepts) als de eerste stap in uw logische app 

  Voordat u de acties die kunnen worden uitgevoerd functies toevoegen kunt, wordt uw logische app moet beginnen met een trigger.

  Als u geen ervaring met logische apps, raadpleegt u [wat is Azure Logic Apps](../logic-apps/logic-apps-overview.md) en [Quick Start: Maak uw eerste logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md).

> [!NOTE]
> Integratie van logica apps met Functions werkt niet als sleuven (preview) zijn ingeschakeld.

<a name="create-function-external"></a>

## <a name="create-functions-outside-logic-apps"></a>Functies buiten logische apps maken

In de [Azure-portal](https://portal.azure.com), maken van uw Azure-functie-app, die moet zijn van hetzelfde Azure-abonnement als uw logische app en maak vervolgens uw Azure-functie.
Als u geen ervaring hebt met het maken van Azure functions, krijgt u informatie over het [uw eerste functie maken in Azure portal](../azure-functions/functions-create-first-azure-function.md), maar houd er rekening mee deze vereisten voor het maken van functies die u vanuit logic apps aanroepen kunt:

* Zorg ervoor dat u selecteert de **HTTP-trigger** functiesjabloon voor **JavaScript** of **C#**.

  ![HTTP-trigger - JavaScript of C#](./media/logic-apps-azure-functions/http-trigger-function.png)

<a name="function-swagger"></a>

* (Optioneel) als u [een API-definitie genereren](../azure-functions/functions-openapi-definition.md), voorheen bekend als een [Swagger-bestand](https://swagger.io/), voor uw functie, krijgt u een rijkere ervaring wanneer u met parameters van de functie in de ontwerper van logische Apps werkt. Voor het instellen van uw functie-app, zodat uw logische app kunt vinden en gebruiken van functies met Swagger beschrijvingen, als volgt te werk:

  1. Zorg ervoor dat uw functie-app actief wordt uitgevoerd.

  2. In uw functie-app instellen [Cross-Origin Resource Sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) zodat alle oorsprongen zijn toegestaan door de volgende stappen:

     1. Uit de **functie-Apps** , selecteert u de functie-app > **platformfuncties** > **CORS**.

        ![Selecteer uw functie-app > 'Platformfuncties' > "CORS"](./media/logic-apps-azure-functions/function-platform-features-cors.png)

     2. Onder **CORS**, voeg de `*` jokertekens teken, maar alle andere oorsprongen verwijderen in de lijst en kies **opslaan**.

        ![Stel ' CORS * voor het jokerteken ' * '](./media/logic-apps-azure-functions/function-platform-features-cors-origins.png)

### <a name="access-property-values-inside-http-requests"></a>Toegang tot eigenschapswaarden in HTTP-aanvragen

Webhook-functies kunnen accepteren van HTTP-aanvragen als invoer en deze aanvragen doorsturen naar andere functies. Bijvoorbeeld, hoewel de Logic Apps heeft [functies waarmee u datum-/ tijdwaarden converteren](../logic-apps/workflow-definition-language-functions-reference.md), in dit eenvoudige voorbeeld JavaScript-functie laat zien hoe u toegang hebt tot een eigenschap in een Aanvraagobject voor de die wordt doorgegeven aan de functie en bewerkingen uitvoeren op de waarde van die eigenschap. Voor toegang tot de eigenschappen in objecten, in dit voorbeeld wordt de [punt (.)](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/Property_accessors): 

```javascript
function convertToDateString(request, response){
   var data = request.body;
   response = {
      body: data.date.ToDateString();
   }
}
```

Dit is wat er gebeurt binnen deze functie:

1. Hiermee maakt u de functie een `data` variabele en wijst de `body` object binnen de `request` object naar die variabele. De functie maakt gebruik van de punt (.) om te verwijzen naar de `body` object binnen de `request` object: 

   ```javascript
   var data = request.body;
   ```

2. De functie hebben nu toegang tot de `date` eigenschap via de `data` variabele en converteren naar die eigenschapswaarde van het type datum/tijd naar DateString type door het aanroepen van de `ToDateString()` functie. De functie ook retourneert het resultaat via de `body` eigenschap in het antwoord van de functie: 

   ```javascript
   body: data.date.ToDateString();
   ```

Nu dat u uw Azure-functie hebt gemaakt, volgt u de stappen voor het [functies toevoegen aan logische apps](#add-function-logic-app).

<a name="create-function-designer"></a>

## <a name="create-functions-inside-logic-apps"></a>Functies in logische apps maken

Voordat u een Azure-functie binnen uw logische app in de Logic App Designer vanaf maken kunt, moet u eerst een Azure-functie-app, dat een container voor uw functies is hebben. Als u een functie-app niet hebt, maakt u eerst die functie-app. Zie [uw eerste functie maken in Azure portal](../azure-functions/functions-create-first-azure-function.md). 

1. In de [Azure-portal](https://portal.azure.com), opent u uw logische app in Logic App Designer. 

2. Het maken en uw functie toevoegen, volg de stappen die van toepassing op uw scenario is:

   * Kies onder de laatste stap in uw logic app-werkstroom, **nieuwe stap**.

   * Tussen bestaande stappen in uw logic app-werkstroom Beweeg de muis over de pijl, kies de plusknop (+) Meld u aan en selecteer vervolgens **een actie toevoegen**.

3. Typ 'azure functions' als filter in het zoekvak.
Selecteer deze actie uit de lijst met acties: **Een Azure-functie kiezen - Azure Functions** 

   ![Zoek 'Azure functions'](./media/logic-apps-azure-functions/find-azure-functions-action.png)

4. Selecteer uw functie-app in de functie App-lijst. Nadat de lijst met acties wordt geopend, selecteert u deze actie: **Azure Functions - maken nieuwe functie**

   ![Selecteer uw functie-app](./media/logic-apps-azure-functions/select-function-app-create-function.png)

5. In de editor van de definitie van functie door uw functie te definiëren:

   1. In de **functienaam** Geef een naam voor uw functie. 

   2. In de **Code** Voeg uw code aan de functiesjabloon, met inbegrip van de reactie en u wilt dat de nettolading die wordt geretourneerd aan uw logische app na voltooiing van de functie uitgevoerd wordt. 

      ![Uw functie definiëren](./media/logic-apps-azure-functions/function-definition.png)

      In de code van de sjabloon wordt de  *`context` object* verwijst naar het bericht dat uw logische app via verzendt de **aanvraagtekst** veld in een latere stap. 
      Toegang krijgen tot de `context` van het object eigenschappen uit binnen uw functie, gebruik de volgende syntaxis: 

      `context.body.<property-name>`

      Bijvoorbeeld, om te verwijzen naar de `content` eigenschap binnen de `context` object, gebruik de volgende syntaxis: 

      `context.body.content`

      De sjablooncode bevat ook een `input` -variabele, waarmee de waarde van slaat de `data` parameter zodat uw functie bewerkingen op deze waarde kunt uitvoeren. 
      In JavaScript-functies, de `data` variabele is ook een snelkoppeling voor `context.body`.

      > [!NOTE]
      > De `body` hier van toepassing op de `context` object en is niet hetzelfde als de **hoofdtekst** -token van een actie-uitvoer, die u mogelijk ook worden doorgegeven in uw functie. 
 
   3. Wanneer u klaar bent, kiest u **Maken**.

6. In de **aanvraagtekst** geeft u de functie de invoer van, die moet worden opgemaakt als een JavaScript Object Notation (JSON)-object. 

   Deze invoer is de *context-object* of het bericht dat uw logische app naar uw functie verzendt. Wanneer u klikt op de **aanvraagtekst** veld, de lijst met dynamische inhoud wordt weergegeven, zodat u tokens voor uitvoer uit de vorige stappen selecteren kunt. In dit voorbeeld geeft aan dat de nettolading van de context een eigenschap met de naam bevat `content` waarvoor de **van** token de waarde van de e-mailtrigger:

   !["Aanvraagtekst" voorbeeld - context-object nettolading](./media/logic-apps-azure-functions/function-request-body-example.png)

   Hier het context-object is niet cast-conversie uitvoeren als een tekenreeks, zodat de inhoud van het object rechtstreeks naar de JSON-nettolading wordt toegevoegd. Als het context-object niet is een JSON-token dat wordt doorgegeven van een tekenreeks, een JSON-object of een JSON-matrix, krijgt u echter een fout. Als dit voorbeeld gebruikt de **ontvangen** token in plaats daarvan, u kunt casten het context-object als een tekenreeks door dubbele aanhalingstekens toe te voegen:  

   ![CAST-object als tekenreeks](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

7. Geef andere details, zoals de methode voor het gebruik, aanvraagheaders of queryparameters, kies **geavanceerde opties weergeven**.

<a name="add-function-logic-app"></a>

## <a name="add-existing-functions-to-logic-apps"></a>Bestaande functies toevoegen aan logische apps

U kunt voor het aanroepen van bestaande Azure-functies van uw logische apps, Azure functions, zoals een andere actie toevoegen in de Logic App Designer. 

1. In de [Azure-portal](https://portal.azure.com), opent u uw logische app in Logic App Designer. 

2. Kies onder de stap waarin u wilt toevoegen van de functie, **nieuwe stap** > **een actie toevoegen**. 

3. Typ 'azure functions' als filter in het zoekvak.
Selecteer deze actie uit de lijst met acties: **Een Azure-functie kiezen - Azure Functions** 

   ![Zoek 'Azure functions'](./media/logic-apps-azure-functions/find-azure-functions-action.png)

4. Selecteer uw functie-app in de functie App-lijst. Nadat de functielijst met wordt weergegeven, selecteert u uw functie. 

   ![Selecteer uw functie-app en de Azure-functie](./media/logic-apps-azure-functions/select-function-app-existing-function.png)

   Voor functies die API-definities (Swagger beschrijvingen) hebben en [instellen zodat uw logische app kan vinden en toegang deze functies tot](#function-swagger), kunt u **Swagger-acties**:

   ![Selecteer uw functie-app, 'Swagger-acties' ', en uw Azure-functie](./media/logic-apps-azure-functions/select-function-app-existing-function-swagger.png)

5. In de **aanvraagtekst** geeft u de functie de invoer van, die moet worden opgemaakt als een JavaScript Object Notation (JSON)-object. 

   Deze invoer is de *context-object* of het bericht dat uw logische app naar uw functie verzendt. Wanneer u klikt op de **aanvraagtekst** veld, de lijst met dynamische inhoud wordt weergegeven, zodat u tokens voor uitvoer uit de vorige stappen selecteren kunt. In dit voorbeeld geeft aan dat de nettolading van de context een eigenschap met de naam bevat `content` waarvoor de **van** token de waarde van de e-mailtrigger:

   !["Aanvraagtekst" voorbeeld - context-object nettolading](./media/logic-apps-azure-functions/function-request-body-example.png)

   Hier het context-object is niet cast-conversie uitvoeren als een tekenreeks, zodat de inhoud van het object rechtstreeks naar de JSON-nettolading wordt toegevoegd. Als het context-object niet is een JSON-token dat wordt doorgegeven van een tekenreeks, een JSON-object of een JSON-matrix, krijgt u echter een fout. Als dit voorbeeld gebruikt de **ontvangen** token in plaats daarvan, u kunt casten het context-object als een tekenreeks door dubbele aanhalingstekens toe te voegen: 

   ![CAST-object als tekenreeks](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

6. Geef andere details, zoals de methode voor het gebruik, aanvraagheaders of queryparameters, kies **geavanceerde opties weergeven**.

<a name="call-logic-app"></a>

## <a name="call-logic-apps-from-functions"></a>Logische apps aanroepen van functies

Als u wilt voor het activeren van een logische app uit in een Azure-functie, worden de logische app moet beginnen met een trigger die een aanroepbare eindpunt biedt. Bijvoorbeeld, kun u de logische app met de **HTTP**, **aanvragen**, **Azure Queues**, of **Event Grid** trigger. Binnen de functie een HTTP POST-aanvraag verzenden naar de URL van de trigger en bevatten de nettolading die u wilt dat deze logische app te verwerken. Zie voor meer informatie, [aanroepen, trigger of nesten van logische apps](../logic-apps/logic-apps-http-endpoint.md). 

## <a name="get-support"></a>Ondersteuning krijgen

* Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
* Als u ideeën voor functies wilt indienen of erop wilt stemmen, gaat u naar de [website voor feedback van Logic Apps-gebruikers](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [Logic Apps-connectors](../connectors/apis-list.md)
