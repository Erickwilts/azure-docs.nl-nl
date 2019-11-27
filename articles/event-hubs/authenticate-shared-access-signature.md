---
title: Toegang tot Azure Event Hubs verifiëren met hand tekeningen voor gedeelde toegang
description: Dit artikel laat u zien hoe u de toegang tot Event Hubs-resources kunt verifiëren met behulp van hand tekeningen voor gedeelde toegang.
services: event-hubs
ms.service: event-hubs
documentationcenter: ''
author: spelluru
ms.topic: conceptual
ms.date: 11/26/2019
ms.author: spelluru
ms.openlocfilehash: d17026dba26b3c1cb846d60967180c29563c425d
ms.sourcegitcommit: a678f00c020f50efa9178392cd0f1ac34a86b767
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2019
ms.locfileid: "74545592"
---
# <a name="authenticate-access-to-event-hubs-resources-using-shared-access-signatures-sas"></a>Toegang tot Event Hubs resources verifiëren met behulp van Shared Access signatures (SAS)
Shared Access Signature (SAS) biedt u gedetailleerde controle over het type toegang dat u verleent aan de clients die de Shared Access Signature hebben. Hier volgen enkele van de besturings elementen die u in een SAS kunt instellen: 

- Het interval waarover de SA'S geldig zijn, met inbegrip van de begin tijd en verloop tijd.
- De machtigingen die door de SAS worden verleend. Een SAS voor een Event Hubs naam ruimte kan bijvoorbeeld de machtiging Luis teren verlenen, maar niet de machtiging verzenden.
- Alleen clients die geldige referenties kunnen gegevens verzenden naar een event hub.
- Een client kan geen andere client imiteren.
- Een Rouge-client kan worden geblokkeerd voor het verzenden van gegevens naar een Event Hub.

Dit artikel heeft betrekking op het verifiëren van de toegang tot Event Hubs-resources met behulp van SAS. Zie [dit artikel](authorize-access-shared-access-signature.md)voor meer informatie over het **verlenen** van toegang tot Event hubs-resources met behulp van SAS. 

> [!NOTE]
> Micro soft raadt u aan om Azure AD-referenties, indien mogelijk, te gebruiken als een beveiligings best practice, in plaats van de hand tekeningen voor gedeelde toegang te gebruiken, wat eenvoudiger kan worden aangetast. Hoewel u Shared Access signatures (SAS) kunt blijven gebruiken om nauw keurige toegang te verlenen tot uw Event Hubs-resources, biedt Azure AD soort gelijke mogelijkheden zonder dat u SAS-tokens hoeft te beheren, of om u zorgen te maken over het intrekken van een SAS.
> 
> Zie [toegang tot Event hubs toestaan met Azure AD](authorize-access-azure-active-directory.md)voor meer informatie over Azure AD-integratie in azure Event hubs. 


## <a name="configuring-for-sas-authentication"></a>Configureren voor SAS-verificatie
U kunt de Event hubs-verificatie regel voor gedeelde toegang op een Event Hubs naam ruimte of een entiteit (Event Hub instantie of Kafka onderwerp in een Event Hubs voor de naam ruimte Kafka ingeschakeld) configureren. Het configureren van een regel voor gedeelde toegangs autorisatie voor een consumenten groep wordt momenteel niet ondersteund, maar u kunt regels die zijn geconfigureerd op een naam ruimte of entiteit gebruiken om de toegang tot de Consumer groep te beveiligen. 

De volgende afbeelding laat zien hoe de autorisatie regels van toepassing zijn op de voorbeeld entiteiten. 

![Autorisatie regel configureren](./media/authenticate-shared-access-signature/configure-sas-authorization-rule.png)

In dit voor beeld heeft de voor beeld-Event Hubs naam ruimte (ExampleNamespace) twee entiteiten: EH1 en topic1. De autorisatie regels worden gedefinieerd op het niveau van de entiteit en ook op het niveau van de naam ruimte.  

De manageRuleNS-, sendRuleNS-en listenRuleNS-autorisatie regels zijn van toepassing op zowel Event Hub instantie EH1 als onderwerp T1. De autorisatie regels listenRule-eh en sendRule-eh zijn alleen van toepassing op de Event Hub instantie EH1 en sendRuleT autorisatie regel is alleen van toepassing op onderwerp topic1. 

