---
title: Translator Text-API methode vertalen
titleSuffix: Azure Cognitive Services
description: Gebruik de methode Translator-API-vertalen.
services: cognitive-services
author: rajdeep-in
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 02/01/2019
ms.author: v-pawal
ms.openlocfilehash: be61d8932288b9a6b2cc96e53d3630124ec0f610
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66389754"
---
# <a name="translator-text-api-30-translate"></a>Translator Text-API 3.0: Translate

Vertaalt tekst.

## <a name="request-url"></a>Aanvraag-URL

Verzendt een `POST` aanvragen:

```HTTP
https://api.cognitive.microsofttranslator.com/translate?api-version=3.0
```

## <a name="request-parameters"></a>Aanvraagparameters

Parameters van de aanvraag doorgegeven aan de query-tekenreeks zijn:

<table width="100%">
  <th width="20%">Queryparameter</th>
  <th>Description</th>
  <tr>
    <td>api-version</td>
    <td><em>Vereiste parameter</em>.<br/>De versie van de API die is aangevraagd door de client. De waarde moet liggen <code>3.0</code>.</td>
  </tr>
  <tr>
    <td>from</td>
    <td><em>Optionele parameter</em>.<br/>Hiermee geeft u de taal van de invoertekst. Welke talen zijn beschikbaar voor de omzetting van door het opzoeken <a href="./v3-0-languages.md">ondersteunde talen</a> met behulp van de <code>translation</code> bereik. Als de <code>from</code> parameter niet wordt opgegeven, wordt automatische taaldetectie toegepast om te bepalen van de source-taal.</td>
  </tr>
  <tr>
    <td>tot</td>
    <td><em>Vereiste parameter</em>.<br/>Hiermee geeft u de taal van de uitvoertekst. De doeltaal moet een van de <a href="./v3-0-languages.md">ondersteunde talen</a> opgenomen in de <code>translation</code> bereik. Gebruik bijvoorbeeld <code>to=de</code> te vertalen in Duitsland.<br/>Het is mogelijk te vertalen in meerdere talen tegelijkertijd door te herhalen van de parameter in de query-tekenreeks. Gebruik bijvoorbeeld <code>to=de&to=it</code> te vertalen in het Duits en Italiaans.</td>
  </tr>
  <tr>
    <td>textType</td>
    <td><em>Optionele parameter</em>.<br/>Hiermee wordt aangegeven of de tekst wordt vertaald tekst zonder opmaak of HTML-tekst. HTML-code moet een juist opgemaakte en volledige-element. Mogelijke waarden zijn: <code>plain</code> (standaard) of <code>html</code>.</td>
  </tr>
  <tr>
    <td>category</td>
    <td><em>Optionele parameter</em>.<br/>Een tekenreeks op te geven de categorie (domein) van de vertaling. Deze parameter wordt gebruikt om op te halen van vertalingen van een aangepast systeem die zijn gebouwd met <a href="../customization.md">aangepaste Translator</a>. De categorie-ID van uw aangepaste Translator toevoegen <a href="https://docs.microsoft.com/azure/cognitive-services/translator/custom-translator/how-to-create-project#view-project-details">projectdetails</a> aangepast voor deze parameter gebruiken uw geïmplementeerde systeem. Standaardwaarde: <code>general</code>.</td>
  </tr>
  <tr>
    <td>ProfanityAction</td>
    <td><em>Optionele parameter</em>.<br/>Hiermee geeft u op hoe profanities moeten worden behandeld in vertalingen. Mogelijke waarden zijn: <code>NoAction</code> (standaard), <code>Marked</code> of <code>Deleted</code>. Zie voor meer informatie over manieren om te behandelen grof taalgebruik, <a href="#handle-profanity">grof taalgebruik verwerking</a>.</td>
  </tr>
  <tr>
    <td>profanityMarker</td>
    <td><em>Optionele parameter</em>.<br/>Hiermee geeft u op hoe profanities moet worden gemarkeerd met de vertaling. Mogelijke waarden zijn: <code>Asterisk</code> (standaard) of <code>Tag</code>. Zie voor meer informatie over manieren om te behandelen grof taalgebruik, <a href="#handle-profanity">grof taalgebruik verwerking</a>.</td>
  </tr>
  <tr>
    <td>includeAlignment</td>
    <td><em>Optionele parameter</em>.<br/>Hiermee geeft u op of u wilt de projectie van de uitlijning van tekst naar vertaalde tekst bevatten. Mogelijke waarden zijn: <code>true</code> of <code>false</code> (standaard). </td>
  </tr>
  <tr>
    <td>includeSentenceLength</td>
    <td><em>Optionele parameter</em>.<br/>Hiermee geeft u op of u wilt opnemen zin grenzen voor de invoertekst en de vertaalde tekst. Mogelijke waarden zijn: <code>true</code> of <code>false</code> (standaard).</td>
  </tr>
  <tr>
    <td>suggestedFrom</td>
    <td><em>Optionele parameter</em>.<br/>Hiermee geeft u een alternatieve taal als de taal van de ingevoerde tekst kan niet worden geïdentificeerd. Automatische taaldetectie wordt toegepast wanneer de <code>from</code> parameter wordt weggelaten. Als de detectie is mislukt, de <code>suggestedFrom</code> taal wordt verondersteld.</td>
  </tr>
  <tr>
    <td>fromScript</td>
    <td><em>Optionele parameter</em>.<br/>Hiermee geeft u het script uit de invoertekst.</td>
  </tr>
  <tr>
    <td>toScript</td>
    <td><em>Optionele parameter</em>.<br/>Hiermee geeft u het script van de vertaalde tekst.</td>
  </tr>
  <tr>
    <td>allowFallback</td>
    <td><em>Optionele parameter</em>.<br/>Hiermee geeft u op dat de service mag terugvallen op een algemene systeem wanneer een aangepast systeem niet bestaat. Mogelijke waarden zijn: <code>true</code> (standaard) of <code>false</code>.<br/><br/><code>allowFallback=false</code> Hiermee geeft u de vertaling moet alleen gebruiken voor systemen die zijn getraind voor de <code>category</code> opgegeven door de aanvraag. Als een vertaling voor de taal die X Y vereist via een taal pivot E, klikt u vervolgens alle systemen in de keten-koppeling (X -> E- en E -> Y) moet worden aangepast en hebben dezelfde categorie. Als er geen systeem met een specifieke categorie wordt gevonden, wordt de aanvraag een 400-statuscode geretourneerd. <code>allowFallback=true</code> Hiermee geeft u op dat de service mag terugvallen op een algemene systeem wanneer een aangepast systeem niet bestaat.
