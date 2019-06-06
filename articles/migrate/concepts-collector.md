---
title: Over het Collector-apparaat in Azure Migrate | Microsoft Docs
description: Bevat informatie over het Collector-apparaat in Azure Migrate.
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: snehaa
services: azure-migrate
ms.openlocfilehash: 865e0679ed05823d115baeb9eea3c01d7fb5f2a5
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/31/2019
ms.locfileid: "66428479"
---
# <a name="about-the-collector-appliance"></a>Over het Collector-apparaat

 In dit artikel bevat informatie over Azure Migrate Collector.

De Azure Migrate Collector is een lichtgewicht apparaat die kan worden gebruikt voor het detecteren van een on-premises vCenter-omgeving voor evaluatiedoeleinden met de [Azure Migrate](migrate-overview.md) service vóór de migratie naar Azure.  

## <a name="discovery-method"></a>Detectiemethode

Eerder waren er twee opties voor de collector-apparaat, detectie van eenmalige en continue detectie. Het model eenmalige detectie is beëindigd omdat het vertrouwen op de vCenter-Server-instellingen voor statistieken voor verzameling van prestatiegegevens (vereist statistieken instellingen moet worden ingesteld op niveau 3) en ook verzameld gemiddelde items (in plaats van piekuren), wat leidde tot te voorzichtige grootte. Het model continue detectie zorgt ervoor dat het verzamelen van gedetailleerde gegevens en resulteert in een nauwkeurige schaling vanwege verzameling piek-items. Hieronder ziet u hoe het werkt:

Het collector-apparaat continu is verbonden met het Azure Migrate-project en continu verzamelt prestatiegegevens van virtuele machines.

- De collector profielen continu de on-premises omgeving voor het verzamelen van gegevens over het gebruik van realtime elke 20 seconden.
- Het toestel totaliseert de voorbeelden 20 seconden en maakt u één gegevenspunt om de 15 minuten.
- Punt het apparaat de hoogste waarde selecteert in de voorbeelden 20 seconden en verzendt dit naar Azure te maken van de gegevens.
- Dit model afhankelijk niet van de instellingen voor statistieken vCenter-Server om prestatiegegevens te verzamelen.
- U kunt stoppen continue profilering op elk gewenst moment van de Collector.

**Snelle evaluaties:** Met het continue detectie-apparaat, wanneer de detectie voltooid is (het duurt enkele uren, afhankelijk van het aantal virtuele machines), u kunt onmiddellijk een evaluatie maken. Omdat het verzamelen van prestatiegegevens wordt gestart wanneer u detectie activeert, moet u het schaalcriterium in de evaluatie instellen als *as on-premises* wanneer u direct resultaat wilt. Voor evaluaties op basis van prestaties is het raadzaam om ten minste een dag te wachten na het activeren van de detectie om betrouwbare aanbevelingen voor de schaal te krijgen.

Alleen verzamelt prestatiegegevens continu het toestel, detecteert niet elke configuratiewijziging in de on-premises-omgeving (dat wil zeggen VM toevoegen, verwijderen en schijf toevoegen, enz.). Als er een configuratiewijziging in de on-premises omgeving is, kunt u het volgende doen om de wijzigingen door te voeren in de portal:

- Toevoegen van items (virtuele machines, schijven, kernen enz.): om deze wijzigingen in de Azure-portal door te voeren, kunt u de detectie vanaf het apparaat stoppen en opnieuw starten. Dit zorgt ervoor dat de wijzigingen worden bijgewerkt in het Azure Migrate-project.

- Verwijderen van VM’s: vanwege de manier waarop het apparaat is ontworpen, wordt het verwijderen van VM’s niet doorgevoerd, zelfs niet als u de detectie stopt en opnieuw start. Dit komt doordat gegevens uit volgende detecties worden toegevoegd aan de oudere detecties en niet worden overschreven. In dit geval kunt u eenvoudigweg de VM in de portal negeren door deze uit uw groep te verwijderen en de evaluatie opnieuw te berekenen.

> [!NOTE]
> Ondersteuning van het apparaat voor eenmalige detectie is nu beëindigd omdat deze methode gebaseerd was op statistiekinstellingen van vCenter Server voor de beschikbaarheid van prestatiegegevenspunten en gemiddelde prestatiemeteritems verzamelde, wat leidde tot een te voorzichtige schaling van virtuele machines voor migratie naar Azure.

