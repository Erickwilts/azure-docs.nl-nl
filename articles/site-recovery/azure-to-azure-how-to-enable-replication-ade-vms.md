---
title: Replicatie configureren voor Vm's met Azure Disk Encryption ingeschakeld in Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven hoe u replicatie kunt configureren voor Azure Disk Encryption Vm's van een Azure-regio naar een andere met behulp van Site Recovery.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/08/2019
ms.author: sutalasi
ms.openlocfilehash: b000610b5cba6f768a629ad797500a57597f2569
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68335691"
---
# <a name="replicate-azure-disk-encryption-enabled-virtual-machines-to-another-azure-region"></a>Virtuele machines die met Azure Disk Encryption zijn ingeschakeld repliceren naar een andere Azure-regio

In dit artikel wordt beschreven hoe u Azure Disk Encryption-Vm's van de ene Azure-regio naar de andere repliceert.

>[!NOTE]
>Azure Site Recovery ondersteunt momenteel alleen virtuele Azure-machines waarop een Windows-besturings systeem wordt uitgevoerd en die zijn [ingeschakeld voor versleuteling met Azure Active Directory (Azure AD)](https://aka.ms/ade-aad-app).

## <a id="required-user-permissions"></a>Vereiste gebruikers machtigingen
Site Recovery moet de gebruiker machtigingen hebben voor het maken van de sleutel kluis in de doel regio en het kopiëren van sleutels naar de regio.

De gebruiker heeft de volgende machtigingen nodig om replicatie van virtuele machines met schijf versleuteling in te scha kelen op basis van de Azure Portal:

- Sleutel kluis machtigingen
    - Weer geven, maken en ophalen
    
- Geheime machtigingen voor sleutel kluis
    - Geheimenbeheerbewerkingen
        - Ophalen, lijst en instellen
    
- Sleutel kluis sleutel machtigingen (alleen vereist als de virtuele machines de sleutel versleutelings sleutel gebruiken voor het versleutelen van schijf versleutelings sleutels)
    - Sleutelbeheerbewerkingen
        - Ophalen, vermelden en maken
    - Cryptografische bewerkingen
        - Ontsleutelen en versleutelen

Als u machtigingen wilt beheren, gaat u naar de resource van de sleutel kluis in de portal. Voeg de vereiste machtigingen voor de gebruiker toe. In het volgende voor beeld ziet u hoe u machtigingen inschakelt voor de sleutel kluis *ContosoWeb2Keyvault*die zich in de bron regio bevindt.

1. Ga naar **Home** > -ContosoWeb2KeyVault-**kluizen** >  **> toegangs beleid**.

   ![Het venster sleutel kluis machtigingen](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-1.png)

2. U kunt zien dat er geen gebruikers machtigingen zijn. Selecteer **Nieuw toevoegen**. Voer de gegevens van de gebruiker en de machtiging in.

   ![Machtigingen voor de sleutel kluis](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-2.png)

Als de gebruiker die nood herstel (DR) inschakelt, niet over de machtigingen beschikt om de sleutels te kopiëren, kan een beveiligings beheerder met de juiste machtigingen het volgende script gebruiken om de versleutelings geheimen en sleutels te kopiëren naar de doel regio.

Raadpleeg de [belang rijke problemen met machtigingen voor sleutel kluis](#trusted-root-certificates-error-code-151066) verderop in dit artikel voor het oplossen van problemen.

>[!NOTE]
>Als u de replicatie van virtuele machines met schijf versleuteling wilt inschakelen vanuit de portal, moet u ten minste ' lijst ' machtigingen hebben voor de sleutel kluizen, geheimen en sleutels.

## <a name="copy-disk-encryption-keys-to-the-dr-region-by-using-the-powershell-script"></a>Schijf versleutelings sleutels kopiëren naar de DR-regio met behulp van het Power shell-script

1. [Open de onbewerkte script code ' CopyKeys '](https://aka.ms/ade-asr-copy-keys-code).
2. Kopieer het script naar een bestand en geef het de naam **copy-Keys. ps1**.
3. Open de Windows Power shell-toepassing en ga naar de map waarin u het bestand hebt opgeslagen.
4. Voer Copy-Keys. ps1 uit.
5. Geef Azure-referenties op om u aan te melden.
6. Selecteer het **Azure-abonnement** van uw vm's.
7. Wacht tot de resource groepen zijn geladen en selecteer vervolgens de **resource groep** van uw vm's.
8. Selecteer de virtuele machines in de lijst die wordt weer gegeven. Alleen virtuele machines die zijn ingeschakeld voor schijf versleuteling, staan in de lijst.
9. Selecteer de **doel locatie**.

    - **Sleutel kluizen voor schijf versleuteling**
    - **Key Encryption Key-kluizen**

   Site Recovery maakt standaard een nieuwe sleutel kluis in de doel regio. De naam van de kluis heeft een ' ASR '-achtervoegsel dat is gebaseerd op de versleutelings sleutels van de bron-VM-schijf. Als er al een sleutel kluis bestaat die is gemaakt door Site Recovery, wordt deze opnieuw gebruikt. Selecteer indien nodig een andere sleutel kluis in de lijst.

## <a name="enable-replication"></a>Replicatie inschakelen

Voor dit voor beeld is de primaire Azure-regio Azië-oost en is de secundaire regio Zuid Azië-oost.

1. Selecteer **+ repliceren**in de kluis.
2. Let op de volgende velden.
    - **Bron**: Het oorsprongs punt van de virtuele machines, in dit geval **Azure**.
    - **Bron locatie**: De Azure-regio waar u uw virtuele machines wilt beveiligen. Voor dit voor beeld is de bron locatie Azië-oost.
    - **Implementatie model**: Het Azure-implementatie model van de bron machines.
    - **Bron abonnement**: Het abonnement waartoe de virtuele bron machines behoren. Dit kan elk abonnement zijn dat zich in dezelfde Azure Active Directory Tenant bevindt als de Recovery Services-kluis.
    - **Resourcegroep**: De resource groep waartoe de virtuele bron machines behoren. Alle virtuele machines in de geselecteerde resource groep worden in de volgende stap weer gegeven voor beveiliging.

3. Selecteer in **virtual machines** > **virtuele machines selecteren**de VM die u wilt repliceren. U kunt alleen machines selecteren waarvoor replicatie kan worden ingeschakeld. Selecteer vervolgens **OK**.

4. In **instellingen**kunt u de volgende instellingen voor de doel site configureren.

    - **Doellocatie**: De locatie waar de gegevens van de virtuele bron machine worden gerepliceerd. Site Recovery biedt een lijst met geschikte doel regio's op basis van de locatie van de geselecteerde machine. U wordt aangeraden dezelfde locatie te gebruiken als de locatie van de Recovery Services kluis.
    - **Doelabonnement**: Het doel abonnement dat wordt gebruikt voor herstel na nood gevallen. Het doel abonnement is standaard hetzelfde als het bron abonnement.
    - **Doelresourcegroep**: De resource groep waar al uw gerepliceerde virtuele machines deel van uitmaken. Site Recovery maakt standaard een nieuwe resource groep in de doel regio. Met de naam wordt het achtervoegsel ' ASR ' opgehaald. Als er al een resource groep bestaat die is gemaakt door Azure Site Recovery, wordt deze opnieuw gebruikt. U kunt er ook voor kiezen om deze aan te passen, zoals wordt weer gegeven in de volgende sectie. De locatie van de doel resource groep kan een wille keurige Azure-regio zijn, behalve de regio waarin de virtuele bron machines worden gehost.
    - **Virtueel doelnetwerk**: Site Recovery maakt standaard een nieuw virtueel netwerk in de doel regio. Met de naam wordt het achtervoegsel ' ASR ' opgehaald. Het wordt toegewezen aan uw bron netwerk en wordt gebruikt voor toekomstige beveiliging. [Meer informatie](site-recovery-network-mapping-azure-to-azure.md) over netwerk toewijzing.
    - **Doel opslag accounts (als uw bron-VM geen beheerde schijven gebruikt)** : Site Recovery maakt standaard een nieuw doel opslag account door de mimicking van de bron-VM te configureren. Als er al een opslag account bestaat, wordt dit opnieuw gebruikt.
    - **Beheerde replica schijven (als de bron-VM beheerde schijven gebruikt)** : Site Recovery maakt nieuwe replica beheerde schijven in de doel regio om de beheerde schijven van de bron-VM van hetzelfde opslag type (Standard of Premium) te spie gelen als de beheerde schijven van de bron-VM.
    - **Cacheopslagaccounts**: Site Recovery moet een extra opslag account met de naam *cache opslag* in de bron regio hebben. Alle wijzigingen op de bron-Vm's worden bijgehouden en verzonden naar het cache-opslag account. Ze worden vervolgens gerepliceerd naar de doel locatie.
    - **Beschikbaarheidsset**: Site Recovery maakt standaard een nieuwe beschikbaarheidsset in de doel regio. De naam heeft het achtervoegsel ' ASR '. Als een beschikbaarheidsset die is gemaakt door Site Recovery al bestaat, wordt deze opnieuw gebruikt.
    - **Sleutelkluizen voor schijfversleuteling**: Site Recovery maakt standaard een nieuwe sleutel kluis in de doel regio. Het bevat een "ASR"-achtervoegsel dat is gebaseerd op de versleutelings sleutels van de bron-VM-schijf. Als een sleutel kluis die is gemaakt door Azure Site Recovery al bestaat, wordt deze opnieuw gebruikt.
    - **Sleutelkluizen voor versleutelingssleutel**: Site Recovery maakt standaard een nieuwe sleutel kluis in de doel regio. De naam heeft een ' ASR '-achtervoegsel dat is gebaseerd op de versleutelings sleutels van de bron-VM. Als een sleutel kluis die is gemaakt door Azure Site Recovery al bestaat, wordt deze opnieuw gebruikt.
    - **Replicatie beleid**: Hiermee worden de instellingen voor de Bewaar geschiedenis voor herstel punten en de frequentie van de app-consistente moment opname gedefinieerd. Site Recovery maakt standaard een nieuw replicatie beleid met standaard instellingen van *24 uur* voor de Bewaar periode van het herstel punt en *60 minuten* voor de frequentie van de app-consistente moment opname.

## <a name="customize-target-resources"></a>Doel resources aanpassen

Volg deze stappen om de Site Recovery standaard doel instellingen te wijzigen.

1. Selecteer **aanpassen** naast doel abonnement om het standaard doel abonnement te wijzigen. Selecteer het abonnement in de lijst met abonnementen die beschikbaar zijn in de Azure AD-Tenant.

2. Selecteer **aanpassen** naast resource groep, netwerk, opslag en beschikbaarheids sets om de volgende standaard instellingen te wijzigen:
    - Voor **doel resource groep**selecteert u de resource groep in de lijst met resource groepen op de doel locatie van het abonnement.
    - Voor het **virtuele netwerk**van het doel selecteert u het netwerk in een lijst met virtuele netwerken op de doel locatie.
    - Voor **beschikbaarheidsset**kunt u instellingen voor beschikbaarheids sets toevoegen aan de VM als deze deel uitmaken van een beschikbaarheidsset in de bron regio.
    - Selecteer voor **doel opslag accounts**het account dat u wilt gebruiken.

2. Selecteer **aanpassen** naast versleutelings instellingen om de volgende standaard instellingen te wijzigen:
   - Selecteer voor de **doel schijf versleutelings sleutel**kluis de doel schijf versleutelings sleutel kluis in de lijst met sleutel kluizen op de doel locatie van het abonnement.
   - Selecteer de sleutel kluis van de doel sleutel versleutelen in de lijst met sleutel kluizen op de doel locatie van het abonnement.

3. Selecteer **doel bron** > maken**replicatie inschakelen**.
4. Nadat de Vm's zijn ingeschakeld voor replicatie, kunt u de status van de virtuele machines controleren onder **gerepliceerde items**.

>[!NOTE]
>Tijdens de eerste replicatie kan het enige tijd duren voordat de status is vernieuwd, zonder dat de voortgang wordt verduidelijkt. Klik op **vernieuwen** om de meest recente status op te halen.

## <a name="update-target-vm-encryption-settings"></a>Versleutelings instellingen voor de doel-VM bijwerken
In de volgende scenario's moet u de versleutelings instellingen voor de doel-VM bijwerken:
  - U hebt Site Recovery replicatie ingeschakeld op de VM. Later hebt u schijf versleuteling ingeschakeld op de bron-VM.
  - U hebt Site Recovery replicatie ingeschakeld op de VM. Later hebt u de schijf versleutelings sleutel of sleutel versleutelings sleutel gewijzigd op de bron-VM.

U kunt [een script](#copy-disk-encryption-keys-to-the-dr-region-by-using-the-powershell-script) gebruiken om de versleutelings sleutels te kopiëren naar de doel regio en vervolgens de instellingen voor doel versleuteling bij te werken in de **Recovery Services-kluis** > *gerepliceerde item* > **Eigenschappen**  >  **Compute en netwerk**.

![Dialoog venster ADE-instellingen bijwerken](./media/azure-to-azure-how-to-enable-replication-ade-vms/update-ade-settings.png)

## <a id="trusted-root-certificates-error-code-151066"></a>Problemen met machtigingen voor sleutel kluis oplossen tijdens replicatie van Azure naar Azure-VM'S

Azure Site Recovery moet ten minste lees machtigingen hebben voor de bron regio sleutel kluis en schrijf machtiging voor de sleutel kluis van de doel regio om het geheim te lezen en te kopiëren naar de sleutel kluis met de doel regio. 

**Oorzaak 1:** U beschikt niet over de machtiging ' GET ' voor de **sleutel kluis van de bron regio** om de sleutels te lezen. </br>
**Oplossen:** Ongeacht of u een abonnements beheerder bent of niet, is het belang rijk dat u de machtiging verkrijgen voor de sleutel kluis hebt.

1. Ga naar de bron regio sleutel kluis die in dit voor beeld ' ContososourceKeyvault ' is > **toegangs beleid** 
2. Onder **Principal selecteren** uw gebruikers naam toevoegen bijvoorbeeld:dradmin@contoso.com
3. Selecteer bij **sleutel machtigingen** Get 
4. Selecteer onder **geheime machtigingen** ophalen 
5. Het toegangs beleid opslaan

**Oorzaak 2:** U hebt niet de vereiste machtigingen voor de **sleutel kluis van de doel regio** om de sleutels te schrijven. </br>

*Bijvoorbeeld*: U probeert een virtuele machine met sleutel kluis *ContososourceKeyvault* te repliceren op een bron regio.
U hebt alle machtigingen voor de sleutel kluis van de bron regio. Maar tijdens de beveiliging selecteert u de al gemaakte sleutel kluis ContosotargetKeyvault, die geen machtigingen heeft. Er treedt een fout op.

Machtiging vereist voor de [doel sleutel kluis](#required-user-permissions)

**Oplossen:** Ga naar **Home** > -ContosotargetKeyvault > -**toegangs beleid** en voeg de juiste machtigingen toe. > 

## <a name="next-steps"></a>Volgende stappen

[Meer informatie](site-recovery-test-failover-to-azure.md) over het uitvoeren van een testfailover.
