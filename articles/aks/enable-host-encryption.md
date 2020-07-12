---
title: Op hosts gebaseerde versleuteling inschakelen op de Azure Kubernetes-service (AKS)
description: Meer informatie over het configureren van een op een host gebaseerde versleuteling in een Azure Kubernetes service (AKS)-cluster
services: container-service
ms.topic: article
ms.date: 07/10/2020
ms.openlocfilehash: 7b9d930d62d0acea30af9b5e7e12e43fa8fcd5da
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2020
ms.locfileid: "86244307"
---
# <a name="host-based-encryption-on-azure-kubernetes-service-aks-preview"></a>Versleuteling op basis van een host op de Azure Kubernetes-service (AKS) (preview)

Met versleuteling op basis van een host worden de gegevens die zijn opgeslagen op de VM-host van de virtuele machines van uw AKS-agent, versleuteld op rest en stromen die zijn versleuteld met de opslag service. Dit betekent dat de tijdelijke schijven in rust worden versleuteld met sleutels die door het platform worden beheerd. De cache van het besturings systeem en de gegevens schijven is versleuteld met door het platform beheerde sleutels of door de klant beheerde sleutels, afhankelijk van het versleutelings type dat op die schijven is ingesteld. Wanneer u AKS gebruikt, worden besturings systeem-en gegevens schijven standaard versleuteld met door het platform beheerde sleutels, wat inhoudt dat de caches voor deze schijven ook standaard versleuteld zijn met door het platform beheerde sleutels.  U kunt uw eigen beheerde sleutels opgeven na [het maken van uw eigen sleutels (BYOK) met Azure-schijven in de Azure Kubernetes-service](azure-disk-customer-managed-keys.md). De cache voor deze schijven wordt vervolgens ook versleuteld met behulp van de sleutel die u in deze stap opgeeft.


## <a name="before-you-begin"></a>Voordat u begint

Deze functie kan alleen worden ingesteld tijdens het maken van het cluster of het maken van een knooppunt groep.

