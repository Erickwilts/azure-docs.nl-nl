---
title: Over het oplossen van Azure Monitor voor containers | Microsoft Docs
description: Dit artikel wordt beschreven hoe u kunt problemen op te lossen met Azure Monitor voor containers.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/27/2018
ms.author: magoedte
ms.openlocfilehash: 2e3e39ef24d82393d981c0ce276b3338419e0b2d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65521776"
---
# <a name="troubleshooting-azure-monitor-for-containers"></a>Oplossen van problemen met Azure Monitor voor containers

Wanneer u de bewaking van uw cluster Azure Kubernetes Service (AKS) met Azure Monitor voor containers configureert, kunt u een probleem te voorkomen dat het verzamelen van gegevens of rapportage over de status kunt tegenkomen. Dit artikel worden enkele veelvoorkomende problemen en stappen voor probleemoplossing.

## <a name="authorization-error-during-onboarding-or-update-operation"></a>Autorisatiefout tijdens het onboarding- of update-bewerking
Tijdens het inschakelen van Azure Monitor voor containers of bijwerken van een cluster ter ondersteuning van verzameld metrische gegevens, kan er een foutmelding die lijkt op het volgende - *de client < van gebruikers-id >' met object-id "< gebruiker van object-id >" geen om uit te voeren actie 'Microsoft.Authorization/roleAssignments/write' over scope*

Tijdens de onboarding of update verlenen de **bewaking metrische gegevens Publisher** roltoewijzing wordt toegepast op de cluster-bron. De gebruiker het proces starten om in te schakelen van Azure Monitor voor containers of de update voor de ondersteuning van het verzamelen van metrische gegevens moet toegang hebben tot de **Microsoft.Authorization/roleAssignments/write** machtiging voor het AKS-cluster bereik van de resource. Alleen leden van de **eigenaar** en **Administrator voor gebruikerstoegang** ingebouwde rollen toegang tot deze machtiging worden verleend. Als uw beveiligingsbeleid gedetailleerde machtigingen toe te wijzen vereisen, raden wij aan u [aangepaste rollen](../../role-based-access-control/custom-roles.md) en wijs deze toe aan de gebruikers die moeten worden opgeslagen. 

U kunt ook handmatig deze rol verlenen vanuit Azure portal door de volgende stappen uit:

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 
2. Klik in Azure Portal in de linkerbovenhoek op **Alle services**. Typ in de lijst met resources **Kubernetes**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Azure Kubernetes**.
3. Selecteer in de lijst met Kubernetes-clusters, een in de lijst.
2. Klik in het menu links op **toegangsbeheer (IAM)** .
3. Selecteer **+ toevoegen** een roltoewijzing toevoegen en selecteer de **Publisher van metrische gegevens controleren** rol en klikt u onder de **selecteren** vak **AKS** naar filter de resultaten op alleen de clusters service-principals die zijn gedefinieerd in het abonnement. Selecteer in de lijst die specifiek is voor dat cluster.
4. Selecteer **opslaan** voltooien van de rol toe te wijzen. 

## <a name="azure-monitor-for-containers-is-enabled-but-not-reporting-any-information"></a>Azure Monitor voor containers zijn ingeschakeld, maar niet alle informatie rapporteren
Als Azure Monitor voor containers met succes is ingeschakeld en geconfigureerd, maar u kunt geen statusinformatie weergeven of er zijn geen resultaten worden geretourneerd door een logboekquery, kunt u het probleem onderzoeken door de volgende stappen: 

1. Controleer de status van de agent met de opdracht: 

    `kubectl get ds omsagent --namespace=kube-system`

    De uitvoer moet eruitzien zoals in het volgende, waarmee wordt aangegeven dat deze correct is geïmplementeerd:

    ```
    User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
    NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
    omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
    ```  
2. Controleer de implementatiestatus met agentversie *06072018* of met behulp van de opdracht:

    `kubectl get deployment omsagent-rs -n=kube-system`

    De uitvoer moet eruitzien zoals in het volgende voorbeeld, waarin wordt aangegeven dat deze correct is geïmplementeerd:

    ```
    User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
    NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
    omsagent   1         1         1            1            3h
    ```

