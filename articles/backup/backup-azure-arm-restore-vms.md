---
title: 'Azure Backup: Virtuele machines herstellen met behulp van de Azure Portal'
description: Een virtuele Azure-machine herstellen vanaf een herstel punt met behulp van de Azure Portal
ms.reviewer: geg
author: dcurwin
manager: carmonm
keywords: back-up terugzetten; herstellen; herstel punt;
ms.service: backup
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: dacurwin
ms.openlocfilehash: f961f472c0b00932bf5ee6302af58f39fa8421ed
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68720448"
---
# <a name="restore-azure-vms"></a>Azure-VM's herstellen

In dit artikel wordt beschreven hoe u Azure VM-gegevens terugzet van de herstel punten die zijn opgeslagen in [Azure Backup](backup-overview.md) Recovery Services kluizen.



## <a name="restore-options"></a>Opties herstellen

Azure Backup biedt een aantal manieren om een virtuele machine te herstellen.

**Optie voor terugzetten** | **Details**
--- | ---
**Een nieuwe virtuele machine maken** | Maakt en haalt snel een standaard-VM op die vanaf een herstel punt actief is.<br/><br/> U kunt een naam opgeven voor de virtuele machine, de resource groep en het virtuele netwerk (VNet) selecteren waarin deze wordt geplaatst en een opslag account opgeven voor de herstelde VM.
**Schijf herstellen** | Hiermee wordt een VM-schijf teruggezet die vervolgens kan worden gebruikt om een nieuwe virtuele machine te maken.<br/><br/> Azure Backup biedt een sjabloon waarmee u een virtuele machine kunt aanpassen en maken. <br/><br> De herstel taak genereert een sjabloon die u kunt downloaden en gebruiken om aangepaste VM-instellingen op te geven en om een virtuele machine te maken.<br/><br/> De schijven worden gekopieerd naar het opslag account dat u opgeeft.<br/><br/> U kunt de schijf ook koppelen aan een bestaande virtuele machine of een nieuwe virtuele machine maken met behulp van Power shell.<br/><br/> Deze optie is handig als u de virtuele machine wilt aanpassen, configuratie-instellingen wilt toevoegen die niet aanwezig zijn op het moment van de back-up of instellingen toevoegen die moeten worden geconfigureerd met de sjabloon of Power shell.
**Bestaande vervangen** | U kunt een schijf herstellen en gebruiken om een schijf op de bestaande virtuele machine te vervangen.<br/><br/> De huidige VM moet bestaan. Als de optie is verwijderd, kan deze niet worden gebruikt.<br/><br/> Azure Backup maakt een moment opname van de bestaande virtuele machine voordat de schijf wordt vervangen en slaat deze op in de faserings locatie die u opgeeft. Bestaande schijven die zijn verbonden met de virtuele machine, worden vervangen door het geselecteerde herstel punt.<br/><br/> De moment opname wordt gekopieerd naar de kluis en bewaard in overeenstemming met het Bewaar beleid. <br/><br/> Bestaande vervangen wordt ondersteund voor niet-versleutelde beheerde Vm's. Het wordt niet ondersteund voor niet-beheerde schijven, [gegeneraliseerde vm's](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource)of vm's [die zijn gemaakt met behulp van aangepaste installatie kopieën](https://azure.microsoft.com/resources/videos/create-a-custom-virtual-machine-image-in-azure-resource-manager-with-powershell/).<br/><br/> Als het herstel punt meer of minder schijven heeft dan de huidige virtuele machine, wordt in het aantal schijven in het herstel punt alleen de VM-configuratie weer gegeven.<br/><br/>


> [!NOTE]
> U kunt ook specifieke bestanden en mappen op een virtuele Azure-machine herstellen. [Meer informatie](backup-azure-restore-files-from-vm.md).
>
> Als u de [nieuwste versie](backup-instant-restore-capability.md) van Azure backup voor virtuele machines van Azure (ook wel direct terugzetten genoemd) gebruikt, worden moment opnamen Maxi maal zeven dagen bewaard en kunt u een VM herstellen vanaf moment opnamen voordat de back-upgegevens naar de kluis worden verzonden. Als u een virtuele machine uit een back-up van de afgelopen zeven dagen wilt herstellen, is het sneller om te herstellen vanuit de moment opname en niet vanuit de kluis.

## <a name="storage-accounts"></a>Opslagaccounts

Enkele details over opslag accounts:

- **VM maken**: Wanneer u een nieuwe virtuele machine maakt, wordt de virtuele machine opgenomen in het opslag account dat u opgeeft.
- **Herstel schijf**: Wanneer u een schijf herstelt, wordt de schijf gekopieerd naar het opslag account dat u opgeeft. De herstel taak genereert een sjabloon die u kunt downloaden en gebruiken om aangepaste VM-instellingen op te geven. Deze sjabloon wordt opgenomen in het opgegeven opslag account.
- **Vervang de schijf**: Wanneer u een schijf op een bestaande virtuele machine vervangt, maakt Azure Backup een moment opname van de bestaande virtuele machine voordat de schijf wordt vervangen. De moment opname wordt opgeslagen op de faserings locatie (opslag account) die u opgeeft. Dit opslag account wordt gebruikt om de moment opname tijdelijk op te slaan tijdens het herstel proces. u wordt aangeraden een nieuw account te maken om dit te doen. Dit kan later gemakkelijk worden verwijderd.
- **Locatie van opslag account** : Het opslag account moet zich in dezelfde regio bevinden als de kluis. Alleen deze accounts worden weer gegeven. Als de locatie geen opslag accounts bevat, moet u er een maken.
- **Opslag type** : Blob-opslag wordt niet ondersteund.
- **Opslag redundantie**: Zone-redundante opslag (ZRS) wordt niet ondersteund. De gegevens over replicatie en redundantie voor het account worden tussen haakjes weer gegeven na de account naam. 
- **Premium-opslag**:
    - Wanneer u niet-Premium Vm's herstelt, worden Premium-opslag accounts niet ondersteund.
    - Premium Storage-accounts die zijn geconfigureerd met netwerk regels worden niet ondersteund bij het herstellen van beheerde Vm's.


## <a name="before-you-start"></a>Voordat u begint

