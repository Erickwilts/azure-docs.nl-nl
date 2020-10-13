---
title: Een Azure-functie instellen om gegevens te verwerken
titleSuffix: Azure Digital Twins
description: Zie een Azure-functie maken die kan worden geopend en geactiveerd door Digital apparaatdubbels.
author: baanders
ms.author: baanders
ms.date: 8/27/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 11034a585db6ef3a3e1e445bbed8a1e51f83948d
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2020
ms.locfileid: "91949783"
---
# <a name="connect-azure-functions-apps-for-processing-data"></a>Azure Functions-apps verbinden voor het verwerken van gegevens

Het bijwerken van digitale apparaatdubbels op basis van gegevens wordt verwerkt met behulp van [**gebeurtenis routes**](concepts-route-events.md) via Compute-resources, zoals [Azure functions](../azure-functions/functions-overview.md). Een Azure-functie kan worden gebruikt om een digitale dubbele taak bij te werken als reactie op:
* telemetrie-gegevens van apparaten die afkomstig zijn van IoT Hub
* eigenschaps wijziging of andere gegevens die afkomstig zijn van een andere digitale twee in het dubbele diagram

Dit artikel helpt u bij het maken van een Azure-functie voor gebruik met Azure Digital Apparaatdubbels. 

Hier volgt een overzicht van de stappen die het bevat:

1. Een Azure Functions-app maken in Visual Studio
2. Een Azure-functie schrijven met een [Event grid](../event-grid/overview.md) trigger
3. Voeg verificatie code toe aan de functie (om toegang te krijgen tot Azure Digital Apparaatdubbels)
4. De functie-app publiceren in azure
5. [Beveiligings](concepts-security.md) toegang instellen voor de Azure function-app

## <a name="create-an-azure-functions-app-in-visual-studio"></a>Een Azure Functions-app maken in Visual Studio

Selecteer in Visual Studio 2019 _bestand > nieuw > project_ en zoek naar de _Azure functions_ sjabloon en selecteer _volgende_.

:::image type="content" source="media/how-to-create-azure-function/create-azure-function-project.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

Geef een naam op voor de functie-app en selecteer _maken_.

:::image type="content" source="media/how-to-create-azure-function/configure-new-project.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

Selecteer het type functie-app *Event grid trigger* en selecteer _maken_.

:::image type="content" source="media/how-to-create-azure-function/eventgridtrigger-function.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

Als uw functie-app is gemaakt, heeft uw Visual Studio automatisch een code voorbeeld in het **Function.cs** -bestand in de projectmap. Deze korte Azure-functie wordt gebruikt om gebeurtenissen te registreren.

:::image type="content" source="media/how-to-create-azure-function/visual-studio-sample-code.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

## <a name="write-an-azure-function-with-an-event-grid-trigger"></a>Een Azure-functie schrijven met een Event Grid trigger

