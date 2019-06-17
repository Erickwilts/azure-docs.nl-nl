---
title: Een volume maken voor Azure NetApp Files | Microsoft Docs
description: Dit artikel beschrijft hoe u een volume kunt maken voor Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 6/6/2019
ms.author: b-juche
ms.openlocfilehash: 657bacc153b5721d5a9f34792eaf4796cb477755
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66808877"
---
# <a name="create-a-volume-for-azure-netapp-files"></a>Een volume maken voor Azure NetApp Files

Elke capaciteit van toepassingen kan maximaal 500 volumes hebben. Capaciteitsgebruik van een volume wordt in mindering gebracht op de ingerichte capaciteit van de pool. Azure NetApp Files ondersteunt SMBv3- en NFS-volumes. 

## <a name="before-you-begin"></a>Voordat u begint 
U dient al een capaciteitspool te hebben ingesteld.   
[Een capaciteitspool instellen](azure-netapp-files-set-up-capacity-pool.md)   
Er moet een subnet zijn gedelegeerd aan Azure NetApp Files.  
[Een subnet delegeren aan Azure NetApp Files](azure-netapp-files-delegate-subnet.md)

## <a name="create-an-nfs-volume"></a>Maken van een NFS-volume

1.  Klik op de **Volumes** blade op de blade van de capaciteit van toepassingen. 

    ![Navigeer naar de Volumes](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png)

2.  Klik op **+ Volume toevoegen** om een volume te maken.  
    De maken die een venster van het Volume wordt weergegeven.

