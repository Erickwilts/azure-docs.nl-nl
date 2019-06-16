---
title: Ophalen van een specifieke aanbiedings-API | Azure Marketplace
description: API haalt de opgegeven aanbieding binnen de naamruimte van de uitgever.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: reference
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: bb6bbd457ff372ad46091f49cf4ae7e4b34b3d83
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64935444"
---
<a name="retrieve-a-specific-offer"></a>Een specifieke aanbiedings ophalen
=========================

Hiermee haalt u de opgegeven aanbieding binnen de naamruimte van de uitgever.  

U kunt ook ophalen van een bepaalde versie van de aanbieding of de aanbieding in concept, weergave of productiesites ophalen. Als een site niet is opgegeven, is de standaardinstelling `draft`. Het ophalen van een aanbieding die niet zijn bekeken of gepubliceerd resulteert in een `404 Not Found` fout.

> [!WARNING]
> De geheime waarden voor de velden van het geheim type wordt niet worden opgehaald door deze API.

``` http
    GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>?api-version=2017-10-31

    GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/versions/<version>?api-version=2017-10-31

    GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/slot/<slotId>?api-version=2017-10-31

```


<a name="uri-parameters"></a>URI-parameters
--------------


| **Naam**    | **Beschrijving**                                                                          | **Gegevenstype** |
|-------------|------------------------------------------------------------------------------------------|---------------|
| publisherId | publisherId. Bijvoorbeeld: Contoso                                                        | String        |
| offerId     | De GUID die de aanbieding wordt aangeduid.                                                 | String        |
| version     | De versie van de aanbieding wordt opgehaald. Standaard zijn de meest recente versie van de aanbieding wordt opgehaald. | Geheel getal       |
| slotId      | De sleuf van waaruit de aanbieding moet worden opgehaald, is mogelijk een van:      <br/>  - `Draft` (standaard) Hiermee haalt u de versie van de aanbieding op dit moment in concept.  <br/>  -  `Preview` Hiermee haalt u de versie van de aanbieding die momenteel in preview.     <br/>  -  `Production` Hiermee haalt u de versie van de aanbieding op dit moment in productie.          |      enum |
| api-version | Meest recente versie van de API                                                                    | Date          |
|  |  |  |


<a name="header"></a>Header
------

|  **Naam**          |   **Waarde**            |
|  ---------------   |  --------------        |
|  Content-Type      | `application/json`     |
|  Autorisatie     | `Bearer YOUR_TOKEN`    |
|  |  |


<a name="body-example"></a>Voorbeeld van de hoofdtekst
------------

### <a name="response"></a>Antwoord

