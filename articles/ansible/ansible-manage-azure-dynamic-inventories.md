---
title: 'Zelfstudie: dynamische voorraden van uw Azure-resources met behulp van Ansible configureren | Microsoft Docs'
description: Informatie over het beheren van dynamische voorraden in Azure met Ansible
keywords: ansible, azure, devops, bash, cloudshell, dynamische voorraad
ms.topic: tutorial
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: 46b13fae437a555edf0bdd0b0d4c1496d7596e0f
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2019
ms.locfileid: "65230695"
---
# <a name="tutorial-configure-dynamic-inventories-of-your-azure-resources-using-ansible"></a>Zelfstudie: Dynamische voorraden van uw Azure-resources met behulp van Ansible configureren

Ansible kan worden gebruikt om voorraadinformatie uit verschillende bronnen ((waaronder cloudbronnen zoals Azure) in een *dynamische voorraad* op te halen. 

[!INCLUDE [ansible-tutorial-goals.md](../../includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * Configureer twee virtuele testmachines. 
> * Tag een van de virtuele machines
> * Installeer Nginx op de gelabelde virtuele machines
> * Configureren van een dynamische voorraad met de geconfigureerde Azure-resources

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [open-source-devops-prereqs-create-service-principal.md](../../includes/open-source-devops-prereqs-create-service-principal.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-the-test-vms"></a>De test-VM's maken

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Open [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

1. Maak een Azure-resourcegroep om de virtuele machines voor deze zelfstudie in op te slaan.

    > [!IMPORTANT]  
    > De Azure-resourcegroep die u in deze stap u maakt, moet een naam hebben die volledig uit kleine letters bestaat. Anders mislukt het genereren van de dynamische voorraad.

    ```azurecli-interactive
    az group create --resource-group ansible-inventory-test-rg --location eastus
    ```

1. Maak twee virtuele Linux-machines in Azure met een van de volgende technieken:

    - **Ansible-playbook**: in het artikel [Een eenvoudige virtuele machine maken in Azure met Ansible](/azure/virtual-machines/linux/ansible-create-vm) wordt geïllustreerd hoe u een virtuele machine maakt op basis van een Ansible-playbook. Als u een playbook gebruikt voor het definiëren van een van de of beide virtuele machines, moet u ervoor zorgen dat de SSH-verbinding wordt gebruikt in plaats van een wachtwoord.

    - **Azure CLI**: voer elk van de volgende opdrachten in de Cloud Shell uit om de twee virtuele machines te maken:

        ```azurecli-interactive
        az vm create --resource-group ansible-inventory-test-rg \
                     --name ansible-inventory-test-vm1 \
                     --image UbuntuLTS --generate-ssh-keys
        ```

        ```azurecli-interactive
        az vm create --resource-group ansible-inventory-test-rg \
                     --name ansible-inventory-test-vm2 \
                     --image UbuntuLTS --generate-ssh-keys
        ```

## <a name="tag-a-vm"></a>Een virtuele machine taggen

U kunt [tags gebruiken om uw Azure-resources te organiseren](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags#azure-cli) met behulp van door de gebruiker gedefinieerde categorieën. 

Voer de volgende opdracht [az resource tag](/cli/azure/resource?view=azure-cli-latest.md#az-resource-tag) in om de virtuele machine `ansible-inventory-test-vm1` met de sleutel `nginx` te taggen:

```azurecli-interactive
az resource tag --tags nginx --id /subscriptions/<YourAzureSubscriptionID>/resourceGroups/ansible-inventory-test-rg/providers/Microsoft.Compute/virtualMachines/ansible-inventory-test-vm1
```
## <a name="generate-a-dynamic-inventory"></a>Een dynamische voorraad genereren

Zodra u uw virtuele machines hebt gedefinieerd (en getagd), is het tijd om de dynamische voorraad te genereren.

### <a name="using-ansible-version--28"></a>Wanneer u Ansible gebruikt versie < 2.8

Ansible biedt een Python-script met de naam [azure_rm.py](https://github.com/ansible/ansible/blob/devel/contrib/inventory/azure_rm.py) die genereert een dynamische voorraad van uw Azure-resources. In de volgende stappen wordt uitgelegd hoe u het script `azure_rm.py` gebruikt om verbinding te maken met de twee virtuele testmachines van Azure:

1. Gebruik de GNU-opdracht `wget` om het script `azure_rm.py` op te halen:

    ```azurecli-interactive
    wget https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/azure_rm.py
    ```

1. Gebruik de opdracht `chmod` om de machtigingen voor toegang tot het script `azure_rm.py` te wijzigen. De volgende opdracht maakt gebruik van de parameter `+x` om uitvoering van het opgegeven bestand (`azure_rm.py`) toe te staan:

    ```azurecli-interactive
    chmod +x azure_rm.py
    ```

1. Gebruik de [ansible-opdracht](https://docs.ansible.com/ansible/2.4/ansible.html) om verbinding te maken met de resourcegroep: 

    ```azurecli-interactive
    ansible -i azure_rm.py ansible-inventory-test-rg -m ping 
    ```

1. Als u verbinding hebt, ziet u resultaten die vergelijkbaar zijn met de volgende uitvoer:

    ```Output
    ansible-inventory-test-vm1 | SUCCESS => {
        "changed": false,
        "failed": false,
        "ping": "pong"
    }
    ansible-inventory-test-vm2 | SUCCESS => {
        "changed": false,
        "failed": false,
        "ping": "pong"
    }
    ```

### <a name="ansible-version--28"></a>Ansible versie > = 2.8

Beginnen met Ansible 2.8, Ansible biedt een [Azure dynamische voorraad invoegtoepassing](https://github.com/ansible/ansible/blob/devel/lib/ansible/plugins/inventory/azure_rm.py). De volgende stappen helpen u bij het gebruik van de invoegtoepassing:

1. De inventaris-invoegtoepassing vereist een configuratiebestand. Het configuratiebestand moet eindigen op `azure_rm` en hebben de extensie van een van beide `yml` of `yaml`. Voor dit voorbeeld van de zelfstudie, sla de volgende playbook als `myazure_rm.yml`:

    ```yml
    plugin: azure_rm
    include_vm_resource_groups:
    - ansible-inventory-test-rg
    auth_source: auto
    ```

1. Voer de volgende opdracht om te pingen van virtuele machines in de resourcegroep:

    ```bash
    ansible all -m ping -i ./myazure_rm.yml
    ```

1. Wanneer u de voorgaande opdracht uitvoert, kan het volgende foutbericht worden weergegeven:

    ```Output
    Failed to connect to the host via ssh: Host key verification failed.
    ```
    
    Als u de verificatiefout 'host-sleutel ' ontvangt, moet u de volgende regel toevoegen voor het configuratiebestand Ansible. Het configuratiebestand Ansible bevindt zich in `/etc/ansible/ansible.cfg`.

    ```bash
    host_key_checking = False
    ```

1. Als u de playbook hebt uitgevoerd, ziet u resultaat is vergelijkbaar met de volgende uitvoer:
  
    ```Output
    ansible-inventory-test-vm1_0324 : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
    ansible-inventory-test-vm2_8971 : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
    ```

## <a name="enable-the-vm-tag"></a>De code van de virtuele machine inschakelen
Als u een label hebt ingesteld, moet u 'inschakelen' dat label. Een manier om in te schakelen van een label is door de tag te exporteren naar een omgevingsvariabele `AZURE_TAGS` via de `export` opdracht:

```azurecli-interactive
export AZURE_TAGS=nginx
```

- Als u Ansible < 2.8, voer de volgende opdracht uit:

    ```bash
    ansible -i azure_rm.py ansible-inventory-test-rg -m ping
    ```

- Als u Ansible > = 2.8, voer de volgende opdracht uit:
  
    ```bash
    ansible all -m ping -i ./myazure_rm.yml
    ```

Nu ziet u slechts één virtuele machine (met de tag overeenkomt met de waarde die wordt geëxporteerd naar de `AZURE_TAGS` omgevingsvariabele):

```Output
ansible-inventory-test-vm1 | SUCCESS => {
    "changed": false,
    "failed": false,
    "ping": "pong"
}
```

## <a name="set-up-nginx-on-the-tagged-vm"></a>Nginx instellen op de getagde virtuele machine

Het doel van tags is om de mogelijkheid in te schakelen snel en eenvoudig met subgroepen van uw virtuele machines te werken. Stel dat u Nginx bijvoorbeeld wilt installeren op virtuele machines waarop u een tag van `nginx` hebt toegewezen. In de volgende stappen ziet u hoe u dit gemakkelijk kunt doen:

1. Maak een bestand met de naam `nginx.yml`:

   ```azurecli-interactive
   code nginx.yml
   ```

1. Plak de volgende voorbeeldcode in de editor:

    ```yml
    ---
    - name: Install and start Nginx on an Azure virtual machine
      hosts: all
      become: yes
      tasks:
      - name: install nginx
        apt: pkg=nginx state=installed
        notify:
        - start nginx

      handlers:
        - name: start nginx
          service: name=nginx state=started
    ```

1. Sla het bestand op en sluit de editor af.

1. Voer de playbook met behulp de `ansible-playbook` opdracht:

   - Ansible < 2.8:

    ```bash
    ansible-playbook -i azure_rm.py nginx.yml
    ```

   - Ansible > = 2.8:

    ```bash
     ansible-playbook  -i ./myazure_rm.yml  nginx.yml
    ```

1. Nadat de playbook is uitgevoerd, ziet u uitvoer die vergelijkbaar is met de volgende resultaten:

    ```Output
    PLAY [Install and start Nginx on an Azure virtual machine] 

    TASK [Gathering Facts] 
    ok: [ansible-inventory-test-vm1]

    TASK [install nginx] 
    changed: [ansible-inventory-test-vm1]

    RUNNING HANDLER [start nginx] 
    ok: [ansible-inventory-test-vm1]

    PLAY RECAP 
    ansible-inventory-test-vm1 : ok=3    changed=1    unreachable=0    failed=0
    ```

## <a name="test-nginx-installation"></a>Nginx-installatie testen

In deze sectie wordt één techniek geïllustreerd om te testen of Nginx op uw virtuele machine is geïnstalleerd.

1. Gebruik de opdracht [az vm list-ip-addresses](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-list-ip-addresses) om het IP-adres van de virtuele machine `ansible-inventory-test-vm1` op te halen. De geretourneerde waarde (het IP-adres van de virtuele machine) wordt vervolgens als de parameter gebruikt voor de SSH-opdracht om verbinding te maken met de virtuele machine.

    ```azurecli-interactive
    ssh `az vm list-ip-addresses \
    -n ansible-inventory-test-vm1 \
    --query [0].virtualMachine.network.publicIpAddresses[0].ipAddress -o tsv`
    ```

1. Terwijl u verbinding hebt met de virtuele machine `ansible-inventory-test-vm1`, voert u de opdracht [nginx - v](https://nginx.org/en/docs/switches.html) uit om te bepalen of Nginx is geïnstalleerd.

    ```azurecli-interactive
    nginx -v
    ```

1. Zodra u de opdracht `nginx -v` uitvoert, ziet u de Nginx-versie (tweede regel) die aangeeft dat Nginx is geïnstalleerd.

    ```Output
    tom@ansible-inventory-test-vm1:~$ nginx -v

    nginx version: nginx/1.10.3 (Ubuntu)

    tom@ansible-inventory-test-vm1:~$
    ```

1. Klik op de `<Ctrl>D` combinatie om de SSH-sessie te verbreken op het toetsenbord.

1. Doet de voorgaande stappen voor het `ansible-inventory-test-vm2` virtuele machine levert een informatief bericht waarmee wordt aangegeven waar vind u Nginx (Dit betekent dat u deze nog niet geïnstalleerd op dit moment):

    ```Output
    tom@ansible-inventory-test-vm2:~$ nginx -v
    The program 'nginx' can be found in the following packages:
    * nginx-core
    * nginx-extras
    * nginx-full
    * nginx-lightTry: sudo apt install <selected package>
    tom@ansible-inventory-test-vm2:~$
    ```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"] 
> [Snelstart: Virtuele Linux-machines in Azure met behulp van Ansible configureren](/azure/virtual-machines/linux/ansible-create-vm)