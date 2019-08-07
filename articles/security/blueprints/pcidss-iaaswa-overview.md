---
title: Azure-blauwdruk voor beveiliging en naleving-IaaS-webtoepassing voor PCI DSS
description: Azure-blauwdruk voor beveiliging en naleving-IaaS-webtoepassing voor PCI DSS
services: security
author: meladie
ms.assetid: 9c825380-2abe-4fdd-800c-59d1fed1780b
ms.service: security
ms.topic: article
ms.date: 07/03/2018
ms.author: meladie
ms.openlocfilehash: e8e9faa16474335463cbdccd9fe7b3d749be0f25
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/05/2019
ms.locfileid: "68781013"
---
# <a name="azure-security-and-compliance-blueprint-iaas-web-application-for-pci-dss"></a>Azure-blauwdruk voor beveiliging en naleving: IaaS-webtoepassing voor PCI DSS

## <a name="overview"></a>Overzicht

Deze Azure-blauwdruk voor beveiliging en naleving biedt richt lijnen voor de implementatie van een IaaS-omgeving (Infrastructure as a Service) die voldoet aan de normen van de betalings kaart (PCI DSS 3,2) die geschikt is voor de verzameling, opslag en het ophalen van gegevens van kaart houder. Er wordt een algemene referentie architectuur gedemonstreerd en de juiste verwerking van creditcard gegevens (inclusief kaart nummer, verval datum en verificatie gegevens) in een beveiligde, compatibele omgeving met meerdere lagen. Deze blauw druk illustreert een end-to-end-oplossing om te voldoen aan de behoeften van organisaties die een Cloud benadering nodig hebben om de belasting en kosten voor de implementatie te verminderen.

Deze referentie architectuur, implementatie handleiding en bedreigings model bieden een basis voor klanten om te voldoen aan de vereisten van PCI DSS 3,2. Deze oplossing biedt een basis lijn om klanten te helpen bij het implementeren van werk belastingen naar Azure op een PCI DSS 3,2-bestendige manier. deze oplossing mag echter niet worden gebruikt als-in een productie omgeving, omdat aanvullende configuratie is vereist.

Voor het bereiken van PCI DSS-naleving is vereist dat een goedgekeurde gekwalificeerde beveiligings-beoordelaar (QSA) een productie klant oplossing certificeren. Klanten zijn verantwoordelijk voor het uitvoeren van passende beveiligings-en nalevings beoordelingen van alle oplossingen die zijn gebouwd met behulp van deze architectuur, aangezien de vereisten kunnen variëren afhankelijk van de specifieke implementaties van elke klant.

## <a name="architecture-diagram-and-components"></a>Architectuur diagram en onderdelen

Met deze oplossing wordt een referentie architectuur voor een IaaS-webtoepassing met een SQL Server back-end geïmplementeerd. De architectuur bevat een weblaag, gegevenslaag, Active Directory-infra structuur, Application Gateway en Load Balancer. Virtuele machines die zijn geïmplementeerd op de web-en gegevenslaagsen, worden geconfigureerd in een beschikbaarheidsset en SQL Server exemplaren worden geconfigureerd in een AlwaysOn-beschikbaarheids groep voor hoge Beschik baarheid. Virtuele machines zijn lid van een domein en Active Directory groeps beleid wordt gebruikt voor het afdwingen van beveiligings-en nalevings configuraties op het niveau van het besturings systeem. Een bastion-host van beheer biedt een beveiligde verbinding voor beheerders om toegang te krijgen tot geïmplementeerde bronnen. **Azure raadt aan om een VPN-of ExpressRoute-verbinding te configureren voor beheer en gegevens import in het subnet met referentie architectuur.**

![IaaS-webtoepassing voor PCI DSS referentie architectuur diagram](images/pcidss-iaaswa-architecture.png "IaaS-webtoepassing voor PCI DSS referentie architectuur diagram")