> [!NOTE]
> Versleuteling op basis van een host is beschikbaar in [Azure-regio's][supported-regions] die ondersteuning bieden voor versleuteling aan de server zijde van Azure Managed disks en alleen met specifieke [ondersteunde VM-grootten][supported-sizes].

### <a name="prerequisites"></a>Vereisten

- Controleer of u de `aks-preview` cli-extensie v 0.4.55 of hoger hebt geïnstalleerd
- Zorg ervoor dat u de `EncryptionAtHost` functie vlag onder `Microsoft.Compute` ingeschakeld hebt.
- Zorg ervoor dat u de `EncryptionAtHost` functie vlag onder `Microsoft.ContainerService` ingeschakeld hebt.

### <a name="register-encryptionathost--preview-features"></a>`EncryptionAtHost`Preview-functies registreren

Als u een AKS-cluster wilt maken dat gebruikmaakt van versleuteling op basis van een host, moet u de `EncryptionAtHost` functie vlag inschakelen voor uw abonnement.

Registreer de `EncryptionAtHost` functie vlag met de opdracht [AZ feature REGI ster][az-feature-register] , zoals weer gegeven in het volgende voor beeld:

```azurecli-interactive
az feature register --namespace "Microsoft.Compute" --name "EncryptionAtHost"

az feature register --namespace "Microsoft.ContainerService"  --name "EncryptionAtHost"
```

Het duurt enkele minuten voordat de status is *geregistreerd*. U kunt de registratie status controleren met de opdracht [AZ Feature List][az-feature-list] :

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.Compute/EncryptionAtHost')].{Name:name,State:properties.state}"

az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/EncryptionAtHost')].{Name:name,State:properties.state}"
```

Als u klaar bent, vernieuwt u de registratie van de `Microsoft.ContainerService` `Microsoft.Compute` resource providers met de opdracht [AZ provider REGI ster][az-provider-register] :

```azurecli-interactive
az provider register --namespace Microsoft.Compute

az provider register --namespace Microsoft.ContainerService
```

> [!IMPORTANT]
> De preview-functies van AKS zijn self-service opt-in. Previews worden ' as-is ' en ' as available ' gegeven en zijn uitgesloten van de service level agreements en beperkte garantie. AKS-previews worden gedeeltelijk gedekt door klant ondersteuning, op basis van de beste inspanningen. Daarom zijn deze functies niet bedoeld voor productie gebruik. Raadpleeg de volgende artikelen met technische ondersteuning voor meer informatie.
>
> - [AKS-ondersteunings beleid](support-policies.md)
> - [Veelgestelde vragen over ondersteuning voor Azure](faq.md)

### <a name="install-aks-preview-cli-extension"></a>AKS-preview CLI-extensie installeren

Als u een AKS-cluster wilt maken dat is gebaseerd op een host-gebaseerde versleuteling, hebt u de meest recente *AKS-preview cli-* extensie nodig. Installeer de Azure CLI *-extensie AKS-preview* met behulp van de opdracht [AZ extension add][az-extension-add] of Controleer of er beschik bare updates zijn met behulp van de opdracht [AZ extension update][az-extension-update] :

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="limitations"></a>Beperkingen

- Kan alleen worden ingeschakeld voor nieuwe knooppunt groepen of nieuwe clusters.
- Kan alleen worden ingeschakeld in [Azure-regio's][supported-regions] die ondersteuning bieden voor versleuteling aan de server zijde van Azure Managed disks en alleen met specifieke [ondersteunde VM-grootten][supported-sizes].
- Vereist een AKS-cluster en-knooppunt groep op basis van Virtual Machine Scale Sets (VMSS) als *type VM-set*.

## <a name="use-host-based-encryption-on-new-clusters-preview"></a>Op hosts gebaseerde versleuteling gebruiken voor nieuwe clusters (preview-versie)

Configureer de cluster agent-knoop punten voor het gebruik van versleuteling op basis van een host wanneer het cluster wordt gemaakt. Gebruik de `--aks-custom-headers` markering om de koptekst in te stellen `EnableEncryptionAtHost` .

```azurecli-interactive
az aks create --name myAKSCluster --resource-group myResourceGroup -s Standard_DS2_v2 -l westus2 --aks-custom-headers EnableEncryptionAtHost=true
```

Als u clusters wilt maken zonder versleuteling op basis van een host, kunt u dit doen door de aangepaste para meter weg te laten `--aks-custom-headers` .

## <a name="use-host-based-encryption-on-existing-clusters-preview"></a>Op hosts gebaseerde versleuteling op bestaande clusters gebruiken (preview-versie)

U kunt op een host gebaseerde versleuteling op bestaande clusters inschakelen door een nieuwe knooppunt groep toe te voegen aan uw cluster. Configureer een nieuwe knooppunt groep om versleuteling op basis van een host te gebruiken met behulp van de `--aks-custom-headers` vlag.

```azurecli
az aks nodepool add --name hostencrypt --cluster-name myAKSCluster --resource-group myResourceGroup -s Standard_DS2_v2 -l westus2 --aks-custom-headers EnableEncryptionAtHost=true
```

Als u nieuwe knooppunt groepen wilt maken zonder de op de host gebaseerde versleutelings functie, kunt u dit doen door de aangepaste para meter weg te laten `--aks-custom-headers` .

## <a name="next-steps"></a>Volgende stappen

Lees de [Aanbevolen procedures voor AKS-cluster beveiliging][best-practices-security] meer informatie over [op hosts gebaseerde versleuteling](../virtual-machines/linux/disk-encryption.md#encryption-at-host---end-to-end-encryption-for-your-vm-data).


<!-- LINKS - external -->

<!-- LINKS - internal -->
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[best-practices-security]: ./operator-best-practices-cluster-security.md
[supported-regions]: ../virtual-machines/linux/disk-encryption.md#supported-regions
[supported-sizes]: ../virtual-machines/linux/disk-encryption.md#supported-vm-sizes
[azure-cli-install]: /cli/azure/install-azure-cli
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
