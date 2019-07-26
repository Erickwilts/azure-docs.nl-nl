---
title: Azure Functions Premium-abonnement (preview) | Microsoft Docs
description: Details en configuratie opties (VNet, geen koude start, onbeperkte uitvoerings duur) voor het Azure Functions Premium-abonnement.
services: functions
author: jeffhollan
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 4/11/2019
ms.author: jehollan
ms.openlocfilehash: 8ad09550e572c98931346b44a6c6f84da29a85e4
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443965"
---
# <a name="azure-functions-premium-plan-preview"></a>Azure Functions Premium-abonnement (preview-versie)

Het Premium-abonnement van Azure Functions is een hosting optie voor functie-apps. Het Premium-abonnement biedt functies als VNet-connectiviteit, geen koud start en Premium-hardware.  Meerdere functie-apps kunnen worden geïmplementeerd in hetzelfde Premium-abonnement en met het plan kunt u de grootte van het reken exemplaar, de grootte van het basis plan en de maximale plan grootte configureren.  Zie [functie schaal en hosting opties](functions-scale.md)voor een vergelijking van het Premium-abonnement en andere typen plannen en hosting.

## <a name="create-a-premium-plan"></a>Een Premium-abonnement maken

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]

U kunt ook een Premium-abonnement maken met behulp van [AZ functionapp plan Create](/cli/azure/functionapp/plan#az-functionapp-plan-create) in de Azure cli. In het volgende voor beeld wordt een _elastisch Premium 1_ -laag abonnement gemaakt:

```azurecli-interactive
az functionapp plan create --resource-group <RESOURCE_GROUP> --name <PLAN_NAME> \
--location <REGION> --sku EP1
```

In dit voor beeld vervangt `<RESOURCE_GROUP>` u door de resource groep `<PLAN_NAME>` en met een naam voor uw abonnement dat uniek is in de resource groep. Geef een [ondersteund `<REGION>` ](#regions)op. Als u een Premium-abonnement wilt maken dat Linux ondersteunt `--is-linux` , neemt u de optie op.

Met het plan dat u hebt gemaakt, kunt u [AZ functionapp Create](/cli/azure/functionapp#az-functionapp-create) gebruiken om uw functie-app te maken. In de portal worden zowel het abonnement als de app tegelijkertijd gemaakt. 

## <a name="features"></a>Functies

De volgende functies zijn beschikbaar voor het werken met apps die zijn geïmplementeerd in een Premium-abonnement.

### <a name="pre-warmed-instances"></a>Vooraf gewarmde instanties

Als er vandaag geen gebeurtenissen en uitvoeringen in het verbruiks abonnement worden uitgevoerd, kan uw app omlaag worden geschaald naar nul exemplaren. Wanneer er nieuwe gebeurtenissen binnenkomen in, moet er een nieuw exemplaar worden gespecialiseerd in de app die erop wordt uitgevoerd.  Het maken van nieuwe instanties kan enige tijd duren, afhankelijk van de app.  Deze extra latentie bij de eerste aanroep wordt vaak app koude start genoemd.

In het Premium-abonnement kunt u uw app vooraf laten opwarmen op een opgegeven aantal exemplaren, tot aan de minimale plan grootte.  Met vooraf gewarmde instanties kunt u een app vooraf schalen voor een hoge belasting. Wanneer de app wordt geschaald, wordt deze voor het eerst geschaald naar de vooraf gewarmde instanties. Aanvullende instanties blijven in de voor bereiding van de volgende schaal bewerking direct in de buffer worden geplaatst en warme. Door een buffer van vooraf gewarmde instanties te hebben, kunt u de latentie van koude start effectief vermijden.  Voor bereide instanties is een functie van het Premium-abonnement en u moet ten minste één instantie actief houden en op elk moment dat het abonnement actief is.

U kunt het aantal vooraf gewarmde instanties in de Azure Portal configureren door uw **functie-app**te selecteren, naar het tabblad **platform functies** te gaan en de opties  voor uitschalen te selecteren. In het venster functie-app bewerken zijn vooraf gewarmde exemplaren specifiek voor die app, maar de minimum-en maximum exemplaren zijn van toepassing op uw hele abonnement.

![Instellingen voor Elastic Scale](./media/functions-premium-plan/scale-out.png)

U kunt ook vooraf gehete instanties configureren voor een app met de Azure CLI

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.preWarmedInstanceCount=<desired_prewarmed_count> --resource-type Microsoft.Web/sites
```

### <a name="private-network-connectivity"></a>Connectiviteit van particuliere netwerken

Azure Functions geïmplementeerd in een Premium-abonnement maakt gebruik van de [nieuwe VNet-integratie voor web-apps](../app-service/web-sites-integrate-with-vnet.md).  Als uw app is geconfigureerd, kan deze communiceren met resources binnen uw VNet of worden beveiligd via service-eind punten.  Er zijn ook IP-beperkingen beschikbaar op de app om inkomend verkeer te beperken.

Wanneer u een subnet aan uw functie-app toewijst in een Premium-abonnement, hebt u een subnet met voldoende IP-adressen nodig voor elk mogelijk exemplaar. Hoewel het maximum aantal exemplaren kan variëren tijdens de preview-periode, is een IP-blok vereist met ten minste 100 beschik bare adressen.

Zie [uw functie-app integreren met een VNet voor](functions-create-vnet.md)meer informatie.

### <a name="rapid-elastic-scale"></a>Snel elastisch schalen

Er worden automatisch extra reken instanties toegevoegd voor uw app met dezelfde snelle schaal logica als het verbruiks abonnement.  Zie [functie schaal en hosting](./functions-scale.md#how-the-consumption-and-premium-plans-work)voor meer informatie over de werking van schalen.

### <a name="unbounded-run-duration"></a>Niet-gebonden uitvoerings duur

Azure Functions in een verbruiks abonnement zijn beperkt tot 10 minuten voor één uitvoering.  In het Premium-abonnement wordt de uitvoerings duur standaard ingesteld op 30 minuten om overmatige uitvoeringen te voor komen. U kunt echter [de host. json-configuratie wijzigen](./functions-host-json.md#functiontimeout) om deze niet-gebonden te maken voor apps uit het Premium-abonnement.

In de preview-periode is uw duur niet meer dan 12 minuten gegarandeerd en krijgt de beste kans om meer dan 30 minuten uit te voeren als uw app niet meer wordt geschaald dan het minimale aantal werk nemers.

## <a name="plan-and-sku-settings"></a>Plannings-en SKU-instellingen

Wanneer u het plan maakt, configureert u twee instellingen: het minimum aantal exemplaren (of de plan grootte) en de maximale burst-limiet.  De minimum instanties voor een Premium-abonnement zijn 1 en de maximale burst tijdens de preview is 20.  Minimum exemplaren worden gereserveerd en worden altijd uitgevoerd.

> [!IMPORTANT]
> Er worden kosten in rekening gebracht voor elke instantie die wordt toegewezen in het minimum aantal exemplaren, ongeacht of de functies worden uitgevoerd.

Als uw app exemplaren vereist die groter zijn dan de grootte van uw abonnement, kan deze worden uitgeschaald totdat het aantal exemplaren de maximale burst-limiet bereikt.  Er worden alleen exemplaren van uw abonnement in rekening gebracht wanneer ze worden uitgevoerd en aan u worden gehuurd.  We maken het beste om uw app te schalen naar de gedefinieerde maximum limiet, terwijl de minimale plan exemplaren gegarandeerd zijn voor uw app.

U kunt de plan grootte en maximum waarden in de Azure Portal configureren door de opties  voor uitschalen te selecteren in het plan of een functie-app die is geïmplementeerd in dat plan (onder **platform functies**).

U kunt ook de maximale burst-limiet van de Azure CLI verhogen:

```azurecli-interactive
az resource update -g <resource_group> -n <premium_plan_name> --set properties.maximumElasticWorkerCount=<desired_max_burst> --resource-type Microsoft.Web/serverfarms 
```

### <a name="available-instance-skus"></a>Beschik bare exemplaar-Sku's

Bij het maken of schalen van uw plan kunt u kiezen uit drie instantie grootten.  Er worden kosten in rekening gebracht voor het totale aantal kernen en het geheugen dat per seconde wordt verbruikt.  Uw app kan automatisch uitschalen naar meerdere exemplaren als dat nodig is.  

|SKU|Kerngeheugens|Geheugen|Storage|
|--|--|--|--|
|EP1|1|3,5 GB|250 GB|
|EP2|2|7GB|250 GB|
|EP3|4|14GB|250 GB|

## <a name="regions"></a>Regions

Hieronder ziet u de regio's die momenteel worden ondersteund voor de open bare preview van elk besturings systeem.

|Regio| Windows | Linux |
|--| -- | -- |
|Australië - oost| ✔ | |
|Australië - zuidoost | ✔ | ✔ |
|Canada - midden| ✔ |  |
|US - centraal| ✔ |  |
|Azië - oost| ✔ |  |
|East US | | ✔ |
|US - oost 2| ✔ |  |
|Frankrijk - centraal| ✔ |  |
|Japan - oost|  | ✔ |
|Japan - west| ✔ | |
|Korea - centraal| ✔ |  |
|US - noord-centraal| ✔ |  |
|Europa - noord| ✔ | ✔ |
|US - zuid-centraal| ✔ |  |
|India - zuid | ✔ | |
|Azië - zuidoost| ✔ | ✔ |
|Verenigd Koninkrijk West| ✔ |  |
|Europa -west| ✔ | ✔ |
|India - west| ✔ |  |
|US - west| ✔ | ✔ |

## <a name="known-issues"></a>Bekende problemen

U kunt de status van bekende problemen van de [open bare preview-versie op github](https://github.com/Azure/Azure-Functions/wiki/Premium-plan-known-issues)bijhouden.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Opties voor het schalen en hosten van Azure Functions begrijpen](functions-scale.md)
