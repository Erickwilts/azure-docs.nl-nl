---
title: Een Azure-abonnement overdragen naar een andere Azure AD-Directory (preview)
description: Meer informatie over het overdragen van een Azure-abonnement en bekende gerelateerde resources naar een andere Azure Active Directory-Directory (Azure AD).
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: how-to
ms.workload: identity
ms.date: 07/01/2020
ms.author: rolyon
ms.openlocfilehash: 0a504285b2d79ba1386bcd13dd72fc3faec202ff
ms.sourcegitcommit: 420c30c760caf5742ba2e71f18cfd7649d1ead8a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2020
ms.locfileid: "89055648"
---
# <a name="transfer-an-azure-subscription-to-a-different-azure-ad-directory-preview"></a>Een Azure-abonnement overdragen naar een andere Azure AD-Directory (preview)

> [!IMPORTANT]
> Volg deze stappen om een abonnement over te dragen naar een andere Azure AD-Directory, momenteel beschikbaar als open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Organisaties kunnen verschillende Azure-abonnementen hebben. Elk abonnement is gekoppeld aan een bepaalde Azure Active Directory-Directory (Azure AD). Als u het beheer eenvoudiger wilt maken, kunt u een abonnement overdragen naar een andere Azure AD-adres lijst. Wanneer u een abonnement overbrengt naar een andere Azure AD-adres lijst, worden sommige resources niet overgebracht naar de doel directory. Alle roltoewijzingen en aangepaste rollen in azure op rollen gebaseerd toegangs beheer (Azure RBAC) worden bijvoorbeeld **permanent** verwijderd uit de bronmap en worden niet overgebracht naar de doel directory.

In dit artikel worden de basis stappen beschreven die u kunt volgen om een abonnement over te dragen naar een andere Azure AD-Directory en een aantal resources na de overdracht opnieuw te maken.

> [!NOTE]
> Voor Azure CSP-abonnementen wordt het wijzigen van de Azure AD-Directory voor het abonnement niet ondersteund.

## <a name="overview"></a>Overzicht

Het overdragen van een Azure-abonnement naar een andere Azure AD-Directory is een complex proces dat zorgvuldig moet worden gepland en uitgevoerd. Voor veel Azure-Services zijn beveiligings-principals (identiteiten) vereist om normaal te werken of om zelfs andere Azure-resources te beheren. Dit artikel bevat een overzicht van de meeste Azure-Services die sterk afhankelijk zijn van beveiligings-principals, maar dat is niet volledig.

> [!IMPORTANT]
> In sommige scenario's is het overdragen van een abonnement mogelijk downtime nodig om het proces te volt ooien. Er is een zorgvuldige planning vereist om te beoordelen of downtime nodig is voor uw migratie.

In het volgende diagram ziet u de basis stappen die u moet volgen wanneer u een abonnement overbrengt naar een andere map.

1. Voorbereiden voor de overdracht

1. Eigendom van de facturering van een Azure-abonnement overdragen aan een ander account

1. Resources opnieuw maken in de doel directory, zoals roltoewijzingen, aangepaste rollen en beheerde identiteiten

    ![Abonnements diagram overdragen](./media/transfer-subscription/transfer-subscription.png)

### <a name="deciding-whether-to-transfer-a-subscription-to-a-different-directory"></a>Bepalen of u een abonnement wilt overdragen naar een andere map

Hier volgen enkele redenen waarom u een abonnement wilt overdragen:

- Als gevolg van een fusie of aanschaf van het bedrijf wilt u een aangeschaft abonnement in uw primaire Azure AD-Directory beheren.
- Iemand in uw organisatie heeft een abonnement gemaakt en u wilt het beheer op een bepaalde Azure AD-adres lijst consolideren.
- U hebt toepassingen die afhankelijk zijn van een bepaalde abonnements-ID of URL en het is niet eenvoudig om de configuratie of code van de toepassing te wijzigen.
- Een deel van uw bedrijf is opgesplitst in een afzonderlijk bedrijf en u moet een deel van uw resources verplaatsen naar een andere Azure AD-adres lijst.
- U wilt een deel van uw resources in een andere Azure AD-Directory beheren voor beveiligings isolatie doeleinden.

