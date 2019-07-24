---
title: On-premises virtuele VMware-machines migreren naar Azure met agentloze Azure Migrate server migratie | Microsoft Docs
description: Hierin wordt beschreven hoe u on-premises virtuele VMware-machines naar Azure kunt migreren en zonder agents met behulp van Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/08/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 885c877f219f59ab5049cf7b8e01243077d6d3eb
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68348399"
---
# <a name="migrate-vmware-vms-to-azure-agentless"></a>Virtuele VMware-machines migreren naar Azure (zonder agent)

In dit artikel wordt beschreven hoe u on-premises virtuele VMware-machines naar Azure migreert met behulp van migratie zonder agent met het hulp programma voor migratie van Azure Migrate-server.

[Azure migrate](migrate-services-overview.md) biedt een centrale hub voor het bijhouden van detectie, evaluatie en migratie van uw on-premises apps en werk belastingen en AWS/GCP VM-instanties naar Azure. De hub biedt Azure Migrate-hulpprogram ma's voor evaluatie en migratie, evenals onafhankelijke ISV'S-aanbiedingen (Independent Software Vendor) van derden.

Deze zelf studie is de derde in een serie die laat zien hoe u virtuele VMware-machines kunt beoordelen en migreren naar Azure met Azure Migrate server evaluatie en-migratie. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Vm's voorbereiden voor migratie.
> * Voeg het migratie hulpprogramma van Azure Migration server toe.
> * Ontdek Vm's die u wilt migreren.
> * Repliceren van virtuele machines starten.
> * Voer een test migratie uit om te controleren of alles werkt zoals verwacht.
> * Voer een volledige VM-migratie uit.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint.

## <a name="migration-methods"></a>Migratie methoden

U kunt virtuele VMware-machines migreren naar Azure met behulp van het hulp programma voor migratie van Azure Migrate-server. Dit hulp programma biedt een aantal opties voor de migratie van virtuele VMware-machines:

- Migratie met replicatie zonder agent. Migreer Vm's zonder dat u iets hoeft te installeren.
- Migratie met een agent voor replicatie. Installeer een agent op de virtuele machine voor replicatie.

Als u wilt beslissen of u zonder agents of migratie op basis van een agent wilt gebruiken, raadpleegt u de volgende artikelen:

