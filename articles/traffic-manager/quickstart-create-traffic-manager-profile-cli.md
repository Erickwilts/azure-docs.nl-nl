---
title: 'Quick Start: een Traffic Manager profiel maken voor een hoge Beschik baarheid van toepassingen met behulp van Azure CLI'
description: In dit Quick Start-artikel wordt beschreven hoe u een Traffic Manager profiel maakt om een Maxi maal beschik bare webtoepassing te bouwen.
services: traffic-manager
author: asudbring
mnager: twooley
Customer intent: As an IT admin, I want to direct user traffic to ensure high availability of web applications.
ms.service: traffic-manager
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/29/2019
ms.author: allensu
ms.openlocfilehash: dc7e555eb95cf88ecf57a6df4999672bb721b8cf
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68620588"
---
# <a name="quickstart-create-a-traffic-manager-profile-for-a-highly-available-web-application-using-azure-cli"></a>Quickstart: Een Traffic Manager profiel maken voor een Maxi maal beschik bare webtoepassing met behulp van Azure CLI

In deze quickstart wordt beschreven hoe u een Traffic Manager-profiel maakt die hoge beschikbaarheid van uw webtoepassing biedt.

In deze Quick Start maakt u twee exemplaren van een webtoepassing. Ze worden elk in een andere Azure-regio uitgevoerd. U maakt een Traffic Manager-profiel op basis van [eindpuntprioriteit](traffic-manager-routing-methods.md#priority). het profiel stuurt gebruikersverkeer door naar de primaire site waar de webtoepassing wordt uitgevoerd. Traffic Manager bewaakt de webtoepassing continu. Als de primaire site niet beschikbaar is, biedt Traffic Manager automatische failover voor de back-upsite.

Als u nog geen abonnement op Azure hebt, maak dan nu een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze zelfstudie versie 2.0.28 of hoger van Azure CLI uitvoeren. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Maak een resourcegroep maken met [az group create](https://docs.microsoft.com/cli/azure/group). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli-interactive

  az group create \
    --name myResourceGroup \
    --location eastus

```

## <a name="create-a-traffic-manager-profile"></a>Een Traffic Manager-profiel maken

Maak een Traffic Manager profiel met [AZ Network Traffic-Manager profile Create](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile?view=azure-cli-latest#az-network-traffic-manager-profile-create) waarmee gebruikers verkeer wordt omgeleid op basis van de eindpunt prioriteit.

Vervang in het volgende voor beeld **< probestands naam >** met een unieke Traffic Manager profiel naam.

```azurecli-interactive

az network traffic-manager profile create \
    --name <profile_name> \
    --resource-group myResourceGroup \
    --routing-method Priority \
    --path "/" \
    --protocol HTTP \
    --unique-dns-name <profile_name> \
    --ttl 30 \
    --port 80

```

## <a name="create-web-apps"></a>Web-apps maken

Voor deze quickstart moeten twee exemplaren van een webtoepassing worden geïmplementeerd in twee verschillende Azure-regio's (*US - oost* en *Europa - west*). Elk exemplaar dient als primair en failover-eindpunt voor Traffic Manager.

### <a name="create-web-app-service-plans"></a>Web app service-abonnementen maken
Maak Web app service-plannen met behulp van [AZ appservice plan Create](https://docs.microsoft.com/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) voor de twee exemplaren van de webtoepassing die u in twee verschillende Azure-regio's gaat implementeren.

Vervang in het volgende voor beeld **< appspname_eastus >** en **< appspname_westeurope >** door een unieke app service plan naam

```azurecli-interactive

az appservice plan create \
    --name <appspname_eastus> \
    --resource-group myResourceGroup \
    --location eastus \
    --sku S1

az appservice plan create \
    --name <appspname_westeurope> \
    --resource-group myResourceGroup \
    --location westeurope \
    --sku S1

```
### <a name="create-a-web-app-in-the-app-service-plan"></a>Een web-app maken in het app service-plan
Maak twee exemplaren van de webtoepassing met [AZ webapp maken](https://docs.microsoft.com/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) in de app service plannen in het *VS-Oost* en *Europa-West* Azure-regio's.

Vervang in het volgende voor beeld **< app1name_eastus >** en **< app2name_westeurope >** door een unieke app-naam en vervang **< appspname_eastus >** en **< appspname_westeurope >** met de naam die u hebt gebruikt voor het maken van de App Service plannen in de vorige sectie.

```azurecli-interactive

az webapp create \
    --name <app1name_eastus> \
    --plan <appspname_eastus> \
    --resource-group myResourceGroup

az webapp create \
    --name <app2name_westeurope> \
    --plan <appspname_westeurope> \
    --resource-group myResourceGroup

```

## <a name="add-traffic-manager-endpoints"></a>Traffic Manager-eindpunten toevoegen
Voeg de twee Web Apps als Traffic Manager eind punten met [AZ Network Traffic-Manager endpoint Create](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint?view=azure-cli-latest#az-network-traffic-manager-endpoint-create) to the Traffic Manager profile als volgt toe:

- Bepaal de Web-App-ID en voeg de web-app toe die zich bevindt in de Azure-regio *VS-Oost* als het primaire eind punt om alle gebruikers verkeer te routeren. 
- Bepaal de Web-App-ID en voeg de web-app toe die zich bevindt in de Azure-regio *Europa-West* als het failover-eind punt. 

Als het primaire eindpunt niet beschikbaar is, wordt het verkeer automatisch naar het failover-eindpunt gerouteerd.

Vervang in het volgende voor beeld **< app1name_eastus >** en **< app2name_westeurope >** met de namen van de apps die u voor elke regio hebt gemaakt in de vorige sectie, vervang **< appspname_eastus >** en **< appspname_ Europa West >** met de naam die wordt gebruikt voor het maken van de app service plannen in de vorige sectie en vervang < profilecode **>** door de profiel naam die in de vorige sectie wordt gebruikt. 

**VS-Oost-eind punt**

```azurecli-interactive

az webapp show \
    --name <app1name_eastus> \
    --resource-group myResourceGroup \
    --query id

```
Noteer de id die wordt weer gegeven in uitvoer en gebruik in de volgende opdracht om het eind punt toe te voegen:

```azurecli-interactive

az network traffic-manager endpoint create \
    --name <app1name_eastus> \
    --resource-group myResourceGroup \
    --profile-name <profile_name> \
    --type azureEndpoints \
    --target-resource-id <ID from az webapp show> \
    --priority 1 \
    --endpoint-status Enabled
```

**Europa-west-eind punt**

```azurecli-interactive

az webapp show \
    --name <app2name_westeurope> \
    --resource-group myResourceGroup \
    --query id

```
Noteer de id die wordt weer gegeven in uitvoer en gebruik in de volgende opdracht om het eind punt toe te voegen:

```azurecli-interactive

az network traffic-manager endpoint create \
    --name <app1name_westeurope> \
    --resource-group myResourceGroup \
    --profile-name <profile_name> \
    --type azureEndpoints \
    --target-resource-id <ID from az webapp show> \
    --priority 2 \
    --endpoint-status Enabled

```

## <a name="test-your-traffic-manager-profile"></a>Uw Traffic Manager-profiel testen

In deze sectie controleert u de domeinnaam van uw Traffic Manager-profiel. Tevens configureert u het primaire eindpunt zodanig dat het niet beschikbaar is. Ten slotte kunt u zien dat de web-app nog steeds beschikbaar is. Dat komt omdat Traffic Manager het verkeer naar het failover-eindpunt stuurt.

Vervang in het volgende voor beeld **< app1name_eastus >** en **< app2name_westeurope >** met de namen van de apps die u voor elke regio hebt gemaakt in de vorige sectie, vervang **< appspname_eastus >** en **< appspname_ Europa West >** met de naam die wordt gebruikt voor het maken van de app service plannen in de vorige sectie en vervang < profilecode **>** door de profiel naam die in de vorige sectie wordt gebruikt.

### <a name="determine-the-dns-name"></a>DNS-naam bepalen

Bepaal de DNS-naam van het Traffic Manager profiel met [AZ Network Traffic-Manager profile show](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile?view=azure-cli-latest#az-network-traffic-manager-profile-show).

```azurecli-interactive

az network traffic-manager profile show \
    --name <profile_name> \
    --resource-group myResourceGroup \
    --query dnsConfig.fqdn

```

Kopieer de waarde **RelativeDnsName** . De DNS-naam van uw Traffic Manager profiel is *http://<* relativednsname *>. de. net*. 

### <a name="view-traffic-manager-in-action"></a>Traffic Manager in werking zien
1. Voer in een webbrowser de DNS-naam van uw Traffic Manager profiel (*http://<* relativednsname *>. de. net*) in om de standaard website van uw web-app weer te geven.

    > [!NOTE]
    > In dit quickstartscenario worden alle aanvragen gerouteerd naar het primaire eindpunt. Het is ingesteld op **Priority 1**.
2. Als u Traffic Manager failover in actie wilt weer geven, schakelt u de primaire site uit met [AZ Network Traffic-Manager endpoint update](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint?view=azure-cli-latest#az-network-traffic-manager-endpoint-update).

   ```azurecli-interactive

    az network traffic-manager endpoint update \
        --name <app1name_eastus> \
        --resource-group myResourceGroup \
        --profile-name <profile_name> \
        --type azureEndpoints \
        --endpoint-status Disabled
    
   ```

3. Kopieer de DNS-naam van uw Traffic Manager profiel (*http://<* relativednsname *>. de. net*) om de website in een nieuwe webbrowser sessie weer te geven.
4. Controleer of de web-app nog beschikbaar is.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent, verwijdert u de resource groepen, webtoepassingen en alle gerelateerde resources met [AZ Group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete).

```azurepowershell-interactive

az group delete \
    --resource-group myResourceGroup

```

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u een Traffic Manager profiel gemaakt dat hoge Beschik baarheid biedt voor uw webtoepassing. Voor meer informatie over het routeren van verkeer gaat u door naar de zelfstudies voor Traffic Manager.

> [!div class="nextstepaction"]
> [Traffic Manager tutorials](tutorial-traffic-manager-improve-website-response.md) (Traffic Manager-zelfstudies)