Als u een virtuele machine wilt herstellen (een nieuwe virtuele machine maken), zorg er dan voor dat u de juiste RBAC- [machtigingen](backup-rbac-rs-vault.md#mapping-backup-built-in-roles-to-backup-management-actions) (op rollen gebaseerde toegangs beheer) hebt voor de bewerking VM herstellen.

Als u geen machtigingen hebt, kunt u [een schijf herstellen](#restore-disks)en nadat de schijf is hersteld, kunt u [de sjabloon](#use-templates-to-customize-a-restored-vm) die is gegenereerd als onderdeel van de herstel bewerking, gebruiken om een nieuwe virtuele machine te maken.



## <a name="select-a-restore-point"></a>Selecteer een herstel punt

1. Klik in de kluis die is gekoppeld aan de virtuele machine die u wilt herstellen op **back-**  > upitems**Azure virtual machine**.
2. Klik op een virtuele machine. Standaard worden in het VM-dash board herstel punten van de afgelopen 30 dagen weer gegeven. U kunt herstel punten ouder dan 30 dagen weer geven of filteren om herstel punten te vinden op basis van datums, peri Oden en verschillende soorten consistentie van moment opnamen.
3. Als u de virtuele machine wilt herstellen, klikt u op **VM herstellen**.

    ![Herstelpunt](./media/backup-azure-arm-restore-vms/restore-point.png)

4. Selecteer een herstel punt dat voor het herstel moet worden gebruikt.

## <a name="choose-a-vm-restore-configuration"></a>Een configuratie voor het herstellen van VM'S kiezen

1. Selecteer in **configuratie herstellen**een optie voor terugzetten:
    - **Nieuwe maken**: Gebruik deze optie als u een nieuwe virtuele machine wilt maken. U kunt een virtuele machine maken met eenvoudige instellingen, of een schijf herstellen en een aangepaste virtuele machine maken.
    - **Bestaande vervangen**: Gebruik deze optie als u de schijven op een bestaande virtuele machine wilt vervangen.

        ![Wizard configuratie herstellen](./media/backup-azure-arm-restore-vms/restore-configuration.png)

2. Geef de instellingen voor de geselecteerde terugzet optie op.

## <a name="create-a-vm"></a>Een virtuele machine maken

Als een van de [Opties voor terugzetten](#restore-options)kunt u snel een virtuele machine maken met de basis instellingen van een herstel punt.

1. In **herstel configuratie** > **Nieuw** > **herstel type**maken selecteert u **een virtuele machine maken**.
2. Geef in **naam van virtuele machine**een virtuele machine op die niet voor komt in het abonnement.
3. Selecteer in **resource groep**een bestaande resource groep voor de nieuwe virtuele machine of maak een nieuwe met een wereld wijd unieke naam. Als u een naam toewijst die al bestaat, wijst Azure de groep dezelfde naam als de virtuele machine toe.
4. Selecteer in **virtueel netwerk**het VNet waarin de virtuele machine wordt geplaatst. Alle VNets die aan het abonnement zijn gekoppeld, worden weer gegeven. Selecteer het subnet. Het eerste subnet is standaard geselecteerd.
5. Geef in **opslag locatie**het opslag account voor de virtuele machine op. [Meer informatie](#storage-accounts).

    ![Wizard configuratie herstellen](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard1.png)

6. Selecteer in **configuratie herstellen**de optie **OK**. Klik in **herstellen**op **herstellen** om de herstel bewerking te activeren.


## <a name="restore-disks"></a>Schijven herstellen

Als een van de [Opties voor terugzetten](#restore-options)kunt u een schijf maken op basis van een herstel punt. Vervolgens kunt u met de schijf een van de volgende handelingen uitvoeren:

- Gebruik de sjabloon die tijdens de herstel bewerking is gegenereerd om de instellingen aan te passen en VM-implementatie te activeren. U bewerkt de standaard sjabloon instellingen en verzendt de sjabloon voor de implementatie van de virtuele machine.
- De herstelde [schijven koppelen](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal) aan een bestaande virtuele machine.
- [Maak een nieuwe virtuele machine](https://docs.microsoft.com/azure/backup/backup-azure-vms-automation#create-a-vm-from-restored-disks) op basis van de herstelde schijven met behulp van Power shell.

1. Selecteer in **configuratie** > herstellen**nieuwe** > **herstel type**maken de optie **schijven herstellen**.
2. Selecteer in **resource groep**een bestaande resource groep voor de herstelde schijven of maak een nieuw item met een wereld wijd unieke naam.
3. Geef in **opslag account**het account op waarnaar de vhd's moeten worden gekopieerd. [Meer informatie](#storage-accounts).

    ![Herstel configuratie is voltooid](./media/backup-azure-arm-restore-vms/trigger-restore-operation1.png)

4. Selecteer in **configuratie herstellen**de optie **OK**. Klik in **herstellen**op **herstellen** om de herstel bewerking te activeren.

Als uw virtuele machine gebruikmaakt van beheerde schijven en u de optie **virtuele machine maken** selecteert, gebruikt Azure Backup niet het opgegeven opslag account. In het geval van **herstel schijven** en **direct terugzetten**wordt het opslag account alleen gebruikt voor het opslaan van de sjabloon. Beheerde schijven worden gemaakt in de opgegeven resource groep.
Als uw virtuele machine gebruikmaakt van niet-beheerde schijven, worden deze hersteld als blobs naar het opslag account.

### <a name="use-templates-to-customize-a-restored-vm"></a>Sjablonen gebruiken om een herstelde VM aan te passen

Nadat de schijf is hersteld, gebruikt u de sjabloon die is gegenereerd als onderdeel van de herstel bewerking om een nieuwe virtuele machine aan te passen en te maken:

1. Open de **herstel taak Details** voor de relevante taak.

2. Selecteer in **taak Details herstellen**de optie **sjabloon implementeren** om de implementatie van een sjabloon te initiëren.

    ![Inzoomen op taak herstellen](./media/backup-azure-arm-restore-vms/restore-job-drill-down1.png)

3. Als u de VM-instelling die in de sjabloon is opgenomen, wilt aanpassen, klikt u op **sjabloon bewerken**. Als u meer aanpassingen wilt toevoegen, klikt u op **para meters bewerken**.
    - Meer [informatie](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template) over het implementeren van resources vanuit een aangepaste sjabloon.
    - [Meer informatie](../azure-resource-manager/resource-group-authoring-templates.md) over het ontwerpen van sjablonen.

   ![Implementatie van sjabloon laden](./media/backup-azure-arm-restore-vms/edit-template1.png)

4. Voer de aangepaste waarden voor de virtuele machine in, accepteer de **voor waarden** en klik op **kopen**.

   ![Sjabloon implementatie verzenden](./media/backup-azure-arm-restore-vms/submitting-template1.png)


## <a name="replace-existing-disks"></a>Bestaande schijven vervangen

Als een van de [Opties voor terugzetten](#restore-options)kunt u een bestaande VM-schijf vervangen door het geselecteerde herstel punt. Alle terugzet opties [bekijken](#restore-options) .

1. Klik in **herstel configuratie**op **bestaande vervangen**.
2. Selecteer in **type herstel**de optie **schijf/s vervangen**. Dit is het herstel punt dat wordt gebruikt om bestaande VM-schijven te vervangen.
3. Geef bij **faserings locatie**op waar moment opnamen van de huidige beheerde schijven moeten worden opgeslagen tijdens het herstel proces. [Meer informatie](#storage-accounts).

   ![Wizard configuratie herstellen bestaande vervangen](./media/backup-azure-arm-restore-vms/restore-configuration-replace-existing.png)


## <a name="restore-vms-with-special-configurations"></a>Vm's herstellen met speciale configuraties

Er zijn een aantal algemene scenario's waarin u mogelijk Vm's moet herstellen.

**Scenario** | **Hulp**
--- | ---
**Vm's herstellen met voor deel voor hybride gebruik** | Als een virtuele Windows-machine gebruikmaakt van [hybride licenties voor voor delen (hub)](../virtual-machines/windows/hybrid-use-benefit-licensing.md), herstelt u de schijven en maakt u een nieuwe virtuele machine met behulp van de meegeleverde sjabloon (waarbij **licentie type** is ingesteld op **Windows_Server**) of Power shell.  Deze instelling kan ook worden toegepast nadat de virtuele machine is gemaakt.
**Vm's herstellen tijdens een nood geval in azure Data Center** | Als de kluis gebruikmaakt van GRS en het primaire Data Center voor de virtuele machine uitvalt, Azure Backup ondersteunt het herstellen van back-ups van virtuele machines naar het gekoppelde Data Center. Selecteer een opslag account in het gekoppelde Data Center en herstel als normaal. Azure Backup gebruikt de compute-service op de gekoppelde locatie om de herstelde VM te maken. Meer [informatie](../resiliency/resiliency-technical-guidance-recovery-loss-azure-region.md) over de flexibiliteit van data centers.
**VM van één domein controller herstellen in één domein** | Herstel de VM, zoals elke andere virtuele machine. Houd rekening met het volgende:<br/><br/> Vanuit een Active Directory perspectief is de virtuele machine van Azure net als elke andere virtuele machine.<br/><br/> Directory Services Restore Mode (DSRM) is ook beschikbaar, dus alle Active Directory herstel scenario's zijn levensvatbaar. Meer [informatie](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/virtualized-domain-controllers-hyper-v) over het maken van back-ups en het herstellen van overwegingen voor gevirtualiseerde domein controllers.
**Meerdere Vm's van domein controllers in één domein herstellen** | Als andere domein controllers in hetzelfde domein kunnen worden bereikt via het netwerk, kan de domein controller worden hersteld, zoals elke VM. Als het de laatste domein controller in het domein is, of als er een herstel in een geïsoleerd netwerk is uitgevoerd, gebruikt u een [forest recovery](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/ad-forest-recovery-single-domain-in-multidomain-recovery).
**Meerdere domeinen in één forest herstellen** | We raden u aan een [forest-herstel](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/ad-forest-recovery-single-domain-in-multidomain-recovery)uit te voeren.
**Bare-metal terugzet bewerking** | Het belangrijkste verschil tussen Azure Vm's en on-premises Hyper visors is dat er geen VM-console beschikbaar is in Azure. Een-console is vereist voor bepaalde scenario's, zoals het herstellen met behulp van een Bare-Metal Recovery (BMR)-type back-up. Het terugzetten van de VM vanuit de kluis is echter een volledige vervanging van BMR.
**Vm's herstellen met speciale netwerk configuraties** | Speciale netwerk configuraties zijn Vm's met behulp van interne of externe taak verdeling, met behulp van meerdere NIC'S of meerdere gereserveerde IP-adressen. U herstelt deze Vm's met behulp van de [optie schijf herstellen](#restore-disks). Met deze optie maakt u een kopie van de Vhd's naar het opgegeven opslag account en kunt u vervolgens een virtuele machine maken met een [intern](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/) of [extern](https://azure.microsoft.com/documentation/articles/load-balancer-internet-getstarted/) Load Balancer, [meerdere nic's](../virtual-machines/windows/multiple-nics.md)of [meerdere gereserveerde IP-adressen](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md), in overeenstemming met uw configuratie.
**Netwerk beveiligings groep (NSG) op NIC/subnet** | Azure VM Backup ondersteunt het maken van back-ups en het herstellen van NSG-informatie op vnet-, subnet-en NIC-niveau.
**Met zone vastgemaakte Vm's** | Azure Backup ondersteunt het maken van back-ups en het herstellen van vastgemaakte Vm's in een zone. [Meer informatie](https://azure.microsoft.com/global-infrastructure/availability-zones/)

## <a name="track-the-restore-operation"></a>De herstel bewerking bijhouden
Nadat u de herstel bewerking hebt geactiveerd, maakt de back-upservice een taak voor tracering. Azure Backup worden meldingen over de taak weer gegeven in de portal. Als ze niet zichtbaar zijn, selecteert u het **meldingen** symbool en selecteert u vervolgens **alle taken weer geven** om de status van het herstel proces te bekijken.

![Herstellen geactiveerd](./media/backup-azure-arm-restore-vms/restore-notification1.png)

 Volg de volgende stappen om de herstel bewerking te volgen:

1. Als u bewerkingen voor de taak wilt weer geven, klikt u op de koppeling meldingen. U kunt ook op **back-uptaken**in de kluis klikken en vervolgens op de relevante virtuele machine klikken.

    ![Lijst met virtuele machines in een kluis](./media/backup-azure-arm-restore-vms/restore-job-in-progress1.png)

2. Als u de voortgang van de herstel bewerking wilt controleren, klikt u op een restore-taak met de status **in uitvoering**. Hiermee wordt de voortgangs balk weer gegeven met informatie over de voortgang van het terugzetten:

    - **Geschatte tijd van herstellen**: In eerste instantie is de tijd die nodig is om de herstel bewerking te volt ooien. Wanneer de bewerking wordt uitgevoerd, wordt de tijd die nodig is om de herstel bewerking te volt ooien, gereduceerd en bereikt.
    - Het **percentage van de herstel bewerking**. Toont het percentage van de herstel bewerking dat is uitgevoerd.
    - **Aantal overgedragen bytes**: Als u herstelt door een nieuwe VM te maken, worden de bytes weer gegeven die zijn overgedragen met het totale aantal bytes dat moet worden overgedragen.

## <a name="post-restore-steps"></a>Stappen na herstel

Er zijn een aantal dingen die u moet weten na het herstellen van een virtuele machine:

- Uitbrei dingen die aanwezig zijn tijdens de back-upconfiguratie, worden geïnstalleerd, maar niet ingeschakeld. Als er een probleem wordt weer geven, installeert u de uitbrei dingen opnieuw.
- Als de back-up van de virtuele machine een statisch IP-adres heeft, heeft de herstelde VM een dynamisch IP-adres om een conflict te voor komen. U kunt [een statisch IP-adres toevoegen aan de herstelde VM](../virtual-network/virtual-networks-reserved-private-ip.md#how-to-add-a-static-internal-ip-to-an-existing-vm).
- Een herstelde VM heeft geen beschikbaarheidsset. Als u de optie schijf herstellen gebruikt, kunt u [een beschikbaarheidsset opgeven](../virtual-machines/windows/tutorial-availability-sets.md) wanneer u een virtuele machine van de schijf maakt met behulp van de meegeleverde sjabloon of Power shell.
- Als u een Linux-distributie op basis van Cloud-init gebruikt, zoals Ubuntu, om veiligheids redenen wordt het wacht woord na het herstellen geblokkeerd. Gebruik de VMAccess-extensie op de herstelde VM om [het wacht woord opnieuw](../virtual-machines/linux/reset-password.md)in te stellen. U kunt het beste SSH-sleutels gebruiken voor deze distributies, dus u hoeft het wacht woord niet opnieuw in te stellen na het herstellen.


## <a name="backing-up-restored-vms"></a>Back-ups maken van herstelde Vm's

- Als u een virtuele machine hebt hersteld naar dezelfde resource groep met dezelfde naam als de oorspronkelijke back-up van de virtuele machine, wordt de back-up na het herstellen voortgezet op de virtuele machine.
- Als u de VM hebt hersteld naar een andere resource groep of als u een andere naam hebt opgegeven voor de herstelde VM, moet u een back-up instellen voor de herstelde VM.

## <a name="next-steps"></a>Volgende stappen

- Als u problemen ondervindt tijdens het herstel [](backup-azure-vms-troubleshoot.md#restore) proces, raadpleegt u veelvoorkomende problemen en fouten.
- Meer informatie over het [beheren van virtuele machines](backup-azure-manage-vms.md) nadat de virtuele machine is hersteld
