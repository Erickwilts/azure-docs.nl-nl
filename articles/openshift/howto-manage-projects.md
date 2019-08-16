---
title: Resources beheren in azure Red Hat open Shift | Microsoft Docs
description: Projecten, sjablonen, afbeeldings stromen beheren in een Azure Red Hat open Shift-cluster
services: openshift
keywords: Red Hat open Shift-projecten aanvragen Self-provisioner
author: mjudeikis
ms.author: b-majude
ms.date: 07/19/2019
ms.topic: conceptual
ms.service: container-service
ms.openlocfilehash: 5028ce3c71538e67b50a15abb6076871d5af7050
ms.sourcegitcommit: a6888fba33fc20cc6a850e436f8f1d300d03771f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69559480"
---
# <a name="manage-projects-templates-image-streams-in-an-azure-red-hat-openshift-cluster"></a>Projecten, sjablonen, afbeeldings stromen beheren in een Azure Red Hat open Shift-cluster 

In een open Shift container platform worden projecten gebruikt om verwante objecten te groeperen en te isoleren. Als beheerder kunt u ontwikkel aars toegang geven tot specifieke projecten, toestaan dat ze hun eigen projecten maken en ze beheerders rechten verlenen aan afzonderlijke projecten.

## <a name="self-provisioning-projects"></a>Projecten zelf inrichten

U kunt ontwikkel aars in staat stellen hun eigen projecten te maken. Een API-eind punt is verantwoordelijk voor het inrichten van een project volgens een sjabloon met de naam project-Request. De webconsole en de `oc new-project` opdracht maken gebruik van dit eind punt wanneer een ontwikkelaar een nieuw project maakt.

Wanneer een project aanvraag wordt ingediend, vervangt de API de volgende para meters in de sjabloon:

| Parameter               | Description                                    |
| ----------------------- | ---------------------------------------------- |
| PROJECT_NAME            | De naam van het project. Vereist.             |
| PROJECT_DISPLAYNAME     | De weergave naam van het project. Kan leeg zijn. |
| PROJECT_DESCRIPTION     | De beschrijving van het project. Kan leeg zijn.  |
| PROJECT_ADMIN_USER      | De gebruikers naam van de gebruiker met beheerders rechten.       |
| PROJECT_REQUESTING_USER | De gebruikers naam van de gebruiker die de aanvraag heeft ingediend.           |

De toegang tot de API wordt verleend aan ontwikkel aars met de cluster functie van Self-provisioners. Deze functie is standaard beschikbaar voor alle geverifieerde ontwikkel aars.

## <a name="modify-the-template-for-a-new-project"></a>De sjabloon voor een nieuw project wijzigen 

1. Meld u aan als een gebruiker `customer-admin` met bevoegdheden.

2. Bewerk de standaard sjabloon project-aanvraag.

   ```
   oc edit template project-request -n openshift
   ```

3. Verwijder de standaard project sjabloon uit het update proces van Azure Red Hat open Shift (ARO) door de volgende aantekening toe te voegen:`openshift.io/reconcile-protect: "true"`

   ```
   ...
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
   ...
   ```

   De sjabloon project-aanvraag wordt niet bijgewerkt door het ARO-update proces. Hierdoor kunnen klanten de sjabloon aanpassen en deze aanpassingen behouden wanneer het cluster wordt bijgewerkt.

## <a name="disable-the-self-provisioning-role"></a>De rol voor zelf inrichting uitschakelen

U kunt voor komen dat een geverifieerde gebruikers groep nieuwe projecten zelf inricht.

1. Meld u aan als een gebruiker `customer-admin` met bevoegdheden.

2. Bewerk de cluster functie binding Self-inrichting.

   ```
   oc edit clusterrolebinding self-provisioners
   ```

3. Verwijder de rol uit het ARO-update proces door de volgende aantekening `openshift.io/reconcile-protect: "true"`toe te voegen:.

   ```
   ...
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
   ...
   ```

4. Wijzig de binding van de cluster functie `system:authenticated:oauth` om te voor komen dat projecten worden gemaakt:

   ```
   apiVersion: authorization.openshift.io/v1
   groupNames:
   - osa-customer-admins
   kind: ClusterRoleBinding
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
     labels:
       azure.openshift.io/owned-by-sync-pod: "true"
     name: self-provisioners
   roleRef:
     name: self-provisioner
   subjects:
   - kind: SystemGroup
     name: osa-customer-admins
   ```

## <a name="manage-default-templates-and-imagestreams"></a>Standaard sjablonen en imageStreams beheren

In azure Red Hat open Shift kunt u updates uitschakelen voor alle standaard sjablonen en afbeeldings stromen binnen `openshift` de naam ruimte.
Updates voor alle en `Templates` `ImageStreams` in `openshift` de naam ruimte uitschakelen:

1. Meld u aan als een gebruiker `customer-admin` met bevoegdheden.

2. Naam `openshift` ruimte bewerken:

   ```
   oc edit namespace openshift
   ```

3. Verwijder `openshift` de naam ruimte uit het Aro-update proces door de volgende aantekening toe te voegen:`openshift.io/reconcile-protect: "true"`

   ```
   ...
   metadata:
     annotations:
       openshift.io/reconcile-protect: "true"
   ...
   ```

   Elk afzonderlijk object in de `openshift` naam ruimte kan worden verwijderd uit het update proces door `openshift.io/reconcile-protect: "true"` hieraan een aantekening toe te voegen.

## <a name="next-steps"></a>Volgende stappen

Probeer de zelf studie:
> [!div class="nextstepaction"]
> [Een Azure Red Hat OpenShift-cluster maken](tutorial-create-cluster.md)