3.  Klik in het maken van een venster van het Volume, **maken** en geef informatie op voor de volgende velden:   
    * **Volumenaam**      
        Geef de naam op voor het volume dat u wilt maken.   

        De naam van een volume moet uniek zijn binnen elke capaciteit van toepassingen. De naam moet minstens drie tekens bevatten. U kunt geen alfanumerieke tekens gebruiken.

    * **Capaciteit van toepassingen**  
        Geef de capaciteit van toepassingen waar u het volume moet worden gemaakt.

    * **Quotum**  
        Geef de hoeveelheid logische opslag op die u wilt toewijzen aan het volume.  

        Het veld **Beschikbare quotum** toont hoeveel ongebruikte ruimte er is in de gekozen capaciteitspool, die u kunt gebruiken om een nieuw volume te maken. De grootte van het nieuwe volume mag niet groter zijn dan het beschikbare quotum.  

    * **Virtueel netwerk**  
        Geef het virtuele Azure-netwerk (Vnet) op dat u wilt gebruiken om het volume te benaderen.  

        Het opgegeven VNet moet een subnet bevatten dat is gedelegeerd aan Azure NetApp Files. De Azure NetApp Files-service is alleen toegankelijk vanuit hetzelfde VNet, of vanuit een VNet in dezelfde regio als het volume via VNet-peering. U kunt ook toegang tot het volume van uw on-premises netwerk via Expressroute.   

    * **Subnet**  
        Geef het subnet op dat u wilt gebruiken voor het volume.  
        Het opgegeven subnet moet zijn gedelegeerd aan Azure NetApp Files. 
        
        Als u geen subnet hebt gedelegeerd, kunt u klikken op **Nieuwe maken** op de pagina Een volume maken. Geef vervolgens op de pagina Subnet maken de subnetgegevens op, en selecteer **Microsoft.NetApp/volumes** om het subnet te delegeren aan Azure NetApp Files. In elk Vnet kan slechts één subnet worden overgedragen naar Azure NetApp bestanden.   
 
        ![Een volume maken](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Subnet maken](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

4. Klik op **Protocol**en selecteer vervolgens **NFS** als het protocoltype voor het volume.   
    * Geef de **bestandspad** die wordt gebruikt voor het maken van het pad voor exporteren voor het nieuwe volume. Het exportpad wordt gebruikt om het volume te koppelen en benaderen.

        Het bestandspad mag alleen letters, cijfers en afbreekstreepjes ('-') bevatten. Het bestandspad moet tussen de 16 en 40 tekens lang zijn. 

        Het bestandspad moet uniek zijn binnen elk abonnement en elke regio. 

    * U kunt desgewenst [export-beleid voor het NFS-volume configureren](azure-netapp-files-configure-export-policy.md)

    ![NFS-protocol opgeven](../media/azure-netapp-files/azure-netapp-files-protocol-nfs.png)

5. Klik op **beoordelen en maken** om te controleren van de details van het volume.  Klik vervolgens op **maken** het NFS-volume te maken.

    Het volume dat u hebt gemaakt, wordt weergegeven op de pagina Volumes. 
 
    Een volume neemt het abonnement, de resourcegroep en de locatiekenmerken over van de bijbehorende capaciteitspool. U kunt de implementatiestatus van het volume controleren vanuit het tabblad Meldingen.

## <a name="create-an-smb-volume"></a>Een SMB-volume maken

Azure NetApp-bestanden ondersteunt SMBv3-volumes. U moet Active Directory-verbindingen maken voordat u een SMB-volume toevoegt. 

### <a name="requirements-for-active-directory-connections"></a>Vereisten voor Active Directory-verbindingen

 De vereisten voor Active Directory-verbindingen zijn als volgt: 

* Het beheerdersaccount dat u moet mogelijk te maken van computeraccounts in de organisatie-eenheid (OE)-pad dat u opgeeft.  

* Juiste poorten moeten worden geopend op de toepasselijke Windows Active Directory (AD).  
    De vereiste poorten zijn als volgt: 

    |     Service           |     Poort     |     Protocol     |
    |-----------------------|--------------|------------------|
    |    AD Web Services    |    9389      |    TCP           |
    |    DNS                |    53        |    TCP           |
    |    DNS                |    53        |    UDP           |
    |    ICMPv4             |    N/A       |    Echoantwoord    |
    |    Kerberos           |    464       |    TCP           |
    |    Kerberos           |    464       |    UDP           |
    |    Kerberos           |    88        |    TCP           |
    |    Kerberos           |    88        |    UDP           |
    |    LDAP               |    389       |    TCP           |
    |    LDAP               |    389       |    UDP           |
    |    LDAP               |    3268      |    TCP           |
    |    NetBIOS-naam       |    138       |    UDP           |
    |    SAM/LSA            |    445       |    TCP           |
    |    SAM/LSA            |    445       |    UDP           |
    |    Secure LDAP        |    636       |    TCP           |
    |    Secure LDAP        |    3269      |    TCP           |
    |    W32Time            |    123       |    UDP           |

### <a name="create-an-active-directory-connection"></a>Maak een Active Directory-verbinding

1. Uw account NetApp, klik op **Active Directory-verbindingen**, klikt u vervolgens op **Join**.  

    ![Active Directory Connections](../media/azure-netapp-files/azure-netapp-files-active-directory-connections.png)

2. Geef in het Active Directory Join-venster de volgende informatie:

    * **Primaire DNS**  
        Dit is de DNS-server die is vereist voor de Active Directory-domein en de SMB-bewerkingen voor verificatie. 
    * **Secundaire DNS**   
        Dit is de secundaire DNS-server om ervoor te zorgen redundante naamservices. 
    * **Domein**  
        Dit is de domeinnaam van uw Active Directory Domain Services die u wilt deelnemen.
    * **Het voorvoegsel voor SMB-server (computeraccount)**  
        Dit is het naamvoorvoegsel voor het computeraccount in Active Directory die Azure NetApp bestanden voor het maken van nieuwe accounts wilt gebruiken.

        Bijvoorbeeld, als de naamgevingsnorm die uw organisatie worden gebruikt voor bestandsservers NAS-01,..., NAS-02 voert NAS-045, dan hebt u 'NAS' voor het voorvoegsel. 

        De service maakt extra computeraccounts in Active Directory zo nodig.

    * **Pad van de organisatie-eenheid**  
        Dit is de LDAP-pad voor de organisatie-eenheid (OE) waar de computeraccounts van SMB-server worden gemaakt. Dat wil zeggen, OU = tweede niveau, OU = eerste niveau. 
    * Referenties, inclusief uw **gebruikersnaam** en **wachtwoord**

    ![Join Active Directory](../media/azure-netapp-files/azure-netapp-files-join-active-directory.png)

3. Klik op **Deelnemen**.  

    De Active Directory-verbinding die u hebt gemaakt weergegeven.

    ![Active Directory Connections](../media/azure-netapp-files/azure-netapp-files-active-directory-connections-created.png)

### <a name="add-an-smb-volume"></a>Een SMB-volume toevoegen

1. Klik op de **Volumes** blade op de blade van de capaciteit van toepassingen. 

    ![Navigeer naar de Volumes](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png)

2. Klik op **+ Volume toevoegen** om een volume te maken.  
    De maken die een venster van het Volume wordt weergegeven.

3. Klik in het maken van een venster van het Volume, **maken** en geef informatie op voor de volgende velden:   
    * **Volumenaam**      
        Geef de naam op voor het volume dat u wilt maken.   

        De naam van een volume moet uniek zijn binnen elke capaciteit van toepassingen. De naam moet minstens drie tekens bevatten. U kunt geen alfanumerieke tekens gebruiken.

    * **Capaciteit van toepassingen**  
        Geef de capaciteit van toepassingen waar u het volume moet worden gemaakt.

    * **Quotum**  
        Geef de hoeveelheid logische opslag op die u wilt toewijzen aan het volume.  

        Het veld **Beschikbare quotum** toont hoeveel ongebruikte ruimte er is in de gekozen capaciteitspool, die u kunt gebruiken om een nieuw volume te maken. De grootte van het nieuwe volume mag niet groter zijn dan het beschikbare quotum.  

    * **Virtueel netwerk**  
        Geef het virtuele Azure-netwerk (Vnet) op dat u wilt gebruiken om het volume te benaderen.  

        Het opgegeven VNet moet een subnet bevatten dat is gedelegeerd aan Azure NetApp Files. De Azure NetApp Files-service is alleen toegankelijk vanuit hetzelfde VNet, of vanuit een VNet in dezelfde regio als het volume via VNet-peering. U kunt ook toegang tot het volume van uw on-premises netwerk via Expressroute.   

    * **Subnet**  
        Geef het subnet op dat u wilt gebruiken voor het volume.  
        Het opgegeven subnet moet zijn gedelegeerd aan Azure NetApp Files. 
        
        Als u geen subnet hebt gedelegeerd, kunt u klikken op **Nieuwe maken** op de pagina Een volume maken. Geef vervolgens op de pagina Subnet maken de subnetgegevens op, en selecteer **Microsoft.NetApp/volumes** om het subnet te delegeren aan Azure NetApp Files. In elk Vnet kan slechts één subnet worden overgedragen naar Azure NetApp bestanden.   
 
        ![Een volume maken](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Subnet maken](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

4. Klik op **Protocol** en voltooi de volgende informatie:  
    * Selecteer **SMB** als het protocoltype voor het volume. 
    * Selecteer uw **Active Directory** verbinding van de vervolgkeuzelijst.
    * Geef de naam van het gedeelde volume in **sharenaam**.

    ![SMB-protocol opgeven](../media/azure-netapp-files/azure-netapp-files-protocol-smb.png)

5. Klik op **beoordelen en maken** om te controleren van de details van het volume.  Klik vervolgens op **maken** om de SMB-volume te maken.

    Het volume dat u hebt gemaakt, wordt weergegeven op de pagina Volumes. 
 
    Een volume neemt het abonnement, de resourcegroep en de locatiekenmerken over van de bijbehorende capaciteitspool. U kunt de implementatiestatus van het volume controleren vanuit het tabblad Meldingen.

## <a name="next-steps"></a>Volgende stappen  

* [Koppelen of ontkoppelen van een volume voor Windows of Linux-machines](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Exportbeleid voor een NFS-volume configureren](azure-netapp-files-configure-export-policy.md)
* Meer informatie over [Integratie van virtuele netwerken voor Azure-services](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)
