---
title: Overzicht van Azure-beleid
description: Azure Policy is een service in Azure die u gebruikt om beleidsdefinities in uw Azure-omgeving te maken, toe te wijzen en te beheren.
author: DCtheGeek
ms.author: dacoulte
ms.date: 12/06/2018
ms.topic: overview
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: da045a561fba81bf9a5e412a6f2bf0a6160acfa7
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807450"
---
# <a name="overview-of-the-azure-policy-service"></a>Overzicht van de Azure Policy-service

Governance valideert dat uw organisatie van de bijbehorende doelstellingen door effectief en efficiënt gebruik van bereiken kan IT. Het zorgt voor duidelijkheid tussen zakelijke doelstellingen en IT-projecten.

Ondervindt uw bedrijf een significant aantal IT-problemen die nooit lijken te worden opgelost?
Goed IT-beheer omvat het plannen van uw initiatieven en het stellen van prioriteiten op strategisch niveau, zodat problemen beheerst en voorkomen kunnen worden. Hiervoor gebruikt u Azure Policy.

Azure Policy is een service in Azure die u gebruikt om beleidsregels te maken, toe te wijzen en te beheren. Met deze beleidsregels worden verschillende regels en effecten afgedwongen voor uw resources, zodat deze resources voldoen aan de standaarden en service level agreements in uw bedrijf. Azure Policy voorziet in deze behoefte door uw resources met toegewezen beleid te controleren op niet-naleving. U kunt bijvoorbeeld beleid hebben op basis waarvan alleen virtuele machines van een bepaalde SKU-grootte in uw omgeving zijn toegestaan. Wanneer dit beleid is geïmplementeerd, worden nieuwe en bestaande resources gecontroleerd op naleving. Met het juiste type beleid kunt u ervoor zorgen dat bestaande resources conform zijn. Verderop in deze documentatie wordt dieper ingegaan op het maken en implementeren van beleidsregels met Azure Policy.

> [!IMPORTANT]
> Evaluatie van de naleving van Azure Policy is nu beschikbaar voor alle toewijzingen, ongeacht de prijscategorie. Als uw toewijzingen de nalevingsgegevens niet weergeven, controleert u of het abonnement is geregistreerd bij de resourceprovider Microsoft.PolicyInsights.

[!INCLUDE [service-provider-management-toolkit](../../../includes/azure-lighthouse-supported-service.md)]

## <a name="how-is-it-different-from-rbac"></a>Wat is het verschil met RBAC?

Er zijn enkele belangrijke verschillen tussen Azure Policy en op rollen gebaseerd toegangsbeheer (RBAC). RBAC is gericht op de acties van gebruikers in verschillende bereiken. Mogelijk wordt u voor een resourcegroep toegevoegd aan de rol Inzender, zodat u wijzigingen kunt aanbrengen in die resourcegroep. Azure Policy-richt zich op de resource-eigenschappen tijdens de implementatie en voor al bestaande resources. Azure Policy bepaalt de eigenschappen op, zoals de typen of de locaties van resources. In tegenstelling tot RBAC, Azure Policy is een standaardsysteem voor toestaan en expliciet weigeren.

### <a name="rbac-permissions-in-azure-policy"></a>RBAC-machtigingen in Azure Policy

Azure Policy heeft diverse machtigingen, oftewel bewerkingen, in twee verschillende resourceproviders:

