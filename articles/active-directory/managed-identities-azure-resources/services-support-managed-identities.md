---
title: Azure-Services die ondersteuning bieden voor beheerde identiteiten voor Azure-resources
description: Lijst met services die ondersteuning bieden voor beheerde identiteiten voor Azure-resources en Azure AD-verificatie
services: active-directory
author: MarkusVi
ms.author: markvi
ms.date: 06/19/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: msi
manager: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: ca461f3be740c3b0bac18795991bb721a5305240
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67611542"
---
# <a name="services-that-support-managed-identities-for-azure-resources"></a>Services die ondersteuning bieden voor beheerde identiteiten voor Azure-resources

Beheerde identiteiten voor Azure-resources bieden Azure-services met een automatisch beheerde identiteit in Azure Active Directory. Met behulp van een beheerde identiteit, kunt u verifiëren met een service die ondersteuning biedt voor Azure AD-verificatie zonder referenties in uw code. We zijn bezig met integratie van beheerde identiteiten voor Azure-resources en Azure AD-verificatie in Azure. Controleer regelmatig of er updates.

> [!NOTE]
> Beheerde identiteiten voor Azure-resources is de nieuwe naam voor de service die eerder de naam Managed Service Identity (MSI) had.

## <a name="azure-services-that-support-managed-identities-for-azure-resources"></a>Azure-services die beheerde identiteiten voor Azure-resources ondersteunen

De volgende Azure-services bieden ondersteuning voor beheerde identiteiten voor Azure-resources:

### <a name="azure-virtual-machines"></a>Azure Virtual Machines

| ID-type beheerd | Algemeen beschikbaar<br>Globale Azure-regio 's | Azure Government | Azure Duitsland | Azure China 21Vianet |
| --- | --- | --- | --- | --- |
| Systeem toegewezen | Beschikbaar | Preview | Preview | Preview | 
| Toegewezen gebruiker | Preview | Preview | Preview | Preview |