3. Controleer de status van de schil om te controleren of deze wordt uitgevoerd met de opdracht: `kubectl get pods --namespace=kube-system`

    De uitvoer is vergelijkbaar met het volgende voorbeeld met de status van *met* voor de omsagent:

    ```
    User@aksuser:~$ kubectl get pods --namespace=kube-system 
    NAME                                READY     STATUS    RESTARTS   AGE 
    aks-ssh-139866255-5n7k5             1/1       Running   0          8d 
    azure-vote-back-4149398501-7skz0    1/1       Running   0          22d 
    azure-vote-front-3826909965-30n62   1/1       Running   0          22d 
    omsagent-484hw                      1/1       Running   0          1d 
    omsagent-fkq7g                      1/1       Running   0          1d 
    ```

4. Raadpleeg de logboeken van de agent. Wanneer de agent in containers wordt geïmplementeerd, wordt een snelle controle uitgevoerd door het OMI-opdrachten uitvoeren en de versie van de agent en de provider weergeven. 

5. Om te controleren dat de agent is geïmplementeerd, moet u de opdracht uitvoeren: `kubectl logs omsagent-484hw --namespace=kube-system`

    De status moet lijken op het volgende voorbeeld:

    ```
    User@aksuser:~$ kubectl logs omsagent-484hw --namespace=kube-system
    :
    :
    instance of Container_HostInventory
    {
        [Key] InstanceID=3a4407a5-d840-4c59-b2f0-8d42e07298c2
        Computer=aks-nodepool1-39773055-0
        DockerVersion=1.13.1
        OperatingSystem=Ubuntu 16.04.3 LTS
        Volume=local
        Network=bridge host macvlan null overlay
        NodeRole=Not Orchestrated
        OrchestratorType=Kubernetes
    }
    Primary Workspace: b438b4f6-912a-46d5-9cb1-b44069212abc    Status: Onboarded(OMSAgent Running)
    omi 1.4.2.2
    omsagent 1.6.0.23
    docker-cimprov 1.0.0.31
    ```

## <a name="error-messages"></a>Foutberichten

De onderstaande tabel bevat een overzicht van de bekende problemen die optreden kunnen tijdens het gebruik van Azure Monitor voor containers.

| Foutberichten  | Bewerking |  
| ---- | --- |  
| Foutbericht `No data for selected filters`  | Het duurt enige tijd tot stand brengen van bewaking gegevensstroom voor nieuwe clusters. Toestaan dat gegevens worden weergegeven voor uw cluster ten minste 10 tot 15 minuten. |   
| Foutbericht `Error retrieving data` | Terwijl Azure Kubenetes Service-cluster voor basisstatus en -prestaties instelt, een verbinding tot stand is gebracht tussen het cluster en Azure Log Analytics-werkruimte. Een Log Analytics-werkruimte wordt gebruikt voor het opslaan van alle bewakingsgegevens voor uw cluster. Deze fout kan optreden wanneer uw Log Analytics-werkruimte is verwijderd of verloren gaan. Controleer of uw werkruimte beschikbaar aan de hand is [toegang beheren](../platform/manage-access.md#view-workspace-details). Als de werkruimte ontbreekt, moet u opnieuw inschakelen van het cluster controleren met Azure Monitor voor containers. Als u wilt opnieuw inschakelt, moet u [uitschakelen](container-insights-optout.md) bewaking voor het cluster en [inschakelen](container-insights-enable-new-cluster.md) Azure Monitor voor containers opnieuw. |  
| `Error retrieving data` na het toevoegen van Azure Monitor voor containers via az aks cli | Wanneer het inschakelen van bewaking met `az aks cli`, Azure-Monitor voor containers mogelijk niet correct worden geïmplementeerd. Controleer of de oplossing is geïmplementeerd. Om dit te doen, gaat u naar uw Log Analytics-werkruimte en of de oplossing beschikbaar is, door te selecteren **oplossingen** in het deelvenster aan de linkerkant. U lost dit probleem, moet u de oplossing implementeren door de instructies te volgen op [Azure Monitor for containers implementeren](container-insights-onboard.md) |  

Om u te helpen bij het vaststellen van het probleem, wij een probleemoplossing script beschikbaar hebt opgegeven [hier](https://github.com/Microsoft/OMS-docker/tree/ci_feature_prod/Troubleshoot#troubleshooting-script).  

## <a name="next-steps"></a>Volgende stappen
Deze metrische gegevens voor servicestatus zijn met bewaking ingeschakeld om vast te leggen van de gezondheid van metrische gegevens voor de AKS-clusterknooppunten en de schillen, beschikbaar in de Azure-portal. Zie voor meer informatie over het gebruik van Azure Monitor voor containers, [weergave Azure Kubernetes Service health](container-insights-analyze.md).