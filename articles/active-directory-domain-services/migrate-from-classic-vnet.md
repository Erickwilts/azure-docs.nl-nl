---
title: Azure AD Domain Services migreren vanuit een klassiek virtueel netwerk | Microsoft Docs
description: Meer informatie over het migreren van een bestaande Azure AD Domain Services beheerde domein instantie vanuit het klassieke virtuele netwerk model naar een op Resource Manager gebaseerd virtueel netwerk.
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 01/22/2020
ms.author: iainfou
ms.openlocfilehash: e7caacf23cb489da6f9f85748ae839bc4babff8e
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2020
ms.locfileid: "77917302"
---
# <a name="migrate-azure-ad-domain-services-from-the-classic-virtual-network-model-to-resource-manager"></a>Azure AD Domain Services migreren van het klassieke virtuele netwerk model naar Resource Manager

Azure Active Directory Domain Services (AD DS) ondersteunt eenmalige overstap voor klanten die momenteel gebruikmaken van het klassieke virtuele netwerk model naar het Resource Manager-model van het virtuele netwerk. Azure AD DS beheerde domeinen die gebruikmaken van het Resource Manager-implementatie model bieden extra functies, zoals een verfijnd wachtwoord beleid, audit logboeken en beveiliging tegen account vergrendeling.

In dit artikel vindt u een overzicht van de voor delen en overwegingen voor migratie en vervolgens de vereiste stappen om een bestaand exemplaar van Azure AD DS te migreren.

