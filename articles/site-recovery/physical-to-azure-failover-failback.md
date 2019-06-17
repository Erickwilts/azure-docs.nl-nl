---
title: Failover en failback van fysieke servers voor herstel na noodgevallen naar Azure met Site Recovery | Microsoft Docs
description: Leer hoe u fysieke servers naar Azure failover en failback uitvoeren naar de on-premises site voor herstel na noodgeval met Azure Site Recovery
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 05/30/2019
ms.author: raynew
ms.openlocfilehash: 14fa5822c575f2d2d60a956263cf916ee8f9bb4d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66400038"
---
# <a name="fail-over-and-fail-back-physical-servers-replicated-to-azure"></a>Failover en failback van fysieke servers gerepliceerd naar Azure

Deze zelfstudie wordt beschreven hoe u een fysieke server naar Azure failover. Nadat u een failover hebt, schakelt u de server terug naar uw on-premises site wanneer deze beschikbaar is.

## <a name="preparing-for-failover-and-failback"></a>Voorbereiden voor failover en failback

Fysieke servers gerepliceerd naar Azure met Site Recovery kunnen alleen een failover weer als virtuele VMware-machines. Een VMware-infrastructuur hebt u nodig om een failback uitvoeren.

Failover en failback bestaat uit vier fasen:

1. **Failover naar Azure**: Failover van computers van de on-premises site naar Azure.
2. **Azure-VM’s opnieuw beveiligen**: De Azure-VM's beveiligen zodat ze gaan repliceren naar on-premises VMware-VM's.
3. **Failover naar on-premises**: Een failover uitvoeren voor failback vanuit Azure.
4. **On-premises VM's opnieuw beveiligen**: Nadat de gegevens terug is mislukt, opnieuw beveiligen de on-premises VMware-machines die u niet terug naar, zodat ze gaan repliceren naar Azure.

## <a name="verify-server-properties"></a>Controleer de servereigenschappen van