## <a name="deploying-the-collector"></a>Implementatie van de Collector

U implementeert het Collector-apparaat met behulp van een OVF-sjabloon:

- U downloaden de OVF-sjabloon van een Azure Migrate-project in Azure portal. U kunt het gedownloade bestand importeren met vCenter-Server voor het instellen van de virtuele machine van de Collector-apparaat.
- Stelt u een virtuele machine met 8 kernen, 16 GB RAM-geheugen en één schijf van 80 GB van de OVF in VMware. Het besturingssysteem is Windows Server 2016 (64 bits).
- Wanneer u de Collector uitvoert, wordt een aantal controles uitgevoerd om ervoor te zorgen dat de Collector verbinding met het Azure Migrate maken kunt.

- [Meer informatie](tutorial-assessment-vmware.md#create-the-collector-vm) over het maken van de Collector.


## <a name="collector-prerequisites"></a>Collector-vereisten

De Collector moet slagen voor een aantal controles om ervoor te zorgen deze verbinding kan maken met de Azure Migrate-service via internet en het uploaden van gegevens gedetecteerd.

- **Controleer of de Azure-cloud**: De Collector moet weten van de Azure-cloud waarop u van plan bent om te migreren.
    - Selecteer Azure Government als u van plan bent om te migreren naar Azure Government-cloud.
    - Selecteer in de globale Azure als u van plan bent om te migreren naar commerciële Azure-cloud.
    - Op basis van de cloud die u hier opgeeft, wordt het apparaat naar de respectieve eindpunten gedetecteerde metagegevens verzonden.
- **Controleer de internetverbinding**: De Collector verbinding kan maken met internet rechtstreeks of via een proxyserver.
    - Het controleren van de vereisten controleert of de verbinding met [URL's voor vereiste en optionele](#urls-for-connectivity).
    - Als u een directe verbinding met internet hebt, is geen specifieke actie vereist is, dan ervoor te zorgen dat de Collector de vereiste URL's kan bereiken.
    - Als u verbinding via een proxy maakt, noteert u de onderstaande vereisten.
- **Tijdsynchronisatie controleren**: De Collector moeten worden gesynchroniseerd met de tijdserver op internet om te controleren of de aanvragen voor de service worden geverifieerd.
    - De portal.azure.com url moet bereikbaar is vanaf de Collector zodat de tijd kan worden gevalideerd.
    - Als de machine is niet gesynchroniseerd, moet u de klok op de Collector-VM zodat deze overeenkomen met de huidige tijd verandert. Doen dit open een beheerder op de virtuele machine, voer **w32tm /tz** om te controleren of de tijdzone. Voer **w32tm/resync** de tijd te synchroniseren.
- **Controleer de collector-service die wordt uitgevoerd**:  De Azure Migrate Collector-service moet worden uitgevoerd op de Collector-VM.
    - Deze service wordt automatisch gestart wanneer de computer wordt opgestart.
    - Als de service niet actief is, start u deze in het Configuratiescherm.
    - De Collector-service maakt verbinding met vCenter-Server, de VM-metagegevens en de prestaties gegevens verzamelt en verzendt dit naar de service Azure Migrate.
- **Controleren van VMware PowerCLI 6.5 is geïnstalleerd**: De VMware PowerCLI 6.5 PowerShell-module moet worden geïnstalleerd op de Collector-VM, zodat deze met de vCenter-Server communiceren kan.
    - Als de Collector toegang heeft tot de URL's die nodig zijn om de module te installeren, wordt deze installatie automatisch tijdens de implementatie van de Collector.
    - Als de Collector u de module tijdens de implementatie installeren kunt, moet u deze handmatig installeren.
- **Controleer de verbinding met vCenter-Server**: De Collector moeten kunnen vCenter-Server en query's uitvoeren voor virtuele machines, de metagegevens en prestatiemeteritems. [Vereisten controleren](#connect-to-vcenter-server) om verbinding te maken.


### <a name="connect-to-the-internet-via-a-proxy"></a>Verbinding maken met internet via een proxy

- Als de proxyserver verificatie is vereist, kunt u de gebruikersnaam en wachtwoord opgeven bij het instellen van de Collector.
- De IP-adres/de FQDN van de proxyserver moet worden opgegeven als *http:\//IPaddress* of *http:\//FQDN*.
- Alleen HTTP-proxy wordt ondersteund. Op HTTPS gebaseerde proxy-servers worden niet ondersteund door de Collector.
- Als de proxy-server een onderschept proxy is, moet u de proxy-certificaat importeren met de Collector-VM.
  1. In de collector-VM, gaat u naar **startmenu** > **computercertificaten beheren**.
  2. In het hulpprogramma voor certificaten onder **certificaten - lokale Computer**, vinden **vertrouwde uitgevers** > **certificaten**.

      ![Hulpprogramma voor certificaten](./media/concepts-intercepting-proxy/certificates-tool.png)

  3. Kopieer de proxy-certificaat naar de collector-VM. U moet mogelijk opvragen bij uw netwerkbeheerder.
  4. Dubbelklik op het certificaat en klik op **certificaat installeren**.
  5. In de Wizard Certificaat importeren > locatie van de Store, kiest u **lokale computer**.

     ![Certificaatarchieflocatie](./media/concepts-intercepting-proxy/certificate-store-location.png)

  6. Selecteer **alle certificaten in het onderstaande archief plaatsen** > **Bladeren** > **vertrouwde uitgevers**. Klik op **voltooien** om het certificaat te importeren.

     ![Archief met certificaten](./media/concepts-intercepting-proxy/certificate-store.png)

  7. Controleer of het certificaat wordt geïmporteerd zoals verwacht en controleer dat het internet connectiviteit controle werkt zoals verwacht.


### <a name="urls-for-connectivity"></a>URL's voor connectiviteit

De connectiviteitscontrole is gevalideerd door verbinding te maken met een lijst met URL's.

**URL** | **Details**  | **Het controleren van vereisten**
--- | --- | ---
*.portal.azure.com | Van toepassing op Azure wereldwijd. Controleert de connectiviteit met de Azure-service en tijdsynchronisatie. | Toegang tot URL vereist.<br/><br/> Controle van vereisten mislukt als er geen verbinding.
*.portal.azure.us | Alleen van toepassing op Azure Government. Controleert de connectiviteit met de Azure-service en tijdsynchronisatie. | Toegang tot URL vereist.<br/><br/> Controle van vereisten mislukt als er geen verbinding.
*.oneget.org:443<br/><br/> *.github.com/oneget/oneget<br/><br/> *.windows.net:443<br/><br/> *.windowsazure.com:443<br/><br/> *.azure.microsoft.com<br/><br/> *.azure.microsoft.com/en-us<br/><br/> *.powershellgallery.com:443<br/><br/> *.msecnd.net:443<br/><br/> *.visualstudio.com:443<br/><br/> *.visualstudio.microsoft.com | Voor het downloaden van de PowerShell-module voor vCenter PowerCLI. | Toegang tot URL's is vereist.<br/><br/> Controle van vereisten wordt niet mislukken.<br/><br/> Installatie van de automatische module op de Collector-VM mislukt. U moet de module handmatig installeren op een computer die verbinding heeft met internet en kopieer vervolgens de modules naar het apparaat. [Meer informatie door te gaan naar stap 4 in deze handleiding voor probleemoplossing](https://docs.microsoft.com/azure/migrate/troubleshooting-general#error-unhandledexception-internal-error-occurred-systemiofilenotfoundexception).


### <a name="install-vmware-powercli-module-manually"></a>VMware PowerCLI-module handmatig installeren

1. Installeer de module met [stappen](https://blogs.vmware.com/PowerCLI/2017/04/powercli-install-process-powershell-gallery.html). Deze stappen wordt zowel online als offline-installatie beschreven.
2. Als de Collector-VM is offline en installeren op de module op een andere computer met toegang tot internet, moet u de bestanden te kopiëren VMware.* vanaf deze computer met de Collector-VM.
3. U kunt de controles op vereisten om te controleren of PowerCLI is geïnstalleerd opnieuw na de installatie.

### <a name="connect-to-vcenter-server"></a>Verbinding maken met vCenter-Server

De Collector verbinding maakt met de vCenter-Server en query's voor VM-metagegevens en prestatiemeteritems. Dit is wat u nodig hebt voor de verbinding.

- Alleen worden vCenter Server 5.5, 6.0 of 6.5 te of 6.7 ondersteund.
- U hebt een alleen-lezen-account nodig met de machtigingen die zijn samengevat hieronder voor detectie. Alleen datacenters die toegankelijk zijn met het account is toegankelijk voor detectie.
- Standaard u verbinding maken met vCenter-Server met een FQDN-naam of IP-adres. VCenter-Server op een andere poort luistert, u verbinding maken met met behulp van het formulier *IPAddress:Port_Number* of *FQDN:Port_Number*.
- De Collector moet een netwerk verbinding met de vCenter-server hebben.

#### <a name="account-permissions"></a>Accountmachtigingen

**Account** | **Machtigingen**
--- | ---
Ten minste een alleen-lezen-gebruikersaccount | Datacentrumobject –> doorgeven naar onderliggend object, rol=Alleen-lezen   


## <a name="collector-communications"></a>Communicatie van de collector

De collector communiceert zoals in het volgende diagram en de volgende tabel wordt samengevat.

![Diagram van de collector-communicatie](./media/tutorial-assessment-vmware/portdiagram.PNG)


**Collector communiceert met** | **Poort** | **Details**
--- | --- | ---
Azure Migrate-service | TCP 443 | Collector communiceert met Azure Migrate-service via SSL 443.
vCenter Server | TCP 443 | De Collector moet in staat om te communiceren met de vCenter-Server zijn.<br/><br/> Standaard maakt deze verbinding met vCenter op 443.<br/><br/> Als de vCenter-Server op een andere poort luistert, moet deze poort beschikbaar als uitgaande poort in de Collector zijn.
RDP | TCP 3389 |

## <a name="collected-metadata"></a>Verzamelde metagegevens

> [!NOTE]
> Metagegevens die zijn gedetecteerd door de Azure Migrate collector-apparaat wordt gebruikt om u te helpen de juiste grootte uw toepassingen bij het migreren naar Azure, Azure-geschiktheid analyse, afhankelijkheidsanalyse van de toepassing en kosten planning uitvoeren. Microsoft gebruikt deze gegevens ten opzichte van een licentie nalevingscontroles niet.

Het collector-apparaat wordt de volgende metagegevens van de configuratie voor elke virtuele machine gedetecteerd. De configuratiegegevens voor de virtuele machines zijn beschikbaar een uur nadat u de detectie begint.

- Naam van de virtuele machine weergegeven (op de vCenter-Server)
- Pad van de inventaris van de virtuele machine (de host/map op de vCenter-Server)
- IP-adres
- MAC-adres
- Besturingssysteem
- Aantal kernen, schijven, NIC 's
- Grootte van geheugen, schijf-grootten
- Prestatiemeteritems van de VM, schijf en netwerk.

### <a name="performance-counters"></a>Prestatiemeteritems

 Het collector-apparaat verzamelt de volgende prestatiemeteritems voor elke virtuele machine van de ESXi-host met een interval van 20 seconden. Deze items zijn prestatiemeteritems van vCenter en hoewel de terminologie aangeeft dat deze gemiddelde, de voorbeelden 20 seconden real-time-prestatiemeteritems zijn. De prestatiegegevens voor de virtuele machines wordt gestart steeds beschikbaar in de portal twee uur nadat u de detectie hebt gestart. Het is raadzaam om te wachten op ten minste een dag voor het maken van beoordelingen op basis van prestaties om nauwkeurige juiste formaat aanbevelingen te krijgen. Als u direct resultaat zoekt, kunt u evaluaties maken met het criterium voor het instellen als *zoals on-premises* die wordt geen rekening gehouden met de prestatiegegevens voor de juiste grootte.

**Teller** |  **Gevolgen voor de evaluatie**
--- | ---
cpu.usage.average | Aanbevolen VM-grootte en kosten  
mem.usage.average | Aanbevolen VM-grootte en kosten  
virtualDisk.read.average | Berekent de grootte van de schijf, de kosten voor gegevensopslag, VM-grootte
virtualDisk.write.average | Berekent de grootte van de schijf, de kosten voor gegevensopslag, VM-grootte
virtualDisk.numberReadAveraged.average | Berekent de grootte van de schijf, de kosten voor gegevensopslag, VM-grootte
virtualDisk.numberWriteAveraged.average | Berekent de grootte van de schijf, de kosten voor gegevensopslag, VM-grootte
NET.Received.Average | Berekent de VM-grootte                          
net.transmitted.average | Berekent de VM-grootte     

De volledige lijst met items van VMware die zijn verzameld door Azure Migrate vindt u hieronder:

**Categorie** |  **Metadata** | **vCenter datapoint**
--- | --- | ---
MachineDetails | VM-ID | vm.Config.InstanceUuid
MachineDetails | VM-naam | vm.Config.Name
MachineDetails | vCenter Server-ID | VMwareClient.InstanceUuid
MachineDetails |  Beschrijving van de virtuele machine |  vm.Summary.Config.Annotation
MachineDetails | De naam van de licentie-product | vm.Client.ServiceContent.About.LicenseProductName
MachineDetails | Besturingssysteemtype | virtuele machine. Summary.Config.GuestFullName
MachineDetails | Besturingssysteem | virtuele machine. Summary.Config.GuestFullName
MachineDetails | Opstarttype | vm.Config.Firmware
MachineDetails | Aantal kerngeheugens | vm.Config.Hardware.NumCPU
MachineDetails | Geheugen (MB) | vm.Config.Hardware.MemoryMB
MachineDetails | Aantal schijven | virtuele machine. Config.Hardware.Device.ToList(). FindAll(x => x is VirtualDisk).count
MachineDetails | Lijst met schijven grootte | virtuele machine. Config.Hardware.Device.ToList(). FindAll (x = > x is VirtualDisk)
MachineDetails | Lijst met netwerkadapters | virtuele machine. Config.Hardware.Device.ToList(). FindAll (x = > x is VirtualEthernetCard)
MachineDetails | CPU-gebruik | cpu.usage.average
MachineDetails | Geheugengebruik | mem.usage.average
Details van schijf (per schijf) | De sleutelwaarde schijf | schijf. Sleutel
Details van schijf (per schijf) | Eenheid schijfnummer | schijf. UnitNumber
Details van schijf (per schijf) | Schijf-controller-sleutelwaarde | disk.ControllerKey.Value
Details van schijf (per schijf) | Gigabytes ingericht | virtualDisk.DeviceInfo.Summary
Details van schijf (per schijf) | Schijfnaam | Deze waarde wordt gegenereerd op basis van de schijf. UnitNumber, schijf. Sleutel en de schijf. ControllerKey.Value
Details van schijf (per schijf) | Aantal leesbewerkingen per seconde | virtualDisk.numberReadAveraged.average
Details van schijf (per schijf) | Aantal schrijfbewerkingen per seconde | virtualDisk.numberWriteAveraged.average
Details van schijf (per schijf) | MB per seconde gelezen doorvoer | virtualDisk.read.average
Details van schijf (per schijf) | MB per seconde van de doorvoer van schrijfbewerkingen | virtualDisk.write.average
Details van netwerkadapter (per NIC) | Naam van de netwerkadapter | nic.Key
Details van netwerkadapter (per NIC) | MAC-adres | ((VirtualEthernetCard)nic).MacAddress
Details van netwerkadapter (per NIC) | IPv4-adressen | vm.Guest.Net
Details van netwerkadapter (per NIC) | IPv6-adressen | vm.Guest.Net
Details van netwerkadapter (per NIC) | MB per seconde gelezen doorvoer | NET.Received.Average
Details van netwerkadapter (per NIC) | MB per seconde van de doorvoer van schrijfbewerkingen | net.transmitted.average
Inventarisgegevens van pad | Name | container.GetType().Name
Inventarisgegevens van pad | Het type onderliggend object | container.ChildType
Inventarisgegevens van pad | Details van verwijzing | container.MoRef
Inventarisgegevens van pad | Pad van de volledige inventarisatie | de container. Geef de naam met het volledige pad
Inventarisgegevens van pad | Details van de bovenliggende | Container.Parent
Inventarisgegevens van pad | Details van een map voor elke virtuele machine | ((Folder)container).ChildEntity.Type
Inventarisgegevens van pad | Datacenter-details voor elke VM-map | ((Datacenter)container).VmFolder
Inventarisgegevens van pad | Datacenter-details voor elke Host-map | ((Datacenter)container).HostFolder
Inventarisgegevens van pad | Clusterdetails voor elke Host | ((ClusterComputeResource)container).Host)
Inventarisgegevens van pad | Host-details voor elke virtuele machine | ((HostSystem)container).Vm


## <a name="securing-the-collector-appliance"></a>Het Collector-apparaat beveiligen

U wordt aangeraden de volgende stappen uit voor het beveiligen van het Collector-apparaat:

- Geen delen of beheerderswachtwoorden misplace met niet-gemachtigde gebruikers.
- Sluit het apparaat als niet in gebruik.
- Plaats het apparaat in een beveiligd netwerk.
- Nadat de migratie is voltooid, verwijdert u het apparaat-exemplaar.
- Bovendien na de migratie ook verwijderen de schijf-back-upbestanden (vmdk's), omdat de schijven mogelijk vCenter-referenties in cache op deze.

## <a name="os-license-in-the-collector-vm"></a>OS-licentie in de collector-VM

De collector wordt geleverd met een licentie voor de evaluatie van Windows Server 2016 die 180 dagen geldig is. Als de evaluatieperiode is verlopen voor de collector-VM, is het aanbevolen om een nieuwe OVA downloaden en een nieuwe toepassing maken.

## <a name="updating-the-os-of-the-collector-vm"></a>Bijwerken van het besturingssysteem van de Collector-VM

Hoewel de collector een evaluatielicentie heeft die 180 dagen geldig is, moet het besturingssysteem van de collector continu worden bijgewerkt om automatisch afsluiten van de collector te voorkomen.

- Als de collector zestig dagen lang niet wordt bijgewerkt, wordt de machine automatisch afgesloten.
- Als er detectie wordt uitgevoerd, wordt de machine niet uitgeschakeld, ook niet als er zestig dagen voorbij zijn. De machine wordt uitgeschakeld als de detectie is voltooid.
- Als u de collector langer dan zestig dagen hebt gebruikt, wordt u aangeraden de machine up-to-date te houden door Windows Update continu uit te voeren.

## <a name="upgrading-the-collector-appliance-version"></a>Bijwerken van de Collector-apparaat-versie

U kunt de Collector upgraden naar de meest recente versie zonder het ova-bestand opnieuw te downloaden.

1. Download de [nieuwste vermeld upgradepakket](concepts-collector-upgrade.md)
2. Om ervoor te zorgen dat de gedownloade hotfix beveiligd is, open opdrachtvenster voor beheerders en voer de volgende opdracht voor het genereren van de hash voor het ZIP-bestand. De gegenereerde hash moet overeenkomen met de hash op basis van de specifieke versie vermeld:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    (example usage C:\>CertUtil -HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.14.zip SHA256)
3. Kopieer het zip-bestand naar de Azure Migrate collector virtuele machine (collector-apparaat).
4. Met de rechtermuisknop op het zip-bestand en selecteer Alles uitpakken.
5. Met de rechtermuisknop op Setup.ps1 en selecteer uitvoeren met PowerShell en volg de instructies op het scherm om de update te installeren.

## <a name="discovery-process"></a>Discovery-proces

Nadat het apparaat is ingesteld, kunt u de detectie kunt uitvoeren. Dit is hoe het werkt:

- U kunt een detectie uitvoeren door een bereik. Alle virtuele machines in het pad van de voorraad opgegeven vCenter wordt gedetecteerd.
    - U instellen een bereik op een tijdstip.
    - Het bereik kan zijn 1500 VM's of minder.
    - Het bereik mag een datacenter-, map- of ESXi-host.
- Nadat u verbinding met vCenter-Server, hebt u verbinding maken met een migration-project voor de verzameling op te geven.
- Virtuele machines zijn gedetecteerd en hun metagegevens en prestaties worden verzonden naar Azure. Deze acties zijn onderdeel van een taak.
    - Het Collector-apparaat wordt een specifieke Collector-ID die is permanent voor een bepaalde virtuele machine, ook detecties gegeven.
    - Een actieve taak krijgt een bepaalde sessie-ID. De ID voor elke taak wordt gewijzigd, en kan worden gebruikt voor het oplossen van problemen.

## <a name="next-steps"></a>Volgende stappen

[Instellen van een evaluatie voor on-premises VMware-machines](tutorial-assessment-vmware.md)
