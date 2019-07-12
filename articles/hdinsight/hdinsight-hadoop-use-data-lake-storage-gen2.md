---
title: Azure Data Lake Storage Gen2 gebruiken met Azure HDInsight-clusters
description: Informatie over het gebruik van Azure Data Lake Storage Gen2 met Azure HDInsight-clusters.
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: hrasheed
ms.openlocfilehash: dd639ae7e05309ab4528eb460ce38550db4cffe1
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67670765"
---
# <a name="use-azure-data-lake-storage-gen2-with-azure-hdinsight-clusters"></a>Azure Data Lake Storage Gen2 gebruiken met Azure HDInsight-clusters

Azure Data Lake Storage Gen2 is een cloudopslagservice die is toegewezen aan de analyse van big data, gebouwd op Azure Blob-opslag. Data Lake Storage Gen2 combineert de mogelijkheden van Azure Blob storage en Azure Data Lake Storage Gen1. De resulterende service biedt functies van Azure Data Lake Storage Gen1, zoals de semantiek van het bestandssysteem, directory-bestand op het niveau en beveiliging en schaalbaarheid, samen met de lage kosten, gelaagde opslag, hoge beschikbaarheid en herstel na noodgevallen mogelijkheden uit Azure Blob storage.

## <a name="data-lake-storage-gen2-availability"></a>Data Lake Storage Gen2 beschikbaarheid

Data Lake Storage Gen2 is beschikbaar als een opslagoptie voor vrijwel alle Azure HDInsight-clustertypen als zowel standaard als een extra opslagaccount. HBase, kan echter slechts één Gen2 van Data Lake Storage-account hebben.

> [!Note]  
> Nadat u Data Lake Storage Gen2 als uw **primaire opslagtype**, u kunt een Gen1 van Data Lake Storage-account niet selecteren als extra opslag.

## <a name="create-a-cluster-with-data-lake-storage-gen2-through-the-azure-portal"></a>Een cluster maken met Data Lake Storage Gen2 via Azure portal

Volg deze stappen om een Data Lake Storage Gen2-account te configureren voor het maken van een HDInsight-cluster dat gebruik maakt van Data Lake Storage Gen2 voor opslag.

### <a name="create-a-user-assigned-managed-identity"></a>Een door de gebruiker toegewezen beheerde identiteit maken