</td>
  </tr>
</table> 

Aanvraagheaders zijn onder andere:

<table width="100%">
  <th width="20%">Headers</th>
  <th>Description</th>
  <tr>
    <td>Verificatie of meerdere berichtkoppen</td>
    <td><em>Vereiste aanvraagheader</em>.<br/>Zie <a href="https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication">beschikbare opties voor verificatie</a>.</td>
  </tr>
  <tr>
    <td>Content-Type</td>
    <td><em>Vereiste aanvraagheader</em>.<br/>Hiermee geeft u het type inhoud van de nettolading. Mogelijke waarden zijn: <code>application/json</code>.</td>
  </tr>
  <tr>
    <td>Content-Length</td>
    <td><em>Vereiste aanvraagheader</em>.<br/>De lengte van de aanvraagtekst.</td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td><em>Optioneel</em>.<br/>Een client gegenereerde GUID voor het aanduiden van de aanvraag. U kunt deze header weglaten als u de trace-ID opnemen in de querytekenreeks met behulp van een queryparameter met de naam <code>ClientTraceId</code>.</td>
  </tr>
</table> 

## <a name="request-body"></a>Aanvraagbody

De hoofdtekst van de aanvraag is een JSON-matrix. Elk matrixelement is een JSON-object met de tekenreekseigenschap van een met de naam `Text`, die staat voor de tekenreeks die moet worden vertaald.

