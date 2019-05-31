---
title: Problemen met Azure Backup Server oplossen
description: Problemen oplossen-installatie, de registratie van Azure Backup-Server, en back-up en herstel van werkbelastingen van toepassingen.
services: backup
author: kasinh
manager: vvithal
ms.service: backup
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: kasinh
ms.openlocfilehash: 06faed8ceca77edc20b67f73a76d885839aa7dbc
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66304325"
---
# <a name="troubleshoot-azure-backup-server"></a>Problemen met Azure Backup Server oplossen

Gebruik de informatie in de volgende tabellen om het oplossen van fouten die optreden tijdens het gebruik van Azure Backup Server.

## <a name="basic-troubleshooting"></a>Eenvoudige probleemoplossing

Is het beter om u de onderstaande validatie, voordat u begint het oplossen van Microsoft Azure Backup-Server (MABS):

- [Zorg ervoor dat Microsoft Azure Recovery Services Agent (MARS) is bijgewerkt](https://go.microsoft.com/fwlink/?linkid=229525&clcid=0x409)
- [Zorg ervoor dat er netwerkverbinding is tussen de MARS-agent en Azure](https://aka.ms/AB-A4dp50)
- Controleer of Microsoft Azure Recovery Services wordt uitgevoerd (in Service-console). Indien nodig opnieuw en probeer het opnieuw
- [Zorg ervoor dat er 5-10% ruimte vrij is op de locatie van de scratch-map](https://aka.ms/AB-AA4dwtt)
- Als de registratie is mislukt, controleert vervolgens de server waarop u probeert te installeren van Azure Backup Server is niet geregistreerd bij een andere kluis
- Als een push-installatie mislukt, controleert u of de DPM-agent al aanwezig is. Zo ja, verwijdert u de agent en start u de installatie opnieuw
- [Controleer of er geen ander proces of antivirussoftware conflicten veroorzaakt met Azure Backup](https://aka.ms/AA4nyr4)<br>
- Zorg ervoor dat de SQL Agent-service wordt uitgevoerd en in de MAB-server is ingesteld op automatisch<br>


## <a name="invalid-vault-credentials-provided"></a>Ongeldige kluisreferenties ingevoerd

| Bewerking | Foutdetails | Tijdelijke oplossing |
| --- | --- | --- |
| Registreren bij een kluis | Er zijn ongeldige kluisreferenties opgegeven. Het bestand is beschadigd of heeft niet zijn de meest recente referenties die zijn gekoppeld aan de recovery-service. | Aanbevolen actie: <br> <ul><li> Download het meest recente referentiebestand uit de kluis en probeer het opnieuw. <br>(OR)</li> <li> Als de vorige bewerking is niet werkt, probeer het downloaden van de referenties naar een andere lokale map of een nieuwe kluis maken. <br>(OR)</li> <li> Probeer het bijwerken van de datum- en tijdinstellingen zoals beschreven in [deze blog](https://azure.microsoft.com/blog/troubleshooting-common-configuration-issues-with-azure-backup/). <br>(OR)</li> <li> Controleer of c:\windows\temp meer dan 65000 zijn bestanden heeft. Verouderde bestanden verplaatsen naar een andere locatie of verwijder de items in de map Temp. <br>(OR)</li> <li> Controleer de status van certificaten. <br> a. Open **computercertificaten beheren** (in het Configuratiescherm). <br> b. Vouw de **persoonlijke** en de onderliggende knooppunten **certificaten**.<br> c.  Verwijder het certificaat **Windows Azure-hulpprogramma's**. <br> d. Probeer de registratie in de Azure Backup-client. <br> (OR) </li> <li> Controleer als een groepsbeleid ingesteld is. </li></ul> |

## <a name="replica-is-inconsistent"></a>De replica is inconsistent

| Bewerking | Foutdetails | Tijdelijke oplossing |
| --- | --- | --- |
| Backup | De replica is inconsistent | Controleer of de optie Automatische consistentiecontrole selectievakje in de Wizard beveiligingsgroep is ingeschakeld. Zie het artikel voor meer informatie over de oorzaken van replica inconsistentie en relevante suggesties [de Replica is inconsistent](https://technet.microsoft.com/library/cc161593.aspx).<br> <ol><li> Een back-up van systeemstatus/BMR, Controleer of Windows Server back-up is geïnstalleerd op de beveiligde server.</li><li> Controleren op problemen met betrekking tot ruimte in de DPM-opslaggroep op de DPM/Microsoft Azure Backup Server en opslag waar nodig toewijzen.</li><li> Controleer de status van de Volume Shadow Copy-Service op de beveiligde server. Als deze uitgeschakeld is, stelt u deze handmatig te starten. Start de service op de server. Vervolgens gaat u terug naar de console DPM/Microsoft Azure Backup Server en start u de synchronisatie met de consistentiecontrole.</li></ol>|

## <a name="online-recovery-point-creation-failed"></a>Online herstelpunt is mislukt

| Bewerking | Foutdetails | Tijdelijke oplossing |
| --- | --- | --- |
| Backup | Online herstelpunt is mislukt | **Foutbericht**: Windows Azure Backup-Agent is niet te maken van een momentopname van het geselecteerde volume. <br> **Tijdelijke oplossing**: Verhoog de ruimte op replica en herstelpuntvolume.<br> <br> **Foutbericht**: De Windows Azure Backup Agent kan geen verbinding maken met de OBEngine-service <br> **Tijdelijke oplossing**: Controleer of de OBEngine bestaat in de lijst met services op de computer. Als de OBEngine-service is niet uitgevoerd, gebruikt u de opdracht 'net start OBEngine' om de OBEngine-service te starten. <br> <br> **Foutbericht**: De versleutelingswachtwoordzin voor deze server is niet ingesteld. Configureer een wachtwoordzin voor versleuteling. <br> **Tijdelijke oplossing**: Probeer een wachtwoordzin voor versleuteling configureren. Als dit mislukt, voert u de volgende stappen uit: <br> <ol><li>Controleer of de nieuwe locatie bestaat. Dit is de locatie die wordt vermeld in het register **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Azure Backup\Config**, met de naam van de **ScratchLocation** moet bestaan.</li><li> Als de nieuwe locatie bestaat, probeert u opnieuw te registreren met behulp van de oude wachtwoordzin. *Wanneer u een wachtwoordzin voor versleuteling configureert, kunt u deze op een veilige locatie opslaan.*</li><ol>|

## <a name="the-vault-credentials-provided-are-different-from-the-vault-the-server-is-registered"></a>De kluisreferenties verschillen van de kluis die de server is geregistreerd

| Bewerking | Foutdetails | Tijdelijke oplossing |
| --- | --- | --- |
| Herstellen | **Foutcode**: Fout CBPServerRegisteredVaultDontMatchWithCurrent/kluis: 100110 <br/> <br/>**Foutbericht**: De kluisreferenties verschillen van de kluis die de server is geregistreerd | **Oorzaak**: Dit probleem treedt op wanneer u probeert om bestanden te herstellen op een andere server van de oorspronkelijke server met behulp van de hersteloptie externe DPM en als de server die wordt hersteld en de oorspronkelijke server vanaf waar de gegevens een back-up is niet gekoppeld aan dezelfde Recovery Services-kluis.<br/> <br/>**Tijdelijke oplossing** om op te lossen dit probleem Zorg ervoor dat zowel de oorspronkelijke en alternatieve server is geregistreerd bij dezelfde kluis.|

## <a name="online-recovery-point-creation-jobs-for-vmware-vm-fail"></a>Taken voor het maken van online herstelpunten voor VMware-VM is mislukt

| Bewerking | Foutdetails | Tijdelijke oplossing |
| --- | --- | --- |
| Backup | Taken voor het maken van online herstelpunten voor VMware-VM is mislukt. Er is een fout van VMware in DPM opgetreden bij het ophalen van gegevens voor wijzigingen bijhouden. Foutcode: FileFaultFault (ID 33621) |  <ol><li> Voor de betrokken virtuele machines opnieuw in de CTK op VMware.</li> <li>Controleer dat onafhankelijke schijf is niet aanwezig op VMware.</li> <li>Stop de beveiliging voor de betrokken virtuele machines en opnieuw te beveiligen met de **vernieuwen** knop. </li><li>Voer een CC voor de betrokken virtuele machines.</li></ol>|


## <a name="the-agent-operation-failed-because-of-a-communication-error-with-the-dpm-agent-coordinator-service-on-the-server"></a>De agentbewerking is mislukt vanwege een communicatiefout met de service DPM agent coordinator op de server

| Bewerking | Foutdetails | Tijdelijke oplossing |
| --- | --- | --- |
| Agent (s) pushen naar beveiligde servers | De agentbewerking is mislukt vanwege een communicatiefout met de service DPM Agent Coordinator op \<ServerName >. | **Voer de volgende stappen uit als de aanbevolen actie wordt weergegeven in het product niet werkt,** : <ul><li> Als u een computer uit een niet-vertrouwd domein toevoegen wilt, voert u de [stappen](https://technet.microsoft.com/library/hh757801(v=sc.12).aspx). <br> (OR) </li><li> Als u een computer van een vertrouwd domein toevoegen wilt, problemen oplossen met behulp van de stappen in [deze blog](https://blogs.technet.microsoft.com/dpm/2012/02/06/data-protection-manager-agent-network-troubleshooting/). <br>(OR)</li><li> Probeer het uitschakelen van antivirusprogramma het oplossen van problemen. Als deze het probleem wordt opgelost, wijzigt u de antivirus-instellingen zoals aangegeven in [in dit artikel](https://technet.microsoft.com/library/hh757911.aspx).</li></ul> |

## <a name="setup-could-not-update-registry-metadata"></a>Setup kan de metagegevens van het register niet bijwerken

| Bewerking | Foutdetails | Tijdelijke oplossing |
|-----------|---------------|------------|
|Installatie | Setup kan de metagegevens van het register niet bijwerken. Deze update kan leiden tot overusage opslagverbruik van. Om te voorkomen dat de registervermelding ReFS Trimming bijgewerkt. | De registersleutel aanpassen **SYSTEM\CurrentControlSet\Control\FileSystem\RefsEnableInlineTrim**. Stel de Dword-waarde in op 1. |
|Installatie | Setup kan de metagegevens van het register niet bijwerken. Deze update kan leiden tot overusage opslagverbruik van. Om dit te voorkomen, werken de registervermelding Volume snapoptimization bij te werken. | Maak de registersleutel **SOFTWARE\Microsoft Data Protection Manager\Configuration\VolSnapOptimization\WriteIds** met een lege tekenreeks-waarde. |

## <a name="registration-and-agent-related-issues"></a>Registratie en -agents oplossen

| Bewerking | Foutdetails | Tijdelijke oplossing |
| --- | --- | --- |
| Agent (s) pushen naar beveiligde servers | De referenties die zijn opgegeven voor de server zijn ongeldig. | **Als de aanbevolen actie die wordt weergegeven in het product niet werkt, voert u de volgende stappen uit**: <br> Probeer de beveiligingsagent handmatig installeren op de productieserver zoals opgegeven in [in dit artikel](https://technet.microsoft.com/library/hh758186(v=sc.12).aspx#BKMK_Manual).|
| Azure Backup-Agent is geen verbinding maken met de Azure Backup-service (ID: 100050) | De Azure Backup-Agent is geen verbinding maken met de Azure Backup-service. | **Als de aanbevolen actie die wordt weergegeven in het product niet werkt, voert u de volgende stappen uit**: <br>1. Voer de volgende opdracht uit vanaf een opdrachtprompt: **psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe**. Hiermee opent u het venster van Internet Explorer. <br/> 2. Ga naar **extra** > **Internetopties** > **verbindingen** > **LAN-instellingen**. <br/> 3. Wijzig de instellingen voor het gebruik van een proxyserver. Geeft u de proxy-server-gegevens.<br/> 4. Als uw computer toegang tot internet beperkt heeft, zorgt u ervoor dat de firewall-instellingen op de machine of de proxy dit toestaan [URL's](backup-configure-vault.md#verify-internet-access) en [IP-adres](backup-configure-vault.md#verify-internet-access).|
| Azure Backup-Agent-installatie is mislukt | De Microsoft Azure Recovery Services-installatie is mislukt. Alle wijzigingen die door de Microsoft Azure Recovery Services-installatie zijn aangebracht in het systeem worden teruggedraaid. (ID: 4024) | Azure-Agent handmatig installeren.


## <a name="configuring-protection-group"></a>Beveiligingsgroep configureren

| Bewerking | Foutdetails | Tijdelijke oplossing |
| --- | --- | --- |
| Beveiligingsgroepen configureren | DPM kan niet opsommen voor het toepassingsonderdeel op de beveiligde computer (beveiligde computernaam). | Selecteer **vernieuwen** op het scherm configureren beveiliging groep UI op het niveau van de relevante datasource-component. |
| Beveiligingsgroepen configureren | Kan geen beveiliging configureren | Als de beveiligde server een SQL-server is, moet u controleren dat de rol sysadmin te op het systeem-account (NTAuthority\System) op de beveiligde computer vinden is zoals beschreven in [in dit artikel](https://technet.microsoft.com/library/hh757977(v=sc.12).aspx).
| Beveiligingsgroepen configureren | Er is onvoldoende vrije ruimte in de opslaggroep voor deze beveiligingsgroep. | De schijven die zijn toegevoegd aan de opslaggroep [mag niet een partitie](https://technet.microsoft.com/library/hh758075(v=sc.12).aspx). Verwijder alle bestaande volumes op de schijven. Voeg ze toe aan de opslaggroep.|
| Beleid wijzigen |Het back-upbeleid kan niet worden gewijzigd. Fout: De huidige bewerking is mislukt vanwege een interne servicefout [0x29834]. Voer de bewerking na enige tijd is verstreken. Als het probleem zich blijft voordoen, neem dan contact op met Microsoft ondersteuning. | **Oorzaak:**<br/>Deze fout treedt op onder drie voorwaarden: Wanneer beveiligingsinstellingen zijn ingeschakeld, wanneer u probeert te verminderen van de bewaartermijn hieronder de minimale waarden eerder hebt opgegeven, en wanneer u zich op een niet-ondersteunde versie. (Niet-ondersteunde versies zijn die onder versie 2.0.9052 van de Microsoft Azure Backup Server en de Azure Backup Server-update 1). <br/>**Aanbevolen actie:**<br/> Om door te gaan met updates met betrekking tot beleid, moet u de bewaarperiode boven de minimale periode opgegeven bewaarperiode instellen. (De minimale bewaarperiode is zeven dagen voor dagelijks, vier weken voor wekelijkse, drie weken voor maandelijks of één jaar voor jaarlijkse.) <br><br>(Optioneel) voorkeur een andere aanpak is het bijwerken van de backup-agent en de Azure Backup-Server als u wilt gebruikmaken van alle beveiligingsupdates. |

## <a name="backup"></a>Backup

| Bewerking | Foutdetails | Tijdelijke oplossing |
| --- | --- | --- |
| Backup | Er is een onverwachte fout opgetreden terwijl de taak werd uitgevoerd. Het apparaat is niet gereed. | **Als de aanbevolen actie die wordt weergegeven in het product niet werkt, kunt u de volgende stappen:** <br> <ul><li>De ruimte opslagruimte op schaduwkopie instellen op onbeperkt op de items in de beveiligingsgroep toe en voer de consistentiecontrole.<br></li> (OR) <li>Verwijder de bestaande beveiliging groep en het maken van meerdere nieuwe groepen. Elke nieuwe beveiligingsgroep moet een afzonderlijk item hebben erin.</li></ul> |
| Backup | Als u een back-up alleen de systeemstatus, controleert u of er is onvoldoende vrije ruimte op de beveiligde computer voor het opslaan van de systeemstatusback-up. | <ol><li>Controleer of Windows Server back-up is geïnstalleerd op de beveiligde machine.</li><li>Controleer of er voldoende ruimte is op de beveiligde computer voor de systeemstatus. De eenvoudigste manier om te controleren of dit is de beveiligde computer naar Windows Server back-up openen, klikt u op via de instellingen en selecteer vervolgens BMR. De gebruikersinterface vervolgens ziet u hoeveel ruimte is vereist. Open **WSB** > **lokale back-up** > **back-upschema** > **back-upconfiguratie selecteren**  >  **Volledige server** (grootte wordt weergegeven). Deze grootte gebruiken voor verificatie.</li></ol>
| Backup | Back-up mislukt voor BMR | Als de grootte van de BMR groot is, sommige toepassingsbestanden verplaatsen naar het station van het besturingssysteem en probeer het opnieuw. |
| Backup | De optie voor het opnieuw beveiligen van een VMware-VM op een nieuwe Microsoft Azure Backup Server wordt niet weergegeven als beschikbaar om toe te voegen. | VMware-eigenschappen zijn op een oude, buiten gebruik gestelde exemplaar van Microsoft Azure Backup Server gericht. Los dit probleem als volgt op:<br><ol><li>In VCenter (SC-VMM-equivalent), gaat u naar de **samenvatting** tabblad, en van daaruit naar **aangepaste kenmerken**.</li>  <li>Verwijder de oude Microsoft Azure Backup Server-naam van de **DPMServer** waarde.</li>  <li>Ga terug naar de nieuwe Microsoft Azure Backup Server en de pagina wijzigen  Nadat u hebt geselecteerd de **vernieuwen** knop, de virtuele machine wordt weergegeven met een selectievakje in als beschikbaar om toe te voegen aan de beveiliging.</li></ol> |
| Backup | Fout bij het openen van bestanden/gedeelde mappen | Wijzig de antivirus-instellingen zoals aangegeven in de TechNet-artikel [antivirussoftware uitvoeren op de DPM-server](https://technet.microsoft.com/library/hh757911.aspx).|


## <a name="change-passphrase"></a>Wachtwoordzin wijzigen

| Bewerking | Foutdetails | Tijdelijke oplossing |
| --- | --- | --- |
| Wachtwoordzin wijzigen |De beveiligingspincode die is ingevoerd, is onjuist. Geef de juiste beveiliging PINCODE om deze bewerking te voltooien. |**Oorzaak:**<br/> Deze fout treedt op wanneer u een ongeldige of verlopen beveiligingspincode tijdens het uitvoeren van een kritieke bewerking (zoals het wijzigen van een wachtwoordzin). <br/>**Aanbevolen actie:**<br/> Als u wilt de bewerking is voltooid, moet u een geldige beveiligingspincode invoeren. Als u de PINCODE, zich aanmelden bij de Azure-portal en gaat u naar de Recovery Services-kluis. Ga vervolgens naar **instellingen** > **eigenschappen** > **BEVEILIGINGSPINCODE genereren**. Gebruik deze PINCODE te wijzigen van de wachtwoordzin. |
| Wachtwoordzin wijzigen |Bewerking is mislukt. ID: 120002 |**Oorzaak:**<br/>Deze fout treedt op wanneer de beveiligingsinstellingen zijn ingeschakeld, of wanneer u probeert te wijzigen van de wachtwoordzin wanneer u een niet-ondersteunde versie.<br/>**Aanbevolen actie:**<br/> Als u wilt wijzigen van de wachtwoordzin, moet u eerst de backup-agent bijwerken naar de minimaal versie 2.0.9052 is. U moet ook Azure Backup Server bijwerken naar het minimum van update 1 en voer een geldige beveiligingspincode. Als de PINCODE, meld u aan bij de Azure-portal en gaat u naar de Recovery Services-kluis. Ga vervolgens naar **instellingen** > **eigenschappen** > **BEVEILIGINGSPINCODE genereren**. Gebruik deze PINCODE te wijzigen van de wachtwoordzin. |


## <a name="configure-email-notifications"></a>E-mailmeldingen configureren

| Bewerking | Foutdetails | Tijdelijke oplossing |
| --- | --- | --- |
| E-mailmeldingen met een Office 365-account instellen |Fout-ID: 2013| **Oorzaak:**<br> Het gebruik van Office 365-account <br>**Aanbevolen actie:**<ol><li> Het eerste wat om ervoor te zorgen, is dat 'Toestaan anonieme Relay op een Ontvangconnector' voor uw DPM-server is ingesteld op Exchange. Zie voor meer informatie over hoe u dit wilt configureren, [toestaan anonieme Relay op een Connector ontvangen](https://technet.microsoft.com/library/bb232021.aspx) op TechNet.</li> <li> Als u kunt geen gebruik van een interne SMTP-relay en instellen moet met behulp van uw Office 365-server, kunt u IIS instellen om te worden van een relay. De DPM-server configureren [doorsturen van de SMTP tot O365 met behulp van IIS](https://technet.microsoft.com/library/aa995718(v=exchg.65).aspx).<br><br> **BELANGRIJK:** Zorg ervoor dat u de gebruiker\@domein.com indeling en *niet* domein\gebruiker.<br><br><li>DPM-punt op de lokale server-naam gebruiken als de SMTP-server-poort 587. Wijs deze het e-mailadres voor een gebruiker die de e-mailberichten van afkomstig moeten zijn.<li> De gebruikersnaam en het wachtwoord op de pagina van de installatie van DPM SMTP moet voor een domeinaccount in het domein waarop DPM ingeschakeld is. </li><br> **OPMERKING**: Wanneer u het adres van de SMTP-server wijzigen wilt, de wijziging aanbrengen in de nieuwe instellingen, instellingen voor het sluiten en weer openen om er zeker van te zijn dat dit de nieuwe waarde weergegeven.  Eenvoudig wijzigen en testen niet altijd mogelijk de nieuwe instellingen toe te passen, zodat het testen van het op deze manier is een aanbevolen procedure.<br><br>Op elk gewenst moment tijdens dit proces kunt u deze instellingen wissen door de DPM-console sluiten en het bewerken van de volgende registersleutels: **HKLM\SOFTWARE\Microsoft\Microsoft Data Protection Manager\Notification\ <br/> SMTPPassword verwijderen en SMTPUserName sleutels**. U kunt deze toevoegen terug naar de gebruikersinterface als u het opnieuw start.
