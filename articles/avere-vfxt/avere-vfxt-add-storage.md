---
title: Avere vFXT opslag - Azure configureren
description: Het opslagsysteem van een back-end toevoegen aan uw vFXT Avere voor Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/29/2019
ms.author: v-erkell
ms.openlocfilehash: 6d35d5cdeafb80a36f910d71393802a3affb4df8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60516027"
---
# <a name="configure-storage"></a>Opslag configureren

Deze stap stelt u een back-end-opslagsysteem voor uw cluster vFXT.

> [!TIP]
> Als u een nieuwe Azure Blob-container, samen met het Avere vFXT cluster hebt gemaakt, die container is al ingesteld voor gebruik en u hoeft geen opslag toevoegen.

Volg deze instructies als u een nieuwe Blob-container niet met uw cluster hebt gemaakt, of als u wilt toevoegen van een extra hardware of een systeem voor cloud-gebaseerde opslag.

Er zijn twee belangrijke taken:

1. [Maken van een filer core](#create-a-core-filer), die uw cluster vFXT verbindt met een bestaand storage-systeem of een Azure Storage-Account.

1. [Een naamruimte koppelingspunten](#create-a-junction), waarmee wordt gedefinieerd met het pad dat clients gekoppeld.

Deze stappen gebruiken het Avere van het Configuratiescherm. Lezen [toegang tot het cluster vFXT](avere-vfxt-cluster-gui.md) voor meer informatie over het gebruik ervan.

## <a name="create-a-core-filer"></a>Maken van een filer core

'Core filer' is een term vFXT voor een systeem voor back-end-opslag. De opslag kan een hardware-NAS-apparaat, zoals NetApp of Isilon, of een archief-object in de cloud kan zijn. Meer informatie over core filter kan worden gevonden [cluster in de Avere instellingen handleiding](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/settings_overview.html#managing-core-filers).

Als u wilt toevoegen een filer core, kies een van de twee belangrijkste querytypen core filter:

  * [NAS-core, filer](#nas-core-filer) -wordt beschreven hoe u een NAS core filer toevoegen 
  * [Azure Storage cloud core filer](#azure-storage-cloud-core-filer) -wordt beschreven hoe u een Azure Storage-account toevoegen als een cloud core filer

### <a name="nas-core-filer"></a>NAS core filer

Een NAS core filer mag een on-premises NetApp of Isilon, of een NAS-eindpunt in de cloud. Het opslagsysteem moet een betrouwbare snelle verbinding met het Avere vFXT cluster - bijvoorbeeld een 1 Gbps ExpressRoute-verbinding (niet een VPN) - en deze moet de toegang tot de hoofdmap van het cluster geven tot de NAS-uitvoer die wordt gebruikt.

De volgende stappen uit toevoegen een NAS core filer:

1. Klik in het Configuratiescherm Avere op de **instellingen** tabblad bovenaan.

1. Klik op **Core Filer** > **Core-filter beheren** aan de linkerkant.

1. Klik op **Create**.

   ![Schermafbeelding van de pagina met een cursor boven de knop maken toevoegen, nieuwe filer core](media/avere-vfxt-add-core-filer-start.png)

1. Vul de vereiste gegevens in de wizard: 

   * Naam van uw filer core.
   * Geef een volledig gekwalificeerde domeinnaam (FQDN) indien beschikbaar. Anders, Geef een IP-adres of de hostnaam die wordt omgezet naar uw filer core.
   * Kies uw filer-klasse in de lijst. Als u niet zeker, kies **andere**.

     ![Schermafbeelding van de pagina met de naam van de filer core en de volledig gekwalificeerde domeinnaam toevoegen, nieuwe filer core](media/avere-vfxt-add-core-filer.png)
  
   * Klik op **volgende** en kiest u een cache-beleid. 
   * Klik op **Filer toevoegen**.
   * Raadpleeg voor meer informatie gedetailleerde, [filer toe te voegen een nieuwe NAS core](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/new_core_filer_nas.html) cluster in de Avere instellingen handleiding.

Vervolgens gaat u verder met [maken van een verbinding](#create-a-junction).  

### <a name="azure-storage-cloud-core-filer"></a>Azure Storage cloud core filer

Voor het gebruik van Azure Blob-opslag als opslagruimte voor back-end van uw cluster vFXT, moet u een lege container om toe te voegen als een filer core.

> [!TIP] 
> Als u ervoor kiest om te maken van een blob-container op hetzelfde moment als die u het Avere vFXT-cluster maakt, de sjabloon voor de implementatie of het script maakt u een opslagcontainer, gedefinieerd als een filer core en wordt de verbinding van de naamruimte als onderdeel van het maken van een cluster vFXT gemaakt. De sjabloon maakt ook een storage-service-eindpunt in het virtuele netwerk van het cluster. 

Blob-opslag toevoegen aan uw cluster, moet u deze taken:

* Een opslagaccount maken (stap 1 hieronder)
* Maak een lege blobcontainer (stap 2-3)
* De toegangssleutel voor opslag toevoegen als een cloudreferentie voor het cluster vFXT (stap 4-6)
* De Blob-container toevoegen als een filer core voor het cluster vFXT (stap 7-9)
* Een naamruimte koppelingspunten die clients gebruiken voor het koppelen van de filer core ([maken van een verbinding](#create-a-junction), hetzelfde zijn voor de hardware-en cloudopslag)

Blob-opslag toevoegen nadat het cluster is gemaakt, volg deze stappen. 

1. Maak een opslagaccount voor algemeen gebruik V2 met deze instellingen:

   * **Abonnement** : hetzelfde als het cluster vFXT
   * **Resourcegroep** : hetzelfde als de clustergroep vFXT (optioneel)
   * **Locatie** : hetzelfde als het cluster vFXT
   * **Prestaties** - standaard (Premium storage wordt niet ondersteund)
   * **Soort account** -General-purpose V2 (StorageV2)
   * **Replicatie** -lokaal redundante opslag (LRS)
   * **Toegangslaag** - ' hot '
   * **Veilige overdracht vereist** -Schakel deze optie (niet-standaard waarde)
   * **Virtuele netwerken** - niet vereist

   U kunt de Azure-portal gebruiken of klik op de knop 'Implementeren naar Azure' hieronder.

   [![om de storage-account maken](media/deploytoazure.png)](https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAvere%2Fmaster%2Fsrc%2Fvfxt%2Fstorageaccount%2Fazuredeploy.json)

1. Nadat het account is gemaakt, Ga naar pagina van het opslagaccount.

   ![Nieuwe storage-account in Azure portal](media/avere-vfxt-new-storage-acct.png)

1. Een blob-container maken door te klikken op **Blobs** op de overzichtspagina en klik vervolgens op **+ Container**. Een willekeurige containernaam gebruiken en zorg ervoor dat toegang is ingesteld op **persoonlijke**.

   ![Pagina van de opslag-blobs met geen bestaande containers](media/avere-vfxt-blob-no-container.png)

1. De Azure Storage-accountsleutel ophalen door te klikken op **toegangssleutels** onder **instellingen**:

   ![Azure-portal GUI voor het kopiëren van de sleutel](media/avere-vfxt-copy-storage-key.png) 

1. Open het Configuratiescherm Avere voor uw cluster. Klik op **instellingen**en open vervolgens **Cluster** > **Cloudreferenties** in het navigatiedeelvenster links. Klik op de pagina Cloudreferenties **referentie toevoegen**.

   ![Klik op de knop referentie toevoegen op de pagina van de configuratie van Cloud-referenties](media/avere-vfxt-new-credential-button.png)

1. Vul in de volgende informatie om te maken van een referentie voor de cloud core filer: 

   | Veld | Value |
   | --- | --- |
   | Referentienaam | een beschrijvende naam |
   | Servicetype | (Selecteer Azure-toegangssleutel voor opslag) |
   | Tenant | De naam van opslagaccount |
   | Abonnement | abonnements-ID |
   | Toegangssleutel voor opslag | Azure storage-accountsleutel (in de vorige stap hebt gekopieerd) | 

   Klik op **Indienen**.

   ![Cloud-referentie-formulier in het Configuratiescherm Avere voltooid](media/avere-vfxt-new-credential-submit.png)

1. Maak vervolgens de filer core. Klik in de linkerkant van het Configuratiescherm Avere **Core Filer** >  **Core-filter beheren**. 

1. Klik op de **maken** knop op de **Core-filter beheren** instellingenpagina.

1. Vul de wizard:

   * Selecteer filer type **Cloud**. 
   * Naam van de nieuwe core filer en klikt u op **volgende**.
   * Accepteer het standaardbeleid voor cache en de derde pagina blijven.
   * In **servicetype**, kiest u **Azure storage**. 
   * Kies de referentie op die eerder hebt gemaakt.
   * Stel **Bucket inhoud** naar **leeg zijn**
   * Wijziging **certificaatverificatie** naar **uitgeschakeld**
   * Wijziging **compressiemethode** naar **None**  
   * Klik op **volgende**.
   * Geef op de vierde pagina, de naam van de container in **Bucketnaam** als *storage_account_name*/*container_name*.
   * (Optioneel) Stel **versleutelingstype** naar **geen**.  Azure Storage is standaard versleuteld.
   * Klik op **Filer toevoegen**.

   Lees voor meer informatie gedetailleerde, [toe te voegen een nieuwe cloud core filer](<https://azure.github.io/Avere/legacy/ops_guide/4_7/html/new_core_filer_cloud.html>) in de handleiding voor Avere cluster. 

De pagina wordt vernieuwd, of u kunt de pagina om uw nieuwe core filer weer te vernieuwen.

Vervolgens, moet u [maken van een verbinding](#create-a-junction).

## <a name="create-a-junction"></a>Een verbinding maken

Een verbinding is een pad dat u voor clients maakt. Clients koppelen van het pad en binnenkomen op de bestemming die u kiest.

Bijvoorbeeld, kunt u `/avere/files` om toe te wijzen om uw core NetApp filer `/vol0/data` exporteren en de `/project/resources` submap.

Meer informatie over koppelingen vindt u de [naamruimte-sectie van de handleiding voor cluster Avere](https://azure.github.io/Avere/legacy/ops_guide/4_7/html/gui_namespace.html).

Volg deze stappen in de interface Avere Configuratiescherm-instellingen:

* Klik op **VServer** > **Namespace** in de linkerbovenhoek.
* Geef een naamruimte pad begint met / (schuine streep), bijvoorbeeld ``/avere/data``.
* Kies uw filer core.
* Kies de core filer exporteren.
* Klik op **volgende**.

  ![Schermafbeelding van de pagina 'Nieuwe verbinding toevoegen' met de velden voor de verbinding, core filer en export is voltooid](media/avere-vfxt-add-junction.png)

De verbinding wordt weergegeven na een paar seconden. Aanvullende koppelingen maken naar behoefte.

Nadat de verbinding is gemaakt, clients kunnen [koppelen van het cluster van Avere vFXT](avere-vfxt-mount-clients.md) voor toegang tot het bestandssysteem.
