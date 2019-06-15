---
title: Azure Monitor HTTP-gegevensverzamelaar-API | Microsoft Docs
description: U kunt de API van Azure Monitor HTTP Data Collector POST-JSON-gegevens toevoegen aan een Log Analytics-werkruimte van een willekeurige client die de REST-API kunt aanroepen. Dit artikel wordt beschreven hoe u de API en bevat voorbeelden van hoe u gegevens publiceert met behulp van verschillende programmeertalen.
services: log-analytics
documentationcenter: ''
author: bwren
manager: jwhit
editor: ''
ms.assetid: a831fd90-3f55-423b-8b20-ccbaaac2ca75
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/02/2019
ms.author: bwren
ms.openlocfilehash: 0f5a996d68c80fd9b1f55a36de37579ea245d99d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64922788"
---
# <a name="send-log-data-to-azure-monitor-with-the-http-data-collector-api-public-preview"></a>Logboekgegevens verzenden naar Azure Monitor met de HTTP Data Collector-API (preview-versie)
Dit artikel leest u hoe de API HTTP Data Collector gebruikt om te verzenden van logboekgegevens naar Azure Monitor van een REST-API-client.  Dit wordt beschreven hoe u gegevens die zijn verzameld door het script of een toepassing opmaken, opnemen in een aanvraag en die aanvraag heeft geautoriseerd door Azure Monitor.  Voorbeelden zijn bedoeld voor PowerShell, C# en Python.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

> [!NOTE]
> De API van Azure Monitor HTTP Data Collector is in openbare preview.

## <a name="concepts"></a>Concepten
De HTTP Data Collector-API kunt u logboekgegevens naar een Log Analytics-werkruimte in Azure Monitor verzenden vanaf een willekeurige client die een REST-API kunt aanroepen.  Dit wordt mogelijk een runbook in Azure Automation die management verzamelt mogelijk gegevens vanuit Azure of een andere cloud, of het een ander beheersysteem die gebruikmaakt van Azure Monitor om te consolideren en analyseren van logboekgegevens.

Alle gegevens in de Log Analytics-werkruimte wordt opgeslagen als een record met een bepaald type.  U uw gegevens worden verzonden naar de API HTTP Data Collector als meerdere records in de JSON-indeling.  Wanneer u de gegevens hebt ingediend, wordt een afzonderlijke record gemaakt in de opslagplaats voor elke record in de nettolading van de aanvraag.


![Overzicht van de HTTP-gegevensverzamelaar](media/data-collector-api/overview.png)



## <a name="create-a-request"></a>Een aanvraag maken
Voor het gebruik van de API HTTP Data Collector, maakt u een POST-aanvraag met de gegevens worden verzonden in JavaScript Object Notation (JSON).  De volgende drie tabellen worden de kenmerken die vereist voor elke aanvraag zijn. Elk kenmerk in meer detail later in dit artikel wordt beschreven.

### <a name="request-uri"></a>Aanvraag-URI
| Kenmerk | Eigenschap |
|:--- |:--- |
| Methode |POST |
| URI |https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01 |
| Type inhoud |application/json |

### <a name="request-uri-parameters"></a>Aanvraag-URI-parameters
| Parameter | Description |
|:--- |:--- |
| CustomerID |De unieke id voor de Log Analytics-werkruimte. |
| Resource |De naam van de API-resource: / api/logs. |
| API-versie |De versie van de API voor gebruik met deze aanvraag. Het is momenteel, 2016-04-01. |

