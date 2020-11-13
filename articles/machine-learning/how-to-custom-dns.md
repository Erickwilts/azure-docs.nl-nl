---
title: Aangepaste DNS-server gebruiken
titleSuffix: Azure Machine Learning
description: Een aangepaste DNS-server configureren voor gebruik met een Azure Machine Learning-werk ruimte en een persoonlijk eind punt.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.author: jhirono
author: jhirono
ms.date: 11/13/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: e3d95be52215b03a30dc4b5c7f251357f163b24a
ms.sourcegitcommit: 9706bee6962f673f14c2dc9366fde59012549649
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2020
ms.locfileid: "94616090"
---
# <a name="how-to-use-your-workspace-with-a-custom-dns-server"></a>Werkruimte gebruiken met een aangepaste DNS-server

Wanneer u Azure Machine Learning met een virtueel netwerk gebruikt, zijn er [verschillende manieren om DNS-naam omzetting te verwerken](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md). Standaard verwerkt Azure automatisch naam omzetting voor uw werk ruimte en persoonlijk eind punt. Als u in plaats daarvan _uw eigen aangepaste DNS-server gebruikt_ , moet u hand matig DNS-vermeldingen maken voor de werk ruimte.

> [!IMPORTANT]
> In dit artikel wordt alleen beschreven hoe u de Fully Qualified Domain Name (FQDN) en IP-adressen voor deze vermeldingen kunt vinden. Deze bevat geen informatie over het configureren van de DNS-records voor deze items. Raadpleeg de documentatie bij uw DNS-software voor informatie over het toevoegen van records.

## <a name="prerequisites"></a>Vereisten

- Een Azure-Virtual Network die gebruikmaakt van [uw eigen DNS-server](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).

- Een Azure Machine Learning-werk ruimte met een persoonlijk eind punt. Zie [een Azure machine learning-werk ruimte maken](how-to-manage-workspace.md)voor meer informatie.

- Vertrouwd met het gebruik van [netwerk isolatie tijdens de training & deinterferentie](./how-to-network-security-overview.md).

- Optioneel, [Azure cli](/cli/azure/install-azure-cli) of [Azure PowerShell](/powershell/azure/install-az-ps).

## <a name="find-the-ip-addresses"></a>De IP-adressen zoeken

De volgende lijst bevat de volledig gekwalificeerde domein namen (FQDN) die worden gebruikt door uw werk ruimte en persoonlijk eind punt:

* `<workspace-GUID>.workspace.<region>.api.azureml.ms`
* `<workspace-GUID>.workspace.<region>.experiments.azureml.net`
* `<workspace-GUID>.workspace.<region>.modelmanagement.azureml.net`
* `<workspace-GUID>.workspace.<region>.aether.ms`
* `ml-<workspace-name>-<region>-<workspace-guid>.notebooks.azure.ml`
* Als u een reken instantie maakt, moet u ook een vermelding voor toevoegen `<instance-name>.<region>.instances.azureml.ms` met het privé-IP-eind punt van de werk ruimte.

    > [!NOTE]
    > Reken instanties kunnen alleen worden geopend vanuit het virtuele netwerk.

Gebruik een van de volgende methoden om de interne IP-adressen voor de FQDN-namen in het VNet te vinden:

> [!NOTE]
> De volledig gekwalificeerde domein namen en IP-adressen verschillen op basis van uw configuratie. De GUID-waarde in de domein naam is bijvoorbeeld specifiek voor uw werk ruimte.

# <a name="azure-cli"></a>[Azure-CLI](#tab/azure-cli)

```azurecli
az network private-endpoint show --endpoint-name <endpoint> --resource-group <resource-group> --query 'customDnsConfigs[*].{FQDN: fqdn, IPAddress: ipAddresses[0]}' --output table
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell
$workspaceDns=Get-AzPrivateEndpoint -Name <endpoint> -resourcegroupname <resource-group>
$workspaceDns.CustomDnsConfigs | format-table
```

# <a name="azure-portal"></a>[Azure-portal](#tab/azure-portal)

1. Selecteer uw Azure Machine Learning- __werk ruimte__ In de [Azure Portal](https://portal.azure.com).
1. Selecteer in de sectie __instellingen__ de optie verbindingen met een __privé-eind punt__.
1. Selecteer de koppeling in de kolom met het __persoonlijke eind punt__ die wordt weer gegeven.
1. Onder aan de pagina vindt u een lijst met de FQDN-namen (Fully Qualified Domain names) en IP-adressen voor het persoonlijke eind punt van de werk ruimte.

:::image type="content" source="./media/how-to-custom-dns/private-endpoint-custom-dns.png" alt-text="Lijst met FQDN-namen in de portal":::

---

De informatie die door alle methoden wordt geretourneerd, is hetzelfde. een lijst met de FQDN en het persoonlijke IP-adres voor de resources.

| FQDN | IP-adres |
| ----- | ----- |
| `fb7e20a0-8891-458b-b969-55ddb3382f51.workspace.eastus.api.azureml.ms` | `10.1.0.5` |
| `ml-myworkspace-eastus-fb7e20a0-8891-458b-b969-55ddb3382f51.notebooks.azure.net` | `10.1.0.6` |

> [!IMPORTANT]
> Sommige FQDN-vormen worden niet weer gegeven in de lijst die wordt weer gegeven door het persoonlijke eind punt, maar zijn vereist voor de werk ruimte. Deze FQDN-namen worden weer gegeven in de volgende tabel en moeten ook worden toegevoegd aan uw DNS-server:
>
> * `<workspace-GUID>.workspace.<region>.experiments.azureml.net`
> * `<workspace-GUID>.workspace.<region>.modelmanagement.azureml.net`
> * `<workspace-GUID>.workspace.<region>.aether.ms`
> * Als u een reken instantie hebt, gebruikt u `<instance-name>.<region>.instances.azureml.ms` , waarbij `<instance-name>` de naam van uw reken instantie is. Gebruik een privé-IP-adres van het persoonlijke eind punt van de werk ruimte. Houd er rekening mee dat Compute instance alleen kan worden geopend vanuit het virtuele netwerk.
>
> Voor al deze IP-adressen gebruikt u hetzelfde adres als de `*.api.azureml.ms` vermeldingen die in de vorige stappen zijn geretourneerd.

## <a name="next-steps"></a>Volgende stappen

Zie het [overzicht van Virtual Network](how-to-network-security-overview.md)voor meer informatie over het gebruik van Azure machine learning met een virtueel netwerk.