Deze oplossing maakt gebruik van de volgende Azure-Services. Details van de implementatie architectuur vindt u in de sectie [implementatie architectuur](#deployment-architecture) .

- Beschikbaarheidssets
    - (1) Active Directory domein controllers
    - (1) SQL-cluster knooppunten
    - (1) Web/IIS
- Azure Active Directory
- Azure Application Gateway
    - (1) Web Application firewall
        - Firewall modus: voor komen
        - Regelset: OWASP 3,0
        - Listener-poort: 443
- Azure Key Vault
- Azure Load Balancer
- Azure Monitor
- Azure Resource Manager
- Azure Security Center
- Azure Storage
    - (7) geografisch redundante opslag accounts
- Azure Virtual Machines
    - (1) beheer/Bastion (Windows Server 2016 Data Center)
    - (2) Active Directory domein controller (Windows Server 2016 Data Center)
    - (2) SQL Server cluster knooppunt (SQL Server 2017 op Windows Server 2016)
    - (2) Web/IIS (Windows Server 2016 Data Center)
- Azure Virtual Network
    - (1)/16 netwerk
    - (5)/24 netwerken
    - (5) netwerk beveiligings groepen
- Cloudwitness
- Recovery Services kluis

## <a name="deployment-architecture"></a>Implementatie architectuur

De volgende sectie bevat informatie over de implementatie-en implementatie-elementen.

**Bastion-host**: De bastion-host is het enige toegangs punt dat gebruikers in staat stelt om toegang te krijgen tot de geïmplementeerde resources in deze omgeving. De bastion-host biedt een beveiligde verbinding met geïmplementeerde bronnen door alleen extern verkeer van open bare IP-adressen in een veilige lijst toe te staan. Als u RDP-verkeer (extern bureau blad) wilt toestaan, moet de bron van het verkeer worden gedefinieerd in de netwerk beveiligings groep.

Met deze oplossing maakt u een virtuele machine als een aan een domein gekoppelde bastion-host met de volgende configuraties:
-   [Uitbrei ding voor antimalware](https://docs.microsoft.com/azure/security/fundamentals/antimalware)
-   [Azure Diagnostics extensie](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) Azure Key Vault gebruiken
-   Een [beleid voor automatisch afsluiten](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) om het verbruik van resources van de virtuele machine te verminderen wanneer het niet wordt gebruikt
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) is ingeschakeld zodat referenties en andere geheimen worden uitgevoerd in een beveiligde omgeving die is geïsoleerd van het actieve besturings systeem

### <a name="virtual-network"></a>Virtueel netwerk

De architectuur definieert een particulier virtueel netwerk met een adres ruimte van 10.200.0.0/16.

**Netwerk beveiligings groepen**: Met deze oplossing worden bronnen geïmplementeerd in een architectuur met een afzonderlijk websubnet, een Data Base-subnet, Active Directory subnet en het beheer subnet in een virtueel netwerk. Subnetten worden logisch gescheiden door de regels voor de netwerk beveiligings groep die worden toegepast op de afzonderlijke subnetten om het verkeer tussen subnetten alleen te beperken tot de gegevens die nodig zijn voor systeem-en beheer functionaliteit.

Zie de configuratie voor [netwerk beveiligings groepen](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) die met deze oplossing zijn geïmplementeerd. Organisaties kunnen netwerk beveiligings groepen configureren door het bovenstaande bestand te bewerken met behulp van [deze documentatie](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) als richt lijn.

Elk van de subnetten heeft een toegewezen netwerk beveiligings groep:
- 1 netwerk beveiligings groep voor Application Gateway (LBNSG)
- 1 netwerk beveiligings groep voor Bastion-host (MGTNSG)
- 1 netwerk beveiligings groep voor primaire en back-updomein controllers (ADNSG)
- 1 netwerk beveiligings groep voor SQL-servers en Cloud-Witness (SQLNSG)
- 1 netwerk beveiligings groep voor de weblaag (WEBNSG)

### <a name="data-in-transit"></a>Actieve gegevens

Azure versleutelt standaard alle communicatie naar en van Azure-data centers. Daarnaast worden alle trans acties Azure Storage via de Azure Portal uitgevoerd via HTTPS.

### <a name="data-at-rest"></a>Data-at-rest

De architectuur beveiligt gegevens in rust door meerdere metingen, inclusief versleuteling en database controle.

**Azure Storage**: Om te voldoen aan de versleutelde gegevens op rest-vereisten, gebruikt alle [Azure Storage](https://azure.microsoft.com/services/storage/) [Storage service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Dit helpt de gegevens van de kaart houder te beschermen en te beschermen ter ondersteuning van organisatie beveiligings verplichtingen en nalevings vereisten die zijn gedefinieerd door PCI DSS 3,2.

**Azure Disk Encryption**: Azure Disk Encryption wordt gebruikt voor versleutelde Windows IaaS-schijven van virtuele machines. [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) maakt gebruik van de BitLocker-functie van Windows om volume versleuteling te bieden voor besturings systeem-en gegevens schijven. De oplossing is geïntegreerd met Azure Key Vault om de versleutelings sleutels van de schijf te controleren en te beheren.

**SQL Server**: Het SQL Server exemplaar maakt gebruik van de volgende data base Security-maat eenheden:
- Met [Active Directory verificatie en autorisatie](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) kan identiteits beheer van database gebruikers en andere micro soft-Services op één centrale locatie worden beheerd.
-   Met [SQL database controle](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) worden database gebeurtenissen bijgehouden en naar een audit logboek in een Azure-opslag account geschreven.
-   SQL Database is geconfigureerd voor het gebruik van [transparent Data Encryption](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), waarmee in realtime versleuteling en ontsleuteling van de data base, gekoppelde back-ups en transactie logboek bestanden worden uitgevoerd om informatie te beveiligen. Transparent Data Encryption biedt zekerheid dat opgeslagen kaart gegevens niet zijn onderworpen aan onbevoegde toegang.
-   [Firewall regels](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voor komen dat alle toegang tot database servers tot de juiste machtigingen worden verleend. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt.
-   Met de [detectie van SQL-bedreigingen](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) kunnen mogelijke dreigingen worden gedetecteerd en gereageerd als ze optreden door beveiligings waarschuwingen te bieden voor verdachte database activiteiten, potentiële kwetsbaar heden, SQL-injectie aanvallen en afwijkende database toegangs patronen.
-   [Always encrypted kolommen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) zorgen ervoor dat gevoelige kaart gegevens nooit als tekst zonder opmaak in het database systeem worden weer gegeven. Na het inschakelen van gegevens versleuteling, hebben alleen client toepassingen of toepassings servers met toegang tot de sleutels toegang tot tekst zonder opmaak.
- De functie [uitgebreide eigenschappen](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) kan worden gebruikt om de verwerking van gegevens houders te stoppen, omdat hiermee gebruikers aangepaste eigenschappen kunnen toevoegen aan database objecten en gegevens als ' stopgezet ' kunnen labelen om toepassings logica te ondersteunen om te voor komen dat de verwerking van gekoppelde gegevens van kaart houder.
- Met [beveiliging op rijniveau](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) kunnen gebruikers beleid definiëren om de toegang tot gegevens te beperken om de verwerking te stoppen.
- [SQL database dynamische gegevens maskering](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) beperkt de bloot stelling van de gegevens van de kaart door de gegevens te maskeren voor niet-gemachtigde gebruikers of toepassingen. Met dynamische gegevens maskering kunnen mogelijk gevoelige gegevens automatisch worden gedetecteerd en worden de juiste maskers voorgesteld om te worden toegepast. Dit helpt om de toegang tot gegevens van de kaart houder te identificeren en te beperken, zodat de data base niet wordt afgesloten via onbevoegde toegang. Klanten zijn verantwoordelijk voor het aanpassen van instellingen voor dynamische gegevens maskering om te voldoen aan hun database schema.

### <a name="identity-management"></a>Identiteitsbeheer

De volgende technologieën bieden mogelijkheden voor het beheren van toegang tot gegevens in de Azure-omgeving:

- [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) is een&#39;op de cloud gebaseerde Directory-en identiteits beheer service van micro soft s met meerdere tenants. Alle gebruikers voor deze oplossing worden gemaakt in Azure Active Directory, met inbegrip van gebruikers die toegang hebben tot de Azure SQL Database.
- Verificatie voor de toepassing wordt uitgevoerd met behulp van Azure Active Directory. Zie [toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)voor meer informatie. Daarnaast maakt de database kolom versleuteling gebruik van Azure Active Directory om de toepassing te verifiëren voor Azure SQL Database. Zie voor meer informatie hoe u [gevoelige gegevens in Azure SQL database kunt beveiligen](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
- Met op [rollen gebaseerd toegangs beheer van Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) kunnen beheerders verfijnde toegangs machtigingen definiëren om alleen de hoeveelheid toegang te verlenen die gebruikers nodig hebben om hun taken uit te voeren. In plaats van elke gebruiker onbeperkte machtiging voor Azure-resources geven, kunnen beheerders alleen bepaalde acties toestaan voor toegang tot gegevens. Abonnements toegang is beperkt tot de abonnements beheerder.
- Met [Azure Active Directory privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) kunnen klanten het aantal gebruikers dat toegang heeft tot bepaalde informatie minimaliseren. Beheerders kunnen Azure Active Directory Privileged Identity Management gebruiken om bevoegde identiteiten en hun toegang tot bronnen te detecteren, beperken en controleren. Deze functie kan ook worden gebruikt voor het afdwingen van alleen-in-time-beheer toegang op aanvraag als dat nodig is.
- [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) detecteert mogelijke beveiligings problemen die&#39;van invloed zijn op een organisatie s-identiteiten, configureert automatische antwoorden op&#39;gedetecteerde verdachte acties die betrekking hebben op de identiteit van een organisatie en onderzoekt verdachte incidenten om deze problemen op te lossen.

### <a name="security"></a>Beveiliging

**Beheer van geheimen**: De oplossing maakt gebruik van [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) voor het beheer van sleutels en geheimen. Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt. Met de volgende Azure Key Vault mogelijkheden kunnen klanten dergelijke gegevens beveiligen en benaderen:

- Geavanceerd toegangs beleid wordt geconfigureerd op basis van behoefte.
- Key Vault toegangs beleid wordt gedefinieerd met mini maal vereiste machtigingen voor sleutels en geheimen.
- Alle sleutels en geheimen in Key Vault hebben verloop datums.
- Alle sleutels in Key Vault worden beveiligd door gespecialiseerde hardware security modules. Het sleutel type is een met HSM beschermde 2048-bits RSA-sleutel.
- Aan alle gebruikers en identiteiten worden mini maal vereiste machtigingen verleend met toegangs beheer op basis van rollen.
- Diagnostische logboeken voor Key Vault zijn ingeschakeld met een Bewaar periode van ten minste 365 dagen.
- Toegestane cryptografische bewerkingen voor sleutels zijn beperkt tot de vereisten die nodig zijn.
- De oplossing is geïntegreerd met Azure Key Vault voor het beheren van IaaS en geheimen voor de schijf versleutelings sleutels van de virtuele machine.

**Patch beheer**: Virtuele Windows-machines die zijn geïmplementeerd als onderdeel van deze referentie architectuur, worden standaard geconfigureerd voor het ontvangen van automatische updates van Windows Update-service. Deze oplossing omvat ook de [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) -service waarmee bijgewerkte implementaties kunnen worden gemaakt om virtuele machines te patchen wanneer dat nodig is.

**Bescherming tegen schadelijke software**: [Micro soft antimalware](https://docs.microsoft.com/azure/security/fundamentals/antimalware) voor virtual machines biedt realtime beschermings mogelijkheden waarmee u virussen, spyware en andere schadelijke software kunt identificeren en verwijderen, met Configureer bare waarschuwingen wanneer bekende of ongewenste software probeert om op beveiligde virtuele machines installeren of uitvoeren.

**Azure Security Center**: Met [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)kunnen klanten centraal beveiligings beleid Toep assen en beheren voor werk belastingen, bloot stelling aan bedreigingen beperken en aanvallen detecteren en erop reageren. Daarnaast Azure Security Center de bestaande configuraties van Azure-Services openen om aanbevelingen voor de configuratie en service te bieden voor het verbeteren van de beveiliging postuur en het beveiligen van gegevens.

Azure Security Center maakt gebruik van diverse detectie mogelijkheden om klanten te waarschuwen over mogelijke aanvallen die zijn gericht op hun omgeving. Deze waarschuwingen bevatten waardevolle informatie over de trigger van de waarschuwing, de betrokken resources en de bron van de aanval. Azure Security Center heeft een reeks [vooraf gedefinieerde beveiligings waarschuwingen](https://docs.microsoft.com/azure/security-center/security-center-alerts-type), die worden geactiveerd wanneer een bedreiging of verdachte activiteit plaatsvindt. Met [aangepaste waarschuwings regels](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) in azure Security Center kunnen klanten nieuwe beveiligings waarschuwingen definiëren op basis van gegevens die al zijn verzameld uit hun omgeving.

Azure Security Center biedt beveiligings waarschuwingen en-incidenten met prioriteit, waardoor klanten eenvoudiger mogelijke beveiligings problemen kunnen detecteren en oplossen. Er wordt een [Threat Intelligence-rapport](https://docs.microsoft.com/azure/security-center/security-center-threat-report) voor elke gedetecteerde bedreiging gegenereerd om de incidenten te helpen bij het onderzoeken en oplossen van bedreigingen.

**Azure-toepassing gateway**: De architectuur vermindert het risico op beveiligings problemen met behulp van een Azure-toepassing gateway waarvoor een Web Application Firewall is geconfigureerd en de ruleset OWASP ingeschakeld. Aanvullende mogelijkheden zijn onder andere:

- [End-to-end-SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [SSL-offload](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal) inschakelen
- [TLS v 1.0 en v 1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell) uitschakelen
- [Web Application firewall](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (preventie modus)
- [Preventie modus](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) met OWASP 3,0 ruleSet
- [Logboek registratie van diagnostische gegevens](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics) inschakelen
- [Aangepaste status tests](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Security Center](https://azure.microsoft.com/services/security-center) en [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) bieden extra beveiliging en meldingen. Azure Security Center biedt ook een reputatie systeem.

### <a name="business-continuity"></a>Bedrijfscontinuïteit

**Hoge beschikbaarheid**: De oplossing implementeert alle virtuele machines in een [beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Beschikbaarheids sets zorgen ervoor dat de virtuele machines over meerdere geïsoleerde hardwareconfiguraties worden gedistribueerd om de beschik baarheid te verbeteren. Er is ten minste één virtuele machine beschikbaar tijdens een geplande of niet-geplande onderhouds gebeurtenis die voldoet aan de 99,95% Azure SLA.

**Recovery Services kluis**: De [Recovery Services kluis](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) maakt back-upgegevens en beveiligt alle configuraties van Azure virtual machines in deze architectuur. Met een Recovery Services kluis kunnen klanten bestanden en mappen herstellen vanaf een virtuele IaaS-machine zonder de volledige virtuele machine te herstellen, waardoor de herstel tijd sneller verloopt.

**Cloudwitness**: [Cloudwitness](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) is een type quorum-witness voor failoverclusters in Windows Server 2016 die Azure als arbitrage punt gebruikt. De Cloudwitness, zoals elke andere quorumwitness, haalt een stem op en kan deel nemen aan de quorum berekeningen, maar maakt gebruik van de standaard openbaar beschik bare Azure-Blob Storage. Dit elimineert de extra onderhouds overhead van virtuele machines die worden gehost in een open bare Cloud.

### <a name="logging-and-auditing"></a>Logboek registratie en controle

Azure-Services registreren systeem-en gebruikers activiteiten uitvoerig, evenals systeem status:
- **Activiteiten logboeken**: [Activiteiten logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bieden inzicht in bewerkingen die worden uitgevoerd op resources in een abonnement. Activiteiten logboeken kunnen helpen bij het bepalen van de initiator, het tijdstip van de gebeurtenis en de status van een bewerking.
- **Diagnostische logboeken**: [Diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) bevatten alle logboeken die elke resource heeft verzonden. Deze logboeken bevatten Windows-gebeurtenis systeem logboeken, Azure Storage logboeken, Key Vault controle logboeken en Application Gateway toegang en firewall Logboeken. Alle Diagnostische logboeken schrijven naar een gecentraliseerd en versleuteld Azure Storage-account voor archivering. De retentie kan door de gebruiker worden geconfigureerd, tot 730 dagen, om te voldoen aan de specifieke vereisten voor het bewaren van een organisatie.

**Azure monitor logboeken**: Deze logboeken worden samengevoegd in [Azure monitor logboeken](https://azure.microsoft.com/services/log-analytics/) voor verwerking, opslag en dashboard rapportage. Zodra de gegevens zijn verzameld, worden deze in verschillende tabellen ingedeeld voor elk gegevens type binnen Log Analytics werk ruimten, waardoor alle gegevens kunnen worden geanalyseerd, ongeacht de oorspronkelijke bron. Bovendien kan Azure Security Center worden geïntegreerd met Azure Monitor-logboeken, zodat klanten Kusto-query's kunnen gebruiken om toegang te krijgen tot de gegevens van de beveiligings gebeurtenis en deze te combi neren met gegevens uit andere services.

De volgende Azure- [bewakings oplossingen](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) zijn opgenomen als onderdeel van deze architectuur:
-   [Active Directory-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): De Active Directory Health Check-oplossing evalueert het risico en de status van de server omgevingen volgens een regel matig interval en biedt een lijst met aanbevelingen die specifiek zijn voor de geïmplementeerde server infrastructuur.
- [SQL-evaluatie](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): De SQL Health Check-oplossing evalueert het risico en de status van de server omgevingen met een regel matig interval en biedt klanten een lijst met prioriteiten die specifiek zijn voor de geïmplementeerde server infrastructuur.
- [Status van agent](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): De Status van agent oplossing meldt hoeveel agents zijn geïmplementeerd en wat hun geografische distributie zijn, en hoeveel agents niet reageren en het aantal agents dat operationele gegevens verzendt.
-   [Analyse van activiteitenlogboek](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): De Analyse van activiteitenlogboek oplossing helpt bij het analyseren van de activiteiten logboeken van Azure in alle Azure-abonnementen voor een klant.

**Azure Automation**: [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) winkels, uitvoeren en beheren van runbooks. In deze oplossing helpen runbooks bij het verzamelen van logboeken van Azure SQL Database. Met de oplossing Automation [Wijzigingen bijhouden](https://docs.microsoft.com/azure/automation/automation-change-tracking) kunnen klanten eenvoudig wijzigingen in de omgeving identificeren.

**Azure monitor**: [Azure monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) helpt gebruikers bij het volgen van prestaties, het onderhouden van de beveiliging en het identificeren van trends door organisaties in te scha kelen, waarschuwingen te maken en gegevens te archiveren, met inbegrip van tracking-API-aanroepen in hun Azure-resources.

## <a name="threat-model"></a>Bedreigings model

Het gegevensstroom diagram (GSD) voor deze referentie architectuur is beschikbaar voor [downloaden](https://aka.ms/pcidss-iaaswa-tm) of kan hieronder worden weer gegeven. Dit model kan klanten helpen om inzicht te krijgen in de punten van potentieel risico in de systeem infrastructuur bij het aanbrengen van wijzigingen.

![IaaS-webtoepassing voor PCI DSS referentie architectuur diagram](images/pcidss-iaaswa-threat-model.png "IaaS-webtoepassing voor PCI DSS referentie architectuur diagram")

## <a name="compliance-documentation"></a>Documentatie voor naleving

De [gebruikers verantwoordelijkheid matrix voor het Azure-blauwdruk voor beveiliging en naleving-PCI DSS](https://aka.ms/pcidss-crm) bevat verantwoordelijkheden voor alle PCI DSS 3,2-vereisten.

De [Azure-blauwdruk voor beveiliging en naleving-PCI DSS IaaS Web Application implementation-matrix](https://aka.ms/pcidss-iaaswa-cim) bevat informatie over welke PCI DSS 3,2-vereisten worden verholpen door de architectuur van de IaaS-webtoepassing, met inbegrip van gedetailleerde beschrijvingen van de manier waarop de implementatie voldoet aan de vereisten van elk gedekt artikel.

## <a name="guidance-and-recommendations"></a>Richt lijnen en aanbevelingen

### <a name="vpn-and-expressroute"></a>VPN-en ExpressRoute

Een beveiligde VPN-tunnel of [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) moet worden geconfigureerd om een veilige verbinding tot stand te brengen met de resources die zijn geïmplementeerd als onderdeel van deze IaaS-referentie architectuur voor webtoepassingen. Door een VPN-of ExpressRoute in te stellen, kunnen klanten een beveiligingslaag toevoegen voor gegevens die onderweg zijn.

Door een beveiligde VPN-tunnel te implementeren met Azure, kan een virtuele particuliere verbinding tussen een on-premises netwerk en een Azure-Virtual Network worden gemaakt. Deze verbinding vindt plaats via internet en stelt klanten in staat om ' tunnel-informatie veilig ' te maken binnen een versleutelde koppeling tussen het netwerk van de klant en Azure. Site-naar-site-VPN is een veilige, rijpere technologie die is geïmplementeerd door ondernemingen van elke omvang voor tien tallen. De [IPSec-tunnel modus](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) wordt in deze optie als een versleutelings mechanisme gebruikt.

Omdat verkeer binnen de VPN-tunnel via internet met een site-naar-site-VPN gaat, biedt micro soft een andere, nog veiliger verbindings optie. Azure ExpressRoute is een specifieke WAN-verbinding tussen Azure en een on-premises locatie of een Exchange-hosting provider. Omdat ExpressRoute-verbindingen niet via internet werken, bieden deze verbindingen meer betrouw baarheid, hogere snelheden, lagere latenties en een betere beveiliging dan typische verbindingen via internet. Omdat dit een directe verbinding is tussen de telecom municatie-provider van de klant, worden de gegevens niet via internet verzonden en worden ze daarom niet blootgesteld aan IT.

Aanbevolen procedures voor het implementeren van een veilig hybride netwerk dat een on-premises netwerk uitbreidt naar Azure, zijn [beschikbaar](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Vrijwaring

- Dit document is alleen ter informatie bedoeld. MICRO SOFT BIEDT GEEN ENKELE GARANTIE, UITDRUKKELIJK, IMPLICIET OF WETTELIJK, MET BETREKKING TOT DE INFORMATIE IN DIT DOCUMENT. Dit document wordt in de as-is opgenomen. Informatie en weer gaven in dit document, inclusief URL'S en andere website verwijzingen, kunnen zonder kennisgeving worden gewijzigd. Klanten die dit document lezen, hebben het risico van het gebruik ervan.
- Dit document biedt klanten geen juridische rechten voor intellectueel eigendom in een micro soft-product of-oplossingen.
- Klanten kunnen dit document kopiëren en gebruiken voor interne referentie doeleinden.
- Bepaalde aanbevelingen in dit document kunnen leiden tot meer gegevens-, netwerk-of COMPUTE-resource gebruik in azure, en kunnen de kosten van de Azure-licentie of het abonnement van de klant verhogen.
- Deze architectuur is bedoeld om te fungeren als basis voor klanten om aan hun specifieke vereisten aan te passen en mag niet worden gebruikt in een productie omgeving.
- Dit document is ontwikkeld als referentie en mag niet worden gebruikt voor het definiëren van de manier waarop een klant kan voldoen aan specifieke nalevings vereisten en-voor Schriften. Klanten moeten juridische ondersteuning van hun organisatie zoeken op goedgekeurde klant implementaties.
