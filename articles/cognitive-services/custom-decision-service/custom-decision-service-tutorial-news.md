---
title: 'Zelfstudie: Persoonlijke aanpassingen van artikel - Custom Decision Service'
titlesuffix: Azure Cognitive Services
description: Een zelfstudie om artikelen aan persoonlijke voorkeuren aan te passen voor contextuele besluitvorming.
services: cognitive-services
author: slivkins
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: tutorial
ms.date: 05/08/2018
ms.author: slivkins
ROBOTS: NOINDEX
ms.openlocfilehash: f7eafed9db25fba904d98ddea652671dc45aa01d
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707222"
---
# <a name="tutorial-article-personalization-for-contextual-decision-making"></a>Zelfstudie: Persoonlijke aanpassingen van het artikel voor contextuele besluitvorming

Deze zelfstudie richt zich op het aanpassen van de selectie artikelen op de eerste pagina van een website. Custom Decision Service is bijvoorbeeld van invloed op *meerdere* lijsten met artikelen op een eerste pagina. Die pagina kan bijvoorbeeld een nieuwswebsite zijn die alleen over politiek en sport gaat. Er worden drie gerangschikte lijsten met artikelen weergegeven: politiek, sport en recent.

## <a name="applications-and-action-sets"></a>Toepassingen en actiesets

Lees hier hoe het scenario in het framework past. Laten we uitgaan van drie toepassingen, één voor elke lijst die wordt geoptimaliseerd: app-politiek, app-sport en app-recent. Er zijn twee actiesets om de mogelijke artikelen voor elke toepassing op te geven: één voor politiek en één voor sport. De actieset voor app-recent ontstaat automatisch door samenvoeging van de andere twee sets.

> [!TIP]
> Actiesets kunnen worden gedeeld door toepassingen in Custom Decision Service.

## <a name="prepare-action-set-feeds"></a>Actiesetfeeds voorbereiden

Custom Decision Service verbruikt actiesets via RSS- of Atom-feeds die door de klant worden geleverd. U levert twee feeds: één voor politiek en één voor sport. Stel dat ze worden aangeleverd vanuit `http://www.domain.com/feeds/<feed-name>`.

Met elke feed wordt een lijst met artikelen geleverd. In RSS wordt elk artikel als volgt opgegeven door een `<item>`-element:

```xml
<rss version="2.0"><channel>
   <item>
      <title><![CDATA[article title]]></title>
      <link>"article url"</link>
      <pubDate>publication date</pubDate>
    </item>
</channel></rss>
```

De volgorde van de artikelen is belangrijk. Die bepaalt namelijk de standaardvolgorde. Dit is de beste inschatting van hoe de artikelen moeten worden gerangschikt. De standaardrangschikking wordt vervolgens gebruikt voor een vergelijking van de prestaties op het dashboard.

