---
title: OMS-portal naar Azure verplaatst | Microsoft Docs
description: De OMS-portal wordt sunsetted met alle functionaliteit verplaatst naar de Azure portal. Dit artikel bevat informatie over deze overgang.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: bwren
ms.openlocfilehash: b4d67b64d8fc7c5063e96ed2a2502e9cbecd6065
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65229879"
---
# <a name="oms-portal-moving-to-azure"></a>OMS-portal naar Azure verplaatsen

> [!NOTE]
> In dit artikel is van toepassing op zowel de openbare cloud van Azure en de government-cloud behalve indien anders vermeld.

**De OMS-portal voor de openbare cloud van Azure is officieel gebruik is gesteld. De OMS-portal voor het Azure US Government-cloud wordt officieel beëindigd op 15 mei 2019.** We zijn trots te verplaatsen naar de Azure portal en de overgang naar eenvoudig worden verwacht. Maar we begrijpen dat wijzigingen zijn moeilijk en kunnen worden ervaren. Verzenden van eventuele vragen, feedback of opmerkingen aan **LAUpgradeFeedback\@microsoft.com**. De rest van dit artikel gaat over de belangrijkste scenario's en het schema voor deze overgang.

De Azure-portal is de hub voor alle Azure-services en biedt een uitgebreide beheerervaring, met mogelijkheden zoals dashboards om vast te zetten resources, intelligent zoeken naar resources zoeken en labels voor resourcebeheer. Om te consolideren en de controle en beheer de werkstroom te stroomlijnen, begonnen we met het toevoegen van de OMS-portal-mogelijkheden in Azure portal. Alle functies van de OMS-portal zijn nu onderdeel van de Azure-portal. Enkele van de nieuwe functies zoals Traffic Analytics zijn in feite alleen beschikbaar in Azure portal. U wordt mogelijk zijn om uit te voeren van alles wat die je mee bezig was in de OMS-portal met de Azure-portal en meer. Als u dit nog niet hebt gedaan, leest u eerst met behulp van de Azure-portal vandaag nog!

## <a name="what-is-changing"></a>Wat is gewijzigd? 
De volgende wijzigingen zijn met de afschaffing van de OMS-portal wordt aangekondigd. Elk van deze wijzigingen wordt beschreven in de secties hieronder in meer detail.