Raadpleeg de volgende lijst voor het configureren van beheerde identiteit voor Azure Virtual Machines (in regio's indien beschikbaar):

- [Azure-portal](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure-CLI](qs-configure-cli-windows-vm.md)
- [Azure Resource Manager-sjablonen](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-virtual-machine-scale-sets"></a>Azure Virtual Machine Scale Sets

|ID-type beheerd | Algemeen beschikbaar<br>Globale Azure-regio 's | Azure Government | Azure Duitsland | Azure China 21Vianet |
| --- | --- | --- | --- | --- |
| Systeem toegewezen | Beschikbaar | Preview | Preview | Preview |
| Toegewezen gebruiker | Preview | Preview | Preview | Preview |

Raadpleeg de volgende lijst om de beheerde identiteit configureren voor Azure Virtual Machine Scale Sets (in regio's indien beschikbaar):

- [Azure-portal](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure-CLI](qs-configure-cli-windows-vm.md)
- [Azure Resource Manager-sjablonen](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-app-service"></a>Azure App Service

| ID-type beheerd | Algemeen beschikbaar<br>Globale Azure-regio 's | Azure Government | Azure Duitsland | Azure China 21Vianet |
| --- | --- | --- | --- | --- |
| Systeem toegewezen | Beschikbaar | Beschikbaar | Beschikbaar | Beschikbaar |
| Toegewezen gebruiker | Preview | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar |

Raadpleeg de volgende lijst voor het configureren van beheerde identiteit voor Azure App Service (in regio's indien beschikbaar):

- [Azure-portal](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure-CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure Resource Manager-sjabloon](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-blueprints"></a>Azure Blueprints

|ID-type beheerd | Algemeen beschikbaar<br>Globale Azure-regio 's | Azure Government | Azure Duitsland | Azure China 21Vianet |
| --- | --- | --- | --- | --- |
| Systeem toegewezen | Preview | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar |
| Toegewezen gebruiker | Preview | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar |

Raadpleeg de volgende lijst om het gebruik van een beheerde identiteit met [Azure blauwdrukken](../../governance/blueprints/overview.md):

- [Azure portal - blauwdruktoewijzing](../../governance/blueprints/create-blueprint-portal.md#assign-a-blueprint)
- [REST-API - blauwdruktoewijzing](../../governance/blueprints/create-blueprint-rest-api.md#assign-a-blueprint)

### <a name="azure-functions"></a>Azure Functions

ID-type beheerd |Algemeen beschikbaar<br>Globale Azure-regio 's | Azure Government | Azure Duitsland | Azure China 21Vianet |
| --- | --- | --- | --- | --- |
| Systeem toegewezen | Beschikbaar | Beschikbaar | Beschikbaar | Beschikbaar |
| Toegewezen gebruiker | Preview | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar |

Raadpleeg de volgende lijst voor het configureren van beheerde identiteit voor Azure Functions (in regio's indien beschikbaar):

- [Azure-portal](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure-CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure Resource Manager-sjabloon](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-logic-apps"></a>Azure Logic Apps

ID-type beheerd | Algemeen beschikbaar<br>Globale Azure-regio 's | Azure Government | Azure Duitsland | Azure China 21Vianet |
| --- | --- | --- | --- | --- |
| Systeem toegewezen | Preview | Preview | Niet beschikbaar | Preview |
| Toegewezen gebruiker | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar |

Raadpleeg de volgende lijst om de beheerde identiteit configureren voor Azure Logic Apps (in regio's indien beschikbaar):

- [Azure-portal](/azure/logic-apps/create-managed-service-identity#azure-portal)
- [Azure Resource Manager-sjabloon](/azure/app-service/overview-managed-identity)

### <a name="azure-data-factory-v2"></a>Azure Data Factory V2

ID-type beheerd | Algemeen beschikbaar<br>Globale Azure-regio 's | Azure Government | Azure Duitsland | Azure China 21Vianet |
| --- | --- | --- | --- | --- |
| Systeem toegewezen | Beschikbaar | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar |
| Toegewezen gebruiker | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar |

Raadpleeg de volgende lijst om de beheerde identiteit configureren voor Azure Data Factory V2 (in regio's indien beschikbaar):

- [Azure-portal](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity)
- [PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity-using-powershell)
- [REST](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity-using-rest-api)
- [SDK](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity-using-sdk)

### <a name="azure-api-management"></a>Azure API Management

ID-type beheerd | Algemeen beschikbaar<br>Globale Azure-regio 's | Azure Government | Azure Duitsland | Azure China 21Vianet |
| --- | --- | --- | --- | --- |
| Systeem toegewezen | Beschikbaar | Beschikbaar | Niet beschikbaar | Niet beschikbaar |
| Toegewezen gebruiker | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar |

Raadpleeg de volgende lijst om de beheerde identiteit configureren voor Azure API Management (in regio's indien beschikbaar):

- [Azure Resource Manager-sjabloon](/azure/api-management/api-management-howto-use-managed-service-identity)

### <a name="azure-container-instances"></a>Azure Container Instances

ID-type beheerd | Algemeen beschikbaar<br>Globale Azure-regio 's | Azure Government | Azure Duitsland | Azure China 21Vianet |
| --- | --- | --- | --- | --- |
| Systeem toegewezen | Linux: Preview<br>Windows: Niet beschikbaar | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar |
| Toegewezen gebruiker | Linux: Preview<br>Windows: Niet beschikbaar | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar |

Raadpleeg de volgende lijst om de beheerde identiteit configureren voor Azure Container Instances (in regio's indien beschikbaar):

- [Azure-CLI](~/articles/container-instances/container-instances-managed-identity.md)
- [Azure Resource Manager-sjabloon](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-resource-manager-template)
- [YAML](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-yaml-file)

### <a name="azure-container-registry-tasks"></a>Azure Container Registry Tasks

ID-type beheerd | Algemeen beschikbaar<br>Globale Azure-regio 's | Azure Government | Azure Duitsland | Azure China 21Vianet |
| --- | --- | --- | --- | --- |
| Systeem toegewezen | Beschikbaar | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar |
| Toegewezen gebruiker | Preview | Niet beschikbaar | Niet beschikbaar | Niet beschikbaar |

Raadpleeg de volgende lijst voor het configureren van beheerde identiteit voor Azure Container Registry-taken (in regio's indien beschikbaar):

- [Azure-CLI](~/articles/container-registry/container-registry-tasks-authentication-managed-identity.md)

## <a name="azure-services-that-support-azure-ad-authentication"></a>Azure-services die ondersteuning voor Azure AD-verificatie

De volgende services ondersteuning bieden voor Azure AD-verificatie, en zijn getest met clientservices die gebruikmaken van beheerde identiteiten voor Azure-resources.

### <a name="azure-resource-manager"></a>Azure Resource Manager

Raadpleeg de volgende lijst voor het configureren van toegang tot Azure Resource Manager:

- [Toegang via Azure portal toewijzen](howto-assign-access-portal.md)
- [Toegang via Powershell toewijzen](howto-assign-access-powershell.md)
- [Toewijzen van toegang via Azure CLI](howto-assign-access-CLI.md)
- [Toegang via Azure Resource Manager-sjabloon toewijzen](../../role-based-access-control/role-assignments-template.md)

| Cloud | Resource-ID | Status |
|--------|------------|--------|
| Azure Global | `https://management.azure.com/`| Beschikbaar |
| Azure Government | `https://management.usgovcloudapi.net/` | Beschikbaar |
| Azure Duitsland | `https://management.microsoftazure.de/` | Beschikbaar |
| Azure China 21Vianet | `https://management.chinacloudapi.cn` | Beschikbaar |

### <a name="azure-key-vault"></a>Azure Key Vault

| Cloud | Resource-ID | Status |
|--------|------------|--------|
| Azure Global | `https://vault.azure.net`| Beschikbaar |
| Azure Government | `https://vault.usgovcloudapi.net` | Beschikbaar |
| Azure Duitsland |  `https://vault.microsoftazure.de` | Beschikbaar |
| Azure China 21Vianet | `https://vault.azure.cn` | Beschikbaar |

### <a name="azure-data-lake"></a>Azure Data Lake 

| Cloud | Resource-ID | Status |
|--------|------------|--------|
| Azure Global | `https://datalake.azure.net/` | Beschikbaar |
| Azure Government |  | Niet beschikbaar |
| Azure Duitsland |   | Niet beschikbaar |
| Azure China 21Vianet |  | Niet beschikbaar |

### <a name="azure-sql"></a>Azure SQL 

| Cloud | Resource-ID | Status |
|--------|------------|--------|
| Azure Global | `https://database.windows.net/` | Beschikbaar |
| Azure Government | `https://database.usgovcloudapi.net/` | Beschikbaar |
| Azure Duitsland | `https://database.cloudapi.de/` | Beschikbaar |
| Azure China 21Vianet | `https://database.chinacloudapi.cn/` | Beschikbaar |

### <a name="azure-event-hubs"></a>Azure Event Hubs

| Cloud | Resource-ID | Status |
|--------|------------|--------|
| Azure Global | `https://eventhubs.azure.net` | Preview |
| Azure Government |  | Niet beschikbaar |
| Azure Duitsland |   | Niet beschikbaar |
| Azure China 21Vianet |  | Niet beschikbaar |

### <a name="azure-service-bus"></a>Azure Service Bus

| Cloud | Resource-ID | Status |
|--------|------------|--------|
| Azure Global | `https://servicebus.azure.net`  | Preview |
| Azure Government |  | Niet beschikbaar |
| Azure Duitsland |   | Niet beschikbaar |
| Azure China 21Vianet |  | Niet beschikbaar |

### <a name="azure-storage-blobs-and-queues"></a>Azure Storage-blobs en wachtrijen

| Cloud | Resource-ID | Status |
|--------|------------|--------|
| Azure Global | `https://storage.azure.com/` | Beschikbaar |
| Azure Government | `https://storage.azure.com/` | Beschikbaar |
| Azure Duitsland | `https://storage.azure.com/` | Beschikbaar |
| Azure China 21Vianet | `https://storage.azure.com/` | Beschikbaar |

### <a name="azure-analysis-services"></a>Azure Analysis Services

| Cloud | Resource-ID | Status |
|--------|------------|--------|
| Azure Global | `https://*.asazure.windows.net` | Beschikbaar |
| Azure Government | `https://*.asazure.usgovcloudapi.net` | Beschikbaar |
| Azure Duitsland | `https://*.asazure.cloudapi.de` | Beschikbaar |
| Azure China 21Vianet | `https://*.asazure.chinacloudapi.cn` | Beschikbaar |