- [Meer informatie over](server-migrate-overview.md) de werking van agentloze migratie en [de beperkingen](server-migrate-overview.md#agentless-migration-limitations).
- [Lees dit artikel](tutorial-migrate-vmware-agent.md) als u de op de agent gebaseerde methode wilt gebruiken.

## <a name="prerequisites"></a>Vereisten

Voordat u aan deze zelfstudie begint, dient u eerst:

1. Meer [informatie](migrate-architecture.md) over de architectuur van VMware-migratie.
2. [Voltooi de eerste zelf studie](tutorial-prepare-vmware.md) in deze serie om Azure en VMware in te stellen voor migratie. In deze zelf studie hebt u het volgende nodig:
    - [Azure voorbereiden](tutorial-prepare-vmware.md#prepare-azure) voor migratie.
    - [De on-premises omgeving voorbereiden](tutorial-prepare-vmware.md#prepare-for-agentless-vmware-migration) voor migratie.
    
3. U wordt aangeraden VMware-Vm's te evalueren met Azure Migrate server evaluatie voordat u deze naar Azure migreert. Als u de beoordeling wilt instellen, voltooit u [de tweede zelf studie](tutorial-assess-vmware.md) in deze serie. Als u geen Vm's wilt beoordelen, kunt u deze zelf studie overs Laan. We raden u echter aan om een evaluatie uit te voeren, maar u hoeft geen evaluatie te uitvoeren voordat u een migratie uitvoert.



## <a name="add-the-azure-migrate-server-migration-tool"></a>Het hulp programma voor migratie van Azure Migrate server toevoegen

Als u de tweede zelf studie niet hebt gevolgd om VMware-Vm's te beoordelen, moet u [deze instructies volgen](how-to-add-tool-first-time.md) om een Azure migrate project in te stellen en het Azure migrate server-migratie programma selecteren. 

Als u de tweede zelf studie hebt gevolgd en al een Azure Migrate project hebt ingesteld, voegt u het hulp programma voor migratie van Azure Migrate server als volgt toe:

1. Klik in het Azure Migrate project op **overzicht**. 
2. Klik in **servers detecteren, evalueren en migratie**op **servers beoordelen en migreren**.

     ![Servers beoordelen en migreren](./media/tutorial-migrate-vmware/assess-migrate.png)

3. In **migratie hulpprogramma's**selecteert u **hier om een migratie hulpmiddel toe te voegen wanneer u klaar bent om te migreren**.

    ![Hulpprogramma selecteren](./media/tutorial-migrate-vmware/select-migration-tool.png)

4. Selecteer **in de lijst met hulpprogram ma's Azure migrate: Hulp programma**server migratie > **toevoegen**

    ![Hulp programma voor server migratie](./media/tutorial-migrate-vmware/server-migration-tool.png)

## <a name="set-up-the-azure-migrate-appliance"></a>Het Azure Migrate apparaat instellen

Azure Migrate server-migratie voert een Lightweight VMware VM-apparaat uit. Het apparaat voert VM-detectie uit en verzendt meta gegevens en prestatie gegevens van de virtuele machine naar Azure Migrate server migratie. Hetzelfde apparaat wordt ook gebruikt door het hulp programma voor het evalueren van Azure Migrate-servers.

Als u de tweede zelf studie hebt gevolgd om VMware-Vm's te beoordelen, hebt u het apparaat al ingesteld tijdens deze zelf studie. Als u deze zelf studie niet hebt gevolgd, moet u het apparaat nu instellen. Hiervoor gaat u als volgt te werk: 

- Down load een bestand met een eicellen-sjabloon en importeer het naar vCenter Server.
- Maak het apparaat en controleer of het verbinding kan maken met Azure Migrate server beoordeling. 
- Configureer het apparaat voor de eerste keer en registreer het bij het Azure Migrate-project.

Volg de instructies in [dit artikel](how-to-set-up-appliance-vmware.md) om het apparaat in te stellen.


## <a name="prepare-vms-for-migration"></a>Vm's voorbereiden voor migratie

Azure Migrate vereist enkele VM-wijzigingen om ervoor te zorgen dat Vm's kunnen worden gemigreerd naar Azure.

- Voor sommige [besturings systemen](server-migrate-overview.md#agentless-migration-limitations)worden deze wijzigingen automatisch door Azure migrate aangebracht.
- Als u een virtuele machine met een van deze besturings systemen wilt migreren, volgt u de instructies om de virtuele machine voor te bereiden.
- Het is belang rijk dat u deze wijzigingen aanbrengt voordat u begint met de migratie. Als u de virtuele machine migreert voordat u de wijziging aanbrengt, wordt de VM mogelijk niet opgestart in Azure.
- Wijzigingen in de configuratie van on-premises Vm's worden gerepliceerd naar Azure nadat de replicatie voor de virtuele machine is ingeschakeld. Om ervoor te zorgen dat de wijzigingen worden gerepliceerd, moet u ervoor zorgen dat het herstel punt dat u migreert, later is dan het tijdstip waarop de configuratie wijzigingen on-premises zijn aangebracht.


### <a name="prepare-windows-server-vms"></a>Virtuele machines met Windows Server voorbereiden

**Actie** | **Details** | **Schriften**
--- | --- | ---
Zorg ervoor dat Windows-volumes in de Azure-VM gebruikmaken van dezelfde stationsletter toewijzingen als de on-premises VM. | Configureer het SAN-beleid als online. | 1. Meld u met een beheerders account aan bij de virtuele machine en open een opdracht venster.<br/> 2. Typ **Disk Part** om het hulp programma Disk part uit te voeren.<br/> 3. Type **San-beleid = OnlineAll**<br/> 4. Typ exit om disk Part te verlaten en sluit de opdracht prompt.
Azure Serial Access-console inschakelen voor de Azure-VM | Dit helpt bij het oplossen van problemen. U hoeft de virtuele machine niet opnieuw op te starten. De virtuele machine van Azure wordt opgestart met behulp van de schijf installatie kopie en dit komt overeen met het opnieuw opstarten voor de nieuwe virtuele machine. | Volg [deze instructies](https://docs.microsoft.com/azure/virtual-machines/windows/serial-console) om in te scha kelen.
Hyper-V-gast integratie installeren | Als u computers met Windows Server 2003 wilt migreren, installeert u Hyper-V-gast integratie Services op het VM-besturings systeem. | [Meer informatie](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services#install-or-update-integration-services).
Extern bureaublad | Schakel Extern bureaublad in op de virtuele machine en controleer of de Windows Firewall geen Extern bureaublad toegang blokkeert op netwerk profielen. | [Meer informatie](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access).

### <a name="prepare-linux-vms"></a>Linux Vm's voorbereiden

**Actie** | **Details** 
--- | --- | ---
Hyper-V Linux-integratie Services installeren | De meeste nieuwe versies van Linux-distributies hebben deze standaard inbegrepen.
Bouw de Linux init-installatie kopie opnieuw op om de benodigde Hyper-V-Stuur Programma's te bevatten | Dit zorgt ervoor dat de virtuele machine wordt opgestart in Azure en alleen is vereist voor bepaalde distributies.
Logboek registratie van Azure Serial console inschakelen | Dit helpt bij het oplossen van problemen. U hoeft de virtuele machine niet opnieuw op te starten. De virtuele machine van Azure wordt opgestart met behulp van de schijf installatie kopie en dit komt overeen met het opnieuw opstarten voor de nieuwe virtuele machine.<br/> Volg [deze instructies](https://docs.microsoft.com/azure/virtual-machines/linux/serial-console) om in te scha kelen.
Apparaat toewijzings bestand bijwerken | Werk het apparaat toewijzings bestand met de apparaatnaam bij naar volume koppelingen om permanente apparaat-id's te gebruiken
Fstab-vermeldingen bijwerken | Update vermeldingen om permanente volume-id's te gebruiken.
Udev-regel verwijderen | Verwijder alle udev-regels waarmee interface namen worden gereserveerd op basis van een Mac-adres, enzovoort.
Netwerk interfaces bijwerken | Werk netwerk interfaces bij om IP-adres te ontvangen op basis van DHCP.
SSH inschakelen | Zorg ervoor dat SSH is ingeschakeld en dat de sshd-service is ingesteld om automatisch te worden gestart bij het opnieuw opstarten.<br/> Zorg ervoor dat binnenkomende SSH-verbindings aanvragen niet worden geblokkeerd door de firewall van het besturings systeem of door script bare regels.

In [dit artikel](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic) wordt beschreven hoe u deze stappen volgt voor het uitvoeren van een virtuele Linux-machine in Azure en over de instructies voor een aantal populaire Linux-distributies.  


## <a name="replicate-vms"></a>Vm's repliceren

Als de detectie is voltooid, kunt u beginnen met de replicatie van virtuele VMware-machines naar Azure.

1. **Azure migrate in het Azure migrate project > **servers**: Server migratie**, klikt u op **repliceren**.

    ![Vm's repliceren](./media/tutorial-migrate-vmware/select-replicate.png)

2. In **repliceren**, > **bron instellingen** > **zijn uw machines gevirtualiseerd?** , selecteert u **Ja, met VMware vSphere**.
3. Selecteer in **on-premises apparaat**de naam van het Azure migrate apparaat dat u > **OK**hebt ingesteld. 

    ![Bron instellingen](./media/tutorial-migrate-vmware/source-settings.png)

    - In deze stap wordt ervan uitgegaan dat u al een apparaat hebt ingesteld wanneer u de zelf studie hebt voltooid.
    - Als u geen apparaat hebt ingesteld, volgt u de instructies in [dit artikel](how-to-set-up-appliance-vmware.md).

4. Selecteer in **virtuele machines**de computers die u wilt repliceren.
    - Als u een evaluatie voor de Vm's hebt uitgevoerd, kunt u de aanbevelingen voor VM-grootte en schijf type (Premium/Standard) Toep assen vanuit de evaluatie resultaten. Als u dit wilt doen, selecteert u in **migratie-instellingen importeren uit een Azure migrate beoordeling?** de optie **Ja** .
    - Als u geen beoordeling hebt uitgevoerd of als u de evaluatie-instellingen niet wilt gebruiken, selecteert u de optie **geen** .
    - Als u hebt geselecteerd om de evaluatie te gebruiken, selecteert u de VM-groep en de naam van de evaluatie.

    ![Evaluatie selecteren](./media/tutorial-migrate-vmware/select-assessment.png)

5. Zoek in **virtuele machines**naar behoefte aan vm's en controleer elke virtuele machine die u wilt migreren. Klik vervolgens **op volgende: Doel instellingen**.

    ![Vm's selecteren](./media/tutorial-migrate-vmware/select-vms.png)

6. Selecteer in **doel instellingen**het abonnement en de doel regio waarnaar u wilt migreren, en geef de resource groep op waarin de virtuele Azure-machines na de migratie worden geplaatst. In **Virtual Network**selecteert u het Azure VNet/subnet waaraan de virtuele Azure-machines na de migratie moeten worden toegevoegd.
7. In **Azure Hybrid Benefit**:

    - Selecteer **Nee** als u Azure Hybrid Benefit niet wilt Toep assen. Klik op **Volgende**.
    - Selecteer **Ja** als u Windows Server-computers hebt die worden gedekt door actieve Software Assurance-of Windows Server-abonnementen en u het voor deel wilt Toep assen op de machines die u wilt migreren. Klik op **Volgende**.

    ![Doel instellingen](./media/tutorial-migrate-vmware/target-settings.png)

8. Controleer in Compute de naam van de virtuele machine, de grootte, het schijf type van het besturings systeem en de beschikbaarheidsset. Vm's moeten voldoen aan de [vereisten van Azure](migrate-support-matrix-vmware.md#agentless-migration-vmware-vm-requirements).

    - **VM-grootte**: Als u evaluatie aanbevelingen gebruikt, bevat de vervolg keuzelijst VM-grootte de aanbevolen grootte. Anders Azure Migrate een grootte gekozen op basis van het dichtstbijzijnde resultaat van het Azure-abonnement. U kunt ook een hand matige grootte kiezen in de **Azure VM-grootte**. 
    - **Besturingssysteem schijf**: Geef de opstart schijf van het besturings systeem voor de virtuele machine op. De besturingssysteem schijf is de schijf met de bootloader van het besturings systeem en het installatie programma. 
    - **Beschikbaarheidsset**: Als de virtuele machine na de migratie een Azure-beschikbaarheidsset moet hebben, geeft u de set op. De set moet in de doel resource groep zijn die u opgeeft voor de migratie.

    ![Computer instellingen voor de virtuele machine](./media/tutorial-migrate-vmware/compute-settings.png)

9. Geef in **schijven**op of de VM-schijven moeten worden gerepliceerd naar Azure en selecteer het schijf type (standaard SSD/HDD of Premium-Managed disks) in Azure. Klik op **Volgende**.
    - U kunt schijven uitsluiten van replicatie.
    - Als u schijven uitsluit, wordt na de migratie niet weer gegeven op de Azure-VM. 

    ![Disks](./media/tutorial-migrate-vmware/disks.png)

10. Controleer in **controleren en starten van replicatie**de instellingen en klik op **repliceren** om de initiële replicatie voor de servers te starten.

> [!NOTE]
> U kunt replicatie-instellingen op elk gewenst moment bijwerken voordat de replicatie wordt gestart, in**replicerende machines** **beheren** > . De instellingen kunnen niet worden gewijzigd nadat de replicatie is gestart.

### <a name="provisioning-for-the-first-time"></a>De eerste keer inrichten

Als dit de eerste VM is die u repliceert in het Azure Migrate-project, worden deze resources door Azure Migrate Server Migration automatisch ingericht in dezelfde resource groep als het project.

- **Service Bus**: Azure Migrate server migratie gebruikt de service bus om replicatie-indelings berichten naar het apparaat te verzenden.
- **Gateway-opslag account**: Server migratie gebruikt het opslag account van de gateway om status informatie op te slaan over de virtuele machines die worden gerepliceerd.
- **Opslag account voor logboek**: Het Azure Migrate apparaat uploadt replicatie logboeken voor Vm's naar een logboek opslag account. Azure Migrate worden de replicatie gegevens toegepast op de door de replica beheerde schijven.
- **Sleutel kluis**: Het Azure Migrate apparaat gebruikt de sleutel kluis voor het beheren van verbindings reeksen voor de service bus en toegangs sleutels voor de opslag accounts die worden gebruikt voor replicatie. U moet de machtigingen instellen die de sleutel kluis nodig heeft voor toegang tot het opslag account tijdens de voor bereiding. [Controleer deze machtigingen](tutorial-prepare-vmware.md#assign-role-assignment-permissions).   


## <a name="track-and-monitor"></a>Bijhouden en controleren


- Wanneer **u op replicatie** starten klikt, begint de taak. 
- Wanneer de taak replicatie starten is voltooid, beginnen de machines hun initiële replicatie naar Azure.
- Tijdens de eerste replicatie wordt een VM-moment opname gemaakt. Schijf gegevens van de moment opname worden gerepliceerd naar met replica beheerde schijven in Azure.
- Nadat de initiële replicatie is voltooid, begint de replicatie van verschillen. Incrementele wijzigingen van on-premises schijven worden periodiek gerepliceerd naar de replica schijven in Azure.

U kunt de taak status volgen in de portal meldingen.

U kunt de replicatie status controleren door te  klikken op replicerende **servers in azure migrate: Server migratie**.
![Replicatie controleren](./media/tutorial-migrate-vmware/replicating-servers.png)




## <a name="run-a-test-migration"></a>Een testmigratie uitvoeren


Wanneer Delta replicatie begint, kunt u een test migratie voor de virtuele machines uitvoeren voordat u een volledige migratie naar Azure uitvoert. We raden u ten zeerste aan om dit ten minste één keer te doen voor elke machine voordat u deze migreert.

- Bij het uitvoeren van een test migratie wordt gecontroleerd of de migratie werkt zoals verwacht, zonder dat dit van invloed is op de on-premises machines, die operationeel blijven, en door gaan met repliceren. 
- Met test migratie wordt de migratie gesimuleerd door een Azure-VM te maken met behulp van gerepliceerde gegevens (die meestal worden gemigreerd naar een niet-productie-VNet in uw Azure-abonnement).
- U kunt de gerepliceerde virtuele machine van Azure gebruiken om de migratie te valideren, apps te testen en problemen op te lossen voordat u de volledige migratie uitvoert.

Ga als volgt te werk om een test migratie uit te voeren:


1. In **migratie doelen** > **servers** > Azuremigrate **: Server migratie**klikt u op **gemigreerde servers testen**.

     ![Gemigreerde servers testen](./media/tutorial-migrate-vmware/test-migrated-servers.png)

2. Klik met de rechter muisknop op de virtuele machine die u wilt testen en klik op gemigreerd **testen**.

    ![Migratie testen](./media/tutorial-migrate-vmware/test-migrate.png)

3. Selecteer bij **test migratie**het Azure-VNet waarin de virtuele Azure-machine na de migratie moet worden gevonden. U kunt het beste een niet-productie-VNet gebruiken.
4. De **test migratie** taak wordt gestart. Controleer de taak in de portal meldingen.
5. Nadat de migratie is voltooid, bekijkt u de gemigreerde Azure VM in **virtual machines** in het Azure Portal. De computer naam heeft een achtervoegsel **-test**.
6. Nadat de test is voltooid, klikt u met de rechter muisknop op de virtuele machine van Azure in **replicerende machines**en klikt **u op test migratie**opschonen.

    ![Migratie opruimen](./media/tutorial-migrate-vmware/clean-up.png)


## <a name="migrate-vms"></a>Virtuele machines migreren

Nadat u hebt gecontroleerd of de test migratie werkt zoals verwacht, kunt u de on-premises machines migreren.

1. In het Azure migrate project > **servers** > **Azure migrate: Server migratie**, klikt u op **servers repliceren**.

    ![Servers repliceren](./media/tutorial-migrate-vmware/replicate-servers.png)

2. Klik in **replicerende machines**met de rechter muisknop op de virtuele machine die > **migreren**.
3. Selecteer  >  JaOKbijhet > migreren van**virtuele machines afsluiten en het uitvoeren van een geplande migratie zonder gegevens verlies**.
    - Standaard Azure Migrate de on-premises VM afsluit en wordt een replicatie op aanvraag uitgevoerd om eventuele wijzigingen in de virtuele machine te synchroniseren sinds de laatste replicatie is uitgevoerd. Dit zorgt ervoor dat er geen gegevens verloren gaan.
    - Als u de virtuele machine niet wilt afsluiten, selecteert u **Nee**
4. Er wordt een migratie taak gestart voor de virtuele machine. Volg de taak in azure Notifications.
5. Nadat de taak is voltooid, kunt u de virtuele machine bekijken en beheren vanaf de pagina **virtual machines** .

## <a name="complete-the-migration"></a>Migratie voltooien

1. Nadat de migratie is voltooid, klikt u met de rechter muisknop op de virtuele machine > de **migratie stoppen**. Hiermee wordt de replicatie voor de on-premises computer gestopt en worden de gegevens van de replicatie status voor de virtuele machine opgeschoond.
2. Installeer de Azure VM [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) -of [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) -agent op de gemigreerde computers.
3. Voer correcties van de app uit na de migratie, zoals updates van de databaseverbindingsreeksen en webserverconfiguraties.
4. Voer acceptatietesten van de toepassing en de migratie uit op de gemigreerde toepassing die nu wordt uitgevoerd in Azure.
5. Verdeel het verkeer naar het gemigreerde Azure VM-exemplaar.
6. Verwijder de on-premises VM's uit uw lokale VM-inventaris.
7. Verwijder de on-premises VM's uit de lokale back-ups.
8. Werk eventuele interne documentatie bij met de nieuwe locatie en het nieuwe IP-adres van de Azure VM's. 

## <a name="post-migration-best-practices"></a>Best practices na de migratie

- Voor grotere flexibiliteit:
    - Houd uw gegevens veilig door back-ups van virtuele Azure VM‘s te maken met behulp van de Azure Backup-service. [Meer informatie](../backup/quick-backup-vm-portal.md).
    - Houd workloads continu beschikbaar door Azure VM‘s naar een secundaire regio te repliceren met Site Recovery. [Meer informatie](../site-recovery/azure-to-azure-tutorial-enable-replication.md).
- Voor betere beveiliging:
    - Vergrendel en beperk de toegang tot inkomend verkeer met [Azure Security Center-just-in-time-beheer](https://docs.microsoft.com/azure/security-center/security-center-just-in-time).
    - Beperk het netwerkverkeer naar beheereindpunten met [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/security-overview).
    - Implementeer [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview) om schijven te beveiligen en gegevens te beschermen tegen diefstal en onbevoegde toegang.
    - Lees meer informatie over [IaaS-resources beveiligen](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/) en bezoek het [Azure Security Center](https://azure.microsoft.com/services/security-center/).
- Voor controle en beheer:
-  Overweeg de implementatie van [Azure Cost Management](https://docs.microsoft.com/azure/cost-management/overview) om uw resourcegebruik en uitgaven te bewaken.


## <a name="next-steps"></a>Volgende stappen

Onderzoek de [reis voor Cloud migratie](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/migrate) in het Azure Cloud-acceptatie Framework.
