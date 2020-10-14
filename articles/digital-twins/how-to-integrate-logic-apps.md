---
title: Integreren met Logic Apps
titleSuffix: Azure Digital Twins
description: Zie Logic Apps verbinding maken met Azure Digital Apparaatdubbels met behulp van een aangepaste connector
author: baanders
ms.author: baanders
ms.date: 9/11/2020
ms.topic: how-to
ms.service: digital-twins
ms.reviewer: baanders
ms.openlocfilehash: 4e9b9a7fb6e739b3bd288557457d1c152e372e26
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/14/2020
ms.locfileid: "92045292"
---
# <a name="integrate-with-logic-apps-using-a-custom-connector"></a>Integreren met Logic Apps met behulp van een aangepaste connector

[Azure Logic apps](../logic-apps/logic-apps-overview.md) is een Cloud service die u helpt bij het automatiseren van werk stromen in apps en services. Door Logic Apps te verbinden met de Azure Digital Apparaatdubbels Api's, kunt u dergelijke geautomatiseerde stromen maken rond Azure Digital Apparaatdubbels en hun gegevens.

Azure Digital Apparaatdubbels heeft momenteel geen gecertificeerde (vooraf gebouwde) connector voor Logic Apps. In plaats daarvan is het huidige proces voor het gebruik van Logic Apps met Azure Digital Apparaatdubbels een [**aangepaste Logic Apps-Connector**](../logic-apps/custom-connector-overview.md)te maken met behulp van een [aangepaste Azure Digital apparaatdubbels Swagger](/samples/azure-samples/digital-twins-custom-swaggers/azure-digital-twins-custom-swaggers/) die is aangepast aan het gebruik van Logic apps.

> [!NOTE]
> Er zijn meerdere versies van de Swagger opgenomen in het aangepaste Swagger-voor beeld dat hierboven is gekoppeld. De nieuwste versie vindt u in de submap met de meest recente datum, maar eerdere versies die in het voor beeld zijn opgenomen, worden ook nog steeds ondersteund.