Wanneer u sendRuleNS-verificatie regel gebruikt, kunnen client toepassingen worden verzonden naar EH1 en topic1. Wanneer sendRuleT-verificatie regel wordt gebruikt, wordt nauw keurigere toegang tot topic1 afgedwongen. Daarom kunnen client toepassingen die gebruikmaken van deze regel voor toegang, nu niet verzenden naar EH1, maar alleen op topic1.

## <a name="generate-a-shared-access-signature-token"></a>Een Shared Access Signature-token genereren 
Elke client die toegang heeft tot de naam van een verificatie regel naam en een van de handtekening sleutels kan een SAS-token genereren. Het token wordt gegenereerd door een teken reeks in de volgende indeling te maken:

- `se`: token verloop datum direct. Geheel getal dat seconden weergeeft sinds de epoche 00:00:00 UTC op 1 januari 1970 (UNIX-epoche) wanneer het token verloopt
- `skn`: de naam van de autorisatie regel, de naam van de SAS-sleutel.
- `sr`: de URI van de bron waartoe toegang wordt verkregen.
- `sig` – hand tekening.

De hand tekening: de SHA-256-hash wordt berekend op basis van de resource-URI (bereik zoals beschreven in de vorige sectie) en de teken reeks representatie van het token verloop, gescheiden door CRLF.

De hash-berekening ziet er ongeveer uit als de volgende pseudo code en retourneert een 256-bits/32-byte hash-waarde. 

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

Het token bevat de niet-gehashte waarden, zodat de ontvanger de hash opnieuw kan berekenen met dezelfde para meters, zodat u kunt controleren of de uitgever in het bezit is van een geldige ondertekeningssleutel.

De resource-URI is de volledige URI van de Service Bus resource waarmee de toegang wordt geclaimd. Bijvoorbeeld http://<namespace>. servicebus.windows.net/<entityPath> of `sb://<namespace>.servicebus.windows.net/<entityPath>;` dat wil zeggen `http://contoso.servicebus.windows.net/eventhubs/eh1`.

De URI moet een percentage zijn gecodeerd.

De gedeelde toegangs autorisatie regel die wordt gebruikt voor ondertekening moet worden geconfigureerd voor de entiteit die door deze URI wordt opgegeven of door een van de hiërarchische bovenliggende items. Bijvoorbeeld `http://contoso.servicebus.windows.net/eventhubs/eh1` of `http://contoso.servicebus.windows.net` in het vorige voor beeld.

Een SAS-token is geldig voor alle resources die worden voorafgegaan door de <resourceURI> gebruikt in de teken reeks voor hand tekening.

