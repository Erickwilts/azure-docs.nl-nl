---
title: Azure Key Vault beheerd opslagaccount - CLI
description: Opslagaccountsleutels bieden een naadloze integratie tussen Azure Key Vault en toegang tot de sleutel op basis van Azure Storage-Account.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
author: msmbaldwin
ms.author: mbaldwin
manager: barbkess
ms.date: 03/01/2019
ms.openlocfilehash: 190375700f65cf2d3ea47335a646562eb46b2d49
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65232557"
---
# <a name="azure-key-vault-managed-storage-account---cli"></a>Azure Key Vault beheerd opslagaccount - CLI

> [!NOTE]
> [Azure storage-integratie met Azure Active Directory (Azure AD)] is van Microsoft cloud-gebaseerde identiteits- en toegangsbeheer management-service. Azure AD-integratie is beschikbaar voor de services Blob en wachtrij. (https://docs.microsoft.com/azure/storage/common/storage-auth-aad). Het is raadzaam om met behulp van Azure AD voor verificatie en autorisatie, waarmee u OAuth2-tokens gebaseerde toegang tot Azure-opslag, net als Azure Key Vault. Hiermee kunt u naar:
> - Verifiëren van uw clienttoepassing met behulp van een toepassings- of -identiteit, in plaats van de referenties van het opslagaccount. 
> - Gebruik een [Azure AD beheerde identiteit](/azure/active-directory/managed-identities-azure-resources/) bij het uitvoeren op Azure. Beheerde identiteiten verwijderen de noodzaak voor clientverificatie samen en opslaan van referenties in of met uw toepassing.
> - Gebruik rollen gebaseerd toegangsbeheer (RBAC) voor het beheren van autorisatie, die ook wordt ondersteund door Key Vault.

Een [Azure storage-account](/azure/storage/storage-create-storage-account) maakt gebruik van een referentie die uit een accountnaam en een sleutel bestaat. De sleutel is gegenereerd, en fungeert als een 'wachtwoord' in plaats van een cryptografische sleutel. Key Vault deze opslagaccountsleutels kunt beheren door op te slaan als [Key Vault-geheimen](/azure/key-vault/about-keys-secrets-and-certificates#key-vault-secrets). 

## <a name="overview"></a>Overzicht

De Key Vault beheerde functie voert verschillende functies voor beheer namens uw storage-account:

- Een lijst met (synchronisatie) sleutels met een Azure storage-account.
- Hiermee wordt opnieuw gegenereerd (gedraaid) periodiek de sleutels.
- Hiermee beheert u de sleutels voor storage-accounts en klassieke opslagaccounts.
- Sleutelwaarden worden nooit geretourneerd in antwoord op de oproepende functie.

Wanneer u de belangrijkste functie van beheerde opslag account gebruikt:

- **Alleen toestaan Key Vault voor het beheren van uw opslagaccountsleutels.** Niet proberen ze om zelf te beheren, zoals u leiden tot met de processen van de Sleutelkluis problemen zult.
- **Opslagaccountsleutels worden beheerd door meer dan een Key Vault-object niet toestaan**.
- **Niet handmatig uw storage-accountsleutels opnieuw genereren**. Het is raadzaam dat u ze via Key Vault genereren.
- Key Vault voor het beheren van uw storage-account waarin wordt gevraagd, kan worden gedaan door een UPN voor nu en niet een Service-Principal

Het volgende voorbeeld ziet u hoe u Key Vault voor het beheren van uw storage-accountsleutels toestaan.

> [!IMPORTANT]
> Een Azure AD-tenant biedt elke geregistreerde toepassing met een  **[service-principal](/azure/active-directory/develop/developer-glossary#service-principal-object)**, die fungeert als de identiteit van de toepassing. Toepassings-ID van de service-principal wordt gebruikt wanneer u deze machtiging voor toegang tot andere Azure-resources via op rollen gebaseerd toegangsbeheer (RBAC). Omdat de Key Vault is een Microsoft-toepassing, het vooraf geregistreerd in alle Azure AD-tenants onder dezelfde toepassings-ID, binnen elk Azure-cloud:
> - Toepassings-ID in Azure government-cloud Azure AD-tenants gebruiken `7e7c393b-45d0-48b1-a35e-2905ddf8183c`.
> - Azure AD-tenants in de openbare cloud van Azure en alle andere toepassings-ID gebruiken `cfa8b339-82a2-471a-a3c9-0fc0be7a4093`.

<a name="prerequisites"></a>Vereisten
--------------
1. [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) Azure CLI installeren   
2. [Een Opslagaccount maken](https://azure.microsoft.com/services/storage/)
    - Volg de stappen in dit [document](https://docs.microsoft.com/azure/storage/) om een opslagaccount te maken  
    - **Richtlijnen voor de naamgeving:** Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten.        
      
<a name="step-by-step-instructions-on-how-to-use-key-vault-to-manage-storage-account-keys"></a>Voor stap door stapsgewijze instructies over het gebruik van Key Vault voor het beheren van Opslagaccountsleutels
--------------------------------------------------------------------------------
Conceptueel gezien zijn van de lijst met stappen die worden gevolgd
- Krijgen we eerst een opslagaccount (bestaand)
- We vervolgens ophalen een (vooraf bestaande) sleutelkluis
- We vervolgens een Key Vault beheerde storage-account toevoegen aan de kluis Key1 als de actieve sleutel en met een punt opnieuw genereren van 180 dagen in te stellen
- Ten slotte stellen we een storage-context voor het opgegeven opslagaccount, met Key1

In de onderstaande instructies volgen, zijn we Key Vault toewijzen als een service operatormachtigingen hebben voor uw storage-account

> [!NOTE]
> . Houd er rekening mee dat zodra u hebt ingesteld met Azure Key Vault beheerde opslag accountsleutels ze moet **geen** meer behalve gewijzigd via Key Vault. Storage-accountsleutels betekent dat Key Vault beheren de opslagaccountsleutel draaien beheerd

> [!IMPORTANT]
> Een Azure AD-tenant biedt elke geregistreerde toepassing met een  **[service-principal](/azure/active-directory/develop/developer-glossary#service-principal-object)**, die fungeert als de identiteit van de toepassing. Toepassings-ID van de service-principal wordt gebruikt wanneer u deze machtiging voor toegang tot andere Azure-resources via op rollen gebaseerd toegangsbeheer (RBAC). Omdat de Key Vault is een Microsoft-toepassing, het vooraf geregistreerd in alle Azure AD-tenants onder dezelfde toepassings-ID, binnen elk Azure-cloud:
> - Toepassings-ID in Azure government-cloud Azure AD-tenants gebruiken `7e7c393b-45d0-48b1-a35e-2905ddf8183c`.
> - Azure AD-tenants in de openbare cloud van Azure en alle andere toepassings-ID gebruiken `cfa8b339-82a2-471a-a3c9-0fc0be7a4093`.

> - Op dit moment kunt u gebruikers-Principal te vragen van Key Vault voor het beheren van een storage-account en niet een Service-Principal


1. Na het maken van een storage-account de volgende opdracht om op te halen van de resource-ID van het opslagaccount dat wilt u beheren

    ```
    az storage account show -n storageaccountname 
    ```
    ID-veld van de resultaten van de bovenstaande opdracht ziet eruit als hieronder kopiëren
    ```
    /subscriptions/0xxxxxx-4310-48d9-b5ca-0xxxxxxxxxx/resourceGroups/ResourceGroup/providers/Microsoft.Storage/storageAccounts/StorageAccountName
    ```
            "objectId": "93c27d83-f79b-4cb2-8dd4-4aa716542e74"
    
2. RBAC-rol 'Storage Account Key Operator Service rol' toewijzen aan Key Vault, beperken van het toegangsbereik tot uw opslagaccount. Gebruik voor een klassiek opslagaccount 'Klassieke Storage Account Key Operator Service rol.'
    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id <ObjectIdOfKeyVault> --scope 93c27d83-f79b-4cb2-8dd4-4aa716542e74
    ```
    
    '93c27d83-f79b-4cb2-8dd4-4aa716542e74' is de Object-ID voor Key Vault in openbare Cloud. Als u Zie de Object-ID voor Key Vault in nationale clouds de bovenstaande belangrijk sectie
    
3. Maken van een Key Vault beheerde Storage-Account.     <br /><br />
   Hieronder, zijn we een periode van 90 dagen voor opnieuw regenereren instellen. Na 90 dagen wordt Key Vault opnieuw genereren 'key1' en de actieve sleutel uit 'key2' naar 'key1' wisselen. Er wordt nu Key1 markeren als de actieve sleutel. 
   
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key1 --auto-regenerate-key --regeneration-period P90D --resource-id <Id-of-storage-account>
    ```

<a name="step-by-step-instructions-on-how-to-use-key-vault-to-create-and-generate-sas-tokens"></a>Voor stap door stapsgewijze instructies over het gebruik van Key Vault maken en genereren van SAS-tokens
--------------------------------------------------------------------------------
Key Vault voor het genereren van SAS (Shared Access Signature)-tokens kunt u ook vragen. Een shared access signature biedt gedelegeerde toegang tot resources in uw opslagaccount. Met een SAS kunt u clients toegang verlenen tot bronnen in uw opslagaccount zonder het delen van sleutels van uw account. De belangrijkste reden voor het gebruik van handtekeningen voor gedeelde toegang in uw toepassingen is dat een SAS een veilige manier is voor het delen van uw opslagresources zonder dat uw accountsleutels in gevaar komen.

Nadat u hebt voltooid, kunnen de volgende opdrachten om te vragen van Key Vault voor het genereren van SAS-tokens voor u in de u bovenstaande stappen uitvoeren. 

De lijst met zaken die kan worden uitgevoerd in de onderstaande stappen zijn
- Hiermee stelt u een account-SAS-definitie met de naam `<YourSASDefinitionName>` op een Key Vault-beheerd opslagaccount `<YourStorageAccountName>` in uw kluis `<VaultName>`. 
- Hiermee maakt u een account-SAS-token voor de services Blob, bestand, tabel en wachtrij voor het woord brontypen Service, Container en Object, met alle machtigingen via https en met de opgegeven begin- en einddatums
- Hiermee stelt u een Key Vault beheerde opslag SAS-definitie in de kluis, met de uri van de sjabloon als de SAS-token gemaakt hierboven, van SAS-type 'account' en geldige N dagen
- De werkelijke toegangstoken opgehaald uit de Key Vault-geheim dat overeenkomt met de SAS-definitie

1. In deze stap maken we de definitie van een SAS. Zodra deze SAS-definitie is gemaakt, kunt u Key Vault voor het genereren van meer SAS-tokens voor u te vragen. Deze bewerking moet de machtiging voor opslag/setsas.

```
$sastoken = az storage account generate-sas --expiry 2020-01-01 --permissions rw --resource-types sco --services bfqt --https-only --account-name storageacct --account-key 00000000
```
Ziet u meer informatie over de bovenstaande bewerking [hier](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-generate-sas)

Wanneer deze bewerking wordt uitgevoerd, ziet u uitvoer die lijkt op, zoals hieronder wordt weergegeven. Kopiëren

```console
   "se=2020-01-01&sp=***"
```

1. In deze stap gebruiken we de uitvoer ($sasToken) gegenereerd hierboven om de definitie van een SAS te maken. Lees voor meer documentatie [hier](https://docs.microsoft.com/cli/azure/keyvault/storage/sas-definition?view=azure-cli-latest#required-parameters)   

```
az keyvault storage sas-definition create --vault-name <YourVaultName> --account-name <YourStorageAccountName> -n <NameOfSasDefinitionYouWantToGive> --validity-period P2D --sas-type account --template-uri $sastoken
```
                        

 > [!NOTE] 
 > In het geval de gebruiker beschikt niet over machtigingen voor het opslagaccount, krijgen we eerst de Object-Id van de gebruiker

 ```
 az ad user show --upn-or-object-id "developer@contoso.com"

 az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover     purge restore set setsas update
 ```
    
## <a name="fetch-sas-tokens-in-code"></a>SAS-tokens in de code ophalen

In deze sectie bespreken we hoe u bewerkingen op uw storage-account kunt doen met het ophalen van [SAS-tokens](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) uit Key Vault

In de onderstaande sectie we laten zien hoe u voor het ophalen van SAS-tokens wanneer de definitie van een SAS zoals hierboven is gemaakt.

> [!NOTE]
>   Er zijn 3 manieren om te verifiëren naar Key Vault, omdat u kunt lezen de [basisconcepten](key-vault-whatis.md#basic-concepts)
> - Met behulp van de beheerde Service-identiteit (ten zeerste aanbevolen)
> - Met behulp van Service-Principal en certificaat 
> - Met Service-Principal en het wachtwoord (niet aanbevolen)

```cs
// Once you have a security token from one of the above methods, then create KeyVaultClient with vault credentials
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(securityToken));

// Get a SAS token for our storage from Key Vault. SecretUri is of the format https://<VaultName>.vault.azure.net/secrets/<ExamplePassword>
var sasToken = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using the SAS token.
var accountSasCredential = new StorageCredentials(sasToken.Value);

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client.
var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null);

var blobClientWithSas = accountWithSas.CreateCloudBlobClient();
```

Als uw SAS-token bijna verlopen is, klikt u vervolgens u het SAS-token opnieuw ophalen uit de Sleutelkluis en de code bijwerken

```cs
// If your SAS token is about to expire, get the SAS Token again from Key Vault and update it.
sasToken = await kv.GetSecretAsync("SecretUri");
accountSasCredential.UpdateSASToken(sasToken);
```

### <a name="relevant-azure-cli-commands"></a>Relevante Azure CLI-opdrachten

[Azure Storage van de CLI-opdrachten](https://docs.microsoft.com/cli/azure/keyvault/storage?view=azure-cli-latest)

## <a name="see-also"></a>Zie ook

- [Informatie over sleutels, geheimen en certificaten](https://docs.microsoft.com/rest/api/keyvault/)
- [Key Vault-teamblog](https://blogs.technet.microsoft.com/kv/)