> [!NOTE]
> In 2017 is Azure AD Domain Services beschikbaar voor de host in een Azure Resource Manager netwerk. Sindsdien hebben we een veiligere service met de moderne mogelijkheden van Azure Resource Manager kunnen bouwen. Omdat Azure Resource Manager implementaties volledig worden vervangen door klassieke implementaties, worden Azure AD DS-implementaties met een klassiek virtueel netwerk ingetrokken op 1 maart 2023.
>
> Zie voor meer informatie de [officiële afschaffing melding](https://azure.microsoft.com/updates/we-are-retiring-azure-ad-domain-services-classic-vnet-support-on-march-1-2023/)

## <a name="overview-of-the-migration-process"></a>Overzicht van het migratieproces

Bij het migratie proces wordt een bestaand exemplaar van Azure AD DS uitgevoerd dat in een klassiek virtueel netwerk draait en het verplaatst naar een bestaand virtueel netwerk van Resource Manager. De migratie wordt uitgevoerd met behulp van Power shell en heeft twee belang rijke fasen voor het uitvoeren van de voor *bereiding* en *migratie*.

![Overzicht van het migratie proces voor Azure AD DS](media/migrate-from-classic-vnet/migration-overview.png)

In de *voorbereidings* fase maakt Azure AD DS een back-up van het domein om de laatste moment opname van gebruikers, groepen en wacht woorden die zijn gesynchroniseerd met het beheerde domein, op te halen. De synchronisatie wordt vervolgens uitgeschakeld en de Cloud service die als host fungeert voor het door Azure AD DS beheerde domein, wordt verwijderd. Tijdens de voorbereidings fase kan het door Azure AD DS beheerde domein geen gebruikers verifiëren.

![Voorbereidings fase voor het migreren van Azure AD DS](media/migrate-from-classic-vnet/migration-preparation.png)

In de *migratie* fase worden de onderliggende virtuele schijven voor de domein controllers uit het klassieke Azure AD DS Managed Domain gekopieerd om de vm's te maken met behulp van het Resource Manager-implementatie model. Het door Azure AD DS beheerde domein wordt vervolgens opnieuw gemaakt, inclusief de LDAPS-en DNS-configuratie. Synchronisatie met Azure AD wordt opnieuw gestart en LDAP-certificaten worden hersteld. U hoeft geen computers opnieuw samen te voegen met een door Azure AD DS beheerd domein. ze blijven lid worden van het beheerde domein en zonder wijzigingen worden uitgevoerd.

![Migratie van Azure AD DS](media/migrate-from-classic-vnet/migration-process.png)

## <a name="migration-benefits"></a>Migratie voordelen

Wanneer u een door Azure AD DS beheerd domein verplaatst met dit migratie proces, voor komt u dat de computers opnieuw aan het beheerde domein worden toegevoegd of dat u het Azure AD DS-exemplaar verwijdert en een volledig nieuwe maakt. Vm's blijven aan het eind van het migratie proces lid worden van de Azure AD DS beheerde domein.

Na de migratie biedt Azure AD DS veel functies die alleen beschikbaar zijn voor domeinen die gebruikmaken van virtuele netwerken van Resource Manager, zoals:

* Ondersteuning van nauw keurig wachtwoord beleid.
* AD-account vergrendelings beveiliging.
* E-mail meldingen van waarschuwingen in het door Azure AD DS beheerde domein.
* Audit logboeken met behulp van Azure Monitor.
* Integratie van Azure Files
* HD Insights-integratie

Met Azure AD DS beheerde domeinen die gebruikmaken van een virtueel netwerk van Resource Manager kunt u up-to-date blijven met de nieuwste nieuwe functies. Ondersteuning voor Azure-AD DS met behulp van klassieke virtuele netwerken moet in de toekomst worden afgeschaft.

## <a name="example-scenarios-for-migration"></a>Voorbeeld scenario's voor migratie

Enkele veelvoorkomende scenario's voor het migreren van een door Azure AD DS beheerd domein zijn onder andere de volgende voor beelden.

> [!NOTE]
> Converteer het klassieke virtuele netwerk pas nadat u een geslaagde migratie hebt bevestigd. Als u het virtuele netwerk converteert, wordt de optie voor het terugdraaien of herstellen van het beheerde domein van Azure AD DS verwijderd als er problemen zijn tijdens de migratie-en verificatie fasen.

### <a name="migrate-azure-ad-ds-to-an-existing-resource-manager-virtual-network-recommended"></a>Azure AD DS migreren naar een bestaand virtueel netwerk van Resource Manager (aanbevolen)

Een veelvoorkomend scenario is waar u al andere bestaande klassieke resources hebt verplaatst naar een Resource Manager-implementatie model en een virtueel netwerk. Peering wordt vervolgens gebruikt vanuit het virtuele netwerk van Resource Manager naar het klassieke virtuele netwerk dat Azure AD DS blijft uitvoeren. Op deze manier kunnen Resource Manager-toepassingen en-services gebruikmaken van de verificatie-en beheer functionaliteit van het door Azure AD DS beheerde domein in het klassieke virtuele netwerk. Eenmaal gemigreerd, worden alle resources uitgevoerd met het Resource Manager-implementatie model en het virtuele netwerk.

![Azure AD DS migreren naar een bestaand virtueel netwerk van Resource Manager](media/migrate-from-classic-vnet/migrate-to-existing-vnet.png)

Stappen op hoog niveau voor dit voorbeeld migratie scenario bevatten de volgende onderdelen:

1. Verwijder bestaande VPN-gateways of peering van het virtuele netwerk die is geconfigureerd op het klassieke virtuele netwerk.
1. Migreer de Azure AD DS beheerde domein met behulp van de stappen die in dit artikel worden beschreven.
1. Test en bevestig een geslaagde migratie en verwijder vervolgens het klassieke virtuele netwerk.

### <a name="migrate-multiple-resources-including-azure-ad-ds"></a>Meerdere resources migreren, inclusief Azure AD DS

In dit voorbeeld scenario migreert u Azure AD DS en andere gekoppelde resources van het klassieke implementatie model naar het Resource Manager-implementatie model. Als sommige resources in het klassieke virtuele netwerk nog steeds worden uitgevoerd naast het beheerde domein van Azure AD DS, kunnen ze al profiteren van de migratie naar het Resource Manager-implementatie model.

![Meerdere resources migreren naar het Resource Manager-implementatie model](media/migrate-from-classic-vnet/migrate-multiple-resources.png)

Stappen op hoog niveau voor dit voorbeeld migratie scenario bevatten de volgende onderdelen:

1. Verwijder bestaande VPN-gateways of peering van het virtuele netwerk die is geconfigureerd op het klassieke virtuele netwerk.
1. Migreer de Azure AD DS beheerde domein met behulp van de stappen die in dit artikel worden beschreven.
1. Stel de peering van het virtuele netwerk in tussen het klassieke virtuele netwerk en het Resource Manager-netwerk.
1. Test en bevestig een geslaagde migratie.
1. [Verplaats extra klassieke resources, zoals vm's][migrate-iaas].

### <a name="migrate-azure-ad-ds-but-keep-other-resources-on-the-classic-virtual-network"></a>Azure AD DS migreren, maar andere resources in het klassieke virtuele netwerk blijven

In dit voorbeeld scenario beschikt u over de minimale hoeveelheid downtime in één sessie. U migreert Azure AD DS alleen naar een virtueel netwerk van Resource Manager en bewaart bestaande resources in het klassieke implementatie model en het virtuele netwerk. In een volgende onderhouds periode kunt u de extra resources van het klassieke implementatie model en het virtuele netwerk naar wens migreren.

![Migreer alleen Azure-AD DS naar het Resource Manager-implementatie model](media/migrate-from-classic-vnet/migrate-only-azure-ad-ds.png)

Stappen op hoog niveau voor dit voorbeeld migratie scenario bevatten de volgende onderdelen:

1. Verwijder bestaande VPN-gateways of peering van het virtuele netwerk die is geconfigureerd op het klassieke virtuele netwerk.
1. Migreer de Azure AD DS beheerde domein met behulp van de stappen die in dit artikel worden beschreven.
1. Stel de peering van het virtuele netwerk in tussen het klassieke virtuele netwerk en het nieuwe virtuele netwerk van Resource Manager.
1. Daarna kunt u [de extra resources][migrate-iaas] van het klassieke virtuele netwerk naar wens migreren.

## <a name="before-you-begin"></a>Voordat u begint

Tijdens de voor bereiding en het migreren van een Azure AD DS beheerd domein, zijn er enkele overwegingen rond de beschik baarheid van verificatie-en beheer Services. De Azure AD DS Managed Domain is gedurende een bepaalde periode niet beschikbaar tijdens de migratie. Toepassingen en services die afhankelijk zijn van Azure AD DSe downtime tijdens de migratie.

> [!IMPORTANT]
> Lees dit artikel over de migratie en richt lijnen voordat u het migratie proces start. Het migratie proces is van invloed op de beschik baarheid van de Azure AD DS domein controllers voor Peri Oden. Gebruikers, services en toepassingen kunnen niet bij het beheerde domein verifiëren tijdens het migratie proces.

### <a name="ip-addresses"></a>IP-adressen

De IP-adressen van de domein controller voor een door Azure AD DS beheerde domein wijziging na de migratie. Deze wijziging omvat het open bare IP-adres voor het beveiligde LDAP-eind punt. De nieuwe IP-adressen bevinden zich in het adres bereik voor het nieuwe subnet in het Resource Manager-virtuele netwerk.

In het geval van terugdraaien kunnen de IP-adressen na terugdraaien worden gewijzigd.

Azure AD DS maakt doorgaans gebruik van de eerste twee beschik bare IP-adressen in het adres bereik, maar dit is niet gegarandeerd. U kunt op dit moment geen IP-adressen opgeven die na de migratie moeten worden gebruikt.

### <a name="downtime"></a>Downtime

Bij het migratie proces moeten de domein controllers gedurende een bepaalde tijd offline zijn. Domein controllers zijn niet toegankelijk terwijl Azure AD DS worden gemigreerd naar het Resource Manager-implementatie model en het virtuele netwerk. Gemiddeld is de downtime ongeveer 1 tot 3 uur. Deze periode is van waaruit de domein controllers offline worden gezet naar het moment dat de eerste domein controller weer online is. Dit gemiddelde omvat niet de tijd die nodig is om de tweede domein controller te repliceren, of de tijd die het kan duren om aanvullende resources te migreren naar het Resource Manager-implementatie model.

### <a name="account-lockout"></a>Account vergrendeling

In azure AD DS beheerde domeinen die worden uitgevoerd op klassieke virtuele netwerken, is het AD-account vergrendelings beleid niet aanwezig. Als Vm's worden blootgesteld aan Internet, kunnen aanvallers met behulp van wacht woord-spray methoden gebruiken om accounts te laten afdwingen. Er is geen account vergrendelings beleid om deze pogingen te stoppen. Voor Azure AD DS beheerde domeinen die gebruikmaken van het Resource Manager-implementatie model en virtuele netwerken, wordt het AD-account vergrendelings beleid beschermd tegen deze aanvallen met een wacht woord.

Standaard hebben vijf mislukte wachtwoord pogingen binnen twee minuten een account voor 30 minuten vergrendeld.

Een vergrendeld account kan niet worden gebruikt om aan te melden, wat kan leiden tot de mogelijkheid om het door Azure AD DS beheerde domein of toepassingen die worden beheerd door het account te beheren. Nadat een door Azure AD DS beheerd domein is gemigreerd, kunnen accounts zich voordoen als een permanente vergren deling als gevolg van herhaalde mislukte pogingen om zich aan te melden. Twee veelvoorkomende scenario's na migratie zijn onder andere:

* Een service account dat een verlopen wacht woord gebruikt.
    * Het service account probeert zich herhaaldelijk aan te melden met een verlopen wacht woord, waardoor het account wordt vergrendeld. U kunt dit probleem verhelpen door de toepassing of virtuele machine te zoeken met verlopen referenties en het wacht woord bij te werken.
* Een kwaadwillende entiteit maakt gebruik van felle pogingen om zich aan te melden bij accounts.
    * Wanneer Vm's worden blootgesteld aan Internet, kunnen aanvallers vaak gemeen schappelijke combi Naties van gebruikers namen en wacht woorden proberen als ze zich proberen aan te melden. Deze herhaalde mislukte aanmeldings pogingen kunnen de accounts vergren delen. Het is niet raadzaam om Administrator-accounts te gebruiken met algemene namen, zoals *admin* of *Administrator*, om te voor komen dat beheerders accounts worden vergrendeld.
    * Minimaliseer het aantal Vm's dat wordt blootgesteld aan Internet. U kunt [Azure Bastion][azure-bastion] gebruiken om veilig verbinding te maken met virtuele machines met behulp van de Azure Portal.

Als u vermoedt dat sommige accounts na de migratie kunnen worden vergrendeld, wordt in de laatste migratie stappen beschreven hoe u controle inschakelt of hoe u de instellingen voor een verfijnend wachtwoord beleid kunt wijzigen.

### <a name="roll-back-and-restore"></a>Terugdraaien en herstellen

Als de migratie is mislukt, wordt het proces voor het terugdraaien of herstellen van een beheerd domein van Azure AD DS. Rollback is een selfservice optie om de status van het beheerde domein onmiddellijk te retour neren vóór de migratie poging. Ondersteunings technici van Azure kunnen ook een beheerd domein herstellen vanaf een back-up als een laatste redmiddel. Zie [het terugdraaien of herstellen van een mislukte migratie](#roll-back-and-restore-from-migration)voor meer informatie.

### <a name="restrictions-on-available-virtual-networks"></a>Beperkingen voor beschik bare virtuele netwerken

Er zijn enkele beperkingen voor de virtuele netwerken waarmee een beheerd domein van Azure AD DS kan worden gemigreerd. Het virtuele netwerk van de doel Resource Manager moet voldoen aan de volgende vereisten:

* Het virtuele netwerk van Resource Manager moet zich in hetzelfde Azure-abonnement bevindt als het klassieke virtuele netwerk waarop Azure AD DS momenteel is geïmplementeerd.
* Het virtuele netwerk van Resource Manager moet zich in dezelfde regio bevinden als het klassieke virtuele netwerk waarin Azure AD DS momenteel is geïmplementeerd.
* Het subnet van het virtuele netwerk van Resource Manager moet ten minste 3-5 beschik bare IP-adressen hebben.
* Het subnet van het virtuele netwerk van Resource Manager moet een toegewezen subnet zijn voor Azure AD DS en mag geen andere workloads hosten.

Zie overwegingen voor het [ontwerpen van virtuele netwerken en configuratie opties][network-considerations]voor meer informatie over de vereisten voor virtuele netwerken.

## <a name="migration-steps"></a>Migratiestappen

De migratie naar het Resource Manager-implementatie model en het virtuele netwerk is onderverdeeld in vijf hoofd stappen:

| Stap    | Uitgevoerd via  | Geschatte tijd  | Downtime  | Terugdraaien/herstellen? |
|---------|--------------------|-----------------|-----------|-------------------|
| [Stap 1: het nieuwe virtuele netwerk bijwerken en zoeken](#update-and-verify-virtual-network-settings) | Azure-portal | 15 minuten | Geen downtime vereist | N.v.t. |
| [Stap 2: het beheerde domein van Azure AD DS voorbereiden voor migratie](#prepare-the-managed-domain-for-migration) | PowerShell | 15 – 30 minuten op gemiddeld | De downtime van Azure AD DS begint nadat deze opdracht is voltooid. | Terugdraaien en herstellen beschikbaar. |
| [Stap 3: het beheerde domein van Azure AD DS verplaatsen naar een bestaand virtueel netwerk](#migrate-the-managed-domain) | PowerShell | 1 – 3 uur gemiddeld | Er is één domein controller beschikbaar zodra deze opdracht is voltooid, de downtime wordt beëindigd. | Bij fouten zijn zowel terugdraaien (Self Service) als herstel beschikbaar. |
| [Stap 4: testen en wachten op de replica domein controller](#test-and-verify-connectivity-after-the-migration)| Power shell en Azure Portal | 1 uur of langer, afhankelijk van het aantal tests | Beide domein controllers zijn beschikbaar en moeten normaal functioneren. | N.v.t. Zodra de eerste virtuele machine is gemigreerd, is er geen optie voor terugdraaien of herstellen. |
| [Stap 5-optionele configuratie stappen](#optional-post-migration-configuration-steps) | Azure Portal en Vm's | N.v.t. | Geen downtime vereist | N.v.t. |

> [!IMPORTANT]
> Lees alle informatie over dit migratie artikel en richt lijnen voordat u het migratie proces start om extra uitval tijd te voor komen. Het migratie proces is van invloed op de beschik baarheid van de Azure AD DS domein controllers gedurende een bepaalde periode. Gebruikers, services en toepassingen kunnen niet bij het beheerde domein verifiëren tijdens het migratie proces.

## <a name="update-and-verify-virtual-network-settings"></a>De instellingen van het virtuele netwerk bijwerken en controleren

Voordat u met het migratie proces begint, moet u de volgende eerste controles en updates uitvoeren. Deze stappen kunnen worden uitgevoerd op elk moment vóór de migratie en zijn niet van invloed op de werking van het door Azure AD DS beheerde domein.

1. Werk uw lokale Azure PowerShell-omgeving bij naar de nieuwste versie. Voor het volt ooien van de migratie stappen moet u ten minste versie *2.3.2*hebben.

    Zie [Azure PowerShell Overview][azure-powershell]voor meer informatie over het controleren en bijwerken van uw Power shell-versie.

1. Maak of kies een bestaand virtueel netwerk van Resource Manager.

    Zorg ervoor dat de netwerk instellingen de vereiste poorten die vereist zijn voor Azure AD DS niet blokkeert. Poorten moeten zijn geopend op het klassieke virtuele netwerk en het virtuele netwerk van Resource Manager. Deze instellingen zijn onder andere route tabellen (hoewel het niet aanbevolen is om route tabellen te gebruiken) en netwerk beveiligings groepen.

    Zie [netwerk beveiligings groepen en vereiste poorten][network-ports]om de vereiste poorten weer te geven. Om netwerk communicatie problemen te minimaliseren, wordt het aanbevolen om een netwerk beveiligings groep of route tabel toe te passen op het Resource Manager-virtuele netwerk nadat de migratie is voltooid.

    Noteer deze doel resource groep, het virtuele netwerk van het doel en het subnet van het virtuele netwerk. Deze resource namen worden gebruikt tijdens het migratie proces.

1. Controleer de status van de Azure AD DS Managed Domain in de Azure Portal. Als u waarschuwingen voor het beheerde domein hebt, moet u deze oplossen voordat u het migratie proces start.
1. Als u van plan bent om andere resources te verplaatsen naar het Resource Manager-implementatie model en het virtuele netwerk, moet u er ook voor zorgen dat deze bronnen kunnen worden gemigreerd. Zie door [het platform ondersteunde migratie van IaaS-resources van klassiek naar Resource Manager][migrate-iaas]voor meer informatie.

    > [!NOTE]
    > Converteer het klassieke virtuele netwerk niet naar een virtueel netwerk van Resource Manager. Als u dit doet, is er geen optie om het beheerde domein van Azure AD DS terug te zetten of te herstellen.

## <a name="prepare-the-managed-domain-for-migration"></a>Het beheerde domein voorbereiden voor migratie

Azure PowerShell wordt gebruikt om het door Azure AD DS beheerde domein voor te bereiden voor migratie. Deze stappen omvatten het maken van een back-up, het onderbreken van synchronisatie en het verwijderen van de Cloud service die als host fungeert voor Azure AD DS. Wanneer deze stap is voltooid, wordt Azure AD DS gedurende een bepaalde tijd offline gezet. Als de voorbereidings stap mislukt, kunt u [teruggaan naar de vorige status](#roll-back).

Voer de volgende stappen uit om het beheerde domein voor Azure AD DS voor te bereiden voor migratie:

1. Installeer het `Migrate-Aaads` script vanuit de [PowerShell Gallery][powershell-script]. Dit Power shell-migratie script is digitaal ondertekend door het technische team van Azure AD.

    ```powershell
    Install-Script -Name Migrate-Aadds
    ```

1. Maak een variabele voor de referenties voor het migratie script met behulp van de cmdlet [Get-Credential][get-credential] .

    Voor het gebruikers account dat u opgeeft, moeten *globale beheerders* rechten in uw Azure AD-Tenant zijn vereist om Azure AD DS in te scha kelen en vervolgens de bevoegdheden voor *inzenders* in uw Azure-abonnement om de vereiste Azure AD DS-resources te maken.

    Wanneer u hierom wordt gevraagd, voert u het juiste gebruikers account en wacht woord in:

    ```powershell
    $creds = Get-Credential
    ```

1. Voer nu de `Migrate-Aadds` cmdlet uit met de para meter *-Prepare* . Geef de *-ManagedDomainFqdn* op voor uw eigen Azure AD DS beheerde domein, zoals *aaddscontoso.com*:

    ```powershell
    Migrate-Aadds `
        -Prepare `
        -ManagedDomainFqdn aaddscontoso.com `
        -Credentials $creds
    ```

## <a name="migrate-the-managed-domain"></a>Het beheerde domein migreren

Met het door Azure AD DS beheerde domein dat is voor bereid en waarvan een back-up is gemaakt, kan het domein worden gemigreerd. Met deze stap worden de Azure AD Domain Services-Vm's van de domein controller opnieuw gemaakt met behulp van het Resource Manager-implementatie model. Het kan 1 tot drie uur duren voordat deze stap is voltooid.

Voer de `Migrate-Aadds` cmdlet uit met behulp van de para meter *-commit* . Geef de *-ManagedDomainFqdn* op voor uw eigen Azure AD DS beheerde domein dat in de vorige sectie is voor bereid, zoals *aaddscontoso.com*:

Geef de doel resource groep op die het virtuele netwerk bevat waarnaar u Azure-AD DS wilt migreren, zoals *myResourceGroup*. Geef het virtuele netwerk van het doel op, bijvoorbeeld *myVnet*, en het subnet, zoals *DomainServices*.

Wanneer deze opdracht wordt uitgevoerd, kunt u de volgende keer niet terugdraaien:

```powershell
Migrate-Aadds `
    -Commit `
    -ManagedDomainFqdn aaddscontoso.com `
    -VirtualNetworkResourceGroupName myResourceGroup `
    -VirtualNetworkName myVnet `
    -VirtualSubnetName DomainServices `
    -Credentials $creds
```

Nadat het script het beheerde domein heeft gevalideerd, wordt de migratie voor bereid. Voer *Y* in om het migratie proces te starten.

> [!IMPORTANT]
> Converteer het klassieke virtuele netwerk niet naar een virtueel netwerk van Resource Manager tijdens het migratie proces. Als u het virtuele netwerk converteert, kunt u de door Azure AD DS beheerde domein niet terugdraaien of herstellen, omdat het oorspronkelijke virtuele netwerk niet meer bestaat.

Elke twee minuten tijdens het migratie proces, een voortgangs indicator rapporteert de huidige status, zoals wordt weer gegeven in de volgende voorbeeld uitvoer:

![Voortgangs indicator van de migratie van Azure AD DS](media/migrate-from-classic-vnet/powershell-migration-status.png)

Het migratie proces blijft actief, zelfs als u het Power shell-script sluit. In de Azure Portal wordt de status van het beheerde domein gerapporteerd als *migratie*.

Wanneer de migratie is voltooid, kunt u het IP-adres van uw eerste domein controller weer geven in de Azure Portal of via Azure PowerShell. Er wordt ook een tijd schatting weer gegeven op de tweede domein controller die beschikbaar is.

In deze fase kunt u eventueel andere bestaande resources verplaatsen vanuit het klassieke implementatie model en het virtuele netwerk. U kunt ook de resources op het klassieke implementatie model blijven gebruiken en de virtuele netwerken aan elkaar koppelen nadat de migratie van Azure AD DS is voltooid.

## <a name="test-and-verify-connectivity-after-the-migration"></a>De connectiviteit na de migratie testen en controleren

Het kan enige tijd duren voordat de tweede domein controller is geïmplementeerd en beschikbaar is voor gebruik in de Azure AD DS beheerde domein.

Met het Resource Manager-implementatie model worden de netwerk resources voor het beheerde Azure AD DS-domein weer gegeven in de Azure Portal of Azure PowerShell. Zie [netwerk bronnen die worden gebruikt door Azure AD DS][network-resources]voor meer informatie over deze netwerk bronnen en.

Wanneer ten minste één domein controller beschikbaar is, voert u de volgende configuratie stappen uit voor netwerk connectiviteit met Vm's:

* **DNS-server instellingen bijwerken** Werk de DNS-instellingen bij met de IP-adressen van de nieuwe domein controllers, zodat andere resources in het virtuele netwerk van Resource Manager het beheerde domein van Azure AD DS kunnen omzetten en gebruiken. De Azure Portal kan deze instellingen automatisch voor u configureren. Zie [DNS-instellingen bijwerken voor het virtuele Azure-netwerk][update-dns]voor meer informatie over het configureren van het virtuele netwerk van Resource Manager.
* **Opnieuw op domein gebaseerde Vm's opnieuw opstarten** : als de IP-adressen van de DNS-server voor de AD DS domein controllers van Azure worden gewijzigd, start u alle vm's die zijn gekoppeld aan het domein opnieuw op, zodat ze de nieuwe DNS-server instellingen gebruiken. Als toepassingen of Vm's hand matig geconfigureerde DNS-instellingen hebben, moet u ze hand matig bijwerken met de nieuwe IP-adressen van de DNS-server van de domein controllers die worden weer gegeven in de Azure Portal.

Test nu de verbinding met het virtuele netwerk en de naam omzetting. Voer de volgende netwerk communicatie tests uit op een VM die is verbonden met het virtuele netwerk van Resource Manager of hieraan is gekoppeld:

1. Controleer of u het IP-adres van een van de domein controllers kunt pingen, zoals `ping 10.1.0.4`
    * De IP-adressen van de domein controllers worden weer gegeven op de pagina **Eigenschappen** voor de Azure AD DS beheerde domein in de Azure Portal.
1. Naam omzetting van het beheerde domein verifiëren, zoals `nslookup aaddscontoso.com`
    * Geef de DNS-naam voor uw eigen Azure AD DS beheerde domein op om te controleren of de DNS-instellingen juist zijn en worden omgezet.

De tweede domein controller moet beschikbaar zijn 1-2 uur nadat de migratie-cmdlet is voltooid. Als u wilt controleren of de tweede domein controller beschikbaar is, kijkt u op de pagina **Eigenschappen** van het Azure AD DS beheerde domein in de Azure Portal. Als er twee IP-adressen worden weer gegeven, is de tweede domein controller klaar.

## <a name="optional-post-migration-configuration-steps"></a>Optionele configuratie stappen na de migratie

Wanneer het migratie proces is voltooid, kunt u de volgende optionele configuratie stappen gebruiken om controle Logboeken of e-mail meldingen in te scha kelen of het verfijnde wachtwoord beleid bij te werken.

### <a name="subscribe-to-audit-logs-using-azure-monitor"></a>Abonneren op audit logboeken met Azure Monitor

In azure AD DS worden audit logboeken weer gegeven om te helpen bij het oplossen van problemen en gebeurtenissen op de domein controllers te bekijken. Zie [audit logboeken inschakelen en gebruiken][security-audits]voor meer informatie.

U kunt sjablonen gebruiken om belang rijke informatie te bewaken die in de logboeken wordt weer gegeven. De sjabloon audit logboek werkmap kan bijvoorbeeld mogelijke account vergrendelingen bewaken in het door Azure AD DS beheerde domein.

### <a name="configure-azure-ad-domain-services-email-notifications"></a>Azure AD Domain Services e-mail meldingen configureren

Als u een melding wilt ontvangen wanneer er een probleem wordt gedetecteerd in het beheerde domein van Azure AD DS, moet u de instellingen voor e-mail meldingen in de Azure Portal bijwerken. Zie [instellingen voor meldingen configureren][notifications]voor meer informatie.

### <a name="update-fine-grained-password-policy"></a>Nauw keurig wachtwoord beleid bijwerken

Als dat nodig is, kunt u het beleid voor verfijnde wacht woorden bijwerken zodat het minder beperkend is dan de standaard configuratie. U kunt de audit Logboeken gebruiken om te bepalen of een minder beperkende instelling zinvol is, en vervolgens het beleid configureren als dat nodig is. Gebruik de volgende stappen op hoog niveau om de beleids instellingen te controleren en bij te werken voor accounts die na de migratie herhaaldelijk worden vergrendeld:

1. [Configureer wachtwoord beleid][password-policy] voor minder beperkingen op het Azure AD DS beheerde domein en Bekijk de gebeurtenissen in de audit Logboeken.
1. Als voor service accounts verlopen wacht woorden worden gebruikt, zoals aangegeven in de audit logboeken, moet u deze accounts bijwerken met het juiste wacht woord.
1. Als een virtuele machine wordt blootgesteld aan Internet, controleert u op algemene account namen, zoals *Administrator*, *gebruiker*of *gast* , met hoge aanmeldings pogingen. Waar mogelijk kunt u deze Vm's bijwerken met minder algemeen benoemde accounts.
1. Gebruik een netwerk tracering op de virtuele machine om de bron van de aanvallen te zoeken en te voor komen dat deze IP-adressen zich kunnen aanmelden.
1. Wanneer er minimale vergrendelings problemen zijn, werkt u het verfijnde wachtwoord beleid zo strikt mogelijk aan.

### <a name="creating-a-network-security-group"></a>Een netwerk beveiligings groep maken

Azure AD DS heeft een netwerk beveiligings groep nodig om de poorten die nodig zijn voor het beheerde domein te beveiligen en alle andere inkomend verkeer te blok keren. Deze netwerk beveiligings groep fungeert als een extra beveiligingslaag om de toegang tot het beheerde domein te vergren delen en wordt niet automatisch gemaakt. Als u de netwerk beveiligings groep wilt maken en de vereiste poorten wilt openen, raadpleegt u de volgende stappen:

1. Selecteer uw Azure AD DS-resource in het Azure Portal. Op de pagina overzicht wordt een knop weer gegeven voor het maken van een netwerk beveiligings groep als er geen is gekoppeld aan Azure AD Domain Services.
1. Als u beveiligde LDAP gebruikt, voegt u een regel toe aan de netwerk beveiligings groep om binnenkomend verkeer voor *TCP* -poort *636*toe te staan. Zie [Configure secure LDAP][secure-ldap](Engelstalig) voor meer informatie.

## <a name="roll-back-and-restore-from-migration"></a>Terugdraaien en herstellen vanaf migratie

Tot een bepaald punt in het migratie proces kunt u ervoor kiezen om het door Azure AD DS beheerde domein terug te zetten of te herstellen.

### <a name="roll-back"></a>Terugdraaien

Als er een fout optreedt tijdens het uitvoeren van de Power shell-cmdlet om de migratie voor te bereiden in stap 2 of voor de migratie zelf in stap 3, kan het door Azure AD DS beheerde domein worden teruggezet naar de oorspronkelijke configuratie. Voor deze terugdraaien is het oorspronkelijke klassieke virtuele netwerk vereist. Houd er rekening mee dat de IP-adressen na het terugdraaien mogelijk nog steeds worden gewijzigd.

Voer de `Migrate-Aadds`-cmdlet uit met de para meter *-abort* . Geef de *-ManagedDomainFqdn* voor uw eigen Azure AD DS beheerde domein die is voor bereid in een vorige sectie, zoals *aaddscontoso.com*, en de naam van het klassieke virtuele netwerk, zoals *myClassicVnet*:

```powershell
Migrate-Aadds `
    -Abort `
    -ManagedDomainFqdn aaddscontoso.com `
    -ClassicVirtualNetworkName myClassicVnet `
    -Credentials $creds
```

### <a name="restore"></a>Herstellen

Als laatste redmiddel kunnen Azure AD Domain Services worden hersteld vanuit de laatst beschik bare back-up. Er wordt een back-up gemaakt in stap 1 van de migratie om er zeker van te zijn dat de meest recente back-up beschikbaar is. Deze back-up wordt 30 dagen opgeslagen.

Als u het met Azure AD DS beheerde domein wilt herstellen vanuit een back-up, [opent u een ondersteunings ticket met behulp van de Azure Portal][azure-support]. Geef de Directory-ID, de domein naam en de reden voor het herstellen op. Het ondersteunings-en herstel proces kan meerdere dagen duren.

## <a name="troubleshooting"></a>Problemen oplossen

Als u problemen ondervindt na de migratie naar het Resource Manager-implementatie model, raadpleegt u enkele van de volgende veelvoorkomende probleemoplossings gebieden:

* [Problemen met domein deelname oplossen][troubleshoot-domain-join]
* [Problemen met account vergrendeling oplossen][troubleshoot-account-lockout]
* [Problemen met het aanmelden van accounts oplossen][troubleshoot-sign-in]
* [Problemen met de beveiliging van LDAP-connectiviteit oplossen][tshoot-ldaps]

## <a name="next-steps"></a>Volgende stappen

Als uw Azure AD DS beheerd domein is gemigreerd naar het Resource Manager-implementatie model, [maakt u en domein-lid van een Windows-VM][join-windows] en installeert u vervolgens [beheer hulpprogramma's][tutorial-create-management-vm].

<!-- INTERNAL LINKS -->
[azure-bastion]: ../bastion/bastion-overview.md
[network-considerations]: network-considerations.md
[azure-powershell]: /powershell/azure/overview
[network-ports]: network-considerations.md#network-security-groups-and-required-ports
[Connect-AzAccount]: /powershell/module/az.accounts/connect-azaccount
[Set-AzContext]: /powershell/module/az.accounts/set-azcontext
[Get-AzResource]: /powershell/module/az.resources/get-azresource
[Set-AzResource]: /powershell/module/az.resources/set-azresource
[network-resources]: network-considerations.md#network-resources-used-by-azure-ad-ds
[update-dns]: tutorial-create-instance.md#update-dns-settings-for-the-azure-virtual-network
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
[security-audits]: security-audit-events.md
[notifications]: notifications.md
[password-policy]: password-policy.md
[secure-ldap]: tutorial-configure-ldaps.md
[migrate-iaas]: ../virtual-machines/windows/migration-classic-resource-manager-overview.md
[join-windows]: join-windows-vm.md
[tutorial-create-management-vm]: tutorial-create-management-vm.md
[troubleshoot-domain-join]: troubleshoot-domain-join.md
[troubleshoot-account-lockout]: troubleshoot-account-lockout.md
[troubleshoot-sign-in]: troubleshoot-sign-in.md
[tshoot-ldaps]: tshoot-ldaps.md
[get-credential]: /powershell/module/microsoft.powershell.security/get-credential

<!-- EXTERNAL LINKS -->
[powershell-script]: https://www.powershellgallery.com/packages/Migrate-Aadds/