> [!NOTE]
> U genereert een toegangs token voor Event Hubs met behulp van het beleid voor gedeelde toegang. Zie voor meer informatie [verificatie beleid voor gedeelde toegang](authorize-access-shared-access-signature.md#shared-access-authorization-policies).

### <a name="generating-a-signaturetoken-from-a-policy"></a>Een hand tekening (token) genereren op basis van een beleid 
In de volgende sectie ziet u hoe u een SAS-token genereert met behulp van een handtekening beleid voor gedeelde toegang

#### <a name="nodejs"></a>Node.js

```javascript
function createSharedAccessToken(uri, saName, saKey) { 
    if (!uri || !saName || !saKey) { 
            throw "Missing required parameter"; 
        } 
    var encoded = encodeURIComponent(uri); 
    var now = new Date(); 
    var week = 60*60*24*7;
    var ttl = Math.round(now.getTime() / 1000) + week;
    var signature = encoded + '\n' + ttl; 
    var signatureUTF8 = utf8.encode(signature); 
    var hash = crypto.createHmac('sha256', saKey).update(signatureUTF8).digest('base64'); 
    return 'SharedAccessSignature sr=' + encoded + '&sig=' +  
        encodeURIComponent(hash) + '&se=' + ttl + '&skn=' + saName; 
```

#### <a name="java"></a>JAVA

```java
private static String GetSASToken(String resourceUri, String keyName, String key)
  {
      long epoch = System.currentTimeMillis()/1000L;
      int week = 60*60*24*7;
      String expiry = Long.toString(epoch + week);

      String sasToken = null;
      try {
          String stringToSign = URLEncoder.encode(resourceUri, "UTF-8") + "\n" + expiry;
          String signature = getHMAC256(key, stringToSign);
          sasToken = "SharedAccessSignature sr=" + URLEncoder.encode(resourceUri, "UTF-8") +"&sig=" +
                  URLEncoder.encode(signature, "UTF-8") + "&se=" + expiry + "&skn=" + keyName;
      } catch (UnsupportedEncodingException e) {

          e.printStackTrace();
      }

      return sasToken;
  }


public static String getHMAC256(String key, String input) {
    Mac sha256_HMAC = null;
    String hash = null;
    try {
        sha256_HMAC = Mac.getInstance("HmacSHA256");
        SecretKeySpec secret_key = new SecretKeySpec(key.getBytes(), "HmacSHA256");
        sha256_HMAC.init(secret_key);
        Encoder encoder = Base64.getEncoder();

        hash = new String(encoder.encode(sha256_HMAC.doFinal(input.getBytes("UTF-8"))));

    } catch (InvalidKeyException e) {
        e.printStackTrace();
    } catch (NoSuchAlgorithmException e) {
        e.printStackTrace();
   } catch (IllegalStateException e) {
        e.printStackTrace();
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
    }

    return hash;
}
```
#### <a name="php"></a>PHP

```php
function generateSasToken($uri, $sasKeyName, $sasKeyValue) 
{ 
    $targetUri = strtolower(rawurlencode(strtolower($uri))); 
    $expires = time();  
    $expiresInMins = 60; 
    $week = 60*60*24*7;
    $expires = $expires + $week; 
    $toSign = $targetUri . "\n" . $expires; 
    $signature = rawurlencode(base64_encode(hash_hmac('sha256',             
     $toSign, $sasKeyValue, TRUE))); 
    
    $token = "SharedAccessSignature sr=" . $targetUri . "&sig=" . $signature . "&se=" . $expires .      "&skn=" . $sasKeyName; 
    return $token; 
}
```

#### <a name="c"></a>C#

```csharp
private static string createToken(string resourceUri, string keyName, string key)
{
    TimeSpan sinceEpoch = DateTime.UtcNow - new DateTime(1970, 1, 1);
    var week = 60 * 60 * 24 * 7;
    var expiry = Convert.ToString((int)sinceEpoch.TotalSeconds + week);
    string stringToSign = HttpUtility.UrlEncode(resourceUri) + "\n" + expiry;
    HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(key));
    var signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
    var sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}", HttpUtility.UrlEncode(resourceUri), HttpUtility.UrlEncode(signature), expiry, keyName);
    return sasToken;
}
```

## <a name="authenticating-event-hubs-publishers-with-sas"></a>Event Hubs uitgevers verifiëren met SAS 
Een gebeurtenisuitgever definieert een virtuele-eindpunt voor een event hub. De uitgever kan alleen worden gebruikt om berichten te verzenden naar een Event Hub en geen berichten te ontvangen.

Normaal gesproken een event hub maakt gebruik van een uitgever per client. Alle berichten die worden verzonden naar een van de uitgevers van een event hub zijn in de wachtrij in die event hub. Uitgevers maken nauw keurig toegangs beheer mogelijk.

Elke client Event Hubs is een unieke token, dat is geüpload naar de client toegewezen. De tokens worden zodanig gegenereerd dat elk uniek token toegang verleent tot een andere unieke Uitgever. Een client die een token bevat, kan slechts naar één uitgever en geen andere uitgever verzenden. Als meerdere clients hetzelfde token delen, deelt elk-client de uitgever.

Alle tokens worden toegewezen met SAS-sleutels. Normaal gesproken zijn alle tokens ondertekend met dezelfde sleutel. Clients zijn niet op de hoogte van de sleutel, waardoor clients geen productie tokens meer kunnen produceren. Clients werken op dezelfde tokens totdat ze verlopen.

U moet bijvoorbeeld een regel voor het verzenden van een autorisatie definiëren om autorisatie regels op te geven die zijn beperkt tot het verzenden/publiceren naar Event Hubs. Dit kan worden gedaan op het niveau van de naam ruimte of een nauw keurigere Scope geven aan een bepaalde entiteit (Event hubs-instantie of een onderwerp). Een client of een toepassing die met een dergelijke gedetailleerde toegang is gericht, wordt Event Hubs Publisher genoemd. Voer hiertoe de volgende stappen uit:

1. Maak een SAS-sleutel voor de entiteit die u wilt publiceren om de **Verzend** Scope hierop toe te wijzen. Zie [Shared Access Authorization Policies](authorize-access-shared-access-signature.md#shared-access-authorization-policies)(Engelstalig) voor meer informatie.
2. Genereer een SAS-token met een verloop tijd voor een specifieke uitgever met behulp van de sleutel die is gegenereerd in stap 1-4.

    ```csharp
    var sasToken = SharedAccessSignatureTokenProvider.GetPublisherSharedAccessSignature(
                new Uri("Service-Bus-URI"),
                "eventub-name",
                "publisher-name",
                "sas-key-name",
                "sas-key",
                TimeSpan.FromMinutes(30));
    ```
3. Geef het token op voor de Publisher-client, die alleen kan verzenden naar de entiteit en de uitgever die het token toegang verleent.

    Zodra het token is verlopen, verliest de client de toegang tot het verzenden/publiceren naar de entiteit. 


> [!NOTE]
> Hoewel het niet wordt aangeraden, is het mogelijk om apparaten met tokens te voorzien die toegang verlenen tot een Event Hub of een naam ruimte. Elk apparaat dat dit token bevat, kan berichten rechtstreeks naar die Event Hub verzenden. Daarnaast wordt het apparaat kan niet worden op de blokkeringslijst naar die event hub verzendt.
> 
> Het wordt altijd aanbevolen specifieke en nauw keurige bereiken te geven.

> [!IMPORTANT]
> Zodra de tokens zijn gemaakt, moet elke client is ingericht met een eigen unieke token.
>
> Wanneer de client gegevens naar een Event Hub verzendt, wordt de aanvraag met het token gecodeerd. Om te voorkomen dat een aanvaller niet kan worden afgeluisterd en stelen van het token, moet de communicatie tussen de client en de event hub vindt plaats via een versleuteld kanaal.
> 
> Als een token wordt gestolen door een aanvaller, kan de aanvaller de wie een token is gestolen client imiteren. Als u een Publisher op de zwarte lijst maakt, wordt die client onbruikbaar weer gegeven totdat er een nieuw token wordt ontvangen dat gebruikmaakt van een andere uitgever.


## <a name="authenticating-event-hubs-consumers-with-sas"></a>Event Hubs consumenten verifiëren met SAS 
Voor het verifiëren van back-end-toepassingen die gebruikmaken van de gegevens die zijn gegenereerd door Event Hubs producenten, moeten Event Hubs-token verificatie zijn clients hebben om de machtigingen **beheren** of de **Luister** bevoegdheid te hebben toegewezen aan de Event hubs naam ruimte of het event hub exemplaar of onderwerp. Gegevens worden uit Event Hubs gebruikt met behulp van consumenten groepen. Hoewel SAS-beleid u een nauw keurig bereik biedt, wordt dit bereik alleen gedefinieerd op het niveau van de entiteit en niet op het niveau van de consument. Dit betekent dat de bevoegdheden die zijn gedefinieerd op het niveau van de naam ruimte of het Event Hub-exemplaar-of onderwerps niveau, worden toegepast op de Consumer groepen van die entiteit.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen:

- [Autoriseren met SAS](authenticate-shared-access-signature.md)
- [Autoriseren met behulp van Role-Base Access Control (RBAC)](authenticate-shared-access-signature.md)
- [Meer informatie over Event Hubs](event-hubs-about.md)

Raadpleeg de volgende verwante artikelen:

- [Aanvragen voor Azure Event Hubs verifiëren vanuit een toepassing met behulp van Azure Active Directory](authenticate-application.md)
- [Een beheerde identiteit verifiëren met Azure Active Directory om toegang te krijgen tot Event Hubs bronnen](authenticate-managed-identity.md)
- [Toegang tot Event Hubs resources autoriseren met behulp van Azure Active Directory](authorize-access-azure-active-directory.md)
- [Toegang tot Event Hubs resources autoriseren met behulp van hand tekeningen voor gedeelde toegang](authorize-access-shared-access-signature.md)