- U kunt nieuwe maken [alleen werkruimten](#new-workspaces) in Azure portal.
- De nieuwe ervaring voor waarschuwingsbeheer [vervangt het beheersysteem voor waarschuwing](#changes-to-alerts).
- [Toegangsbeheer voor gebruiker](#user-access-and-role-migration) wordt nu uitgevoerd in Azure portal met behulp van op rollen gebaseerd toegangsbeheer in Azure.
- De [Application Insights-Connector is niet langer vereist](#application-insights-connector-and-solution) omdat dezelfde functionaliteit wordt ingeschakeld via meerdere werkruimten query's.
- De [mobiele OMS-App](#oms-mobile-app) wordt afgeschaft. 
- De [NSG-oplossing wordt vervangen](#azure-network-security-group-analytics) met uitgebreide functionaliteit die beschikbaar zijn via Traffic Analytics-oplossing.
- Nieuwe verbindingen van System Center Operations Manager naar Log Analytics vereist [bijgewerkt management packs](#system-center-operations-manager).
- Zie [uw OMS-Update-implementaties te migreren naar Azure](../../automation/migrate-oms-update-deployments.md) voor meer informatie over wijzigingen in [updatebeheer](../../automation/automation-update-management.md).


## <a name="what-should-i-do-now"></a>Wat moet ik nu doen?
Hoewel de meeste functies werken blijven zonder dat u een migratie uitvoert, hoeft u de volgende taken uitvoeren:

- U moet [migreren van uw gebruikersmachtigingen](#user-access-and-role-migration) naar de Azure-portal.
- Zie [uw OMS-Update-implementaties te migreren naar Azure](../../automation/migrate-oms-update-deployments.md) voor meer informatie over het overstappen van de oplossing Update Management.

Raadpleeg [Veelgestelde vragen voor overgang van OMS-portal naar Azure-portal voor gebruikers van de Log Analytics](oms-portal-faq.md) voor informatie over het overstappen naar de Azure-portal. Alle feedback, vragen of opmerkingen aan verzenden **LAUpgradeFeedback\@microsoft.com**.

## <a name="user-access-and-role-migration"></a>Toegang en de rol gebruikersmigratie
Beheer van Azure portal toegang is een uitgebreidere en krachtiger zijn dan de toegangsbeheer in de OMS-Portal. Zie [werkruimten beheren](manage-access.md#manage-accounts-and-users) voor meer informatie over toegangsbeheer in Log Analytics.

> [!NOTE]
> Eerdere versies van dit artikel wordt aangegeven dat de machtigingen zou automatisch worden geconverteerd van de OMS-portal naar de Azure-portal. Deze automatische conversie niet meer is gepland en u moet de conversie zelf uitvoeren.

Mogelijk hebt u al bepaalde toegang in Azure portal in welk geval u hoeft te wijzigen. Er zijn een aantal gevallen waar u geen geschikte toegang kan hebben in dat geval uw beheerder moet u machtigingen toewijzen.

- U hebt alleen-lezen machtigingen in de OMS-portal, maar er zijn geen machtigingen in Azure portal. 
- Hebt u bijdragerechten in de OMS-portal, maar alleen leestoegang hebben in Azure portal.
 
In beide gevallen moet de beheerder handmatig toegewezen aan de juiste rol in de volgende tabel. Het is raadzaam dat u deze rol op het niveau van de resource-groep of abonnement toewijzen.  Meer richtlijnen wordt binnenkort voor beide dergelijke gevallen worden opgegeven.

| OMS-portal machtiging | Azure-rol |
|:---|:---|
| ReadOnly | Lezer van Log Analytics |
| Inzender | Inzender van Log Analytics |
| Beheerder | Eigenaar | 
 

## <a name="new-workspaces"></a>Nieuwe werkruimten
U bent niet langer om nieuwe werkruimten met behulp van de OMS-portal te maken. Volg de instructies in [een Log Analytics-werkruimte maken in Azure portal](../learn/quick-create-workspace.md) een nieuwe werkruimte maken in Azure portal.

## <a name="changes-to-alerts"></a>Wijzigingen van waarschuwingen

### <a name="alert-extension"></a>Waarschuwing-extensie  

Waarschuwingen zijn [uitgebreid naar Azure portal](alerts-extend.md) bestaande waarschuwingen worden weergegeven in de OMS-portal, maar u kunt ze alleen beheren in Azure portal. Als u waarschuwingen programmatisch met behulp van de Log Analytics Alert REST API of Log Analytics-waarschuwing Resourcesjabloon opent, moet u actiegroepen gebruiken in plaats van acties in uw API-aanroepen, Azure Resource Manager-sjablonen en PowerShell-opdrachten.

### <a name="alert-management-solution"></a>Oplossing voor waarschuwing
Als een wijziging ten opzichte van de aankondiging in een vorige de [Waarschuwingenbeheeroplossing](alert-management-solution.md) blijft beschikbaar en worden volledig ondersteund in Azure portal. U kunt blijven voor het installeren van de oplossing op Azure Marketplace.

Hoewel het beheersysteem voor waarschuwing blijft beschikbaar is, raden we u aan gebruik [van Azure Monitor unified interface waarschuwingen](alerts-overview.md) te visualiseren en alle meldingen beheren in Azure. Deze nieuwe ervaring aggregeert systeemeigen waarschuwingen uit meerdere bronnen binnen Azure inclusief waarschuwingen van Log Analytics. Als u van geïntegreerde waarschuwingen van Azure Monitor-interface gebruikmaakt, is klikt u vervolgens de oplossing van waarschuwingen te beheren alleen vereist om in te schakelen van de integratie van waarschuwingen van System Center Operation Manager naar Azure. In geïntegreerde waarschuwingen interface van Azure Monitor, kunt u distributies van uw waarschuwingen bekijken, profiteer van geautomatiseerde groeperen van gerelateerde waarschuwingen via slimme groepen en weergeven van waarschuwingen voor meerdere abonnementen terwijl geavanceerde filters zijn toegepast. Toekomstige ontwikkelingen in het waarschuwingsbeheer zijn voornamelijk beschikbaar in deze nieuwe ervaring. 

De gegevens die zijn verzameld door de oplossing van waarschuwingen te beheren (records met een type van de waarschuwing) blijft in Log Analytics, zolang de oplossing voor de werkruimte is geïnstalleerd. 

## <a name="oms-mobile-app"></a>Mobiele OMS-App
De mobiele OMS-app worden sunsetted samen met de OMS-portal. In plaats van de mobiele OMS-app voor toegang tot informatie over uw IT-infrastructuur, dashboards en opgeslagen query's, kunt u de Azure-portal rechtstreeks openen vanuit uw browser in uw mobiele apparaat. Als u waarschuwingen, moet u configureren [Azure-actiegroepen](action-groups.md) voor het ontvangen van meldingen in de vorm van SMS- of een telefoongesprek

## <a name="application-insights-connector-and-solution"></a>Application Insights-Connector en oplossing
[Application Insights-Connector](app-insights-connector.md) biedt een manier om op te nemen van Application Insights-gegevens in een Log Analytics-werkruimte. De duplicatie van deze gegevens is vereist voor het inschakelen van zichtbaarheid in de infrastructuur-en toepassingsgegevens. Met Application Insights uitgebreide ondersteuning voor het bewaren van gegevens in maart 2019 en de mogelijkheid om uit te voeren [query's voor meerdere bronnen](../log-query/cross-workspace-query.md) naast het [meerdere Azure Monitor Application Insights-resources weergeven ](../log-query/unify-app-resource-data.md), hoeft niet te dupliceren van gegevens uit uw Application Insights-resources en deze verzenden naar Log Analytics. Daarnaast wordt de Connector verzendt een subset van de eigenschappen van toepassingen naar Log Analytics, terwijl de query's van meerdere bronnen biedt u flexibiliteit verbeterd.  

Application Insights-Connector wordt als zodanig worden afgeschaft en verwijderd uit Azure Marketplace, samen met de OMS-portal-afschaffing op 30 maart 2019, terwijl bestaande verbindingen blijft tot en met 30 juni 2019. Met OMS portal afschaffing is er geen manier om te configureren en bestaande verbindingen verwijderen uit de portal. Dit wordt ondersteund met de REST-API die beschikbaar zijn in januari, 2019 zullen worden gemaakt en een melding wordt gepost op [Azure-updates](https://azure.microsoft.com/updates/). 

## <a name="azure-network-security-group-analytics"></a>Analyse van Azure-netwerkbeveiligingsgroep
De [oplossing Azure Network Security Group Analytics](../insights/azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-azure-monitor) wordt vervangen met onlangs geopende [Traffic Analytics](https://azure.microsoft.com/blog/traffic-analytics-in-preview/) waarmee u inzicht in gebruikers en toepassingen op cloudnetwerken. Traffic Analytics kunt u controle-activiteit op het netwerk, beveiligde toepassingen en gegevens van uw organisatie, werkbelasting prestaties te optimaliseren en blijven voldoen. 

Deze oplossing analyseert Stroomlogboeken logboeken en biedt inzicht in de volgende.

- Verkeer stroomt tussen uw netwerken tussen Azure en Internet, openbare cloud-regio's, VNETs en subnetten.
- Toepassingen en protocollen in uw netwerk, zonder de noodzaak voor sniffers of toegewezen stroom verzameling apparaten.
- Belangrijke gebruikers, intensieve toepassingen, VM-gesprekken in de cloud, verkeer hotspots.
- Bronnen en bestemmingen van verkeer in vnet's, tussen relaties tussen kritische zakelijke services en toepassingen.
- Beveiliging met inbegrip van schadelijk verkeer, poorten openen op het Internet, toepassingen of virtuele machines die probeert toegang tot Internet.
- Gebruik van capaciteit, waarmee u problemen met inrichten of ondermaats gebruik elimineren.

U kunt blijven zijn afhankelijk van instellingen voor diagnostische gegevens voor het verzenden van NSG-logboeken naar Log Analytics, zodat uw bestaande zoekopdrachten, waarschuwingen, dashboards opgeslagen blijven werken. Klanten die de oplossing al hebt geïnstalleerd kunnen echter ook doorgaan met het tot nadere kennisgeving. Vanaf 5 September, de Network Security Group Analytics-oplossing wordt verwijderd uit de marketplace en beschikbaar gesteld via de community als een [Azure QuickStart-sjabloon](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Operationalinsights).

## <a name="system-center-operations-manager"></a>System Center Operations Manager
Als u hebt [uw Operations Manager-beheergroep verbonden met Log Analytics](om-agents.md), en vervolgens deze blijven werken zonder te wijzigen. Voor nieuwe verbindingen echter, u moet volgen de richtlijnen in [Microsoft System Center Operations Manager Management Pack voor het configureren van Operations Management Suite](https://blogs.technet.microsoft.com/momteam/2018/07/25/microsoft-system-center-operations-manager-management-pack-to-configure-operations-management-suite/).

## <a name="next-steps"></a>Volgende stappen
- Zie [Veelgestelde vragen voor overgang van OMS-portal naar Azure-portal voor gebruikers van de Log Analytics](oms-portal-faq.md) voor hulp bij het verplaatsen van de OMS-portal naar de Azure-portal.