### <a name="request-headers"></a>Aanvraagheaders
| Header | Description |
|:--- |:--- |
| Autorisatie |De autorisatie-handtekening. Later in dit artikel, kunt u lezen over het maken van een HMAC-SHA256-header. |
| Log-Type |Geef het recordtype van de gegevens die wordt verzonden. De maximale grootte voor deze parameter is 100 tekens. |
| x-ms-date |De datum waarop de aanvraag is verwerkt, in de RFC 1123-indeling. |
| x-ms-AzureResourceId | Resource-ID van de Azure-resource de gegevens moet worden gekoppeld. Dit vult de [_ResourceId](log-standard-properties.md#_resourceid) eigenschap en maakt het mogelijk de gegevens moeten worden opgenomen in [resource-georiënteerde](manage-access.md#access-modes) query's. Als dit veld niet wordt opgegeven, worden de gegevens niet worden opgenomen in de resource-georiënteerde query's. |
| Time-gegenereerd-veld | De naam van een veld in de gegevens die met het tijdstempel van het gegevensitem. Als u een veld opgeven en vervolgens de inhoud ervan worden gebruikt voor **TimeGenerated**. Als dit veld niet wordt opgegeven, de standaardwaarde voor **TimeGenerated** is de tijd die het bericht wordt opgenomen. De inhoud van het berichtenveld diende de ISO 8601-notatie jjjj-MM-ddTHH. |

## <a name="authorization"></a>Autorisatie
Elk verzoek aan de API van Azure Monitor HTTP Data Collector moet een autorisatie-header bevatten. Als u wilt een aanvraag worden geverifieerd, moet u zich aanmelden met de aanvraag met de primaire of de secundaire sleutel voor de werkruimte die de aanvraag wordt uitgevoerd. Geeft de handtekening die als onderdeel van de aanvraag.   

Hier volgt de indeling voor de autorisatie-header:

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

*Werkruimte-id* is de unieke id voor de Log Analytics-werkruimte. *Handtekening* is een [HMAC Hash-based Message Authentication Code ()](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) die is samengesteld uit de aanvraag en vervolgens worden berekend met behulp van de [algoritme SHA256](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx). Klik, codeert u deze met behulp van Base64-codering.

Deze indeling gebruiken om te coderen de **SharedKey** handtekening tekenreeks:

```
StringToSign = VERB + "\n" +
                  Content-Length + "\n" +
               Content-Type + "\n" +
                  x-ms-date + "\n" +
                  "/api/logs";
```

Hier volgt een voorbeeld van een handtekening-tekenreeks:

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

Wanneer u de handtekening-tekenreeks hebt, deze met behulp van de HMAC-SHA256-algoritme op de UTF-8-gecodeerde tekenreeks te coderen en vervolgens het resultaat als Base64 coderen. Gebruik deze indeling:

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

De voorbeelden in de volgende secties hebben voorbeeldcode voor het maken van een autorisatie-header.

## <a name="request-body"></a>Aanvraagbody
De hoofdtekst van het bericht moet zich in JSON. Er moet een of meer records met de eigenschap naam / waarde-paren opnemen in deze indeling:

```json
[
    {
        "property 1": "value1",
        "property 2": "value2",
        "property 3": "value3",
        "property 4": "value4"
    }
]
```

U kunt meerdere records samen in één aanvraag batch met behulp van de volgende indeling hebben. Alle records moet hetzelfde type zijn.

```json
[
    {
        "property 1": "value1",
        "property 2": "value2",
        "property 3": "value3",
        "property 4": "value4"
    },
    {
        "property 1": "value1",
        "property 2": "value2",
        "property 3": "value3",
        "property 4": "value4"
    }
]
```

## <a name="record-type-and-properties"></a>Recordtype en eigenschappen
Wanneer u gegevens via de API van Azure Monitor HTTP Data Collector verzendt definieert u een aangepaste recordtype. U kunt gegevens op dit moment kan niet schrijven naar bestaande typen bronrecords vermeld die zijn gemaakt door andere gegevenstypen en -oplossingen. Azure Monitor leest de binnenkomende gegevens en maakt vervolgens eigenschappen die overeenkomen met de gegevenstypen van de waarden die u invoert.

Elke aanvraag aan de Data Collector-API moet bevatten een **Logboektype** -header met de naam van het recordtype. Het achtervoegsel **_CL** wordt automatisch toegevoegd aan de naam die u invoert voor deze onderscheidt van andere typen logboeken als een aangepast logboek. Bijvoorbeeld, als u de naam invoeren **MyNewRecordType**, maakt u een record met het type Azure Monitor **MyNewRecordType_CL**. Dit zorgt ervoor dat er geen conflicten tussen typenamen die door de gebruiker heeft gemaakt en die in de huidige of toekomstige Microsoft-oplossingen hebt verzonden zijn.

Voor het identificeren van het gegevenstype van een eigenschap, voegt Azure Monitor het achtervoegsel aan de naam van de eigenschap. Als een eigenschap een null-waarde bevat, wordt de eigenschap is niet opgenomen in die record. Deze tabel bevat de eigenschap gegevenstype en de bijbehorende achtervoegsel:

| Eigenschap gegevenstype | Suffix |
|:--- |:--- |
| String |_s |
| Boolean |_b |
| Double |_d |
| Datum/tijd |_t |
| GUID |_g |

Het gegevenstype dat Azure Monitor voor elke eigenschap gebruikt is afhankelijk van of het recordtype voor de nieuwe record al bestaat.

* Als het recordtype niet bestaat nog, wordt een nieuw wachtwoord met behulp van het type JSON Deductie om te bepalen van het gegevenstype voor elke eigenschap voor de nieuwe record gemaakt in Azure Monitor.
* Als het recordtype bestaat, wordt Azure Monitor probeert te maken van een nieuwe record op basis van bestaande eigenschappen. Als de gegevens voor een eigenschap in de nieuwe record komt niet overeen met en kan niet worden geconverteerd naar het type van een bestaande, of als de record een eigenschap die niet bestaat bevat, maakt u een nieuwe eigenschap met het achtervoegsel van de relevante Azure Monitor.

Deze vermelding inzending zou bijvoorbeeld een record maken met drie eigenschappen **number_d**, **boolean_b**, en **string_s**:

![Voorbeeldrecord 1](media/data-collector-api/record-01.png)

Als u deze volgende vermelding, klik vervolgens met alle waarden die zijn opgemaakt als tekenreeksen hebt ingediend, worden de eigenschappen zou niet wijzigen. Deze waarden kunnen worden geconverteerd naar bestaande gegevenstypen:

![Voorbeeldrecord 2](media/data-collector-api/record-02.png)

Maar als u deze volgende verzending vervolgens gemaakt, maakt Azure Monitor de eigenschappen van nieuwe **boolean_d** en **string_d**. Deze waarden kunnen niet worden geconverteerd:

![Voorbeeldrecord 3](media/data-collector-api/record-03.png)

Als u de volgende vermelding, klikt u vervolgens verzonden voordat het recordtype is gemaakt, Azure Monitor een record wilt maken met drie eigenschappen **gunstig**, **boolean_s**, en **string_s**. In deze post is elk van de oorspronkelijke waarden opgemaakt als een tekenreeks:

![Voorbeeldrecord 4](media/data-collector-api/record-04.png)

## <a name="reserved-properties"></a>Gereserveerde eigenschappen
De volgende eigenschappen zijn gereserveerd en mag niet worden gebruikt in een aangepaste recordtype. U ontvangt een foutmelding als de nettolading een van de namen van deze eigenschappen bevat.

- tenant

## <a name="data-limits"></a>Gegevenslimieten
Er zijn enkele beperkingen om de gegevens in de gegevensverzameling van Azure Monitor API geplaatst.

* Maximaal 30 MB per post naar Azure Monitor Data Collector-API. Dit is een maximale grootte voor een enkel bericht. Als de gegevens van één die boeken groter is dan 30 MB, moet u de gegevens tot een kleinere grootte segmenten splitsen en ze gelijktijdig te verzenden.
* Maximum van 32 KB-limiet voor veldwaarden. Als de veldwaarde groter dan 32 KB is, wordt de gegevens worden afgekapt.
* Aanbevolen maximumaantal velden voor een bepaald type is 50. Dit is een limiet van bruikbaarheid en zoeken ervaring perspectief.  
* Een tabel in een werkruimte voor Log Analytics biedt alleen ondersteuning voor maximaal 500 kolommen (aangeduid als een veld in dit artikel). 
* Het maximum aantal tekens in voor de naam van de kolom is 500.

## <a name="return-codes"></a>Retourcodes
De HTTP-statuscode 200 betekent dat de aanvraag is ontvangen voor verwerking. Hiermee wordt aangegeven dat de bewerking is voltooid.

Deze tabel bevat de volledige reeks statuscodes die de service kan worden geretourneerd:

| Code | Status | Foutcode | Description |
|:--- |:--- |:--- |:--- |
| 200 |OK | |De aanvraag is geaccepteerd. |
| 400 |Ongeldig verzoek |InactiveCustomer |De werkruimte is gesloten. |
| 400 |Ongeldig verzoek |InvalidApiVersion |De API-versie die u hebt opgegeven is niet herkend door de service. |
| 400 |Ongeldig verzoek |InvalidCustomerId |De opgegeven werkruimte-ID is ongeldig. |
| 400 |Ongeldig verzoek |InvalidDataFormat |Ongeldige JSON is ingediend. De antwoordtekst kan bevatten meer informatie over de fout op te lossen. |
| 400 |Ongeldig verzoek |InvalidLogType |Het logboektype opgegeven ingesloten speciale tekens of cijfers. |
| 400 |Ongeldig verzoek |MissingApiVersion |De API-versie is niet opgegeven. |
| 400 |Ongeldig verzoek |MissingContentType |Het inhoudstype is niet opgegeven. |
| 400 |Ongeldig verzoek |MissingLogType |De vereiste waarde Logboektype is niet opgegeven. |
| 400 |Ongeldig verzoek |UnsupportedContentType |Het inhoudstype is niet ingesteld op **application/json**. |
| 403 |Verboden |InvalidAuthorization |De service kan niet verifiëren van de aanvraag. Controleer of de sleutel van de werkruimte-ID en verbinding geldig zijn. |
| 404 |Niet gevonden | | De opgegeven URL is onjuist, of de aanvraag is te groot. |
| 429 |Te veel aanvragen | | De service ondervindt een groot aantal gegevens uit uw account. Probeer de aanvraag later opnieuw. |
| 500 |Interne serverfout |UnspecifiedError |De service is een interne fout opgetreden. Probeer de aanvraag. |
| 503 |Service is niet beschikbaar |ServiceUnavailable |De service is momenteel niet beschikbaar is om aanvragen te ontvangen. Probeer uw aanvraag. |

## <a name="query-data"></a>Querygegevens
Query uitvoeren op gegevens verzonden door de Azure Monitor HTTP Data Collector-API, zoeken naar records met **Type** die gelijk is aan de **LogType** waarde die u hebt opgegeven, met het achtervoegsel **_CL**. Als u gebruikt bijvoorbeeld **MyCustomLog**, zou u alle records geretourneerd `MyCustomLog_CL`.

## <a name="sample-requests"></a>Van voorbeeldaanvragen
In de volgende secties vindt u voorbeelden van hoe u gegevens naar de API van Azure Monitor HTTP Data Collector indienen met behulp van verschillende programmeertalen.

Voer deze stappen om de variabelen voor de autorisatie-header voor elk voorbeeld:

1. Ga naar uw Log Analytics-werkruimte in de Azure-portal.
2. Selecteer **geavanceerde instellingen** en vervolgens **verbonden bronnen**.
2. Aan de rechterkant van **werkruimte-ID**, selecteer het kopieerpictogram en plak de-ID als de waarde van de **klant-ID** variabele.
3. Aan de rechterkant van **primaire sleutel**, selecteer het kopieerpictogram en plak de-ID als de waarde van de **gedeelde sleutel** variabele.

U kunt ook de variabelen voor de Logboektype en JSON-gegevens wijzigen.

### <a name="powershell-sample"></a>Voorbeeld van PowerShell
```powershell
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify the name of the record type that you'll be creating
$LogType = "MyRecordType"

# You can use an optional field to specify the timestamp from the data. If the time field is not specified, Azure Monitor assumes the time is the message ingestion time
$TimeStampField = ""


# Create two records with the same set of properties to create
$json = @"
[{  "StringValue": "MyString1",
    "NumberValue": 42,
    "BooleanValue": true,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "9909ED01-A74C-4874-8ABF-D2678E3AE23D"
},
{   "StringValue": "MyString2",
    "NumberValue": 43,
    "BooleanValue": false,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "8809ED01-A74C-4874-8ABF-D2678E3AE23D"
}]
"@

# Create the function to create the authorization signature
Function Build-Signature ($customerId, $sharedKey, $date, $contentLength, $method, $contentType, $resource)
{
    $xHeaders = "x-ms-date:" + $date
    $stringToHash = $method + "`n" + $contentLength + "`n" + $contentType + "`n" + $xHeaders + "`n" + $resource

    $bytesToHash = [Text.Encoding]::UTF8.GetBytes($stringToHash)
    $keyBytes = [Convert]::FromBase64String($sharedKey)

    $sha256 = New-Object System.Security.Cryptography.HMACSHA256
    $sha256.Key = $keyBytes
    $calculatedHash = $sha256.ComputeHash($bytesToHash)
    $encodedHash = [Convert]::ToBase64String($calculatedHash)
    $authorization = 'SharedKey {0}:{1}' -f $customerId,$encodedHash
    return $authorization
}


# Create the function to create and post the request
Function Post-LogAnalyticsData($customerId, $sharedKey, $body, $logType)
{
    $method = "POST"
    $contentType = "application/json"
    $resource = "/api/logs"
    $rfc1123date = [DateTime]::UtcNow.ToString("r")
    $contentLength = $body.Length
    $signature = Build-Signature `
        -customerId $customerId `
        -sharedKey $sharedKey `
        -date $rfc1123date `
        -contentLength $contentLength `
        -method $method `
        -contentType $contentType `
        -resource $resource
    $uri = "https://" + $customerId + ".ods.opinsights.azure.com" + $resource + "?api-version=2016-04-01"

    $headers = @{
        "Authorization" = $signature;
        "Log-Type" = $logType;
        "x-ms-date" = $rfc1123date;
        "time-generated-field" = $TimeStampField;
    }

    $response = Invoke-WebRequest -Uri $uri -Method $method -ContentType $contentType -Headers $headers -Body $body -UseBasicParsing
    return $response.StatusCode

}

