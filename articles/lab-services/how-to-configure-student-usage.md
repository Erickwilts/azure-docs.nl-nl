---
title: Gebruiks instellingen configureren in de klassikale Labs van Azure Lab Services
description: Meer informatie over het configureren van het aantal studenten voor een lab, het ontvangen van ze met het lab, het aantal uren dat ze de virtuele machine mogen gebruiken en meer.
ms.topic: article
ms.date: 11/11/2020
ms.openlocfilehash: d3100f1a7e67e3b0d403375de02cb3daf5fcfb31
ms.sourcegitcommit: dc342bef86e822358efe2d363958f6075bcfc22a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2020
ms.locfileid: "94555716"
---
# <a name="add-and-manage-lab-users"></a>Labgebruikers toevoegen en beheren

In dit artikel wordt beschreven hoe u gebruikers van studenten toevoegt aan een lab, ze registreert bij het lab, hoe u het aantal extra uren voor de virtuele machine (VM) kunt beheren. 

## <a name="add-users-to-a-lab"></a>Gebruikers toevoegen aan een Lab

In deze sectie voegt u studenten hand matig toe aan een Lab of door een CSV-bestand te uploaden. Ga als volgt te werk:

1. Selecteer in het linkerdeel venster de optie **gebruikers**. 

    De optie **toegang beperken** is standaard ingeschakeld en, tenzij ze in de lijst met gebruikers staan, kunnen studenten zich niet bij het lab registreren, zelfs niet als ze een registratie koppeling hebben. Alleen gebruikers die worden weer gegeven, kunnen zich bij het lab registreren met de registratie koppeling die u verzendt. In deze procedure kunt u gebruikers toevoegen aan de lijst. U kunt de **toegang ook beperken** uitschakelen, zodat studenten zich bij het lab kunnen registreren zolang ze de registratie koppeling hebben. 

1. Selecteer aan de bovenkant van het deel venster **gebruikers** de optie **gebruikers toevoegen** en selecteer vervolgens **toevoegen per e-mail adres**. 

    ![De knop gebruikers toevoegen](./media/how-to-configure-student-usage/add-users-button.png)

1. Voer in het deel venster **gebruikers toevoegen** de e-mail adressen van studenten in op afzonderlijke regels of op één regel, gescheiden door punt komma's. 

    ![E-mail adressen van gebruikers toevoegen](./media/how-to-configure-student-usage/add-users-email-addresses.png)

1. Selecteer **Opslaan**. 

    In de lijst worden de e-mail adressen en statussen van de huidige gebruikers weer gegeven, ongeacht of ze zijn geregistreerd bij het lab of niet. 

    ![Lijst met gebruikers](./media/how-to-configure-student-usage/list-of-added-users.png)

    > [!NOTE]
    > Nadat de studenten zijn geregistreerd bij het lab, worden de namen weer gegeven in de lijst. De naam die wordt weer gegeven in de lijst, wordt samengesteld op basis van de voor-en achternaam van de studenten in Azure Active Directory. 

### <a name="add-users-by-uploading-a-csv-file"></a>Gebruikers toevoegen door een CSV-bestand te uploaden

U kunt ook gebruikers toevoegen door een CSV-bestand te uploaden dat hun e-mail adressen bevat. 

Een CSV-tekst bestand wordt gebruikt voor het opslaan van gegevens in tabel vorm (getallen en tekst) met komma's gescheiden (CSV). In plaats van informatie op te slaan in kolom velden (zoals in werk bladen), slaat een CSV-bestand gegevens op, gescheiden door komma's. Elke regel in een CSV-bestand heeft hetzelfde aantal velden met door komma's gescheiden waarden. " U kunt Excel gebruiken om eenvoudig CSV-bestanden te maken en te bewerken.

1. Maak in micro soft Excel een CSV-bestand met een lijst met e-mail adressen van studenten in één kolom.

    ![Lijst met gebruikers in een CSV-bestand](./media/how-to-configure-student-usage/csv-file-with-users.png)

1. Selecteer aan de bovenkant van het deel venster **gebruikers** de optie **gebruikers toevoegen** en selecteer vervolgens **CSV uploaden**.

    ![De knop voor het uploaden van CSV](./media/how-to-configure-student-usage/upload-csv-button.png)