- [Microsoft.Authorization](../../role-based-access-control/resource-provider-operations.md#microsoftauthorization)
- [Microsoft.PolicyInsights](../../role-based-access-control/resource-provider-operations.md#microsoftpolicyinsights)

Veel ingebouwde rollen wijzen machtigingen toe aan Azure Policy-resources. De **Inzender voor Resourcebeleid (Preview)** rol omvat de meeste Azure Policy-bewerkingen. Degene met de rol **Eigenaar** heeft volledige rechten. Beide **Inzender** en **lezer** kunt alle Azure Policy leesbewerkingen, maar **Inzender** kunnen ook herstel activeren.

Als geen van de ingebouwde rollen de vereiste machtigingen heeft, maakt u een [aangepaste rol](../../role-based-access-control/custom-roles.md).

## <a name="policy-definition"></a>Beleidsdefinitie

Het maken en implementeren van een beleidsregel in Azure Policy begint met het maken van een beleidsdefinitie. Elke beleidsdefinitie heeft voorwaarden voor het afdwingen ervan. Er is een bijbehorend effect dat wordt uitgevoerd als aan de voorwaarden wordt voldaan.

In Azure Policy wordt een aantal ingebouwde beleidsregels geboden dat standaard beschikbaar is. Bijvoorbeeld:

- **SQL Server 12.0 vereisen**: Hiermee controleert u of alle SQL-servers gebruikmaken van versie 12.0. Het bijbehorende effect is om alle servers te weigeren die niet voldoen aan deze criteria.
- **Toegestane opslagaccount-SKU's**: Bepaalt of opslagaccounts die worden geïmplementeerd zich in een set SKU-grootten bevinden. Het bijbehorende effect is om alle opslagaccounts te weigeren die niet in overeenstemming zijn met de gedefinieerde SKU-grootten.
- **Toegestaan resourcetype**: Hiermee definieert u de resourcetypen die u kunt implementeren. Het bijbehorende effect is om alle resources te weigeren die geen deel uitmaken van deze gedefinieerde lijst.
- **Toegestane locaties**: Hiermee beperkt u de beschikbare locaties voor nieuwe resources. Het bijbehorende effect wordt gebruikt om uw geografisch nalevingsvereisten af te dwingen.
- **Toegestane SKU's van virtuele machines**: Hiermee wordt een set SKU's voor virtuele machines gespecificeerd die u kunt implementeren.
- **Tag met standaardwaarde toepassen**: Hiermee wordt een vereiste tag met bijbehorende standaardwaarde toegepast als de tag niet is opgegeven tijdens de implementatieaanvraag.
- **Tag en waarde afdwingen**: Hiermee wordt een vereiste tag met de bijbehorende waarde afgedwongen in een resource.
- **Niet toegestane resourcetypen**: Hiermee voorkomt u dat een lijst met resourcetypen wordt geïmplementeerd.

Als u deze beleidsdefinities (zowel de ingebouwde als de aangepaste) wilt implementeren, dient u ze eerst toe te wijzen. U kunt elk van deze typen beleid toewijzen via Azure Portal, PowerShell of Azure CLI.

Beleid wordt beoordeeld bij verschillende acties, zoals het toewijzen of bijwerken van beleidsregels. Zie voor een volledige lijst [Beleidbeoordelingstriggers](./how-to/get-compliance-data.md#evaluation-triggers).

Raadpleeg [Structuur van beleidsdefinities](./concepts/definition-structure.md) voor meer informatie over de structuur van beleidsdefinities.

## <a name="policy-assignment"></a>Beleidstoewijzing

Een beleidstoewijzing is een beleidsdefinitie die is toegewezen om te worden toegepast binnen een bepaald bereik. Dit bereik kan variëren van een [beheergroep](../management-groups/overview.md) tot een resourcegroep. De term *bereik* verwijst naar alle resourcegroepen, abonnementen of beheergroepen waaraan de beleidsdefinitie is toegewezen. Beleidstoewijzingen worden overgenomen door alle onderliggende resources. Dit betekent dat als beleid wordt toegepast op een resourcegroep, dit beleid ook van toepassing is op alle resources in de resourcegroep. U kunt echter een subbereik uitsluiten van een beleidstoewijzing.

U kunt op het abonnementsbereik bijvoorbeeld een beleid toepassen op basis waarvan het maken van netwerkresources wordt voorkomen. U kunt echter één resourcegroep binnen het abonnement uitsluiten, namelijk degene die is bedoeld voor netwerkinfrastructuur. U verleent vervolgens toegang tot deze netwerkresourcegroep aan gebruikers aan wie u het maken van de netwerkresourcegroep toevertrouwt.

In een ander voorbeeld wilt u mogelijk een resource toewijzen type lijst toestaan op niveau van de beheergroep. En vervolgens wilt u een ruimer beleid (zodat meer resourcetypen zijn toegestaan) toewijzen via een onderliggende beheergroep of zelfs rechtstreeks in abonnementen. Dit voorbeeld zou echter niet goed werken, omdat beleid een expliciet weigersysteem is. In plaats daarvan moet u de onderliggende beheergroep of het abonnement uitsluiten van de beleidstoewijzing op beheergroepniveau. Vervolgens kunt u het ruimere beleid toewijzen aan het niveau van de onderliggende beheergroep of het abonnement. Als beleidsresultaten in een resource worden geweigerd, vormt het aanpassen van het weigeringsbeleid de enige manier om de resource toe te staan.

Zie [Een beleidstoewijzing maken om niet-compatibele resources in uw Azure-abonnement te identificeren](assign-policy-portal.md) voor meer informatie over het instellen van beleidsdefinities en -toewijzingen via de portal. Er zijn ook stappen voor [PowerShell](assign-policy-powershell.md) en [Azure CLI](assign-policy-azurecli.md) beschikbaar.

## <a name="policy-parameters"></a>Beleidsparameters

Beleidsparameters helpen uw beleidsbeheer te vereenvoudigen door het aantal beleidsdefinities te verminderen dat u moet maken. U kunt bij het maken van een beleidsdefinitie parameters definiëren om deze algemener te maken. Vervolgens kunt u deze beleidsdefinitie hergebruiken voor verschillende scenario's. Dit doet u door verschillende waarden door te voeren bij het toepassen van de beleidsdefinitie. Bijvoorbeeld, door een reeks locaties voor een abonnement op te geven.

Parameters worden gedefinieerd bij het maken van een beleidsdefinitie. Als een parameter wordt gedefinieerd, krijgt deze een naam en optioneel een waarde. U kunt bijvoorbeeld een parameter definiëren voor een beleid met de naam *locatie*. Vervolgens kunt u deze verschillende waarden toekennen, zoals *EastUS* of *WestUS* bij het toewijzen van beleid.

Zie [Definitiestructuur - parameters](./concepts/definition-structure.md#parameters) voor meer informatie over beleidsparameters.

## <a name="initiative-definition"></a>Initiatiefdefinitie

Een initiatiefdefinitie is een verzameling beleidsdefinities die zijn aangepast voor het bereiken van één overkoepelend doel. Initiatiefdefinities maken het beheren en toewijzen van beleidsdefinities eenvoudiger. Dit gebeurt door het groeperen van een reeks beleidsregels als één item. U kunt bijvoorbeeld een initiatief maken met de titel **Controleren inschakelen in Azure Security Center** met als doel om alle beschikbare beveiligingsaanbevelingen in uw Azure Security Center te controleren.

Onder dit initiatief vallen beleidsdefinities zoals:

- **Niet-versleutelde SQL-database controleren in Security Center** – Voor het controleren van niet-versleutelde SQL-databases en -servers.
- **OS-kwetsbaarheden controleren in Security Center**: voor het controleren van servers die niet voldoen aan de geconfigureerde uitgangswaarde.
- **Ontbrekende Endpoint Protection controleren in Security Center** – Voor het controleren van servers zonder een geïnstalleerde Endpoint Protection-agent.

## <a name="initiative-assignment"></a>Initiatieftoewijzing

Net zoals een beleidstoewijzing is een initiatieftoewijzing een initiatiefdefinitie die is toegewezen aan een bepaald bereik. Initiatieftoewijzingen verminderen de noodzaak om verschillende initiatiefdefinities te maken voor elk bereik. Dit bereik kan ook variëren van een beheergroep tot een resourcegroep.

Elk initiatief kan aan meerdere bereiken worden toegewezen. Eén initiatief kan zowel aan **subscriptionA** als aan **subscriptionB** worden toegewezen.

## <a name="initiative-parameters"></a>Initiatiefparameters

Net zoals beleidsparameters helpen initiatiefparameters om initiatiefbeheer te vereenvoudigen door redundantie te verminderen. Initiatiefparameters zijn de parameters die worden gebruikt voor de beleidsdefinities binnen het initiatief.

Neem bijvoorbeeld een scenario met initiatiefdefinitie **initiativeC**, waarbij voor beleidsdefinities **policyA** en **policyB** elk een ander type parameter wordt verwacht:

| Beleid | Naam van parameter |Type parameter  |Opmerking |
|---|---|---|---|
| policyA | allowedLocations | matrix  |Op basis van deze parameter wordt een lijst met tekenreeksen verwacht voor een waarde, omdat het parametertype is gedefinieerd als een matrix |
| policyB | allowedSingleLocation |tekenreeks |Op basis van deze parameter wordt één woord verwacht voor een waarde, omdat het parametertype is gedefinieerd als een tekenreeks |

In dit scenario hebt u, bij het definiëren van de initiatiefparameters voor **initiativeC**, drie opties:

- Gebruik de parameters van de beleidsdefinities binnen dit initiatief: in dit voorbeeld worden *allowedLocations* en *allowedSingleLocation* initiatiefparameters voor **initiativeC** .
- Geef waarden op voor de parameters van de beleidsdefinities binnen deze initiatiefdefinitie. In dit voorbeeld kunt u een lijst met locaties opgeven naar **parameter van policyA – allowedLocations** en **parameter van policyB – allowedSingleLocation**. U kunt ook waarden opgeven bij het toewijzen van dit initiatief.
- Geef een lijst met *waarde*opties op die kunnen worden gebruikt bij het toewijzen van dit initiatief. Als u dit initiatief toewijst, kunnen de overgenomen parameters van de beleidsdefinities binnen het initiatief, alleen waarden hebben uit de opgegeven lijst.

Als u waardeopties maakt in een initiatiefdefinitie, is het niet mogelijk om tijdens het toewijzen van het initiatief andere waarden op te geven, omdat deze niet op de lijst staan.

## <a name="maximum-count-of-azure-policy-objects"></a>Maximum aantal objecten dat Azure Policy

[!INCLUDE [policy-limits](../../../includes/azure-policy-limits.md)]

## <a name="recommendations-for-managing-policies"></a>Aanbevelingen voor het beheren van beleid

Enkele tips om rekening mee te houden:

- Begin met een controle-effect in plaats van een weigeringseffect om bij te houden wat de invloed van uw beleidsdefinitie is op de resources in uw omgeving. Als u al actieve scripts hebt voor automatische schaalaanpassing van uw toepassingen, kan het instellen van een weigeractie een belemmering vormen voor deze automatische taken die u al hebt geïmplementeerd.

- U kunt bij het maken van definities en toewijzingen gebruikmaken van organisatiehiërarchieën. Het is raadzaam om op een hoger niveau definities te maken, zoals op het niveau van de beheergroep of het abonnement. Vervolgens maakt u de toewijzing op het eerstvolgende onderliggende niveau. Als u een definitie voor een beheergroep maakt, kan de toewijzing worden gericht op een abonnement of resource in die beheergroep.

- Het is raadzaam om initiatiefdefinities te maken en toe te wijzen, zelfs voor slechts één beleidsdefinitie.
U hebt bijvoorbeeld de beleidsdefinitie *policyDefA* en u maakt deze met de initiatiefdefinitie *initiativeDefC*. Als u later nog een beleidsdefinitie maakt voor *policyDefB*, met doelen die lijken op die van *policyDefA*, kunt u deze toevoegen bij *initiativeDefC* en ze samen volgen.

- Als u een initiatieftoewijzing hebt gemaakt, worden beleidsdefinities die worden toegevoegd aan het initiatief ook deel van de toewijzingen van het initiatief.

- Wanneer een initiatieftoewijzing wordt beoordeeld, worden alle beleidsregels in het initiatief ook beoordeeld. Als u een beleidsregel afzonderlijk wilt beoordelen, is het beter deze niet op te nemen in een initiatief.

## <a name="video-overview"></a>Video-overzicht

Het volgende overzicht van Azure Policy is afkomstig van build 2018. Voor het downloaden van dia's of video's, gaat u naar [Azure-omgeving beheren via Azure Policy](https://channel9.msdn.com/events/Build/2018/THR2030) op Channel 9.

> [!VIDEO https://www.youtube.com/embed/dxMaYF2GB7o]

## <a name="next-steps"></a>Volgende stappen

Nu u een overzicht hebt van Azure Policy en enkele van de belangrijkste geïntroduceerde concepten, ziet u hier de voorgestelde volgende stappen:

- [Een beleidsdefinitie met behulp van de portal toewijzen](assign-policy-portal.md).
- [Een beleidsdefinitie met de Azure CLI toewijzen](assign-policy-azurecli.md).
- [Een beleidsdefinitie met behulp van PowerShell toewijzen](assign-policy-powershell.md).
- Lees wat een beheergroep met is [organiseren van uw resources met Azure-beheergroepen](..//management-groups/overview.md).
- Weergave [regelen van uw Azure-omgeving via Azure Policy](https://channel9.msdn.com/events/Build/2018/THR2030) op Channel 9.