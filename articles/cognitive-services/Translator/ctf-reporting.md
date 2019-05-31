---
title: Collaborative Translation Framework (CTF) rapportage - Translator Text-API
titlesuffix: Azure Cognitive Services
description: Het gebruik Collaborative Translation Framework (CTF) rapportage.
services: cognitive-services
author: rajdeep-in
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: v-pawal
ms.openlocfilehash: 166c152828a91889d7d1d7eb6f8c03dac48172f5
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66389376"
---
# <a name="how-to-use-collaborative-translation-framework-ctf-reporting"></a>CTF-rapportage (Collaborative Translation Framework) gebruiken

> [!NOTE]
> Deze methode is afgeschaft. Het is niet beschikbaar in V3.0 van de Translator Text-API.
> 
> De gezamenlijke vertalingen Framework (CTF), eerder beschikbaar was voor V2.0 van de Translator Text-API is afgeschaft vanaf 1 februari 2018. De functies AddTranslation en AddTranslationArray toestaan dat gebruikers correcties via Collaborative Translation Framework inschakelen. Na 31 januari 2018 nieuwe zin inzendingen heeft niet geaccepteerd door deze twee functies en ontvangen gebruikers een foutbericht weergegeven. Deze functies zijn buiten gebruik gesteld en worden niet vervangen.

De Collaborative Translation Framework (CTF) rapportage-API retourneert statistieken en de daadwerkelijke inhoud in de store CTF. Deze API wijkt af van de methode GetTranslations() omdat deze:
* Retourneert de vertaalde inhoud en het totale aantal alleen uit uw account (appId of Azure Marketplace-account).
* Retourneert de vertaalde inhoud en het totale aantal zonder een overeenkomst van de bron-zin.
* Retourneert de automatische vertaling (automatische vertaling genoemd) niet.

## <a name="endpoint"></a>Eindpunt
Het eindpunt van de CTF rapportage-API https://api.microsofttranslator.com/v2/beta/ctfreporting.svc


## <a name="methods"></a>Methoden
| Name |    Description|
|:---|:---|
| GetUserTranslationCounts Method | Telt het aantal van de vertaling op die zijn gemaakt door de gebruiker ophalen. |
| Methode GetUserTranslations | Hiermee haalt u de vertalingen die zijn gemaakt door de gebruiker. |

Deze methoden kunnen u naar:
* De volledige reeks gebruiker vertalingen en correcties onder uw account-ID voor het downloaden ophalen.
* De lijst van de frequente inzenders verkrijgen. Zorg ervoor dat de juiste gebruikersnaam is opgegeven in AddTranslation().
* Bouw een gebruikersinterface (UI) waarmee uw vertrouwde gebruikers om te zien van alle beschikbare kandidaten, indien nodig, beperkt tot een gedeelte van uw site, op basis van het URI-voorvoegsel.

> [!NOTE]
> De beide methoden zijn relatief traag en duur. Het verdient aanbeveling om ze te gebruiken spaarzaam.

## <a name="getusertranslationcounts-method"></a>Methode GetUserTranslationCounts

Deze methode haalt de telling van vertalingen die zijn gemaakt door de gebruiker. De lijst met translation aantallen gegroepeerd op de uriPrefix, gebruiker, minRating en maxRating aanvraagparameters a.

**Syntaxis**

> [!div class="tabbedCodeSnippets"]
> ```cs
> UserTranslationCount[]GetUserTranslationCounts(
>            string appId,
>            string uriPrefix,
>            string from,
>            string to,
>            int? minRating,
>            int? maxRating,
>            string user,
>            string category
>            DateTime? minDateUtc,
>            DateTime? maxDateUtc,
>            int? skip,
>            int? take);
> ```

**Parameters**

