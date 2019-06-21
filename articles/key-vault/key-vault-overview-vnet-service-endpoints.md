---
title: Virtual network-service-eindpunten voor Azure Key Vault - Azure Key Vault | Microsoft Docs
description: Overzicht van virtual network-service-eindpunten voor Key Vault
services: key-vault
author: amitbapat
ms.author: ambapat
manager: barbkess
ms.date: 01/02/2019
ms.service: key-vault
ms.topic: conceptual
ms.openlocfilehash: 45499dac3cc50e2b6e79f9ebcb1bc3e7b4330beb
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165858"
---
# <a name="virtual-network-service-endpoints-for-azure-key-vault"></a>Virtual network-service-eindpunten voor Azure Key Vault

De service-eindpunten voor virtueel netwerk voor Azure Key Vault kunnen u toegang tot een opgegeven virtuele netwerk te beperken. De eindpunten kunt u toegang tot een lijst met IPv4 (internet protocolversie 4)-adresbereiken beperken. Elke gebruiker die verbinding maken met uw key vault vanuit buiten deze bronnen is toegang geweigerd.

Er is één belangrijk uitzondering op deze beperking. Als een gebruiker heeft ervoor hebt gekozen, worden opgenomen in om toe te staan van vertrouwde Microsoft-services worden verbindingen van deze services kunt via de firewall. Bijvoorbeeld, deze services Office 365 Exchange Online opnemen, Office 365 SharePoint Online, Azure compute, Azure Resource Manager en Azure Backup. Dergelijke gebruikers nog steeds nodig hebt voor een geldig Azure Active Directory-token en moet beschikken over machtigingen (geconfigureerd als toegangsbeleid) voor het uitvoeren van de aangevraagde bewerking. Zie voor meer informatie, [service-eindpunten voor virtuele netwerken](../virtual-network/virtual-network-service-endpoints-overview.md).

## <a name="usage-scenarios"></a>Gebruiksscenario's

U kunt configureren [Key Vault-firewalls en virtuele netwerken](key-vault-network-security.md) voor het weigeren van toegang tot verkeer via alle netwerken (met inbegrip van internetverkeer) standaard. U kunt toegang verlenen op verkeer van specifieke Azure-netwerken en het openbare internet-IP-adres-adresbereiken, zodat u kunt een beveiligde netwerkgrens voor uw toepassingen te bouwen.

> [!NOTE]
> Key Vault-firewalls en virtuele netwerkregels alleen van toepassing op de [gegevenslaag](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) van Key Vault. Key Vault bewerkingen voor de controlelaag (zoals maken, verwijderen en aanpassen van bewerkingen, instellen van toegangsbeleid, instelling firewalls en virtuele netwerkregels) worden niet beïnvloed door firewalls en virtual network-regels.

Hier volgen enkele voorbeelden van hoe u service-eindpunten kunt gebruiken:

* U Key Vault gebruikt voor het opslaan van versleutelingssleutels en toepassingsgeheimen certificaten, en u wilt blokkeren van toegang tot uw key vault via het openbare internet.
* U kunt toegang tot uw key vault vergrendelen zodat alleen uw toepassing of een korte lijst van bepaalde hosts verbinding met uw key vault maken kunt.
* U hebt een toepassing die wordt uitgevoerd in uw Azure-netwerk en dit virtuele netwerk is vergrendeld voor alle binnenkomend en uitgaand verkeer. Uw toepassing moet nog steeds verbinding maken met Key Vault voor het ophalen van geheimen of certificaten en cryptografische sleutels te gebruiken.

## <a name="configure-key-vault-firewalls-and-virtual-networks"></a>Key Vault-firewalls en virtuele netwerken configureren

Hier volgen de stappen die vereist voor het configureren van firewalls en virtuele netwerken. Deze stappen zijn van toepassing of u met PowerShell, de Azure CLI of Azure portal.

1. Schakel [logboekregistratie van Key Vault](key-vault-logging.md) gedetailleerde toegang Logboeken. Dit helpt bij diagnostische gegevens, wanneer u firewalls en virtuele netwerkregels te voorkomen toegang tot een key vault dat. (Deze stap is optioneel maar ten zeerste aanbevolen.)
2. Schakel **service-eindpunten voor key vault** voor virtuele netwerken en subnetten.
3. Firewalls en virtuele-netwerkregels voor een key vault toegang te beperken die key vault vanuit een specifieke virtuele netwerken, subnetten en IPv4-adresbereiken instellen.
4. Als deze sleutelkluis toegankelijk zijn via een vertrouwde Microsoft-services moet, schakelt u de optie om toe te staan **vertrouwde Azure-Services** verbinding maken met Key Vault.

Zie voor meer informatie, [Azure Key Vault configureren van firewalls en virtuele netwerken](key-vault-network-security.md).