# Submit the data to the API endpoint
Post-LogAnalyticsData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a>C#-voorbeeld
```csharp
using System;
using System.Net;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Security.Cryptography;
using System.Text;
using System.Threading.Tasks;

namespace OIAPIExample
{
    class ApiExample
    {
        // An example JSON object, with key/value pairs
        static string json = @"[{""DemoField1"":""DemoValue1"",""DemoField2"":""DemoValue2""},{""DemoField3"":""DemoValue3"",""DemoField4"":""DemoValue4""}]";

        // Update customerId to your Log Analytics workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

        // For sharedKey, use either the primary or the secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

        // LogName is name of the event type that is being submitted to Azure Monitor
        static string LogName = "DemoExample";

        // You can use an optional field to specify the timestamp from the data. If the time field is not specified, Azure Monitor assumes the time is the message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
            // Create a hash for the API signature
            var datestring = DateTime.UtcNow.ToString("r");
            var jsonBytes = Encoding.UTF8.GetBytes(json);
            string stringToHash = "POST\n" + jsonBytes.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

        // Build the API signature
        public static string BuildSignature(string message, string secret)
        {
            var encoding = new System.Text.ASCIIEncoding();
            byte[] keyByte = Convert.FromBase64String(secret);
            byte[] messageBytes = encoding.GetBytes(message);
            using (var hmacsha256 = new HMACSHA256(keyByte))
            {
                byte[] hash = hmacsha256.ComputeHash(messageBytes);
                return Convert.ToBase64String(hash);
            }
        }

        // Send a request to the POST API endpoint
        public static void PostData(string signature, string date, string json)
        {
            try
            {
                string url = "https://" + customerId + ".ods.opinsights.azure.com/api/logs?api-version=2016-04-01";

                System.Net.Http.HttpClient client = new System.Net.Http.HttpClient();
                client.DefaultRequestHeaders.Add("Accept", "application/json");
                client.DefaultRequestHeaders.Add("Log-Type", LogName);
                client.DefaultRequestHeaders.Add("Authorization", signature);
                client.DefaultRequestHeaders.Add("x-ms-date", date);
                client.DefaultRequestHeaders.Add("time-generated-field", TimeStampField);

                System.Net.Http.HttpContent httpContent = new StringContent(json, Encoding.UTF8);
                httpContent.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                Task<System.Net.Http.HttpResponseMessage> response = client.PostAsync(new Uri(url), httpContent);

                System.Net.Http.HttpContent responseContent = response.Result.Content;
                string result = responseContent.ReadAsStringAsync().Result;
                Console.WriteLine("Return Result: " + result);
            }
            catch (Exception excep)
            {
                Console.WriteLine("API Post Exception: " + excep.Message);
            }
        }
    }
}

```

