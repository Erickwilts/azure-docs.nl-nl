---
title: Extensie-handler voor desired state Configuration van Azure | Microsoft Docs
description: Een Power shell DSC-configuratie op een Azure-VM uploaden en Toep assen met behulp van DSC-extensie
services: virtual-machines-windows
documentationcenter: ''
author: bobbytreed
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: windows
ms.workload: ''
ms.date: 03/26/2018
ms.author: robreed
ms.openlocfilehash: ee5a6c732bcb48cd347b8d87b95d2896d7230a08
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2019
ms.locfileid: "70092380"
---
# <a name="powershell-dsc-extension"></a>Power shell DSC-extensie

## <a name="overview"></a>Overzicht

De Power shell DSC-extensie voor Windows wordt gepubliceerd en ondersteund door micro soft. De uitbrei ding uploadt en past een Power shell DSC-configuratie op een virtuele machine van Azure toe. De DSC-extensie roept Power shell DSC aan om de ontvangen DSC-configuratie op de virtuele machine te regelen. Dit document bevat een overzicht van de ondersteunde platforms, configuraties en implementatie opties voor de DSC-extensie van de virtuele machine voor Windows.

## <a name="prerequisites"></a>Vereisten

### <a name="operating-system"></a>Besturingssysteem

De DSC-extensie ondersteunt de volgende besturings systemen

Windows Server 2019, Windows Server 2016, Windows Server 2012R2, Windows Server 2012, Windows Server 2008 R2 SP1, Windows-client 7/8.1/10

### <a name="internet-connectivity"></a>Internetconnectiviteit

De DSC-extensie voor Windows vereist dat de virtuele doel machine kan communiceren met Azure en de locatie van het configuratie pakket (zip-bestand) als deze wordt opgeslagen op een locatie buiten Azure. 

## <a name="extension-schema"></a>Extensieschema

