---
title: Beheer van de levenscyclus van de Azure Storage
description: Informatie over het maken lifecycle beleidsregels overgang ouderdom gegevens van warm naar koud en archief lagen.
services: storage
author: mhopkins-msft
ms.service: storage
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: mhopkins
ms.reviewer: yzheng
ms.subservice: common
ms.openlocfilehash: ce2559f62d29c7b062cfd1ad1dcb61146adfd91c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66001745"
---
# <a name="manage-the-azure-blob-storage-lifecycle"></a>De levenscyclus van de Azure Blob storage beheren

Gegevenssets hebben unieke Lifecycle-beleid. Vroeg in de levenscyclus, mensen toegang tot bepaalde gegevens vaak. Maar hebt u aanzienlijk als de gegevens van de leeftijd door de noodzaak voor toegang wordt geweigerd. Sommige gegevens in de cloud blijft niet-actieve en zelden worden gebruikt wanneer deze zijn opgeslagen. Sommige gegevens verlopen dagen of maanden na het maken, terwijl andere gegevenssets actief worden gelezen en gewijzigd gedurende hun levensduur. Azure Blob storage-levenscyclusbeheer biedt een uitgebreide, op regels gebaseerde beleid voor GPv2- en Blob storage-accounts. Gebruik het beleid aan uw gegevens naar de juiste laag overgang of verloopt aan het einde van de levenscyclus van de gegevens.

De levenscyclus van management-beleid kunt u:

- Overgang van blobs en een minder dynamische opslaglaag (' hot ' naar ' Cool ', ' hot ' als u wilt archiveren of koud naar archief) om te optimaliseren voor prestaties en kosten
- Verwijderen van blobs aan het einde van de levenscycli hiervan
- Definieer regels moet één keer per dag worden uitgevoerd op het niveau van de storage-account
- Regels toepassen op containers of een subset van blobs (met behulp van voorvoegsels als filters)

U hebt een scenario waarbij gegevens regelmatig toegang opgehaald tijdens de eerste fasen van de levenscyclus van, maar alleen af en toe na twee weken. Na de eerste maand, wordt de gegevensset zelden worden gebruikt. In dit scenario wordt er tijdens de eerste fasen hot storage aanbevolen. Koude opslag is het meest geschikt is voor incidentele toegang. Archiefopslag is de beste optie laag nadat de gegevens van de leeftijd meer dan een maand. Door het aanpassen van opslaglagen met betrekking tot de leeftijd van de gegevens, kunt u de minst dure opslagopties ontwerpen voor uw behoeften. Voor het bereiken van deze overgang, zijn beheerbeleidsregels lifecycle veroudering om gegevens te verplaatsen naar een minder dynamische laag beschikbaar.

## <a name="storage-account-support"></a>Ondersteuning voor Storage-account

De levenscyclus van management-beleid is beschikbaar bij zowel algemeen gebruik v2 (GPv2) accounts en Blob storage-accounts. In de Azure-portal, kunt u een bestaande account voor algemeen gebruik (GPv1) upgraden naar een GPv2-account. Zie [Overzicht van Azure-opslagaccount](../common/storage-account-overview.md) voor meer informatie over opslagaccounts.  

## <a name="pricing"></a>Prijzen