In dit artikel gaat u de [Azure Portal](https://portal.azure.com) gebruiken om **een aangepaste connector te maken** die kan worden gebruikt om Logic apps te verbinden met een Azure Digital apparaatdubbels-exemplaar. Vervolgens maakt u **een logische app** die gebruikmaakt van deze verbinding voor een voorbeeld scenario, waarbij gebeurtenissen die door een timer worden geactiveerd, automatisch een dubbele update bijwerkt in uw Azure Digital apparaatdubbels-exemplaar. 

## <a name="prerequisites"></a>Vereisten

Als u geen abonnement op Azure hebt, **maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)** voordat u begint.
Meld u aan bij de [Azure Portal](https://portal.azure.com) met dit account. 

U moet ook de volgende items uitvoeren als onderdeel van de vereiste configuratie. In de rest van deze sectie wordt u stapsgewijs begeleid bij de volgende stappen:
- Een Azure Digital Twins-instantie instellen
- Client geheim voor registratie van apps ophalen
- Voeg een digitale dubbele

### <a name="set-up-azure-digital-twins-instance"></a>Een Azure Digital Twins-exemplaar instellen

Als u een Azure Digital Apparaatdubbels-exemplaar wilt verbinden met Logic Apps in dit artikel, moet u de **Azure Digital apparaatdubbels-instantie** al hebben ingesteld. 

Stel eerst een Azure Digital Twins-instantie in en de vereiste verificatie, zodat u ermee kunt werken. Volg hiervoor de instructies in [*Instructies: een exemplaar en verificatie instellen*](how-to-set-up-instance-portal.md). Afhankelijk van uw favoriete ervaring wordt het configuratie-artikel aangeboden voor de [Azure Portal](how-to-set-up-instance-portal.md), [CLI](how-to-set-up-instance-cli.md)of [geautomatiseerd voorbeeld van een Cloud Shell-implementatiescript](how-to-set-up-instance-scripted.md). Alle versies van de instructies bevatten ook stappen om te controleren of u elke stap hebt voltooid en gereed bent om door te gaan met het nieuwe exemplaar.

In deze zelf studie hebt u verschillende waarden nodig bij het instellen van uw exemplaar. Als u deze waarden opnieuw wilt opzoeken, gebruikt u de koppelingen naar de bijbehorende secties in het artikel over installeren om ze te vinden in de [Azure-portal](https://portal.azure.com).
* Azure Digital Twins-exemplaar **_hostnaam_** ([beschikbaar in de portal](how-to-set-up-instance-portal.md#verify-success-and-collect-important-values))
* Azure AD-app-registratie **_Toepassings-id (client)_** ([beschikbaar in de portal](how-to-set-up-instance-portal.md#collect-important-values))
* Azure AD-app-registratie **_Map-id (tenant)_** ([beschikbaar in de portal](how-to-set-up-instance-portal.md#collect-important-values))

### <a name="get-app-registration-client-secret"></a>Client geheim voor registratie van apps ophalen

U moet ook een **_client geheim_** maken voor de registratie van uw Azure AD-app. Als u dit wilt doen, gaat u naar de pagina [app-registraties](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) in het Azure Portal (u kunt deze koppeling gebruiken of ernaar zoeken in de zoek balk van de portal). Selecteer uw registratie in de lijst om de details ervan te openen. 

Klik op *certificaten en geheimen* in het menu van de registratie en selecteer *+ Nieuw client geheim*.

:::image type="content" source="media/how-to-integrate-logic-apps/client-secret.png" alt-text="Portal weergave van een Azure AD-App-registratie. Er is een hooglicht rondom ' certificaten en geheimen ' in het resource menu en een highlight op de pagina rondom ' nieuw client geheim '":::

Voer de gewenste waarden in voor beschrijving en verloopt en klik op *toevoegen*.

:::image type="content" source="media/how-to-integrate-logic-apps/add-client-secret.png" alt-text="Portal weergave van een Azure AD-App-registratie. Er is een hooglicht rondom ' certificaten en geheimen ' in het resource menu en een highlight op de pagina rondom ' nieuw client geheim '":::

Controleer nu of het client geheim zichtbaar is op de pagina _certificaten & geheimen_ met velden _Expires_ en _waarde_ . Noteer de _waarde_ die u later wilt gebruiken (u kunt deze ook naar het klem bord kopiëren met het Kopieer pictogram)

:::image type="content" source="media/how-to-integrate-logic-apps/client-secret-value.png" alt-text="Portal weergave van een Azure AD-App-registratie. Er is een hooglicht rondom ' certificaten en geheimen ' in het resource menu en een highlight op de pagina rondom ' nieuw client geheim '":::

### <a name="add-a-digital-twin"></a>Voeg een digitale dubbele

In dit artikel wordt Logic Apps gebruikt voor het bijwerken van een dubbele in uw Azure Digital Apparaatdubbels-exemplaar. Als u wilt door gaan, moet u ten minste één dubbele naam toevoegen aan uw exemplaar. 

U kunt apparaatdubbels toevoegen met behulp van de [DigitalTwins-api's](how-to-use-apis-sdks.md), de [.net (C#) SDK](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core)of de [Azure Digital apparaatdubbels cli](how-to-use-cli.md). Zie [*How-to: Manage Digital apparaatdubbels*](how-to-manage-twin.md)(Engelstalig) voor gedetailleerde stappen voor het maken van apparaatdubbels met behulp van deze methoden.

U hebt de **_dubbele id_** nodig van een dubbele naam in uw exemplaar dat u hebt gemaakt.

## <a name="create-custom-logic-apps-connector"></a>Aangepaste Logic Apps-Connector maken

In deze stap gaat u een [aangepaste Logic Apps-Connector](../logic-apps/custom-connector-overview.md) maken voor de Azure Digital Apparaatdubbels-api's. Nadat u dit hebt gedaan, kunt u Azure Digital Apparaatdubbels koppelen bij het maken van een logische app in de volgende sectie.

Ga naar de pagina [Logic apps aangepaste connector](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Web%2FcustomApis) in de Azure Portal (u kunt deze koppeling gebruiken of ernaar zoeken in de zoek balk van de portal). Druk op *+ toevoegen*.

:::image type="content" source="media/how-to-integrate-logic-apps/logic-apps-custom-connector.png" alt-text="Portal weergave van een Azure AD-App-registratie. Er is een hooglicht rondom ' certificaten en geheimen ' in het resource menu en een highlight op de pagina rondom ' nieuw client geheim '":::

Selecteer uw abonnement en resource groep en een naam en implementatie locatie voor uw nieuwe connector op de pagina *Logic apps aangepaste connector maken* die volgt. Druk op *beoordelen + maken*. 

:::image type="content" source="media/how-to-integrate-logic-apps/create-logic-apps-custom-connector.png" alt-text="Portal weergave van een Azure AD-App-registratie. Er is een hooglicht rondom ' certificaten en geheimen ' in het resource menu en een highlight op de pagina rondom ' nieuw client geheim '":::

Hiermee gaat u naar het tabblad *controleren en maken* , waar u onder *maken* kunt raken om uw resource te maken.

:::image type="content" source="media/how-to-integrate-logic-apps/review-logic-apps-custom-connector.png" alt-text="Portal weergave van een Azure AD-App-registratie. Er is een hooglicht rondom ' certificaten en geheimen ' in het resource menu en een highlight op de pagina rondom ' nieuw client geheim '":::

U wordt naar de implementatie pagina voor de connector geleid. Wanneer de implementatie is voltooid, klikt u op de knop *Ga naar resource* om de details van de connector in de portal weer te geven.

### <a name="configure-connector-for-azure-digital-twins"></a>Connector configureren voor Azure Digital Apparaatdubbels

Vervolgens configureert u de connector die u hebt gemaakt om Azure Digital Apparaatdubbels te bereiken.

Down load eerst een aangepaste Apparaatdubbels Swagger van Azure die is gewijzigd om met Logic Apps te werken. Down load het voor beeld met **aangepaste Swagger-apparaatdubbels van Azure** via [**deze koppeling**](/samples/azure-samples/digital-twins-custom-swaggers/azure-digital-twins-custom-swaggers/) door te klikken op de knop voor het downloaden van het *zip-bestand* . Ga naar de map gedownloade *Azure_Digital_Twins_Custom_Swaggers.zip* en pak deze uit. 

De aangepaste Swagger voor deze zelf studie bevindt zich in de map _**Azure_Digital_Twins_Custom_Swaggers \logicapps**_ . Deze map bevat submappen met de naam *stabiel* en *Preview*, die beide verschillende versies van de Swagger bevatten, geordend op datum. De map met de meest recente datum bevat de laatste kopie van de Swagger. Welke versie u selecteert, is het Swagger-bestand _** met de naamdigitaltwins.jsop**_.

> [!NOTE]
> Tenzij u werkt met een preview-functie, wordt het over het algemeen aanbevolen de meest recente *stabiele* versie van de Swagger te gebruiken. Eerdere versies en Preview-versies van de Swagger worden echter ook nog steeds ondersteund. 

Ga vervolgens naar de overzichts pagina van uw connector in de [Azure Portal](https://portal.azure.com) en klik op *bewerken*.

:::image type="content" source="media/how-to-integrate-logic-apps/edit-connector.png" alt-text="Portal weergave van een Azure AD-App-registratie. Er is een hooglicht rondom ' certificaten en geheimen ' in het resource menu en een highlight op de pagina rondom ' nieuw client geheim '":::

Configureer de volgende gegevens op de pagina *Logic apps aangepaste connector bewerken* :
* **Aangepaste connectors**
    - API-eind punt: REST (laat de standaard instelling)
    - Import modus: OpenAPI-bestand (standaard instelling behouden)
    - Bestand: dit is het aangepaste Swagger-bestand dat u eerder hebt gedownload. Klik op *importeren*, zoek het bestand op uw computer (*Azure_Digital_Twins_Custom_Swaggers \logicapps \...\digitaltwins.jsaan*) en klik op *openen*.
* **Algemene informatie**
    - Pictogram: Upload een pictogram dat u wilt
    - Achtergrond kleur van pictogram: Voer hexadecimale code in de notatie #xxxxxx in voor de kleur.
    - Beschrijving: Vul de gewenste waarden in.
    - Schema: HTTPS (standaard instelling behouden)
    - Host: de *hostnaam* van uw Azure Digital apparaatdubbels-exemplaar.
    - Basis-URL:/(standaard instelling behouden)

Klik vervolgens op de knop *beveiliging* aan de onderkant van het venster om door te gaan naar de volgende configuratie stap.

:::image type="content" source="media/how-to-integrate-logic-apps/configure-next.png" alt-text="Portal weergave van een Azure AD-App-registratie. Er is een hooglicht rondom ' certificaten en geheimen ' in het resource menu en een highlight op de pagina rondom ' nieuw client geheim '":::

Klik in de beveiligings stap op *bewerken* en configureer deze gegevens:
* **Verificatie type**: OAuth 2,0
* **OAuth 2,0**:
    - ID-provider: Azure Active Directory
    - Client-ID: de *toepassing (client)-ID* voor uw Azure AD-App-registratie
    - Client geheim: het *client geheim* dat u hebt gemaakt in [*vereisten*](#prerequisites) voor uw Azure AD-App-registratie
    - Aanmeldings-URL: https://login.windows.net (standaard behouden)
    - Tenant-ID: de *Directory-id (Tenant)* voor uw Azure AD-App-registratie
    - Resource-URL: 0b07f429-9f4b-4714-9392-cc5e8e80c8b0
    - Bereik: map. AccessAsUser. all
    - Omleidings-URL: (de standaard instelling voor nu laten staan)

Houd er rekening mee dat het veld omleidings-URL *de aangepaste connector opslaat om de omleidings-URL te genereren*. Doe dit nu door op de pagina *Update connector* boven aan het deel venster te gaan om de connector instellingen te bevestigen.

:::image type="content" source="media/how-to-integrate-logic-apps/update-connector.png" alt-text="Portal weergave van een Azure AD-App-registratie. Er is een hooglicht rondom ' certificaten en geheimen ' in het resource menu en een highlight op de pagina rondom ' nieuw client geheim '":::

<!-- Success message? didn't see one -->

Ga terug naar het veld omleidings-URL en kopieer de waarde die is gegenereerd. U gebruikt deze in de volgende stap.

:::image type="content" source="media/how-to-integrate-logic-apps/copy-redirect-url.png" alt-text="Portal weergave van een Azure AD-App-registratie. Er is een hooglicht rondom ' certificaten en geheimen ' in het resource menu en een highlight op de pagina rondom ' nieuw client geheim '":::

Dit is alle informatie die nodig is voor het maken van de connector (u hoeft de beveiliging van de definitie niet te voortzetten. U kunt het deel venster *aangepaste connector bewerken Logic apps* sluiten.

>[!NOTE]
>Op de overzichts pagina van uw connector waar u oorspronkelijk hebt *geklikt, moet u*er rekening mee houden dat het hele proces van het invoeren van de configuratie *keuzes opnieuw wordt* gestart. Uw waarden worden niet gevuld wanneer u deze voor het laatst hebt door lopen, dus als u een bijgewerkte configuratie met gewijzigde waarden wilt opslaan, moet u alle andere waarden ook opnieuw invoeren om te voor komen dat ze worden overschreven door de standaard instellingen.

### <a name="grant-connector-permissions-in-the-azure-ad-app"></a>Connector machtigingen verlenen in de Azure AD-app

Gebruik vervolgens de *omleidings-URL* -waarde van de aangepaste connector die u in de laatste stap hebt gekopieerd om de connector machtigingen te verlenen in uw Azure AD-App-registratie.

Ga naar de pagina [app-registraties](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) in het Azure Portal en selecteer uw registratie in de lijst. 

Onder *verificatie* vanuit het menu registratie voegt u een URI toe.

:::image type="content" source="media/how-to-integrate-logic-apps/add-uri.png" alt-text="Portal weergave van een Azure AD-App-registratie. Er is een hooglicht rondom ' certificaten en geheimen ' in het resource menu en een highlight op de pagina rondom ' nieuw client geheim '"::: 

Voer de *omleidings-URL* van de aangepaste connector in het nieuwe veld in en klik op het pictogram *Opslaan* .

:::image type="content" source="media/how-to-integrate-logic-apps/save-uri.png" alt-text="Portal weergave van een Azure AD-App-registratie. Er is een hooglicht rondom ' certificaten en geheimen ' in het resource menu en een highlight op de pagina rondom ' nieuw client geheim '":::

U bent nu klaar met het instellen van een aangepaste connector die toegang heeft tot de Azure Digital Apparaatdubbels-Api's. 

## <a name="create-logic-app"></a>Logische app maken

Vervolgens maakt u een logische app die uw nieuwe connector gebruikt voor het automatiseren van Azure Digital Apparaatdubbels-updates.

Zoek in de [Azure Portal](https://portal.azure.com)naar *logische apps* in de zoek balk van de portal. Als u deze optie selecteert, wordt u naar de pagina *Logic apps* geleid. Klik op de knop *logische app maken* om een nieuwe logische app te maken.

:::image type="content" source="media/how-to-integrate-logic-apps/create-logic-app.png" alt-text="Portal weergave van een Azure AD-App-registratie. Er is een hooglicht rondom ' certificaten en geheimen ' in het resource menu en een highlight op de pagina rondom ' nieuw client geheim '":::

Voer op de pagina *logische app* die volgt uw abonnement en de resource groep in. Kies ook een naam voor uw logische app en selecteer de implementatie locatie.

Druk op de knop _beoordeling + maken_ .

Hiermee gaat u naar het tabblad *controleren en maken* , waar u uw gegevens kunt bekijken en onder aan de slag wilt gaan om uw resource *te maken.*

U gaat naar de implementatie pagina voor de logische app. Wanneer de implementatie is voltooid, klikt u op de knop *Ga naar resource* om door te gaan naar de *Logic apps Designer*, waarin u de logica van de werk stroom moet invullen.

### <a name="design-workflow"></a>Ontwerp werk stroom

Selecteer in de *ontwerp functie voor Logic apps*onder *beginnen met een algemene trigger de*optie _**terugkeer patroon**_.

:::image type="content" source="media/how-to-integrate-logic-apps/logic-apps-designer-recurrence.png" alt-text="Portal weergave van een Azure AD-App-registratie. Er is een hooglicht rondom ' certificaten en geheimen ' in het resource menu en een highlight op de pagina rondom ' nieuw client geheim '":::

Wijzig de frequentie van het **terugkeer patroon** in de pagina *Logic apps Designer* die volgt op de *tweede*plaats, zodat de gebeurtenis elke 3 seconden wordt geactiveerd. Dit maakt het eenvoudig om de resultaten later te bekijken zonder lang te hoeven wachten.

Druk op *+ nieuwe stap*.

Hiermee opent u een vak *actie kiezen* . Schakel over naar het tabblad *aangepast* . U ziet de aangepaste connector eerder in het bovenste vak.

:::image type="content" source="media/how-to-integrate-logic-apps/custom-action.png" alt-text="Portal weergave van een Azure AD-App-registratie. Er is een hooglicht rondom ' certificaten en geheimen ' in het resource menu en een highlight op de pagina rondom ' nieuw client geheim '":::

Selecteer deze optie om de lijst met Api's in die connector weer te geven. Gebruik de zoek balk of blader door de lijst om **DigitalTwins_Add**te selecteren. (Dit is de API die in dit artikel wordt gebruikt, maar u kunt ook een andere API selecteren als een geldige keuze voor een Logic Apps verbinding).

U wordt mogelijk gevraagd om u aan te melden met uw Azure-referenties om verbinding te maken met de connector. Als u een dialoog venster *machtigingen hebt aangevraagd* , volgt u de aanwijzingen om toestemming te geven voor uw app en om te accepteren.

Vul in het vak nieuwe *DigitalTwinsAdd* de velden als volgt in:
* _id_: Vul de *dubbele id* van het digitale dubbele item in uw exemplaar dat u wilt dat de logische app wordt bijgewerkt.
* _dubbele_: in dit veld voert u de hoofd tekst in die voor de gekozen API-aanvraag nodig is. Voor *DigitalTwinsUpdate*is deze tekst in de vorm van de JSON-patch code. Voor meer informatie over het structureren van een JSON-patch voor het bijwerken van uw twee, raadpleegt u het artikel [een digitale dubbele sectie bijwerken](how-to-manage-twin.md#update-a-digital-twin) van *How-to: Manage Digital apparaatdubbels*.
* _API-Version_: de nieuwste API-versie. In de huidige open bare preview is deze waarde *2020-05-31-preview*

Druk op *Opslaan* in de Logic apps Designer.

U kunt andere bewerkingen kiezen door _+ nieuwe stap_ te selecteren in hetzelfde venster.

:::image type="content" source="media/how-to-integrate-logic-apps/save-logic-app.png" alt-text="Portal weergave van een Azure AD-App-registratie. Er is een hooglicht rondom ' certificaten en geheimen ' in het resource menu en een highlight op de pagina rondom ' nieuw client geheim '":::

## <a name="query-twin-to-see-the-update"></a>Query dubbele om de update te zien

Nu uw logische app is gemaakt, moet de gebeurtenis voor dubbele updates die u hebt gedefinieerd in de Logic Apps Designer een herhaling van elke drie seconden plaatsvinden. Dit betekent dat u na drie seconden een query kunt uitvoeren op uw dubbele gegevens en de nieuwe patches weer geven.

U kunt een query uitvoeren op uw dubbele manier via uw keuze methode (zoals een [aangepaste client-app](tutorial-command-line-app.md), de voor [beeld-app van Azure Digital apparaatdubbels Explorer](/samples/azure-samples/digital-twins-explorer/digital-twins-explorer/), de [Sdk's en api's](how-to-use-apis-sdks.md)of de [cli](how-to-use-cli.md)). 

Zie [*How-to: query uitvoeren op de dubbele grafiek*](how-to-query-graph.md)voor meer informatie over het uitvoeren van query's op uw Azure Digital apparaatdubbels-exemplaar.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een logische app gemaakt die regel matig een dubbele update bijwerkt in uw Azure Digital Apparaatdubbels-exemplaar met een patch die u hebt ingevoerd. U kunt proberen om andere Api's te selecteren in de aangepaste connector om Logic Apps te maken voor diverse acties op uw exemplaar.

Ga voor meer informatie over de beschik bare Api's en de details die ze nodig hebben naar [*instructies: gebruik de Azure Digital Apparaatdubbels api's en sdk's*](how-to-use-apis-sdks.md).