```json
[
    {"Text":"I would really like to drive your car around the block a few times."}
]
```

Er gelden de volgende beperkingen:

* De matrix kan maximaal 100 elementen hebben.
* De volledige tekst is opgenomen in de aanvraag kan niet langer zijn dan 5000 tekens inclusief spaties.

## <a name="response-body"></a>De hoofdtekst van antwoord

Een geslaagde reactie is een JSON-matrix met één resultaat voor elke tekenreeks in de invoermatrix. Een resultaatobject bevat de volgende eigenschappen:

  * `detectedLanguage`: Een object met een beschrijving van de gedetecteerde taal via de volgende eigenschappen:

      * `language`: Een tekenreeks die de code van de gedetecteerde taal.

      * `score`: Een float-waarde die aangeeft van het vertrouwen in het resultaat. De score tussen 0 en een en een lage score geeft aan dat een lage vertrouwen.

    De `detectedLanguage` eigenschap is alleen aanwezig in het resultaatobject wanneer automatische taaldetectie wordt aangevraagd.

  * `translations`: Een matrix van de resultaten van de vertaling. De grootte van de matrix komt overeen met het aantal doeltalen opgegeven via de `to` queryparameter. Elk element in de matrix omvat:

    * `to`: Een tekenreeks die de taalcode van de doel-taal.

    * `text`: Een tekenreeks waarin de vertaalde tekst.

    * `transliteration`: Een object waarin de vertaalde tekst in het script dat is opgegeven door de `toScript` parameter.

      * `script`: Een tekenreeks die het doel-script op te geven.   

      * `text`: Een tekenreeks waarin de vertaalde tekst in het doel-script.

    De `transliteration` object wordt niet meegeleverd als vele niet plaats vindt.

    * `alignment`: Een object met een enkele tekenreeks-eigenschap met de naam `proj`, dat is toegewezen invoertekst vertaalde tekst. De uitlijning van de informatie is alleen beschikbaar wanneer de aanvraagparameter `includeAlignment` is `true`. Uitlijning wordt geretourneerd als een string-waarde van de volgende indeling: `[[SourceTextStartIndex]:[SourceTextEndIndex]–[TgtTextStartIndex]:[TgtTextEndIndex]]`.  De puntkomma's gescheiden begin en einde index, het streepje worden gescheiden van de talen en ruimte worden gescheiden van de woorden. Een woord kan uitlijnen met nul, één of meerdere woorden in de andere taal en het is mogelijk dat de uitgelijnde woorden niet-aaneengesloten. Als er geen uitlijning informatie beschikbaar is, wordt de uitlijning van element niet leeg zijn. Zie [uitlijning informatie](#obtain-alignment-information) voor een voorbeeld en beperkingen.

    * `sentLen`: Een object zin grenzen in de invoer- en tekst worden geretourneerd.

      * `srcSentLen`: Een matrix met gehele getallen vertegenwoordigt de lengte van de zinnen in de invoertekst. De lengte van de matrix is het aantal zinnen en de waarden worden de lengte van elke zin.

      * `transSentLen`:  Een matrix met gehele getallen vertegenwoordigt de lengte van de zinnen in de vertaalde tekst. De lengte van de matrix is het aantal zinnen en de waarden worden de lengte van elke zin.

    Zin grenzen zijn alleen opgenomen wanneer de aanvraagparameter `includeSentenceLength` is `true`.

  * `sourceText`: Een object met een enkele tekenreeks-eigenschap met de naam `text`, waardoor de ingevoerde tekst in het standaardscript van de source-taal. `sourceText` de eigenschap is alleen beschikbaar wanneer de invoer wordt uitgedrukt in een script dat is niet de gebruikelijke script voor de taal. Bijvoorbeeld, als de invoer zijn Arabisch vervolgens die zijn geschreven in Latijns schrift `sourceText.text` zou worden dezelfde Arabische tekst geconverteerd naar Arabische script.

