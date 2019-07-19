---
title: Api's voor Marketplace-meet service | Azure Marketplace
description: Gebruiks gebeurtenis voor SaaS-aanbiedingen in azure Marketplace.
author: qianw211
manager: evansma
ms.author: v-qiwe
ms.service: marketplace
ms.topic: conceptual
ms.date: 07/11/2019
ms.openlocfilehash: 35e6c61a8e8537035d70323c85dfc7a76f87cbcd
ms.sourcegitcommit: 10251d2a134c37c00f0ec10e0da4a3dffa436fb3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2019
ms.locfileid: "67869558"
---
# <a name="marketplace-metering-service-apis"></a>Service-API's voor Marketplace-meting

Met de gebruiks gebeurtenis-API kunt u gebruiks gebeurtenissen voor een specifieke aangeschafte entiteit verzenden. De gebruiks gebeurtenis aanvraag verwijst naar de meet dimensie services die door de uitgever is gedefinieerd bij het publiceren van de aanbieding.

## <a name="usage-event"></a>Gebruiks gebeurtenis

**POST**:`https://marketplaceapi.microsoft.com/api/usageEvent?api-version=<ApiVersion>`

*Query parameters:*

|            |          |
| ---------- | ---------------------- |
| `ApiVersion` | De versie van de bewerking die moet worden gebruikt voor deze aanvraag. De nieuwste API-versie is 2018-08-31. |

*Aanvraag headers:*

| Inhouds type       | `application/json`    |
| ------------------ | ---------------------------- |
| `x-ms-requestid`     | Een unieke teken reeks waarde voor het bijhouden van de aanvraag van de client, bij voor keur een GUID. Als deze waarde niet wordt gegeven, wordt er een gegenereerd en weer gegeven in de antwoord headers. |
| `x-ms-correlationid` | Unieke teken reeks waarde voor de bewerking op de client. Deze para meter verbindt alle gebeurtenissen van de client bewerking met gebeurtenissen aan de server zijde. Als deze waarde niet is opgenomen, wordt er een gegenereerd en geleverd in de antwoord headers. |
| `authorization`   | [Het JWT-Bearer-token (JSON Web token) ophalen.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app) Opmerking: Bij het maken van de http- `Bearer` aanvraag, voor voegsel voor het token dat is verkregen van de koppeling waarnaar wordt verwezen. |

*Schot*

```json
{
  "resourceId": "Identifier of the resource against which usage is emitted",
  "quantity": 5.0,
  "dimension": "Dimension identifier",
  "effectiveStartTime": "Time in UTC when the usage event occurred",
  "planId": "Plan associated with the purchased offer"
}
```

### <a name="responses"></a>Responses

Code: 200<br>
OK 

```json
{
  "usageEventId": "Unique identifier associated with the usage event",
  "status": "Accepted",
  "messageTime": "Time this message was created in UTC",
  "resourceId": "Identifier of the resource against which usage is emitted",
  "quantity": 5.0,
  "dimension": "Dimension identifier",
  "effectiveStartTime": "Time in UTC when the usage event occurred",
  "planId": "Plan associated with the purchased offer"
}
```

Code: 400 <br>
Ongeldige aanvraag, ontbrekende of ongeldige gegevens verschaft of verlopen

```json
{
  "message": "One or more errors have occurred.",
  "target": "usageEventRequest",
  "details": [
    {
      "message": "The resourceId is required.",
      "target": "ResourceId",
      "code": "BadArgument"
    }
  ],
  "code": "BadArgument"
}
```

Code: 403<br>
Ongeldige aanvraag, ontbrekende of ongeldige gegevens verschaft of verlopen

```json
{
  "code": "Forbidden",
  "message": "User is not allowed authorized to call this"
}
```

Code: 409<br>
Er is een conflict opgetreden bij het ontvangen van de gebruiks oproep voor de resource-ID van het gebruik en het daad werkelijke gebruik dat al bestaat. Het antwoord `additionalInfo` bevat een veld met informatie over het geaccepteerde bericht.

```json
{
  "code": "Conflict",
  "additionalInfo": {
    "usageEventId": "Unique identifier associated with the usage event",
    "status": "Accepted|NotProcessed|Expired",
    "messageTime": "Time this message was created in UTC",
    "resourceId": "Identifier of the resource against which usage is emitted",
    "quantity": 5.0,
    "dimension": "Dimension identifier",
    "effectiveStartTime": "Time in UTC when the usage event occurred",
    "planId": "Plan associated with the purchased offer"
  }
}
```

## <a name="batch-usage-event"></a>Batch Usage-gebeurtenis

Met de gebeurtenis-API voor batch gebruik kunt u gebruiks gebeurtenissen voor meer dan één aangeschafte entiteit in één keer verzenden. De gebeurtenis aanvraag voor batch gebruik verwijst naar de meet dimensie services die door de uitgever is gedefinieerd bij het publiceren van de aanbieding.

