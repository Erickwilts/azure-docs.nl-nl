---
title: Opzetten van een leslokaallab met Azure Lab Services | Microsoft Docs
description: In deze zelfstudie stelt u een lab in voor gebruik in een leslokaal.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 06/11/2019
ms.author: spelluru
ms.openlocfilehash: 803fe6eff8804dbd407642386865fe975c8db524
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67123274"
---
# <a name="tutorial-set-up-a-classroom-lab"></a>Zelfstudie: Een leslokaallab instellen 
In deze zelfstudie stelt u een leslokaallab in met virtuele machines die worden gebruikt door studenten in het leslokaal.  

In deze zelfstudie voert u de volgende acties uit:

> [!div class="checklist"]
> * Een leslokaallab maken
> * Gebruikers toevoegen aan het lab
> * Registratiekoppeling naar studenten verzenden

## <a name="prerequisites"></a>Vereisten
Als u een leslokaallab in een lab-account instelt, moet u lid zijn van een van deze rollen in het lab-account: Eigenaar, Labmaker of Inzender. Het account dat u hebt gebruikt voor het maken van een labaccount wordt automatisch toegevoegd aan de eigenaarsrol.

Een labeigenaar kan andere gebruikers toevoegen aan de rol **Labmaker**. Zo kan een labeigenaar bijvoorbeeld professoren toevoegen aan de rol Labmaker. Vervolgens maken de professoren labs met VM’s voor hun lessen. Studenten gebruiken de registratiekoppeling die ze ontvangen van professoren om zich te registreren bij het lab. Wanneer ze zijn geregistreerd, kunnen ze VM’s in de labs gebruiken voor het werk in de les en voor hun huiswerk. Zie voor gedetailleerde stappen voor het toevoegen van gebruikers aan de rol Labmaker [Een gebruiker toevoegen aan de rol Labmaker](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).


## <a name="create-a-classroom-lab"></a>Een leslokaallab maken