> [!IMPORTANT]
> Nadat de firewall-regels zijn van kracht, gebruikers alleen Key Vault kunnen uitvoeren [gegevenslaag](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) operations wanneer hun aanvragen afkomstig uit de zijn toegestane virtuele netwerken of een IPv4-adresbereik. Dit geldt ook voor toegang tot Key Vault vanuit Azure portal. Hoewel gebruikers naar een key vault vanuit Azure portal bladeren kunnen, ze niet mogelijk een lijst met sleutels, geheimen of certificaten als hun clientmachine zich niet in de lijst met toegestane. Dit ook van invloed op de kiezer voor Key Vault met andere Azure-services. Gebruikers mogelijk overzicht van sleutelkluizen, maar niet sleutels, weergeven als de firewall-regels te voorkomen dat de client-computer.


> [!NOTE]
> Houd rekening met de volgende configuratie-beperkingen:
> * Maximaal 127 virtueel netwerk en 127 IPv4-regels zijn toegestaan. 
> * Kleine-adresbereiken die gebruikmaken van de '/ 31' of '/ 32' voorvoegsel grootten worden niet ondersteund. Configureer in plaats daarvan deze bereiken met behulp van afzonderlijke regels voor IP-adres.
> * IP-netwerkregels zijn alleen toegestaan voor openbare IP-adressen. IP-adresbereiken is gereserveerd voor particuliere netwerken (zoals gedefinieerd in RFC 1918) zijn niet toegestaan in IP-regels. Particuliere netwerken bevatten adressen die met beginnen **10.** , **172.16-31**, en **192.168.** . 
> * Alleen IPv4-adressen worden ondersteund op dit moment.

## <a name="trusted-services"></a>Betrouwbare services

Hier volgt een lijst van vertrouwde services die zijn toegestaan voor toegang tot een key vault als de **vertrouwde-services toestaan** optie is ingeschakeld.

|Vertrouwde service|Gebruiksscenario's|
| --- | --- |
|Azure Virtual Machines-service voor implementatie|[Certificaten implementeren op virtuele machines uit de Sleutelkluis door de klant beheerde](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/).|
|Azure Resource Manager-sjabloon voor implementatie van service|[Beveiligde waarden doorgeven tijdens implementatie](../azure-resource-manager/resource-manager-keyvault-parameter.md).|
|Azure Disk Encryption volume versleuteling-service|Toegang tot BitLocker Key (Windows-VM) of DM wachtwoordzin (Linux-VM) en Key-versleutelingssleutel, tijdens de implementatie van de virtuele machine toestaan. Hierdoor kunnen [Azure Disk Encryption](../security/azure-security-disk-encryption.md).|
|Azure Backup|Toestaan dat back-up en herstel van de relevante sleutels en geheimen tijdens back-up van virtuele Machines van Azure, met behulp van [Azure Backup](../backup/backup-introduction-to-azure-backup.md).|
|Exchange Online en SharePoint Online|Toegang tot de klantsleutel toestaan voor Azure Storage-Serviceversleuteling met [Klantsleutel](https://support.office.com/article/Controlling-your-data-in-Office-365-using-Customer-Key-f2cd475a-e592-46cf-80a3-1bfb0fa17697).|
|Azure Information Protection|Toegang tot de tenantsleutel voor [Azure Information Protection.](https://docs.microsoft.com/azure/information-protection/what-is-information-protection)|
|Azure App Service|[Azure Web App Certificate via Key Vault implementeren](https://azure.github.io/AppService/2016/05/24/Deploying-Azure-Web-App-Certificate-through-Key-Vault.html).|
|Azure SQL Database|[De Transparent Data Encryption met Bring Your Own Key-ondersteuning voor Azure SQL Database en Data Warehouse](../sql-database/transparent-data-encryption-byok-azure-sql.md?view=sql-server-2017&viewFallbackFrom=azuresqldb-current).|
|Azure Storage|[Versleuteling voor opslagservice met behulp van de klant beheerde sleutels in Azure Key Vault](../storage/common/storage-service-encryption-customer-managed-keys.md).|
|Azure Data Lake Store|[Versleuteling van gegevens in Azure Data Lake Store](../data-lake-store/data-lake-store-encryption.md) met een door de klant beheerde sleutel.|
|Azure databricks|[Snel, eenvoudig en gezamenlijke op basis van Apache Spark analytics-service](../azure-databricks/what-is-azure-databricks.md)|
|Azure API Management|[Certificaten implementeren voor aangepast domein uit Key Vault met behulp van MSI](../api-management/api-management-howto-use-managed-service-identity.md#use-the-managed-service-identity-to-access-other-resources)|



> [!NOTE]
> U moet de relevante toegangsbeleid van Key Vault instellen waarmee de bijbehorende services toegang krijgen tot Key Vault.

## <a name="next-steps"></a>Volgende stappen

* [Uw Key Vault beveiligen](key-vault-secure-your-key-vault.md)
* [Azure Key Vault-firewalls en virtuele netwerken configureren](key-vault-network-security.md)
