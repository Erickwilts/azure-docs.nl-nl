---
title: De standaard TEMP-map is te klein voor een rol | Microsoft Docs
description: Een Cloud serviceprovider heeft een beperkte hoeveelheid ruimte voor de map TEMP. In dit artikel vindt u enkele suggesties om te voor komen dat er bijna geen ruimte meer beschikbaar is.
services: cloud-services
documentationcenter: ''
author: simonxjx
manager: felixwu
editor: ''
tags: top-support-issue
ms.assetid: 9f2af8dd-2012-4b36-9dd5-19bf6a67e47d
ms.service: cloud-services
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 06/15/2018
ms.author: v-six
ms.openlocfilehash: 9b731eba4e1cd79a07c20271f1bd33c8c2c35d92
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2019
ms.locfileid: "68945364"
---
# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>De standaard TEMP-map is te klein voor de web-of worker-rol van de Cloud service
De standaard tijdelijke map van een Cloud Service medewerker of Web Role heeft een maximale grootte van 100 MB. deze kan op een bepaald moment vol raken. In dit artikel wordt beschreven hoe u kunt voor komen dat er geen ruimte meer is voor de tijdelijke map.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Waarom kan ik geen ruimte meer gebruiken?
De standaard Windows-omgevings variabelen TEMP en TMP zijn beschikbaar voor code die wordt uitgevoerd in uw toepassing. Zowel TEMP als TMP verwijzen naar één map met een maximale grootte van 100 MB. Gegevens die in deze map worden opgeslagen, worden niet bewaard gedurende de levens cyclus van de Cloud service. Als de rolinstanties in een Cloud service worden gerecycled, wordt de Directory gereinigd.

## <a name="suggestion-to-fix-the-problem"></a>Suggestie voor het oplossen van het probleem
Implementeer een van de volgende alternatieven:

* Configureer een lokale opslag resource en open deze rechtstreeks in plaats van het gebruik van TEMP of TMP. Als u toegang wilt krijgen tot een lokale opslag resource vanuit code die wordt uitgevoerd in uw toepassing, roept u de methode [RoleEnvironment. GetLocalResource](/previous-versions/azure/reference/ee772845(v=azure.100)) aan.
* Configureer een lokale opslag resource en wijs de tijdelijke en TMP-mappen aan om naar het pad van de lokale opslag bron te verwijzen. Deze wijziging moet worden uitgevoerd binnen de methode [RoleEntryPoint. ONSTART](/previous-versions/azure/reference/ee772851(v=azure.100)) .

In het volgende code voorbeeld ziet u hoe u de doel mappen voor TEMP en TMP wijzigt in de methode onstart:

```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen
Lees een blog waarin wordt beschreven [hoe u de tijdelijke map ASP.net van Azure Web Role verg root](https://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).

Bekijk meer [artikelen over probleem oplossing](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/vs-azure-tools-debugging-cloud-services-overview.md) voor Cloud Services.

Bekijk [de blog serie van Kevin Williamson](https://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)voor informatie over het oplossen van problemen met Cloud service rollen met behulp van Azure PaaS computer diagnostische gegevens.