Voorbeeld van JSON-antwoorden vindt u in de [voorbeelden](#examples) sectie.

## <a name="response-headers"></a>Antwoordheaders

<table width="100%">
  <th width="20%">Headers</th>
  <th>Description</th>
    <tr>
    <td>X-RequestId</td>
    <td>De waarde die wordt gegenereerd door de service voor het identificeren van de aanvraag. Het wordt gebruikt voor het oplossen van problemen.</td>
  </tr>
  <tr>
    <td>X-MT-systeem</td>
    <td>Hiermee geeft u het type dat is gebruikt voor de vertaling voor elke 'to'-taal voor vertaling aangevraagd. De waarde is een door komma's gescheiden lijst met tekenreeksen. Elke tekenreeks geeft aan dat een type:<br/><ul><li>Aangepast - aanvraag bevat een aangepast systeem en ten minste één aangepaste system is gebruikt tijdens de conversie.</li><li>Team - alle andere aanvragen</li></td>
  </tr>
</table> 

## <a name="response-status-codes"></a>Antwoord-statuscodes

Hier volgen de mogelijke HTTP-statuscodes die een aanvraag retourneert. 

<table width="100%">
  <th width="20%">Statuscode</th>
  <th>Description</th>
  <tr>
    <td>200</td>
    <td>Geslaagd.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>Een van de queryparameters is ontbreekt of is ongeldig. De parameters van de juiste aanvraag voordat opnieuw wordt geprobeerd.</td>
  </tr>
  <tr>
    <td>401</td>
    <td>De aanvraag kan niet worden geverifieerd. Controleer of de referenties opgegeven en is geldig zijn.</td>
  </tr>
  <tr>
    <td>403</td>
    <td>De aanvraag is niet gemachtigd. Controleer het foutbericht voor meer informatie. Dit betekent meestal dat alle gratis vertalingen voorzien van een proefabonnement zijn verbruikt.</td>
  </tr>
  <tr>
    <td>408</td>
    <td>De aanvraag kan niet worden uitgevoerd omdat een bron ontbreekt. Controleer het foutbericht voor meer informatie. Wanneer u een aangepaste <code>category</code>, dit betekent meestal dat de aangepaste vertaalsysteem nog niet beschikbaar is voor het verzenden van aanvragen. De aanvraag moet opnieuw worden uitgevoerd na een wachttijd (bijvoorbeeld 1 minuut).</td>
  </tr>
  <tr>
    <td>429</td>
    <td>De server heeft de aanvraag geweigerd omdat de client aanvraaglimieten heeft overschreden.</td>
  </tr>
  <tr>
    <td>500</td>
    <td>Er is een onverwachte fout opgetreden. Als de fout zich blijft voordoen, rapporteren met: datum en tijd van de fout, aanvraag-id van de reactieheader <code>X-RequestId</code>, en de client-id van aanvraagheader <code>X-ClientTraceId</code>.</td>
  </tr>
  <tr>
    <td>503</td>
    <td>De server is tijdelijk niet beschikbaar. De aanvraag opnieuw. Als de fout zich blijft voordoen, rapporteren met: datum en tijd van de fout, aanvraag-id van de reactieheader <code>X-RequestId</code>, en de client-id van aanvraagheader <code>X-ClientTraceId</code>.</td>
  </tr>
</table> 

Als er een fout optreedt, wordt de aanvraag ook een JSON-fout antwoord retourneren. De foutcode is een 6-cijferige numerieke combineren het 3-cijferige HTTP-statuscode gevolgd door een getal 3 cijfers en verder categoriseren van de fout. Veelvoorkomende foutcodes kunnen u vinden op de [v3 Translator Text-API-verwijzingspagina](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#errors). 

## <a name="examples"></a>Voorbeelden

### <a name="translate-a-single-input"></a>Een enkele invoer vertalen

In dit voorbeeld laat zien hoe een enkele zin in het Engels naar vereenvoudigd Chinees vertalen.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

Hoofdtekst van het antwoord is:

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"}
        ]
    }
]
```

De `translations` matrix bevat een element, dat zorgt voor de omzetting van de los stukje van tekst in de invoer.

### <a name="translate-a-single-input-with-language-auto-detection"></a>Een enkele invoer met automatische taaldetectie vertalen

In dit voorbeeld laat zien hoe een enkele zin in het Engels naar vereenvoudigd Chinees vertalen. De aanvraag heeft niet de invoer taal opgeven. Automatische detectie van de source-taal wordt in plaats daarvan gebruikt.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

Hoofdtekst van het antwoord is:

```
[
    {
        "detectedLanguage": {"language": "en", "score": 1.0},
        "translations":[
            {"text": "你好, 你叫什么名字？", "to": "zh-Hans"}
        ]
    }
]
```
Het antwoord is vergelijkbaar met het antwoord uit het vorige voorbeeld. Omdat automatische taaldetectie is aangevraagd, wordt ook informatie over de taal voor de invoertekst gedetecteerd in het antwoord bevat. 

### <a name="translate-with-transliteration"></a>Met vele vertalen

Laten we het vorige voorbeeld uitbreiden door vele toe te voegen. De volgende aanvraag gevraagd of u een Chinees vertaling die in Latijns schrift zijn geschreven.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=zh-Hans&toScript=Latn" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

Hoofdtekst van het antwoord is:

```
[
    {
        "detectedLanguage":{"language":"en","score":1.0},
        "translations":[
            {
                "text":"你好, 你叫什么名字？",
                "transliteration":{"script":"Latn", "text":"nǐ hǎo , nǐ jiào shén me míng zì ？"},
                "to":"zh-Hans"
            }
        ]
    }
]
```

Het resultaat van de vertaling bevat nu een `transliteration` getoond, hetgeen resulteert in de vertaalde tekst met behulp van Latijnse tekens.

### <a name="translate-multiple-pieces-of-text"></a>Meerdere soorten tekst vertalen

Vertalen van meerdere tekenreeksen in één keer is gewoon een kwestie van het opgeven van een matrix met tekenreeksen in de aanvraagtekst.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}, {'Text':'I am fine, thank you.'}]"
```