De volgende JSON toont het schema voor het gedeelte instellingen van de DSC-uitbrei ding in een Azure Resource Manager sjabloon. 

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "Microsoft.Powershell.DSC",
  "apiVersion": "2018-10-01",
  "location": "<location>",
  "properties": {
    "publisher": "Microsoft.Powershell",
    "type": "DSC",
    "typeHandlerVersion": "2.77",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "wmfVersion": "latest",
        "configuration": {
            "url": "http://validURLToConfigLocation",
            "script": "ConfigurationScript.ps1",
            "function": "ConfigurationFunction"
        },
        "configurationArguments": {
            "argument1": "Value1",
            "argument2": "Value2"
        },
        "configurationData": {
            "url": "https://foo.psd1"
        },
        "privacy": {
            "dataCollection": "enable"
        },
        "advancedOptions": {
            "forcePullAndApply": false,
            "downloadMappings": {
                "specificDependencyKey": "https://myCustomDependencyLocation"
            }
        } 
    },
    "protectedSettings": {
        "configurationArguments": {
            "parameterOfTypePSCredential1": {
                "userName": "UsernameValue1",
                "password": "PasswordValue1"
            },
            "parameterOfTypePSCredential2": {
                "userName": "UsernameValue2",
                "password": "PasswordValue2"
            }
        },
        "configurationUrlSasToken": "?g!bber1sht0k3n",
        "configurationDataUrlSasToken": "?dataAcC355T0k3N"
    }
  }
}
```

### <a name="property-values"></a>Waarden van eigenschappen

| Name | Waarde / voorbeeld | Gegevenstype |
| ---- | ---- | ---- |
| apiVersion | 10-01-2018 | date |
| publisher | Microsoft.Powershell.DSC | string |
| type | DSC | string |
| typeHandlerVersion | 2.77 | int |

### <a name="settings-property-values"></a>Eigenschaps waarden van instellingen

| Name | Gegevenstype | Description
| ---- | ---- | ---- |
| settings.wmfVersion | string | Hiermee geeft u de versie op van het Windows Management Framework dat op uw virtuele machine moet worden geïnstalleerd. Als u deze eigenschap instelt op ' meest recente ', wordt de meest recente versie van WMF geïnstalleerd. De enige huidige mogelijke waarden voor deze eigenschap zijn ' 4,0 ', ' 5,0 ' en ' meest recent '. Deze mogelijke waarden zijn onderhevig aan updates. De standaard waarde is ' meest recent '. |
| settings.configuration.url | string | Hiermee geeft u de URL-locatie van waaruit u uw DSC-configuratie-zip-bestand wilt downloaden. Als voor de opgegeven URL een SAS-token voor toegang is vereist, moet u de eigenschap protectedSettings. configurationUrlSasToken instellen op de waarde van uw SAS-token. Deze eigenschap is vereist als instellingen. Configuration. script en/of settings. Configuration. functie zijn gedefinieerd.
| settings.configuration.script | string | Hiermee geeft u de bestands naam op van het script dat de definitie van uw DSC-configuratie bevat. Dit script moet zich in de hoofdmap van het zip-bestand bevindt dat is gedownload van de URL die is opgegeven door de eigenschap Configuration. URL. Deze eigenschap is vereist als instellingen. Configuration. URL en/of settings. Configuration. script zijn gedefinieerd.
| settings.configuration.function | string | Hiermee geeft u de naam van uw DSC-configuratie op. De configuratie met de naam moet zijn opgenomen in het script dat is gedefinieerd door Configuration. script. Deze eigenschap is vereist als instellingen. Configuration. URL en/of settings. Configuration. functie zijn gedefinieerd.
| settings.configurationArguments | Collection | Hiermee definieert u de para meters die u wilt door geven aan uw DSC-configuratie. Deze eigenschap wordt niet versleuteld.
| settings.configurationData.url | string | Hiermee geeft u de URL op waaruit uw pds1-bestand (configuratie gegevens) moet worden gedownload om te worden gebruikt als invoer voor uw DSC-configuratie. Als voor de opgegeven URL een SAS-token voor toegang is vereist, moet u de eigenschap protectedSettings. configurationDataUrlSasToken instellen op de waarde van uw SAS-token.
| Settings. privacy. dataEnabled | string | Hiermee wordt de verzameling telemetrie in-of uitgeschakeld. De enige mogelijke waarden voor deze eigenschap zijn ' Enable ', ' Disable ', ' of ' $null. Als u deze eigenschap leeg of null laat, wordt telemetrie ingeschakeld
| settings.advancedOptions.forcePullAndApply | Bool | Deze instelling is ontworpen om de ervaring van het werken met de uitbrei ding voor het registreren van knoop punten met Azure Automation DSC te verbeteren.  Als de waarde is `$true`, wacht de uitbrei ding op de eerste uitvoering van de configuratie die wordt opgehaald uit de service voordat het succes/de fout wordt geretourneerd.  Als de waarde is ingesteld op $false, verwijst de status die door de uitbrei ding wordt geretourneerd alleen naar of het knoop punt is geregistreerd met Azure Automation status configuratie is geslaagd en de knooppunt configuratie niet wordt uitgevoerd tijdens de registratie.
| settings.advancedOptions.downloadMappings | Collection | Definieert alternatieve locaties voor het downloaden van afhankelijkheden, zoals WMF en .NET

### <a name="protected-settings-property-values"></a>Eigenschaps waarden voor beveiligde instellingen

| Name | Gegevenstype | Description
| ---- | ---- | ---- |
| protectedSettings.configurationArguments | string | Hiermee definieert u de para meters die u wilt door geven aan uw DSC-configuratie. Deze eigenschap wordt versleuteld. |
| protectedSettings.configurationUrlSasToken | string | Hiermee geeft u het SAS-token voor toegang tot de URL die is gedefinieerd door Configuration. URL. Deze eigenschap wordt versleuteld. |
| protectedSettings.configurationDataUrlSasToken | string | Hiermee geeft u het SAS-token voor toegang tot de URL die is gedefinieerd door configurationData. URL. Deze eigenschap wordt versleuteld. |


## <a name="template-deployment"></a>Sjabloonimplementatie

Azure VM-extensies kunnen worden geïmplementeerd met Azure Resource Manager-sjablonen.
Sjablonen zijn ideaal bij het implementeren van een of meer virtuele machines waarvoor na de implementatie configuratie een vereiste is.
Een voor beeld van een resource manager-sjabloon die de DSC-extensie voor Windows bevat, kunt u vinden in de [Galerie van Azure Quick Start](https://github.com/Azure/azure-quickstart-templates/blob/master/101-automation-configuration/nested/provisionServer.json#L91).

## <a name="troubleshoot-and-support"></a>Problemen oplossen en ondersteuning

### <a name="troubleshoot"></a>Problemen oplossen

Gegevens over de status van extensie-implementaties kunnen worden opgehaald uit de Azure-portal en met behulp van de Azure CLI. Als wilt zien de implementatiestatus van extensies voor een bepaalde virtuele machine, voert u de volgende opdracht uit met de Azure CLI.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Het uitbreidings pakket wordt gedownload en geïmplementeerd op deze locatie op de Azure VM
```
C:\Packages\Plugins\{Extension_Name}\{Extension_Version}
```

Extensie status bestand bevat de substatussen en statussen geslaagd/fout, samen met de gedetailleerde fout en beschrijving voor elke uitbrei ding.
```
C:\Packages\Plugins\{Extension_Name}\{Extension_Version}\Status\{0}.Status  -> {0} being the sequence number
```

Uitvoer logboeken voor extensies worden vastgelegd in de volgende map:

```
C:\WindowsAzure\Logs\Plugins\{Extension_Name}\{Extension_Version}
```

### <a name="error-codes-and-their-meanings"></a>Foutcodes en hun betekenis

| Foutcode | Betekenis | Mogelijke actie |
| :---: | --- | --- |
| 1000 | Algemene fout | Het bericht voor deze fout wordt gegeven door de specifieke uitzonde ring in extensie logboeken |
| 52 | Fout bij installatie van extensie | Het bericht voor deze fout wordt gegeven door de specifieke uitzonde ring |
| 1002 | WMF-installatie fout | Fout bij het installeren van WMF. |
| 1004 | Ongeldig zip-pakket | Ongeldige zip; Fout bij het uitpakken van de zip |
| 1100 | Argument fout | Geeft een probleem aan met de invoer van de gebruiker. Het bericht voor de fout wordt gegeven door de specifieke uitzonde ring|


### <a name="support"></a>Ondersteuning

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op het [forums voor Azure MSDN en Stack Overflow](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer Get-ondersteuning. Voor meer informatie over het gebruik van ondersteuning voor Azure, de [Veelgestelde vragen over Microsoft Azure-ondersteuning](https://azure.microsoft.com/support/faq/).
