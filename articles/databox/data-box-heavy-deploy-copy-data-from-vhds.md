---
title: Zelfstudie voor het kopiëren van gegevens vanaf VHD's naar beheerde schijven met Azure Data Box zware | Microsoft Docs
description: Informatie over het kopiëren van gegevens vanaf VHD's van on-premises VM-workloads voor uw Azure Data Box zware
services: databox
author: alkohli
ms.service: databox
ms.subservice: Heavy
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: alkohli
ms.openlocfilehash: 8065d29c0cb984244178d49fe8c8c5aa853ee682
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595761"
---
# <a name="tutorial-use-data-box-heavy-to-import-data-as-managed-disks-in-azure"></a>Zelfstudie: Gebruik Data Box zware voor het importeren van gegevens als beheerde schijven in Azure

Deze zelfstudie wordt beschreven hoe u met Azure Data Box zware kunt u on-premises VHD's migreren naar managed disks in Azure. De VHD's van de on-premises VM's worden gekopieerd naar Data Box zware als pagina-blobs en zijn geüpload naar Azure als beheerde schijven. Deze beheerde schijven kunnen vervolgens worden gekoppeld aan Azure-VM's.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Lees de voorwaarden
> * Verbinding maken met Data Box-zwaar
> * Gegevens kopiëren naar Data Box-zwaar


## <a name="prerequisites"></a>Vereisten

Zorg voordat u begint voor het volgende:

