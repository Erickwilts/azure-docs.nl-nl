---
title: Over Azure Migrate
description: Meer informatie over de Azure Migrate-service.
ms.topic: overview
ms.date: 04/15/2020
ms.custom: mvc
ms.openlocfilehash: 72f2dfb3e5e05466892d838af4e1c84c855b94a9
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2020
ms.locfileid: "86122078"
---
# <a name="about-azure-migrate"></a>Over Azure Migrate

In dit artikel vindt u een kort overzicht van de service Azure Migrate.

Azure Migrate is een gecentraliseerde hub voor het beoordelen en migreren naar on-premises servers, infrastructuur, toepassingen en gegevens in Azure. Deze biedt het volgende:

- **Geïntegreerd migratieplatform**: Eén portal om uw migratie naar Azure te starten, uit te voeren en te volgen.
- **Aantal hulpprogramma's**: Een reeks hulpprogramma's voor evaluatie en migratie. Azure Migrate-hulpprogramma’s omvatten Server Assessment en Azure Migrate: Server Migration. Azure Migrate integreert ook met andere Azure-services en -hulpprogramma's, en met aanbiedingen van onafhankelijke softwareleveranciers (ISV’s).
- **Evaluatie en migratie**: In de Azure Migrate hub kunt u het volgende evalueren en migreren:
    - **Servers**: Evalueer on-premises servers en migreer deze naar virtuele Azure-machine of Azure VMware Solution Solution (AVS) (Preview).
    - **Databases**: Evalueer on-premises databases en migreer deze naar Azure SQL Database of SQL Managed Instance.
    - **Webtoepassingen**: Evalueer on-premises webtoepassingen en migreer deze naar Azure App Service met behulp van de Azure App Service Migration Assistant.
    - **Virtuele bureaubladen**: Evalueer uw on-premises VDI (Virtual Desktop Infrastructure) en migreer deze naar Windows Virtual Desktop in Azure.
    - **Gegevens**: Migreer grote hoeveelheden gegevens snel en tegen aanvaardbare kosten naar Azure met behulp van Azure Data Box-producten.

## <a name="integrated-tools"></a>Geïntegreerde hulpprogramma's

De Azure Migrate-hub bevat deze hulpprogramma's:

