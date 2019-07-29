---
title: 'Azure PowerShell-voorbeeldscript: toepassingscertificaat toevoegen aan een cluster | Microsoft Docs'
description: 'Azure PowerShell-voorbeeldscript: een toepassingscertificaat toevoegen aan een Service Fabric-cluster.'
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 01/18/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: 1f2bd57a799bb00e9328721a08eb93665650f7f4
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68592296"
---
# <a name="add-an-application-certificate-to-a-service-fabric-cluster"></a>Een toepassingscertificaat toevoegen aan een Service Fabric-cluster

In dit voorbeeldscript wordt een zelfondertekend certificaat gemaakt in de opgegeven Azure-sleutelkluis en geïnstalleerd op alle knooppunten van het Service Fabric-cluster. Het certificaat kan ook worden gedownload naar een lokale map. De naam van het gedownloade certificaat is dezelfde als de naam van het certificaat in de sleutelkluis. Pas de parameters zo nodig aan.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Installeer zo nodig de Azure PowerShell volgens de instructies in de [Azure PowerShell handleiding](/powershell/azure/overview) en voer vervolgens `Connect-AzAccount` uit om verbinding maken met Azure. 

## <a name="sample-script"></a>Voorbeeldscript

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-application-certificate/add-new-application-certificate.ps1 "Add an application certificate to a cluster")]

## <a name="script-explanation"></a>Uitleg van het script

In dit script worden de volgende opdrachten gebruikt: Elke opdracht in de tabel is een koppeling naar specifieke documentatie over de opdracht.

| Opdracht | Opmerkingen |
|---|---|
| [Add-AzServiceFabricApplicationCertificate](/powershell/module/az.servicefabric/Add-azServiceFabricApplicationCertificate) | Voeg een nieuw toepassingscertificaat toe aan de virtuele-machineschaalset waaruit het cluster bestaat.  |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure PowerShell-module de [documentatie van Azure PowerShell](/powershell/azure/overview).

Meer voorbeelden voor Azure Powershell voor Azure Service Fabric vindt u in de [voorbeelden van Azure PowerShell](../service-fabric-powershell-samples.md).