>[!Note]
>U kunt meerdere SaaS-aanbiedingen registreren in de commerciële Marketplace van micro soft. Elke geregistreerde SaaS-aanbieding heeft een unieke Azure AD-toepassing die is geregistreerd voor verificatie-en autorisatie doeleinden. De gebeurtenissen die in batch worden gegenereerd, moeten deel uitmaken van aanbiedingen met dezelfde Azure AD-toepassing op het moment van de registratie van de aanbieding.

**POST:** `https://marketplaceapi.microsoft.com/api/batchUsageEvent?api-version=<ApiVersion>`

*Query parameters:*

|            |     |
| ---------- | -------------------- |
| `ApiVersion` | De versie van de bewerking die moet worden gebruikt voor deze aanvraag. De nieuwste API-versie is 2018-08-31. |

*Aanvraag headers:*

| Inhouds type       | `application/json`       |
| ------------------ | ------ |
| `x-ms-requestid`     | Een unieke teken reeks waarde voor het bijhouden van de aanvraag van de client, bij voor keur een GUID. Als deze waarde niet wordt gegeven, wordt er een gegenereerd en gegeven in de antwoord headers. |
| `x-ms-correlationid` | Unieke teken reeks waarde voor de bewerking op de client. Deze para meter verbindt alle gebeurtenissen van de client bewerking met gebeurtenissen aan de server zijde. Als deze waarde niet is opgenomen, wordt er een gegenereerd en gegeven in de antwoord headers. |
| `authorization`      | [Het JWT-Bearer-token (JSON Web token) ophalen.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app) Opmerking: Bij het maken van de http- `Bearer` aanvraag, voor voegsel voor het token dat is verkregen van de koppeling waarnaar wordt verwezen.  |

*Schot*
```json
{
  "request": [
    {
      "resourceId": "Identifier of the resource against which usage is emitted",
      "quantity": 5.0,
      "dimension": "Dimension identifier",
      "effectiveStartTime": "Time in UTC when the usage event occurred",
      "planId": "Plan associated with the purchased offer"
    },
    {
      "resourceId": "Identifier of the resource against which usage is emitted",
      "quantity": 5.0,
      "dimension": "Dimension identifier",
      "effectiveStartTime": "Time in UTC when the usage event occurred",
      "planId": "Plan associated with the purchased offer"
    }
  ]
}
```
### <a name="responses"></a>Responses

Code: 200<br>
OK

```json
{
  "count": 2,
  "result": [
    {
      "usageEventId": "Unique identifier associated with the usage event",
      "status": "Accepted|Expired|Duplicate|Error|ResourceNotFound|ResourceNotAuthorized|InvalidDimension|BadArgument",
      "messageTime": "Time this message was created in UTC",
      "resourceId": "Identifier of the resource against which usage is emitted",
      "quantity": 5.0,
      "dimension": "Dimension identifier",
      "effectiveStartTime": "Time in UTC when the usage event occurred",
      "planId": "Plan associated with the purchased offer",
      "error": "Error object (optional)"
    },
    {
      "usageEventId": "Unique identifier associated with the usage event",
      "status": "Accepted|Expired|Duplicate|Error|ResourceNotFound|ResourceNotAuthorized|InvalidDimension|BadArgument",
      "messageTime": "Time this message was created in UTC",
      "resourceId": "Identifier of the resource against which usage is emitted",
      "quantity": 5.0,
      "dimension": "Dimension identifier",
      "effectiveStartTime": "Time in UTC when the usage event occurred",
      "planId": "Plan associated with the purchased offer",
      "error": "Error object (optional)"
    }
  ]
}
```

Beschrijving van de status code waarnaar wordt `BatchUsageEvent` verwezen in de API-reactie:

| Statuscode  | Description |
| ---------- | -------------------- |
| `Accepted` | Geaccepteerde code. |
| `Expired` | Verlopen gebruik. |
| `Duplicate` | Er is een dubbel gebruik gegeven. |
| `Error` | Fout code. |
| `ResourceNotFound` | De gegeven gebruiks resource is ongeldig. |
| `ResourceNotAuthorized` | U bent niet gemachtigd om gebruik te maken van deze resource. |
| `InvalidDimension` | De dimensie waarvoor het gebruik wordt door gegeven, is ongeldig voor deze aanbieding/dit plan. |
| `InvalidQuantity` | De door gegeven hoeveelheid is < 0. |
| `BadArgument` | De invoer ontbreekt of is onjuist. |

Code: 400<br>
Ongeldige aanvraag, ontbrekende of ongeldige gegevens verschaft of verlopen

```json
{
  "message": "One or more errors have occurred.",
  "target": "usageEventRequest",
  "details": [
    {
      "message": "Invalid data format.",
      "target": "usageEventRequest",
      "code": "BadArgument"
    }
  ],
  "code": "BadArgument"
}
```
Code: 403<br>
De gebruiker is niet gemachtigd om deze aanroep uit te voeren

```json
{
  "code": "Forbidden",
  "message": "User is not allowed to call this"
}
```

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie [SaaS-data limiet](./saas-metered-billing.md)voor gefactureerd.