Maak een beheerde identiteit voor de gebruiker toegewezen, als u er nog geen hebt. Zie [maken, list, delete of wijs een rol aan een gebruiker toegewezen beheerde identiteit met Azure portal](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md#create-a-user-assigned-managed-identity). Zie voor meer informatie over de manier waarop beheerde identiteiten werk in Azure HDInsight, [beheerde identiteiten in Azure HDInsight](hdinsight-managed-identities.md).

![Een door de gebruiker toegewezen beheerde identiteit maken](./media/hdinsight-hadoop-use-data-lake-storage-gen2/create-user-assigned-managed-identity-portal.png)

### <a name="create-a-data-lake-storage-gen2-account"></a>Een Data Lake Storage Gen2-account maken

Een Azure Data Lake Storage Gen2-opslagaccount maken. Zorg ervoor dat de **hiërarchische naamruimte** optie is ingeschakeld. Zie voor meer informatie [Snelstart: Maken van een storage-account van Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-quickstart-create-account.md).

![Schermafbeelding van de opslagaccount is gemaakt in Azure portal](./media/hdinsight-hadoop-data-lake-storage-gen2/azure-data-lake-storage-account-create-advanced.png)

### <a name="set-up-permissions-for-the-managed-identity-on-the-data-lake-storage-gen2-account"></a>Instellen van machtigingen voor de identiteit van de beheerde op het Data Lake Storage Gen2-account

Toewijzen van de beheerde identiteit op de **Gegevenseigenaar voor Blob Storage** -rol op het storage-account. Zie voor meer informatie, [beheren toegangsrechten tot Azure BLOB Storage en Queue gegevens met RBAC (Preview)](../storage/common/storage-auth-aad-rbac.md).

1. In de [Azure-portal](https://portal.azure.com), gaat u naar uw storage-account.
1. Selecteer uw storage-account en selecteer vervolgens **toegangsbeheer (IAM)** om de instellingen voor toegangsbeheer voor het account weer te geven. Selecteer de **roltoewijzingen** tabblad om te bekijken van de lijst van roltoewijzingen.
    
    ![Schermafbeelding van de instellingen voor toegangsbeheer opslag](./media/hdinsight-hadoop-data-lake-storage-gen2/portal-access-control.png)
    
1. Selecteer de **+ roltoewijzing toevoegen** knop een nieuwe rol toe te voegen.
1. In de **roltoewijzing toevoegen** venster de **Gegevenseigenaar voor Blob Storage** rol. Selecteer het abonnement dat de beheerde identiteits- en storage-account is. Vervolgens zoekt u naar de gebruiker toegewezen beheerde identiteit die u eerder hebt gemaakt. Selecteer ten slotte de beheerde identiteit, en wordt weergegeven onder **geselecteerde leden**.
    
    ![Schermopname die laat zien hoe u een RBAC-rol toewijzen](./media/hdinsight-hadoop-data-lake-storage-gen2/add-rbac-role3.png)
    
1. Selecteer **Opslaan**. De gebruiker toegewezen identiteit die u hebt geselecteerd, wordt nu weergegeven onder de geselecteerde rol.
1. Nadat de initiële installatie is voltooid, kunt u een cluster via de portal kunt maken. Het cluster moet zich in dezelfde Azure-regio als het opslagaccount. In de **opslag** sectie van het cluster maken-menu selecteert u de volgende opties:
        
    * Voor **primaire opslagtype**, selecteer **Azure Data Lake Storage Gen2**.
    * Onder **selecteert u een opslagaccount**, zoek en selecteer het zojuist gemaakte opslagaccount voor Data Lake Storage Gen2.
        
        ![Instellingen voor de opslag voor het gebruik van Data Lake Storage Gen2 met Azure HDInsight](./media/hdinsight-hadoop-data-lake-storage-gen2/primary-storage-type-adls-gen2.png)
    
    * Onder **identiteit**, selecteert u het juiste abonnement en het zojuist gemaakte gebruiker toegewezen identiteit wordt beheerd.
        
        ![Instellingen van de identiteit voor het gebruik van Data Lake Storage Gen2 met Azure HDInsight](./media/hdinsight-hadoop-data-lake-storage-gen2/managed-identity-cluster-creation.png)
        
> [!Note]
> Als u wilt toevoegen een secundaire Gen2 van Data Lake Storage-account, op het niveau van de storage-account, moet u gewoon de beheerde identiteit eerder hebt gemaakt met de nieuwe Data Lake Storage Gen2 storage-account, die u wilt toevoegen toewijzen. Houd er rekening mee dat een secundaire Gen2 van Data Lake Storage-account via de blade 'extra opslagaccounts' toe te voegen op HDInsight wordt niet ondersteund. 

## <a name="create-a-cluster-with-data-lake-storage-gen2-through-the-azure-cli"></a>Een cluster maken met Data Lake Storage Gen2 via de Azure CLI

U kunt [een voorbeeldbestand van de sjabloon downloaden](https://github.com/Azure-Samples/hdinsight-data-lake-storage-gen2-templates/blob/master/hdinsight-adls-gen2-template.json) en [een parameters voorbeeldbestand downloaden](https://github.com/Azure-Samples/hdinsight-data-lake-storage-gen2-templates/blob/master/parameters.json). Voordat u de sjabloon en het onderstaande codefragment Azure CLI gebruikt, kunt u de volgende tijdelijke aanduidingen vervangen door de juiste waarden:

| Tijdelijke aanduiding | Description |
|---|---|
| `<SUBSCRIPTION_ID>` | De ID van uw Azure-abonnement |
| `<RESOURCEGROUPNAME>` | De resourcegroep waar u het nieuwe cluster en storage-account gemaakt. |
| `<MANAGEDIDENTITYNAME>` | De naam van de beheerde identiteit die machtigingen voor uw account voor Azure Data Lake Storage Gen2 wordt gegeven. |
| `<STORAGEACCOUNTNAME>` | Het nieuwe Azure Data Lake Storage Gen2-account dat wordt gemaakt. |
| `<CLUSTERNAME>` | De naam van uw HDInsight-cluster. |
| `<PASSWORD>` | Uw gekozen wachtwoord voor aanmelding bij het cluster via SSH, evenals de Ambari-dashboard. |

Het onderstaande codefragment wordt de volgende eerste stappen uit:

1. Logboeken in uw Azure-account.
1. Hiermee stelt u het actieve abonnement waarin de bewerkingen maken wordt uitgevoerd.
1. Hiermee maakt u een nieuwe resourcegroep voor de nieuwe implementaties uit te schakelen. 
1. Hiermee maakt u een beheerde identiteit voor de gebruiker toegewezen.
1. Toevoegen van een uitbreiding naar de Azure CLI met functies voor Data Lake Storage Gen2.
1. Hiermee maakt u een nieuw Data Lake Storage Gen2-account met behulp van de `--hierarchical-namespace true` vlag. 

```azurecli
az login
az account set --subscription <SUBSCRIPTION_ID>

# Create resource group
az group create --name <RESOURCEGROUPNAME> --location eastus

# Create managed identity
az identity create -g <RESOURCEGROUPNAME> -n <MANAGEDIDENTITYNAME>

az extension add --name storage-preview

az storage account create --name <STORAGEACCOUNTNAME> \
    --resource-group <RESOURCEGROUPNAME> \
    --location eastus --sku Standard_LRS \
    --kind StorageV2 --hierarchical-namespace true
```

Vervolgens Meld u aan de portal. De nieuwe gebruiker toegewezen beheerde identiteit toevoegen aan de **Gegevensbijdrager voor Blob** -rol op het storage-account, zoals beschreven in stap 3 onder [met behulp van de Azure-portal](hdinsight-hadoop-use-data-lake-storage-gen2.md).

Nadat u de rol voor de gebruiker toegewezen beheerde identiteit hebt toegewezen, moet u de sjabloon implementeren met behulp van het volgende codefragment.

```azurecli
az group deployment create --name HDInsightADLSGen2Deployment \
    --resource-group <RESOURCEGROUPNAME> \
    --template-file hdinsight-adls-gen2-template.json \
    --parameters parameters.json
```

## <a name="access-control-for-data-lake-storage-gen2-in-hdinsight"></a>Toegangsbeheer voor Data Lake Storage Gen2 in HDInsight

### <a name="what-kinds-of-permissions-does-data-lake-storage-gen2-support"></a>Wat voor soort machtigingen biedt ondersteuning voor Data Lake Storage Gen2?

Data Lake Storage Gen2 maakt gebruik van een model voor toegangsbeheer die ondersteuning biedt voor op rollen gebaseerd toegangsbeheer (RBAC) zowel POSIX-achtige toegangsbeheerlijsten (ACL's). Data Lake Storage Gen1 ondersteunt ACL alleen voor het beheren van toegang tot gegevens.

RBAC gebruikt roltoewijzingen om machtigingen effectief worden toegepast op gebruikers, groepen en service-principals voor Azure-resources. Deze Azure-resources worden normaal gesproken beperkt tot op het hoogste niveau van resources (bijvoorbeeld Azure storage-accounts). Dit mechanisme is voor Azure Storage, en ook Data Lake Storage Gen2 uitgebreid naar het bestand system resource.

 Zie voor meer informatie over machtigingen voor bestanden met RBAC [Azure op rollen gebaseerd toegangsbeheer (RBAC)](../storage/blobs/data-lake-storage-access-control.md#azure-role-based-access-control-rbac).

Zie voor meer informatie over machtigingen voor bestanden met ACL's, [toegangsbeheerlijsten voor bestanden en mappen](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories).

### <a name="how-do-i-control-access-to-my-data-in-data-lake-storage-gen2"></a>Hoe beheers ik toegang tot mijn gegevens in Data Lake Storage Gen2?

Uw HDInsight-cluster de mogelijkheid om bestanden te openen in Data Lake Storage Gen2 wordt geregeld via beheerde identiteiten. Een beheerde identiteit is een identiteit die is geregistreerd bij Azure Active Directory (Azure AD) waarvan de referenties worden beheerd door Azure. Met beheerde identiteiten moet u geen service-principals in Azure AD registreren of beheren van referenties, zoals certificaten.

Azure-services zijn twee soorten beheerde identiteiten: systeem toegewezen en de gebruiker toegewezen. HDInsight maakt gebruik van beheerde identiteiten voor toegang tot Data Lake Storage Gen2 gebruiker toegewezen. Een gebruiker toegewezen beheerde identiteit wordt gemaakt als zelfstandige Azure-resource. Via een productieproces maakt Azure een identiteit in de Azure AD-tenant, die wordt vertrouwd door het gebruikte abonnement. Nadat de identiteit is gemaakt, kan deze worden toegewezen aan een of meer Azure-service-exemplaren.

De levenscyclus van een door de gebruiker toegewezen identiteit wordt afzonderlijk beheerd van de levenscyclus van de Azure Service-exemplaren waaraan de identiteit is toegewezen. Zie voor meer informatie over beheerde identiteiten [hoe kan de beheerde identiteit voor Azure-resources work?](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work).

### <a name="how-do-i-set-permissions-for-azure-ad-users-to-query-data-in-data-lake-storage-gen2-by-using-hive-or-other-services"></a>Hoe stel ik machtigingen voor Azure AD-gebruikers kunt u gegevens in Data Lake Storage Gen2 opvragen met behulp van Hive of andere services?

Om machtigingen voor gebruikers om gegevens te doorzoeken, gebruikt u Azure AD-beveiligingsgroepen als de toegewezen principal in de ACL's. Geen machtigingen voor toegang tot de bestanden rechtstreeks toewijzen aan individuele gebruikers of service-principals. Wanneer u Azure AD-beveiligingsgroepen gebruikt voor het beheren van de stroom van machtigingen, kunt u deze kunt toevoegen en verwijderen van gebruikers of service-principals zonder ACL's toepassen op een volledige mapstructuur. U hoeft alleen te toevoegen of verwijderen van de gebruikers van de juiste Azure AD-beveiligingsgroep. ACL's worden niet overgenomen, zodat het toepassen van ACL's is vereist voor het bijwerken van de ACL voor elk bestand en de submap.

## <a name="next-steps"></a>Volgende stappen

* [Integratie van Azure HDInsight met Data Lake Storage Gen2 preview - ACL en beveiliging bijwerken](https://azure.microsoft.com/blog/azure-hdinsight-integration-with-data-lake-storage-gen-2-preview-acl-and-security-update/)
* [Inleiding tot Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md)