``` json
{
    "offerTypeId": "microsoft-azure-virtualmachines",
    "publisherId": "contoso",
    "status": "failed",
    "id": "059afc24-07de-4126-b004-4e42a51816fe",
    "version": 5,
    "definition": {
        "displayText": "Contoso Virtual Machine Offer",
        "offer": {
            "microsoft-azure-marketplace-testdrive.enabled": false,
            "microsoft-azure-marketplace-testdrive.videos": [],
            "microsoft-azure-marketplace.title": "Contoso App",
            "microsoft-azure-marketplace.summary": "Contoso App makes dev ops a breeze",
            "microsoft-azure-marketplace.longSummary": "Contoso App makes dev ops a breeze",
            "microsoft-azure-marketplace.description": "Contoso App makes dev ops a breeze",
            "microsoft-azure-marketplace.offerMarketingUrlIdentifier": "contosoapp",
            "microsoft-azure-marketplace.allowedSubscriptions": [
                "59160c40-2e25-4dcf-a2fd-6514cb08bf08"
            ],
            "microsoft-azure-marketplace.usefulLinks": [
            {
                "linkTitle": "Contoso App for Azure",
                "linkUrl": "https://azuremarketplace.microsoft.com"
            }
            ],
            "microsoft-azure-marketplace.categories": [
                "devService",
                "networking",
                "database",
                "cache",
                "security"
            ],
            "microsoft-azure-marketplace.smallLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/6218c455-9cbc-450c-9920-f2e7a69ee132.png?sv=2014-02-14&sr=b&sig=6O8MM9dgiJ48VK0MwddkyVbprRAnBszyhVkVHGShhkI%3D&se=2019-03-28T19%3A46%3A50Z&sp=r",
            "microsoft-azure-marketplace.mediumLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/557e714b-2f31-4e12-b0cc-e48dd840edf4.png?sv=2014-02-14&sr=b&sig=NwL67NTQf9Gc9VScmZehtbHXpYmxhwZc2foy3o4xavs%3D&se=2019-03-28T19%3A46%3A49Z&sp=r",
            "microsoft-azure-marketplace.largeLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/142485da-784c-44cb-9523-d4f396446258.png?sv=2014-02-14&sr=b&sig=xaMxhwx%2FlKYfz33mJGIg8UBdVpsOwVvqhjTJ883o0iY%3D&se=2019-03-28T19%3A46%3A49Z&sp=r",
            "microsoft-azure-marketplace.wideLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/48af9013-1df7-4c94-8da8-4626e5039ce0.png?sv=2014-02-14&sr=b&sig=%2BnN7f2tprkrqb45ID6JlT01zXcy1PMTkWXtLKD6nfoE%3D&se=2019-03-28T19%3A46%3A49Z&sp=r",
            "microsoft-azure-marketplace.screenshots": [],
            "microsoft-azure-marketplace.videos": [],
            "microsoft-azure-marketplace.leadDestination": "None",
            "microsoft-azure-marketplace.tableLeadConfiguration": {},
            "microsoft-azure-marketplace.blobLeadConfiguration": {},
            "microsoft-azure-marketplace.salesForceLeadConfiguration": {},
            "microsoft-azure-marketplace.crmLeadConfiguration": {},
            "microsoft-azure-marketplace.httpsEndpointLeadConfiguration": {},
            "microsoft-azure-marketplace.marketoLeadConfiguration": {},
            "microsoft-azure-marketplace.privacyURL": "https://azuremarketplace.microsoft.com",
            "microsoft-azure-marketplace.termsOfUse": "Terms of use",
            "microsoft-azure-marketplace.engineeringContactName": "Jon Doe",
            "microsoft-azure-marketplace.engineeringContactEmail": "jondoe@outlook.com",
            "microsoft-azure-marketplace.engineeringContactPhone": "555-555-5555",
            "microsoft-azure-marketplace.supportContactName": "Jon Doe",
            "microsoft-azure-marketplace.supportContactEmail": "jondoe@outlook.com",
            "microsoft-azure-marketplace.supportContactPhone": "555-555-5555",
            "microsoft-azure-marketplace.publicAzureSupportUrl": "",
            "microsoft-azure-marketplace.fairfaxSupportUrl": ""
            },
            "plans": [
            {
                "planId": "contososkuidentifier",
                "microsoft-azure-virtualmachines.skuTitle": "Contoso App",
                "microsoft-azure-virtualmachines.skuSummary": "Contoso App makes dev ops a breeze.",
                "microsoft-azure-virtualmachines.skuDescription": "This is a description for the Contoso App that makes dev ops a breeze.",
                "microsoft-azure-virtualmachines.hideSKUForSolutionTemplate": false,
                "microsoft-azure-virtualmachines.cloudAvailability": [
                    "PublicAzure"
                ],
                "microsoft-azure-virtualmachines.certificationsFairfax": [],
                "virtualMachinePricing": {
                    "isByol": true,
                    "freeTrialDurationInMonths": 0
                },
                "microsoft-azure-virtualmachines.operatingSystemFamily": "Windows",
                "microsoft-azure-virtualmachines.windowsOSType": "Other",
                "microsoft-azure-virtualmachines.operationSystem": "Contoso App",
                "microsoft-azure-virtualmachines.recommendedVMSizes": [
                    "a0-basic",
                    "a0-standard",
                    "a1-basic",
                    "a1-standard",
                    "a2-basic",
                    "a2-standard"
                ],
                "microsoft-azure-virtualmachines.openPorts": [],
                "microsoft-azure-virtualmachines.vmImages": {
                    "1.0.1": {
                    "osVhdUrl": "http://contosoteststorage.blob.core.windows.net/test/contosoVM.vhd?sv=2014-02-14&sig=WlDo6Q4xwYH%2B5QEJbItPUVdgHhBcrVxPBmntZ2vU96w%3D&st=2016-06-25T18%3A30%3A00Z&se=2017-06-25T18%3A30%3A00Z&sp=rl",
                    "lunVhdDetails": []
                    }
                },
                "regions": [
                    "DZ",
                    "AR"
                ]
            }
            ]
        },
        "changedTime": "2017-06-07T06:15:39.7349221Z"
    }
}
```


### <a name="response-body-properties"></a>Eigenschappen van de hoofdtekst van reactie

|  **Naam**       |   **Beschrijving**                                                                                                               |
|  -------------  |   -----------------------------------------------------------------------------------------------------                         |
|  offerTypeId    | Geeft het type aanbieding                                                                                                    |
|  publisherId    | De unieke id van de uitgever                                                                                              |
|  status         | De status van de aanbieding. Zie voor een lijst van mogelijke waarden, [status van het voorstel](#offer-status) hieronder.                                  |
|  Id             | GUID die de unieke identificatie van de aanbieding                                                                                         |
|  version        | Huidige versie van het product. De versie-eigenschap kan niet worden gewijzigd door de client. Het wordt verhoogd na elke publiceren.    |
|  Definitie     | Werkelijke definitie van de werkbelasting                                                                                               |
|  changedTime    | UTC datum/tijd waarop de aanbieding voor het laatst is gewijzigd                                                                                   |
|  |  |


### <a name="response-status-codes"></a>Antwoord-statuscodes

| **Code**  | **Beschrijving**                                                                                                                 |
|  ------   | ------------------------------------------------------------------------------------------------------------------------------- |
|  200      | `OK` -De aanvraag is verwerkt en de aanbiedingen onder de uitgever zijn geretourneerd naar de client.               |
|  400      | `Bad/Malformed request` -De antwoordtekst voor fout bevat mogelijk meer informatie.                                                 |
|  403      | `Forbidden` -De client heeft geen toegang tot de opgegeven naamruimte.                                                        |
|  404      | `Not found` -De opgegeven entiteit bestaat niet. Client moet controleren de publisherId, offerId en versie (indien opgegeven).      |
|  |  |


### <a name="offer-status"></a>Status van aanbieding

|  **Naam**                   |   **Beschrijving**                             |
| --------------------------- |  -------------------------------------------- |
|  NeverPublished             | Aanbieding is nog nooit gepubliceerd.               |
|  NotStarted                 | Aanbieding is nieuw, maar is niet gestart.              |
|  WaitingForPublisherReview  | Aanbieding wacht op goedkeuring van de uitgever.      |
|  In uitvoering                    | Verzending van de aanbieding wordt verwerkt.          |
|  Geslaagd                  | Verzending van de aanbieding is verwerkt.    |
|  Geannuleerd                   | Verzending van de aanbieding is geannuleerd.                |
|  Mislukt                     | Verzending van de aanbieding is mislukt.                      |
|  |  |
