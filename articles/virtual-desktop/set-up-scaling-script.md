---
title: Automatisch schalen Windows Virtual Desktop Preview sessie hosts - Azure
description: Beschrijft hoe u het script voor automatische schaling instellen voor Windows Virtual Desktop Preview sessie hosts.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: e9f500e3ab965b9dbfc5e395a6572497c85f6f8f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66755125"
---
# <a name="automatically-scale-session-hosts"></a>Sessiehosts automatisch schalen

Voor veel implementaties van Windows Virtual Desktop Preview in Azure vertegenwoordigen de kosten voor de virtuele machine groot deel van de totale kosten voor virtuele Windows-bureaublad-implementatie. Als u wilt verlagen, is het raadzaam te sluit en toewijzing ongedaan maken sessie hosten van virtuele machines (VM's) tijdens het van de minder drukke gebruiksuren af en start deze opnieuw tijdens piekuren.

In dit artikel wordt een eenvoudig script vergroten/verkleinen sessie hosten van virtuele machines automatisch schalen in uw virtuele Windows-bureaublad-omgeving. Zie voor meer informatie over de werking van het script vergroten/verkleinen, de [de werking van het script vergroten/verkleinen](#how-the-scaling-script-works) sectie.

## <a name="prerequisites"></a>Vereisten

De omgeving waarin u het script uitvoert, moet de volgende zaken:

- Een virtuele Windows-bureaublad-tenant en -account of een service-principal met machtigingen om op te vragen die tenant (zoals Inzender voor extern bureaublad-services).
- Sessie host pool-VM's geconfigureerd en geregistreerd bij de virtuele Windows-bureaublad-service.
- Een extra virtuele machine die de geplande taak wordt uitgevoerd via Taakplanner en heeft toegang tot het netwerk voor hosts van de sessie. Dit wordt genoemd in het document later scaler VM.
- De [Microsoft Azure Resource Manager PowerShell-module](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) geïnstalleerd op de virtuele machine waarop de geplande taak wordt uitgevoerd.
- De [Windows virtuele bureaublad PowerShell-module](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) geïnstalleerd op de virtuele machine waarop de geplande taak wordt uitgevoerd.

## <a name="recommendations-and-limitations"></a>Aanbevelingen en beperkingen

Als u de vergroten/verkleinen script is uitgevoerd, moet u de volgende zaken waarmee u rekening moet:

- Met dit script vergroten/verkleinen kan slechts één host groep per exemplaar van de geplande taak die wordt uitgevoerd het vergroten/verkleinen script verwerken.
- De geplande taken die worden uitgevoerd, vergroten/verkleinen scripts moeten zich op een virtuele machine die is altijd ingeschakeld.
- Maak een afzonderlijke map voor elk exemplaar van het script vergroten/verkleinen en de configuratie ervan.
- Met dit script biedt geen ondersteuning voor aanmelden als een beheerder met virtuele Windows-bureaublad met een Azure AD-gebruikersaccount die meervoudige verificatie vereisen. U wordt aangeraden dat u service-principals gebruiken voor toegang tot de virtuele Windows-bureaublad-service en Azure. Ga als volgt [in deze zelfstudie](create-service-principal-role-powershell.md) maken van een service-principal en een roltoewijzing met PowerShell.
- SLA-garantie van Azure geldt alleen voor virtuele machines in een beschikbaarheidsset. De huidige versie van het document beschrijft een omgeving met één virtuele machine doet de schaal die wellicht niet voldoen aan de vereisten voor beschikbaarheid.

## <a name="deploy-the-scaling-script"></a>Implementeert u het script voor vergroten/verkleinen

De volgende procedures wordt uitgelegd hoe u om de schaal script te implementeren.

### <a name="prepare-your-environment-for-the-scaling-script"></a>Bereid uw omgeving voor het script vergroten/verkleinen

Bereid eerst uw omgeving voor het vergroten/verkleinen script:

1. Meld u aan de virtuele machine (VM scaler) dat de geplande taak wordt uitgevoerd met een domeinaccount met beheerdersrechten.
2. Maak een map op de virtuele machine om het script vergroten/verkleinen en de configuratie van de scaler (bijvoorbeeld **C:\\schalen HostPool1**).
3. Download de **basicScale.ps1**, **Config.xml**, en **functies PSStoredCredentials.ps1** bestanden, en de **PowershellModules** map van de [schalen scriptbibliotheek](https://github.com/Azure/RDS-Templates/tree/master/wvd-sh/WVD%20scaling%20script) en kopieer deze naar de map die u in stap 2 hebt gemaakt. Er zijn twee primaire manieren om op te halen van de bestanden voordat ze kopiëren naar de scaler VM:
    - De git-opslagplaats naar uw lokale computer klonen.
    - Weergave de **Raw** versie van elk bestand kopiëren en plak de inhoud van elk bestand naar een teksteditor en sla vervolgens de bestanden met de bijbehorende naam en het bestandstype. 

### <a name="create-securely-stored-credentials"></a>Veilig opgeslagen referenties maken

Vervolgens moet u de veilig opgeslagen referenties maken:

1. Open PowerShell ISE als beheerder.
2. De extern bureaublad-services PowerShell-module importeren door de volgende cmdlet:

    ```powershell
    Install-Module Microsoft.RdInfra.RdPowershell
    ```
    
3. Open het deelvenster bewerken en laad de **functie PSStoredCredentials.ps1** bestand.
4. Voer de volgende cmdlet uit:
    
    ```powershell
    Set-Variable -Name KeyPath -Scope Global -Value <LocalScalingScriptFolder>
    ```
    
    Bijvoorbeeld, **variabele instellen - naam KeyPath-Scope globale-waarde ' c:\\schalen HostPool1 "**
5. Voer de **New-StoredCredential - sleutelpad \$sleutelpad** cmdlet. Voer desgevraagd de referenties van uw virtuele Windows-bureaublad met machtigingen voor het opvragen van de groep host (de groep host is opgegeven in de **config.xml**).
    - Als u andere service-principals of standard-account gebruikt, voert u de **New-StoredCredential - sleutelpad \$sleutelpad** cmdlet eenmaal voor elk account voor het maken van lokaal referenties opgeslagen.
6. Voer **Get-StoredCredentials-lijst** om te bevestigen van de referenties zijn gemaakt.

### <a name="configure-the-configxml-file"></a>Het bestand config.xml configureren

Geef de relevante waarden in de volgende velden in de script-instellingen in config.xml vergroten/verkleinen bijwerken:

| Veld                     | Description                    |
|-------------------------------|------------------------------------|
| AADTenantId                   | Azure AD-Tenant-ID die wordt gekoppeld aan het abonnement waarin de VM's voor het hosten van de sessie uitvoeren     |
| AADApplicationId              | Service-principal toepassings-ID                                                       |
| AADServicePrincipalSecret     | Dit kan worden ingevoerd tijdens de testfase, maar moet worden bewaard leeg als u referenties met maakt **functies PSStoredCredentials.ps1**    |
| currentAzureSubscriptionId    | De ID van het Azure-abonnement waarin de VM's voor het hosten van de sessie uitvoeren                        |
| tenantName                    | De naam van de tenant virtuele Windows-bureaublad                                                    |
| hostPoolName                  | Virtuele Windows-bureaublad-hostnaam voor groep van toepassingen                                                 |
| RDBroker                      | URL naar de service WVD standaard waarde https:\//rdbroker.wvd.microsoft.com             |
| Gebruikersnaam                      | De service principal toepassings-ID (dit is mogelijk dezelfde serviceprincipal als in AADApplicationId) of een normale gebruiker zonder multi-factor authentication |
| isServicePrincipal            | Geaccepteerde waarden zijn **waar** of **false**. Geeft aan of de tweede set referenties die wordt gebruikt een service-principal of een standard-account. |
| BeginPeakTime                 | Wanneer gebruik piektijd begint                                                            |
| EndPeakTime                   | Wanneer gebruik piektijd eindigt                                                              |
| TimeDifferenceInHours         | Verschil in tijd tussen lokale tijd en UTC, in uren                                   |
| SessionThresholdPerCPU        | Maximum aantal sessies per CPU-drempelwaarde die wordt gebruikt om te bepalen wanneer een nieuwe sessiehost-VM moet worden gestart tijdens de piekuren.  |
| MinimumNumberOfRDSH           | Minimum aantal host pool-VM's blijven uitvoeren tijdens minder drukke gebruikstijd             |
| LimitSecondsToForceLogOffUser | Het aantal seconden dat moet worden gewacht voordat gebruikers zich afmelden. Als u instelt op 0, gebruikers afmelden worden niet afgedwongen.  |
| LogOffMessageTitle            | Titel van het bericht verzonden naar een gebruiker voordat ze zich afmelden bent gedwongen                  |
| LogOffMessageBody             | De hoofdtekst van de waarschuwing verzonden naar gebruikers voordat ze bent afgemeld. Bijvoorbeeld: ' deze computer wordt afgesloten omlaag in X minuten. Sla uw werk op en meld u af." |

### <a name="configure-the-task-scheduler"></a>Configureren van de Taakplanner

Na het configureren van de configuratie-XML-bestand, moet u de Task Scheduler om uit te voeren van het bestand basicScaler.ps1 met een regelmatig interval te configureren.

1. Start **Task Scheduler**.
2. In de **Task Scheduler** venster **-taak maken...**
3. In **de taak maken** dialoogvenster, selecteer de **algemene** tabblad, voer een **naam** (bijvoorbeeld 'Dynamische RDSH'), selecteer **uitgevoerd of gebruiker is aangemeld of niet** en **uitvoeren met de hoogste bevoegdheden**.
4. Ga naar de **Triggers** tabblad, en selecteer vervolgens **nieuw...**
5. In de **nieuwe Trigger** dialoogvenster onder **geavanceerde instellingen**, Controleer **taak herhalen elke** en selecteer de gewenste periode en de duur (bijvoorbeeld **15 minuten** of **voor onbepaalde tijd**).
6. Selecteer de **acties** tabblad en **nieuw...**
7. In de **nieuwe actie** dialoogvenster Voer **powershell.exe** in de **programma/script** veld, voert u **C:\\schalen\\ RDSScaler.ps1** in de **argumenten toevoegen (optioneel)** veld.
8. Ga naar de **voorwaarden** en **instellingen** tabbladen en selecteer **OK** naar accepteer de standaardinstellingen voor elk.
9. Voer het wachtwoord voor de Administrator-account waar u van plan bent de vergroten/verkleinen script uit te voeren.

## <a name="how-the-scaling-script-works"></a>De werking van het script vergroten/verkleinen

Met dit script vergroten/verkleinen leest de instellingen van een bestand config.xml, met inbegrip van het begin en einde van de periode voor het gebruik van piek gedurende de dag.

Tijdens het gebruik van piektijd controleert het script het huidige aantal sessies en de huidige actieve RDSH-capaciteit voor elke groep host. Als de actieve sessiehost virtuele machines voldoende capaciteit hebt voor de ondersteuning van bestaande sessies op basis van de parameter SessionThresholdPerCPU is gedefinieerd in het bestand config.xml wordt berekend. Als dat niet het geval is, start het script extra sessiehost VM's in de groep host.

Tijdens het gebruik van buiten piektijden bepaalt het script welke VM's af op basis van de parameter MinimumNumberOfRDSH in het bestand config.xml sluiten te-sessiehost. Het script wordt de sessie hosten van virtuele machines verwijderen uit de modus om te voorkomen dat nieuwe sessies verbinding te maken met de hosts ingesteld. Als u de **LimitSecondsToForceLogOffUser** parameter in het bestand config.xml op een positieve waarde dan nul, het script wordt op de hoogte stellen een aangemeld gebruikers te werk op te slaan, wacht de ingestelde tijd wordt opgelost en dwing de gebruikers afmelden. Zodra alle gebruikerssessies ondertekend zijn uitgeschakeld op een VM-sessiehost, kan het script de server wordt afgesloten.

Als u de **LimitSecondsToForceLogOffUser** parameter in het bestand config.xml op nul, het script wordt de configuratie-instelling van de sessie in de host Pooleigenschappen toestaan om af te handelen ondertekening gebruikerssessies afmelden. Als er geen sessies op een VM-sessiehost, blijft tot de host virtuele machine uitgevoerd. Als er geen sessies niet, kan het script de sessiehost-VM wordt afgesloten.

Het script is ontworpen om periodiek wordt uitgevoerd op de scaler-VM-server met behulp van Taakplanner. Selecteer het juiste tijdsinterval op basis van de grootte van uw omgeving voor extern bureaublad-Services en houd er rekening mee dat starten en afsluiten van virtuele machines kunnen enige tijd duren. Het is raadzaam om het script vergroten/verkleinen is om de 15 minuten uitgevoerd.

## <a name="log-files"></a>Logboekbestanden

Het vergroten/verkleinen script maakt u twee logboekbestanden, **WVDTenantScale.log** en **WVDTenantUsage.log**. De **WVDTenantScale.log** bestand loggen de gebeurtenissen en fouten (indien aanwezig) tijdens de uitvoering van het script vergroten/verkleinen.

De **WVDTenantUsage.log** bestand worden vastgelegd het actieve aantal kernen en het aantal actieve virtuele machines telkens wanneer u het vergroten/verkleinen script uitvoeren. U kunt deze informatie gebruiken om te schatten het werkelijke gebruik van Microsoft Azure-VM's en de kosten. Het bestand is opgemaakt als door komma's gescheiden waarden met elk item met de volgende gegevens:

>tijd, de groep van de host, kernen, virtuele machines

Naam van het bestand kan ook worden gewijzigd om een CSV-extensie, geladen in Microsoft Excel en geanalyseerd.
