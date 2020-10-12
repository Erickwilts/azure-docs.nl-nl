---
title: Azure Event Grid-probleemoplossings validatie van abonnementen
description: In dit artikel wordt beschreven hoe u de validatie van abonnementen kunt oplossen.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 48844859013507ab684ef8879b7b85dd6b6fe8cd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "86118984"
---
# <a name="troubleshoot-azure-event-grid-subscription-validations"></a>Problemen met de validatie van Azure Event Grid-abonnementen oplossen
In dit artikel vindt u informatie over het oplossen van validaties voor gebeurtenis abonnementen. 

> [!IMPORTANT]
> Zie [webhook Event Delivery](webhook-event-delivery.md)(Engelstalig) voor gedetailleerde informatie over eindpunt validatie voor webhooks.

## <a name="validate-event-grid-event-subscription-using-postman"></a>Event Grid gebeurtenis abonnement valideren met behulp van postman
Hier volgt een voor beeld van het gebruik van Postman voor het valideren van een webhook-abonnement van een Event Grid gebeurtenis: 

![Validatie van gebeurtenis abonnementen Event grid met behulp van postman](./media/troubleshoot-subscription-validation/event-subscription-validation-postman.png)

Hier volgt een voor beeld van een JSON- **SubscriptionValidationEvent** :

```json
[
  {
    "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
    "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "subject": "",
    "data": {
      "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6",
    },
    "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
    "eventTime": "2018-01-25T22:12:19.4556811Z",
    "metadataVersion": "1",
    "dataVersion": "1"
  }
]
```

Dit is het voor beeld van een geslaagde reactie:

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```

Zie [endpoint validator with Event grid Events](webhook-event-delivery.md#endpoint-validation-with-event-grid-events)(Engelstalig) voor meer informatie over Event grid gebeurtenis validatie voor webhooks.


## <a name="validate-event-grid-event-subscription-using-curl"></a>Event Grid gebeurtenis abonnement met behulp van krul valideren 
Hier volgt een voor beeld van een krul opdracht voor het valideren van een webhook-abonnement van een Event Grid gebeurtenis: 

```bash
curl -X POST -d '[{"id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66","topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx","subject": "","data": {"validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"},"eventType": "Microsoft.EventGrid.SubscriptionValidationEvent","eventTime": "2018-01-25T22:12:19.4556811Z", "metadataVersion": "1","dataVersion": "1"}]' -H 'Content-Type: application/json' https://{your-webhook-url.com}
```

## <a name="validate-cloud-event-subscription-using-postman"></a>Cloud gebeurtenis abonnement valideren met behulp van postman
Hier volgt een voor beeld van het gebruik van Postman voor het valideren van een webhook-abonnement van een Cloud gebeurtenis: 

![Validatie van abonnement op Cloud gebeurtenis met behulp van postman](./media/troubleshoot-subscription-validation/cloud-event-subscription-validation-postman.png)

Gebruik de **http-opties** methode voor validatie met Cloud gebeurtenissen. Zie [endpoint validator with Cloud Events](webhook-event-delivery.md#endpoint-validation-with-event-grid-events)(Engelstalig) voor meer informatie over de validatie van de Cloud gebeurtenis voor webhooks.

## <a name="error-code-403"></a>Fout code: 403
Als uw webhook 403 (verboden) in het antwoord retourneert, controleert u of uw webhook zich achter een Azure-toepassing gateway of Web Application firewall bevindt. Als dat het geval is, moet u de volgende firewall regels uitschakelen en opnieuw een HTTP-POST doen:

  - 920300 (aanvraag mist een Accept-header, we kunnen dit oplossen)
  - 942430 (beperkte SQL-teken anomalie detectie (argumenten): aantal speciale tekens is overschreden (12))
  - 920230 (meerdere URL-code ring gedetecteerd)
  - 942130 (SQL-injectie aanval: SQL-Tautology gedetecteerd.)
  - 931130 (mogelijke RFI-aanval (Remote File Include) = Off-Domain referentie/koppeling)

## <a name="next-steps"></a>Volgende stappen
Als u meer hulp nodig hebt, kunt u uw probleem in het [stack overflow forum](https://stackoverflow.com/questions/tagged/azure-eventgrid) plaatsen of een [ondersteunings ticket](https://azure.microsoft.com/support/options/)openen. 