**Hulpprogramma** | **Evalueren en migreren** | **Details**
--- | --- | ---
**Azure Migrate: Server Assessment** | Servers evalueren. | Detecteer en evalueer on-premises VMware-VM's, Hyper-V-VM's en fysieke servers in voorbereiding op migratie naar Azure.
**Azure Migrate: Server Migration** | Servers migreren. | Migreer VMware-VM's, Hyper-V-VM's, fysieke servers, andere gevirtualiseerde machines en VM's ui de openbare cloud naar Azure.
**Gegevensmigratieassistent** | Evalueer SQL Server-databases voor migratie naar Azure SQL Database, Azure SQL Managed Instance of Azure-VM's waarop SQL Server wordt uitgevoerd. | Data Migration Assistant helpt bij het opsporen van mogelijke problemen die de migratie kunnen tegenhouden. De assistent identificeert niet-ondersteunde functies, nieuwe functies die na de migratie van pas kunnen komen en het juiste traject voor databasemigratie. [Meer informatie](/sql/dma/dma-overview?view=sql-server-2017).
**Azure Database Migration Service** | Migreer on-premises databases naar Azure-VM's waarop SQL Server, Azure SQL Database of SQL Managed Instance wordt uitgevoerd. | [Lees meer](../dms/dms-overview.md) over Database Migration Service.
**Movere** | Servers evalueren. | [Lees meer](#movere) over Movere.
**Web App Migration Assistant** | Hiermee kunt u on-premises web-apps beoordelen en migreren naar Azure. |  Gebruik Azure App Service Migration Assistant om on-premises websites te evalueren voor migratie naar Azure App Service.<br/><br/> Gebruik Migration Assistant om .NET- en PHP-web-apps te migreren naar Azure. [Lees meer](https://appmigration.microsoft.com/) over Azure App Service Migration Assistant.
**Azure Data Box** | Offline gegevens migreren. | Gebruik Azure Data Box-producten om grote hoeveelheden offline gegevens naar Azure te verplaatsen. [Meer informatie](../databox/index.yml).

> [!NOTE]
> Als u Azure Government gebruikt, kunnen externe geïntegreerde hulpprogramma's en ISV-aanbiedingen geen gegevens verzenden naar Azure Migrate-projecten. U kunt hulpprogramma's onafhankelijk van elkaar gebruiken.

## <a name="isv-integration"></a>ISV-integratie

Azure Migrate kan worden geïntegreerd met verschillende ISV-aanbiedingen. 

**ISV**    | **Functie**
--- | ---
[Carbonite](https://www.carbonite.com/globalassets/files/datasheets/carb-migrate4azure-microsoft-ds.pdf) | Servers migreren.
[Cloudamize](https://www.cloudamize.com/platform) | Servers evalueren.
[Corent Technology](https://www.corenttech.com/AzureMigrate/) | Servers evalueren en migreren.
[Device42](https://docs.device42.com/) | Servers evalueren.
[Lakeside](https://go.microsoft.com/fwlink/?linkid=2104908) | VDI evalueren.
[RackWare](https://go.microsoft.com/fwlink/?linkid=2102735) | Servers migreren.
[Turbonomic](https://learn.turbonomic.com/azure-migrate-portal-free-trial) | Servers evalueren.
[UnifyCloud](https://www.cloudatlasinc.com/cloudrecon/) | Servers en databases evalueren.

## <a name="azure-migrate-server-assessment-tool"></a>Azure Migrate: Server Assessment

Het hulpprogramma Azure Migrate: Server Assessment detecteert en evalueert on-premises VMware-VM's, Hyper-V-VM's en fysieke servers voor migratie naar Azure. 

Het hulpprogramma doet het volgende:

- **Azure-gereedheid**: Evalueert of on-premises machines gereed zijn voor migratie naar Azure.
- **Azure-groottebepaling**: Evalueer on-premises servers en migreer deze naar virtuele Azure-machine of Azure VMware Solution (AVS) (Preview).
- **Azure-kostenraming**: Schat de kosten voor het uitvoeren van on-premises servers in Azure.
- **Afhankelijkheidsanalyse**: Identificeert afhankelijkheden tussen servers en optimalisatiestrategieën voor het verplaatsen van gerelateerde servers naar Azure. Leer meer over Server Assessment met [afhankelijkheidsanalyse](concepts-dependency-visualization.md).

Server Assessment maakt gebruik van een lichtgewicht [Azure Migrate-apparaat](migrate-appliance.md) dat u on-premises implementeert.

- Het apparaat wordt uitgevoerd op een VM of op een fysieke server. U kunt het apparaat eenvoudig installeren met behulp van een gedownloade sjabloon.
- Het apparaat detecteert on-premises machines. Ook worden er voortdurend metagegevens en prestatiegegevens van de machine naar Azure Migrate verzonden.
- De detectie van apparaten verloopt zonder agent. Er wordt niets geïnstalleerd op gedetecteerde machines.
- Na de detectie van het apparaat kunt u gedetecteerde machines in groepen verzamelen en evaluaties uitvoeren voor elke groep.

## <a name="azure-migrate-server-migration-tool"></a>Azure Migrate: Hulpprogramma voor de migratie van servers

Het hulpprogramma Azure Migrate: Server Migration helpt bij het migreren van deze componenten naar Azure:

**Migreren** | **Details**
--- | ---
On-premises VMware VM's | Migreer VM's naar Azure via migratie met of zonder agent.<br/><br/> Voor migratie zonder agent maakt Server Migration gebruik van een Azure Migrate-apparaat dat u on-premises implementeert. Dit is hetzelfde soort apparaat dat u gebruikt voor Server Assessment.<br/><br/> Voor migratie met agent maakt Server Assessment gebruik van een replicatie-apparaat.
On-premises Hyper-V-VM's | Migreer VM’s naar Azure.<br/><br/> Voor de migratie maakt Server Assessment gebruik van provideragents die op de Hyper-V-host zijn geïnstalleerd.
On-premises fysieke servers | U kunt fysieke machines migreren naar Azure. U kunt ook andere gevirtualiseerde machines migreren, en VM’s uit andere openbare clouds, door ze als virtuele machines te behandelen voor het doel van migratie. | Voor de migratie maakt Server Assessment gebruik van een replicatie-apparaat.


## <a name="selecting-assessment-and-migration-tools"></a>Hulpprogramma's voor evaluatie en migratie selecteren

In de Azure Migrate-hub selecteert u het hulpprogramma dat u wilt gebruiken voor evaluatie of migratie en voegt u dit toe aan een Azure Migrate-project. Als u een ISV-hulpprogramma of Movere toevoegt:

- Als u aan de slag wilt, moet u een licentie aanvragen of u registreren voor een gratis proefversie door de instructies van het hulpprogramma te volgen. Elke ISV of elk hulpprogramma geeft aan welke licentie nodig is.
- Elk hulpprogramma heeft een optie om verbinding te maken met Azure Migrate. Volg de instructies van het hulpprogramma om verbinding te maken.
- Volg uw migratie binnen alle hulpprogramma's vanuit het Azure Migrate-project.

## <a name="movere"></a>Movere

Movere is een SaaS-platform (Software as a Service). Het verbetert de business intelligence door binnen een dag een nauwkeurig beeld te geven van volledige IT-omgevingen. Organisaties en ondernemingen groeien, veranderen en kiezen voor digitale optimalisatie. Tijdens deze processen zorgt Movere ervoor dat ze met het nodige vertrouwen hun omgevingen kunnen visualiseren en beheren, ongeacht het platform, de toepassing of geografie.

Microsoft heeft Movere [overgenomen](https://azure.microsoft.com/blog/microsoft-acquires-movere-to-help-customers-unlock-cloud-innovation-with-seamless-migration-tools/) en het product wordt niet meer als zelfstandige oplossing aangeboden. Movere is beschikbaar via Microsoft Solution Assessment en het Microsoft Cloud Economics-programma. [Lees meer](https://www.movere.io) over Movere.

We raden u aan om ook eens te kijken naar Azure Migrate, onze ingebouwde migratieservice. Azure Migrate biedt een centrale hub om uw cloudmigratie te vereenvoudigen. De hub biedt uitgebreide ondersteuning voor werkbelastingen zoals fysieke en virtuele servers, databases en toepassingen. Met end-to-end zichtbaarheid kunt u de voortgang tijdens detectie, evaluatie en migratie eenvoudig volgen.

Zowel de hulpprogramma's van Azure als van partner-ISV's zijn ingebouwd, waardoor Azure Migrate een uitgebreid scala aan functies te bieden heeft, waaronder:

- Detectie van virtuele en fysieke servers.
- Bepalen van juiste grootte op basis van prestaties.
- Kostenplanning.
- Evaluaties op basis van import.
- Afhankelijkheidsanalyse van toepassingen zonder agent.

Als u op zoek bent naar deskundige hulp om aan de slag te gaan, kan Microsoft u verwijzen naar bekwaam [Azure Expert Managed Service Providers](https://azure.microsoft.com/partners) om u te helpen. Ga voor meer informatie naar de [Azure Migrate-website](https://azure.microsoft.com/services/azure-migrate/). 

## <a name="azure-migrate-versions"></a>Versies van Azure Migrate

Er zijn twee versies van de Azure Migrate-service.

- **Huidige versie**: Gebruik deze versie om Azure Migrate-projecten te maken, on-premises machines te detecteren, en evaluaties en migraties te organiseren. [Lees meer](whats-new.md) over wat er nieuw is in deze versie.
- **Vorige versie**: De vorige versie van Azure Migrate ondersteunt alleen evaluatie van on-premises VMware-VM's. Als u de vorige versie hebt gebruikt, moet u nu de huidige versie gebruiken. U kunt geen Azure Migrate-projecten meer maken met de vorige versie. En we raden u aan om ook geen nieuwe detecties meer uit te voeren met die versie.

    Als u toegang wilt krijgen tot bestaande projecten in de Azure-portal, zoekt en selecteert u **Azure Migrate**. Het dashboard **Azure Migrate** bevat een melding en een koppeling voor toegang tot oude Azure Migrate-projecten.

## <a name="next-steps"></a>Volgende stappen

- Probeer onze zelfstudies om [VMware-VM's](tutorial-prepare-vmware.md), [Hyper-V-VM's](tutorial-prepare-hyper-v.md)of [fysieke servers](tutorial-prepare-physical.md) te evalueren.
- [Bekijk de veelgestelde vragen](resources-faq.md) over Azure Migrate.