1. Selecteer het CSV-bestand dat de e-mail adressen van studenten bevat en selecteer vervolgens **openen**.

    Het venster **gebruikers toevoegen** bevat de lijst met e-mail adressen uit het CSV-bestand. 

    ![Het venster gebruikers toevoegen met e-mail adressen uit een CSV-bestand](./media/how-to-configure-student-usage/add-users-window.png)

1. Selecteer **Opslaan**. 

1. In het deel venster **gebruikers** bekijkt u de lijst met toegevoegde studenten. 

    ![Lijst met toegevoegde gebruikers in het deel venster gebruikers](./media/how-to-configure-student-usage/list-of-added-users.png)

## <a name="send-invitations-to-users"></a>Uitnodigingen voor gebruikers verzenden

Gebruik een van de volgende methoden om een registratie koppeling naar nieuwe gebruikers te verzenden. 

Als de optie **toegang beperken** voor het lab is ingeschakeld, kunnen alleen gebruikers die worden weer gegeven de registratie koppeling gebruiken om zich bij het lab te registreren. Deze optie is standaard ingeschakeld. 

### <a name="invite-all-users"></a>Alle gebruikers uitnodigen

Met deze methode wordt uitgelegd hoe u een e-mail met een registratie koppeling en een optioneel bericht naar alle vermelde studenten verzendt.

1. Selecteer in **Users** het deel venster gebruikers **alle uitnodigingen**. 

    ![De knop Alles uitnodigen](./media/tutorial-setup-classroom-lab/invite-all-button.png)

1. Voer in het venster **uitnodiging per E-mail verzenden** een optioneel bericht in en selecteer vervolgens **verzenden**. 

    Het e-mailbericht bevat automatisch een registratiekoppeling. Als u de registratie koppeling afzonderlijk wilt ophalen en opslaan, selecteert u het weglatings teken ( **...** ) aan de bovenkant van het deel venster **gebruikers** en selecteert u vervolgens **registratie koppeling**. 

    ![Het venster registratie koppeling per e-mail verzenden](./media/tutorial-setup-classroom-lab/send-email.png)

    In de kolom **uitnodigingen** van de lijst **gebruikers** wordt de status van de uitnodiging voor elke toegevoegde gebruiker weer gegeven. De status moet worden gewijzigd om te **verzenden** en vervolgens naar **\<date> verzonden**. 

### <a name="invite-selected-users"></a>Geselecteerde gebruikers uitnodigen

Deze methode laat zien hoe u alleen bepaalde studenten uitnodigt en een registratie koppeling krijgt die u met anderen kunt delen.

1. Selecteer in het deel venster **gebruikers** een student of meerdere studenten in de lijst. 

1. Selecteer in de rij voor de student die u hebt geselecteerd het pictogram **envelop** of Selecteer op de werk balk de optie **uitnodigen**. 

    ![Geselecteerde gebruikers uitnodigen](./media/how-to-configure-student-usage/invite-selected-users.png)

1. Voer in het venster **uitnodiging per E-mail verzenden** een optioneel **bericht** in en selecteer vervolgens **verzenden**. 

    ![E-mail naar geselecteerde gebruikers verzenden](./media/how-to-configure-student-usage/send-invitation-to-selected-users.png)

    In het deel venster **gebruikers** wordt de status van deze bewerking weer gegeven in de kolom **uitnodigingen** van de tabel. De uitnodigings-e-mail bevat de registratie koppeling die studenten kunnen gebruiken om zich bij het lab te registreren.

## <a name="get-the-registration-link"></a>De registratie koppeling ophalen

In deze sectie kunt u de registratie koppeling vanuit de portal ophalen en verzenden met uw eigen e-mail toepassing. 

1. Selecteer **registratie koppeling** in het deel venster **gebruikers** .

    ![Registratiekoppeling voor studenten](./media/how-to-configure-student-usage/registration-link-button.png)

1. Selecteer in het venster **gebruikers registratie** de optie **kopiëren** en selecteer vervolgens **gereed**. 

    ![Het venster gebruikers registratie](./media/how-to-configure-student-usage/registration-link.png)

    De koppeling wordt naar het klembord gekopieerd. 
    
