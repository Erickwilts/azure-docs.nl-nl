---
title: Problemen met Azure Migrate | Microsoft Docs
description: Biedt een overzicht van bekende problemen in de Azure Migrate-service en het oplossen van tips voor veelvoorkomende fouten.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: raynew
ms.openlocfilehash: dff3c96cf3ac8eea7c1160ee1834cc70390c0333
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60533183"
---
# <a name="troubleshoot-azure-migrate"></a>Problemen met Azure Migrate oplossen

## <a name="troubleshoot-common-errors"></a>Veelvoorkomende problemen oplossen

[Azure Migrate](migrate-overview.md) beoordeelt on-premises werkbelastingen voor migratie naar Azure. Gebruik dit artikel voor het oplossen van problemen bij het implementeren en met behulp van Azure Migrate.

### <a name="i-am-using-the-ova-that-continuously-discovers-my-on-premises-environment-but-the-vms-that-are-deleted-in-my-on-premises-environment-are-still-being-shown-in-the-portal"></a>Ik gebruik het ova-bestand die continu detecteert mijn on-premises-omgeving, maar de virtuele machines die zijn verwijderd in mijn on-premises-omgeving worden nog steeds wordt weergegeven in de portal.

Het apparaat voor continue detectie verzamelt alleen continu prestatiegegevens, het detecteert niet elke configuratiewijziging in de on-premises omgeving (dat wil zeggen het toevoegen/verwijderen van VM’s, toevoegen van schijven, enz.). Als er een configuratiewijziging in de on-premises omgeving is, kunt u het volgende doen om de wijzigingen door te voeren in de portal:

- Toevoegen van items (virtuele machines, schijven, kernen enz.): om deze wijzigingen in de Azure-portal door te voeren, kunt u de detectie vanaf het apparaat stoppen en opnieuw starten. Dit zorgt ervoor dat de wijzigingen worden bijgewerkt in het Azure Migrate-project.

   ![Detectie stoppen](./media/troubleshooting-general/stop-discovery.png)

- Verwijderen van VM’s: vanwege de manier waarop het apparaat is ontworpen, wordt het verwijderen van VM’s niet doorgevoerd, zelfs niet als u de detectie stopt en opnieuw start. Dit komt doordat gegevens uit volgende detecties worden toegevoegd aan de oudere detecties en niet worden overschreven. In dit geval kunt u eenvoudigweg de VM in de portal negeren door deze uit uw groep te verwijderen en de evaluatie opnieuw te berekenen.

### <a name="deletion-of-azure-migrate-projects-and-associated-log-analytics-workspace"></a>Verwijdering van de Azure Migrate projecten en gekoppelde Log Analytics-werkruimte

Wanneer u een Azure Migrate-project hebt verwijderd, worden het migratieproject samen met alle groepen en evaluaties verwijderd. Echter, als u een Log Analytics-werkruimte hebt gekoppeld aan het project, deze niet automatisch verwijderd de Log Analytics-werkruimte. Dit komt doordat de dezelfde Log Analytics-werkruimte kan worden gebruikt voor meerdere gebruiksmogelijkheden. Als u verwijderen van de Log Analytics-werkruimte wilt, moet u dit handmatig doen.

1. Blader naar de Log Analytics-werkruimte die is gekoppeld aan het project.
   a. Als u hebt niet de migration-project hebt verwijderd, kunt u de koppeling vinden in de werkruimte van de overzichtspagina van het project in de sectie Essentials.

   ![LA Workspace](./media/troubleshooting-general/LA-workspace.png)

   b. Als u het migratieproject al hebt verwijderd, klikt u op **resourcegroepen** in het linkerdeelvenster in Azure portal en Ga naar de resourcegroep waarin de werkruimte is gemaakt en blader vervolgens naar deze.
2. Volg de instructies [in dit artikel](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace) om de werkruimte te verwijderen.

### <a name="migration-project-creation-failed-with-error-requests-must-contain-user-identity-headers"></a>Maken van het project migratie is mislukt met fout *aanvragen moeten kopteksten voor gebruikersidentiteiten bevatten*