Voor het overdragen van een abonnement is downtime vereist om het proces te volt ooien. Afhankelijk van uw scenario is het wellicht beter om de resources opnieuw te maken en gegevens te kopiëren naar de doel directory en het-abonnement.

### <a name="understand-the-impact-of-transferring-a-subscription"></a>Meer informatie over de gevolgen van het overdragen van een abonnement

Verschillende Azure-resources hebben een afhankelijkheid van een abonnement of een directory. Afhankelijk van uw situatie vermeldt de volgende tabel de bekende impact van het overdragen van een abonnement. Door de stappen in dit artikel uit te voeren, kunt u enkele van de resources die bestonden vóór de overdracht van het abonnement opnieuw maken.

> [!IMPORTANT]
> In deze sectie vindt u de bekende Azure-Services of-bronnen die afhankelijk zijn van uw abonnement. Omdat resource typen in azure voortdurend worden doorlopend, zijn er mogelijk extra afhankelijkheden die hier niet worden weer gegeven. Dit kan leiden tot een ernstige wijziging in uw omgeving. 

| Service of resource | Beïnvloed | Herstel bare | Hebt u last van dit probleem? | Wat u kunt doen |
| --------- | --------- | --------- | --------- | --------- |
| Roltoewijzingen | Ja | Ja | [Lijst met roltoewijzingen weergeven](#save-all-role-assignments) | Alle roltoewijzingen worden definitief verwijderd. U moet gebruikers, groepen en service-principals toewijzen aan de bijbehorende objecten in de doel directory. U moet de roltoewijzingen opnieuw maken. |
| Aangepaste rollen | Ja | Ja | [Aangepaste rollen opvragen](#save-custom-roles) | Alle aangepaste rollen worden permanent verwijderd. U moet de aangepaste rollen en eventuele roltoewijzingen opnieuw maken. |
| Door het systeem toegewezen beheerde identiteiten | Ja | Ja | [Beheerde identiteiten weer geven](#list-role-assignments-for-managed-identities) | U moet de beheerde identiteiten uitschakelen en opnieuw inschakelen. U moet de roltoewijzingen opnieuw maken. |
| Door de gebruiker toegewezen beheerde identiteiten | Ja | Ja | [Beheerde identiteiten weer geven](#list-role-assignments-for-managed-identities) | U moet de beheerde identiteiten verwijderen, opnieuw maken en koppelen aan de juiste resource. U moet de roltoewijzingen opnieuw maken. |
| Azure Key Vault | Ja | Ja | [Key Vault toegangs beleid weer geven](#list-other-known-resources) | U moet de Tenant-ID die is gekoppeld aan de sleutel kluizen bijwerken. U moet een nieuw toegangs beleid verwijderen en toevoegen. |
| Azure SQL-data bases met Azure AD-verificatie integratie ingeschakeld | Ja | Nee | [Azure SQL-data bases controleren met Azure AD-verificatie](#list-azure-sql-databases-with-azure-ad-authentication) |  |  |
| Azure Storage en Azure Data Lake Storage Gen2 | Ja | Ja |  | U moet alle Acl's opnieuw maken. |
| Azure Data Lake Storage Gen1 | Ja | Ja |  | U moet alle Acl's opnieuw maken. |
| Azure Files | Ja | Ja |  | U moet alle Acl's opnieuw maken. |
| Azure File Sync | Ja | Ja |  |  |
| Azure Managed Disks | Ja | N.v.t. |  |  |
| Azure container Services voor Kubernetes | Ja | Ja |  |  |
| Azure Active Directory Domain Services | Ja | Nee |  |  |
| App-registraties | Ja | Ja |  |  |

> [!IMPORTANT]
> Als u versleuteling op rest gebruikt voor een bron, zoals een opslag account of een SQL database en de resource een afhankelijkheid heeft van een sleutel kluis die *niet* in het abonnement dat wordt overgedragen, wordt mogelijk een onherstelbare fout weer gegeven. Gebruik in dit geval een andere sleutel kluis of schakel de door de klant beheerde sleutels tijdelijk uit om een onherstelbare fout te voor komen.

## <a name="prerequisites"></a>Vereisten

Als u deze stappen wilt uitvoeren, hebt u het volgende nodig:

- [Bash in azure Cloud shell](/azure/cloud-shell/overview) of [Azure cli](https://docs.microsoft.com/cli/azure)
- Account beheerder van het abonnement dat u wilt overdragen in de bron directory
- De rol van [eigenaar](built-in-roles.md#owner) in de doel directory

## <a name="step-1-prepare-for-the-transfer"></a>Stap 1: voorbereiden voor de overdracht

### <a name="sign-in-to-source-directory"></a>Aanmelden bij de bron directory

1. Meld u als beheerder aan bij Azure.

1. Een lijst met uw abonnementen ophalen met de opdracht [AZ account list](/cli/azure/account#az-account-list) .

    ```azurecli
    az account list --output table
    ```

1. Gebruik [AZ account set](https://docs.microsoft.com/cli/azure/account#az-account-set) om het actieve abonnement in te stellen dat u wilt overdragen.

    ```azurecli
    az account set --subscription "Marketing"
    ```

### <a name="install-the-resource-graph-extension"></a>De resource-Graph-extensie installeren

 Met de resource grafiek extensie kunt u de opdracht [AZ Graph](https://docs.microsoft.com/cli/azure/ext/resource-graph/graph) gebruiken om te zoeken naar resources die worden beheerd door Azure Resource Manager. U gebruikt deze opdracht in latere stappen.

1. Gebruik [AZ Extension List](https://docs.microsoft.com/cli/azure/extension#az-extension-list) om te zien of de *resource-Graph-* extensie is geïnstalleerd.

    ```azurecli
    az extension list
    ```

1. Als dat niet het geval is, installeert u de *resource-Graph-* extensie.

    ```azurecli
    az extension add --name resource-graph
    ```

### <a name="save-all-role-assignments"></a>Alle roltoewijzingen opslaan

1. Gebruik [AZ Role Assignment List](https://docs.microsoft.com/cli/azure/role/assignment#az-role-assignment-list) om alle roltoewijzingen (inclusief overgenomen roltoewijzingen) weer te geven.

    Om het gemakkelijker te maken om de lijst te bekijken, kunt u de uitvoer exporteren als JSON, TSV of een tabel. Zie roltoewijzingen weer geven [met behulp van Azure RBAC en Azure cli](role-assignments-list-cli.md)voor meer informatie.

    ```azurecli
    az role assignment list --all --include-inherited --output json > roleassignments.json
    az role assignment list --all --include-inherited --output tsv > roleassignments.tsv
    az role assignment list --all --include-inherited --output table > roleassignments.txt
    ```

1. Sla de lijst met roltoewijzingen op.

    Wanneer u een abonnement overdraagt, worden alle roltoewijzingen **permanent** verwijderd zodat het belang rijk is om een kopie op te slaan.

1. Bekijk de lijst met roltoewijzingen. Er zijn mogelijk roltoewijzingen die u niet nodig hebt in de doel directory.

### <a name="save-custom-roles"></a>Aangepaste rollen opslaan

1. Gebruik de [lijst AZ Role definition](https://docs.microsoft.com/cli/azure/role/definition#az-role-definition-list) om uw aangepaste rollen weer te geven. Zie [aangepaste Azure-rollen maken of bijwerken met behulp van Azure cli](custom-roles-cli.md)voor meer informatie.

    ```azurecli
    az role definition list --custom-role-only true --output json --query '[].{roleName:roleName, roleType:roleType}'
    ```

1. Sla elke aangepaste rol die u nodig hebt in de doel directory op als een afzonderlijk JSON-bestand.

    ```azurecli
    az role definition list --name <custom_role_name> > customrolename.json
    ```

1. Kopieën maken van de bestanden van de aangepaste rol.

1. Wijzig elke kopie om de volgende indeling te gebruiken.

    U gebruikt deze bestanden later om de aangepaste rollen in de doel Directory opnieuw te maken.

    ```json
    {
      "Name": "",
      "Description": "",
      "Actions": [],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": []
    }
    ```

### <a name="determine-user-group-and-service-principal-mappings"></a>Toewijzingen van gebruikers, groepen en service-principals bepalen

1. Bepaal op basis van de lijst met roltoewijzingen de gebruikers, groepen en service-principals die u wilt toewijzen in de doel directory.

    U kunt het type Principal identificeren door te kijken naar de `principalType` eigenschap in elke roltoewijzing.

1. Maak, indien nodig, in de doel directory alle gebruikers, groepen of service-principals die u nodig hebt.

### <a name="list-role-assignments-for-managed-identities"></a>Roltoewijzingen voor beheerde identiteiten weer geven

Beheerde identiteiten worden niet bijgewerkt wanneer een abonnement wordt overgedragen naar een andere map. Als gevolg hiervan worden alle bestaande door het systeem toegewezen of door de gebruiker toegewezen beheerde identiteiten verbroken. Na de overdracht kunt u alle door het systeem toegewezen beheerde identiteiten opnieuw inschakelen. Voor door de gebruiker toegewezen beheerde identiteiten moet u deze opnieuw maken en koppelen aan de doel directory.

1. Bekijk de [lijst met Azure-Services die beheerde identiteiten ondersteunen](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md) om te weten waar u mogelijk beheerde identiteiten gebruikt.

1. Gebruik [AZ AD SP List](/cli/azure/identity?view=azure-cli-latest#az-identity-list) om uw door het systeem toegewezen en door de gebruiker toegewezen beheerde identiteiten weer te geven.

    ```azurecli
    az ad sp list --all --filter "servicePrincipalType eq 'ManagedIdentity'"
    ```

1. Bepaal in de lijst met beheerde identiteiten welke door het systeem toegewezen en aan de gebruiker zijn toegewezen. U kunt de volgende criteria gebruiken om het type te bepalen.

    | Criteria | Type beheerde identiteit |
    | --- | --- |
    | `alternativeNames` eigenschap bevat `isExplicit=False` | Systeem toegewezen |
    | `alternativeNames` de eigenschap omvat niet `isExplicit` | Systeem toegewezen |
    | `alternativeNames` eigenschap bevat `isExplicit=True` | Gebruiker toegewezen |

    U kunt ook [AZ ID List](https://docs.microsoft.com/cli/azure/identity#az-identity-list) gebruiken om alleen door de gebruiker toegewezen beheerde identiteiten te vermelden. Zie [een door de gebruiker toegewezen beheerde identiteit maken, weer geven of verwijderen met behulp van de Azure cli](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)voor meer informatie.

    ```azurecli
    az identity list
    ```

1. Een lijst met de `objectId` waarden voor uw beheerde identiteiten ophalen.

1. Zoek in de lijst met roltoewijzingen om te zien of er roltoewijzingen voor uw beheerde identiteiten zijn.

### <a name="list-key-vaults"></a>Sleutel kluizen weer geven

Wanneer u een sleutel kluis maakt, wordt deze automatisch gebonden aan de standaard Azure Active Directory Tenant-ID voor het abonnement waarin deze is gemaakt. Alle vermeldingen van het toegangsbeleid zijn ook gekoppeld aan deze tenant-ID. Zie [een Azure Key Vault naar een ander abonnement verplaatsen](../key-vault/general/move-subscription.md)voor meer informatie.

> [!IMPORTANT]
> Als u versleuteling op rest gebruikt voor een bron, zoals een opslag account of een SQL database en de resource een afhankelijkheid heeft van een sleutel kluis die *niet* in het abonnement dat wordt overgedragen, wordt mogelijk een onherstelbare fout weer gegeven. Gebruik in dit geval een andere sleutel kluis of schakel de door de klant beheerde sleutels tijdelijk uit om een onherstelbare fout te voor komen.

- Als u een sleutel kluis hebt, gebruikt u [AZ Key kluis show](https://docs.microsoft.com/cli/azure/keyvault#az-keyvault-show) om het toegangs beleid weer te geven. Zie [Key Vault verificatie bieden met een toegangscontrole beleid](../key-vault/key-vault-group-permissions-for-apps.md)voor meer informatie.

    ```azurecli
    az keyvault show --name MyKeyVault
    ```

### <a name="list-azure-sql-databases-with-azure-ad-authentication"></a>Azure SQL-data bases weer geven met Azure AD-verificatie

- Gebruik [AZ SQL Server AD-admin List](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#az-sql-server-ad-admin-list) en de [AZ Graph](https://docs.microsoft.com/cli/azure/ext/resource-graph/graph) extension om te zien of u Azure SQL-data bases gebruikt met Azure AD-verificatie. Zie [Configure and manage Azure Active Directory Authentication with SQL](../azure-sql/database/authentication-aad-configure.md)(Engelstalig) voor meer informatie.

    ```azurecli
    az sql server ad-admin list --ids $(az graph query -q 'resources | where type == "microsoft.sql/servers" | project id' -o tsv | cut -f1)
    ```

### <a name="list-acls"></a>Acl's weer geven

1. Als u Azure Data Lake Storage Gen1 gebruikt, vermeldt u de Acl's die worden toegepast op een bestand met behulp van de Azure Portal of Power shell.

1. Als u Azure Data Lake Storage Gen2 gebruikt, vermeldt u de Acl's die worden toegepast op een bestand met behulp van de Azure Portal of Power shell.

1. Als u Azure Files gebruikt, vermeldt u de Acl's die worden toegepast op een bestand.

### <a name="list-other-known-resources"></a>Andere bekende bronnen weer geven

1. Gebruik [AZ account show](https://docs.microsoft.com/cli/azure/account#az-account-show) om uw abonnements-id op te halen.

    ```azurecli
    subscriptionId=$(az account show --query id | sed -e 's/^"//' -e 's/"$//')
    ```

1. Gebruik de [AZ-grafiek](https://docs.microsoft.com/cli/azure/ext/resource-graph/graph) extensie om andere Azure-resources met bekende Azure AD-adreslijst afhankelijkheden weer te geven.

    ```azurecli
    az graph query -q \
    'resources | where type != "microsoft.azureactivedirectory/b2cdirectories" | where  identity <> "" or properties.tenantId <> "" or properties.encryptionSettingsCollection.enabled == true | project name, type, kind, identity, tenantId, properties.tenantId' \
    --subscriptions $subscriptionId --output table
    ```

## <a name="step-2-transfer-billing-ownership"></a>Stap 2: eigendom van de facturering overdragen

In deze stap brengt u het eigendom van het abonnement over van de bron directory naar de doel directory.

> [!WARNING]
> Wanneer u het eigendom van het abonnement overdraagt, worden alle roltoewijzingen in de bronmap **permanent** verwijderd en kunnen deze niet worden hersteld. U kunt niet meer terugkeren zodra u het eigendom van de facturering van het abonnement hebt overgedragen. Zorg ervoor dat u de vorige stappen hebt voltooid voordat u deze stap uitvoert.

1. Volg de stappen in het [eigendom van de facturering van een Azure-abonnement overdragen aan een ander account](../cost-management-billing/manage/billing-subscription-transfer.md). Als u het abonnement wilt overdragen naar een andere Azure AD-Directory, moet u het selectie vakje **Azure AD-Tenant abonnement** controleren.

1. Nadat u het eigendom hebt overgedragen, keert u terug naar dit artikel om de resources in de doel Directory opnieuw te maken.

## <a name="step-3-re-create-resources"></a>Stap 3: resources opnieuw maken

### <a name="sign-in-to-target-directory"></a>Aanmelden bij de doel directory

1. Meld u in de doel directory aan als de gebruiker die de overdrachts aanvraag heeft geaccepteerd.

    Alleen de gebruiker in het nieuwe account die de overdrachts aanvraag heeft geaccepteerd, heeft toegang tot de resources beheren.

1. Een lijst met uw abonnementen ophalen met de opdracht [AZ account list](https://docs.microsoft.com/cli/azure/account#az-account-list) .

    ```azurecli
    az account list --output table
    ```

1. Gebruik [AZ account set](https://docs.microsoft.com/cli/azure/account#az-account-set) om het actieve abonnement in te stellen dat u wilt gebruiken.

    ```azurecli
    az account set --subscription "Contoso"
    ```

### <a name="create-custom-roles"></a>Aangepast rollen maken
        
- Gebruik [AZ Role definition Create](https://docs.microsoft.com/cli/azure/role/definition#az-role-definition-create) om elke aangepaste rol te maken op basis van de bestanden die u eerder hebt gemaakt. Zie [aangepaste Azure-rollen maken of bijwerken met behulp van Azure cli](custom-roles-cli.md)voor meer informatie.

    ```azurecli
    az role definition create --role-definition <role_definition>
    ```

### <a name="create-role-assignments"></a>Maak roltoewijzingen

- Gebruik [AZ Role Assignment maken](https://docs.microsoft.com/cli/azure/role/assignment#az-role-assignment-create) om de roltoewijzingen voor gebruikers, groepen en service-principals te maken. Zie voor meer informatie [roltoewijzingen toevoegen of verwijderen met behulp van Azure RBAC en Azure cli](role-assignments-cli.md).

    ```azurecli
    az role assignment create --role <role_name_or_id> --assignee <assignee> --resource-group <resource_group>
    ```

### <a name="update-system-assigned-managed-identities"></a>Door het systeem toegewezen beheerde identiteiten bijwerken

1. Door het systeem toegewezen beheerde identiteiten uitschakelen en opnieuw inschakelen.

    | Azure-service | Meer informatie | 
    | --- | --- |
    | Virtuele machines | [Beheerde identiteiten configureren voor Azure-resources op een Azure-VM met behulp van Azure CLI](../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md#system-assigned-managed-identity) |
    | Virtuele-machineschaalsets | [Beheerde identiteiten configureren voor Azure-resources op een schaalset voor virtuele machines met behulp van Azure CLI](../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vmss.md#system-assigned-managed-identity) |
    | Overige services | [Services die beheerde identiteiten voor Azure-resources ondersteunen](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md) |

1. Gebruik [AZ Role Assignment maken](https://docs.microsoft.com/cli/azure/role/assignment#az-role-assignment-create) om de roltoewijzingen te maken voor door het systeem toegewezen beheerde identiteiten. Zie [een beheerde identiteits toegang toewijzen aan een resource met behulp van Azure cli](../active-directory/managed-identities-azure-resources/howto-assign-access-cli.md)voor meer informatie.

    ```azurecli
    az role assignment create --assignee <objectid> --role '<role_name_or_id>' --scope <scope>
    ```

### <a name="update-user-assigned-managed-identities"></a>Door de gebruiker toegewezen beheerde identiteiten bijwerken

1. Door de gebruiker toegewezen beheerde identiteiten verwijderen, opnieuw maken en koppelen.

    | Azure-service | Meer informatie | 
    | --- | --- |
    | Virtuele machines | [Beheerde identiteiten configureren voor Azure-resources op een Azure-VM met behulp van Azure CLI](../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md#user-assigned-managed-identity) |
    | Virtuele-machineschaalsets | [Beheerde identiteiten configureren voor Azure-resources op een schaalset voor virtuele machines met behulp van Azure CLI](../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vmss.md#user-assigned-managed-identity) |
    | Overige services | [Services die beheerde identiteiten voor Azure-resources ondersteunen](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)<br/>[Een door de gebruiker toegewezen beheerde identiteit maken, weer geven of verwijderen met behulp van de Azure CLI](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md) |

1. Gebruik [AZ Role Assignment maken](https://docs.microsoft.com/cli/azure/role/assignment#az-role-assignment-create) om de roltoewijzingen voor door de gebruiker toegewezen beheerde identiteiten te maken. Zie [een beheerde identiteits toegang toewijzen aan een resource met behulp van Azure cli](../active-directory/managed-identities-azure-resources/howto-assign-access-cli.md)voor meer informatie.

    ```azurecli
    az role assignment create --assignee <objectid> --role '<role_name_or_id>' --scope <scope>
    ```

### <a name="update-key-vaults"></a>Sleutel kluizen bijwerken

In deze sectie worden de basis stappen beschreven voor het bijwerken van uw sleutel kluizen. Zie [een Azure Key Vault naar een ander abonnement verplaatsen](../key-vault/general/move-subscription.md)voor meer informatie.

1. Werk de Tenant-ID die is gekoppeld aan alle bestaande sleutel kluizen in het abonnement bij naar de doel directory.

1. Alle bestaande vermeldingen van het toegangsbeleid te verwijderen.

1. Nieuwe beleids regels voor toegang toevoegen die zijn gekoppeld aan de doel directory.

### <a name="update-acls"></a>Acl's bijwerken

1. Als u Azure Data Lake Storage Gen1 gebruikt, wijst u de juiste Acl's toe. Zie gegevens beveiligen die zijn [opgeslagen in azure data Lake Storage gen1](../data-lake-store/data-lake-store-secure-data.md)voor meer informatie.

1. Als u Azure Data Lake Storage Gen2 gebruikt, wijst u de juiste Acl's toe. Zie [Toegangsbeheer in Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-access-control.md) voor meer informatie.

1. Als u Azure Files gebruikt, wijst u de juiste Acl's toe.

### <a name="review-other-security-methods"></a>Andere beveiligings methoden controleren

Hoewel roltoewijzingen tijdens de overdracht worden verwijderd, hebben gebruikers in het oorspronkelijke eigenaars account mogelijk nog steeds toegang tot het abonnement via andere beveiligings methoden, waaronder:

- Toegangssleutels voor services zoals Storage.
- [Beheer certificaten](../cloud-services/cloud-services-certs-create.md) die de gebruikers beheerder toegang verlenen tot de abonnements bronnen.
- Referenties voor externe toegang voor services zoals Azure Virtual Machines.

Als u de toegang van gebruikers in de bronmap wilt verwijderen zodat ze geen toegang hebben in de doel directory, kunt u overwegen om de referenties te roteren. Totdat de referenties zijn bijgewerkt, blijven gebruikers na de overdracht toegang hebben.

1. De toegangs sleutels voor het opslag account draaien. Zie [Toegangssleutels voor een opslagaccount beheren](../storage/common/storage-account-keys-manage.md) voor meer informatie.

1. Als u toegangs sleutels gebruikt voor andere services, zoals Azure SQL Database of Azure Service Bus berichten, roteert u de toegangs toetsen.

1. Voor bronnen die gebruikmaken van geheimen, opent u de instellingen voor de resource en werkt u het geheim bij.

1. Werk het certificaat bij voor bronnen die gebruikmaken van certificaten.

## <a name="next-steps"></a>Volgende stappen

- [Eigendom van de facturering van een Azure-abonnement overdragen aan een ander account](../cost-management-billing/manage/billing-subscription-transfer.md)
- [Overdracht van Azure-abonnementen tussen abonnees en CSP's](../cost-management-billing/manage/transfer-subscriptions-subscribers-csp.md)
- [Een Azure-abonnement aan uw Azure Active Directory-tenant toevoegen of koppelen](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)