1. Plak de registratie koppeling in uw e-mail toepassing en verzend vervolgens het e-mail bericht naar een student zodat de student zich kan registreren voor de klasse. 

## <a name="view-registered-users"></a>Geregistreerde gebruikers weergeven

1. Ga naar de website van [Azure Lab Services](https://labs.azure.com) . 
1. Selecteer **Aanmelden** en voer uw referenties in. Azure Lab Services ondersteunt organisatieaccounts en Microsoft-accounts.
1. Selecteer op de pagina **mijn Labs** het lab waarvan u het gebruik wilt bijhouden. 
1. Selecteer in het linkerdeel venster de optie **gebruikers** of selecteer de tegel **gebruikers** . 

    In het deel venster **gebruikers** wordt een lijst weer gegeven met studenten die zijn geregistreerd bij uw Lab.  

    ![Lijst met geregistreerde gebruikers](./media/tutorial-track-usage/registered-users.png)

## <a name="set-quotas-for-users"></a>Quota instellen voor gebruikers

U kunt een uur quotum voor elke student instellen door het volgende te doen: 

1. Selecteer in **Users** het deel venster gebruikers **quotum per gebruiker: \<number> uur** op de werk balk. 
1. Geef in het venster **quotum per gebruiker** het aantal uren op dat u wilt toewijzen aan elke student buiten de geplande klasse tijd en selecteer vervolgens **Opslaan**.

    ![Het venster quotum per gebruiker](./media/how-to-configure-student-usage/quota-per-user.png)    

    De gewijzigde waarden worden nu weer gegeven op de knop **quotum per \<number of hours> gebruiker:** op de werk balk en in de lijst met gebruikers, zoals hier wordt weer gegeven:

    ![Quotum uren per gebruiker](./media/how-to-configure-student-usage/quot-per-user-after.png)

    > [!IMPORTANT]
    > De [geplande uitvoerings tijd van vm's](how-to-create-schedules.md) telt niet af op het quotum dat is toegewezen aan een student. Het quotum geldt voor de tijd buiten de geplande uren die een student op Vm's doorbrengt. 

## <a name="set-additional-quotas-for-specific-users"></a>Aanvullende quota's instellen voor specifieke gebruikers

U kunt quota opgeven voor bepaalde studenten dan de gemeen schappelijke quota die zijn ingesteld voor alle gebruikers in de voor gaande sectie. Als u bijvoorbeeld als docent het quotum voor alle studenten instelt op 10 uur en een extra quotum van 5 uur instelt voor een specifieke student, krijgt deze student 15 (10 + 5) uur aan quota. Als u het algemene quotum later wijzigt in, zegt 15, de student 20 (15 + 5) uur aan quota krijgt. Houd er rekening mee dat dit totale quotum zich buiten het geplande tijdstip bevindt. De tijd die een student tijdens de geplande tijd op een Lab-VM doorbrengt, telt niet op dit quotum. 

Ga als volgt te werk om aanvullende quota's in te stellen:

1. Selecteer in het deel venster **gebruikers** een student uit de lijst en selecteer vervolgens **quota aanpassen** op de werk balk. 

    ![De knop quotum aanpassen](./media/how-to-configure-student-usage/adjust-quota-button.png)

1. Voer in het deel **quotum \<selected user or users email address> aanpassen voor** het aantal extra Lab-uren in dat u wilt verlenen aan de geselecteerde student of studenten, en selecteer vervolgens **Toep assen**. 

    ![De "quota aanpassen..." openen](./media/how-to-configure-student-usage/additional-quota.png)

    In de kolom **gebruik** wordt het bijgewerkte quotum voor de geselecteerde studenten weer gegeven. 

    ![Nieuw gebruik voor de gebruiker](./media/how-to-configure-student-usage/new-usage-hours.png)

## <a name="student-accounts"></a>Studenten accounts

Als u studenten wilt toevoegen aan een leslokaal Lab, gebruikt u hun e-mail accounts. Studenten hebben mogelijk de volgende typen e-mail accounts:

- Een e-mail account voor studenten dat wordt weer gegeven door de Azure Active Directory instantie van uw universiteit.
- Een micro soft-domein e-mail account, zoals *Outlook.com* , *Hotmail.com* , *MSN.com* of *Live.com*.
- Een niet-micro soft-e-mail account, zoals een van Yahoo! of Google. Deze typen accounts moeten echter worden gekoppeld aan een Microsoft-account.
- Een GitHub-account. Dit account moet worden gekoppeld aan een Microsoft-account.

### <a name="use-a-non-microsoft-email-account"></a>Een niet-micro soft-e-mail account gebruiken

Studenten kunnen niet-micro soft-e-mail accounts gebruiken om zich te registreren bij een leslokaal Lab en zich aan te melden.  De registratie vereist echter dat ze eerst een Microsoft-account maken dat is gekoppeld aan een niet-micro soft-e-mail adres.

Veel studenten hebben mogelijk al een Microsoft-account die is gekoppeld aan een niet-micro soft-e-mail adres. Studenten hebben bijvoorbeeld al een Microsoft-account als ze hun e-mail adres hebben gebruikt met andere micro soft-producten of-services, zoals Office, Skype, OneDrive of Windows.  

Wanneer studenten de registratie koppeling gebruiken om zich aan te melden bij een leslokaal, wordt de gebruiker gevraagd om hun e-mail adres en wacht woord. Studenten die zich proberen aan te melden met een niet-Microsoft-account die niet is gekoppeld aan een Microsoft-account, ontvangen het volgende fout bericht: 

![Fout bericht bij het aanmelden](./media/how-to-configure-student-usage/cant-find-account.png)

Hier volgt een koppeling waarmee studenten [zich kunnen registreren voor een Microsoft-account](http://signup.live.com).  

> [!IMPORTANT]
> Wanneer studenten zich aanmelden bij een leslokaal Lab, krijgen ze niet de optie om een Microsoft-account te maken. Daarom wordt u aangeraden deze aanmeldings koppeling op te nemen http://signup.live.com in de inschrijvings-e-mail van het leslokaal dat u verzendt naar studenten die niet-micro soft-accounts gebruiken.

### <a name="use-a-github-account"></a>Een GitHub-account gebruiken

Studenten kunnen ook een bestaand GitHub-account gebruiken om zich te registreren bij een leslokaal Lab en zich aan te melden. Als er al een Microsoft-account is gekoppeld aan hun GitHub-account, kunnen studenten zich aanmelden en hun wacht woord opgeven, zoals wordt weer gegeven in de voor gaande sectie. 

Als ze hun GitHub-account nog niet aan een Microsoft-account hebben gekoppeld, kunnen ze het volgende doen:

1. Selecteer de koppeling **aanmeldings opties** , zoals hier wordt weer gegeven:

    ![De koppeling aanmeldings opties](./media/how-to-configure-student-usage/signin-options.png)

1. Selecteer **Aanmelden met github** in het venster **Opties voor aanmelden** .

    ![De koppeling ' aanmelden bij GitHub '](./media/how-to-configure-student-usage/signin-github.png)

    Na de prompt maken studenten een Microsoft-account dat is gekoppeld aan hun GitHub-account. De koppeling wordt automatisch uitgevoerd wanneer u **volgende** selecteert. Ze zijn vervolgens direct aangemeld en zijn verbonden met het leslokaal Lab.

## <a name="export-a-list-of-users-to-a-csv-file"></a>Een lijst met gebruikers exporteren naar een CSV-bestand

1. Ga naar het deel venster **gebruikers** .
1. Selecteer op de werk balk het beletsel teken ( **...** ) en selecteer vervolgens **CSV exporteren**. 

    ![De knop CSV exporteren](./media/how-to-export-users-virtual-machines-csv/users-export-csv.png)


## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen:

- Voor beheerders: [Lab-accounts maken en beheren](how-to-manage-lab-accounts.md)
- Voor Lab-eigen aars: [maken en beheren van Labs](how-to-manage-classroom-labs.md) en [sjablonen instellen en publiceren](how-to-create-manage-template.md)
- Voor Lab-gebruikers: [toegang tot klassikale Labs](how-to-use-classroom-lab.md)
