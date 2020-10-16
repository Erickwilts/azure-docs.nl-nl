---
title: Aangepast veld toewijzen aan Azure Event Grid schema
description: In dit artikel wordt beschreven hoe u uw aangepaste schema converteert naar het Azure Event Grid schema wanneer uw gebeurtenis gegevens niet overeenkomen met Event Grid schema.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 836e7b340c5c89100207e2f9409710b8dfa5e3bf
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "86105520"
---
# <a name="map-custom-fields-to-event-grid-schema"></a>Aangepaste velden toewijzen aan Event Grid-schema

Als uw gebeurtenis gegevens niet overeenkomen met het verwachte [Event grid schema](event-schema.md), kunt u nog steeds gebruikmaken van Event grid om gebeurtenis te routeren naar abonnees. In dit artikel wordt beschreven hoe u uw schema aan het Event Grid schema kunt toewijzen.

[!INCLUDE [requires-azurerm](../../includes/requires-azurerm.md)]

## <a name="install-preview-feature"></a>Preview-functie installeren

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="original-event-schema"></a>Oorspronkelijk gebeurtenis schema

Stel dat u een toepassing hebt die gebeurtenissen in de volgende indeling verzendt:

```json
[
  {
    "myEventTypeField":"Created",
    "resource":"Users/example/Messages/1000",
    "resourceData":{"someDataField1":"SomeDataFieldValue"}
  }
]
```

Hoewel deze indeling niet overeenkomt met het vereiste schema, Event Grid u in staat stelt uw velden toe te wijzen aan het schema. U kunt ook de waarden in het oorspronkelijke schema ontvangen.

## <a name="create-custom-topic-with-mapped-fields"></a>Aangepast onderwerp maken met toegewezen velden

Wanneer u een aangepast onderwerp maakt, geeft u op hoe velden van de oorspronkelijke gebeurtenis moeten worden toegewezen aan het event grid-schema. Er zijn drie waarden die u gebruikt om de toewijzing aan te passen:

* De waarde van het **invoer schema** geeft u het type schema op. De beschik bare opties zijn CloudEvents schema, aangepast gebeurtenis schema of Event Grid schema. De standaard waarde is Event Grid schema. Gebruik aangepast gebeurtenis schema bij het maken van een aangepaste toewijzing tussen uw schema en het event grid-schema. Wanneer gebeurtenissen zich in het CloudEvents-schema bevinden, gebruikt u het Cloudevents-schema.

* De eigenschap **standaard waarden toewijzen** geeft standaard waarden voor velden in het event grid schema op. U kunt standaard waarden instellen voor `subject` , `eventtype` , en `dataversion` . Normaal gesp roken gebruikt u deze para meter wanneer uw aangepaste schema geen veld bevat dat overeenkomt met een van deze drie velden. U kunt bijvoorbeeld opgeven dat de gegevens versie altijd is ingesteld op **1,0**.

* De waarde van de **toewijzings velden** wijst velden van uw schema toe aan het event grid-schema. U geeft waarden op in een spatie gescheiden sleutel/waarde-paren. Gebruik de naam van het gebeurtenis raster veld voor de sleutel naam. Gebruik de naam van uw veld voor de waarde. U kunt sleutel namen gebruiken voor `id` , `topic` ,,, `eventtime` `subject` `eventtype` en `dataversion` .

Als u een aangepast onderwerp met Azure CLI wilt maken, gebruikt u:

```azurecli-interactive
# If you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create \
  -n demotopic \
  -l eastus2 \
  -g myResourceGroup \
  --input-schema customeventschema \
  --input-mapping-fields eventType=myEventTypeField \
  --input-mapping-default-values subject=DefaultSubject dataVersion=1.0
```

Gebruik voor PowerShell:

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

New-AzureRmEventGridTopic `
  -ResourceGroupName myResourceGroup `
  -Name demotopic `
  -Location eastus2 `
  -InputSchema CustomEventSchema `
  -InputMappingField @{eventType="myEventTypeField"} `
  -InputMappingDefaultValue @{subject="DefaultSubject"; dataVersion="1.0" }