Dit probleem kan optreden voor gebruikers die geen toegang tot de tenant Azure Active Directory (Azure AD) van de organisatie. Wanneer een gebruiker is toegevoegd aan een Azure AD-tenant voor de eerste keer, ontvangt deze een e-mailuitnodiging om op te nemen aan de tenant. Gebruikers moeten gaat u naar het e-mailadres en accepteer de uitnodiging is toegevoegd aan de tenant. Als u niet om te zien van het e-mailbericht, contact op met een gebruiker die al toegang heeft tot de tenant en vraag deze om te verzenden van de uitnodiging voor u met behulp van de stappen die zijn opgegeven [hier](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator#resend-invitations-to-guest-users).

Zodra de uitnodiging per e-mail wordt ontvangen, moet u het e-mailbericht openen en klik op de koppeling in de e-mail de uitnodiging te accepteren. Zodra deze actie is uitgevoerd, moet u afmelden bij Azure portal en aanmelden, de browser te vernieuwen werkt niet. Vervolgens kunt u proberen het migratieproject maken.

### <a name="i-am-unable-to-export-the-assessment-report"></a>Ik kan geen Exporteer het evaluatierapport

Als u zich niet aan het evaluatierapport exporteren vanuit de portal, kunt u met de onderstaande REST-API voor het ophalen van een download-URL voor het evaluatierapport.

1. Installeer *armclient* op uw computer (als deze nog niet al geïnstalleerd):

   a. Voer de volgende opdracht in een beheerder opdrachtprompt-venster: ```@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"```

   b. Voer de volgende opdracht in een beheerder Windows PowerShell-venster: ```choco install armclient```

2. De download-URL ophalen voor het evaluatierapport met behulp van REST-API van Azure migreren

   a.    Voer de volgende opdracht in een beheerder Windows PowerShell-venster: ```armclient login```

        This opens the Azure login pop-up where you need to sign in to Azure.

   b.    Voer in het dezelfde PowerShell-venster de volgende opdracht uit om op te halen van de download-URL voor het evaluatierapport (Vervang de URI-parameters met de juiste waarden, voorbeeld-API-hieronder aanvragen)

       ```armclient POST https://management.azure.com/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/Microsoft.Migrate/projects/{projectName}/groups/{groupName}/assessments/{assessmentName}/downloadUrl?api-version=2018-02-02```

      Voorbeeld van een aanvraag en de uitvoer:

      ```PS C:\WINDOWS\system32> armclient POST https://management.azure.com/subscriptions/8c3c936a-c09b-4de3-830b-3f5f244d72e9/r
   esourceGroups/ContosoDemo/providers/Microsoft.Migrate/projects/Demo/groups/contosopayroll/assessments/assessment_11_16_2
   018_12_16_21/downloadUrl?api-version=2018-02-02
   {
   "assessmentReportUrl": "https://migsvcstoragewcus.blob.core.windows.net/4f7dddac-f33b-4368-8e6a-45afcbd9d4df/contosopayrollassessment_11_16_2018_12_16_21?sv=2016-05-31&sr=b&sig=litQmHuwi88WV%2FR%2BDZX0%2BIttlmPMzfVMS7r7dULK7Oc%3D&st=2018-11-20T16%3A09%3A30Z&se=2018-11-20T16%3A19%3A30Z&sp=r",
   "expirationTime": "2018-11-20T22:09:30.5681954+05:30"```

3. Kopieer de URL uit het antwoord en open het in een browser voor het downloaden van het evaluatierapport.

4. Nadat u het rapport hebt gedownload, kunt u Excel gebruiken om te bladeren naar de map gedownload en open het bestand in Excel om dit te bekijken.

### <a name="performance-data-for-cpu-memory-and-disks-is-showing-up-as-zeroes"></a>Prestatiegegevens voor CPU, geheugen en schijven wordt weergegeven als nullen

Azure Migrate profileert continu de on-premises omgeving voor het verzamelen van prestatiegegevens van de on-premises VM's. Als u detectie van uw omgeving zojuist hebt gestart, moet u ten minste een dag wachten voordat het verzamelen van de prestatiegegevens is uitgevoerd. Als er een evaluatie wordt gemaakt zonder dat u een dag hebt gewacht, worden de prestatiemetrieken als nullen weergegeven. Als u een dag wacht, kunt u een nieuwe evaluatie maken of de bestaande evaluatie bijwerken met de optie Opnieuw berekenen in het evaluatierapport.

### <a name="i-specified-an-azure-geography-while-creating-a-migration-project-how-do-i-find-out-the-exact-azure-region-where-the-discovered-metadata-would-be-stored"></a>Ik heb een Azure-Geografie hebt opgegeven tijdens het maken van een migratieproject hoe vind ik uit de exacte Azure-regio waar de metagegevens van de gedetecteerde zou worden opgeslagen?

Gaat u naar de **Essentials** sectie de **overzicht** pagina van het project voor het identificeren van de exacte locatie waar de metagegevens worden opgeslagen. De locatie wordt willekeurig geselecteerd binnen het geografische gebied Azure Migrate en u het pas wijzigen. Als u een project maken in een bepaalde regio wilt, kunt u de REST API's te maken van het migratieproject doorgeven van de gewenste regio.

   ![Projectlocatie](./media/troubleshooting-general/geography-location.png)

## <a name="collector-issues"></a>Problemen met collector

### <a name="deployment-of-azure-migrate-collector-failed-with-the-error-the-provided-manifest-file-is-invalid-invalid-ovf-manifest-entry"></a>Implementatie van Azure Migrate Collector is mislukt met de fout: Het opgegeven manifestbestand is ongeldig: Ongeldige OVF manifest vermelding.

1. Controleer of als Azure Migrate Collector OVA-bestand correct is gedownload door het controleren van de hash-waarde. Raadpleeg het [artikel](https://docs.microsoft.com/azure/migrate/tutorial-assessment-vmware#verify-the-collector-appliance) om de hash-waarde te controleren. Als de hash-waarde niet overeen komt, het OVA-bestand opnieuw te downloaden en de implementatie opnieuw probeert.
2. Als deze nog steeds mislukt en u VMware vSphere Client gebruikt om de OVF te implementeren, kunt u proberen deze te implementeren via de vSphere Web Client. Als het nog steeds mislukt, probeer het met andere webbrowser.
3. Als u met behulp van vSphere-webclient en probeert te implementeren op de vCenter-Server 6.5 of 6.7, probeert het ova-bestand rechtstreeks op de ESXi-host implementeren door de onderstaande stappen te volgen:
   - Verbinding maken met de ESXi-host rechtstreeks (in plaats van vCenter-Server) met behulp van de webclient (https:// <*host IP-adres*> /ui)
   - Ga naar de introductiepagina van > inventaris
   - Klik op bestand > implementeren van OVF-sjabloon > Ga naar het ova-bestand en de implementatie te voltooien
4. Als de implementatie nog steeds mislukt, moet u contact op met ondersteuning voor Azure Migrate.

### <a name="unable-to-select-the-azure-cloud-in-the-appliance-fails-with-error-azure-cloud-selection-failed"></a>Selecteer het Azure kan niet in de cloud in het toestel, mislukt met fout 'Azure cloud-selectie is mislukt'

Dit is een bekend probleem en een oplossing is beschikbaar voor het probleem. Download de [nieuwste upgrade bits](https://docs.microsoft.com/azure/migrate/concepts-collector-upgrade#continuous-discovery-upgrade-versions) voor het apparaat en de update het toestel om toe te passen van de oplossing.

### <a name="collector-is-not-able-to-connect-to-the-internet"></a>Er is geen collector geen verbinding maken met internet

Dit kan gebeuren wanneer de computer waarop u zich achter een proxy. Zorg ervoor dat u de autorisatiereferenties opgeven als een door de proxy nodig heeft.
Als u een URL-gebaseerde firewallproxy voor het beheren van uitgaande connectiviteit, moet u ervoor zorgen aan lijst met geaccepteerde deze de vereiste URL's:

**URL** | **Doel**  
--- | ---
*.portal.azure.com | Vereist om te controleren van de verbinding met de Azure-service en valideren van tijdsynchronisatie problemen.
*.oneget.org | Vereist voor het downloaden van de powershell op basis van vCenter PowerCLI-module.

**De collector kan geen verbinding met internet maken vanwege een fout bij de validatie van het servercertificaat**

Dit kan gebeuren als u een onderschept proxy verbinding maken met Internet, en als u de proxy-certificaat u aan bij de collector-VM niet geïmporteerd. U kunt de proxycertificaat met behulp van de stappen die worden beschreven importeren [hier](https://docs.microsoft.com/azure/migrate/concepts-collector).

**De collector geen verbinding maken met het project met behulp van de project-ID en sleutel ik gekopieerd vanuit de portal.**

Zorg ervoor dat u hebt gekopieerd en geplakt van de juiste informatie. Installeer de Microsoft Monitoring Agent (MMA) om op te lossen, en controleer of als de MMA worden verbonden met het project als volgt:

1. In de collector-VM, downloadt de [MMA](https://go.microsoft.com/fwlink/?LinkId=828603).
2. Dubbelklik op het gedownloade bestand voor het starten van de installatie.
3. In setup op de **Welkom** pagina, klikt u op **volgende**. Klik op de pagina **Licentievoorwaarden** op **Akkoord** om de licentie te accepteren.
4. In **doelmap**, houden of wijzigen van de standaardinstallatiemap > **volgende**.
5. In **installatieopties voor Agent**, selecteer **Azure Log Analytics** > **volgende**.
6. Klik op **toevoegen** om toe te voegen een nieuwe Log Analytics-werkruimte. Plak in het project-ID en sleutel die u hebt gekopieerd. Klik op **Volgende**.
7. Controleer of dat de agent verbinding met het project maken kunt. Als dit niet, Controleer de instellingen. Als de agent verbinding kunt maken, maar de collector niet kan, moet u contact op met ondersteuning.


### <a name="error-802-date-and-time-synchronization-error"></a>Fout 802: Datum en tijd synchronisatie-fout

De klok van de server mogelijk out van de synchronisatie met de huidige tijd door meer dan vijf minuten. De kloktijd op de collector-VM zodat deze overeenkomen met de huidige tijd, als volgt wijzigen:

1. Open een opdrachtprompt met beheerdersrechten op de virtuele machine.
2. Om te controleren of de tijdzone, w32tm /tz worden uitgevoerd.
3. De om tijd te synchroniseren, w32tm/resync worden uitgevoerd.

### <a name="vmware-powercli-installation-failed"></a>Installatie van VMware PowerCLI is mislukt

Azure Migrate collector PowerCLI downloadt en installeert deze op het apparaat. Fout bij de installatie van PowerCLI kan worden veroorzaakt door niet bereikbaar-eindpunten voor de opslagplaats PowerCLI. Als u wilt oplossen, probeert handmatig installeren van PowerCLI in de collector-VM met behulp van de volgende stap:

1. Open Windows PowerShell in de beheerdersmodus
2. Ga naar de map C:\ProgramFiles\ProfilerService\VMWare\Scripts\
3. Voer het script InstallPowerCLI.ps1

### <a name="error-unhandledexception-internal-error-occurred-systemiofilenotfoundexception"></a>Er is een interne fout UnhandledException opgetreden: System.IO.FileNotFoundException

Dit probleem kan optreden vanwege een probleem met de installatie van VMware PowerCLI. Volg de onderstaande stappen om het probleem te verhelpen:

1. Als u niet op de nieuwste versie van het collector-apparaat, [uw Collector upgraden naar de nieuwste versie](https://aka.ms/migrate/col/checkforupdates) en controleer of het probleem opgelost is.
2. Als u de meest recente versie van de collector al hebt, volgt u de onderstaande stappen om u te doen een schone installatie van PowerCLI:

   a. Sluit de webbrowser in het apparaat.

   b. Stop de 'Azure Migrate Collector-service door te gaan naar Windows Service Manager (Open 'Uitvoeren' en typ services.msc om Windows Service Manager te openen). Klik met de rechtermuisknop op de Azure Migrate Collector-Service en klik op stoppen.

   c. Verwijder alle mappen die beginnen met 'VMware' van de volgende locaties: C:\Program Files\WindowsPowerShell\Modules  
        C:\Program Files (x86)\WindowsPowerShell\Modules

   d. Start de 'Azure Migrate Collector-service in Windows Service Manager (Open 'Uitvoeren' en typ services.msc om Windows Service Manager te openen). Klik met de rechtermuisknop op de Azure Migrate Collector-Service en klik op Start.

   e. Dubbelklik op het bureaublad een snelkoppeling 'Collector uitvoeren' om de collector-toepassing te starten. De collector-toepassing moet automatisch downloaden en installeren van de vereiste versie van PowerCLI.

3. Als de bovenstaande het probleem niet verhelpen, voert u stappen een naar c hierboven en installeer vervolgens handmatig PowerCLI in het apparaat met behulp van de volgende stappen uit:

   a. Opschonen van alle onvolledige PowerCLI bestanden voor installatie door de volgende stappen #a naar #c in stap #2 hierboven.

   b. Ga naar Start > uitvoeren > Open Windows PowerShell(x86) in de beheerdersmodus

   c. Voer de opdracht uit:  Install-Module "VMWare.VimAutomation.Core" - RequiredVersion "6.5.2.6234650" ("A" wanneer deze wordt gevraagd om bevestiging type)

   d. Start de 'Azure Migrate Collector-service in Windows Service Manager (Open 'Uitvoeren' en typ services.msc om Windows Service Manager te openen). Klik met de rechtermuisknop op de Azure Migrate Collector-Service en klik op Start.

   e. Dubbelklik op het bureaublad een snelkoppeling 'Collector uitvoeren' om de collector-toepassing te starten. De collector-toepassing moet automatisch downloaden en installeren van de vereiste versie van PowerCLI.

4. Als u zich niet voor het downloaden van de module in het toestel vanwege problemen met een firewall, downloaden en installeren van de module op een virtuele machine die toegang heeft tot internet met behulp van de volgende stappen uit:

    a. Opschonen van alle onvolledige PowerCLI bestanden voor installatie door de volgende stappen #a naar #c in stap #2 hierboven.

    b. Ga naar Start > uitvoeren > Open Windows PowerShell(x86) in de beheerdersmodus

    c. Voer de opdracht uit:  Install-Module "VMWare.VimAutomation.Core" - RequiredVersion "6.5.2.6234650" ("A" wanneer deze wordt gevraagd om bevestiging type)

    d. Kopieer alle modules die beginnen met "VMware" van 'C:\Program Files (x86) \WindowsPowerShell\Modules' naar dezelfde locatie op de collector-VM.

    e. Start de 'Azure Migrate Collector-service in Windows Service Manager (Open 'Uitvoeren' en typ services.msc om Windows Service Manager te openen). Klik met de rechtermuisknop op de Azure Migrate Collector-Service en klik op Start.

    f. Dubbelklik op het bureaublad een snelkoppeling 'Collector uitvoeren' om de collector-toepassing te starten. De collector-toepassing moet automatisch downloaden en installeren van de vereiste versie van PowerCLI.

### <a name="error-unabletoconnecttoserver"></a>Error UnableToConnectToServer

Kan geen verbinding maken met vCenter Server: 'Servernaam.com:9443' vanwege de fout: Er luistert geen eindpunt op https://Servername.com:9443/sdk dat het bericht kan accepteren.

Als u de nieuwste versie van het collector-apparaat worden uitgevoerd, zo niet, upgrade van het apparaat kan controleren de [meest recente versie](https://docs.microsoft.com/azure/migrate/concepts-collector).

Als het probleem wordt nog steeds in de meest recente versie gebeurt, kan het zijn dat de collector-computer kan niet worden omgezet van de naam van de vCenter-Server opgegeven of de opgegeven poort is onjuist is. Standaard, als de poort niet is gespecificeerd, probeert collector verbinding maakt met het poortnummer 443.

1. Probeer servernaam.com van de collector-machine te pingen.
2. Als stap 1 mislukt, probeert u via het IP-adres verbinding te maken met de vCenter-server.
3. Bepaal het juiste poortnummer om verbinding te maken met de vCenter-server.
4. Controleer tot slot of de vCenter-server actief is.

### <a name="antivirus-exclusions"></a>Uitsluitingen antivirus

Als u wilt beperken van het apparaat Azure Migrate, die u wilt uitsluiten van de volgende mappen in het toestel antivirusscans:

- Map met de binaire bestanden voor Azure Migrate-Service. Sluit alle submappen.
  %ProgramFiles%\ProfilerService  
- Azure Migrate-webtoepassing. Sluit alle submappen.
  %SystemDrive%\inetpub\wwwroot
- Lokale Cache voor de Database en logboekbestanden. Azure migrate-service moet RW toegang tot deze map.
  %SystemDrive%\Profiler

## <a name="dependency-visualization-issues"></a>Problemen met visualisatie van afhankelijkheden

### <a name="i-am-unable-to-find-the-dependency-visualization-functionality-for-azure-government-projects"></a>Ik kan vinden van de functie voor visualisatie van afhankelijkheden voor Azure Government-projecten.

Azure Migrate afhankelijk is van de Service Map voor de functie voor visualisatie van afhankelijkheden en aangezien Serviceoverzicht momenteel niet beschikbaar in Azure Government is, deze functionaliteit is niet beschikbaar in Azure Government.

### <a name="i-installed-the-microsoft-monitoring-agent-mma-and-the-dependency-agent-on-my-on-premises-vms-but-the-dependencies-are-now-showing-up-in-the-azure-migrate-portal"></a>Kan ik de Microsoft Monitoring Agent (MMA) en de agent voor afhankelijkheden op mijn on-premises VM's geïnstalleerd, maar de afhankelijkheden worden nu weergegeven in de Azure Migrate-portal.

Nadat u de agents hebt geïnstalleerd, plaats Azure Migrate vindt meestal voor 15 tot 30 minuten om de afhankelijkheden in de portal weer te geven. Als u meer dan 30 minuten hebt gewacht, controleert u of de MMA-agent kan communiceren met de OMS-werkruimte door de onderstaande stappen te volgen:

Voor Windows VM:
1. Ga naar **Configuratiescherm** en start **Microsoft Monitoring Agent**
2. Ga naar de **Azure Log Analytics (OMS)** tabblad in de pop-MMA-eigenschappen
3. Zorg ervoor dat de **Status** voor de werkruimte groen is.
4. Als de status niet groen is is, probeert de werkruimte verwijderen en opnieuw toe te voegen aan de MMA.
        ![MMA-Status](./media/troubleshooting-general/mma-status.png)

Voor Linux-VM, moet u ervoor zorgen dat de opdrachten voor installatie van MMA- en afhankelijkheidsmonitors agent is voltooid.

### <a name="what-are-the-operating-systems-supported-by-mma"></a>Wat zijn de besturingssystemen die door MMA?

De lijst van Windows-besturingssystemen wordt ondersteund door de MMA is [hier](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems).
De lijst met Linux-besturingssystemen wordt ondersteund door de MMA is [hier](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

### <a name="what-are-the-operating-systems-supported-by-dependency-agent"></a>Wat zijn de besturingssystemen die ondersteund door de agent voor afhankelijkheden?

De lijst van Windows-besturingssystemen wordt ondersteund door de agent voor afhankelijkheden is [hier](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems).
De lijst met Linux-besturingssystemen wordt ondersteund door de agent voor afhankelijkheden is [hier](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems).

### <a name="i-am-unable-to-visualize-dependencies-in-azure-migrate-for-more-than-one-hour-duration"></a>Ik kan geen visualiseren afhankelijkheden in Azure Migrate voor meer dan één uur duurt?
Azure Migrate kunt u visualiseren afhankelijkheden voor maximaal één uur duurt. Hoewel Azure Migrate, u kunt terugkeren naar een bepaalde datum in de geschiedenis voor maximaal laatste maand, is de maximale duur waarvoor u de afhankelijkheden visualiseren maximaal 1 uur. Bijvoorbeeld, u kunt de functionaliteit van de duur van de tijd in de kaart van afhankelijkheden, gebruiken om afhankelijkheden voor gisteren, maar kan alleen weergeven voor een venster van één uur. Echter, kunt u logboeken met Azure Monitor [query uitvoeren op de afhankelijkheidsgegevens](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) gedurende een langere periode.

### <a name="i-am-unable-to-visualize-dependencies-for-groups-with-more-than-10-vms"></a>Ik kan geen visualiseren afhankelijkheden voor groepen met meer dan 10 virtuele machines?
U kunt [visualiseren afhankelijkheden voor groepen](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) dat hebben van 10 virtuele machines, hebt u een groep met meer dan 10 virtuele machines, we raden u aan de groep in kleinere groepen splitsen en de afhankelijkheden visualiseren.

### <a name="i-installed-agents-and-used-the-dependency-visualization-to-create-groups-now-post-failover-the-machines-show-install-agent-action-instead-of-view-dependencies"></a>Ik agents zijn geïnstalleerd en de visualisatie van afhankelijkheden gebruikt voor het maken van groepen. Nu na een failover, de machines actie "Agent installeren" in plaats van 'Afhankelijkheden bij weergave' weergeven
* Post geplande of niet-geplande failover, on-premises machines zijn uitgeschakeld en gelijkwaardige machines in Azure hebben zijn ingeschakeld. Deze machines een ander MAC-adres hebt aanschaft. Ze kunnen een ander IP-adres dat is gebaseerd op of de gebruiker heeft ervoor gekozen op het lokale IP-adres behouden of niet verkrijgen. Als zowel MAC als IP-adressen verschillen, Azure Migrate heeft niet de on-premises machines koppelen aan alle gegevens van de afhankelijkheid Serviceoverzicht en vraagt de gebruiker voor het installeren van agents in plaats van afhankelijkheden weergeven.
* Test-failover, plaatsen de on-premises machines ingeschakeld blijven zoals verwacht. Gelijkwaardige machines hebben ingeschakeld in Azure ander MAC-adres verkrijgen en kunnen verschillende IP-adres hebt aanschaft. Tenzij de gebruiker blokkeert uitgaande Azure Monitor zich verkeer van deze machines, Azure Migrate koppelt de on-premises computers niet met alle gegevens van de afhankelijkheid Serviceoverzicht en vraagt de gebruiker voor het installeren van agents in plaats van afhankelijkheden weergeven.

## <a name="troubleshoot-azure-readiness-issues"></a>Problemen met Azure-gereedheid

**Probleem** | **Fix**
--- | ---
Niet-ondersteund opstarttype | Azure biedt geen ondersteuning voor virtuele machines met EFI-opstarttype. Het verdient aanbeveling om te converteren van het opstarttype in BIOS voordat u een migratie uitvoert. <br/><br/>U kunt [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/tutorial-migrate-on-premises-to-azure) de migratie van dergelijke virtuele machines uitvoert, zoals deze het opstarttype van de virtuele machine naar BIOS tijdens de migratie converteren wordt.
Voorwaardelijk ondersteund Windows-besturingssysteem | Het besturingssysteem is verstreken het einde van ondersteuning voor datum en moet een Custom Support Agreement (CSA) voor [ondersteuning in Azure](https://aka.ms/WSosstatement), houd rekening met het besturingssysteem te upgraden voordat u migreert naar Azure.
Niet-ondersteunde Windows-besturingssysteem | Azure ondersteunt alleen [versies van het Windows-besturingssysteem geselecteerd](https://aka.ms/WSosstatement), houd rekening met het besturingssysteem van de machine te upgraden voordat u migreert naar Azure.
Voorwaardelijk goedgekeurd Linux-besturingssysteem | Azure ondersteunt alleen [geselecteerd Linux OS-versies](../virtual-machines/linux/endorsed-distros.md), houd rekening met het besturingssysteem van de machine te upgraden voordat u migreert naar Azure.
Unendorsed Linux-besturingssysteem | De computer mogelijk op te starten in Azure, maar er is geen ondersteuning voor het besturingssysteem wordt geleverd door Azure, kunt u het besturingssysteem te upgraden een [onderschreven Linux-versie](../virtual-machines/linux/endorsed-distros.md) voordat u migreert naar Azure
Onbekend besturingssysteem | Het besturingssysteem van de virtuele machine is opgegeven als 'Overig' in vCenter Server, vanwege die Azure Migrate niet worden geïdentificeerd met de Azure-gereedheid van de virtuele machine. Zorg ervoor dat het besturingssysteem die op de machine [ondersteund](https://aka.ms/azureoslist) door Azure voordat u de machine migreert.
Niet-ondersteund besturingssysteem | Virtuele machines met 32-bits besturingssystemen kunnen starten in Azure, maar het wordt aanbevolen een upgrade van het besturingssysteem van de virtuele machine van 32-bits naar 64-bits voordat u migreert naar Azure.
Visual Studio-abonnement vereist. | De machines is een Windows-client-besturingssysteem uitgevoerd dat wordt alleen ondersteund in Visual Studio-abonnement.
Geen VM gevonden voor de vereiste opslagprestaties. | De prestaties van opslagruimte (IOPS/doorvoer) vereist voor de machine is groter dan ondersteuning voor Azure-VM. Verminderen de opslagvereisten voor de machine vóór de migratie.
Geen VM gevonden voor de vereiste netwerkprestaties. | De netwerkprestaties (in/uit) dat is vereist voor de machine is groter dan ondersteuning voor Azure-VM. Verminder de netwerkvereisten voor de machine.
De VM is niet gevonden in de opgegeven locatie. | Gebruik een andere doellocatie voor de migratie.
Een of meer ongeschikte schijven. | Een of meer schijven die zijn gekoppeld aan de virtuele machine niet voldoen aan de vereisten voor Azure. Voor elke schijf die is gekoppeld aan de virtuele machine, zorg ervoor dat de grootte van de schijf < 4 TB, zo niet, de grootte van de schijf te verkleinen voordat u migreert naar Azure. Zorg ervoor dat de prestaties (IOPS/doorvoer) die nodig zijn voor elke schijf wordt ondersteund door Azure [beheerde schijven van virtuele machines](https://docs.microsoft.com/azure/azure-subscription-service-limits#storage-limits).   
Een of meer ongeschikte netwerkadapters. | Verwijder niet-gebruikte netwerkadapters van de machine vóór de migratie.
Aantal schijven overschrijdt de limiet | Verwijder niet-gebruikte schijven van de machine vóór de migratie.
Schijfgrootte overschrijdt de limiet | Azure biedt ondersteuning voor schijven met grootte van 4 TB. Schijven verkleinen tot minder dan 4 TB vóór de migratie.
Schijf niet beschikbaar in de opgegeven locatie | Zorg ervoor dat de schijf is op de doellocatie voordat u migreert.
Schijf niet beschikbaar voor de opgegeven redundantie | De schijf moet het opslagtype voor redundantie gedefinieerd in de evaluatie-instellingen (LRS standaard) gebruiken.
Kan de geschiktheid van de schijf vanwege een interne fout niet bepalen. | Maak een nieuwe beoordeling van de groep.
VM met vereiste kernen en geheugen niet gevonden | Azure kan geen geschikte VM-type niet nauwkeurig. Verminder het geheugen en het aantal kernen van de on-premises machine voordat u migreert.
Kan de geschiktheid van de VM vanwege een interne fout niet bepalen. | Maak een nieuwe beoordeling van de groep.
Kan de geschiktheid voor een of meer schijven vanwege een interne fout niet bepalen. | Maak een nieuwe beoordeling van de groep.
Kan de geschiktheid voor een of meer netwerkadapters vanwege een interne fout niet bepalen. | Maak een nieuwe beoordeling van de groep.


## <a name="collect-logs"></a>Logboeken verzamelen

**Hoe ik verzamelen van Logboeken op de collector-VM?**

Logboekregistratie is standaard ingeschakeld. Logboeken bevinden zich als volgt:

- C:\Profiler\ProfilerEngineDB.sqlite
- C:\Profiler\Service.log
- C:\Profiler\WebApp.log

Voor het verzamelen van Event Tracing voor Windows, het volgende doen:

1. Open een PowerShell-opdrachtvenster op de collector-VM.
2. Voer **Get-EventLog - logboeknaam toepassing | export-csv eventlog.csv**.

**Hoe ik verzamelen van logboeken over webverkeer portal netwerk?**

1. Open de browser, ga en meld u [naar de portal](https://portal.azure.com).
2. Druk op F12 te starten van de hulpprogramma's voor ontwikkelaars. Indien nodig, schakelt u de instelling **ingangen op navigatie**.
3. Klik op de **netwerk** tabblad, en start u het netwerkverkeer vast te leggen:
   - Selecteer in Chrome, **behouden log**. De opname start automatisch. Een rode cirkel geeft aan dat verkeer vastleggen wordt. Als deze niet wordt weergegeven, klikt u op de zwarte cirkel te starten
   - In Microsoft Edge/IE, opname automatisch moet worden gestart. Als dat niet het geval is, klikt u op de groene knop.
4. Probeer de fout te reproduceren.
5. Nadat u hebt de fout tijdens het opnemen, opname stoppen en sla een kopie van de geregistreerde activiteit:
   - In Chrome met de rechtermuisknop en klikt u op **opslaan als HAR met inhoud**. Dit zips en exporteert de logboeken als een .har-bestand.
   - In Microsoft Edge/IE, klikt u op de **uitvoer vastgelegd verkeer** pictogram. Dit zips en exporteert het logboek.
6. Navigeer naar de **Console** tabblad om te controleren op fouten of waarschuwingen worden gegenereerd. Om op te slaan in het consolelogboek:
   - Chrome, met de rechtermuisknop op een willekeurige plaats in het consolelogboek. Selecteer **opslaan als**, om te exporteren en postcode van het logboek.
   - In Microsoft Edge/IE, met de rechtermuisknop op de fouten en selecteer **Kopieer alle**.
7. Sluit de hulpprogramma's voor ontwikkelaars.

## <a name="collector-error-codes-and-recommended-actions"></a>Collector-foutcodes en aanbevolen acties

| Foutcode | Naam van de fout   | Message   | Mogelijke oorzaken | Aanbevolen actie  |
| --- | --- | --- | --- | --- |
| 601       | CollectorExpired               | De collector is verlopen.                                                        | De collector is verlopen.                                                                                    | Download een nieuwe versie van de collector en probeer het opnieuw.                                                                                      |
| 751       | UnableToConnectToServer        | Kan geen verbinding maken met vCenter-Server % Name; is vanwege fout: % ErrorMessage;     | Controleer het foutbericht voor meer informatie.                                                             | Los het probleem op en probeer het opnieuw.                                                                                                           |
| 752       | InvalidvCenterEndpoint         | De server (% Name;) is niet een vCenter-Server.                                  | VCenter-Server-gegevens bevatten.                                                                       | Probeer het opnieuw met de juiste vCenter-Serverdetails.                                                                                   |
| 753       | InvalidLoginCredentials        | Kan geen verbinding maken met de vCenter-Server % Name; is vanwege fout: % ErrorMessage; | Verbinding met de vCenter-Server is mislukt wegens ongeldige aanmeldingsreferenties.                             | Zorg ervoor dat de opgegeven aanmeldingsgegevens juist zijn.                                                                                    |
| 754       | NoPerfDataAvailable           | Prestatiegegevens zijn niet beschikbaar.                                               | Niveau van de statistieken in vCenter-Server controleren. Het moet worden ingesteld op 3 om prestatiegegevens beschikbaar. | Wijzig het Statistiekniveau naar 3 (voor 5 minuten, 30 minuten en 2 uur duur) en probeer het na ten minste een dag.                   |
| 756       | NullInstanceUUID               | Er is een machine met null instanceuuid aangetroffen                                  | vCenter-Server mogelijk een onjuist object.                                                      | Los het probleem op en probeer het opnieuw.                                                                                                           |
| 757       | VMNotFound                     | Virtuele machine is niet gevonden                                                  | Virtuele machines kunnen worden verwijderd: % VMID;                                                                | Zorg ervoor dat de virtuele machines zijn geselecteerd en scoping vCenter-voorraad tijdens de detectie bestaat                                      |
| 758       | GetPerfDataTimeout             | Er is een time-out opgetreden voor de VCenter-aanvraag. Bericht % Message;                                  | vCenter-serverreferenties zijn onjuist                                                              | Controleer de vCenter-serverreferenties en zorg ervoor dat de vCenter-Server bereikbaar is. Probeer het opnieuw. Neem contact op met de ondersteuning als het probleem zich blijft voordoen. |
| 759       | VmwareDllNotFound              | VMWare.Vim dll-bestand is niet gevonden.                                                     | PowerCLI is niet juist geïnstalleerd.                                                                   | Controleer of PowerCLI juist is geïnstalleerd. Probeer het opnieuw. Neem contact op met de ondersteuning als het probleem zich blijft voordoen.                               |
| 800       | ServiceError                   | Azure Migrate Collector-service wordt niet uitgevoerd.                               | Azure Migrate Collector-service wordt niet uitgevoerd.                                                       | Gebruik services.msc om te starten van de service en probeer het opnieuw.                                                                             |
| 801       | PowerCLIError                  | Installatie van VMware PowerCLI is mislukt.                                          | Installatie van VMware PowerCLI is mislukt.                                                                  | Probeer het opnieuw. Als het probleem zich blijft voordoen, installeer handmatig en voer de bewerking opnieuw uit.                                                   |
| 802       | TimeSyncError                  | Tijd is niet gesynchroniseerd met de tijdserver op internet.                            | Tijd is niet gesynchroniseerd met de tijdserver op internet.                                                    | Zorg ervoor dat de tijd op de machine juist is ingesteld voor de tijdzone van de machine en probeer het opnieuw.                                 |
| 702       | OMSInvalidProjectKey           | Ongeldige projectsleutel opgegeven.                                                | Ongeldige projectsleutel opgegeven.                                                                        | Voer de bewerking opnieuw uit met de projectsleutel.                                                                                              |
| 703       | OMSHttpRequestException        | Fout tijdens het verzenden van aanvragen. Bericht % Message;                                | Controleer de project-ID en sleutel en zorg ervoor dat het dat eindpunt bereikbaar is.                                       | Probeer het opnieuw. Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen.                                                                     |
| 704       | OMSHttpRequestTimeoutException | Er is een time-out opgetreden voor de HTTP-aanvraag. Bericht % Message;                                     | Controleer de project-ID en sleutel en zorg ervoor dat het dat eindpunt bereikbaar is.                                       | Probeer het opnieuw. Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen.                                                                     |
