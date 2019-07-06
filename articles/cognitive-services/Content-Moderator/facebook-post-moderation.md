---
title: 'Zelfstudie: Beheren van inhoud van de Facebook - Content Moderator'
titlesuffix: Azure Cognitive Services
description: In deze zelfstudie leert u hoe u met behulp van machine learning en Content Moderator berichten en opmerkingen voor Facebook kunt controleren.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: dd06330e82850cc44bc0f4d36ba7caf596ace939
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603505"
---
# <a name="tutorial-moderate-facebook-posts-and-commands-with-azure-content-moderator"></a>Zelfstudie: Gemiddeld Facebook-berichten en opdrachten met Azure Content Moderator

In deze zelfstudie leert u hoe u Azure Content Moderator om u te helpen met het gemiddelde van de berichten en opmerkingen op een Facebook-pagina. Facebook wordt de inhoud die wordt gepost door bezoekers van de Content Moderator-service verzonden. Vervolgens wordt uw Content Moderator-werkstromen de inhoud publiceren of beoordelingen binnen het beoordelingsprogramma, afhankelijk van de drempelwaarden en scores die inhoud maken. Zie de [Build 2017 demovideo](https://channel9.msdn.com/Events/Build/2017/T6033) voor een voorbeeld van een werkende van dit scenario.

In deze zelfstudie ontdekt u hoe u:

> [!div class="checklist"]
> * Een Content Moderator-team samenstellen.
> * Azure-functies maken die luisteren naar HTTP-gebeurtenissen van Content Moderator en Facebook.
> * Koppel een Facebook-pagina aan de Content Moderator met behulp van een Facebook-toepassing.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

Het volgende diagram illustreert elk onderdeel van dit scenario:

![Diagram van Content Moderator ontvangen van gegevens uit Facebook via 'FBListener' en het verzenden van gegevens via "CMListener"](images/tutorial-facebook-moderation.png)

> [!IMPORTANT]
> In 2018, Facebook geïmplementeerd een meer strikt gaan van Facebook-Apps. Niet mogelijk voor het voltooien van de stappen in deze zelfstudie als uw app is niet gecontroleerd en goedgekeurd door het team van de beoordeling Facebook.

## <a name="prerequisites"></a>Vereisten

- Een abonnementssleutel voor Content Moderator. Volg de instructies in [Een Cognitive Services-account maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) om u te abonneren op Content Moderator en een sleutel op te halen.
- Een [Facebook-account](https://www.facebook.com/).

## <a name="create-a-review-team"></a>Een beoordelingsteam maken

Verwijzen naar de [Content Moderator proberen op het web](quick-start.md) Quick Start voor instructies over hoe u zich aanmelden voor de [Content Moderator bekijken hulpprogramma](https://contentmoderator.cognitive.microsoft.com/) en maken van een beoordelingsteam. Noteer de waarde voor **Team Id** op de pagina **Create review team**.

## <a name="configure-image-moderation-workflow"></a>Afbeeldingstoezicht werkstroom configureren

Raadpleeg de [definiëren, test en gebruik werkstromen](review-tool-user-guide/workflows.md) handleiding voor het maken van een aangepaste installatiekopie-werkstroom. Content Moderator gebruikt deze werkstroom automatisch controleren van installatiekopieën op Facebook en sommige aan het beoordelingsprogramma verstuurt. Noteer de werkstroom **naam**.

## <a name="configure-text-moderation-workflow"></a>Tekst toezicht werkstroom configureren

Nogmaals, verwijzen naar de [definiëren, test en gebruik werkstromen](review-tool-user-guide/workflows.md) handleiding; dit keer, maakt u een aangepaste tekst-werkstroom. Content Moderator gebruikt deze werkstroom automatisch controleren op tekst. Noteer de werkstroom **naam**.

![Tekstwerkstroom configureren](images/text-workflow-configure.PNG)

Test uw werkstroom met behulp van de **werkstroom uitvoeren** knop.

![Tekstwerkstroom testen](images/text-workflow-test.PNG)

## <a name="create-azure-functions"></a>Azure-functies maken

Aanmelden bij de [Azure-portal](https://portal.azure.com/) en volg deze stappen:

1. Maak een Azure-functie-app zoals wordt weergegeven op de pagina [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal).
1. Ga naar de zojuist gemaakte functie-App.
1. In de App, gaat u naar de **platformfuncties** tabblad en selecteer **configuratie**. In de **toepassingsinstellingen** sectie van de volgende pagina, selecteer **nieuwe toepassingsinstelling** om toe te voegen van de volgende sleutel/waarde-paren:
    
    | Naam van de App-instelling | value   | 
    | -------------------- |-------------|
    | cm:TeamId   | De id van het Content Moderator-team  | 
    | cm:SubscriptionKey | Uw abonnementssleutel voor Content Moderator: zie [Referenties](review-tool-user-guide/credentials.md) |
    | cm:Region | De naam van uw Content Moderator-regio, zonder de spaties. |
    | cm:ImageWorkflow | De naam van de werkstroom om uit te voeren voor afbeeldingen |
    | cm:TextWorkflow | De naam van de werkstroom om uit te voeren voor tekst |
    | cm:CallbackEndpoint | URL voor de CMListener functie-App die u verderop in deze handleiding maken gaat |
    | fb:VerificationToken | Een token voor geheim dat u hebt gemaakt, gebruikt om u te abonneren op de gebeurtenissen feed Facebook |
    | fb:PageAccessToken | Het toegangstoken voor de Facebook Graph-API verloopt niet en maakt het mogelijk de functie voor het verbergen/verwijderen van berichten namens u uit te voeren. U ontvangt dit token in een latere stap. |

    Klik op de **opslaan** knop aan de bovenkant van de pagina.

1. Ga terug naar de **platformfuncties** tabblad. Gebruik de **+** knop in het linkerdeelvenster om de **nieuwe functie** deelvenster. De functie die u gaat maken, worden gebeurtenissen ontvangen van Facebook.

    ![Azure Functions-deelvenster met de knop van de functie toevoegen gemarkeerd.](images/new-function.png)

    1. Klik op de tegel met de melding dat **Http-trigger**.
    1. Voer de naam **FBListener** in. Stel **Autorisatieniveau** in op **Functie**.
    1. Klik op **Create**.
    1. Vervang de inhoud van de **run.csx** met de inhoud van **FbListener/run.csx**

    [!code-csharp[FBListener: csx file](~/samples-fbPageModeration/FbListener/run.csx?range=1-154)]

1. Maak een nieuwe **Http-trigger** functie met de naam **CMListener**. Deze functie ontvangt gebeurtenissen van Content Moderator. Vervang de inhoud van de **run.csx** met de inhoud van **CMListener/run.csx**

    [!code-csharp[FBListener: csx file](~/samples-fbPageModeration/CmListener/run.csx?range=1-110)]

---

## <a name="configure-the-facebook-page-and-app"></a>De Facebook-pagina en -app configureren

1. Maak een Facebook-app.

    ![Facebook-pagina voor ontwikkelaars](images/facebook-developer-app.png)

    1. Navigeer naar de [site voor Facebook-ontwikkelaars](https://developers.facebook.com/)
    1. Klik op **My Apps**.
    1. Voeg een nieuw app toe.
    1. een andere naam
    1. Selecteer **Webhooks -> Set Up**
    1. Selecteer **pagina** in de vervolgkeuzelijst en selecteer **abonneren op dit object**
    1. Geef **FBListener Url** op als de Callback URL en het **Verify Token** dat u hebt geconfigureerd onder **Function App Settings**
    1. Als u zich hebt geabonneerd, bladert u omlaag naar de feed en selecteert u **subscribe**.
    1. Klik op de **testen** knop van de **feed** rij voor het verzenden van een testbericht naar uw Azure-functie FBListener in, klik op de **verzenden naar MijnServer** knop. Hier ziet u de aanvraag wordt ontvangen op uw FBListener.

1. Maak een Facebook-pagina.

    > [!IMPORTANT]
    > In 2018, Facebook geïmplementeerd een meer strikt gaan van Facebook-apps. Niet mogelijk om uit te voeren secties 2, 3 en 4 als uw app is niet gecontroleerd en goedgekeurd door het team van de beoordeling Facebook.

    1. Navigeer naar [Facebook](https://www.facebook.com/bookmarks/pages) en maak een **nieuwe Facebook-pagina**.
    1. Volg deze stappen om toe te staan dat de Facebook-app toegang heeft tot deze pagina:
        1. Navigeer naar de [Graph API Explorer](https://developers.facebook.com/tools/explorer/).
        1. Selecteer **Application**.
        1. Selecteer **Page Access Token**, verstuur een **Get**-aanvraag.
        1. Klik op de **Page ID** in het antwoord.
        1. Voeg de **/subscribed_apps** toe aan de URL en verstuur een **Get**-aanvraag (leeg antwoord).
        1. Verstuur een **Post**-aanvraag. U krijgt het antwoord als **success: true**.

3. Maak een Graph API-toegangstoken dat niet verloopt.

    1. Navigeer naar de [Graph API Explorer](https://developers.facebook.com/tools/explorer/).
    2. Selecteer de optie **Application**.
    3. Selecteer de optie **Get User Access Token**.
    4. Selecteer onder **Select Permissions** de opties **manage_pages** en **publish_pages**.
    5. We gebruiken het **toegangstoken** (token met korte levensduur) in de volgende stap.

4. We gebruiken Postman voor de volgende stappen.

    1. Open **Postman** (of downloadt het programma [hier](https://www.getpostman.com/)).
    2. Importeer deze twee bestanden:
        1. [Postman Collection](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/Facebook%20Permanant%20Page%20Access%20Token.postman_collection.json)
        2. [Postman Environment](https://github.com/MicrosoftContentModerator/samples-fbPageModeration/blob/master/FB%20Page%20Access%20Token%20Environment.postman_environment.json)       
    3. Werk deze omgevingsvariabelen bij:
    
        | Sleutel | Value   | 
        | -------------------- |-------------|
        | appId   | Voeg hier de id van uw Facebook-app in  | 
        | appSecret | Voeg hier het geheim van uw Facebook-app in | 
        | token_met_korte_levensduur | Voeg hier het toegangstoken met korte levensduur van de gebruiker in dat u in de vorige stap hebt gegenereerd |
    4. Voer nu de drie API's uit die in de verzameling worden vermeld: 
        1. Selecteer **Generate Long-Lived Access Token** en klik op **Send**.
        2. Selecteer **Get User ID** en klik op **Send**.
        3. Selecteer **Get Permanent Page Access Token** en klik op **Send**.
    5. Kopieer de waarde van **access_token** in het antwoord en wijs deze toe aan de app-instelling, **fb:PageAccessToken**.

De oplossing verzendt alle afbeeldingen en tekst die op uw Facebook-pagina worden geplaatst naar Content Moderator. Vervolgens worden de werkstromen die u eerder hebt geconfigureerd, worden aangeroepen. De inhoud die niet met uw criteria die zijn gedefinieerd in de werkstromen wordt doorgegeven aan de beoordelingen binnen het beoordelingsprogramma. De rest van de inhoud wordt automatisch gepubliceerd.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een programma gemaakt voor het analyseren van productafbeeldingen om deze te taggen op producttype, waarna een beoordelingsteam kan beslissen of de afbeeldingen al dan niet geschikt zijn. In de volgende zelfstudie wordt meer aandacht besteed aan het beoordelen van gecontroleerde afbeeldingen.

> [!div class="nextstepaction"]
> [Beheer van afbeeldingen](./image-moderation-api.md)
