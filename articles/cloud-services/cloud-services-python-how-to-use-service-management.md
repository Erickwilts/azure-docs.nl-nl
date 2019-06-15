---
title: Gebruik de servicebeheer-API (Python) - overzicht van de functies
description: Meer informatie over het uitvoeren via een programma algemene servicebeheertaken vanuit Python.
services: cloud-services
documentationcenter: python
author: lmazuel
manager: wpickett
editor: ''
ms.assetid: 61538ec0-1536-4a7e-ae89-95967fe35d73
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/30/2017
ms.author: lmazuel
ms.openlocfilehash: 573c6d3ded8fea58e0c9ba1afa7da2d8dd0fce91
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60525530"
---
# <a name="use-service-management-from-python"></a>Gebruik de servicebeheer van Python
Deze handleiding laat zien hoe u via een programma uitvoeren algemene servicebeheertaken vanuit Python. De **ServiceManagementService** klasse de [Azure SDK voor Python](https://github.com/Azure/azure-sdk-for-python) ondersteunt programmatische toegang tot veel van de service-gerelateerde functies die beschikbaar is in de [Azure Portal][management-portal]. U kunt deze functionaliteit gebruiken om te maken, bijwerken en verwijderen van cloudservices, implementaties, data management-services en virtuele machines. Deze functionaliteit is handig bij het bouwen van toepassingen waarvoor programmatische toegang tot de service management.

## <a name="WhatIs"> </a>Wat is servicebeheer?
De Azure Service Management-API biedt programmatische toegang tot veel van de service management-functionaliteit beschikbaar via de [Azure-portal][management-portal]. U kunt de Azure SDK voor Python gebruiken om uw cloudservices en storage-accounts te beheren.

Voor het gebruik van de servicebeheer-API, moet u [maken van een Azure-account](https://azure.microsoft.com/pricing/free-trial/).

## <a name="Concepts"> </a>Concepten
De Azure SDK voor Python loopt de [Service Management-API][svc-mgmt-rest-api], dit is een REST-API. Alle API-bewerkingen worden uitgevoerd over SSL en wederzijds geverifieerd met behulp van de X.509 v3-certificatien. De management-service kan worden gebruikt een service die wordt uitgevoerd in Azure. Deze kunnen ook worden geopend rechtstreeks via Internet vanuit elke toepassing die u kunt een HTTPS-aanvraag verzenden en ontvangen van een HTTPS-antwoord.

## <a name="Installation"> </a>installatie
Alle functies die worden beschreven in dit artikel zijn beschikbaar in de `azure-servicemanagement-legacy` pakket dat u installeren kunt via pip. Zie voor meer informatie over de installatie (bijvoorbeeld als u geen ervaring met Python) [installeren Python en de Azure SDK](../python-how-to-install.md).

## <a name="Connect"> </a>Verbinding maken met servicebeheer
Voor verbinding met het service management-eindpunt, moet u uw Azure-abonnement-ID en een geldig beheercertificaat. U vindt uw abonnements-ID via de [Azure-portal][management-portal].

> [!NOTE]
> U kunt nu certificaten die zijn gemaakt met OpenSSL wanneer u gebruikmaakt van Windows gebruiken. Python punt 2.7.4 of hoger is vereist. We raden u dat u OpenSSL in plaats van .pfx gebruiken, omdat de ondersteuning voor pfx-certificaten waarschijnlijk in de toekomst worden verwijderd.
>
>

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Certificaten voor op Windows, Mac en Linux (OpenSSL)
U kunt [OpenSSL](https://www.openssl.org/) om uw beheercertificaat te maken. U moet twee certificaten, één voor de server maken (een `.cer` bestand) en één voor de client (een `.pem` bestand). Maakt de `.pem` bestand, worden uitgevoerd:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Maakt de `.cer` certificaat, uitvoeren:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Zie voor meer informatie over Azure-certificaten, [overzicht van certificaten voor Azure Cloud Services](cloud-services-certs-create.md). Zie de documentatie op voor een volledige beschrijving van OpenSSL-parameters, [ https://www.openssl.org/docs/apps/openssl.html ](https://www.openssl.org/docs/apps/openssl.html).

Nadat u deze bestanden hebt gemaakt, uploadt u de `.cer` bestand naar Azure. In de [Azure-portal][management-portal]op de **instellingen** tabblad **uploaden**. Houd er rekening mee opslaglocatie de `.pem` bestand.

Nadat u uw abonnements-ID hebt verkregen, maak een certificaat en upload het `.cer` van het bestand in Azure, verbinding maken met het eindpunt voor Azure. Verbinding maken met het doorgeven van de abonnements-ID en het pad naar de `.pem` van het bestand in **ServiceManagementService**.

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

In het voorgaande voorbeeld `sms` is een **ServiceManagementService** object. De **ServiceManagementService** klasse is de primaire klasse die wordt gebruikt voor het beheren van Azure-services.

### <a name="management-certificates-on-windows-makecert"></a>Certificaten voor op Windows (MakeCert)
U kunt een zelfondertekend certificaat maken op uw computer met behulp van `makecert.exe`. Open een **Visual Studio-opdrachtprompt** als een **beheerder** en gebruikt u de volgende opdracht, vervangt *AzureCertificate* met de naam van het certificaat die u wilt gebruiken:

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

De opdracht maakt u de `.cer` bestands- en installeert deze in de **persoonlijke** certificaatarchief. Zie voor meer informatie, [overzicht van certificaten voor Azure Cloud Services](cloud-services-certs-create.md).

Nadat u het certificaat hebt gemaakt, uploadt u de `.cer` bestand naar Azure. In de [Azure-portal][management-portal]op de **instellingen** tabblad **uploaden**.

Nadat u uw abonnements-ID hebt verkregen, maak een certificaat en upload het `.cer` van het bestand in Azure, verbinding maken met het eindpunt voor Azure. Verbinding maken met het doorgeven van de abonnements-ID en de locatie van het certificaat in uw **persoonlijke** certificaatarchief **ServiceManagementService** (vervang weer *AzureCertificate* met de naam van uw certificaat).

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

In het voorgaande voorbeeld `sms` is een **ServiceManagementService** object. De **ServiceManagementService** klasse is de primaire klasse die wordt gebruikt voor het beheren van Azure-services.

## <a name="ListAvailableLocations"> </a>Lijst met beschikbare locaties
Als u de locaties die beschikbaar zijn voor het hosten van services, gebruikt u de **lijst\_locaties** methode.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

Wanneer u een cloudservice of storage-service maakt, moet u een geldige locatie opgeven. De **lijst\_locaties** methode retourneert altijd een bijgewerkte lijst van de momenteel beschikbare locaties. Op dit moment zijn de beschikbare locaties:

* Europa -west
* Europa - noord
* Azië - zuidoost
* Azië - oost
* US - centraal
* US - noord-centraal
* US - zuid-centraal
* US - west
* US - oost
* Japan - oost
* Japan - west
* Brazilië - zuid
* Australië - oost
* Australië - zuidoost

## <a name="CreateCloudService"> </a>Een cloudservice maken
Wanneer u een toepassing maakt en deze in Azure uitvoert, de code en configuratie die samen zijn met de naam een Azure [cloudservice][cloud service]. (Dit was bekend als een *gehoste service* in eerdere versies van Azure.) U kunt de **maken\_gehost\_service** methode voor het maken van een nieuwe gehoste service. De service maken door de naam van een gehoste service (dit moet uniek zijn in Azure), een label (automatisch gecodeerd naar base64), een beschrijving en een locatie te leveren.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

U kunt een lijst alle gehoste services voor uw abonnement met de **lijst\_gehost\_services** methode.

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Voor informatie over een bepaalde gehoste service, geeft u de naam van de gehoste service voor de **ophalen\_gehost\_service\_eigenschappen** methode.

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

Nadat u een cloudservice hebt gemaakt, implementeert u uw code in de service met de **maken\_implementatie** methode.

## <a name="DeleteCloudService"> </a>Een cloudservice verwijderen
U kunt een cloudservice verwijderen door de naam van de service aan de **verwijderen\_gehost\_service** methode.

    sms.delete_hosted_service('myhostedservice')

Voordat u een service verwijderen kunt, moeten alle implementaties voor de service eerst worden verwijderd. Zie voor meer informatie, [verwijderen van een implementatie](#DeleteDeployment).

## <a name="DeleteDeployment"> </a>Een implementatie verwijderen
Als u wilt verwijderen van een implementatie, gebruikt u de **verwijderen\_implementatie** methode. Het volgende voorbeeld ziet u hoe u een implementatie met de naam verwijderen `v1`:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"> </a>Een storage-service maken
Een [opslagservice](../storage/common/storage-create-storage-account.md) krijgt u toegang tot Azure [blobs](../storage/blobs/storage-python-how-to-use-blob-storage.md), [tabellen](../cosmos-db/table-storage-how-to-use-python.md), en [wachtrijen](../storage/queues/storage-python-how-to-use-queue-storage.md). Voor het maken van een storage-service, moet u een naam voor de service (tussen 3 en 24 tekens in kleine letters en uniek zijn binnen Azure). U moet ook een beschrijving, een label (maximaal 100 tekens, automatisch naar base64-codering) en een locatie. Het volgende voorbeeld laat zien hoe een storage-service maken door een locatie op te geven:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

In het voorgaande voorbeeld wordt de status van de **maken\_opslag\_account** bewerking kan worden opgehaald door door te geven het resultaat geretourneerd door **maken\_opslag\_ account** naar de **ophalen\_bewerking\_status** methode. 

U kunt een lijst uw storage-accounts en hun eigenschappen met de **lijst\_opslag\_accounts** methode.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"> </a>Verwijderen van een storage-service
Als u wilt verwijderen van een storage-service, geeft u de naam van de storage-service naar de **verwijderen\_opslag\_account** methode. Een storage-service verwijdert, worden alle gegevens die zijn opgeslagen in de service (blobs, tabellen en wachtrijen).

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"> </a>Lijst met beschikbare besturingssystemen
Als u de besturingssystemen die beschikbaar zijn voor het hosten van services, gebruikt u de **lijst\_operationele\_systemen** methode.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

U kunt ook kunt u de **lijst\_operationele\_system\_families** methode, die groepen van de besturingssystemen worden uitgevoerd door-familie.

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"> </a>Maken van een installatiekopie van besturingssysteem
Een installatiekopie van besturingssysteem toevoegen aan de opslagplaats voor installatiekopieën, gebruikt u de **toevoegen\_os\_installatiekopie** methode.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Als u de installatiekopieën van besturingssystemen die beschikbaar zijn, gebruikt u de **lijst\_os\_installatiekopieën** methode. Dit omvat alle platform-afbeeldingen en gebruikersafbeeldingen.

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <a name="DeleteVMImage"> </a>Een installatiekopie van het besturingssysteem verwijderen
Als u wilt verwijderen van de gebruikersinstallatiekopie van een, gebruikt u de **verwijderen\_os\_installatiekopie** methode.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"> </a>Een virtuele machine maken
Voor het maken van een virtuele machine, moet u eerst maken een [cloudservice](#CreateCloudService). Maak vervolgens de implementatie van de virtuele machine met behulp van de **maken\_virtuele\_machine\_implementatie** methode.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where the VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <a name="DeleteVM"> </a>Een virtuele machine verwijderen
Als u wilt verwijderen van een virtuele machine, u eerst de implementatie verwijderen met behulp van de **verwijderen\_implementatie** methode.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

De cloudservice kan vervolgens worden verwijderd met behulp van de **verwijderen\_gehost\_service** methode.

    sms.delete_hosted_service(service_name='myvm')

## <a name="create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>Een virtuele machine maken van een vastgelegde VM-installatiekopie
Voor het vastleggen van een VM-installatiekopie, u roept u eerst de **vastleggen\_vm\_installatiekopie** methode.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace the below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

Om ervoor te zorgen dat u de installatiekopie is vastgelegd, gebruikt u de **lijst\_vm\_installatiekopieën** API. Zorg ervoor dat de afbeelding wordt weergegeven in de resultaten.

    images = sms.list_vm_images()

Als u wilt maken ten slotte de virtuele machine met behulp van de vastgelegde installatiekopie, gebruikt u de **maken\_virtuele\_machine\_implementatie** methode als voorheen, maar deze keer doorgeven in de vm_image_name.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

Zie voor meer informatie over het vastleggen van een virtuele Linux-machine in het klassieke implementatiemodel, [een Linux-machine vastleggen](../virtual-machines/linux/classic/capture-image-classic.md).

Zie voor meer informatie over het vastleggen van een Windows-machine in het klassieke implementatiemodel, [Windows-machine vastleggen](../virtual-machines/windows/classic/capture-image-classic.md).

## <a name="What's Next"> </a>Volgende stappen
Nu dat u de basisprincipes van servicebeheer hebt geleerd, kunt u toegang tot de [volledige API-referentiedocumentatie voor de Azure Python SDK](https://azure-sdk-for-python.readthedocs.org/) en complexe taken eenvoudig voor het beheren van uw Python-toepassing uit te voeren.

Raadpleeg het [Python Developer Center](https://azure.microsoft.com/develop/python/) voor meer informatie.

[What is service management?]: #WhatIs
[Concepts]: #Concepts
[Connect to service management]: #Connect
[List available locations]: #ListAvailableLocations
[Create a cloud service]: #CreateCloudService
[Delete a cloud service]: #DeleteCloudService
[Create a deployment]: #CreateDeployment
[Update a deployment]: #UpdateDeployment
[Move deployments between staging and production]: #MoveDeployments
[Delete a deployment]: #DeleteDeployment
[Create a storage service]: #CreateStorageService
[Delete a storage service]: #DeleteStorageService
[List available operating systems]: #ListOperatingSystems
[Create an operating system image]: #CreateVMImage
[Delete an operating system image]: #DeleteVMImage
[Create a virtual machine]: #CreateVM
[Delete a virtual machine]: #DeleteVM
[Next steps]: #NextSteps
[management-portal]: https://portal.azure.com/
[svc-mgmt-rest-api]: https://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[cloud service]:/azure/cloud-services/
