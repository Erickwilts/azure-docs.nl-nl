---
title: Een root-of Apex-domein onboarden naar een bestaande front-deur-Azure Portal
description: Meer informatie over het voorbereiden van een root-of Apex-domein naar een bestaande front-deur met behulp van de Azure Portal.
services: front-door
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 5/21/2019
ms.author: duau
ms.openlocfilehash: 05267ad43f6e7f89ec50b1765d2475a02fae1702
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/02/2020
ms.locfileid: "89399572"
---
# <a name="onboard-a-root-or-apex-domain-on-your-front-door"></a>Onboarding van een basis- of hoofddomein op uw Front door
Azure front-deur gebruikt CNAME-records om het domein eigendom te valideren voor de onboarding van aangepaste domeinen. Bovendien geeft front deur niet het frontend-IP-adres weer dat is gekoppeld aan uw front deur-profiel. u kunt uw Apex-domein niet toewijzen aan een IP-adres als het doel is om het in te stellen op Azure front deur.

Het DNS-protocol voor komt dat de toewijzing van CNAME-records op de zone Apex. Als uw domein bijvoorbeeld is, `contoso.com` kunt u CNAME-records maken voor `somelabel.contoso.com` , maar niet voor zichzelf een CNAME maken `contoso.com` . Deze beperking geeft een probleem met de eigen aren van toepassingen die toepassingen met taak verdeling achter de Azure front-deur hebben. Omdat voor het gebruik van een front-deur profiel een CNAME-record moet worden gemaakt, is het niet mogelijk om naar het voorste deur Profiel van de zone Apex te wijzen.

Dit probleem wordt opgelost met behulp van alias records op Azure DNS. In tegens telling tot CNAME-records worden alias records gemaakt op de zone Apex en de eigen aren van toepassingen kunnen deze gebruiken om hun zone Apex-record te laten wijzen naar een front-deur profiel met open bare eind punten. Toepassings eigenaren verwijzen naar hetzelfde deur profiel dat wordt gebruikt voor elk ander domein binnen hun DNS-zone. `contoso.com`En `www.contoso.com` kunnen bijvoorbeeld verwijzen naar hetzelfde deur profiel voor beide. 

Voor het toewijzen van uw Apex of hoofd domein aan het voorste deur profiel is in principe CNAME-afvlakking of DNS-Chasing vereist. Dit is een mechanisme waarbij de CNAME-vermelding in de DNS-provider recursief wordt opgelost, totdat deze een IP-adres heeft gevonden. Deze functionaliteit wordt ondersteund door Azure DNS voor eind punten van de voor deur. 

> [!NOTE]
> Er zijn ook andere DNS-providers die ondersteuning bieden voor CNAME afvlakking of DNS-Chasing, maar Azure front-deur raadt u aan om Azure DNS voor de klanten te gebruiken voor het hosten van hun domeinen.

U kunt de Azure Portal gebruiken om een Apex-domein op uw voor deur uit te voeren en HTTPS in te scha kelen door het te koppelen aan een certificaat voor TLS-beëindiging. Apex-domeinen worden ook wel root-of onwaar-domeinen genoemd.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een alias record maken die naar uw voorste deur profiel wijst
> * Het hoofd domein toevoegen aan de voor deur
> * HTTPS instellen op het hoofd domein

> [!NOTE]
> Voor deze zelf studie moet u al een voor deur profiel maken. Raadpleeg andere zelf studies zoals [Quick Start: Maak een front](./quickstart-create-front-door.md) -deur of [Maak een voor deur met HTTP-naar-https-omleiding](./front-door-how-to-redirect-https.md) om aan de slag te gaan.

## <a name="create-an-alias-record-for-zone-apex"></a>Een alias record maken voor de zone Apex

1. Open **Azure DNS** configuratie van het domein dat u onboarding wilt uitvoeren.
2. De record voor de zone Apex maken of bewerken.
3. Selecteer het record **type** als _record_ en selecteer vervolgens _Ja_ voor **alias records instellen**. Het **alias type** moet worden ingesteld op de _Azure-resource_.
4. Kies het Azure-abonnement waarin uw front-deur profiel wordt gehost en selecteer vervolgens de voor deur van de **Azure-resource** vervolg keuzelijst.
5. Klik op **OK** om uw wijzigingen in te dienen.

    ![Alias record voor zone Apex](./media/front-door-apex-domain/front-door-apex-alias-record.png)

6. Met de bovenstaande stap maakt u een zone Apex-record die verwijst naar de voor deur en ook een CNAME-record met de naam ' afdverify ' (bijvoorbeeld `afdverify.contosonews.com` ) waarmee `afdverify.<name>.azurefd.net` het domein op uw front-deur profiel wordt gebruikt.

## <a name="onboard-the-custom-domain-on-your-front-door"></a>Het aangepaste domein onboarden op de voor deur

1. Klik op het tabblad front deur Designer op het pictogram ' + ' op de sectie frontend-hosts om een nieuw aangepast domein toe te voegen.
2. Voer de root-of Apex-domein naam in het veld aangepaste hostnaam in, bijvoorbeeld `contosonews.com` .
3. Zodra de CNAME-toewijzing van het domein naar uw voor deur is gevalideerd, klikt u op **toevoegen** om het aangepaste domein toe te voegen.
4. Klik op **Opslaan** om de wijzigingen te verzenden.

![Menu voor aangepaste domeinen](./media/front-door-apex-domain/front-door-onboard-apex-domain.png)

## <a name="enable-https-on-your-custom-domain"></a>HTTPS inschakelen voor uw aangepaste domein

1. Klik op het aangepaste domein dat is toegevoegd en klik onder de sectie HTTPS van het **aangepaste domein**, wijzig de status in **ingeschakeld**.
2. Selecteer het  **type certificaat beheer** om _mijn eigen certificaat te gebruiken_.

> [!WARNING]
> Het beheer type van de front-deur wordt momenteel niet ondersteund voor Apex-of hoofd domeinen. De enige optie die beschikbaar is om HTTPS in te scha kelen op een Apex of hoofd domein voor de voor deur, gebruikt uw eigen aangepaste TLS/SSL-certificaat dat wordt gehost op Azure Key Vault.

3. Zorg ervoor dat u de juiste machtigingen hebt ingesteld voor de voor deur om toegang te krijgen tot uw sleutel kluis, zoals vermeld in de gebruikers interface, voordat u verdergaat met de volgende stap.
4. Kies een **Key Vault-account** in uw huidige abonnement en selecteer vervolgens de juiste **geheime** en **geheime versie** om aan het juiste certificaat toe te wijzen.
5. Klik op **bijwerken** om de selectie op te slaan en klik vervolgens op **Opslaan**.
6. Klik na een paar minuten op **vernieuwen** en klik vervolgens nogmaals op het aangepaste domein om de voortgang van het inrichten van het certificaat te bekijken. 

> [!WARNING]
> Zorg ervoor dat u de juiste routerings regels hebt gemaakt voor het Apex-domein of het domein aan bestaande routerings regels hebt toegevoegd.

## <a name="next-steps"></a>Volgende stappen

- Lees hoe u [een Front Door maakt](quickstart-create-front-door.md).
- Lees [hoe Front Door werkt](front-door-routing-architecture.md).