---

Hoofdtekst van het antwoord is:

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"}
        ]
    },            
    {
        "translations":[
            {"text":"我很好，谢谢你。","to":"zh-Hans"}
        ]
    }
]
```

### <a name="translate-to-multiple-languages"></a>Vertalen in meerdere talen

In dit voorbeeld laat zien hoe de dezelfde invoer voor verschillende talen in één verzoek vertalen.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans&to=de" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

Hoofdtekst van het antwoord is:

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"},
            {"text":"Hallo, was ist dein Name?","to":"de"}
        ]
    }
]
```

### <a name="handle-profanity"></a>Ingang grof taalgebruik

De Translator-service blijft normaal grof taalgebruik dat aanwezig is in de bron in de vertaling. De mate van grof taalgebruik en de context waarmee woorden grof verschillen tussen culturen en als gevolg hiervan de mate van grof taalgebruik in de taal van het doel kan worden versterkt of verminderd.

Als u wilt vermijden grof taalgebruik in de vertaling, ongeacht de aanwezigheid van grof taalgebruik in de brontekst, kunt u de grof taalgebruik filteroptie kunt gebruiken. De optie kunt u kiezen of u zien grof taalgebruik verwijderd wilt, of u markeren profanities met de juiste tags wilt (zodat u de optie voor het toevoegen van uw eigen na verwerking), of u geen actie ondernomen wilt. De geaccepteerde waarden van `ProfanityAction` zijn `Deleted`, `Marked` en `NoAction` (standaard).

