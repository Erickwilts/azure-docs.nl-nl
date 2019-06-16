---
title: Schijven voor schaalsets met Azure CLI Azure versleutelen | Microsoft Docs
description: Informatie over het gebruik van de Azure CLI voor het versleutelen van VM-instanties en de gekoppelde schijven in een Linux VM-schaalset
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2019
ms.author: cynthn
ms.openlocfilehash: 1264c7e4ebaf5e948e624fa49dc5fb0b4cdb31f0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64869059"
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set-with-the-azure-cli"></a>Besturingssysteem en gekoppelde gegevensschijven in een virtuele-machineschaalset met de Azure CLI versleutelen

Als u wilt beschermen en beveiligen van data-at-rest met behulp van de bedrijfstak versleutelingstechnologie, ondersteuning virtuele-machineschaalsets voor Azure Disk Encryption (ADE). Versleuteling kan worden ingeschakeld voor Linux en Windows virtuele machine schaalsets. Zie voor meer informatie, [Azure Disk Encryption voor Linux en Windows](../security/azure-security-disk-encryption.md).

Azure disk encryption wordt ondersteund:
- voor scale sets met beheerde schijven gemaakt en niet ondersteund voor systeemeigen (of niet-beheerde) schijf schaalsets.
- voor het besturingssysteem en volumes in Windows-schaalsets. Schakel versleuteling wordt ondersteund voor besturingssysteem en volumes voor Windows-schaalsets.
- voor de gegevensvolumes in Linux-schaalsets. OS-schijfversleuteling wordt niet ondersteund voor Linux-schaalsets.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om te installeren en de CLI lokaal gebruikt, deze zelfstudie vereist dat u de Azure CLI versie 2.0.31 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren]( /cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="create-a-scale-set"></a>Een schaalset maken

Voordat u een schaalset kunt maken, moet u eerst een resourcegroep maken met [az group create](/cli/azure/group). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Maak nu een virtuele-machineschaalset met [az vmss create](/cli/azure/vmss). In het volgende voorbeeld wordt een schaalset met de naam *myScaleSet* gemaakt die automatisch wordt bijgewerkt als er wijzigingen zijn. Daarnaast worden er SSH-sleutels gegenereerd als deze niet bestaan in *~/.ssh/id_rsa*. Een gegevensschijf van 32Gb is gekoppeld aan elk VM-exemplaar en de Azure [Custom Script Extension](../virtual-machines/linux/extensions-customscript.md) wordt gebruikt voor het voorbereiden van de gegevensschijven met [az vmss extension set](/cli/azure/vmss/extension):

```azurecli-interactive
# Create a scale set with attached data disk
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --generate-ssh-keys \
  --data-disk-sizes-gb 32

# Prepare the data disk for use with the Custom Script Extension
az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group myResourceGroup \
  --vmss-name myScaleSet \
  --settings '{"fileUris":["https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.sh"],"commandToExecute":"./prepare_vm_disks.sh"}'
```

Het duurt enkele minuten om alle schaalsetresources en VM's te maken en te configureren.

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>Maken van een Azure-sleutelkluis ingeschakeld voor schijfversleuteling

Azure Key Vault kunt opslaan, sleutels, geheimen of wachtwoorden waarmee u kunt ze veilig in uw toepassingen en services te implementeren. Cryptografische sleutels worden opgeslagen in Azure Key Vault met behulp van software-beveiliging, of u kunt importeren of genereer uw sleutels in Hardware Security Modules (HSM's) gecertificeerd voor FIPS 140-2 level 2 standaarden. Deze cryptografische sleutels worden gebruikt voor het versleutelen en ontsleutelen van virtuele schijven die zijn gekoppeld aan uw virtuele machine. U behoudt de controle van deze cryptografische sleutels en het gebruik ervan kunt controleren.

Definieer uw eigen unieke *keyvault_name*. Vervolgens maakt u een Sleutelkluis met [az keyvault maken](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-create) in hetzelfde abonnement en dezelfde regio bevinden als de schaal en de *--ingeschakeld-voor--schijfversleuteling* toegangsbeleid.

```azurecli-interactive
# Provide your own unique Key Vault name
keyvault_name=myuniquekeyvaultname

# Create Key Vault
az keyvault create --resource-group myResourceGroup --name $keyvault_name --enabled-for-disk-encryption
```

### <a name="use-an-existing-key-vault"></a>Gebruik een bestaande Key Vault

Deze stap is alleen vereist als u een bestaande Key Vault hebt die u wilt gebruiken met schijfversleuteling. Deze stap overslaan als u een Key Vault in de vorige sectie hebt gemaakt.

Definieer uw eigen unieke *keyvault_name*. Vervolgens uw Key Vault met bijgewerkt [az keyvault update](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-update) en stel de *--ingeschakeld-voor--schijfversleuteling* toegangsbeleid.

```azurecli-interactive
# Provide your own unique Key Vault name
keyvault_name=myuniquekeyvaultname

# Create Key Vault
az keyvault update --name $keyvault_name --enabled-for-disk-encryption
```

## <a name="enable-encryption"></a>Versleuteling inschakelen

Voor het versleutelen van VM-exemplaren in een schaalset, moet u eerst wat informatie over de resource-ID voor Key Vault met ontvangen [az keyvault show](/cli/azure/ext/keyvault-preview/keyvault#ext-keyvault-preview-az-keyvault-show). Deze variabelen worden gebruikt om te beginnen klikt u vervolgens het versleutelingsproces met [az vmss versleuteling inschakelen](/cli/azure/vmss/encryption#az-vmss-encryption-enable):

```azurecli-interactive
# Get the resource ID of the Key Vault
vaultResourceId=$(az keyvault show --resource-group myResourceGroup --name $keyvault_name --query id -o tsv)

# Enable encryption of the data disks in a scale set
az vmss encryption enable \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --disk-encryption-keyvault $vaultResourceId \
    --volume-type DATA
```

Het duurt een minuut of twee voor het versleutelingsproces te starten.

Als de schaalset is Upgradebeleid in de schaalset worden gemaakt in een eerdere stap is ingesteld op *automatische*, de VM-exemplaren automatisch het versleutelingsproces starten. Op schaalsets waar het Upgradebeleid in handmatig is, start u de beleidsregels voor versleuteling op de VM-exemplaren met [az vmss update-instances](/cli/azure/vmss#az-vmss-update-instances).

### <a name="enable-encryption-using-kek-to-wrap-the-key"></a>De sleutel inpakken met KEK-codering inschakelen

U kunt ook een coderingssleutel voor extra beveiliging gebruiken bij het versleutelen van de virtuele-machineschaalset.

```azurecli-interactive
# Get the resource ID of the Key Vault
vaultResourceId=$(az keyvault show --resource-group myResourceGroup --name $keyvault_name --query id -o tsv)

# Enable encryption of the data disks in a scale set
az vmss encryption enable \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --disk-encryption-keyvault $vaultResourceId \
    --key-encryption-key myKEK \
    --key-encryption-keyvault $vaultResourceId \
    --volume-type DATA
```

> [!NOTE]
>  De syntaxis voor de waarde van de schijf-versleuteling-keyvault-parameter is de volledige id-reeks:</br>
/subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br></br>
> De syntaxis voor de waarde van de encryptiesleutel parameter is de volledige URI naar de KEK-sleutel in:</br>
https://[keyvault-name].Vault.Azure.NET/Keys/[kekname]/[kek-Unique-ID]

## <a name="check-encryption-progress"></a>Versleuteling voortgang controleren

Gebruiken om te controleren of de status van schijfversleuteling, [az vmss encryption show](/cli/azure/vmss/encryption#az-vmss-encryption-show):

```azurecli-interactive
az vmss encryption show --resource-group myResourceGroup --name myScaleSet
```

Wanneer de VM-exemplaren zijn versleuteld, wordt de statuscode rapporten *EncryptionState/versleuteld*, zoals weergegeven in de volgende voorbeelduitvoer:

```bash
[
  {
    "disks": [
      {
        "encryptionSettings": null,
        "name": "myScaleSet_myScaleSet_0_disk2_3f39c2019b174218b98b3dfae3424e69",
        "statuses": [
          {
            "additionalProperties": {},
            "code": "EncryptionState/encrypted",
            "displayStatus": "Encryption is enabled on disk",
            "level": "Info",
            "message": null,
            "time": null
          }
        ]
      }
    ],
    "id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/virtualMachines/0",
    "resourceGroup": "MYRESOURCEGROUP"
  }
]
```

## <a name="disable-encryption"></a>Schakel versleuteling uit

Als u niet meer gebruiken van versleutelde VM-exemplaren schijven wilt, kunt u versleuteling met uitschakelen [az vmss-versleuteling uitschakelen](/cli/azure/vmss/encryption?view=azure-cli-latest#az-vmss-encryption-disable) als volgt:

```azurecli-interactive
az vmss encryption disable --resource-group myResourceGroup --name myScaleSet
```

## <a name="next-steps"></a>Volgende stappen

- In dit artikel gebruikt u de Azure CLI voor het versleutelen van een virtuele-machineschaalset. U kunt ook [Azure PowerShell](virtual-machine-scale-sets-encrypt-disks-ps.md) of sjablonen voor [Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-windows-jumpbox) of [Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-vmss-linux-jumpbox).
- Als u wilt dat Azure Disk Encryption toegepast nadat een andere extensie is ingericht, kunt u [extensievolgorde](virtual-machine-scale-sets-extension-sequencing.md). U kunt [deze voorbeelden](../security/azure-security-disk-encryption-extension-sequencing.md#sample-azure-templates) aan de slag.
- Een voorbeeld van de end-to-end-batch-bestand voor schijfversleuteling van Linux scale set gegevens vindt [hier](https://gist.githubusercontent.com/ejarvi/7766dad1475d5f7078544ffbb449f29b/raw/03e5d990b798f62cf188706221ba6c0c7c2efb3f/enable-linux-vmss.bat). In dit voorbeeld wordt een resourcegroep, het Linux-schaalset gemaakt, koppelt u een schijf van 5 GB aan gegevens en versleutelt de virtuele-machineschaalset.