U kunt een Azure-functie schrijven door de SDK toe te voegen aan uw functie-app. De functie-app communiceert met Azure Digital Apparaatdubbels met behulp [van de Azure IOT Digital-client bibliotheek voor .net (C#)](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core). 

Als u de SDK wilt gebruiken, moet u de volgende pakketten in uw project toevoegen. U kunt de pakketten installeren met behulp van Visual Studio NuGet package manager of de pakketten toevoegen met behulp van het `dotnet` opdracht regel programma. Kies een van de volgende methoden: 

**Optie 1. Pakketten toevoegen met behulp van Visual Studio Package Manager:**
    
U kunt dit doen door met de rechter muisknop op het project te klikken en _NuGet-pakketten beheren_ te selecteren in de lijst. In het venster dat wordt geopend, selecteert u op het tabblad _Bladeren_ en zoekt u naar de volgende pakketten. Selecteer _installeren_ en _Accepteer_ de gebruiksrecht overeenkomst om de pakketten te installeren.

* `Azure.DigitalTwins.Core`
* `Azure.Identity` 

Voor de configuratie van de Azure SDK-pijp lijn die op de juiste wijze moet worden ingesteld voor Azure Functions, hebt u ook de volgende pakketten nodig. Herhaal hetzelfde proces als hierboven om alle pakketten te installeren.

* `System.Net.Http`
* `Azure.Core.Pipeline`

**Optie 2. Pakketten toevoegen met behulp van `dotnet` het opdracht regel programma:**

```cmd/sh
dotnet add package Azure.DigitalTwins.Core --version 1.0.0-preview.3
dotnet add package Azure.identity --version 1.2.2
dotnet add package System.Net.Http
dotnet add package Azure.Core.Pipeline
```
Open vervolgens in uw Visual Studio-Solution Explorer het _Function.cs_ -bestand _met_ voorbeeld code en voeg de volgende instructies toe aan uw Azure-functie. 

```csharp
using Azure.DigitalTwins.Core;
using Azure.Identity;
using System.Net.Http;
using Azure.Core.Pipeline;
```
## <a name="add-authentication-code-to-the-azure-function"></a>Verificatie code toevoegen aan de Azure-functie

U declareert nu variabelen op klasseniveau en voegt een verificatie code toe waarmee de functie toegang kan krijgen tot Azure Digital Apparaatdubbels. U gaat het volgende toevoegen aan uw Azure-functie in het bestand {uw functie naam}. cs.

* Lees de URL van de ADT-service als een omgevings variabele. Het is een goed idee om de service-URL te lezen uit een omgevings variabele in plaats van deze op te harden in de functie.
```csharp     
private static readonly string adtInstanceUrl = Environment.GetEnvironmentVariable("ADT_SERVICE_URL");
```
* Een statische variabele die een httpclient maakt-exemplaar kan bevatten. Httpclient maakt is relatief kostbaar en we willen voor komen dat u dit hoeft te doen voor elke functie aanroep.
```csharp
private static readonly HttpClient httpClient = new HttpClient();
```
* U kunt de referenties beheerde identiteit in azure gebruiken.
```csharp
ManagedIdentityCredential cred = new ManagedIdentityCredential("https://digitaltwins.azure.net");
```
* Voeg in uw functie een lokale variabele _DigitalTwinsClient_ toe om uw Azure Digital apparaatdubbels-client exemplaar te bewaren voor het functie project. Maak deze variabele *niet* statisch in uw klasse.
```csharp
DigitalTwinsClient client = new DigitalTwinsClient(new Uri(adtInstanceUrl), cred, new DigitalTwinsClientOptions { Transport = new HttpClientTransport(httpClient) });
```
* Voeg een null-controle toe voor _adtInstanceUrl_ en verpakken de functie logica in een try catch-blok om uitzonde ringen te ondervangen.

Na deze wijzigingen wordt de functie code op de volgende manier weer gegeven:

```csharp
// Default URL for triggering event grid function in the local environment.
// http://localhost:7071/runtime/webhooks/EventGrid?functionName={functionname}
using System;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Extensions.Logging;
using Azure.DigitalTwins.Core;
using Azure.Identity;
using System.Net.Http;
using Azure.Core.Pipeline;

namespace adtIngestFunctionSample
{
    public class Function1
    {
        //Your Digital Twin URL is stored in an application setting in Azure Functions
        private static readonly string adtInstanceUrl = Environment.GetEnvironmentVariable("ADT_SERVICE_URL");
        private static readonly HttpClient httpClient = new HttpClient();

        [FunctionName("TwinsFunction")]
        public void Run([EventGridTrigger] EventGridEvent eventGridEvent, ILogger log)
        {
            log.LogInformation(eventGridEvent.Data.ToString());
            if (adtInstanceUrl == null) log.LogError("Application setting \"ADT_SERVICE_URL\" not set");
            try
            {
                //Authenticate with Digital Twins
                ManagedIdentityCredential cred = new ManagedIdentityCredential("https://digitaltwins.azure.net");
                DigitalTwinsClient client = new DigitalTwinsClient(new Uri(adtInstanceUrl), cred, new DigitalTwinsClientOptions { Transport = new HttpClientTransport(httpClient) });
                log.LogInformation($"ADT service client connection created.");
                /*
                * Add your business logic here
                */
            }
            catch (Exception e)
            {
                log.LogError(e.Message);
            }

        }
    }
}
```

## <a name="publish-the-function-app-to-azure"></a>De functie-app publiceren in azure

Als u de functie-app naar Azure wilt publiceren, selecteert u het functie project (niet de oplossing) in Solution Explorer en kiest u **publiceren**.

> [!IMPORTANT] 
> Als u een Azure-functie publiceert, worden er extra kosten in rekening gebracht voor uw abonnement, onafhankelijk van Azure Digital Apparaatdubbels.

:::image type="content" source="media/how-to-create-azure-function/publish-azure-function.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

Selecteer **Azure** als publicatie doel en selecteer **volgende**.

:::image type="content" source="media/how-to-create-azure-function/publish-azure-function-1.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

:::image type="content" source="media/how-to-create-azure-function/publish-azure-function-2.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

:::image type="content" source="media/how-to-create-azure-function/publish-azure-function-3.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

:::image type="content" source="media/how-to-create-azure-function/publish-azure-function-4.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

:::image type="content" source="media/how-to-create-azure-function/publish-azure-function-5.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

Voer op de volgende pagina de gewenste naam in voor de nieuwe functie-app, een resource groep en andere gegevens.
Als u wilt dat uw functions-app toegang kan krijgen tot Azure Digital Apparaatdubbels, moet deze een door een systeem beheerde identiteit hebben en gemachtigd zijn om toegang te krijgen tot uw Azure Digital Apparaatdubbels-exemplaar.

Vervolgens kunt u beveiligings toegang voor de functie instellen met behulp van CLI of Azure Portal. Kies een van de volgende methoden:

## <a name="set-up-security-access-for-the-azure-function-app"></a>Beveiligings toegang instellen voor de Azure function-app
U kunt de beveiligings toegang voor de Azure function-app instellen met een van de volgende opties:

### <a name="option-1-set-up-security-access-for-the-azure-function-app-using-cli"></a>Optie 1: beveiligings toegang voor de Azure function-app instellen met behulp van CLI

De Azure function-skelet van eerdere voor beelden vereist dat er een Bearer-token wordt door gegeven, zodat het kan worden geverifieerd met Azure Digital Apparaatdubbels. Als u er zeker van wilt zijn dat dit Bearer-token is door gegeven, moet u [Managed Service Identity (MSI)](../active-directory/managed-identities-azure-resources/overview.md) instellen voor de functie-app. U hoeft dit slechts één keer te doen voor elke functie-app.

U kunt door het systeem beheerde identiteit maken en de identiteit van de functie-app toewijzen aan de rol _Azure Digital Apparaatdubbels Owner (preview)_ voor uw Azure Digital apparaatdubbels-instantie. Hiermee geeft u de functie-app toestemming in het exemplaar voor het uitvoeren van activiteiten voor gegevens vlak. Vervolgens stelt u de URL van het Azure Digital Apparaatdubbels-exemplaar toegankelijk te maken voor uw functie door een omgevings variabele in te stellen.

 Gebruik [Azure Cloud shell](https://shell.azure.com) om de opdrachten uit te voeren.

Gebruik de volgende opdracht om de door het systeem beheerde identiteit te maken. Noteer het veld _principalId_ in de uitvoer.

```azurecli 
az functionapp identity assign -g <your-resource-group> -n <your-App-Service-(function-app)-name>   
```
Gebruik de waarde _principalId_ in de volgende opdracht om de identiteit van de functie-app toe te wijzen aan de rol _Azure Digital Apparaatdubbels Owner (preview)_ voor uw Azure Digital apparaatdubbels-exemplaar.

```azurecli 
az dt role-assignment create --dt-name <your-Azure-Digital-Twins-instance> --assignee "<principal-ID>" --role "Azure Digital Twins Owner (Preview)"
```
Ten slotte kunt u de URL van uw Azure Digital Apparaatdubbels-exemplaar toegankelijk maken voor uw functie door een omgevings variabele in te stellen. Zie [*omgevings variabelen*](https://docs.microsoft.com/sandbox/functions-recipes/environment-variables)voor meer informatie over het instellen van omgevings variabelen. 

> [!TIP]
> De URL van het Azure Digital Apparaatdubbels-exemplaar wordt gemaakt door *https://* toe te voegen aan het begin van de *hostnaam*van uw Azure Digital apparaatdubbels-exemplaar. Als u de hostnaam, samen met alle eigenschappen van uw exemplaar, wilt zien, kunt u uitvoeren `az dt show --dt-name <your-Azure-Digital-Twins-instance>` .

```azurecli 
az functionapp config appsettings set -g <your-resource-group> -n <your-App-Service-(function-app)-name> --settings "ADT_SERVICE_URL=https://<your-Azure-Digital-Twins-instance-hostname>"
```
### <a name="option-2-set-up-security-access-for-the-azure-function-app-using-azure-portal"></a>Optie 2: beveiligings toegang instellen voor de Azure function-app met behulp van Azure Portal

Met een door het systeem toegewezen beheerde identiteit kunnen Azure-bronnen worden geverifieerd bij Cloud Services (bijvoorbeeld Azure Key Vault) zonder dat referenties in code worden opgeslagen. Wanneer deze functie is ingeschakeld, kunnen alle benodigde machtigingen worden verleend via Azure Role-based-Access-Control. De levens cyclus van dit type beheerde identiteit is gekoppeld aan de levens cyclus van deze resource. Daarnaast kan elke resource (bijvoorbeeld virtuele machine) slechts één door het systeem toegewezen beheerde identiteit hebben.

Zoek in de [Azure Portal](https://portal.azure.com/)op de zoek balk naar _functie-app_ met de naam van de functie-app die u eerder hebt gemaakt. Selecteer de *functie-app* in de lijst. 

:::image type="content" source="media/how-to-create-azure-function/portal-search-for-functionapp.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

Selecteer in het venster functie-app _identiteit_ in de navigatie balk aan de linkerkant om beheerde identiteit in te scha kelen.
Schakel onder _systeem toegewezen_ tabblad de _status_ in op aan en _Sla_ deze op. Er wordt een pop-up weer gegeven om door het _systeem toegewezen beheerde identiteit in te scha kelen_.
Selecteer de knop _Ja_ . 

:::image type="content" source="media/how-to-create-azure-function/enable-system-managed-identity.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

U kunt in de meldingen controleren dat de functie is geregistreerd bij Azure Active Directory.

:::image type="content" source="media/how-to-create-azure-function/notifications-enable-managed-identity.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

Noteer ook de **object-id** die wordt weer gegeven op de pagina _identiteit_ , zoals deze wordt gebruikt in de volgende sectie.

:::image type="content" source="media/how-to-create-azure-function/object-id.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

### <a name="assign-access-roles-using-azure-portal"></a>Toegangs rollen toewijzen met behulp van Azure Portal

Selecteer de knop _Azure-roltoewijzingen_ , waarmee u de pagina met Azure-functie toewijzingen opent. Selecteer vervolgens _+ roltoewijzing toevoegen (preview)_.

:::image type="content" source="media/how-to-create-azure-function/add-role-assignments.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

Selecteer in het venster _roltoewijzing toevoegen (preview)_ dat wordt geopend:

* _Bereik_: resourcegroep
* _Abonnement_: Selecteer uw Azure-abonnement
* _Resource groep_: Selecteer de resource groep in de vervolg keuzelijst
* _Rol_: Selecteer de _Azure Digital apparaatdubbels-eigenaar (preview)_ in de vervolg keuzelijst

Sla uw gegevens op door de knop _Opslaan_ te selecteren.

:::image type="content" source="media/how-to-create-azure-function/add-role-assignment.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

### <a name="configure-application-settings-using-azure-portal"></a>Toepassings instellingen configureren met behulp van Azure Portal

U kunt de URL van uw Azure Digital Apparaatdubbels-exemplaar toegankelijk maken voor uw functie door een omgevings variabele in te stellen. Zie [*omgevings variabelen*](https://docs.microsoft.com/sandbox/functions-recipes/environment-variables)voor meer informatie. Toepassings instellingen worden weer gegeven als omgevings variabelen voor toegang tot het digitale apparaatdubbels-exemplaar. 

U hebt ADT_INSTANCE_URL nodig om een toepassings instelling te maken.

U kunt ADT_INSTANCE_URL ophalen door **_https://_** toe te voegen aan de hostnaam van uw exemplaar. In de Azure Portal kunt u de hostnaam van uw digitale apparaatdubbels-exemplaar vinden door te zoeken naar uw instantie in de zoek balk. Selecteer vervolgens _overzicht_ op de linkernavigatiebalk om de _hostnaam_weer te geven. Kopieer deze waarde om een toepassings instelling te maken.

:::image type="content" source="media/how-to-create-azure-function/adt-hostname.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

U kunt nu een toepassings instelling maken aan de hand van de volgende stappen:

* Zoek uw Azure-functie met behulp van de functie naam in de zoek balk en selecteer de functie in de lijst
* Selecteer _configuratie_ op de navigatie balk aan de linkerkant om een nieuwe toepassings instelling te maken
* Selecteer op het tabblad _Toepassings instellingen_ _+ nieuwe toepassings instelling_

:::image type="content" source="media/how-to-create-azure-function/search-for-azure-function.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

:::image type="content" source="media/how-to-create-azure-function/application-setting.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

In het venster dat wordt geopend, gebruikt u de waarde die u eerder hebt gekopieerd om een toepassings instelling te maken. \
_Naam_  : ADT_SERVICE_URL \
_Waarde_ : https://{Your-Azure-Digital-apparaatdubbels-hostname}

Selecteer _OK_ om een toepassings instelling te maken.

:::image type="content" source="media/how-to-create-azure-function/add-application-setting.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

U kunt de toepassings instellingen weer geven onder het veld _naam_ in de naam van de toepassing. Sla de toepassings instellingen vervolgens op door de knop _Opslaan_ te selecteren.

:::image type="content" source="media/how-to-create-azure-function/application-setting-save-details.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

Voor wijzigingen in de toepassings instellingen moet de toepassing opnieuw worden gestart. Selecteer _door gaan_ om de toepassing opnieuw op te starten.

:::image type="content" source="media/how-to-create-azure-function/save-application-setting.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

U kunt zien dat de toepassings instellingen worden bijgewerkt door het pictogram _meldingen_ te selecteren. Als uw toepassings instelling niet is gemaakt, kunt u opnieuw proberen een toepassings instelling toe te voegen door het bovenstaande proces te volgen.

:::image type="content" source="media/how-to-create-azure-function/notifications-update-web-app-settings.png" alt-text="Visual Studio: dialoog venster Nieuw project":::

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u de stappen voor het instellen van een Azure-functie gevolgd voor gebruik met Azure Digital Apparaatdubbels. Vervolgens kunt u uw Azure-functie abonneren op Event Grid om te Luis teren naar een eind punt. Dit eind punt kan het volgende zijn:
* Een Event Grid-eind punt dat is gekoppeld aan Azure Digital Apparaatdubbels om berichten te verwerken die afkomstig zijn van Azure Digital Apparaatdubbels zelf (zoals wijzigings berichten van eigenschappen, telemetrie-berichten die worden gegenereerd door [digitale apparaatdubbels](concepts-twins-graph.md) in het dubbele diagram of levenscyclus berichten)
* De IoT-systeem onderwerpen die door IoT Hub worden gebruikt voor het verzenden van telemetrie en andere gebeurtenissen voor apparaten
* Een Event Grid-eind punt dat berichten van andere services ontvangt

Bekijk vervolgens hoe u uw eenvoudige Azure function bouwt om IoT Hub gegevens op te nemen in azure Digital Apparaatdubbels:
* [*Instructies: telemetrie opnemen van IoT Hub*](how-to-ingest-iot-hub-data.md)