1. Navigeer naar de [Azure Lab Services-website](https://labs.azure.com). Houd er rekening mee dat Internet Explorer 11 wordt nog niet ondersteund. 
2. Selecteer **Aanmelden** en voer uw referenties in. Azure Lab Services ondersteunt organisatieaccounts en Microsoft-accounts. 
3. Voer in het venster **Nieuw lab** de volgende acties uit: 
    1. Geef een **naam** voor uw lab op. 
    2. Geef het maximale **aantal virtuele machines** in het lab op. U kunt het aantal virtuele machines vergroten of verkleinen na het maken van de testomgeving of in een bestaand lab. Zie voor meer informatie [Aantal virtuele machines in een testomgeving bijwerken](how-to-configure-student-usage.md#update-number-of-virtual-machines-in-lab)
    6. Selecteer **Opslaan**.

        ![Een leslokaallab maken](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. Voer op de pagina **Specificaties van virtuele machines selecteren** de volgende stappen uit:
    1. Selecteer een **grootte** voor virtuele machines (VM's) die in het lab worden gemaakt. Op dit moment **kleine**, **gemiddeld**, **Gemiddeld (virtualisatie)** , **grote**, en **GPU** grootten zijn toegestaan.
    3. Selecteer de **VM-installatiekopie** die moet worden gebruikt voor het maken van virtuele machines in het lab. Als u een installatiekopie van Linux selecteert, ziet u een optie voor het inschakelen van de verbinding met extern bureaublad voor. Zie voor meer informatie, [verbinding met extern bureaublad voor Linux inschakelen](how-to-enable-remote-desktop-linux.md).
    4. Selecteer **Next**.

        ![VM-specificaties opgeven](../media/tutorial-setup-classroom-lab/select-vm-specifications.png)    
5. Geef op de pagina **Referenties instellen** de standaardreferenties voor alle virtuele machines in het lab op. 
    1. Geef de **naam van de gebruiker** op voor alle virtuele machines in het lab.
    2. Geef het **wachtwoord** voor de gebruiker op. 

        > [!IMPORTANT]
        > Noteer de gebruikersnaam en het wachtwoord. Deze worden niet opnieuw weergegeven.
    3. Selecteer **Maken**. 

        ![Referenties instellen](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. Op de pagina **Sjabloon configureren** wordt de status van het maken van het lab weergegeven. Het maken van de sjabloon in het lab duurt maximaal 20 minuten. 

    ![Sjabloon configureren](../media/tutorial-setup-classroom-lab/configure-template.png)
7. Nadat de configuratie van de sjabloon is voltooid, wordt de volgende pagina weergegeven: 

    ![Sjabloonpagina configureren nadat deze is voltooid](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. Voer de volgende stappen uit op de pagina **Sjabloon configureren**: Deze stappen zijn **optioneel** voor de zelfstudie.
    2. De sjabloon-VM verbinden door **Verbinding maken** te selecteren. Als het een basis van Linux virtuele machine, kunt u kiezen of u verbinding maken met SSH of RDP wilt (als RDP is ingeschakeld).
    1. Selecteer **wachtwoord opnieuw instellen** het wachtwoord opnieuw instellen voor de virtuele machine. 
    1. Software installeren en configureren op uw sjabloon-VM. 
    1. **Stop** de virtuele machine.  
    1. Een **Beschrijving** voor de sjabloon invoeren
9. Selecteer **Volgende** op de sjabloonpagina. 
10. Voer op de pagina **De sjabloon publiceren** de volgende acties uit. 
    1. Als u de sjabloon onmiddellijk publiceren, selecteert u **publiceren**.  

        > [!WARNING]
        > Zodra u de sjabloon hebt gepubliceerd, kan dit niet ongedaan worden gemaakt. 
    2. Als u later wilt publiceren, selecteert u **Opslaan voor later**. Nadat de wizard is voltooid, kunt u de VM-sjabloon publiceren. Zie voor meer informatie over het configureren en publiceren nadat de wizard is voltooid, [publiceren van de sjabloon](how-to-create-manage-template.md#publish-the-template-vm) sectie de [leslokaallabs beheren](how-to-manage-classroom-labs.md) artikel.

        ![Sjabloon publiceren](../media/tutorial-setup-classroom-lab/publish-template.png)
11. U kunt de **voortgang van het publiceren** van de sjabloon bekijken. Dit proces duurt maximaal een uur. 

    ![Sjabloon publiceren - voortgang](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. De volgende pagina wordt weergegeven wanneer de sjabloon is gepubliceerd. Selecteer **Done**.

    ![Sjabloon publiceren - gelukt](../media/tutorial-setup-classroom-lab/publish-success.png)
1. U ziet het **dashboard** voor het lab. 
    
    ![Dashboard Leslokaallab](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)
4. Schakel over naar de pagina **Virtuele machines** door Virtuele machines te selecteren in het menu links of door de tegel Virtuele machines te selecteren. Controleer of u virtuele machines ziet met de status **Niet-toegewezen**. Deze virtuele machines zijn nog niet toegewezen aan studenten. Deze horen de status **Gestopt** te hebben. Op deze pagina kunt u een student-VM starten, verbinding maken met de virtuele machine, de virtuele machine stoppen en de virtuele machine verwijderen. U kunt de virtuele machines zelf starten vanaf deze pagina of ze laten starten door de studenten. 

    ![Virtuele machines met de status Gestopt](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

## <a name="add-users-to-the-lab"></a>Gebruikers toevoegen aan het lab

1. Selecteer **Gebruikers** in het menu links. De optie **Toegang beperken** is standaard ingeschakeld. Wanneer deze instelling is ingeschakeld, kan een gebruiker zich niet registreren bij het lab, zelfs niet als deze de registratiekoppeling heeft, tenzij de gebruiker in de lijst met gebruikers staat. Alleen gebruikers in de lijst kunnen zich registreren bij het lab door de registratiekoppeling te gebruiken die u verzendt. In deze procedure kunt u gebruikers toevoegen aan de lijst. U kunt ook **Toegang beperken** uitschakelen, zodat gebruikers zich bij het lab kunnen registreren zolang ze de registratiekoppeling hebben. 
2. Selecteer **Gebruikers toevoegen** op de werkbalk. 

    ![Knop Gebruikers toevoegen](../media/how-to-configure-student-usage/add-users-button.png)
1. Op de pagina **Gebruikers toevoegen** voert u e-mailadressen van gebruikers in op afzonderlijke regels of op één regel gescheiden door puntkomma's. 

    ![E-mailadressen van gebruikers toevoegen](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. Selecteer **Opslaan**. U ziet de e-mailadressen van gebruikers en hun status (al dan niet geregistreerd) in de lijst. 

    ![Lijst met gebruikers](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="set-quotas-for-users"></a>Quota instellen voor gebruikers
U kunt quota's per gebruiker instellen met behulp van de volgende stappen uit: 

1. Selecteer **gebruikers** in het menu links, als de pagina al actief is. 
2. Selecteer **quotum per gebruiker:** op de werkbalk. 
3. Op de **quotum per gebruiker** pagina, geef het aantal uren dat u wilt verlenen aan elke gebruiker (studenten): 
    1. **0 uur (alleen schedule)** . Alleen tijdens het geplande tijdstip, of wanneer u als de eigenaar van het lab voor deze virtuele machines inschakelt, kunnen gebruikers hun VM's gebruiken.

        ![Nul uur - alleen geplande tijd](../media/how-to-configure-student-usage/zero-hours.png)
    1. **Totale aantal uren per gebruiker lab**. Gebruikers kunnen hun VM's gebruiken voor het aantal uren (opgegeven voor dit veld) **naast het geplande tijdstip**. Als u deze optie selecteert, voert u de **aantal uren** in het tekstvak in. 

        ![Aantal uren per gebruiker](../media/how-to-configure-student-usage/number-of-hours-per-user.png)
    4. Selecteer **Opslaan**. 
5. Ziet u de gewijzigde waarden op de werkbalk nu: **Quotum per gebruiker: &lt;aantal uren&gt;** . 

    ![Quotum per gebruiker](../media/how-to-configure-student-usage/quota-per-user.png)

## <a name="set-a-schedule-for-the-lab"></a>Een schema voor de testomgeving instellen
Als u de quota-instelling geconfigureerd **0 uur (alleen schedule)** , moet u een schema voor de testomgeving instellen. In deze zelfstudie stelt u de planning om te worden van een terugkerende wekelijkse planning.

1. Schakel over naar de **planningen** pagina en selecteer **toevoegen planning** op de werkbalk. 

    ![Schema-knop op de pagina schema toevoegen](../media/how-to-create-schedules/add-schedule-button.png)
2. Op de **toevoegen planning** pagina, gaat u naar **wekelijkse** aan de bovenkant. 
3. Voor **plannen dagen (vereist)** , selecteer de dagen waarop de planning van kracht. In het volgende voorbeeld, is maandag tot vrijdag geselecteerd. 
4. Voor de **van** en voer de **plannen begindatum** of kies een datum selecteert de **agenda** knop. Dit veld is vereist. 
5. Voor **planning einddatum**Typ of Selecteer een einddatum waarop de virtuele machines moeten worden afgesloten. 
6. Voor **begintijd**, selecteert u het tijdstip waarop u wilt dat de virtuele machines worden gestart. De begintijd is vereist als de tijd voor het stoppen is niet ingesteld. Selecteer **verwijderen start gebeurtenis** als u wilt om op te geven alleen de tijd voor het stoppen. Als de **begintijd** is uitgeschakeld, selecteer **toevoegen begingebeurtenis** naast de vervolgkeuzelijst zodat het. 
7. Voor **stoptijd**, selecteert u het tijdstip waarop u wilt dat de virtuele machines worden afgesloten. De tijd voor het stoppen is vereist als de begintijd is niet ingesteld. Selecteer **stop-gebeurtenis Remove** als u wilt opgeven voor alleen de begintijd. Als de **stoptijd** is uitgeschakeld, selecteer **stop-gebeurtenis toevoegen** naast de vervolgkeuzelijst zodat het.
8. Voor **tijdzone (vereist)** , selecteert u de tijdzone voor het starten en stoppen keren dat u hebt opgegeven.  
9. Voor **opmerkingen bij de**, geen beschrijving of notities invoeren voor de planning. 
10. Selecteer **Opslaan**. 

    ![Wekelijks schema](../media/how-to-create-schedules/add-schedule-page-weekly.png)

## <a name="send-an-email-with-the-registration-link"></a>Stuur een e-mail met de registratiekoppeling

1. Schakel over naar de weergave **Gebruikers** als u nog niet op die pagina bent. 
2. Selecteer specifieke of alle gebruikers in de lijst. Selecteer specifieke gebruikers, schakel selectievakjes in de eerste kolom van de lijst. Selecteer alle gebruikers, schakel het selectievakje voor de titel van de eerste kolom (**naam**) of selecteert u alle selectievakjes uit voor alle gebruikers in de lijst. U ziet de status van de **uitnodiging status** in deze lijst.  In de volgende afbeelding, de status van de uitnodiging voor alle studenten is ingesteld op **uitnodiging niet verzonden**. 

    ![Studenten selecteren](../media/tutorial-setup-classroom-lab/select-students.png)
1. Selecteer de **e-mailpictogram (envelop)** in een van de rijen (of) select **uitnodiging verzenden** op de werkbalk. U kunt ook de muisaanwijzer boven de Studentnaam van een in de lijst om te zien van het e-mailpictogram houdt. 

    ![Registratiekoppeling per e-mail verzenden](../media/tutorial-setup-classroom-lab/send-email.png)
4. Op de **registratiekoppeling verzenden via e-mail** pagina, als volgt te werk: 
    1. Typ een **optioneel bericht** die u wilt verzenden naar de studenten. Het e-mailbericht omvat automatisch de registratiekoppeling. 
    2. Op de **registratiekoppeling verzenden via e-mail** weergeeft, schakelt **verzenden**. U ziet de status van de uitnodiging te wijzigen in **uitnodiging verzenden** en van daaruit naar **uitnodiging verzonden**. 
        
        ![Uitnodiging verzonden](../media/tutorial-setup-classroom-lab/invitations-sent.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u een leslokaallab gemaakt en het lab geconfigureerd. Ga voor meer informatie over hoe een student toegang kan krijgen tot een virtuele machine in het lab met behulp van de registratiekoppeling naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Verbinding maken met een virtuele machine in het leslokaallab](tutorial-connect-virtual-machine-classroom-lab.md)