| Parameter | Description |
|:---|:---|
| appId | **Vereiste** als de autorisatie-header wordt gebruikt, laat het veld appid leeg anders geeft u een tekenreeks met "Bearer" + "" + toegangstoken.|
| uriPrefix | **Optionele** een tekenreeks met het voorvoegsel van de URI van de vertaling.|
| from | **Optionele** een tekenreeks voor de taalcode van de vertaalde tekst. |
| tot | **Optionele** een tekenreeks voor de taalcode voor de omzetting van de tekst in.|
| minRating| **Optionele** een integerwaarde voor de beoordeling van de minimale kwaliteit voor de vertaalde tekst. Er is een geldige waarde tussen 10 en 10. De standaardwaarde is 1.|
| maxRating| **Optionele** een integerwaarde voor de beoordeling van de hoogste kwaliteit voor de vertaalde tekst. Er is een geldige waarde tussen 10 en 10. De standaardwaarde is 1.|
| user | **Optionele** een tekenreeks is die wordt gebruikt voor het filteren van het resultaat op basis van de afzender van de inzending. |
| category| **Optionele** een tekenreeks met de categorie of het domein van de vertaling. Deze parameter ondersteunt alleen de standaardoptie algemeen.|
| minDateUtc| **Optionele** de datum uit als u wilt ophalen van de vertalingen. De datum moet zich in de UTC-notatie. |
| maxDateUtc| **Optionele** de datum tot wanneer u wilt ophalen van de vertalingen. De datum moet zich in de UTC-notatie. |
| Overslaan| **Optionele** het aantal resultaten die u wilt overslaan op een pagina. Als u de overslaan op de eerste 20 rijen van de resultaten en de weergave in de 21e resultaatrecord wilt, bijvoorbeeld 20 voor deze parameter opgeven. De standaardwaarde voor deze parameter is 0.|
| toets maken | **Optionele** het aantal resultaten dat u wilt ophalen. Het maximumaantal van elke aanvraag is 100. De standaardwaarde is 100.|

> [!NOTE]
> De aanvraagparameters overslaan en los het probleem inschakelen paginering voor een groot aantal resultaatrecords.

**Retourwaarde**

De resultatenset bevat een matrix van de **UserTranslationCount**. Elke UserTranslationCount heeft de volgende elementen:

| Veld | Description |
|:---|:---|
| Count| Het aantal resultaten dat wordt opgehaald|
| Van | De source-taal|
| Classificatie| De classificatie die wordt toegepast door de indiener in de methodeaanroep AddTranslation()|
| Naar| De doeltaal|
| Uri| De URI in de methodeaanroep AddTranslation() toegepast|
| Gebruiker| Naam van de gebruiker|

**Uitzonderingen**

| Uitzondering | Message | Voorwaarden |
|:---|:---|:---|
| ArgumentOutOfRangeException | De parameter '**maxDateUtc**'moet groter zijn dan of gelijk zijn aan'**minDateUtc**'.| De waarde van de parameter **maxDateUtc** lager is dan de waarde van de parameter **minDateUtc**.|
| TranslateApiException | IP is hoger dan het quotum.| <ul><li>De limiet voor het aantal aanvragen per minuut is bereikt.</li><li>De aangevraagde grootte blijft beperkt 10000 tekens.</li><li>Elk uur en een dagelijkse quotum beperkt het aantal tekens dat de Microsoft Translator-API accepteert.</li></ul>|
| TranslateApiException | AppId is hoger dan het quotum.| De toepassings-ID overschreden de Uurlijkse of dagelijkse quotum.|

> [!NOTE]
> Het quotum wordt aangepast om ervoor te zorgen eerlijkheid tussen alle gebruikers van de service.