1. U hebt de zelfstudie [ Instellen van Azure Data Box zware](data-box-heavy-deploy-set-up.md).
2. U hebt uw Data Box-zwaar ontvangen en de status van de volgorde in de portal **geleverd**.
3. U bent verbonden met een netwerk met hoge snelheid. Voor de snelste kopie snelheden worden bereikt, kunnen twee 40-GbE-verbindingen (één per knooppunt) gelijktijdig worden gebruikt. Als u geen 40-GbE-verbinding beschikbaar hebt, wordt u aangeraden dat u ten minste twee 10 GbE-verbindingen (één per knooppunt hebt). 
4. U hebt bekeken de:

    - Ondersteund [beheerd schijfgrootten in Azure-object groottelimieten](data-box-heavy-limits.md#azure-object-size-limits).
    - [Inleiding tot Azure beheerde schijven](/azure/virtual-machines/windows/managed-disks-overview). 

## <a name="connect-to-data-box-heavy"></a>Verbinding maken met Data Box-zwaar

Op basis van de resourcegroepen die is opgegeven, maakt gegevens in het zware u een bestandsshare voor elke gekoppelde resource-groep per knooppunt. Bijvoorbeeld, als `mydbmdrg1` en `mydbmdrg2` zijn gemaakt tijdens de bestelling plaatsen, de volgende shares worden gemaakt:

- `mydbmdrg1_MDisk`
- `mydbmdrg2_MDisk`

De volgende drie mappen worden gemaakt die overeenkomen met containers in uw opslagaccount in de share.

- Premium SSD
- Standard HDD
- Standard - SSD

De volgende tabel bevat de UNC-paden naar de shares op uw Data Box-zwaar.
 
|        Verbindingsprotocol           |             UNC-pad naar de share                                               |
|-------------------|--------------------------------------------------------------------------------|
| SMB |`\\<DeviceIPAddress>\<ResourceGroupName_MDisk>\<Premium SSD>\file1.vhd`<br> `\\<DeviceIPAddress>\<ResourceGroupName_MDisk>\<Standard HDD>\file2.vhd`<br> `\\<DeviceIPAddress>\<ResourceGroupName_MDisk>\<Standard SSD>\file3.vhd` |  
| NFS |`//<DeviceIPAddress>/<ResourceGroup1_MDisk>/<Premium SSD>/file1.vhd`<br> `//<DeviceIPAddress>/<ResourceGroupName_MDisk>/<Standard HDD>/file2.vhd`<br> `//<DeviceIPAddress>/<ResourceGroupName_MDisk>/<Standard SSD>/file3.vhd` |

De stappen om verbinding te maken zijn op basis van of u SMB- of NFS gebruiken voor verbinding met gegevens in het zware shares, verschillend.

> [!NOTE]
> - Verbinding maken via de REST wordt niet ondersteund voor deze functie.
> - Herhaal de connect-instructies voor het verbinding maken met het tweede knooppunt van de gegevens in het zware.

### <a name="connect-to-data-box-heavy-via-smb"></a>Verbinding maken met Data Box-zwaar via SMB

Als een computer met Windows Server-host, volgt u deze stappen voor het verbinding maken met de Data Box-zwaar.

1. U moet eerst een verificatie uitvoeren en een sessie starten. Ga naar **Verbinding maken en kopiëren**. Klik op **referenties ophalen** om op te halen van de referenties voor toegang voor de bestandsshares die zijn gekoppeld aan de resourcegroep. U krijgt ook de referenties voor toegang vanuit de **Apparaatdetails** in Azure portal.

    > [!NOTE]
    > De referenties voor alle shares voor beheerde schijven zijn identiek.

    ![Sharereferenties 1 ophalen](media/data-box-deploy-copy-data-from-vhds/get-share-credentials1.png)

2. Van de toegang delen en kopiëren in het dialoogvenster van gegevens, kopiëren de **gebruikersnaam** en de **wachtwoord** voor de share. Klik op **OK**.
    
    ![Sharereferenties 1 ophalen](media/data-box-deploy-copy-data-from-vhds/get-share-credentials2.png)

3. Voor toegang tot de shares die zijn gekoppeld aan uw resource (*mydbmdrg1* in het volgende voorbeeld) vanaf uw hostcomputer, open een opdrachtvenster. Typ in de opdrachtprompt:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    De UNC-share-paden in dit voorbeeld zijn er als volgt uit:

    - `\\169.254.250.200\mydbmdrg1_MDisk`
    - `\\169.254.250.200\mydbmdrg2_MDisk`
    
4. Voer het wachtwoord voor de share in wanneer er om wordt gevraagd. Het volgende voorbeeld laat zien hoe u verbinding met een share kunt maken via de voorgaande opdracht.

    ```
    C:\>net use \\169.254.250.200\mydbmdrgl_MDisk /u:mdisk
    Enter the password for ‘mdisk’ to connect to '169.254.250.200':
    The command completed successfully.
    C: \>
    ```

4. Druk op Windows-toets+R. Geef in het venster **Uitvoeren** het `\\<device IP address>\<ShareName>` op. Klik op **OK** om Verkenner te openen.
    
    ![Verbinding met de share maken via Verkenner 2](media/data-box-deploy-copy-data-from-vhds/connect-shares-file-explorer1.png)

    U ziet nu de volgende mappen in elke share precreated.
    
    ![Verbinding met de share maken via Verkenner 2](media/data-box-deploy-copy-data-from-vhds/connect-shares-file-explorer2.png)


### <a name="connect-to-data-box-heavy-via-nfs"></a>Verbinding maken met Data Box-zwaar via NFS

Als u van een Linux-host-computer gebruikmaakt, moet u de volgende stappen voor het configureren van uw apparaat voor toegang tot de NFS-clients uitvoeren.

1. Geef de IP-adressen op van de clients die toegang hebben tot de share. Ga in de lokale gebruikersinterface naar de pagina **Verbinding maken en kopiëren**. Klik onder **NFS-instellingen** op **NFS-clienttoegang**.

    ![NFS-clienttoegang configureren 1](media/data-box-deploy-copy-data-from-vhds/nfs-client-access1.png)

2. Geef het IP-adres op van de NFS-client en klik op **Toevoegen**. U kunt deze stap herhalen om toegang voor meerdere NFS-clients te configureren. Klik op **OK**.

    ![NFS-clienttoegang configureren 2](media/data-box-deploy-copy-data-from-vhds/nfs-client-access2.png)

2. Zorg dat er een [ondersteunde versie](data-box-system-requirements.md) van de NFS-client op de Linux-hostcomputer is geïnstalleerd. Gebruik de specifieke versie voor uw Linux-distributie.

3. Zodra de NFS-client is geïnstalleerd, gebruikt u de volgende opdracht uit om te koppelen van de NFS-share op uw apparaat:

    `sudo mount <Data Box or Data Box Heavy IP>:/<NFS share on Data Box or Data Box Heavy device> <Path to the folder on local Linux computer>`

    Het volgende voorbeeld ziet hoe u verbinding maakt via NFS naar een Data Box of gegevens in het zware share. Het IP-adres apparaat van Data Box of gegevens in het zware `169.254.250.200`, de share `mydbmdrg1_MDisk` is gekoppeld aan de ubuntuVM, koppelen punt wordt `/home/databoxubuntuhost/databox`.

    `sudo mount -t nfs 169.254.250.200:/mydbmdrg1_MDisk /home/databoxubuntuhost/databox`


## <a name="copy-data-to-data-box-heavy"></a>Gegevens kopiëren naar Data Box-zwaar

Nadat u met de server verbonden bent, wordt de volgende stap is om gegevens te kopiëren. Het VHD-bestand wordt gekopieerd naar het tijdelijke opslagaccount dat als pagina-blob. De pagina-blob is vervolgens geconverteerd naar een beheerde schijf en verplaatst naar een resourcegroep.

Bekijk de volgende punten voordat u begint met het kopiëren van gegevens:

- Kopieer de VHD's altijd op een van de precreated mappen. Als u de VHD's buiten deze mappen of in een map die u hebt gemaakt kopieert, wordt de VHD's worden geüpload naar Azure Storage-account als pagina-blobs en schijven niet worden beheerd.
- De vaste VHD's kunnen worden geüpload voor het maken van beheerde schijven. VHDX-bestanden of dynamische en differentiërende VHD's worden niet ondersteund.
- U kunt slechts één beheerde schijf met een specifieke naam in een resourcegroep hebben in alle precreated mappen. Dit betekent dat de VHD's geüpload naar de precreated mappen moeten een unieke naam hebben. Zorg ervoor dat de opgegeven naam komt niet overeen met een al bestaande beheerde schijf in een resourcegroep.
- Bekijk limieten van de beheerde schijf in [Omvangslimieten voor Azure-object](data-box-heavy-limits.md#azure-object-size-limits).

Afhankelijk van of u verbinding via SMB- of NFS maakt, kunt u het volgende gebruiken:

- [Kopiëren van gegevens via SMB](data-box-heavy-deploy-copy-data.md#copy-data-to-data-box-heavy)
- [Kopiëren van gegevens via NFS](data-box-heavy-deploy-copy-data-via-nfs.md#copy-data-to-data-box-heavy)

Wacht tot de kopieertaken zijn voltooid. Zorg ervoor dat de taken kopiëren zonder fouten zijn voltooid voordat u met de volgende stap verdergaat.

![Geen fouten op de pagina **Verbinding maken en kopiëren**](media/data-box-deploy-copy-data-from-vhds/verify-no-errors-connect-and-copy.png)

Als er fouten tijdens het kopiëren zijn, downloadt u de logboeken van de **verbinding maken en kopiëren** pagina.

- Als u een bestand dat zich niet in 512 bytes uitgelijnd hebt gekopieerd, wordt het bestand is niet geüpload als pagina-blob naar de staging storage-account. Hier ziet u een fout in de logboeken. Verwijder het bestand en kopieer een bestand dat is 512 bytes uitgelijnd.

- Als u een VHDX (deze bestanden worden niet ondersteund) met een lange naam hebt gekopieerd, ziet u een fout in de logboeken.

    ![Fout in de logboeken van ** pagina verbinding maken en kopiëren **](media/data-box-deploy-copy-data-from-vhds/errors-connect-and-copy.png)

    Los de fouten voordat u met de volgende stap doorgaat.

Om de gegevensintegriteit te garanderen wordt de controlesom inline berekend terwijl de gegevens worden gekopieerd. Verifieer de gebruikte ruimte en vrije ruimte op uw apparaat na het kopiëren.
    
![Vrije en ongebruikte ruimte verifiëren op het dashboard](media/data-box-deploy-copy-data-from-vhds/verify-used-space-dashboard.png)

Wanneer de kopieertaak is voltooid, kunt u naar **Voorbereiding voor verzending** gaan.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over Azure Data Box zware onderwerpen, zoals:

> [!div class="checklist"]
> * Lees de voorwaarden
> * Verbinding maken met Data Box-zwaar
> * Gegevens kopiëren naar Data Box-zwaar


Ga naar de volgende zelfstudie voor meer informatie over uw gegevens in het zware naar Microsoft verzenden.

> [!div class="nextstepaction"]
> [Uw Azure Data Box-zwaar naar Microsoft verzenden](./data-box-heavy-deploy-picked-up.md)