Zie de [API-verwijzing](custom-decision-service-api-reference.md#action-set-api-customer-provided) voor meer informatie over de feedindeling.

## <a name="register-a-new-app"></a>Een nieuwe app registreren

1. Meld u aan met uw [Microsoft-account](https://portal.ds.microsoft.com/). Klik op het lint op **Mijn portal**.

2. Als u een nieuwe toepassing wilt registreren, klikt u op de knop **Nieuwe app**.

    ![Custom Decision Service-portal](./media/custom-decision-service-tutorial/portal.png)

3. Typ een unieke naam voor uw toepassing in het vak **App-id**. Als deze naam al door een andere klant wordt gebruikt, wordt u gevraagd een andere app-id te kiezen. Schakel het selectievakje **Geavanceerd** in en voer de [verbindingsreeks](../../storage/common/storage-configure-connection-string.md) voor uw Azure Storage-account in. Normaal gesproken gebruikt u hetzelfde opslagaccount voor al uw toepassingen.

    ![Dialoogvenster Nieuwe app](./media/custom-decision-service-tutorial/new-app-dialog.png)

    Nadat u in dit scenario alle drie de toepassingen hebt geregistreerd, worden ze weergegeven:

    ![Lijst met apps](./media/custom-decision-service-tutorial/apps.png)

    U kunt later terugkeren naar deze lijst door op de knop **Apps** te klikken.

4. Geef in het dialoogvenster **Nieuwe app** een actiefeed op. Actiefeeds kunnen ook worden opgegeven door te klikken op de knop **Feeds** en vervolgens op de knop **Nieuwe feed**. Typ een **naam** voor de nieuwe feed, typ de **URL** waarvandaan wordt geleverd en typ de **vernieuwingstijd**. Met de vernieuwingstijd wordt opgegeven hoe vaak Custom Decision Service de feed moet vernieuwen.

    ![Dialoogvenster Nieuwe feed](./media/custom-decision-service-tutorial/new-feed-dialog.png)

    Actiefeeds kunnen worden gebruikt door elke willekeurige app, ongeacht waar deze wordt opgegeven. Nadat u beide actiefeeds in een scenario hebt opgegeven, worden ze weergegeven:

    ![Lijst met feeds](./media/custom-decision-service-tutorial/feeds.png)

    U kunt later terugkeren naar deze lijst door op de knop **Feeds** te klikken.

## <a name="use-the-apis"></a>De API's gebruiken

Custom Decision Service rangschikt artikelen via de Ranking-API. Voeg voor het gebruik van deze API de volgende code in de HTML-kop van de eerste pagina in:

```html
<!-- Define the "callback function" to render UI -->
<script> function callback(data) { … } </script>

<!--  call Ranking API after callback() is defined, separately for each app -->
<script src="https://ds.microsoft.com/api/v2/app-politics/rank/feed-politics" async></script>
<script src="https://ds.microsoft.com/api/v2/app-sports/rank/feed-sports" async></script>
<script src="https://ds.microsoft.com/api/v2/app-recent/rank/feed-politics/feed-sports" async></script>
<!-- NB: action feeds for 'app-recent' are listed one after another. -->
```

Het HTTP-antwoord van de Ranking-API is een tekenreeks in JSONP-indeling. Voor app-politiek bijvoorbeeld ziet de tekenreeks er als volgt uit:

```json
callback({
   "ranking":[{"id":"url1"}, {"id":"url2"}, {"id":"url3"}],
   "eventId":"<opaque event string>",
   "appId":"app-politics",
   "actionSets":[{"id":"feed-politics","lastRefresh":"date"}] });
```

Deze tekenreeks wordt vervolgens in de browser uitgevoerd als een aanroep naar de functie `callback()`. Het argument `data` in de functie `callback()` bevat de app-id en de weer te geven rangschikking van URL's. `callback()` moet met name `data.appId` gebruiken om onderscheid te maken tussen de drie toepassingen. `eventId` wordt intern door Custom Decision Service gebruikt om de geleverde rangschikking te koppelen aan de bijbehorende klik, indien van toepassing.

> [!TIP]
> `callback()` kan elke actiefeed op nieuwheid controleren met behulp van het veld `lastRefresh`. Als een bepaalde feed niet nieuw genoeg is, kan `callback()` de geleverde rangschikking negeren, deze feed rechtstreeks aanroepen en de standaardrangschikking gebruiken die wordt geleverd door de feed.

Zie de [API-naslag](custom-decision-service-api-reference.md) voor meer informatie over specificaties en extra opties die door de Ranking-API worden geleverd.

De best gewaardeerde artikelen van de gebruiker worden geretourneerd door de Beloning-API aan te roepen. Wanneer een van deze best gewaardeerde artikelen wordt ontvangen, moet de volgende code worden aangeroepen op de eerste pagina:

```javascript
$.ajax({
    type: "POST",
    url: '//ds.microsoft.com/api/v2/<appId>/reward/<eventId>',
    contentType: "application/json" })
```

Bij het gebruik van `appId` en `eventId` in de code voor de verwerking van klikken moet zorgvuldig te werk worden gegaan. U kunt de functie `callback()` bijvoorbeeld als volgt implementeren:

```javascript
function callback(data) {
    $.map(data.ranking, function (element) {
        //custom rendering function given content info
        render({appId:data.appId,
                article:element,
                onClick: element.id != data.rewardAction ? null :
                   function() {
                       $.ajax({
                       type: "POST",
                       url: '//ds.microsoft.com/api/v2/' + data.appId + '/reward/' + data.eventId,
                       contentType: "application/json" })}
                });
}}
```

In dit voorbeeld implementeert u de functie `render()` om een bepaald artikel voor een bepaalde toepassing weer te geven. Met deze functie worden de app-id en het artikel (in de indeling van de Ranking-API) ingevoerd. De parameter `onClick` is de functie die moet worden aangeroepen vanuit `render()` om een klik te verwerken. Er wordt gecontroleerd of de klik zich in het belangrijkste tijdvak bevindt. Vervolgens wordt de Beloning-API met de juiste app-id en gebeurtenis-id aangeroepen.

## <a name="next-steps"></a>Volgende stappen

* Raadpleeg de [API-naslag](custom-decision-service-api-reference.md) voor meer informatie over de geleverde functionaliteit.