<table width="100%">
  <th width="20%">ProfanityAction</th>
  <th>Bewerking</th>
  <tr>
    <td><code>NoAction</code></td>
    <td>Dit is de standaardinstelling. Grof taalgebruik geeft van bron naar doel.<br/><br/>
    <strong>Voorbeeld van de bron (Japans)</strong>: 彼はジャッカスです。<br/>
    <strong>Voorbeeld van de vertaling (Engels)</strong>: Hij is een jackass.
    </td>
  </tr>
  <tr>
    <td><code>Deleted</code></td>
    <td>Grof woorden worden verwijderd uit de uitvoer zonder vervanging.<br/><br/>
    <strong>Voorbeeld van de bron (Japans)</strong>: 彼はジャッカスです。<br/>
    <strong>Voorbeeld van de vertaling (Engels)</strong>: Hij is een.
    </td>
  </tr>
  <tr>
    <td><code>Marked</code></td>
    <td>Grof woorden vervangen door een markering in de uitvoer. De markering is afhankelijk van de <code>ProfanityMarker</code> parameter.<br/><br/>
Voor <code>ProfanityMarker=Asterisk</code>, grof woorden vervangen door <code>***</code>:<br/>
    <strong>Voorbeeld van de bron (Japans)</strong>: 彼はジャッカスです。<br/>
    <strong>Voorbeeld van de vertaling (Engels)</strong>: Hij is een \* \* \*.<br/><br/>
Voor <code>ProfanityMarker=Tag</code>, grof woorden worden omringd door de XML-tags &lt;grof taalgebruik&gt; en &lt;/profanity&gt;:<br/>
    <strong>Voorbeeld van de bron (Japans)</strong>: 彼はジャッカスです。<br/>
    <strong>Voorbeeld van de vertaling (Engels)</strong>: Hij is een &lt;grof taalgebruik&gt;jackass&lt;/profanity&gt;.
  </tr>
</table> 

Bijvoorbeeld:

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de&profanityAction=Marked" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'This is a freaking good idea.'}]"
```

---

Dit geeft als resultaat:

```
[
    {
        "translations":[
            {"text":"Das ist eine *** gute Idee.","to":"de"}
        ]
    }
]
```

Vergelijken met:

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de&profanityAction=Marked&profanityMarker=Tag" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'This is a freaking good idea.'}]"
```

---

Deze laatste aanvraag als resultaat:

```
[
    {
        "translations":[
            {"text":"Das ist eine <profanity>verdammt</profanity> gute Idee.","to":"de"}
        ]
    }
]
```

### <a name="translate-content-with-markup-and-decide-whats-translated"></a>Inhoud met opmaak vertaald en bepalen wat er vertaald

Het is gebruikelijk om te vertalen, waaronder markup zoals uit een HTML-pagina-inhoud of inhoud uit een XML-document. Query-parameter opnemen `textType=html` tijdens het vertalen van inhoud met tags. Bovendien is het soms nuttig voor het uitsluiten van specifieke inhoud van de vertaling. U kunt het kenmerk `class=notranslate` om op te geven van inhoud die moet blijven in de oorspronkelijke taal. In het volgende voorbeeld wordt de inhoud van de eerste `div` element wordt niet omgezet, terwijl de inhoud in de tweede `div` element wordt omgezet.

```
<div class="notranslate">This will not be translated.</div>
<div>This will be translated. </div>
```

Hier volgt een voorbeeld van een aanvraag om te illustreren.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans&textType=html" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'<div class=\"notranslate\">This will not be translated.</div><div>This will be translated.</div>'}]"
```

---

Het antwoord is:

```
[
    {
        "translations":[
            {"text":"<div class=\"notranslate\">This will not be translated.</div><div>这将被翻译。</div>","to":"zh-Hans"}
        ]
    }
]
```

### <a name="obtain-alignment-information"></a>Uitlijning informatie verkrijgen

Voor het ontvangen van uitlijningsgegevens, geef `includeAlignment=true` op de query-tekenreeks.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=fr&includeAlignment=true" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'The answer lies in machine translation.'}]"
```

