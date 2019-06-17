---
title: Key Vault integreren met SQL Server op Windows-VM's in Azure (Resource Manager) | Microsoft Docs
description: Informatie over het automatiseren van de configuratie van SQL Server-versleuteling voor gebruik met Azure Key Vault. In dit onderwerp wordt uitgelegd hoe het gebruik van Azure Key Vault-integratie met SQL Server virtuele machines die zijn gemaakt met Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: cd66dfb1-0e9b-4fb0-a471-9deaf4ab4ab8
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/30/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 13d698cfbc0241248a77fd5f3b148a9393320c64
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67076037"
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-resource-manager"></a>Azure Key Vault-integratie configureren voor SQL Server op Azure Virtual Machines (Resource Manager)

> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-ps-sql-keyvault.md)
> * [Klassiek](../sqlclassic/virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Overzicht
Er zijn meerdere functies van SQL Server-versleuteling, zoals [transparante gegevensversleuteling (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [versleuteling op kolom (CLE)](https://msdn.microsoft.com/library/ms173744.aspx), en [back-upversleuteling](https://msdn.microsoft.com/library/dn449489.aspx). Deze vormen van versleuteling moeten u de cryptografische sleutels die u voor versleuteling gebruikt opslaan en beheren. De service Azure Key Vault (AKV) is ontworpen voor het verbeteren van de beveiliging en beheer van deze sleutels op een beveiligde en maximaal beschikbare locatie. De [SQL Server-Connector](https://www.microsoft.com/download/details.aspx?id=45344) kan SQL Server om deze sleutels uit Azure Key Vault te gebruiken.

Als u SQL Server on-premises virtuele machines worden uitgevoerd, zijn er [stappen die u kunt volgen om toegang tot Azure Key Vault vanuit uw on-premises SQL Server-computer](https://msdn.microsoft.com/library/dn198405.aspx). Maar voor SQL Server in virtuele Azure-machines, kunt u tijd besparen met behulp van de *Azure Key Vault-integratie* functie.

Wanneer deze functie is ingeschakeld, installeert en deze automatisch de SQL Server-Connector, configureert u de EKM-provider voor toegang tot Azure Key Vault, maakt u de referentie zodat u toegang krijgt tot uw kluis. Als u de stappen in de documentatie van de eerder genoemde on-premises hebt bekeken, kunt u zien dat deze functie automatiseert de stappen 2 en 3. Het enige wat dat u moet nog steeds handmatig doen is het maken van de key vault en sleutels. Van daaruit is de volledige installatie van uw SQL-VM geautomatiseerd. Als deze functie kan deze installatie is voltooid, kunt u T-SQL-instructies gebruikt om te beginnen met het versleutelen van uw databases of back-ups zoals u gewend kunt uitvoeren.

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

  >[!NOTE]
  > Versie 1.0.4.0 EKM-Provider is geïnstalleerd op de SQL Server-VM via de [SQL IaaS-extensie](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension). De provider-versie wordt niet bijgewerkt als u een upgrade van de SQL IaaS-extensie. Neem overweegt handmatig de versie van de EKM-provider een upgrade uitvoert, indien nodig (bijvoorbeeld bij het migreren naar een SQL beheerd exemplaar).


## <a name="enabling-and-configuring-akv-integration"></a>Inschakelen en configureren van Azure Sleutelkluis-integratie
U kunt tijdens het inrichten van Azure Sleutelkluis-integratie inschakelen of configureren voor bestaande VM's.

### <a name="new-vms"></a>Nieuwe virtuele machines
Als u een nieuwe SQL Server-machine met Resource Manager inricht, biedt de Azure-portal een manier om Azure Key Vault-integratie inschakelen. De Azure Key Vault-functie is alleen beschikbaar voor de Enterprise, Developer en Evaluation-edities van SQL Server.

![Integratie van Azure Sleutelkluis in SQL](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Zie voor een gedetailleerd overzicht van de inrichting van [een SQL Server-machine inrichten in Azure portal](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Bestaande VM 's

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Voor bestaande SQL Server virtuele machines, opent u uw [SQL-resource voor virtuele machines](virtual-machines-windows-sql-manage-portal.md#access-sql-virtual-machine-resource) en selecteer **Security** onder **instellingen**. Selecteer **inschakelen** Azure Key Vault-integratie inschakelen. 

![SQL Azure Sleutelkluis-integratie voor bestaande VM 's](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

Wanneer u klaar bent, selecteert u de **toepassen** knop aan de onderkant van de **Security** pagina uw wijzigingen op te slaan.

> [!NOTE]
> De referentienaam van de die we hier hebben gemaakt wordt later opnieuw worden toegewezen aan een SQL-aanmelding. Hiermee wordt de SQL-aanmelding voor toegang tot de sleutelkluis. 


> [!NOTE]
> U kunt ook configureren met behulp van een sjabloon van Azure Sleutelkluis-integratie. Zie voor meer informatie, [Azure quickstart-sjabloon voor Azure Key Vault-integratie](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).


[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]