```

## <a name="subscribe-to-event-grid-topic"></a>Onderwerp abonneren op Event grid

Wanneer u zich abonneert op het aangepaste onderwerp, geeft u het schema op dat u wilt gebruiken voor het ontvangen van de gebeurtenissen. U geeft het CloudEvents-schema, het aangepaste gebeurtenis schema of het Event Grid schema op. De standaard waarde is Event Grid schema.

In het volgende voor beeld wordt een abonnement op een event grid-onderwerp gemaakt en wordt het Event Grid schema gebruikt. Gebruik voor Azure CLI:

```azurecli-interactive
topicid=$(az eventgrid topic show --name demoTopic -g myResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --source-resource-id $topicid \
  --name eventsub1 \
  --event-delivery-schema eventgridschema \
  --endpoint <endpoint_URL>
```

In het volgende voor beeld wordt het invoer schema van de gebeurtenis gebruikt:

```azurecli-interactive
az eventgrid event-subscription create \
  --source-resource-id $topicid \
  --name eventsub2 \
  --event-delivery-schema custominputschema \
  --endpoint <endpoint_URL>
```

In het volgende voor beeld wordt een abonnement op een event grid-onderwerp gemaakt en wordt het Event Grid schema gebruikt. Gebruik voor PowerShell:

```azurepowershell-interactive
$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName myResourceGroup -Name demoTopic).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName eventsub1 `
  -EndpointType webhook `
  -Endpoint <endpoint-url> `
  -DeliverySchema EventGridSchema
```

In het volgende voor beeld wordt het invoer schema van de gebeurtenis gebruikt:

```azurepowershell-interactive
New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName eventsub2 `
  -EndpointType webhook `
  -Endpoint <endpoint-url> `
  -DeliverySchema CustomInputSchema
```

## <a name="publish-event-to-topic"></a>Gebeurtenis publiceren in onderwerp

U bent nu klaar om een gebeurtenis naar het aangepaste onderwerp te verzenden en het resultaat van de toewijzing te bekijken. Het volgende script voor het plaatsen van een gebeurtenis in het [voorbeeld schema](#original-event-schema):

Gebruik voor Azure CLI:

```azurecli-interactive
endpoint=$(az eventgrid topic show --name demotopic -g myResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name demotopic -g myResourceGroup --query "key1" --output tsv)

event='[ { "myEventTypeField":"Created", "resource":"Users/example/Messages/1000", "resourceData":{"someDataField1":"SomeDataFieldValue"} } ]'

curl -X POST -H "aeg-sas-key: $key" -d "$event" $endpoint
```

Gebruik voor PowerShell:

```azurepowershell-interactive
$endpoint = (Get-AzureRmEventGridTopic -ResourceGroupName myResourceGroup -Name demotopic).Endpoint
$keys = Get-AzureRmEventGridTopicKey -ResourceGroupName myResourceGroup -Name demotopic

$htbody = @{
    myEventTypeField="Created"
    resource="Users/example/Messages/1000"
    resourceData= @{
        someDataField1="SomeDataFieldValue"
    }
}

$body = "["+(ConvertTo-Json $htbody)+"]"
Invoke-WebRequest -Uri $endpoint -Method POST -Body $body -Headers @{"aeg-sas-key" = $keys.Key1}
```

Bekijk nu het eind punt van de webhook. De twee abonnementen die gebeurtenissen in verschillende schema's hebben geleverd.

Het eerste gebruikte abonnement voor het gebeurtenis raster schema. De indeling van de bezorgde gebeurtenis is:

```json
{
  "id": "aa5b8e2a-1235-4032-be8f-5223395b9eae",
  "eventTime": "2018-11-07T23:59:14.7997564Z",
  "eventType": "Created",
  "dataVersion": "1.0",
  "metadataVersion": "1",
  "topic": "/subscriptions/<subscription-id>/resourceGroups/myResourceGroup/providers/Microsoft.EventGrid/topics/demotopic",
  "subject": "DefaultSubject",
  "data": {
    "myEventTypeField": "Created",
    "resource": "Users/example/Messages/1000",
    "resourceData": {
      "someDataField1": "SomeDataFieldValue"
    }
  }
}
```

Deze velden bevatten de toewijzingen van het aangepaste onderwerp. **myEventTypeField** is toegewezen aan **Event**type. De standaard waarden voor **DataVersion** en **subject** worden gebruikt. Het **gegevens** object bevat de oorspronkelijke velden van het gebeurtenis schema.

Voor het tweede abonnement is het invoer gebeurtenis schema gebruikt. De indeling van de bezorgde gebeurtenis is:

```json
{
  "myEventTypeField": "Created",
  "resource": "Users/example/Messages/1000",
  "resourceData": {
    "someDataField1": "SomeDataFieldValue"
  }
}
```

U ziet dat de oorspronkelijke velden zijn geleverd.

## <a name="next-steps"></a>Volgende stappen

* [Event grid aflevering van berichten en probeer het opnieuw](delivery-and-retry.md).
* Zie [Een inleiding tot Event Grid](overview.md) voor een inleiding tot Event Grid.
* Zie [aangepaste gebeurtenissen maken en routeren met Azure Event grid](custom-event-quickstart.md)om snel aan de slag te gaan met Event grid.