De levenscyclus van management-functie is gratis. Klanten betalen voor de bewerkingskosten van de normale voor de [Blobs weergeven](https://docs.microsoft.com/rest/api/storageservices/list-blobs) en [Blob-laag instellen](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) API-aanroepen. Verwijderbewerking is gratis. Zie voor meer informatie over prijzen [prijzen voor blok-Blob](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="regional-availability"></a>Regionale beschikbaarheid

De levenscyclus van management-functie is beschikbaar in alle globale Azure-regio's.

## <a name="add-or-remove-a-policy"></a>Toevoegen of verwijderen van een beleid

U kunt toevoegen, bewerken of verwijderen van een beleid met behulp van de volgende methoden:

* [Azure Portal](https://portal.azure.com)
* [Azure PowerShell](https://github.com/Azure/azure-powershell/releases)
* [Azure-CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)
* [REST API's](https://docs.microsoft.com/rest/api/storagerp/managementpolicies)

In dit artikel laat zien hoe beleid beheren met behulp van de portal en PowerShell-methoden.  

> [!NOTE]
> Als u firewallregels voor uw opslagaccount inschakelt, worden lifecycle management-aanvragen geblokkeerd. U kunt deze aanvragen blokkering opheffen door op te geven van uitzonderingen. De vereiste toegang zijn: `Logging,  Metrics,  AzureServices`. Zie voor meer informatie de sectie uitzonderingen in [firewalls en virtuele netwerken configureren](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions).

### <a name="azure-portal"></a>Azure Portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer **alle resources** en selecteer vervolgens uw opslagaccount.

3. Onder **Blob-Service**, selecteer **levenscyclusbeheer** weergeven of wijzigen van uw beleid.

4. De volgende JSON wordt een voorbeeld van een regel die kan worden geplakt in de **levenscyclusbeheer** portal-pagina.

   ```json
   {
     "rules": [
       {
         "name": "ruleFoo",
         "enabled": true,
         "type": "Lifecycle",
         "definition": {
           "filters": {
             "blobTypes": [ "blockBlob" ],
             "prefixMatch": [ "container1/foo" ]
           },
           "actions": {
             "baseBlob": {
               "tierToCool": { "daysAfterModificationGreaterThan": 30 },
               "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
               "delete": { "daysAfterModificationGreaterThan": 2555 }
             },
             "snapshot": {
               "delete": { "daysAfterCreationGreaterThan": 90 }
             }
           }
         }
       }
     ]
   }
   ```

5. Zie voor meer informatie over deze JSON-voorbeeld, de [beleid](#policy) en [regels](#rules) secties.

### <a name="powershell"></a>PowerShell

De volgende PowerShell-script kan worden gebruikt om een beleid toevoegen aan uw storage-account. De `$rgname` variabele worden geïnitialiseerd met de naam van uw resourcegroep. De `$accountName` variabele worden geïnitialiseerd met de naam van uw opslagaccount.

```powershell
#Install the latest module
Install-Module -Name Az -Repository PSGallery

#Initialize the following with your resource group and storage account names
$rgname = ""
$accountName = ""

#Create a new action object
$action = Add-AzStorageAccountManagementPolicyAction -BaseBlobAction Delete -daysAfterModificationGreaterThan 2555
$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -BaseBlobAction TierToArchive -daysAfterModificationGreaterThan 90
$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -BaseBlobAction TierToCool -daysAfterModificationGreaterThan 30
$action = Add-AzStorageAccountManagementPolicyAction -InputObject $action -SnapshotAction Delete -daysAfterCreationGreaterThan 90

# Create a new filter object
# PowerShell automatically sets BlobType as “blockblob” because it is the only available option currently
$filter = New-AzStorageAccountManagementPolicyFilter -PrefixMatch ab,cd

#Create a new rule object
#PowerShell automatically sets Type as “Lifecycle” because it is the only available option currently
$rule1 = New-AzStorageAccountManagementPolicyRule -Name Test -Action $action -Filter $filter

#Set the policy
$policy = Set-AzStorageAccountManagementPolicy -ResourceGroupName $rgname -StorageAccountName $accountName -Rule $rule1
```

## <a name="azure-resource-manager-template-with-lifecycle-management-policy"></a>Azure Resource Manager-sjabloon met lifecycle management-beleid

U kunt levenscyclusbeheer definiëren met behulp van Azure Resource Manager-sjablonen. Hier volgt een voorbeeldsjabloon voor het implementeren van een RA-GRS GPv2-opslagaccount met lifecycle management-beleid.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {
    "storageAccountName": "[uniqueString(resourceGroup().id)]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "location": "[resourceGroup().location]",
      "apiVersion": "2019-04-01",
      "sku": {
        "name": "Standard_RAGRS"
      },
      "kind": "StorageV2",
      "properties": {
        "networkAcls": {}
      }
    },
    {
      "name": "[concat(variables('storageAccountName'), '/default')]",
      "type": "Microsoft.Storage/storageAccounts/managementPolicies",
      "apiVersion": "2019-04-01",
      "dependsOn": [
        "[variables('storageAccountName')]"
      ],
      "properties": {
        "policy": {...}
      }
    }
  ],
  "outputs": {}
}
```

## <a name="policy"></a>Beleid

Een lifecycle management-beleid is een verzameling van regels in een JSON-document:

```json
{
  "rules": [
    {
      "name": "rule1",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {...}
    },
    {
      "name": "rule2",
      "type": "Lifecycle",
      "definition": {...}
    }
  ]
}
```

Een beleid is een verzameling van regels:

| Parameternaam | Parametertype | Opmerkingen |
|----------------|----------------|-------|
| `rules`        | Een matrix met regelobjecten | Ten minste één regel is in een beleid vereist. U kunt maximaal 100 regels definiëren in een beleid.|

Elke regel in het beleid heeft verschillende parameters:

| Parameternaam | Parametertype | Opmerkingen | Vereist |
|----------------|----------------|-------|----------|
| `name`         | String |Een de regelnaam mag maximaal 256 alfanumerieke tekens bestaan. De naam van regel is hoofdlettergevoelig.  Deze moet uniek zijn binnen een beleid. | True |
| `enabled`      | Boolean | Een optionele Booleaanse waarde waarmee een regel voor het tijdelijk worden uitgeschakeld. Standaardwaarde is ' True ' als deze optie niet is ingesteld. | False | 
| `type`         | Een enum-waarde | De huidige geldig type is `Lifecycle`. | True |
| `definition`   | Een object dat de levenscyclus van regel definieert | Elke definitie bestaat uit een filter en een actie. | True |

## <a name="rules"></a>Regels

De definitie van elke regel bevat een filterset en een actie-set. De [filteren set](#rule-filters) beperkt regelacties op een bepaalde set van objecten in een container of namen van objecten. De [actie set](#rule-actions) van toepassing is de laag of acties aan de gefilterde set objecten verwijderen.

### <a name="sample-rule"></a>Van voorbeeldregel

Het volgende voorbeeldregel filtert de account voor uitvoering van de acties op objecten die voorkomen in `container1` en beginnen met `foo`.  

- Laag-blob naar de koude laag 30 dagen na de laatste wijziging
- Laag-blob naar de laag archief 90 dagen na de laatste wijziging
- Het verwijderen van de blob 2,555 dagen (zeven jaar) na de laatste wijziging
- 90 dagen na het maken van momentopnamen voor blob-momentopnamen verwijderen

```json
{
  "rules": [
    {
      "name": "ruleFoo",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "container1/foo" ]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
            "delete": { "daysAfterModificationGreaterThan": 2555 }
          },
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}
```

### <a name="rule-filters"></a>Regel filters

Filters beperken regelacties tot een subset van de blobs in de storage-account. Als meer dan één filter wordt gedefinieerd, een logische `AND` wordt uitgevoerd op alle filters.

Filters omvatten:

| Naam van het filter | Filtertype | Opmerkingen | Is vereist |
|-------------|-------------|-------|-------------|
| blobTypes   | Een matrix met vooraf gedefinieerde enum-waarden. | De huidige versie ondersteunt `blockBlob`. | Ja |
| prefixMatch | Een matrix met tekenreeksen voor voorvoegsels worden aan. Elke regel kunt tot 10 voorvoegsels definiëren. Een voorvoegseltekenreeks moet beginnen met een containernaam. Bijvoorbeeld, als u wilt om alle blobs onder `https://myaccount.blob.core.windows.net/container1/foo/...` voor een regel, is het de prefixMatch `container1/foo`. | Als u geen prefixMatch definieert, wordt de regel geldt voor alle blobs in de storage-account.  | Nee |

### <a name="rule-actions"></a>Regelacties

Acties worden uitgevoerd voor de gefilterde blobs wanneer de uitvoering voorwaarde wordt voldaan.

Levenscyclusbeheer biedt ondersteuning voor opslaglagen en verwijderen van blobs en verwijderen van blob-momentopnamen. Ten minste één actie voor elke regel op blobs of blobschermopnamen definiëren.

| Bewerking        | Basis-Blob                                   | Momentopname      |
|---------------|---------------------------------------------|---------------|
| tierToCool    | Ondersteuning voor blobs dat zich momenteel in de warme laag         | Niet ondersteund |
| tierToArchive | Ondersteuning voor blobs dat zich momenteel in de opslaglaag hot of cool | Niet ondersteund |
| delete        | Ondersteund                                   | Ondersteund     |

>[!NOTE]
>Als u meer dan één actie op dezelfde blob definieert, is levensduurbeheer van de minst dure actie van toepassing op de blob. Bijvoorbeeld: actie `delete` is goedkoper dan actie `tierToArchive`. Actie `tierToArchive` is goedkoper dan actie `tierToCool`.

De uitvoering voorwaarden zijn gebaseerd op leeftijd. Basis-blobs gebruikt u de laatst gewijzigd om bij te houden van de leeftijd en blob-momentopnamen gebruiken de aanmaaktijd van de momentopname voor het bijhouden van leeftijd.

| Actie uitvoeren van voorwaarde             | Voorwaarde-waarde                          | Description                             |
|----------------------------------|------------------------------------------|-----------------------------------------|
| daysAfterModificationGreaterThan | Geheel getal-waarde die aangeeft van de leeftijd in dagen | De voorwaarde voor base blob-acties     |
| daysAfterCreationGreaterThan     | Geheel getal-waarde die aangeeft van de leeftijd in dagen | De voorwaarde voor acties van blob-momentopname |

## <a name="examples"></a>Voorbeelden

De volgende voorbeelden laten zien hoe u algemene scenario's met de levenscyclus van beleidsregels.

### <a name="move-aging-data-to-a-cooler-tier"></a>Ouderdom gegevens verplaatsen naar een minder dynamische laag

Dit voorbeeld laat zien hoe u voor de overgang van blok-blobs voorafgegaan door `container1/foo` of `container2/bar`. Het beleid overgangen blobs die nog niet zijn gewijzigd in meer dan 30 dagen op koude opslag en -blobs niet worden gewijzigd in 90 dagen voor de archive-laag:

```json
{
  "rules": [
    {
      "name": "agingRule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "container1/foo", "container2/bar" ]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}
```

### <a name="archive-data-at-ingest"></a>Archiveer gegevens in de opname

Sommige gegevens in de cloud blijft niet-actieve en zelden, als tot nooit geopend eenmaal opgeslagen. De volgende lifecycle-beleid is geconfigureerd voor gegevens archiveren zodra deze wordt opgenomen. In dit voorbeeld overgangen blok-blobs in de storage-account in container `archivecontainer` in een archive-laag. De overgang wordt bereikt door het uitvoeren van blobs 0 dagen na het tijdstip laatst gewijzigd:

```json
{
  "rules": [
    {
      "name": "archiveRule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "archivecontainer" ]
        },
        "actions": {
          "baseBlob": {
              "tierToArchive": { "daysAfterModificationGreaterThan": 0 }
          }
        }
      }
    }
  ]
}

```

### <a name="expire-data-based-on-age"></a>Gegevens op basis van leeftijd laten verlopen

Sommige gegevens wordt verlopen dagen of maanden na het maken van verwacht. U kunt een levenscyclus van management-beleid om te verlopen gegevens door verwijderen op basis van gegevens leeftijd configureren. Het volgende voorbeeld ziet een beleid dat Hiermee verwijdert u alle blok-blobs die ouder zijn dan 365 dagen.

```json
{
  "rules": [
    {
      "name": "expirationRule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ]
        },
        "actions": {
          "baseBlob": {
            "delete": { "daysAfterModificationGreaterThan": 365 }
          }
        }
      }
    }
  ]
}
```

### <a name="delete-old-snapshots"></a>Oude momentopnamen verwijderen

Voor gegevens die is gewijzigd en gedurende hun levensduur regelmatig geopend, worden momentopnamen vaak gebruikt voor het bijhouden van oudere versies van de gegevens. U kunt een beleid dat Hiermee verwijdert u de oude momentopnamen op basis van de leeftijd momentopname maken. De leeftijd van de momentopname wordt bepaald door het evalueren van de aanmaaktijd van de momentopname. Deze beleidsregel verwijderen blokkeren blob-momentopnamen binnen de container `activedata` die zijn 90 dagen of ouder nadat de momentopname is gemaakt.

```json
{
  "rules": [
    {
      "name": "snapshotRule",
      "enabled": true,
      "type": "Lifecycle",
    "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ],
          "prefixMatch": [ "activedata" ]
        },
        "actions": {
          "snapshot": {
            "delete": { "daysAfterCreationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}
```

## <a name="faq"></a>Veelgestelde vragen

**Ik heb een nieuw beleid, waarom de acties Voer niet direct gemaakt?**  
Het platform wordt één keer per dag uitgevoerd de lifecycle-beleid. Wanneer u een beleid configureert, duurt het tot 24 uur voor sommige acties uit te voeren voor de eerste keer.  

**Kan ik een blob gearchiveerde handmatig gereactiveerd, hoe ik voorkomen dat deze wordt verplaatst naar de laag archief was tijdelijk?**  
Wanneer een blob wordt verplaatst van een toegangslaag naar een andere laag voor access, wordt de tijd van laatste wijziging niet gewijzigd. Als u handmatig een gearchiveerde blob naar de warme laag rehydrate, zou deze worden teruggezet naar laag archiveren door lifecycle management engine. U kunt voorkomen dat deze door de regel die van invloed op deze blob tijdelijk uit te schakelen. U kunt de blob kopiëren naar een andere locatie als nodig is om te blijven bij de warme laag definitief. U kunt de regel opnieuw inschakelen wanneer de blob veilig teruggeplaatst kan in laag archiveren. 


## <a name="next-steps"></a>Volgende stappen

Informatie over het herstellen van gegevens na het per ongeluk verwijderen:

- [Voorlopig verwijderen voor Azure Storage-blobs](../blobs/storage-blob-soft-delete.md)