---

Het antwoord is:

```
[
    {
        "translations":[
            {
                "text":"La réponse se trouve dans la traduction automatique.",
                "to":"fr",
                "alignment":{"proj":"0:2-0:1 4:9-3:9 11:14-11:19 16:17-21:24 19:25-40:50 27:37-29:38 38:38-51:51"}
            }
        ]
    }
]
```

De van uitlijningsgegevens begint met `0:2-0:1`, wat inhoudt dat de eerste drie tekens in de brontekst (`The`) toewijzen aan de eerste twee tekens in de vertaalde tekst (`La`).

Houd rekening met de volgende beperkingen:

* Uitlijning wordt alleen geretourneerd voor een subset van de taal-paren:
  - in het Engels in een andere taal;
  - vanuit een andere taal in het Engels, met uitzondering van vereenvoudigd Chinees, traditioneel Chinees en Lets naar het Engels;
  - in het Japans, Koreaans of naar Koreaans naar het Japans.
* U ontvangt geen uitlijning als een blik vertaling is de zin. Voorbeeld van een voorgedefinieerde vertaling is 'Dit is een test', 'Fijn dat' en andere zinnen hoge frequentie.

### <a name="obtain-sentence-boundaries"></a>Zin grenzen verkrijgen

Geef voor het ontvangen van informatie over de zinlengte van de in de brontekst en de vertaalde tekst, `includeSentenceLength=true` op de query-tekenreeks.

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=fr&includeSentenceLength=true" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'The answer lies in machine translation. The best machine translation technology cannot always provide translations tailored to a site or users like a human. Simply copy and paste a code snippet anywhere.'}]"
```

---

Het antwoord is:

```
[
    {
        "translations":[
            {
                "text":"La réponse se trouve dans la traduction automatique. La meilleure technologie de traduction automatique ne peut pas toujours fournir des traductions adaptées à un site ou des utilisateurs comme un être humain. Il suffit de copier et coller un extrait de code n’importe où.",
                "to":"fr",
                "sentLen":{"srcSentLen":[40,117,46],"transSentLen":[53,157,62]}
            }
        ]
    }
]
```

### <a name="translate-with-dynamic-dictionary"></a>Vertalen met dynamische woordenlijst

Als u de vertaling die u wilt toepassen op een woord of woordgroep al kent, kunt u deze opgeven als aantekeningen in de aanvraag. Alleen is de dynamische-woordenlijst voor samenstellingen, zoals de namen van de juiste en product veilig.

De volgende syntaxis maakt gebruik van de markering om op te geven.

``` 
<mstrans:dictionary translation=”translation of phrase”>phrase</mstrans:dictionary>
```

Bijvoorbeeld, kunt u de Engelse zin "het woord wordomatic is een dictionary-vermelding." Het woord behouden _wordomatic_ in de vertaling, de aanvraag te verzenden:

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.'}]"
```

Het resultaat is:

```
[
    {
        "translations":[
            {"text":"Das Wort "wordomatic" ist ein Wörterbucheintrag.","to":"de"}
        ]
    }
]
```

Deze functie werkt op dezelfde manier met `textType=text` of met `textType=html`. De functie moet spaarzaam worden gebruikt. De juiste en veel beter manier voor het aanpassen van de vertaling is met behulp van aangepaste Translator. Aangepaste Translator maakt volledig gebruik van context en statistische kansen. Als u zich kunt veroorloven trainingsgegevens waarin uw werk- of woordgroep in context maken of hebt, kunt u veel betere resultaten krijgt. [Meer informatie over aangepaste Translator](../customization.md).
 