### <a name="python-2-sample"></a>Voorbeeld van Python 2
```python
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update the customer ID to your Log Analytics workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For the shared key, use either the primary or the secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# The log type is the name of the event that is being submitted
log_type = 'WebMonitorTest'

# An example JSON web monitor object
json_data = [{
   "slot_ID": 12345,
    "ID": "5cdad72f-c848-4df0-8aaa-ffe033e75d57",
    "availability_Value": 100,
    "performance_Value": 6.954,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "true"
},
{   
    "slot_ID": 67890,
    "ID": "b6bee458-fb65-492e-996d-61c4d7fbb942",
    "availability_Value": 100,
    "performance_Value": 3.379,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "false"
}]
body = json.dumps(json_data)

#####################
######Functions######  
#####################

# Build the API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request to the POST API
def post_data(customer_id, shared_key, body, log_type):
    method = 'POST'
    content_type = 'application/json'
    resource = '/api/logs'
    rfc1123date = datetime.datetime.utcnow().strftime('%a, %d %b %Y %H:%M:%S GMT')
    content_length = len(body)
    signature = build_signature(customer_id, shared_key, rfc1123date, content_length, method, content_type, resource)
    uri = 'https://' + customer_id + '.ods.opinsights.azure.com' + resource + '?api-version=2016-04-01'

    headers = {
        'content-type': content_type,
        'Authorization': signature,
        'Log-Type': log_type,
        'x-ms-date': rfc1123date
    }

    response = requests.post(uri,data=body, headers=headers)
    if (response.status_code >= 200 and response.status_code <= 299):
        print 'Accepted'
    else:
        print "Response code: {}".format(response.status_code)

post_data(customer_id, shared_key, body, log_type)
```
## <a name="alternatives-and-considerations"></a>Alternatieven en overwegingen
De Collector-API moet beslaat van uw behoeften voor het verzamelen van vrije-gegevens in Azure-Logboeken, maar er zijn gevallen waarbij alternatief nodig zijn om het oplossen van enkele van de beperkingen van de API. Alle uw opties zijn als volgt de belangrijkste zaken die zijn opgenomen:

| Alternatieve | Description | Het meest geschikt voor |
|---|---|---|
| [Aangepaste gebeurtenissen](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics?toc=%2Fazure%2Fazure-monitor%2Ftoc.json#properties): Systeemeigen SDK op basis van opname in Application Insights | Application Insights, doorgaans geïnstrumenteerd via een SDK in uw toepassing, biedt de mogelijkheid voor u om aangepaste gegevens door middel van aangepaste gebeurtenissen te verzenden. | <ul><li> Gegevens die is gegenereerd in uw toepassing, maar niet zijn doorgevoerd door SDK via een van de standaard-gegevenstypen (ie: aanvragen, afhankelijkheden, uitzonderingen, enzovoort).</li><li> Gegevens die vaak wordt gecorreleerd met de andere toepassingsgegevens in Application Insights </li></ul> |
| [Gegevensverzamelaar-API](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-collector-api) in Logboeken van Azure Monitor | De Collector-API in Azure Monitor-Logboeken is een volledig mogelijkheden voor opname van gegevens. Geen gegevens ingedeeld in een JSON-object kunnen hier worden verzonden. Als verzonden, wordt verwerkt en beschikbaar zijn in Logboeken om te worden gecorreleerd met andere gegevens in Logboeken of op basis van andere Application Insights gegevens. <br/><br/> Het is redelijk eenvoudig de gegevens te uploaden als bestanden naar een Azure Blob-blob uit waar deze bestanden worden verwerkt en geüpload naar Log Analytics. Raadpleeg [dit](https://docs.microsoft.com/azure/log-analytics/log-analytics-create-pipeline-datacollector-api) artikel voor een Voorbeeldimplementatie van dergelijke een pijplijn. | <ul><li> Gegevens die niet noodzakelijkerwijs worden gegenereerd in een toepassing die is geïnstrumenteerd in Application Insights.</li><li> Voorbeelden zijn onder meer lookup-en feitentabellen, referentiegegevens, vooraf samengevoegde statistieken, enzovoort. </li><li> Bedoeld voor gegevens die op basis van andere Azure Monitor-gegevens (bijvoorbeeld Application Insights, andere gegevenstypen Logboeken, Security Center, Azure-Monitor voor Containers/VM's, enzovoort) waarnaar wordt verwezen. </li></ul> |
| [Azure Data Explorer](https://docs.microsoft.com/azure/data-explorer/ingest-data-overview) | Azure Data Explorer (ADX) is het gegevensplatform dat wordt gebruikt door Application Insights Analytics en logboeken van Azure Monitor. Nu biedt algemeen beschikbaar ('GA'), met behulp van het gegevensplatform in ruwe vorm u volledige flexibiliteit (maar waarvoor de overhead van het management) ten opzichte van het cluster (RBAC, Retentiepercentage, schema, enzovoort). ADX biedt veel [opname opties](https://docs.microsoft.com/azure/data-explorer/ingest-data-overview#ingestion-methods) inclusief [CSV, TSV en JSON](https://docs.microsoft.com/azure/kusto/management/mappings?branch=master) bestanden. | <ul><li> Gegevens die niet wordt worden gecorreleerd met andere gegevens in Application Insights of de logboeken. </li><li> Gegevens vereisen geavanceerde opname of vandaag nog niet beschikbaar in Logboeken van Azure Monitor-verwerkingsmogelijkheden. </li></ul> |


## <a name="next-steps"></a>Volgende stappen
- Gebruik de [Log Search API](../log-query/log-query-overview.md) voor het ophalen van gegevens vanuit de Log Analytics-werkruimte.

- Meer informatie over hoe u [een pijplijn maken met de API van Data Collector](create-pipeline-datacollector-api.md) met behulp van de werkstroom voor Logic Apps naar Azure Monitor.