**Codevoorbeelden voor weergave op GitHib**
* [C#](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslationcounts-example-csharp.md)
* [PHP](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslationcounts-example-php.md)

## <a name="getusertranslations-method"></a>Methode GetUserTranslations

Deze methode haalt de vertaling op die zijn gemaakt door de gebruiker. Het biedt dat de vertalingen gegroepeerd op de uriPrefix uit, en gebruikers-, en minRating maxRating parameters van de aanvraag.

**Syntaxis**

> [!div class="tabbedCodeSnippets"]
> ```cs
> UserTranslation[] GetUserTranslations (
>             string appId,
>             string uriPrefix,
>             string from,
>             string to,
>             int? minRating,
>             int? maxRating,
>             string user,
>             string category
>             DateTime? minDateUtc,
>             DateTime? maxDateUtc,
>             int? skip,
>             int? take);
> ```

**Parameters**

| Parameter | Description |
|:---|:---|
| appId | **Vereiste** als de autorisatie-header wordt gebruikt, laat het veld appid leeg anders geeft u een tekenreeks met "Bearer" + "" + toegangstoken.|
| uriPrefix| **Optionele** een tekenreeks met het voorvoegsel van de URI van de vertaling.|
| from| **Optionele** een tekenreeks voor de taalcode van de vertaalde tekst.|
| tot| **Optionele** een tekenreeks voor de taalcode voor de omzetting van de tekst in.|
| minRating| **Optionele** een integerwaarde voor de beoordeling van de minimale kwaliteit voor de vertaalde tekst. Er is een geldige waarde tussen 10 en 10. De standaardwaarde is 1.|
| maxRating| **Optionele** een integerwaarde voor de beoordeling van de hoogste kwaliteit voor de vertaalde tekst. Er is een geldige waarde tussen 10 en 10. De standaardwaarde is 1.|
| user| **Optioneel. Een tekenreeks is die wordt gebruikt voor het filteren van het resultaat op basis van de afzender van het verzenden van de**|
| category| **Optionele** een tekenreeks met de categorie of het domein van de vertaling. Deze parameter ondersteunt alleen de standaardoptie algemeen.|
| minDateUtc| **Optionele** de datum uit als u wilt ophalen van de vertalingen. De datum moet zich in de UTC-notatie.|
| maxDateUtc| **Optionele** de datum tot wanneer u wilt ophalen van de vertalingen. De datum moet zich in de UTC-notatie.|
| Overslaan| **Optionele** het aantal resultaten die u wilt overslaan op een pagina. Als u de overslaan op de eerste 20 rijen van de resultaten en de weergave in de 21e resultaatrecord wilt, bijvoorbeeld 20 voor deze parameter opgeven. De standaardwaarde voor deze parameter is 0.|
| toets maken| **Optionele** het aantal resultaten dat u wilt ophalen. Het maximumaantal van elke aanvraag is 100. De standaardwaarde is 50.|

> [!NOTE]
> De aanvraagparameters overslaan en los het probleem inschakelen paginering voor een groot aantal resultaatrecords.

**Retourwaarde**

De resultatenset bevat een matrix van de **UserTranslation**. Elke UserTranslation heeft de volgende elementen:

| Veld | Description |
|:---|:---|
| CreatedDateUtc| De aanmaakdatum van de vermelding die met behulp van AddTranslation()|
| Van| De source-taal|
| OriginalText| De tekst source-taal die wordt gebruikt bij het indienen van de aanvraag|
|Classificatie |De classificatie die wordt toegepast door de indiener in de methodeaanroep AddTranslation()|
|Naar|    De doeltaal|
|TranslatedText|    De vertaling verzonden in de methodeaanroep AddTranslation()|
|Uri|   De URI in de methodeaanroep AddTranslation() toegepast|
|Gebruiker   |Naam van de gebruiker|

**Uitzonderingen**

| Uitzondering | Message | Voorwaarden |
|:---|:---|:---|
| ArgumentOutOfRangeException | De parameter '**maxDateUtc**'moet groter zijn dan of gelijk zijn aan'**minDateUtc**'.| De waarde van de parameter **maxDateUtc** lager is dan de waarde van de parameter **minDateUtc**.|
| TranslateApiException | IP is hoger dan het quotum.| <ul><li>De limiet voor het aantal aanvragen per minuut is bereikt.</li><li>De aangevraagde grootte blijft beperkt 10000 tekens.</li><li>Elk uur en een dagelijkse quotum beperkt het aantal tekens dat de Microsoft Translator-API accepteert.</li></ul>|
| TranslateApiException | AppId is hoger dan het quotum.| De toepassings-ID overschreden de Uurlijkse of dagelijkse quotum.|

> [!NOTE]
> Het quotum wordt aangepast om ervoor te zorgen eerlijkheid tussen alle gebruikers van de service.

**Codevoorbeelden voor weergave op GitHib**
* [C#](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslations-example-csharp.md)
* [PHP](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslations-example-php.md)