Controleer de servereigenschappen van de en zorg ervoor dat het voldoet aan [Azure-vereisten](vmware-physical-azure-support-matrix.md#replicated-machines) voor virtuele Azure-machines.

1. In **beveiligde Items**, klikt u op **gerepliceerde Items**, en selecteer de machine.

2. In de **gerepliceerd item** deelvenster bevindt zich een overzicht van machine-informatie, status, en de laatste beschikbare herstelpunten. Klik op **Eigenschappen** om meer details te bekijken.
3. In **Berekening en netwerk** kunt u de Azure-naam, de resourcegroep, de doelgrootte, [beschikbaarheidsset](../virtual-machines/windows/tutorial-availability-sets.md) en beheerde-schijfinstellingen wijzigen
4. U kunt de netwerkinstellingen bekijken en wijzigen, inclusief het netwerk-/subnet waarin de Azure VM zich na failover bevindt en het IP-adres dat eraan wordt toegewezen.
5. In **schijven**, ziet u informatie over de computer, besturingssysteem en gegevensschijven.

## <a name="run-a-failover-to-azure"></a>Een failover naar Azure uitvoeren

1. Klik in **Instellingen** > **Gerepliceerde items** op de machine > **Failover**.
2. Selecteer in **Failover** een **Herstelpunt** waarnaar u de failover wilt uitvoeren. U kunt een van de volgende opties gebruiken:
   - **Laatste**: Met deze optie worden eerst alle gegevens verwerkt die naar Site Recovery zijn verzonden. Dit biedt het laagste RPO (Recovery Point Objective), omdat de na de failover gemaakte Azure-VM alle gegevens heeft die naar Site Recovery is gerepliceerd toen de failover werd geactiveerd.
   - **Laatst verwerkt**: Deze optie wordt er via de machine naar de meest recente herstelpunt dat is verwerkt door Site Recovery. Deze optie heeft een lage RTO (Recovery Time Objective), omdat er geen tijd wordt besteed aan het verwerken van niet-verwerkte gegevens.
   - **Laatste toepassingsconsistente punt**: Deze optie wordt er via de machine naar de meest recente app-consistente herstelpunt verwerkt door Site Recovery.
   - **Aangepast**: Geef een herstelpunt op.

3. Selecteer **sluit de computer voordat u begint met failover** als u wilt dat Site Recovery op de bronmachine afsluiten voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**.
4. Als u de verbinding met de Azure VM hebt voorbereid, maakt u na de failover verbinding om deze te valideren.
5. Na het verifiëren kunt u de failover **Doorvoeren**. Hiermee verwijdert u alle beschikbare herstelpunten.

> [!WARNING]
> Annuleer een failover wordt uitgevoerd niet. Voordat de failover is gestart, gestopt machinereplicatie. Als u de failover te annuleren, wordt gestopt, maar de machine wordt niet meer gerepliceerd.
> Voor fysieke servers, kan de failover extra verwerking ongeveer acht tot tien minuten duren om uit te voeren.

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Als u na een failover verbinding wilt maken met virtuele Azure-machines met behulp van RDP/SSH, volgt u de procedure die [hier](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) wordt beschreven.

Volg de stappen die [hier](site-recovery-failover-to-azure-troubleshoot.md) worden beschreven om eventuele verbindingsproblemen na een failover op te lossen.

## <a name="create-a-process-server-in-azure"></a>Een processerver maken in Azure

De processerver ontvangt gegevens van de Azure VM en verzendt deze naar de on-premises site. Een netwerk met lage latentie is vereist tussen de processerver en de beveiligde machine.

- Als u een Azure ExpressRoute-verbinding hebt, kunt u voor testdoeleinden de on-premises processerver gebruiken die automatisch wordt geïnstalleerd op de configuratieserver.
- Als u een VPN-verbinding hebt of als u failback uitvoert in een productieomgeving, moet u een Azure-VM instellen als een Azure-processerver voor failback.
- Volg de instructies in [in dit artikel](vmware-azure-set-up-process-server-azure.md) voor het instellen van een processerver in Azure.

## <a name="configure-the-master-target-server"></a>De hoofddoelserver configureren

Standaard ontvangt de hoofddoelserver failbackgegevens. Deze wordt uitgevoerd op de on-premises configuratieserver.

- Als de VMware-VM die u failback zich op een ESXi-host die wordt beheerd door een VMware vCenter Server, moet de hoofddoelserver toegang tot gegevensopslag van de VM (VMDK), hebben gerepliceerde gegevens schrijven naar de VM-schijven. Zorg ervoor dat de VM-gegevensopslag is gekoppeld op de host van het hoofddoel, met lees-/schrijftoegang.
- Als de ESXi-host die niet wordt beheerd door een vCenter-server, Site Recovery-service een nieuwe virtuele machine tijdens het opnieuw beveiligen maakt. De virtuele machine wordt gemaakt op de ESX-host waarop u de virtuele machine in het hoofddoel maakt. De harde schijf van de VM moet zich in een gegevensopslag bevinden die toegankelijk is voor de host waarop de hoofddoelserver wordt uitgevoerd.
- Voor fysieke computers die u failback, moet u detectie van de host waarop de hoofddoelserver wordt uitgevoerd, voordat u opnieuw met de machine beveiligen kunt uitvoeren.
- Een andere optie is als de on-premises VM al voor failback bestaat, deze verwijderen voordat u een failback. Failback maakt vervolgens een nieuwe VM op dezelfde host als de ESX-host van de hoofddoelserver. Wanneer u een failback uitvoert naar een alternatieve locatie, worden de gegevens hersteld naar dezelfde gegevensopslag en dezelfde ESX-host als die welke gebruikt wordt door de on-premises hoofddoelserver.
- U kunt Storage vMotion niet gebruiken op de hoofddoelserver. Als u dat doet, werkt failback niet, omdat de schijven er niet beschikbaar voor zijn. Sluit de hoofddoelservers uit van uw vMotion-lijst.

## <a name="reprotect-azure-vms"></a>Azure VM’s opnieuw beveiligen

Bij deze procedure wordt ervan uitgegaan dat de on-premises VM niet beschikbaar is en dat u opnieuw beschermt naar een alternatieve locatie.

1. Klik in **Instellingen** > **Gerepliceerde items** met de rechtermuisknop op de VM waarvoor een failover-overschakeling is uitgevoerd > **Opnieuw beveiligen**.
2. Controleer in **Opnieuw beveiligen** of **Azure naar on-premises** is geselecteerd.
3. Geef de on-premises hoofddoelserver en de processerver op.

4. Selecteer in **Gegevensopslag** de gegevensopslag van het hoofddoel waarnaar u de on-premises schijven wilt herstellen. Gebruik deze optie wanneer de on-premises VM is verwijderd en u nieuwe schijven moet maken. Deze instellingen worden genegeerd als de schijven al bestaan, maar u moet wel een waarde opgeven.
5. Selecteer het bewaarstation van het hoofddoel. Het failback-beleid wordt automatisch geselecteerd.
6. Klik op **OK** om te beginnen met opnieuw beveiligen. Er wordt een taak gestart voor het repliceren van de VM van Azure naar de on-premises site. U kunt de voortgang volgen op het tabblad **Taken**.

> [!NOTE]
> Als u de Azure-VM wilt herstellen naar een bestaande on-premises VM, koppelt u de gegevensopslag van de virtuele machine met lees-/schrijf-toegang aan de ESXi-host van de hoofddoelserver.


## <a name="run-a-failover-from-azure-to-on-premises"></a>Een failover van Azure naar on-premises uitvoeren

Voor opnieuw repliceren naar on-premises wordt een failbackbeleid gebruikt. Dit beleid wordt automatisch gemaakt wanneer u een replicatiebeleid voor replicatie naar Azure hebt gemaakt:

- Het beleid wordt automatisch gekoppeld aan de configuratieserver.
- Het beleid kan niet worden gewijzigd.
- De beleidswaarden zijn:
    - RPO-drempelwaarde = 15 minuten
    - Bewaarperiode van het herstelpunt = 24 uur
    - Frequentie van de app-consistente momentopname = 60 minuten

Voer de failover als volgt uit:

1. Klik op de pagina **Gerepliceerde items** met de rechtermuisknop op de computer > **Niet-geplande failover**.
2. Controleer in **Failover bevestigen** of de failoverrichting van Azure is.

3. Selecteer het herstelpunt dat u wilt gebruiken voor de failover. Een app-consistent herstelpunt treedt op voor het meest recente tijdstip, en dit veroorzaakt enig gegevensverlies. Wanneer failover wordt uitgevoerd, schakelt Site Recovery de Azure VM's uit en start de on-premises VM op. Er zal wat downtime zijn, dus kies een geschikte tijd.
4. Klik met de rechtermuisknop op de computer en klik op **Doorvoeren**. Dit activeert een taak waardoor de Azure VM's worden verwijderd.
5. Controleer of de Azure VM's zijn afgesloten zoals verwacht.


## <a name="reprotect-on-premises-machines-to-azure"></a>On-premises computers opnieuw beveiligen naar Azure

De gegevens moeten nu weer terug zijn op uw on-premises site, maar ze worden niet gerepliceerd naar Azure. U kunt als volgt weer beginnen met repliceren naar Azure:

1. Selecteer in de kluis > **Instellingen** >**Gerepliceerde items** de failback-VM's waarvoor een failback is uitgevoerd en klik op **Opnieuw beveiligen**.
2. Selecteer de processerver die wordt gebruikt om de gerepliceerde gegevens naar Azure te verzenden en klik op **OK**.

Nadat het opnieuw beveiligen is voltooid, repliceert de VM weer naar Azure en kunt u zo nodig een failover uitvoeren.
